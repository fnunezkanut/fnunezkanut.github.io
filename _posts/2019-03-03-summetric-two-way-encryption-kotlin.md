---
published: true
layout: post
title: 'Symmetric strong two way encryption (in Kotlin)'
---
## Symmetric strong two way encryption (in Kotlin)

Quite a lot of the examples one might find on internet regarding symmetric two way encryption are either wrong or use weak ciphers. I threw together a [quick example in Kotlin on JVM](https://github.com/fnunezkanut/kotlin-symmetric-encryption-example)

<!--more-->

Here I decided to use a strong cipher *"AES/GCM/NoPadding"* with 256 bit key. Why? Because applications that care about security must avoid use of insecure or weak cryptographic ciphers to protect sensitive information. For example DES is considered insecure nowadays and can simply be brute forced. Unfortunately quite a lot of examples on internet in Java use encryption in a wrong manner.

One often sees examples like this
```java
SecretKey key = KeyGenerator.getInstance("DES").generateKey();
Cipher cipher = Cipher.getInstance("DES");
cipher.init(Cipher.ENCRYPT_MODE, key);
byte[] encoded = strToBeEncrypted.getBytes("UTF8");
byte[] encrypted = cipher.doFinal(encoded);
```

### Solution

Use the Advanced Encryption Standard (AES) algorithm in Galois/Counter Mode (GCM) to perform the encryption. Also pick a 256 bit key. Please keep in mind you would need at least Java 8 runtime as GCM is not available in earlier versions. My example probably wont work on Kotlin on Android, this is strictly for the JVM. Also please note 256 bit keys require that the "Unlimited Strength Jurisdiction Policy" files are installed and available to the Java runtime environment.
I am also using PBKDF2WithHmacSHA256 hashing of the encryption key to perform key stretching, this ensures brute force attacks take much longer https://en.wikipedia.org/wiki/PBKDF2

### Show me the code!

I made an example that works on strings (linked at start of post)

```kotlin
package com.github.fnunezkanut

fun main() {

    val key = "correct horse battery staple"
    println("supersecretkey: $key")

    val plaintext = "attack at dawn"
    val symmetricEncryption = SymmetricEncryption()
    println("plaintext: $plaintext")

    //encode
    val encrypted = symmetricEncryption.encrypt(
        plaintext = plaintext,
        secret = key
    )
    println("encrypted: $encrypted")

    //decode
    val decrypted = symmetricEncryption.decrypt(
        ciphertext = encrypted,
        secret = key
    )
    println("decrypted: $decrypted")
}
```
