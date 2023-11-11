---
title: AndroidKeyStoreがEd25519に（サイレントで）対応していた件
author: Keiji Ariyama
type: post
date: 2023-11-11T19:00:00+09:00
url: /2023/11/techbookfest15.html
categories:
  - イベント

---

まずは宣伝です。

11月12日（日）、池袋サンシャインシティにて開催される「[技術書典15](https://techbookfest.org/event/tbf15)」に、サークル「[めがねをかけるんだ](https://techbookfest.org/organization/5173176170446848)」として参加させていただくことになりました。

サークル配置は「さ17」。

新刊は「AndroidKeyStoreと過ごした400日」です。

| タイトル | AndroidKeyStoreと過ごした400日 |
|:---- |:----------------- |
| 判型   | B5                |
| ページ数 | 32p（電子版）         |
| 頒布価格 | 500円（会場価格）            |
| 発行   | 個人サークル「めがねをかけるんだ」 |

本文サンプルおよび電子版販売ページは[こちら](https://techbookfest.org/product/cUVLF5TvwkhQU2t4K72RVj)

<!--more-->
----

## AndroidKeyStoreがEd25519に（サイレントで）対応していた件
本題です。

新刊を泣きながら執筆中
「楕円曲線暗号のパラメーターとか全部文字列で指定しなければならないからつらいよね」みたいなことを書いているところで、ふと有りもしない曲線の名前を指定して鍵生成を試してみたところ、予想通りに例外が発生したのですが、そのメッセージがこちら。

```
java.security.InvalidAlgorithmParameterException: Unsupported EC curve name: meganekko-daisuki. Supported: [ed25519, p-224, p-256, p-384, p-521, prime256v1, secp224r1, secp256r1, secp384r1, secp521r1, x25519]
```

注目して欲しいのは、サポートしているキーストアの中に`ed25519`があることです。

ぼくが知っているAndroidKeyStoreちゃんはEd25519にはまだ対応していなかったはずです。[AndroidKeyStoreのページ（日本語）](https://developer.android.com/training/articles/keystore?hl=ja)にも、[SignatureのAPIリファレンス（英語）](https://developer.android.com/reference/java/security/Signature)にも、Curve25519に関する記述はありません。

実際、以前試したときは「Ed25519、知りませんね？」となって、以降、AndroidKeyStoreはCurve25519には対応していないと覚えてきました。

けれど、エラーメッセージには確かに`ed25519`の文字があります（`x25519`も）。

ドキドキしながら`KeyGenParameterSpec.Builder`で`ed25519`の鍵ペアの生成を試みたところ、普通に生成・利用できてしまいました。

### サンプル（テスト）コード
AndroidKeyStoreでEd25519をつかって署名と検証をするコードがこちらです。
新刊には入れられなかったのが悔しいので、置いておきます。

```
package dev.keiji.bocchi.crypto

import android.security.keystore.KeyGenParameterSpec
import android.security.keystore.KeyInfo
import android.security.keystore.KeyProperties
import androidx.test.ext.junit.runners.AndroidJUnit4
import dev.keiji.bocchi.toHex
import org.junit.After
import org.junit.Assert.assertEquals
import org.junit.Assert.assertTrue
import org.junit.Test
import org.junit.runner.RunWith
import java.nio.charset.StandardCharsets
import java.security.KeyFactory
import java.security.KeyPairGenerator
import java.security.KeyStore
import java.security.MessageDigest
import java.security.PrivateKey
import java.security.PublicKey
import java.security.Signature
import java.security.spec.ECGenParameterSpec
import java.util.UUID

@RunWith(AndroidJUnit4::class)
class Ed25519KeyTest {

    companion object {
        // https://cs.android.com/android/platform/superproject/+/android13-dev:frameworks/base/keystore/java/android/security/keystore2/AndroidKeyStoreEdECPublicKey.java;bpv=1;bpt=0
        private val DER_KEY_PREFIX = byteArrayOf(
            0x30,
            0x2a,
            0x30,
            0x05,
            0x06,
            0x03,
            0x2b,
            0x65,
            0x70,
            0x03,
            0x21,
            0x00
        )
    }

    @After
    fun deleteKeys() {
        val keyStore = KeyStore.getInstance("AndroidKeyStore").also {
            it.load(null)
        }
        keyStore.aliases().toList().forEach { alias ->
            keyStore.deleteEntry(alias)
        }
    }

    private fun createEd25519KeyPairInAndroidKeyStore(keyAlias: String): Pair<PrivateKey, PublicKey> {

        val kpg = KeyPairGenerator.getInstance(
            KeyProperties.KEY_ALGORITHM_EC,
            "AndroidKeyStore"
        )

        val keyGenParameterSpec =
            KeyGenParameterSpec.Builder(
                keyAlias,
                (KeyProperties.PURPOSE_SIGN or KeyProperties.PURPOSE_VERIFY)
            )
                .setAlgorithmParameterSpec(ECGenParameterSpec("ed25519"))
                .setDigests(KeyProperties.DIGEST_NONE)
//                .setIsStrongBoxBacked(true)
                .build()

        kpg.initialize(keyGenParameterSpec)

        val keyPair = kpg.generateKeyPair()

        val privateKey: PrivateKey = keyPair.private

        // https://cs.android.com/android/platform/superproject/+/master:frameworks/base/keystore/java/android/security/keystore2/AndroidKeyStoreEdECPublicKey.java
        val publicKey: PublicKey = keyPair.public

        println("algorithm: ${privateKey.algorithm}")
        println("format: ${privateKey.format}")
        println("encoded: ${privateKey.encoded}")

        return Pair(privateKey, publicKey)
    }

    @Test
    fun androidKeyStoreSignVerifyTest() {
        val keyAlias = UUID.randomUUID().toString()

        val plain = "HelloSignature"
        val plainBytes = plain.toByteArray(charset = StandardCharsets.UTF_8)

        val hashBytes = MessageDigest.getInstance("SHA256")
            .digest(plainBytes)

        val (privateKey, publicKey) = createEd25519KeyPairInAndroidKeyStore(keyAlias)

        // java.security.NoSuchAlgorithmException: no such algorithm: EdDSA for provider AndroidKeyStore
//        val keyInfo = getKeyInfo(privateKey)

        val signature = Signature.getInstance("Ed25519").also {
            it.initSign(privateKey)
        }
        val signatureBytes = signature.let {
            it.update(hashBytes)
            it.sign()
        }

        println("signatureBytes: ${signatureBytes.toHex(":")}")

        // java.security.InvalidKeyException: No installed provider supports this key: android.security.keystore2.AndroidKeyStoreEdECPublicKey
        val verifierEd25519 = Signature.getInstance("Ed25519").also {
            it.initVerify(publicKey)
            it.update(hashBytes)
        }

        val encoded = publicKey.encoded
        val publicKeyBytes = encoded.copyOfRange(DER_KEY_PREFIX.size, encoded.size)
        assertEquals(32, publicKeyBytes.size)

        println("encoded: ${encoded.toHex(":")}")
        println("publicKeyBytes: ${publicKeyBytes.toHex(":")}")

        val verifier = org.bouncycastle.crypto.signers.Ed25519Signer().also {
            it.init(
                false,
                org.bouncycastle.crypto.params.Ed25519PublicKeyParameters(publicKeyBytes)
            )
            it.update(hashBytes, 0, hashBytes.size)
        }

        val result = verifier.verifySignature(signatureBytes)

        assertTrue(result)
    }
}

private fun getKeyInfo(privateKey: PrivateKey): KeyInfo {
    val keyStore = KeyStore.getInstance("AndroidKeyStore").also {
        it.load(null)
    }
    val factory = KeyFactory.getInstance(privateKey.algorithm, keyStore.provider)
    return factory.getKeySpec(privateKey, KeyInfo::class.java) as KeyInfo
}
```

### とりあえず使えるけど
AndroidKeyStoreでEd25519が使える。個人的には大ニュースなのですが、実際に使ってみると、署名はできても検証ができないなど、かなり急ぎで入れた印象があります。

AndroidKeyStoreで生成したEd25519の鍵、公開鍵の型は`AndroidKeyStoreEdECPublicKey`となっています（が、`@hide`が付いているのでクラス自体は見えない）。

このクラス名を手がかりに[コミットログ](https://cs.android.com/android/_/android/platform/frameworks/base/+/143fa39384d69d4de7a92ce64b2a7a2ac0ba8728)を見たところ、追加されたのは2022年の5月5日、Android 12以降に含まれているようです。

秘密鍵（`AndroidKeyStoreEdECPrivateKey`）は、そのまま`Signature`を使って署名はできるのですが、公開鍵でverifyしようとするとサポートしていない例外が出ます。どういうことだ。

```
java.security.InvalidKeyException: No installed provider supports this key: android.security.keystore2.AndroidKeyStoreEdECPublicKey
```

しょうがないので、署名はAndroidKeyStore、検証はBouncyCastleに任せることにしました。

### 公開鍵はX.509

BouncyCastleはEd25519の公開鍵は32バイトの長さを持つ値を想定しています。

一方、AndroidKeyStoreで生成したEd25519の公開鍵は「X.509エンコード」と[コミットログ](https://cs.android.com/android/_/android/platform/frameworks/base/+/143fa39384d69d4de7a92ce64b2a7a2ac0ba8728)に書いてあるので、`encode`で取った値はそのままではBouncyCastleでは使えません。

```
Implement support for Ed25519 signing keys in Android Keystore.
Because Conscrypt does not yet handle those keys, the Keystore classes
implement EdECPublicKey directly and parse the keys.

Specifically, AndroidKeyStoreEdECPublicKey can take an encoded X.509 key
specification, validate the encoding is of an Ed25519 key, then parse
the oddity and Y point on the curve.
RFC8032 describes EdDSA signature scheme, particularly Ed25519.
RFC8410, Section 3, defines the OID for Ed25519 keys (1.3.101.112).
RFC8410, Section 4, describes the encoding of the public key.
```

ひとまず、前半を除いて32バイトの値を取り出すことで対応しています（もともとの[AndroidKeyStoreEdECPublicKey](https://cs.android.com/android/platform/superproject/+/main:frameworks/base/keystore/java/android/security/keystore2/AndroidKeyStoreEdECPublicKey.java)も固定値を連結しているだけのようです）。

### ソフトウェアベースかハードウェアベースか。それが問題だ

Ed25519の鍵はハードウェアベースなのか。
StrongBoxを有効にして生成するとエラーになるのでわかりやすいのですが、TEEやSEの中で生成されるかはわかりません。

確認のためKeyInfoを取ろうと頑張ってみましたが、いまのところ成功していません。

```
private fun getKeyInfo(privateKey: PrivateKey): KeyInfo {
    val keyStore = KeyStore.getInstance("AndroidKeyStore").also {
        it.load(null)
    }
    val factory = KeyFactory.getInstance(privateKey.algorithm, keyStore.provider)
    return factory.getKeySpec(privateKey, KeyInfo::class.java) as KeyInfo
}
```

```
java.security.NoSuchAlgorithmException: no such algorithm: EdDSA for provider AndroidKeyStore
```

Security Providerの一覧を取ってみましたが、この中に`EdDSA`がないので、まだ対応していない可能性があります。ドキュメントに記載していないのも、対応が十分ではないからかも知れません。

```
Security Provider Service
algorithm: EC
type: KeyFactory
className: android.security.keystore2.AndroidKeyStoreKeyFactorySpi

Security Provider Service
algorithm: RSA
type: KeyFactory
className: android.security.keystore2.AndroidKeyStoreKeyFactorySpi

Security Provider Service
algorithm: XDH
type: KeyFactory
className: android.security.keystore2.AndroidKeyStoreKeyFactorySpi
```

本記事の執筆現時点ではAndroidKeyStoreがEd25519をハードウェアとソフトウェア、どちらで取り扱っているかは不明です。

しかしながら、生成した`PrivateKey`を`encode`しても`null`が返ってくるところは確認しており、アプリケーションに秘密鍵が露出しないという点で、これまでのソフトウェアベースの鍵一択の状況よりもセキュリティは上がると考えます。

Ed25519（と、X25519）、今後の用途の広がりに期待をしています。

----

最後に宣伝です。

11月12日（日）、池袋サンシャインシティにて開催される「[技術書典15](https://techbookfest.org/event/tbf15)」に、サークル「[めがねをかけるんだ](https://techbookfest.org/organization/5173176170446848)」として参加させていただくことになりました。

サークル配置は「さ17」。

新刊は「AndroidKeyStoreと過ごした400日」です。

| タイトル | AndroidKeyStoreと過ごした400日 |
|:---- |:----------------- |
| 判型   | B5                |
| ページ数 | 32p（電子版）         |
| 頒布価格 | 500円（会場価格）            |
| 発行   | 個人サークル「めがねをかけるんだ」 |

本文サンプルおよび電子版販売ページは[こちら](https://techbookfest.org/product/cUVLF5TvwkhQU2t4K72RVj)

オフライン会場にも参加しますが、持ち込みは電子版だけなので、電子版を売るのとOpenPGP Cardアプリケーション「[Bocchi](https://keiji.github.io/bocchi-docs/)」の実演。あとは、参加者の皆さんとお話できるくらいの気持ちで行きます。

あ、OpenPGPのお互いの公開鍵に署名をしあうのもやってみたいので、OpenPGPの公開鍵を運用している人はぜひ声をかけてください。
