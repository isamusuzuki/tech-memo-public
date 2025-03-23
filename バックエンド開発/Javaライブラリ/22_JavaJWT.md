# Java JWT

作成日 2025/02/06、更新日 2025/02/07

## 概要

Maven Repository => [Java JWT](https://mvnrepository.com/artifact/com.auth0/java-jwt)

リポジトリ => [auth0/java-jwt](https://github.com/auth0/java-jwt)

最新版 => 4.5.0 at 2025/01/29

サンプルコード => [EXAMPLES.md](https://github.com/auth0/java-jwt/blob/master/EXAMPLES.md)

## インストール

Add the dependency via Maven

```xml
<dependency>
  <groupId>com.auth0</groupId>
  <artifactId>java-jwt</artifactId>
  <version>4.5.0</version>
</dependency>
```

## JWT 生成

```java
try {
    Algorithm algorithm = Algorithm.RSA256(rsaPublicKey, rsaPrivateKey);
    String token = JWT.create()
        .withIssuer("auth0")
        .sign(algorithm);
} catch (JWTCreationException exception){
    // Invalid Signing configuration / Couldn't convert Claims.
}
```

## JWT ベリファイ

```java
String token = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXUyJ9.eyJpc3MiOiJhdXRoMCJ9.AbIJTDMFc7yUa5MhvcP03nJPyCPzZtQcGEp-zWfOkEE";
DecodedJWT decodedJWT;
try {
    Algorithm algorithm = Algorithm.RSA256(rsaPublicKey, rsaPrivateKey);
    JWTVerifier verifier = JWT.require(algorithm)
        // specify any specific claim validations
        .withIssuer("auth0")
        // reusable verifier instance
        .build();

    decodedJWT = verifier.verify(token);
} catch (JWTVerificationException exception){
    // Invalid signature/claims
}
```

## サポートする暗号化・復号化アルゴリズム

- HMAC with SHA-256/384/512
- RSASSA-PKCS1-v1_5 with SHA-256/384/512
- ECDSA wih curve P-256/384/521 and SHA-256/384/512

用語解説

- HMAC (Hash-based Message Authentication Code) ... ハッシュ関数
- RSASSA-PKCS1-v1_5 ... RSA 暗号の改良版
- ECDSA (Elliptic Curve Digital Signature Algorithm) ... 楕円曲線 DSA 署名方式
- SHA (Secure Hash Algorithm) ... ハッシュ関数

### 暗号強度について

[CRYPTREC 暗号リスト(電子政府推奨暗号リスト)](https://www.cryptrec.go.jp/list.html)

[【要点抽出】暗号強度要件（アルゴリズム及び鍵長選択）に関する設定基準](https://note.com/nikinusu318/n/n360fe01ebfab)

> 強度の高い暗号技術を採用さえすれば良いのではなく、最終的な暗号の強度は暗号鍵の長さも大きく関わります\
> 各暗号技術の解読しづらさを定量的に表した尺度を示しています\
> 112 ビット/128 ビット/192 ビット/256 ビットの 4 段階で表され、数字が大きいほど強い暗号を意味します
