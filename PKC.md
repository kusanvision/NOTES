---
title: Notes on Public Key Cryptosystem
author: |
        | Dr. Kusan Biswas
        | kusan.biswas@dseu.ac.in
date: \today
toc: true
numbersections: true
geometry:
        - a4paper
        - top=3cm
        - bottom=3cm
        - right=2.75cm
        - left=2.75cm
urlcolor: blue
include-before:
- '`\newpage{}`{=latex}'
---
[comment]: # $pandoc PKC.md --toc -o PKC.pdf -s -V geometry:a4paper (not used, latest below)
[comment]: # $pandoc PKC.md --toc -o PKC.pdf --template=mytemplate.tex

# RSA Public Key Cyptosystem
The RSA Public Key Cryptosystem, named after its inventors Ron Rivest, Adi Shamir and Leonard
Adleman is on of the most important cryptographic algorithms. It is a key part of the TLS and SSL
protocols which are the backbones of internet security. In RSA cryptosystem, all parties first
generate their key-pairs. In our case, let's say there are two parties -- Amit and Rakesh who wish
to send and receive encrypted data among themselves. Both Amit and Rakesh generates their key-pairs.
Amit generates his public and private keys and similarly Rakesh also generates his public and
private key. Amit and Rakesh make their public keys public, but saves the privates keys secretly.
This means, Amit knows Rakesh's public key and Rakesh also knows Amit's public key. However, Amit's
private key is known only to Amit and no one else. Same goes for Rakesh's private key.

The most important point to remember about the RSA (or any public key cryptosystem for that matter)
is that the keys of a key-pair are related. They are related in the sense that, data encrypted with
one public key can only be decrypted with the corresponding private key. In our example, data
encrypted with Amit's public key can only be decrypted with Amit's private key!

Now suppose Rakesh wants to send encrypted data to Amit. He encrypts the plaintext using Amit's
public key and sends the ciphertext to Amit. This ciphertext can only be decrypted by Amit, because
Amit's private key is is available only with Amit and no one else. Since Rakesh knows that this
ciphertext can only be decrypted by Amit, Rakesh does not need to worry about eavesdropping in the
communication channel. He can be sure that even if he broadcasts the ciphertext, nobody except Amit
would be able to decrypt it!


## The RSA Algorithm
1. Key generation by Amit\vspace{0.1cm}
    - Select large primes $p$ and $q$
    - Calculate $n = pq$
    - Calculate $\phi(n) = (p-1)(q-1)$
    - Select integer $e$ such that $gcd(\phi(n), e) = 1$ and $1 < e < \phi(n)$
    - Calculate $d$ such that $ed \equiv 1\, mod\, \phi(n)$
    - Now, Amit's Public Key is: $K_{Pub}^{Amit} = \{e, n\}$
    - And Amit's Private Key is: $K_{Pvt}^{Amit} = \{d, n\}$ \vspace{0.2cm} 

2. Encryption by Rakesh\,(the sender) using Amit's Public Key \vspace{0.1cm}
    - Plaintext is $M$ such that $M < n$
    - Computer ciphertext $C = M^n\,mod(n)$ \vspace{0.2cm}

3. Decryption by Amit (the receiver) using Amit's Private Key \vspace{0.1cm}
    - Ciphertext C (received from Rakesh)
    - Decrypt and get plaintext message $M = C^d\,mod(n)$ \vspace{0.2cm}


## An Example of RSA

1. Key generation by Amit\vspace{0.1cm}
    - Select large(!) primes $p = 3$ and $q = 11$ . Small values are taken for the ease of 
        calculation in this example.
    - Therefore, $n = pq = 3\times11 = 33$
    - Therefore, $\phi(n) = (p-1)(q-1) = (3 - 1)(11 - 1) = 2\times10 = 20$
    - Select integer $e$ such that $gcd(20, e) = 1$ and $1 < e < 20$ . We can take $e = 3$, because
      $gcd(20, 3) = 1$, because $20$ and $3$ do not have any common factor.
    - Calculate $d$ such that $3\times d \equiv 1\, mod\, 20$. We see that $d = 7$ satisfies the
      condition. Because, $3\times 7 \equiv 1\, mod\, 20$.
    - Now, Amit's Public Key is: $K_{Pub}^{Amit} = \{3, 33\}$
    - And Amit's Private Key is: $K_{Pvt}^{Amit} = \{7, 33\}$ \vspace{0.2cm} 

2. Encryption by Rakesh (the sender) using Amit's Public Key \vspace{0.1cm}
    - Let's say plaintext message is $M = 5$.
    - Computer ciphertext $C = 5^3\,mod\,33$\vspace{0.1cm}
    \begin{align*}
     C &= 5^3\,mod\,33\\
      & = 125\,mod\,33\\
      & = 26
    \end{align*}

3. Decryption by Amit (the receiver) using Amit's Private Key \vspace{0.1cm}
    - Ciphertext $C = 26$ (received from Rakesh)
    - Decrypt and get plaintext message $M = C^d\,mod(n) = 26^7\,mod\,33$

    \begin{align*}
     M &= 26^7\,mod\,33\\
      & = \Bigl((26\times26)\,mod\,33 \times (26\times26)\,mod\,33 \times (26\times26)\,mod\,33 \times
      26\,mod\,33\Bigr)\,mod\,33\\
      & = (16\times16\times16\times26)\,mod\,33\\
      & = 106496\,mod\,33\\
      & = 5\\
    \end{align*}

## Exercises on RSA

1. Take $p = 5$, $q = 11$ and plaintext message $M = 9$. Take a suitable value of $e$. Then
   calculate $d$ and compute the ciphertext $C$. Then decrypt $C$.
2. Take $p = 7$, $q = 11$. Take $e = 17$. Compute $d$ and then encrypt $M = 8$
3. Take $p = 11$, $q = 13$, $e = 11$ and message $M = 7$. Compute $d$ and encrypt $M$.

\vspace{1cm}
# The Diffie--Hellman Key Exchange Algorithm

The Diffie--Hellman Key Exchange algorithm was invented by Whitefield Diffie and Martin Hellman in
1976. It enables two parties to calculate a single secret key without first exchanging
(\emph{i.e.}, transmittig) anything secret. In other words, the two parties first exchange with each
other some \emph{public information}, and since these information are public, these information can
be transmitted over an insecure channel. Using these public information, the two parties calculate
two values, which turn out to be the \emph{same}!. This shared secret value can not be computed any
third party even with the knowledge of the public information the two parties exchanged over an
insecure channel! This shared secret value can be used as the secret key of a Private Key
Cryptographic algorithm such as AES.

Imagine two friends -- Naveen and Praveen, wish to secretly transfer a big amount of data to each
other. They have two options. One option is to use a public key cryptography algorithm such as the
RSA. However, the RSA encryption and decryption processes are slow to run when the amount of data
that is to be transmitted is large. The second option is to use a symmetric key cryptography
algorithm such as AES. However, in the symmetric key cryptography there is only one key which must
be used by both Naveen and Praveen. Either Naveen or Praveen must first generate the key. Let's say
Naveen generates the key. Praveen also should have this key. How would Naveen share the key to
Praveen? Naveen can't send the key over the network because the network is not secure yet. Naveen
can visit Praveen and give the key to Praveen in person. This is not always feasible, because the
Naveen and Praveen may be a very long distance apart. This is where the DHKE algorithm come into
picture. The DHKE algorithm works as follows.

## The Diffie--Hellman Algorithm

1. Creation of public parameters\vspace{0.1cm}
    - Naveen chooses a prime number $p$ and a base $g$
    - Naveen sends both $p$ and $g$ to Praveen
    - $p$ and $g$ can be sent to Praveen over an insecure channel, because these are public
      parameters. A malicious third party (i.e., a hacker) may intercept these parameters, but it is
      not a problem.\vspace{0.2cm}

2. Private computation by Naveen and Praveen \vspace{0.1cm}
    - Naveen chooses an integer $a$ and keeps it secret
    - Praveen chooses an integer $b$ and keeps it secret
    - Naveen computes $A = g^a\,mod\,p$
    - Praveen computer $B = g^b\,mod\,p$ \vspace{0.2cm}

3. Public exchange of values \vspace{0.1cm}
    - Naveen sends $A$ to Praveen over the insecure channel. No problem if any hacker intercepts it.
    - Praveen sends $B$ to Naveen over the insecure channel. A hacker may read it, but no problem.
    \vspace{0.2cm}

4. Further private computation \vspace{0.1cm}
    - Naveen computes $A' = B^a\,mod\,p$
    - Praveen computes $B' = A^b\,mod\,p$
    - $A' = B'$ is the shared secret key! Mathematical properties of modular arithmetic ensure that
      the values of $A'$ and $B'$ computed this way are the \emph{same}! This way, two parties --
      Naveen and Praveen \emph{magically} compute the same secret key by exchanging a few public
      information. Once the two parties have the same secret key, they can exchange messages
      encrypted with symmetric key cryptographic algorithms such as AES and DES.

## An Example of Diffie--Hellman Algorithm

1. Creation of public parameters \vspace{0.1cm}
    - Naveen chooses a prime number $p = 23$ and a base $g = 5$
    - Naveen sends both $p = 23$ and $g = 5$ to Praveen \vspace{0.2cm}

2. Private computation by Naveen and Praveen \vspace{0.1cm}
    - Naveen chooses an integer $a = 6$ and keeps it secret
    - Praveen chooses an integer $b = 15$ and keeps it secret
    - Naveen computes $A = g^a\,mod\,p$      
        \begin{align*}
            A &= g^a\,mod\,p\\
            & = 5^6\,mod\,23\\
            & = 8
        \end{align*}

    - Praveen computer $B = g^b\,mod\,p$
        \begin{align*}
            A &= g^a\,mod\,p\\
            & = 5^{15}\,mod\,23\\
            & = 19
        \end{align*}

3. Public exchange of values \vspace{0.1cm}
    - Naveen sends $A = 8$ to Praveen
    - Praveen sends $B = 19$ to Naveen
    \vspace{0.2cm}

4. Further private computation \vspace{0.1cm}
    - Naveen computes $A' = B^a\,mod\,p$
        \begin{align*}
            A' &= B^a\,mod\,p\\
            & = 19^6\,mod\,23\\
            & = 2
        \end{align*}

    - Praveen computes $B' = A^b\,mod\,p$
        \begin{align*}
            B' &= A^b\,mod\,p\\
            & = 8^{15}\,mod\,23\\
            & = 2
        \end{align*}

    - $A' = B' = 2$ is the shared secret key! This way, two parties -- Naveen and Praveen
    \emph{magically} compute the same secret key by exchanging a few public information. Now Naveen
    and Praveen have the same secret key, therefore they can exchange messages encrypted with
    symmetric key cryptographic algorithms such as AES and DES.


## Exercises on Diffie--Hellman Key Exchange Algorithm

1. Take public parameters $p = 7$, $g = 3$. First party's secret $a = 2$ and second party's secret
   $b = 5$. Find out the secret key that both parties compute.

2. Take $p = 17$, $g = 5$. Alice's secret value $a = 4$ and Bob's secret value $b = 6$. Find out the
   secret key that both Alice and Bob compute^[Typeset in markdown backed by \LaTeX ! Hosted at
   [https://github.com/kusanvision/NOTES](https://github.com/kusanvision/NOTES)]
