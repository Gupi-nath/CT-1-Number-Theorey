<!--
# Student: **Gupinath chaterjee**
# Student ID: IT23019
Project: Number Theory and Cryptography Workshop
-->

<h1 align="center">Number Theory and Cryptography Workshop</h1>

<h2 align="center">
  <b> Name:&nbsp&nbsp</b> Gupinath chaterjee &nbsp; | &nbsp;
  <b> ID:&nbsp&nbsp</b> IT23019
</h2>

<p align="center">
  <i>A mathematical and computational study of number theory, modular arithmetic, and classical public-key cryptography using Python.</i>
</p>

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Learning Objectives](#2-learning-objectives)
3. [Environment and Tools](#3-environment-and-tools)
4. [Foundations of Number Theory](#4-foundations-of-number-theory)
5. [Divisibility, Primes, and Factorization](#5-divisibility-primes-and-factorization)
6. [Greatest Common Divisor and Euclidean Algorithm](#6-greatest-common-divisor-and-euclidean-algorithm)
7. [Modular Arithmetic](#7-modular-arithmetic)
8. [Congruences and Modular Inverses](#8-congruences-and-modular-inverses)
9. [Fermat's Little Theorem and Euler's Theorem](#9-fermats-little-theorem-and-eulers-theorem)
10. [Chinese Remainder Theorem](#10-chinese-remainder-theorem)
11. [Cryptography: Core Ideas](#11-cryptography-core-ideas)
12. [RSA Public-Key Cryptosystem](#12-rsa-public-key-cryptosystem)
13. [Python Practice and Experiments](#13-python-practice-and-experiments)
14. [Worked Examples](#14-worked-examples)
15. [Security Notes and Limitations](#15-security-notes-and-limitations)
16. [Learning Outcomes](#16-learning-outcomes)

---

## 1. Introduction

Number theory is the branch of mathematics that studies integers and their properties. It includes divisibility, prime numbers, greatest common divisors, modular arithmetic, and integer equations. These ideas are not only mathematically interesting; they are also essential building blocks of modern cryptography.

Cryptography studies techniques for protecting information. It helps provide confidentiality, integrity, authentication, and secure communication. Public-key cryptography uses a pair of mathematically related keys: a public key that can be shared and a private key that must be kept secret.

This project presents the core mathematical concepts and demonstrates selected algorithms with Python. The emphasis is on understanding the mathematics, translating it into executable code, and checking results with small reproducible experiments.

> **Workshop source:** [Number Theory and Cryptography Workshop â€” mnp-club](https://github.com/mnp-club/Number_Theory_and_Cryptography_Workshop)
>
> This README is a consolidated study document. It is not a claim that every item in the source repository has been independently completed. Lecture-specific exercises should be checked against the original slides and assignments.

## 2. Learning Objectives

By studying and running the examples in this document, the learner should be able to:

- Explain divisibility, prime numbers, factorization, GCD, and modular arithmetic.
- Implement the Euclidean and Extended Euclidean Algorithms.
- Calculate modular powers and modular multiplicative inverses.
- State and apply Fermat's Little Theorem, Euler's theorem, and the Chinese Remainder Theorem.
- Explain the basic purpose of encryption, decryption, public keys, and private keys.
- Demonstrate the mathematical steps of a small RSA example.
- Use Python to verify mathematical results and explore examples.

## 3. Environment and Tools

The examples use standard Python and can be executed in either [Google Colab](https://colab.research.google.com/) or a local Jupyter Notebook.

**Suggested setup**

```bash
python --version
pip install jupyter matplotlib
jupyter notebook
```

Google Colab requires no local installation. Create a new notebook, copy a code cell from this README, and run it with **Shift + Enter**.

The examples use Python's built-in integer arithmetic and `pow(a, b, m)`. The latter computes modular exponentiation efficiently and avoids constructing a potentially enormous intermediate integer.

## 4. Foundations of Number Theory

### 4.1 Integers

The set of integers is

\[
\mathbb{Z}=\{\ldots,-3,-2,-1,0,1,2,3,\ldots\}.
\]

Number theory primarily studies integer properties and relationships.

### 4.2 Divisibility

For integers \(a\) and \(b\), where \(a\ne 0\), we say that **a divides b** if there exists an integer \(k\) such that

\[
b=ak.
\]

It is written as \(a\mid b\). For example, \(4\mid 20\), because \(20=4\times5\). However, \(4\nmid 22\), because no integer multiplied by 4 gives 22.

### 4.3 Quotient and Remainder

For every integer \(a\) and positive integer \(n\), there exist unique integers \(q\) and \(r\) such that

\[
a=qn+r,\qquad 0\le r<n.
\]

Here, \(q\) is the quotient and \(r\) is the remainder. For example,

\[
29=4\times7+1.
\]

Python practice:

```python
a = 29
n = 7
q, r = divmod(a, n)

print("Quotient:", q)
print("Remainder:", r)
print("Check:", q * n + r == a)
```

Expected output:

```text
Quotient: 4
Remainder: 1
Check: True
```

## 5. Divisibility, Primes, and Factorization

### 5.1 Prime and Composite Numbers

A **prime number** is an integer greater than 1 whose only positive divisors are 1 and itself. Examples include 2, 3, 5, 7, 11, and 13. The number 2 is the only even prime.

A **composite number** is an integer greater than 1 that has more than two positive divisors. For example, \(12=2^2\times3\).

The number 1 is neither prime nor composite.

### 5.2 Testing Whether a Number Is Prime

If a composite integer \(n\) has a factor greater than 1, it must have at least one factor no larger than \(\sqrt n\). Therefore, trial division only needs to test possible divisors up to the square root.

```python
def is_prime(n: int) -> bool:
    if n < 2:
        return False
    if n == 2:
        return True
    if n % 2 == 0:
        return False

    d = 3
    while d * d <= n:
        if n % d == 0:
            return False
        d += 2
    return True

for value in [1, 2, 3, 4, 17, 21, 29, 35]:
    print(value, is_prime(value))
```

Expected output:

```text
1 False
2 True
3 True
4 False
17 True
21 False
29 True
35 False
```

### 5.3 Prime Factorization

The Fundamental Theorem of Arithmetic states that every integer greater than 1 can be represented as a product of prime numbers, uniquely apart from the order of the factors.

For example,

\[
360=2^3\times3^2\times5.
\]

```python
def prime_factorization(n: int) -> dict:
    if n < 1:
        raise ValueError("n must be a positive integer")

    factors = {}
    divisor = 2
    while divisor * divisor <= n:
        while n % divisor == 0:
            factors[divisor] = factors.get(divisor, 0) + 1
            n //= divisor
        divisor += 1

    if n > 1:
        factors[n] = factors.get(n, 0) + 1
    return factors

print(prime_factorization(360))
```

Expected output:

```text
{2: 3, 3: 2, 5: 1}
```

## 6. Greatest Common Divisor and Euclidean Algorithm

### 6.1 Greatest Common Divisor (GCD)

The greatest common divisor of two integers, not both zero, is the largest positive integer that divides both. It is denoted by \(\gcd(a,b)\).

For example, the positive divisors of 18 are \(1,2,3,6,9,18\), and those of 30 are \(1,2,3,5,6,10,15,30\). Thus,

\[
\gcd(18,30)=6.
\]

### 6.2 Euclidean Algorithm

The Euclidean Algorithm uses the identity

\[
\gcd(a,b)=\gcd(b,a\bmod b),\qquad b\ne0.
\]

Repeatedly replace the pair \((a,b)\) with \((b,a\bmod b)\) until the second value becomes zero. The remaining nonzero value is the GCD.

Example:

\[
\begin{aligned}
30 &= 18\times1+12\\
18 &= 12\times1+6\\
12 &= 6\times2+0
\end{aligned}
\]

Therefore, \(\gcd(30,18)=6\).

```python
def gcd_euclid(a: int, b: int) -> int:
    a, b = abs(a), abs(b)
    while b != 0:
        a, b = b, a % b
    return a

print(gcd_euclid(30, 18))
print(gcd_euclid(252, 105))
```

Expected output:

```text
6
21
```

### 6.3 Extended Euclidean Algorithm

The Extended Euclidean Algorithm finds integers \(x\) and \(y\) satisfying BÃ©zout's identity:

\[
ax+by=\gcd(a,b).
\]

```python
def extended_gcd(a: int, b: int):
    if b == 0:
        return a, 1, 0
    g, x1, y1 = extended_gcd(b, a % b)
    x = y1
    y = x1 - (a // b) * y1
    return g, x, y

g, x, y = extended_gcd(30, 18)
print("gcd:", g)
print("x, y:", x, y)
print("Verification:", 30*x + 18*y)
```

One valid output is:

```text
gcd: 6
x, y: -1, 2
Verification: 6
```

## 7. Modular Arithmetic

### 7.1 Definition

Two integers \(a\) and \(b\) are congruent modulo \(n\) if they have the same remainder when divided by \(n\). Equivalently,

\[
a\equiv b\pmod n
\quad\Longleftrightarrow\quad
n\mid(a-b).
\]

For example,

\[
17\equiv5\pmod{12},
\]

because \(17-5=12\), which is divisible by 12.

### 7.2 Modular Operations

If \(a\equiv b\pmod n\) and \(c\equiv d\pmod n\), then

\[
\begin{aligned}
a+c&\equiv b+d\pmod n,\\
a-c&\equiv b-d\pmod n,\\
ac&\equiv bd\pmod n.
\end{aligned}
\]

Example:

\[
(14+19)\bmod 6=33\bmod6=3.
\]

Python practice:

```python
n = 6
print((14 + 19) % n)
print((14 * 19) % n)
print((19 - 14) % n)
```

Expected output:

```text
3
2
5
```

### 7.3 Modular Exponentiation

Modular exponentiation calculates

\[
a^k\bmod n.
\]

A direct computation of \(a^k\) can create a very large integer. Repeated squaring computes the result efficiently by reducing modulo \(n\) at each step.

```python
def modular_power(a: int, exponent: int, modulus: int) -> int:
    if exponent < 0:
        raise ValueError("Exponent must be non-negative")
    if modulus <= 0:
        raise ValueError("Modulus must be positive")

    result = 1
    a %= modulus
    while exponent > 0:
        if exponent % 2 == 1:
            result = (result * a) % modulus
        a = (a * a) % modulus
        exponent //= 2
    return result

print(modular_power(7, 128, 13))
print(pow(7, 128, 13))  # Python's built-in verification
```

Both lines should print the same result:

```text
3
3
```

## 8. Congruences and Modular Inverses

### 8.1 Modular Multiplicative Inverse

An integer \(x\) is the modular inverse of \(a\) modulo \(n\) if

\[
ax\equiv1\pmod n.
\]

The inverse exists **if and only if**

\[
\gcd(a,n)=1.
\]

For example, the inverse of 3 modulo 7 is 5 because

\[
3\times5=15\equiv1\pmod7.
\]

### 8.2 Finding an Inverse Using Extended GCD

If \(ax+ny=1\), reducing both sides modulo \(n\) gives \(ax\equiv1\pmod n\). Thus, the coefficient \(x\), reduced modulo \(n\), is the inverse.

```python
def modular_inverse(a: int, n: int):
    if n <= 1:
        raise ValueError("Modulus must be greater than 1")
    g, x, _ = extended_gcd(a, n)
    if g != 1:
        return None
    return x % n

print(modular_inverse(3, 7))
print((3 * modular_inverse(3, 7)) % 7)
print(modular_inverse(6, 9))  # No inverse because gcd(6, 9) != 1
```

Expected output:

```text
5
1
None
```

## 9. Fermat's Little Theorem and Euler's Theorem

### 9.1 Fermat's Little Theorem

If \(p\) is prime and \(p\nmid a\), then

\[
a^{p-1}\equiv1\pmod p.
\]

An equivalent general statement is \(a^p\equiv a\pmod p\) for every integer \(a\).

Example: Since 7 is prime and 3 is not divisible by 7,

\[
3^6\equiv1\pmod7.
\]

```python
print(pow(3, 6, 7))
```

Expected output: `1`.

### 9.2 Euler's Totient Function

Euler's totient function \(\varphi(n)\) counts the integers from 1 through \(n\) that are relatively prime to \(n\).

For a prime \(p\),

\[
\varphi(p)=p-1.
\]

For two distinct primes \(p\) and \(q\),

\[
\varphi(pq)=(p-1)(q-1).
\]

```python
def euler_phi(n: int) -> int:
    if n < 1:
        raise ValueError("n must be positive")
    count = 0
    for k in range(1, n + 1):
        if gcd_euclid(k, n) == 1:
            count += 1
    return count

print(euler_phi(9))
print(euler_phi(10))
```

Expected output:

```text
6
4
```

### 9.3 Euler's Theorem

If \(\gcd(a,n)=1\), then

\[
a^{\varphi(n)}\equiv1\pmod n.
\]

Fermat's Little Theorem is the special case where the modulus is prime.

## 10. Chinese Remainder Theorem

The Chinese Remainder Theorem (CRT) states that if \(n_1,n_2,\ldots,n_k\) are pairwise coprime positive integers, then the system

\[
\begin{aligned}
x&\equiv a_1\pmod{n_1},\\
x&\equiv a_2\pmod{n_2},\\
&\vdots\\
x&\equiv a_k\pmod{n_k}
\end{aligned}
\]

has a unique solution modulo \(N=n_1n_2\cdots n_k\).

Example:

\[
x\equiv2\pmod3,\qquad x\equiv3\pmod5.
\]

The number 8 satisfies both congruences, and the solution is unique modulo 15:

\[
x\equiv8\pmod{15}.
\]

```python
def chinese_remainder(remainders, moduli):
    if len(remainders) != len(moduli) or not moduli:
        raise ValueError("Provide equally sized non-empty lists")

    # This implementation requires pairwise coprime moduli.
    for i in range(len(moduli)):
        for j in range(i + 1, len(moduli)):
            if gcd_euclid(moduli[i], moduli[j]) != 1:
                raise ValueError("Moduli must be pairwise coprime")

    total = 1
    for n in moduli:
        if n <= 0:
            raise ValueError("Moduli must be positive")
        total *= n

    result = 0
    for a, n in zip(remainders, moduli):
        partial = total // n
        inverse = modular_inverse(partial, n)
        result += a * partial * inverse

    return result % total

x = chinese_remainder([2, 3], [3, 5])
print(x)
print(x % 3, x % 5)
```

Expected output:

```text
8
2 3
```

## 11. Cryptography: Core Ideas

### 11.1 What Is Cryptography?

Cryptography is the study of mathematical techniques that protect information against unauthorized access or modification.

Important goals include:

- **Confidentiality:** only authorized parties can read the information.
- **Integrity:** unauthorized changes can be detected.
- **Authentication:** a party can verify who sent a message or which key was used.
- **Non-repudiation:** in suitable digital-signature systems, a signer cannot easily deny having signed a message.

### 11.2 Plaintext, Ciphertext, Encryption, and Decryption

- **Plaintext:** the original readable message.
- **Ciphertext:** the transformed message.
- **Encryption:** converting plaintext into ciphertext using an algorithm and key.
- **Decryption:** recovering plaintext using the appropriate key and algorithm.

A simplified model is

\[
C=E_K(M),\qquad M=D_K(C),
\]

where \(M\) is the message, \(C\) is ciphertext, and \(K\) represents a key.

### 11.3 Symmetric and Asymmetric Cryptography

In **symmetric cryptography**, the communicating parties use the same secret key for encryption and decryption. In **asymmetric (public-key) cryptography**, a key pair is used: a public key and a private key.

Public-key cryptography helps solve the key-distribution problem, but practical secure systems commonly combine public-key techniques with efficient symmetric encryption.

## 12. RSA Public-Key Cryptosystem

RSA is a public-key cryptosystem whose mathematical construction uses prime numbers, modular arithmetic, Euler's totient function, and modular inverses.

### 12.1 Key Generation

1. Choose two distinct prime numbers, \(p\) and \(q\).
2. Calculate \(n=pq\).
3. Calculate \(\varphi(n)=(p-1)(q-1)\).
4. Choose a public exponent \(e\) such that \(1<e<\varphi(n)\) and \(\gcd(e,\varphi(n))=1\).
5. Calculate \(d\), the modular inverse of \(e\) modulo \(\varphi(n)\):

\[
ed\equiv1\pmod{\varphi(n)}.
\]

The public key is \((n,e)\), and the private key includes \(d\) (along with the primes or other securely stored private parameters).

### 12.2 Encryption and Decryption

For a message represented as an integer \(m\) with \(0\le m<n\), encryption is

\[
c\equiv m^e\pmod n.
\]

Decryption is

\[
m\equiv c^d\pmod n.
\]

### 12.3 Small RSA Demonstration

Use the small primes \(p=61\) and \(q=53\).

\[
\begin{aligned}
n&=61\times53=3233,\\
\varphi(n)&=60\times52=3120.
\end{aligned}
\]

Choose \(e=17\). Since \(\gcd(17,3120)=1\), the inverse exists. One valid inverse is \(d=2753\), because

\[
17\times2753=46801=15\times3120+1.
\]

Therefore, the public key is \((3233,17)\), and the private exponent is 2753. For the illustrative message \(m=65\),

\[
c=65^{17}\bmod3233=2790,
\]

and

\[
2790^{2753}\bmod3233=65.
\]

Python demonstration:

```python
p = 61
q = 53
n = p * q
phi = (p - 1) * (q - 1)
e = 17

d = modular_inverse(e, phi)
message = 65
ciphertext = pow(message, e, n)
recovered = pow(ciphertext, d, n)

print("n:", n)
print("phi(n):", phi)
print("Public key:", (n, e))
print("Private exponent:", d)
print("Message:", message)
print("Ciphertext:", ciphertext)
print("Decrypted:", recovered)
print("Successful recovery:", recovered == message)
```

Expected output:

```text
n: 3233
phi(n): 3120
Public key: (3233, 17)
Private exponent: 2753
Message: 65
Ciphertext: 2790
Decrypted: 65
Successful recovery: True
```

### 12.4 Why Does RSA Work?

The exponents are selected so that \(ed\equiv1\pmod{\varphi(n)}\). Thus, for messages relatively prime to \(n\), Euler's theorem gives

\[
(m^e)^d=m^{ed}\equiv m\pmod n.
\]

A complete proof for all valid RSA messages, including messages not relatively prime to \(n\), can be given using the Chinese Remainder Theorem and the fact that \(p\) and \(q\) are prime.

### 12.5 Important Practical Warning

The small numbers above are for learning only. They are not secure. Real RSA requires large, randomly generated primes, secure padding, and a vetted cryptographic library. Textbook RSA without padding is deterministic and is not safe for real-world encryption. Do not use this educational implementation to protect real data.

## 13. Python Practice and Experiments

This section collects short experiments that can be run in Colab/Jupyter.

### Experiment 1: Compare the Built-in GCD

```python
import math

pairs = [(30, 18), (252, 105), (100, 75), (17, 19)]
for a, b in pairs:
    print((a, b), gcd_euclid(a, b), math.gcd(a, b))
```

The last two values on every line should match. This is a simple way to verify the Euclidean Algorithm against Python's standard library.

### Experiment 2: Verify Modular Inverses

```python
for a, n in [(3, 7), (7, 26), (10, 17), (6, 9)]:
    inverse = modular_inverse(a, n)
    if inverse is None:
        print(f"{a} has no inverse modulo {n}")
    else:
        print(f"{a}^(-1) mod {n} = {inverse}; check = {(a * inverse) % n}")
```

Whenever an inverse exists, the check value must be 1.

### Experiment 3: Verify Euler's Theorem

```python
for a, n in [(2, 9), (3, 10), (5, 12)]:
    if gcd_euclid(a, n) == 1:
        print(a, n, pow(a, euler_phi(n), n))
```

Each printed modular result should be 1.

### Experiment 4: Plot Remainders

The following optional visualization shows the remainder of \(x^2\) modulo 11 for integers from 0 to 30.

```python
import matplotlib.pyplot as plt

values = list(range(31))
remainders = [(x * x) % 11 for x in values]

plt.figure(figsize=(9, 4))
plt.scatter(values, remainders)
plt.xticks(range(0, 31, 2))
plt.yticks(range(11))
plt.xlabel("Integer x")
plt.ylabel("xÂ² mod 11")
plt.title("Quadratic residues modulo 11")
plt.grid(True)
plt.show()
```

The plot illustrates that modular arithmetic produces a repeating finite set of residues rather than an unbounded sequence.

## 14. Worked Examples

### Example 1: Find \(\gcd(252,105)\)

\[
\begin{aligned}
252&=2(105)+42,\\
105&=2(42)+21,\\
42&=2(21)+0.
\end{aligned}
\]

Therefore, \(\boxed{\gcd(252,105)=21}\).

### Example 2: Find the inverse of 7 modulo 26

We need \(7x\equiv1\pmod{26}\). Since \(7\times15=105=4\times26+1\), the inverse is

\[
\boxed{7^{-1}\equiv15\pmod{26}}.
\]

### Example 3: Calculate \(2^{10}\bmod13\)

\[
2^{10}=1024=13\times78+10,
\]

so

\[
\boxed{2^{10}\equiv10\pmod{13}}.
\]

### Example 4: Solve a CRT system

\[
x\equiv1\pmod4,\qquad x\equiv2\pmod5.
\]

The number 17 satisfies both conditions. Since 4 and 5 are coprime, the solution is unique modulo 20:

\[
\boxed{x\equiv17\pmod{20}}.
\]

## 15. Security Notes and Limitations

- Mathematical demonstrations with small values are not secure cryptographic systems.
- Prime generation for cryptography must use a cryptographically secure random source and appropriate primality testing.
- Real RSA encryption uses standardized padding such as RSA-OAEP; real RSA signatures use a suitable signature scheme such as RSA-PSS.
- In production software, use well-maintained cryptographic libraries rather than implementing cryptographic protocols from scratch.
- Keep private keys secret and use appropriate key storage, access control, and rotation practices.
- The educational examples here are intended for learning and experimentation only.

## 16. Learning Outcomes

After completing and checking the examples, the learner should be able to:

1. Describe fundamental properties of integers, divisibility, primes, and factorization.
2. Use the Euclidean Algorithm to compute GCD and the Extended Euclidean Algorithm to obtain BÃ©zout coefficients.
3. Perform modular addition, subtraction, multiplication, exponentiation, and inverse calculations.
4. Apply Euler's theorem, Fermat's Little Theorem, and the Chinese Remainder Theorem to small problems.
5. Explain the relationship between elementary number theory and RSA.
6. Write small Python functions to implement and verify mathematical procedures.

---

<p align="center">
  <b>Prepared by gupinath chaterjee</b><br>
  ID: IT23019
</p>
