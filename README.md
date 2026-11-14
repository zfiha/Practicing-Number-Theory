# Practicing-Number-Theory
Cryptography and Number Theory Lab Assignments using Python and Google Colab.
# Cryptography and Number Theory Lab

**Student Name:** Zannatul Fiha  
**Student ID:** It-23021  
**Course:** Cryptography and Cyber Law 
 **Programming Language:** Python  
**Platform:** Google Colab

---

## 1. Introduction

Cryptography is the technique of securing information by converting readable messages into an unreadable format. Number theory plays an important role in modern cryptography.

This project demonstrates four important topics:

1. Diffie-Hellman Key Exchange
2. RSA Encryption Algorithm
3. Shor's Algorithm
4. Many-Time Pad

All experiments were practiced using Python in Google Colab.

---

## 2. Diffie-Hellman Key Exchange

### Objective
To establish a shared secret key between two users over an insecure communication channel.

### Mathematical Formula

Public parameters:

\[
N=11,\quad g=7
\]

Private keys:

\[
a=2,\quad b=4
\]

Public values:

\[
A=g^a \bmod N
\]

\[
B=g^b \bmod N
\]

Shared secret key:

\[
K=B^a \bmod N=A^b \bmod N
\]

### Python Code

```python
N = 11
g = 7

a = 2
b = 4

A = pow(g, a, N)
B = pow(g, b, N)

key_A = pow(B, a, N)
key_B = pow(A, b, N)

print("Alice's Public Value:", A)
print("Bob's Public Value:", B)
print("Alice's Shared Key:", key_A)
print("Bob's Shared Key:", key_B)
```

### Output

```text
Alice's Public Value: 5
Bob's Public Value: 3
Alice's Shared Key: 9
Bob's Shared Key: 9
```

### Observation
Both users generate the same shared secret key without directly sending their private keys.

---

## 3. RSA Encryption Algorithm

### Objective
To demonstrate public-key encryption and decryption using the RSA algorithm.

### Mathematical Formula

Choose two prime numbers:

\[
p=5,\quad q=11
\]

Calculate:

\[
N=p\times q=55
\]

Euler's totient:

\[
\phi(N)=(p-1)(q-1)=40
\]

Public exponent:

\[
e=7
\]

Private exponent:

\[
d=23
\]

Encryption:

\[
C=M^e \bmod N
\]

Decryption:

\[
M=C^d \bmod N
\]

### Python Code

```python
p = 5
q = 11

N = p * q
phi = (p - 1) * (q - 1)

e = 7
d = 23

M = 2

C = pow(M, e, N)
decrypted = pow(C, d, N)

print("N =", N)
print("Phi(N) =", phi)
print("Public Key =", (e, N))
print("Private Key =", d)
print("Plaintext =", M)
print("Ciphertext =", C)
print("Decryptedtext =", decrypted)
```

### Output

```text
N = 55
Phi(N) = 40
Public Key = (7, 55)
Private Key = 23
Plaintext = 2
Ciphertext = 18
Decryptedtext = 2
```

### Observation
The original plaintext is successfully recovered after decryption.

---

## 4. Shor's Algorithm

### Objective
To demonstrate the mathematical idea of factoring a composite number using period finding.

### Mathematical Formula

Given:

\[
N=35,\quad a=8
\]

The period \(r\) satisfies:

\[
a^r \equiv 1 \pmod N
\]

For this example:

\[
8^4 \equiv 1 \pmod {35}
\]

Therefore:

\[
r=4
\]

Factors can be calculated using:

\[
\gcd(a^{r/2}-1,N)
\]

\[
\gcd(a^{r/2}+1,N)
\]

### Python Code

```python
import math

N = 35
a = 8
r = 4

factor1 = math.gcd(a**(r//2) - 1, N)
factor2 = math.gcd(a**(r//2) + 1, N)

print("N =", N)
print("a =", a)
print("Period (r) =", r)
print("GCD 1 =", factor1)
print("GCD 2 =", factor2)
print("Prime Factors:", factor1, "and", factor2)
```

### Output

```text
N = 35
a = 8
Period (r) = 4
GCD 1 = 7
GCD 2 = 5
Prime Factors: 7 and 5
```

### Observation
The example finds the two factors of 35 using the period and GCD calculations. This is a classical mathematical illustration, not an actual quantum-computer implementation.

---

## 5. Many-Time Pad

### Objective
To understand XOR encryption and demonstrate the security problem caused by reusing the same One-Time Pad key.

### 5.1 String to Hexadecimal Conversion

```python
def toHex(s):
    return ''.join(format(ord(ch), '02x') for ch in s)

def toStr(s):
    return bytes.fromhex(s).decode('ascii')

message = "Hello World"

hex_message = toHex(message)

print("Hexadecimal:", hex_message)
print("Original Text:", toStr(hex_message))
```

### 5.2 XOR Encryption and Decryption

The XOR operation follows these rules:

| Input A | Input B | Output |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

Mathematical property:

\[
(A\oplus B)\oplus B=A
\]

### Python Code

```python
def Xor(s1, s2):
    if len(s1) != len(s2):
        raise ValueError("Both inputs must have equal length")

    result = ""

    for i in range(0, len(s1), 2):
        x = int(s1[i:i+2], 16)
        y = int(s2[i:i+2], 16)
        result += format(x ^ y, '02x')

    return result

message = "secret message"
key = "my secret keys"

hex_message = toHex(message)
hex_key = toHex(key)

ciphertext = Xor(hex_message, hex_key)
recovered_message = toStr(Xor(ciphertext, hex_key))

print("Original Message:", message)
print("Ciphertext:", ciphertext)
print("Recovered Message:", recovered_message)
```

### 5.3 Reusing the Same Key

When the same key is used for two different messages:

\[
C_1=M_1\oplus K
\]

\[
C_2=M_2\oplus K
\]

Then:

\[
C_1\oplus C_2=M_1\oplus M_2
\]

### Python Code

```python
message1 = "steal the secret"
message2 = "the cat the bird"

key = "supersecretverys"

hex_message1 = toHex(message1)
hex_message2 = toHex(message2)
hex_key = toHex(key)

ciphertext1 = Xor(hex_message1, hex_key)
ciphertext2 = Xor(hex_message2, hex_key)

xor_ciphertexts = Xor(ciphertext1, ciphertext2)
xor_messages = Xor(hex_message1, hex_message2)

print("XOR of Ciphertexts:", xor_ciphertexts)
print("XOR of Messages:", xor_messages)

if xor_ciphertexts == xor_messages:
    print("XOR of ciphertexts is the same as XOR of messages!")
```

### Observation
When the same key is reused to encrypt two different messages, XOR of the ciphertexts becomes equal to XOR of the original messages.

### Conclusion
Reusing the same One-Time Pad key can reveal information about the original messages. Therefore, a One-Time Pad key must never be reused.

---

## 6. Overall Conclusion

Through these experiments, we learned the basic concepts of cryptography and number theory.

- Diffie-Hellman demonstrates secure shared-key establishment.
- RSA demonstrates public-key encryption and decryption.
- Shor's Algorithm demonstrates the mathematical idea of period finding for integer factorization.
- Many-Time Pad demonstrates XOR encryption and the security risk of reusing a key.

These experiments helped us understand important mathematical concepts behind cryptographic systems.

---

## 7. Tools and Technologies

- Python
- Google Colab
- Jupyter Notebook
- GitHub
- Markdown

## 8. Project Files

- `Cryptography_Assignment.ipynb` — Python implementations and experiment outputs.
- `README.md` — Project description, equations, codes, observations, and conclusions.
