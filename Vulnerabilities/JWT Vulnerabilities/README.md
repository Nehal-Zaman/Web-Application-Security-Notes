# JWT Vulnerabilities

# What are JWTs?

JSON Web Tokens (JWT) are standard format for sending cryptographically signed JSON data between systems.

# Format of a JWT token.

A JWT token consists of 3 parts:

- Header
- Payload
- Signature

The 3 parts are joined by a dot(`.`).

The **header** and the **payload** are just the base64ed URL encoded JSON objects.

The **header** consists of the metadata of the JSON token.

The **payload** consists of the actual data about the user.

The **header** and the **payload** are easily modifiable by anyone if he has access to the token. Therefore, the security of the JWT token is heavily dependent on the cryptographic signature.

JWT Token: `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoiTmVoYWwgWmFtYW4iLCJpc0FkbWluIjpmYWxzZX0.T2IwoOiP1JSAR11wkXDQQB7J-7COa8m7efi60g14qEM`

Header: `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9`

```json
{"alg":"HS256","typ":"JWT"}
```

Payload: `eyJuYW1lIjoiTmVoYWwgWmFtYW4iLCJpc0FkbWluIjpmYWxzZX0`

```json
{"name":"Nehal Zaman","isAdmin":false}
```

Signature: `T2IwoOiP1JSAR11wkXDQQB7J-7COa8m7efi60g14qEM`

# JWT signature

The JWT **signature** is generated by hashing the JWT **header** and **payload** with a secret key in the server side.

- Since the signature is generated based on the **header** and **payload**, any tampering in the **header** and **payload** will lead to the mismatch of signatures.
- Without knowing the secret key, it is not possible to generate the JWT token of a given **header** and **payload**.

JWT are extended by **JWS** (JSON Web Signature) and **JWE** (JSON Web Encryption).

# Different JWT header parameters

The *alg* parameter in the JWT header is mandatory. Apart from this header, there can be other headers that might be useful to an attacker:

- `jwk`: Contains a JSON object representing the key.
- `jku`: Contains a URL from which server can request a set of keys in which a key is used to sign the token.
- `kid`: Contains a unique identifier that is used to select a key if there are multiple keys to choose from.
- `cty`: If we found a way to bypass signature verification, we can use the `cty` header that contains the content-type of the contents in JWT payload, to use `text/xml` or `application/x-java-serialized-object` that enables new attack vectors like `XXE` or `insecure deserialization`.
- `x5c`: Represents x509 certificate chain, very similar to jwk that can be used to inject self-signed certificates.

# Vulnerabilities in JWTs

- **Server accepting arbitrary signatures**: Libraries provides 2 methods to get payload out of JWT: **decode()** and **verify()**. Decode is used to simply get the payload from the JWT, whereas verify is used to verify the signature and decode it. Sometime, developers make the mistake of using **decode()** instead of **verify()**, essentially allowing the attacker to provide any arbitrary signature.
- **Server accepting tokens with no signature**: The *alg* field in the JWT header defines which algorithm must be used to verify the token. It can be set to **none** to provide the server with a signature-less token (unsecured JWT). However there might be filters in place for string keywords like None, which can be bypassed by classic obfuscation techniques. Moreover, the trailing dot must be there after the payload.
- **Brute forcing weak JWT secrets using hashcat**: If the server is using a weak JWT secret to sign the token, we can brute force the token with a wordlist of keys using hashcat. `hashcat -a 0 -m 16500 jwt wordlist.txt`.
- **Injecting self-signed JWT using JWK header**: Sometimes, server verifies the JWT token using the JWK header value present in the token itself. We can exploit this situation by generating our RSA public and private key pair, modify the JWT payload, sign it using the private key and inject the public key in the JKU header.
- **Injecting self-signed JWT using JKU header**: Instead of JWK, servers may also use JKU which is essentially a URL containing a set of keys. JWK sets are sometimes exposed on the standard endpoints like `/.well-known/jwks.json`. Sometimes there may be filters in place for the URL in JKU header, which might be bypassed using standard obfuscation techniques. The json object in the URL mentioned in JKU follows the format: `{ "keys": [ <jwk1>, <jwk2> ... <jwk3> ] }`.
- **Injecting self-signed JWT using kid parameter**: The `kid` header parameter is a value that is used by the server to identify the key used for signing the JWT. The value of `kid` can be anything - it may be a filename that contains the JWK set, it may be a unique string in database that maps to the JWK set or anything else. The `kid` is especially dangerous if a symmetric algorithm is used to sign the token. In that case if the `kid` parameter represents a file name, it might be exploitted to use the directory traversal techniques to find the guessable static file that contains the JWK used to sign the token. In linux, the simplest way is to use `/dev/null` which always returns null as it is an empty file. Signing a base64 encoded null byte will result a null byte. If the kid parameter instead uses database, it might be vulnerable to SQL injections. Essentially, `kid` parameter may lead to RCE, Directory traversal, SQLi kind of vulnerability.
- **JWT Confusion Attack**: If an attacker forges and sends a JWT token using an algorithm that is not anticipated by the developer, he might be successful. This is known as algorithm confusion attack.

# Algorithm Confusion Attack steps:

**NOTE**: You will need the **JWT Editor** plugin for burpsuite for this … 

- **Obtain the server’s public key**
    - Server may expose JWKs through standard endpoints like `jwks.json` or `/.well-known/jwks.json`.
    - If the server does not expose JWKs, we can make use of 2 JWT tokens to find the probable key.
        - We can use the sig2n from portswigger: `docker run --rm -it portswigger/sig2n <token1> <token2>`
        - We have to hit and trial using the the modified JWTs to find the correct key.
        - Then we can go ahead with the algorithm confusion attack.
- Convert the public key to suitable format.
    - Copy the JWK that you found in the server.
    - In the **JWT editor keys** tab, click on **New RSA key**.
    - Paste the JWT and click the **PEM** radio button.
    - Copy the PEM contents.
    - Go to decoder and base64 encode the PEM.
    - Now create a symmetric key in the **JWT Editor Keys** tab.
    - Click **generate**.
    - Replace the value of “k” with the copied **PEM**.
    - Save the key.
- Modify your JWT.
- Sign it using the key you saved earlier.