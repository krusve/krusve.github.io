---
title: "Impact of Quantum Computing on cryptography"
date: 2023-03-13 18:25:00 +0100
categories: [Article, Cryptography]
tags: [cryptography, quantum computing, post quantum, security, frameworks]     # TAG names should always be lowercase
math: true
---



## Summary

This thesis aims to present a picture of the current state of quantum computing
and post-quantum cryptography. 
The goal is to answer how quantum computers
work to be able to understand why Shor’s algorithm is such a significant threat to
cryptography and will be able to break all asymmetric cryptography. Furthermore,
it will discuss the mitigation techniques of Quantum Cryptography and Post
Quantum Cryptography and answer why the only current strategy is to plan to
implement post-quantum cryptography. Lastly, the steps needed to implement
Post Quantum Cryptography are presented. Cryptographic agility, Cryptographic
inventory, and several risk frameworks are introduced to give organizations a clear
path on what steps are required to be prepared for the quantum future.
<br>
<br>


<!-- ## TABLE OF CONTENTS


[TABLE OF CONTENTS](#table-of-contents)

[LIST OF TABLES](#list-of-tables)

[TABLE OF FIGURES](#table-of-figures)

[LIST OF ABBREVIATIONS AND ACRONYMS](#list-of-abbreviations-and-acronyms)

[CHAPTER 1. INTRODUCTION](#chapter-1-introduction)

- [1.1 PROBLEM STATEMENT](#11-problem-statement)

- [1.2 GOAL](#list-of-abbreviations-and-acronyms)

- [1.3 STRUCTURES](#list-of-abbreviations-and-acronyms)

- [1.4 METHODOLOGY](#14-methodology)


[CHAPTER 2. QUANTUM COMPUTING AND PQC INTRODUCTION](#chapter-2-quantum-computing-and-pqc-introduction)

- [2.1 QUANTUM COMPUTING BASICS](#21-quantum-computing-basics)

- [2.1.1 QUBITS](#211-qubits)

- [2.1.2 POLARIZING EXPERIMENT](#212-polarizing-experiment)

- [2.1.3 ENTANGLEMENT](#213-entanglement)

- [2.1.4 GATES](#211-qubits)

- [2.1.5 SPEED-UP](#215-speed-up)

[CHAPTER 3. IMPACT OF QUANTUM COMPUTING ON CRYPTOGRAPHY](#chapter-3-impact-of-quantum-computing-on-cryptography)

[CHAPTER 4 MITIGATION TECHNOLOGIES](#chapter-4-mitigation-technologies)

- [4.1 POST QUANTUM CRYPTOGRAPHY (PQC)](#41-post-quantum-cryptography-pqc)

  - [4.1.1 LATTICE-BASED CRYPTO](#411-lattice-based-crypto)

  - [4.1.2 HASH-BASED CRYPTO](#412-hash-based-crypto)

- [4.2 QUANTUM CRYPTOGRAPHY](#42-quantum-cryptography)

  - [4.2.1 QUANTUM KEY DISTRIBUTION (QKD)](#421-quantum-key-distribution-qkd)

- [4.3 PQC VS. QKD](#43-pqc-vs-qkd)

[CHAPTER 5. WHAT COMPANIES NEED TO DO](#chapter-5-what-companies-need-to-do)

- [5.1 IMPLEMENTING QUANTUM CRYPTOGRAPHY](#51-implementing-quantum-cryptography)

- [5.2 IMPLEMENTING PQC](#52-implementing-pqc)

  - [5.2.1 CRYPTOGRAPHIC AGILITY](#521-cryptographic-agility)

  - [5.2.2 NIST CYBERSECURITY FRAMEWORK (CSF)](#522-nist-cybersecurity-framework-csf)

  - [5.2.3 QUANTUM RISK ASSESSMENT (QRA)](#523-quantum-risk-assessment-qra)

  - [5.2.4 CRYPTO AGILITY RISK ASSESSMENT FRAMEWORK (CARAF)](#524-crypto-agility-risk-assessment-framework-caraf)

[CHAPTER 6. CONCLUSION](#chapter-6-conclusion)

[CHAPTER 7. REFERENCES](#chapter-7-references)

[CHAPTER: 8. SHOR ’ S ALGORITHM](#chapter-8-shor--s-algorithm)

<br>
<br> -->



<!-- # LIST OF TABLES

- Table 1: Security Level now vs. <abbr title="Post Quantum Cryptography">PQC</abbr> <br>
- Table 2: X + Y > Z <br>
- Table 3: Develop research statement <br>
- Table 4: Define criteria for literary acquisition <br>
- Table 5: Define information retrieval systems <br>
- Table 6: Define search criteria <br>
- Table 7: Search strategy <br>
- Table 8: Evaluate potential literature <br>
- Table 9: Evaluation of suitable literature <br>
- Table 10: Incorporation of literature <br>
<br>
<br>


# TABLE OF FIGURES

- Figure 1: Two-dimensional qubit representation TABLE OF FIGURES
- Figure 2: Experiment with one filter
- Figure 3: Reorientation of photon
- Figure 4: Absorbing photon
- Figure 5: Experiment with two filters
- Figure 6: Experiment with three filters
- Figure 7: Shor’s algorithm time complexity
- Figure 8: Lattice example
- Figure 9: Shortest Integer Solution
- Figure 10: Quantum Key Distribution (<abbr title="QKD Algorithm (Bennet and Brassard 1984)">BB84</abbr>)
- Figure 11: Cryptographic Agility

<br>
<br>


# LIST OF ABBREVIATIONS AND ACRONYMS


<abbr title="Advanced Encryption Standard">AES</abbr> Advanced Encryption Standard

<abbr title="Application Programming Interface">API</abbr> Application Programming Interface

<abbr title="QKD Algorithm (Bennet and Brassard 1984)">BB84</abbr> QKD Algorithm (Bennet and Brassard 1984)

<abbr title="Crypto Agility Assessment Framework">CARAF</abbr> Crypto Agility Assessment Framework

<abbr title="Core Processing Unit">CPU</abbr> Core Processing Unit

<abbr title="Cybersecurity Framework">CSF</abbr> Cybersecurity Framework

<abbr title="Gigahertz">GHz</abbr> Gigahertz

<abbr title="GNU Privacy Guard">GnuPG</abbr> GNU Privacy Guard

<abbr title="Information Technology">IT</abbr> Informatin Technology

<abbr title="Java Development Kit">JDK</abbr> Java Development Kit

<abbr title="National Institute of Standards and Technology">NIST</abbr> National Institute of Standards and Technology

<abbr title="NatioNal Security Agency">NSA</abbr> NatioNal Security Agency

<abbr title="Pretty Good Privacy">PGP</abbr> Pretty Good Privacy

<abbr title="Padding Oracle On Downgraded Legacy Encryption">POODLE</abbr> Padding Oracle On Downgraded Legacy Encryption

<abbr title="Post Quantum Cryptography">PQC</abbr> Post Quantum Cryptography

<abbr title="Quantum Cryptography">QC</abbr> Quantum Cryptography

<abbr title="Quantum Key Distribution">QKD</abbr> Quantum Key Distribution

<abbr title="Quantum Risk Assessment">QRA</abbr> Quantum Risk Assessment

<abbr title="Rivest–Shamir–Adleman">RSA</abbr> Rivest–Shamir–Adleman

<abbr title="Signaling Cipher Suite Value">SCSV</abbr> Signaling Cipher Suite Value

<abbr title="Secure Hash Algorithm">SHA</abbr> Secure Hash Algorithm

<abbr title="Shortest Integer Solution Problem">SIS</abbr> Shortest Integer Solution Problem

<abbr title="Secure Socket Layer">SSL</abbr> Secure Socket Layer

<abbr title="Transport Layer Securit">TLS</abbr> Transport Layer Security

<abbr title="Winternitz one-time signature">WOTS</abbr> Winternitz one-time signature

<br>
<br> -->

# CHAPTER 1. INTRODUCTION

## 1.1 PROBLEM STATEMENT

In 1981 Richard P. Feynman ended a lecture with the words: “[...] because nature
isn’t classical, dammit, and if you want to make a simulation of nature, you’d
better make it quantum mechanical, and by golly it’s a wonderful problem,
because it doesn’t look so easy.” (Feynman, 1982, S. 486)<sup>61</sup>. Feynman held this
lecture when quantum computing was barely a theory. Now, 40 years
later, Quantum Computing poses such an immediate threat to the status quo that
<abbr title="National Institute of Standards and Technology">NIST</abbr> (National Institute of Standards and Technology) is choosing algorithms to
become the new standards for Public-Key Encryption and Key-establishments.<sup>1</sup>

Quantum computers utilize quantum mechanics, enabling them to do computations
much more efficiently than classical computers. Additionally, researchers at Google
proved Quantum supremacy; they created a quantum computer that did calculations
in a reasonable amount of time, while the classical counterpart would have needed
thousands of years.<sup>2</sup>
Thankfully, as Feynman said, creating a quantum computer is not easy, so work like
the quantum supremacy mentioned above does not apply to real-world problems.<sup>3</sup>
However, advancements in the field will eventually lead to significant technological
shifts.
One of which is the implication of the current cryptography. As we will see, the status
quo must undergo tremendous changes to keep encryption safe.

The main threat from <abbr title="Quantum Cryptography">QC</abbr> comes from their ability to efficiently factorize large prime
numbers, which is the technique modern cryptography relies on heavily. So, to
protect the security of data, systems, and communications, developing quantum-
safe methods is crucial to continue the safe operation of <abbr title="Information Technology">IT</abbr> while embracing a <abbr title="Quantum Cryptography">QC</abbr>
future.


## 1.2 GOAL

The goal of the thesis is to investigate the current status of quantum computing,
more specifically, its impact on cryptography, with the help of recent research and
literature.
The aim is to clarify the threat <abbr title="Quantum Cryptography">QC</abbr> poses to cryptography, to understand the involved
technologies, and to shed light on new developments in the field.
Throughout the thesis, the following questions are addressed:

- What is <abbr title="Quantum Cryptography">QC</abbr>, and how does it fundamentally work?
- Where does the threat come from, and what solutions are developing?
- What actions mitigate the danger posed by <abbr title="Quantum Cryptography">QC</abbr>?

In the end, actionable recommendations for securing infrastructure and how
companies can begin the process are made.


## 1.3 STRUCTURE

The thesis consists of three parts main parts:
For the later topics discussed in this thesis, it’s vital to understand the
fundamental mechanisms used by quantum computers. Therefore the first part
examines the current status of quantum computing. As it would go beyond the
scope of this thesis, this examination will be high-level.

The main threat posed by quantum computation is towards cryptography, as
algorithms can factorize efficiently, thereby eliminating the basis of modern
cryptography. Therefore, the second part of the thesis is a summary of recent
literature which provides the latest developments needed to evaluate the threat
of <abbr title="Quantum Cryptography">QC</abbr> and its impact on Cryptography.

In the final part, based on the understanding from the preceding chapters,
solutions and mitigation techniques for the quantum computing threat, such as
Post Quantum Cryptography and Quantum Cryptography, are analyzed to
develop recommendations for practical actions.



## 1.4 METHODOLOGY

Because this work aims to understand the field’s condition and discuss
developments and future mitigation approaches, an overall theoretical approach is
applied. Therefore Scientific papers and sources from authoritative institutions are
used to analyze the topics.
Though there are slightly different methodologies for the first and two last parts, due
to the fundamental nature of the first chapter, textbooks and other reliable
monographs are used to present the basic mechanisms behind quantum computing,
which are well-established knowledge.

For the last two parts, a range of sources is used to present the field’s current state
from all critical perspectives and opinions. For this purpose, keywords and topics
are defined, as well as criteria for accepting or rejecting sources.


# CHAPTER 2. QUANTUM COMPUTING AND PQC INTRODUCTION

The fundamental rules and functionality of Quantum Computing must be explained
to understand the risks and opportunities posed by quantum computing.

## 2.1 QUANTUM COMPUTING BASICS

Quantum computers are not related to classical computers, as they use quantum
mechanics, which inhibit specific mechanisms. Before diving into some of these
mechanisms, it’s vital to understand how qubits work contrary to a classical bit. A
classical bit is either a one or a zero. Meanwhile, a qubit can be one and zero, like
a classical bit, but also any value in-between.<sup>4</sup>


## 2.1.1 QUBITS

One way to represent qubits slightly simplified method is to use photons and their
polarization, which inhibit strong similarities to qubits.
When using a photon, measurements are done using polaroids with a particular filter
and can be done in praxis. The states of a qubit can be represented as a result of
the two unit vectors $\ket\uparrow$ and $\ket\rightarrow$, and is represented by the equation:

$$ \ket 𝑣 = 𝑎 \ket\uparrow+𝑏\ket\rightarrow (2.0) $$



By choosing non-zero values for a and b, the qubit takes on a superposition of $\ket\uparrow$
and $\ket\rightarrow$.<sup>5</sup> An example is the 45&deg; degrees line in Figure 1 , which is achieved by choosing both a and b as $\frac{1}{\sqrt{2}}$.





![Single Qubit State](/assets/img/crypto_article_images/single-qubit-state.png){: w="700"}
_Figure 1 : Two-dimensional qubit representation<sup>6</sup>_




The two variables, $a$ and $b$, are the amplitude of the vector in the direction $\ket\uparrow$ and
$\ket\rightarrow$. If we use a filter with polarization $\ket\uparrow$, the photon has a probability of $|a|^2$ to get
through and a likelihood of $|b|^2$ of getting absorbed. The values of $a$ and $b$ are called
the magnitude of the amplitude. In the case of $a = b = \frac{1}{\sqrt{2}}$ , the chances for the photon
to get through is equal to it not getting through, so $\frac{1}{2}$. <sup>7</sup>

## 2.1.2 POLARIZING EXPERIMENT

Using this experiment, we can illustrate what we have learned and demonstrate how
measurements work on a qubit.

### First experiment

Three experiments can be done using various filters to make the previous
statements easier to understand.
The first experiment uses one filter with polarization $\ket\rightarrow$. In this case, most photons
are aligned with the corresponding filter, so the result is intense light.





Figure 2 : Experiment with one filter^8

Those photons with a different polarization will be reoriented and lose some of their
magnitudes, as shown in Figure 3.

Figure 3 : Reorientation of photon^9

The light with opposite polarization will lose all its magnitude, as seen in Figure 4,
so it is absorbed completely.

Figure 4 : Absorbing photon^10


9 (Finley, 2004)^
10 (Finley, 2004)^
(Finley, 2004)



### Second experiment

In the second experiment, two filters are used, with polarization |↑⟩ and |→⟩.

Figure 5 : Experiment with two filters^11

Using the conclusions from the previous experiment, we know that all photons’
amplitudes are aligned with the filter |→⟩, but will then get absorbed entirely by the
filter |↑⟩, because the magnitude falls to zero.

Third experiment

In the last experiment, three filters are used:
One with |→⟩ polarization, then one with |↘⟩ polarization, and the last one with |↑⟩.

Figure 6 : Experiment with three filters^12

The exciting part of this experiment is that we would expect no light to get through,
like in the second experiment. But as we see, some light does not get absorbed.
The reason is that the photons are first aligned with the |→⟩ Filter. If we look at our
previous |𝑣⟩=𝑎|↑⟩+𝑏|→⟩ we have a = 0 and b = 1. When arriving at the second


12 (Finley, 2004)^
(Finley, 2004)



filter, a new measurement is done. The probability of the photons getting through is
shown with the following equation:

|𝑣⟩= (^) √^12 |↘⟩+√^12 |↗⟩ (^2 .1)^
Because we will have some photons that align with the new polarization while others
will not. In our case, this gives us a |√^12 |² = ½ probability of the photon getting through.
At the last filter, the same process repeats. The equation looks as follows:
|𝑣⟩=√^12 |↑⟩+√^12 |→⟩ (2.2)^
resulting in the following probability for the photons to get through: |√^12 |² = ½.^13
Takeaways
Measurement
The concept of measurement in quantum computing refers to transforming the qubit
to a specific state and turning that information into a classical form. This experiment
goes to show that qubits are probabilistic. Whenever a measurement is done, the
amplitudes of its polarization determine what state it takes on in the measurement.^14
Superposition
Furthermore, we saw that until the measurement is done, the qubit will be in a
superposition of various states and only sets on a specific value when a
measurement is done.


14 (Rieffel & Polak , 2011, S. 13)^
(Abhijith J., 2022, S. 5)



## 2.1.3 ENTANGLEMENT

So far, we have looked at singular qubits and how they work, but one of the main
reasons for quantum computing incredible power lies in the phenomenon of
entanglement.^15
This mechanism means that two qubits can be connected, so measuring one qubit
will also give the result of the second qubit. Entanglement can be done to many
qubits, creating one massive unit for computation.^16
Each qubit, which already can be in an infinite number of states, could influence the
other qubits it’s entangled with. N qubits can approximately compute 2 𝑛 Numbers.
This results in exponential growth for each added qubit.^17


## 2.1.4 GATES

While classical computers utilize processors, memory, and other tools to manipulate
bits in the required way, quantum computers use so-called gates. They are written
as matrices that will manipulate the qubit’s amplitudes which can then be measured.


## 2.1.5 SPEED-UP

Quantum speed-up is the concept that quantum algorithms can be written that utilize
a particular task in polynomial time, while the classical counterpart takes an
exponential amount of time. This concept refers to the time complexity of an
algorithm, which is noted using the O() notation. One such example is Shor’s
algorithm, used for factorization and will be further discussed later. The classical
algorithm has an exponential time complexity, while the quantum equivalent has
polynomial complexity, as shown in Figure 7.
Since the time complexity of classical algorithms and quantum algorithms cant be
easily compared, in this case, the runtime is used.


16 (Abhijith J., 2022, S. 4)^
17 (Abhijith J., 2022, S. 5)^
(Abhijith J., 2022, S. 4)


```
Figure 7 : Shor **’** s algorithm time complexity^18


```
(Aumasson, 2018, S. 260)



# CHAPTER 3. IMPACT OF QUANTUM COMPUTING ON CRYPTOGRAPHY


So far, the basics of quantum computing are covered, but it’s still not clear how
exactly it is a threat to cryptography. Since the field is constantly developing, the
best way to evaluate the threat to cryptography is to use recent literature.

The main threats come from the already mentioned Shor’s algorithm. Shor’s
algorithm can achieve exponential speed-up when finding the period of a
function. The detailed functionality of Shor’s algorithm to factorize a number N is
explained in Appendix 8. This can be used to solve classically hard problems,
like factoring, discrete logarithm, and elliptic curve discrete logarithm, essentially
breaking all forms of public-key cryptography, such as <abbr title="Rivest–Shamir–Adleman">RSA</abbr>, Diffie-Hellmann and
Elliptic Curve Cryptography.^19
No equivalent classical algorithm achieves exponential speed-up; the most
significant number factored was <abbr title="Rivest–Shamir–Adleman">RSA</abbr>- 250 , which is an 829 - bit number. This
factorization took approximately 2700 <abbr title="Core Processing Unit">CPU</abbr> years (using Intel Xeon Gold 6130
at 2.10<abbr title="Gigahertz">GHz</abbr> as reference) with the help of the Juwels Supercomputer.^20 Even
though this achievement is impressive, it’s far from the National Institute of
Standards and Technology (<abbr title="National Institute of Standards and Technology">NIST</abbr>) recommendation of 2048 bits.^21
Even for current quantum computers, the available number of qubits is simply
insufficient to factor large numbers. For example, their experiment (Amico,
Saleem, & Kumph, 2019) managed to factor the number 15 but failed at 35
because the currently available quantum computers are too error-prone.^22
Still, in only five years, the number of qubits has gone from approximately
2000 , achieved by D-Wave in 2017,^23 to 5000 in 2022, performed by the
Forschungszentrum Jülich^24.
As the development of new systems and technologies for quantum computers
continues, the time to mitigate the risk posed by future applications of, among
others, Shor’s algorithm is running out.
```
### 19

20 (Aumasson, 2018, S. 259)^
21 (Boudot, et al., 2020, S. 26)^
22 (Barker & Roginsky, 2019, S. 16)^
23 (Amico, Saleem, & Kumph, 2019, S. 7)^
24 (Gibney, 2017, S. 1)^
(Forschungszentrum Jülich, 2022)


```
CHAPTER: 3. IMPACT OF QUANTUM COMPUTING ON CRYPTOGRAPHY 12
Another such algorithm is Grover’s, which threatens symmetric encryption by
achieving quadratic quantum speed-up. This algorithm can find the correct
solution x from n number of options. Thereby satisfying a function 𝑓(𝑥)= 1 if the
solution is found, or 𝑓(𝑥)= 0 if the solution is incorrect. If we take this a step
further and say we have an unknown symmetric key K and known Plaintext and
Ciphertext P and C, we could set up a function where 𝑓(𝑥)= 1 only if x equals
K in the encryption function 𝑒(𝐾,𝑃)=𝐶. A classical equivalent would need
𝑂(𝑛/𝑚). For such a problem, Grover’s, through the quadratic speed-up, only
needs 𝑂(√𝑚𝑛). The good news, in this case, is that Grover’s algorithm essentially
halves the security of any symmetric algorithm. So using <abbr title="Advanced Encryption Standard">AES</abbr> today with 128-bit
security would mean it only has 64-bit security when applying Grover.^25
The conclusion is that symmetric ciphers can still be used. However, their size
has to be doubled Post Quantum to keep the “pre-quantum” security levels used
today.
Meanwhile, as soon as Shor’s algorithm can be applied to large numbers with a
small error rate to be reliable, most Asymmetric Algorithms will essentially be
worthless, such as <abbr title="Rivest–Shamir–Adleman">RSA</abbr> (factorization), Diffie-Hellmann (Discrete Logarithm),
and Elliptic Curve Cryptography (Elliptic Curve Discrete Logarithm).

(Aumasson, 2018, S. 259)



# CHAPTER: 4. MITIGATION TECHNOLOGIES

## 4. MITIGATION TECHNOLOGIES

When looking at mitigation techniques, two main methods can be found. The first is
Post-Quantum Cryptography, and the second is Quantum Cryptography.

While Post Quantum Cryptography looks into implementing new Classical
algorithms that quantum algorithms cannot break, quantum cryptography uses
quantum technology to create unbreakable protocols using the very laws of nature.

### 4.1 POST QUANTUM CRYPTOGRAPHY (PQC)

In 2017, <abbr title="National Institute of Standards and Technology">NIST</abbr> started a competition to collect and evaluate new cryptographic
algorithms to be used as the new standard for <abbr title="Post Quantum Cryptography">PQC</abbr>.
In 2022 <abbr title="National Institute of Standards and Technology">NIST</abbr> selected four algorithms as candidates for the fourth and last
evaluation phase.^26
Among these candidates, one is for Key exchange and Public Key Encryption, while
three are for digital signatures.
Among the chosen ones, two technologies are used:

### 4.1.1 LATTICE-BASED CRYPTO

Lattice-based cryptography is an emerging field that has shown promise as a
potential PQC algorithm.
It consists of a set of problems that are hard to solve for both classical and quantum
computers.
Lattices are grids of coordinates on an n-dimensional plane, as shown in Figure 8
with a 2-dimensional example, that pre-defined basis vectors have generated.



(NIST, 2022)


Figure 8 : Lattice example^27

Because basis vectors generate the points, they can all be reached by combinations
and scaling of the vectors.

Short Integer Solution Problem

Used by several of the <abbr title="National Institute of Standards and Technology">NIST</abbr> competition candidates use the Short Integer Solution
Problem (<abbr title="Shortest Integer Solution Problem">SIS</abbr>).
This problem to be solved in <abbr title="Shortest Integer Solution Problem">SIS</abbr> is to find s in the equation 𝑏=𝐴𝑠 𝑚𝑜𝑑 𝑞 for a given
Lattice A, a given number b, and a prime number q.^28

Figure 9 : Shortest Integer Solution^29

All possible solutions s can be plotted as a Lattice, shown in Figure 9. The issue is
to find the closest (inside the red circle) to but unequal to the origin (blue point).


28 (Alwen, 2018)^
29 (Aumasson, 2018, S. 264)^
(ISARA Corporation, 2022)


Given a 2-dimensional Lattice, s is easy to find. Still, with more dimensions, the
problem gets increasingly challenging because there is no efficient way to solve it
without secret information about the lattice that only the owner has.^30


### 4.1.2 HASH-BASED CRYPTO

This type of cryptography is used for signatures and doesn’t rely on a new kind of
algorithm but uses the “standard” way of computing hashes, such as SHA256 or
Shake256, used by the <abbr title="National Institute of Standards and Technology">NIST</abbr> candidate SPHINCS+.

The primary attack vector on hashes is to find two different strings that result in the
same hash. This is called a collision and would result in the possibility of an attacker
impersonating the actual hash owner. However, in his paper (Bernstein, 2009, S.
10), D.J Bernstein found that the cost of an attack by a quantum computer would
not be any more efficient than those performed by modern classical computers as
their algorithms of attacking such hashes are more efficient.

These forms of hashes are based on the Winternitz one-time signature (<abbr title="Winternitz one-time signature">WOTS</abbr>) and
a high-level work as follows:
A parameter w is given, and the message to be signed is represented by a number
M between 0 and w-1. The private key is created by generating 32 random 256 - bit
numbers.
To create the signature, the hash is repeated M times on K, so
Hash(Hash(...(Hash(K))) = 𝐻𝑎𝑠ℎ𝑀(𝐾). The public key is 𝐻𝑎𝑠ℎw(𝐾). When we want
to verify the signature S, we compute 𝐻𝑎𝑠ℎ𝑤−𝑀(𝑆), which will turn the signature into
the public key 𝐻𝑎𝑠ℎw(𝐾), because we now hashed the signature M + (w – M) = w
times, which is the same as the public key.

Other options available show potential as <abbr title="Post Quantum Cryptography">PQC</abbr> algorithms, such as multivariate and
code-based algorithms, but the ones covered so far are implemented in the <abbr title="National Institute of Standards and Technology">NIST</abbr>
candidates for <abbr title="Post Quantum Cryptography">PQC</abbr> standardization and, therefore, the main options.



(Nejatollahi, et al., 2019, S. 4)




## 4.2 QUANTUM CRYPTOGRAPHY

While the discussed <abbr title="Post Quantum Cryptography">PQC</abbr> algorithms rely on classical algorithms that cannot be
broken by quantum computing, Quantum cryptography uses the laws and
mechanisms of quantum computing to create inherently unbreakable algorithms.
Inherently unbreakable means that some of the quantum cryptography algorithms
have been devised that cannot be broken due to the very laws of nature, as we will
see.^31

### 4.2.1 QUANTUM KEY DISTRIBUTION (QKD)

The idea was initially based on the surreal concept of quantum money, where
photons would be trapped in each money bill with a polarization that only the issuing
bank knows, thereby making forgery impossible. Charles H. Bennet and Gilles
Brassard developed the first ideas of a quantum algorithm for key exchange
(Bennett & Brassard, 1984).

The basic idea is that Alice and Bob need to find a private key while using a public
channel with potential eavesdroppers.
For this purpose, Alice generates a random string of bits.
Alice conveyed these bits in one of two ways. First, photons that have a horizontal
orientation (↕) and those with a 135 ° (⤡) orientation represent ones, while vertical
(↔) and 45° (⤢) orientation represents 0.
When Alice sends the bits using the, she notes down which orientation she uses for
the transmission, then sends them to Bob. When the photons arrive at Bob, he
randomly chooses a direction for measurement, which 50 % of the time will result in
a correct choice, and the other 50 % will not be able to make a measurement. So
far, the pair has been using a quantum channel to transmit the photons (fiber cable),
but the rest of the communication will use a public channel. Bob notes the resulting
bits and reports to Alice which orientation he used for measuring. She replies
whether the chosen directions are equal to the ones she used. Before finalizing the
key, they will select random bits and compare these to check whether an

### 31

```
(Singh, 2000, S. 332)
```

```
CHAPTER: 4. MITIGATION TECHNOLOGIES 17
```
eavesdropper might have caused errors. The resulting bits are the final private key
that can encrypt any following communication.^32
Figure 10 shows the graphical representation of the entire process

Figure 10 : Quantum Key Distribution (<abbr title="QKD Algorithm (Bennet and Brassard 1984)">BB84</abbr>)^33

The pair now has a key, but how can they be so sure that no eavesdropper could
have altered or listened to the transmission?
This is where the quantum phenomena play a crucial role: Because any
measurement will result in a maximum of 50% correct choices, if an eavesdropper
would listen to the transmission and retransmit the measured photons, Bob would
end up with no less than 25% of accurate measurements. When Alice and Bob now
compare a random choice of bits, they would identify a significant deviation in errors
and restart the transmission.^34 Zurek and Wootters (Zurek & Wootters, 1982)
formulated and proved that a qubit, whose state is unknown, cannot be cloned.
Therefore, the eavesdropper cannot recreate the transmission, so Bob and Alice will
know something was changed when comparing the results.


33 (Bennett & Brassard, 1984, S. 4)^
34 (Bennett & Brassard, 1984, S. 5)^
(Singh, 2000, S. 346)



## 4.3 PQC VS. QKD


Although <abbr title="Quantum Key Distribution">QKD</abbr> seems like a promising technology that can stay secure potentially
forever, it has some serious drawbacks when it comes to implementation.
The National Security Agency (<abbr title="NatioNal Security Agency">NSA</abbr>) compiled a list of some of the most immediate
issues about implementing <abbr title="Quantum Key Distribution">QKD</abbr> (NSA, 2021):
The first and most apparent issue is that <abbr title="Quantum Key Distribution">QKD</abbr> cannot implement authentication,
which means that one cannot be sure who is actually at the other end of the channel.
Wang et al. (Wang, et al., 2021) show that <abbr title="Quantum Key Distribution">QKD</abbr> can be implemented with
asymmetric cryptography (<abbr title="Post Quantum Cryptography">PQC</abbr>) such as lattice based-cryptography. But then why
use <abbr title="Quantum Key Distribution">QKD</abbr> when <abbr title="Post Quantum Cryptography">PQC</abbr>, which is cheaper, has to be part of the solution anyway.
Second, various issues are evolving from the hardware limitations of special
equipment for Quantum Key Distribution.
For one, the equipment cannot be easily integrated into existing networks, or the
cloud, as an end-to-end fiber connection is required with special hardware that can
detect the polarization of photons sent. Diamanti et al. (Diamanti, Lo, Qi, & Yuan,
2016) show that many limitations can be overcome using specific topologies or
protocols. However, overall implementation would still require an incredible
investment and continuous improvement that is unreasonable compared to the cost
of <abbr title="Post Quantum Cryptography">PQC</abbr>. The cost of using QKD compared to <abbr title="Post Quantum Cryptography">PQC</abbr> is higher by orders of magnitude.
Even though <abbr title="Quantum Key Distribution">QKD</abbr> can theoretically deliver security protected by the laws of nature,
this statement fails at the engineering challenge, as the equipment needed has a
much smaller fault tolerance than classic cryptography and is much harder to
implement in hardware. Makarov and Hjelme (Makarov & Hjelme, 2005) proved that
hardware attacks exist, allowing a potential eavesdropper to create a key of their
choosing by abusing vulnerabilities in hardware.
On the other hand, <abbr title="Post Quantum Cryptography">PQC</abbr> is under development, and several candidates have passed
the scrutiny of the <abbr title="National Institute of Standards and Technology">NIST</abbr> competition for <abbr title="Post Quantum Cryptography">PQC</abbr> algorithms^35. Even though
vulnerabilities might be identified in the future, these algorithms can be easily
implemented into existing infrastructure and are much better understood than <abbr title="Quantum Key Distribution">QKD</abbr>
so that vulnerabilities can be patched easier. Given the time pressure to implement
a solution, the only solution seems to be to use <abbr title="Post Quantum Cryptography">PQC</abbr> until <abbr title="Quantum Key Distribution">QKD</abbr> has been further
developed. Perhaps some of its drawbacks are fixed by developing new solutions.

35
(NIST, 2022)



Eventually, <abbr title="Quantum Key Distribution">QKD</abbr> might be used in practice, but this requires much more research
and is no immediate solution for companies looking to prevent ‘harvest now, decrypt
later’ attacks as soon as possible.



# CHAPTER 5. WHAT COMPANIES NEED TO DO

In the previous chapters, we have looked at how quantum computers work and the
algorithms used to attack classic asymmetric cryptography. Next, we have looked
at post-quantum cryptography, quantum cryptography, and solutions to this threat.
Now remains to identify how mainly <abbr title="Post Quantum Cryptography">PQC</abbr> can be implemented and what companies
need to do to get started to become resilient against this threat.

## 5.1 IMPLEMENTING QUANTUM CRYPTOGRAPHY

As concluded earlier, Quantum Cryptography needs much more time to mature into
a technology that non-research companies can use at a reasonable cost. However,
there are certain things companies should do regularly. One of these things is to
monitor the developments on a high level. Currently, most users of quantum
cryptography are research projects and universities involved in such, but eventually,
offers will become available for companies to purchase quantum cryptography
solutions. Therefore, it’s in the company’s interest to identify relevant solutions.
Secondly, as quantum cryptography might require the purchase of specialized
hardware, companies should calculate whether the investment is beneficial
compared to just using <abbr title="Post Quantum Cryptography">PQC</abbr> and, if so, make the necessary infrastructure changes.

Because there isn’t much more a company can do regarding <abbr title="Quantum Cryptography">QC</abbr> today, the rest of
this chapter will focus on the requirements for preparing for <abbr title="Post Quantum Cryptography">PQC</abbr>.


## 5.2 IMPLEMENTING PQC

Compared to <abbr title="Quantum Cryptography">QC</abbr>, companies can and should start to prepare for implementing
PQC. Because these are classical algorithms implemented using libraries, they can
be used as soon as a company sees fit. But for most organizations changing
cryptography is not as easy as plugging in a new library.
Because Cryptography plays such a crucial role in everything related to <abbr title="Information Technology">IT</abbr> and
security, the margin for error is minimal, and even the slightest issue could result in
devastating vulnerabilities.
That is why agile cryptography has become much more prominent in recent years,
even though it has been around for much longer.


### 5.2.1 CRYPTOGRAPHIC AGILITY

Cryptographic agility is the concept that an infrastructure’s cryptography is set up
so that it can be adapted or replaced whenever necessary. It goes both for
software as well as hardware.^36

Cryptograph is constantly evolving, and several issues drive the need for
cryptographic agility: The constant development of cryptanalysis and attacks
against ciphers and cryptographic methods, “harvest now, attack later” techniques
on data or devices that have a long lifetime and, of course, as mentioned before,
the development of quantum computing.^37

In the past, crypto agility has already been an important concept. For example, in
2006 , when a new attack against the <abbr title="Secure Hash Algorithm">SHA</abbr>-1 cipher was found, enabling a collision
attack within a realistic amount of operations. Furthermore, Leurent and Peyrin
(Leurent & Peyrin, 2020) identified a collision attack that would allow a
<abbr title="Pretty Good Privacy">PGP</abbr>/<abbr title="GNU Privacy Guard">GnuPG</abbr> user to impersonate another because the old <abbr title="Secure Hash Algorithm">SHA</abbr>- 1 cipher was still
enabled in these libraries until recently.^38 Of course, this vulnerability led to
immediate action recommendations to migrate to <abbr title="Secure Hash Algorithm">SHA</abbr>- 239. However, as Leurent
and Peyrin found (Leurent & Peyrin, 2020), many applications still use <abbr title="Secure Hash Algorithm">SHA</abbr>- 1 ,
some 15 years after it was deemed vulnerable.

These are just some examples where crypto agility would have made an incredible
difference for organizations to adapt to the vulnerability and migrate from <abbr title="Secure Hash Algorithm">SHA</abbr>-1 to
<abbr title="Secure Hash Algorithm">SHA</abbr>-2 without making it a massive feat.

But cryptographic agility is not simple. Moreover, apart from implementing such
agility in extensive, fast-changing infrastructure, the agility itself can be dangerous
if not handled correctly.

### 36

37 (Wiesmaier, et al., 2021, S. 1)^
38 (Mehrez & Omri, 2018, S. 1)^
39 (Leurent & Peyrin, 2020, S. 4)^
(Wang, Yin, & Yu, 2005)


```
CHAPTER: 5. WHAT COMPANIES NEED TO DO 22
```
In 2014 Secure Socket Layer (<abbr title="Secure Socket Layer">SSL</abbr>) 3.0 was found vulnerable to an attack that
abused its configuration of ciphers, specifically its padding. Therefore <abbr title="Secure Socket Layer">SSL</abbr> 3.0 can
be manipulated for man-in-the-middle attacks.^40 At the time, this led to its
deprecation and the recommendation of using only Transport Layer Security (<abbr title="Transport Layer Securit">TLS</abbr>)
1.0 and above.^41

The issue is that for legacy systems that required <abbr title="Secure Socket Layer">SSL</abbr> 3.0, many browsers and
clients still offered <abbr title="Secure Socket Layer">SSL</abbr> 3.0. Using <abbr title="Padding Oracle On Downgraded Legacy Encryption">POODLE</abbr>, attackers would force a downgrade to
<abbr title="Secure Socket Layer">SSL</abbr> 3.0 and then abuse the vulnerability. This attack can be mitigated by
completely removing <abbr title="Secure Socket Layer">SSL</abbr> 3.0 or preventing Protocol fallback attacks using a
Signaling Cipher Suite Value (<abbr title="Signaling Cipher Suite Value">SCSV</abbr>), which is available as an extension to <abbr title="Transport Layer Securit">TLS</abbr>.^42

<abbr title="Secure Socket Layer">SSL</abbr> Pulse is a project that scans the top 150.000 websites and, as of September
4 th, 2022 , still found that 5% did not enable the <abbr title="Signaling Cipher Suite Value">SCSV</abbr> extension and were
vulnerable to a <abbr title="Padding Oracle On Downgraded Legacy Encryption">POODLE</abbr> attack.^43

This attack is not directly related to crypto agility but demonstrates the difficulty of
securely configuring websites. But if Crypto agility is used, such misconfigurations
remain a vulnerability that requires exact configuration of any changes.

Returning to the need for crypto agility, a look at the security levels of currently
used algorithms after Quantum computers are matured, as seen in Table 1, should
suffice. These algorithms are now the standard and are used in almost all
systems. Migrating from these will be difficult. Additionally, it’s hard to predict what
algorithms will become the new standard and whether they will change due to
future vulnerabilities.

### 40

41 (Möller, Duong, & Kotowicz, 2014, S. 2)^
42 (Barnes, Thomson, Pironti, & Langley, 2015)^
43 (Moeller & Langley, 2015, S. 3)^
(Qualys Inc., 2022)


```
CHAPTER: 5. WHAT COMPANIES NEED TO DO 23
```
Table 1 : Security Level now vs. PQC^44

Cryptographic inventory

For an organization to implement crypto agility, it first needs to identify all the
cryptography used within the company. This includes its developed software and
what cryptography is used by software and hardware procured from vendors. This
identification is a cryptography inventory and should be kept up to date whenever
changes are made.^45
Many automated tools can help achieve this inventory, from cryptographic discovery
tools such as cryptosense analyzer to endpoint managers like Tanium. However, in
the end, most of the discovery has to be manual because tools will always overlook
some details or miss a proprietary algorithm it doesn’t recognize.
After this painstaking task is done, the implementation of the actual agility can begin.

### 44

45 (Mehrez & Omri, 2018, S. 2)^
(ISACA, 2018, S. 12)


```
CHAPTER: 5. WHAT COMPANIES NEED TO DO 24
```
Cryptographic Agility

All procured software must undergo an assessment. Vendor’s cryptography
handling must become an important topic, and requirements should be set in place.
Only vendors that either use agile cryptography themselves or at least report their
use of cryptography and any changes to it in the future should be allowed so the
organization has information to act on if deemed necessary.^46

For in-house developed applications, it’s a little easier. In general, the organization
needs to abstract the cryptography from applications and create a one-stop shop
<abbr title="Application Programming Interface">API</abbr> for all cryptographic operations. Figure 11 shows what such a solution might
look like. While currently, every App Version holds cryptographic libraries that need
to be updated with any significant change, using such an <abbr title="Application Programming Interface">API</abbr> would eliminate the
need to worry about it. 47

Figure 11 : Cryptographic Agility^48

### 46

47 (Computing Community Consortium, 2019, S. 22)^
48 (Wiesmaier, et al., 2021, S. 12)^
(Macaulay & Henderson, 2021, S. 8)


```
CHAPTER: 5. WHAT COMPANIES NEED TO DO 25
```
But how could such an <abbr title="Application Programming Interface">API</abbr> be developed?
Thankfully, several frameworks already implement crypto agility and can easily be
used to create such an <abbr title="Application Programming Interface">API</abbr>.
The main goal is to prevent hardcoding algorithm implementations, which would
make changing algorithms difficult later. To prevent this, both .NET and Java
Development Kit (<abbr title="Java Development Kit">JDK</abbr>) implement abstractions. Within these abstractions, a factory
method allows developers to create their implementations by supplying the name
from a configuration file or database.^49
Now, whenever a change is needed, or a new algorithm should be used, the
configuration file needs to be updated, the old algorithm removed, and the new one
added.
Some of the essential features crypto agility provides are:^50

1. Extensibility allows us to extend or reduce the current range of algorithms by
    any new ones.
2. Backward Compatibility has to be supported not to shut down all legacy
    applications and needs to be implemented carefully not to introduce
    vulnerabilities
3. Interoperability is needed to ensure that different platforms and applications
    can communicate with various services and devices that might require
    different algorithms. Essentially requiring more than one algorithm for each
    use-case but must be careful not to allow vulnerable algorithms that some
    platform uses.
4. Updateability allows updating the algorithms themselves. If a flaw is found in
    the implementation of, e.g., SHA256, a patch must be able to fix any
    vulnerabilities.
5. ...

Many more features should be considered, but these present some of the most
important ones.

Another option to achieve cryptographic agility, other than to develop own solutions,
is to use cryptography as a service (CaaS). Usually, this is a provider of a

### 49

50 (Sullivan , 2010, S. 3)^
(Mehrez & Omri, 2018, S. 4)


```
CHAPTER: 5. WHAT COMPANIES NEED TO DO 26
```
cryptographic <abbr title="Application Programming Interface">API</abbr>, which has several advantages to developing an in-house
solution. One reason would be that the organization may not have the expertise to
build a cryptographic <abbr title="Application Programming Interface">API</abbr> in-house. Additionally, it enables rapid scaling without the
often expensive investments in the hardware infrastructure needed to host such a
solution. Also, the maintenance and patching are left to the vendor, and often such
solutions are cost-effective and can easily be used for cloud and on-prem
applications.^51

In addition to latency and transfer security, issues arise because the maintenance
is in the hands of the vendor. Therefore, before deciding on a CaaS, the organization
should ensure that the vendor intends to support <abbr title="Post Quantum Cryptography">PQC</abbr> algorithms and that the
platform enables all the features needed to be agile.^52

One last important crypto agility topic is incorporating it into an incident response
plan. Organizations should prepare for the worst-case scenario in which an
internally approved algorithm turns vulnerable and create a plan to migrate the
algorithm quickly. This can be applied to encryption algorithms, certificates, keys,
and other use cases.

Cryptographic agility goes a long way to make migrating to <abbr title="Post Quantum Cryptography">PQC</abbr> easier, but more
steps can and should be taken.

It should be thoroughly assessed to prepare the infrastructure for the migration
event. Several Frameworks designed for this exact situation will be presented in
the following.

### 51

52 (Rahimi, Reed, & Gupta, 2018, S. 2)^
(Rahimi, Reed, & Gupta, 2018, S. 12)



## 5.2.2 NIST CYBERSECURITY FRAMEWORK (CSF)

The <abbr title="Cybersecurity Framework">CSF</abbr> is a Framework developed to “[...] improve critical infrastructure
cybersecurity.” (NIST, 2018)
The framework is not aimed at Cryptographic activities specifically but to manage
cybersecurity risks to the entire <abbr title="Information Technology">IT</abbr> of an organization. This automatically includes
risks through Quantum computers and is an essential framework because it is one
of the most widely used. (Dimensional Research, 2016)
The framework is split into Core, Tier, and Profile.

The Core consists of 5 steps that present the lifecycle of cybersecurity risk
management and are the following:

1. Identify: Understand the risk to the companies assets and infrastructure
2. Protect: Find protection mechanisms for the identified risks
3. Detect: Set up detection mechanisms to be alerted of any events
4. Respond: Develop activities on how to respond to an event
5. Recover: How a company can restore functionality after an event

The Tiers system allows organizations to identify the wanted integration of
cybersecurity practices into operations and ranges from Tier 1 to Tier 4.
Tier 1 defines a company with little to no formal processes regarding cybersecurity
and takes on risks and challenges ad hoc. At the same time, Tier 4 is a company
that constantly evolves cybersecurity practices, learns from past events, and plays
a proactive role in preventing and mitigating cybersecurity incidents.
These Tiers help companies identify their current position and actions needed to
achieve the ideal balance of security and operations.^53

Lastly, a Profile is chosen to align requirements, risk tolerance, and organizational
strategy to identify desired outcomes and activities to find potential gaps and needs
for improvement.

### 53

```
(NIST, 2018)
```


## 5.2.3 QUANTUM RISK ASSESSMENT (QRA)

While the <abbr title="Cybersecurity Framework">CSF</abbr> framework is rather general for overall risk assessment, the <abbr title="Quantum Risk Assessment">QRA</abbr>
identifies the risks specifically related to quantum computing and should be used as
an additional tool to other frameworks.
The framework is divided into 6 phases that, to some degree, overlap with the
previous <abbr title="Cybersecurity Framework">CSF</abbr> framework but are more related to Cryptography^54 :

Phase 1: Identify and document

As with <abbr title="Cybersecurity Framework">CSF</abbr>, the first phase is identifying relevant assets and documenting how they
are currently protected by cryptography. This is especially important for valuable
and sensitive assets and information. That automatically implies that the
organization has evaluated the business value of various data and assets. It’s also
essential to note the legal requirements to protect the identified assets.

Phase 2: Research the state of quantum computers and <abbr title="Post Quantum Cryptography">PQC</abbr>

Understanding where the development of <abbr title="Post Quantum Cryptography">PQC</abbr> and Quantum computers stands is
vital so the organization can put it in context and draw conclusions for its reactions.
This means continuously monitoring the most recent events in the fields and having
a dedicated team or contacting other organizations that can understand what is
happening in the areas and their impact on organizations.

Phase 3: Identify threat actors and estimate time to quantum computers access (“z”)

So the organization can create a plan of action, it needs to know what the timeline
looks like until its threat actors have access to quantum computers that could be
used to break the currently used cryptography. The time “z” is a number that will
later be needed to estimate the risk.
Identifying the timeline is no easy task, as it requires a thorough understanding of
the developments, but many resources offer such estimates, which can be
aggregated and used. These frequently change with new developments and thus
should be monitored continuously.

54
(Mosca & Mulholland, 2017)


```
CHAPTER: 5. WHAT COMPANIES NEED TO DO 29
```
Phase 4: Identify the **lifetime of assets “x”** and time to transform organization
**“y”**

If a company owns data set to be deleted in a few years, there is no reason to worry
about quantum computers, as the data will be long gone by the time they come
around. However, reality usually looks very different. Most organizations keep
records, such as financial records, medical data, and others, are saved for decades,
often lifetimes, and thus can already be harvested by malicious actors and
decrypted as soon as the quantum technology can do so. So in these cases, x is a
large number of perhaps several decades.
Secondly, the organization will still be vulnerable if the transformation to <abbr title="Post Quantum Cryptography">PQC</abbr> occurs
during this time. Therefore the time of transformation is estimated with the variable
“y.” Small companies with an agile infrastructure will have a much shorter
transformation time than large companies with massive, old infrastructure.

Phase 5: x + y > z? Determine quantum risk

Now it’s time to see how dire the organization’s situation is. By comparing the sum
of x and y with the value of z, a comparison is made between the time it takes for
quantum technology to mature (“z”) and how long the transformation of the company
will take, combined with the lifetime of its data. This essentially results in whether
the organization’s assets will become vulnerable before it can start protecting them.
An example of estimations can be seen in Table 2.

Table 2 : X + Y > Z


```
CHAPTER: 5. WHAT COMPANIES NEED TO DO 30
```
Phase 6: Prioritize activities

The last thing a company has to do after the assessment is made is a migration plan
on achieving the required security and what actions need to be taken. So again, a
lot can be done. Still, because the field is changing, the organization needs to
continuously monitor what activities make sense and prioritize the ones that will lead
to the highest degree of security.

### 5.2.4 CRYPTO AGILITY RISK ASSESSMENT FRAMEWORK (CARAF)

A group of researchers (Ma, Colon, Dera, Rashidi, & Garg, 2021) argued that both
the <abbr title="Cybersecurity Framework">CSF</abbr> and <abbr title="Quantum Risk Assessment">QRA</abbr> are not suitable for assessing the risk posed by <abbr title="Post Quantum Cryptography">PQC</abbr> because
<abbr title="Cybersecurity Framework">CSF</abbr> is focused on current and not so much on future threats. Meanwhile, <abbr title="Quantum Risk Assessment">QRA</abbr> is
too focused on quantum computing and not so much on crypto agility. So instead,
they combined both frameworks and made tweaks, which resulted in a framework
that enables companies to make better decisions regarding crypto agility.
The framework uses five steps:

Step 1: Identify threats

While other frameworks start with asset identification, this framework begins by
identifying the relevant threats so the organization can later limit the asset pertinent
to those impacted by a threat—for example, the threat by quantum computers to
symmetric algorithms is much smaller than to asymmetric cryptography. And so,
factors like regulations, technology, and vulnerabilities all change the threat level.

Step 2: Asset inventory
In this step, the organizations must identify all assets impacted by a threat from step
one and that use cryptography. The inventory must include secrets management
and list the cryptography used. Priorizatinos should be made by sensitivity, location
(cloud vs. on-prem), and other factors. This creates both an inventory and a
prioritization of those assets.


```
CHAPTER: 5. WHAT COMPANIES NEED TO DO 31
```
Step 3: risk estimation
For risk estimation, several models should be used. For one, the X + Y > Z model
used by Moscas’ <abbr title="Quantum Risk Assessment">QRA</abbr> framework can be a helpful tool to estimate risk by timeline.
Another technique is calculating the cost of mitigation, so a less agile asset will incur
a higher cost to becoming agile and thus be more vulnerable.

Step 4: secure assets through risk mitigation
Here organizations must decide whether the impacted asset should be secured by
incurring the cost to upgrade it, accept the risk and leave the asset as-is, or
terminate it, thereby eliminating the risk. This decision is based on the asset
prioritization from step 2 and the risk estimation from step 3.

Step 5: organizational roadmap
Lastly, the organization needs to implement a plan to address these threats for
various functions. Processes must be set in place to guide teams in managing their
assets and address issues like vendor assessment, incident response, new security
architecture, product development for future developments, and plans for change
management.

With these tools in hand, an organization should be well-equipped to address the
upcoming challenge of quantum computers and position itself to be secure.


```
CHAPTER: 6. CONCLUSION 32
```
# CHAPTER 6. CONCLUSION

This report has covered the basics of quantum computing to understand the main
principles that make it a threat, such as entanglement, speed-up, and more. It then
used this knowledge to explain what algorithms have an impact and how exactly
cryptography is threatened and might be broken. Finally, concluding that even
though quantum computing still needs many years to fully mature, it will eventually
become the biggest threat to cryptography. Thus, organizations must prepare as
soon as possible, especially if they hold sensitive data stored for a long time.

The two possible prevention methods, <abbr title="Quantum Cryptography">QC</abbr> and PKQ, were discussed to
understand their characteristics. <abbr title="Quantum Cryptography">QC</abbr> leads to cryptography that will be unbreakable
because of the laws of nature. However, as all of the literature supported, this will
take much longer than it will take for quantum computers to mature, simply
because the engineering challenge is too complex and too little is known about the
technology for it to be implemented anytime soon. Additionally, <abbr title="Quantum Cryptography">QC</abbr> requires
significant changes to the current infrastructure organizations use.

Therefore the only safe way for organizations to prepare for when cryptography
beaks is to start preparing their infrastructure for <abbr title="Post Quantum Cryptography">PQC</abbr>. Even though these
techniques are relatively new, they are much better understood than <abbr title="Quantum Cryptography">QC</abbr> and can
be implemented into the current infrastructure. Therefore, organizations need to
make their infrastructure agile enough to implement these new algorithms when
necessary, and for that should create a crypto inventory. In addition, several risk
frameworks were presented, one of which concentrates on how to prepare for the
required cryptographic agility.

With these steps, organizations should be able to develop a resistant infrastructure
in time.

In the Goal definition, the following questions were set to be answered:

- What is <abbr title="Quantum Cryptography">QC</abbr>, and how does it fundamentally work?
- Where does the threat come from, and what solutions are developing?
- What actions mitigate the danger posed by <abbr title="Quantum Cryptography">QC</abbr>?


```
CHAPTER: 6. CONCLUSION 33
```
The thesis has shown how quantum computing works and impacts cryptography,
how algorithms are threatened, and what mitigation methods and concrete steps
organizations must take to be safe.

Thereby all questions have been answered.


```
CHAPTER: 7. REFERENCES VII
```
# CHAPTER 7. REFERENCES

<sup>3, 5, 14, 15, 16, 17</sup>
Abhijith J., A. A. (2022). Quantum Algorithm Implementations for Beginners. ACM
Transactions on Quantum Computing, 3 (4), 1 - 92.
doi:https://doi.org/10.1145/3517340<br>

<sup>27</sup>
Alwen, J. (2018). medium. Retrieved 10 19, 2022, from What is Lattice-Based
Cryptography & Why You Should Care: https://medium.com/cryptoblog/what-
is-lattice-based-cryptography-why-should-you-care-dbf9957ab717<br>

<sup>22</sup>
Amico, M., Saleem, Z. H., & Kumph, M. (2019). Experimental study of Shor's
factoring algorithm using the IBM Q Experience. American Physical Society,
100 (1). doi:10.1103/PhysRevA.100.012305
<br>

<sup>2</sup>
Arute, F., Arya, K., Babbush, R., Bacon, D., Bardin, J. C., & Barends, R. (2019).
Quantum supremacy using a programmable superconducting processor.
Nature, 574(7779), 505-510. doi:10.1038/s41586- 019 - 1666 - 5
<br>

<sup>4, 18, 19, 25, 28</sup>
Aumasson, J.-P. (2018). Serious Cryptography: A Practical Introduction to Modern
Encryption (5th ed.). No Starch Press. Retrieved 10 12, 2022
<br>

<sup>21</sup>
Barker, E., & Roginsky, A. (2019). Transitioning the Use of Cryptographic Algorithms
and Key Lengths. National Institute of Standards and Technology.
doi:doi.org/10.6028/NIST.SP.800-131Ar2
<br>

<sup>41</sup>
Barnes, R., Thomson, M., Pironti, A., & Langley, A. (2015). Deprecating Secure
Sockets Layer Version 3.0. Internet Engineering Task Force. Retrieved 11
09, 2022, from https://datatracker.ietf.org/doc/html/rfc7568
<br>

<sup>32, 33</sup>
Bennett, C. H., & Brassard, G. (1984). QUANTUM CRYPTOGRAPHY: PUBLIC KEY
DISTRIBUTION AND COIN TOSSING. International Conference on
Computers, Systems & Signal Processing. 1 , pp. 175-179. Bangalore: IEEE.
Retrieved 10 25, 202 2, from
https://arxiv.org/ftp/arxiv/papers/2003/2003.06557.pdf
<br>

Bernstein, D. J. (2009). Cost analysis of hash collisions: Will quantum computers
make SHARCS obsolete? SHARCS'09 Workshop Record, 4 , pp. 105-116.
Lausanne. Retrieved 10 20, 2022, from [http://cr.yp.to/hash/collisioncost-](http://cr.yp.to/hash/collisioncost-)
20090823.pdf
<br>

<sup>20</sup>
Boudot, F., Gaudry, P., Guillevic, A., Heninger, N., Thomé, E., & Zimmermann, P.
(2020). Comparing the difficulty of factorization and Discrete Logarithm: A
240 - Digit Experiment. Advances in Cryptology _–_ CRYPTO 2020. 12171 , pp.
62 - 91. Santa Barbara: Springer. doi:https://doi.org/10.1007/978- 3 - 030 -
56880 - 1_3
<br>

<sup>60</sup>
Chuang, I. L., & Nielsen, M. A. (2010). Quantum Computation and Quantum
Information (10th ed.). Cambridge University Press. Retrieved 11 15, 2022,
from [http://mmrc.amss.cas.cn/tlb/201702/W020170224608149940643.pdf](http://mmrc.amss.cas.cn/tlb/201702/W020170224608149940643.pdf)
<br>

<sup>46</sup>
Computing Community Consortium. (2019). Identifying Research Challenges in
Post Quantum Cryptography Migration and Cryptographic Agility. Computing
Community Consortium. Retrieved 11 11, 2022, from
https://arxiv.org/ftp/arxiv/papers/1909/1909.07353.pdf
<br>


Diamanti, E., Lo, H.-K., Qi, B., & Yuan, Z. (2016). Practical challenges in quantum
key distribution. npj Quantum Information, 2 (16025).
doi:10.1038/npjqi.2016.25
<br>

Dimensional Research. (2016). TRENDS IN SECURITY FRAMEWORK
ADOPTION. Retrieved 11 03, 2022, from
https://static.tenable.com/marketing/tenable-csf-report.pdf
<br>

<sup>61</sup>
Feynman, R. P. (1982). Simulating physics with computers. International Journal of
Theoretical Physics, 21, 467–488. doi:doi.org/10.1007/BF02650179
<br>

<sup>8, 9, 10, 11, 12</sup>
Finley, D. R. (2004). Polarizer. Retrieved 10 13, 2022, from Third-Polarizing-Filter
Experiment Demystified: [http://alienryderflex.com/polarizer/](http://alienryderflex.com/polarizer/)
<br>

<sup>24</sup>
Forschungszentrum Jülich. (2022, January). Press Release. Retrieved 10 16, 2022,
from Europe’s First Quantum Computer with More Than 5,000 Qubits
Launched at Jülich: https://www.fz-juelich.de/en/news/archive/press-
release/2022/2022- 01 - 17 - juniq-europes-first-quantum-computer-with- 5000 -
qubits
<br>

<sup>23</sup>
Gibney, E. (2017). D-Wave upgrade: How scientists are using the world’s most
controversial quantum computer. Nature, 541 , 447 - 448.
doi:doi.org/10.1038/541447b
<br>

<sup>45</sup>
ISACA. (2018). Assessing Cryptographic Systems. ISACA. Retrieved 11 11, 2022,
from https://www.isaca.org/why-isaca/about-us/newsroom/press-
releases/2017/isaca-instructs-basic-cryptographic-adoption-in-four-phases
<br>

<sup>29</sup>
ISARA Corporation. (2022). blog-posts. Retrieved 10 19, 2022, from lattice-based-
cryptography: https://www.isara.com/blog-posts/lattice-based-
cryptography.html
<br>

<sup>38</sup>
Leurent, G., & Peyrin, T. (2020). SHA-1 is a Shambles: First Chosen-Prefix Collision
on SHA-1 and Application to the PGP Web of Trust. 29. USENIX Security
Symposium. Retrieved 11 09, 2022, from
https://www.usenix.org/system/files/sec20-leurent.pdf
<br>

Ma, C., Colon, L., Dera, J., Rashidi, B., & Garg, V. (2021). <abbr title="Crypto Agility Assessment Framework">CARAF</abbr>: Crypto Agility
Risk Assessment Framework. Journal of Cybersecurity, 1-11.
doi:10.1093/cybsec/tyab013
<br>

<sup>48</sup>
Macaulay, T., & Henderson, R. (2021). Cryptographic Agility in Practice: Emerging
Use-Cases. Retrieved 11 11, 2022, from https://uploads-
ssl.webflow.com/612fec6a451c71c9308f4b69/614b712e53ce8f7fad0c3c4a
_ISG_AgilityUseCases_Whitepaper-FINAL.pdf
<br>

Makarov, V., & Hjelme, D. R. (2005). Faked states attack on quantum
cryptosystems. Journal of Modern Optics, 52 (5), 691 - 705.
doi:10.1080/09500340410001730986
<br>

<sup>37, 44, 50</sup>
Mehrez, H. A., & Omri, O. E. (2018). The Crypto-Agility Properties. Proceedings of
The 12th International Multi-Conference on Society, Cybernetics and
Informatics (IMSCI 2018). 1 , pp. 99 - 104. International Institute of Informatics
and Systemics. Retrieved 11 09, 2022, from
https://www.iiis.org/cds2018/cd2018summer/papers/ha536vg.pdf
<br>

<sup>21, 42</sup>
Moeller, B., & Langley, A. (2015). TLS Fallback Signaling Cipher Suite Value
(SCSV) for Preventing Protocol Downgrade Attacks. Internet Engineering
Task Force. Retrieved 11 09, 2022, from https://www.rfc-
editor.org/rfc/rfc7507
<br>

<sup>40</sup>
Möller, B., Duong, T., & Kotowicz, K. (2014). This POODLE Bites: Exploiting The
SSL 3.0 Fallback. Google. Retrieved 11 09, 2022, from
https://www.openssl.org/~bodo/ssl-poodle.pdf
<br>

<sup>54</sup>
Mosca, M., & Mulholland, J. (2017). A Methodology for Quantum Risk Assessment.
Global Risk Institute. Retrieved 11 07, 2022, from
https://globalriskinstitute.org/publications/3423-2/
<br>

<sup>30</sup>
Nejatollahi, H., Dutt, N., Ray, S., Regazzoni, F., Banerjee, I., & Cammarota, R.
(2019). Post-Quantum Lattice-Based Cryptography Implementations: A
Survey. ACM Computing Surveys, 51(6), 1-41. doi:doi.org/10.1145/3292548
<br>

<sup>53</sup>
NIST. (2018). Framework for Improving Critical Infrastructure Cybersecurity. NIST.
doi:https://doi.org/10.6028/NIST.CSWP.04162018
<br>

<sup>1, 26, 35</sup>
NIST. (2022, 09). Computer Security Resource Center. Retrieved 10 04, 2022, from
Selected Algorithms 2022: https://csrc.nist.gov/Projects/post-quantum-
cryptography/selected-algorithms- 2022
<br>

NSA. (2021). Cybersecurity. Retrieved 10 25, 2022, from Quantum Key Distribution
and Quantum Cryptography: https://www.nsa.gov/Cybersecurity/Quantum-
Key-Distribution-QKD-and-Quantum-Cryptography-QC/
<br>

<sup>43</sup>
Qualys Inc. (2022, 09 4). SSL Pulse. Retrieved 11 09, 2022, from Monthly Scan
September 2022: https://www.ssllabs.com/ssl-pulse/
<br>

<sup>51, 52</sup>
Rahimi, N., Reed, J. J., & Gupta, B. (2018). On the Significance of Cryptography as
a Service. Journal of Information Security, 9 , 242 - 256.
doi:doi.org/10.4236/jis.2018.94017
<br>

<sup>6,7,13</sup>
Rieffel, E., & Polak , W. (2011). Quantum Computing: A Gentle Introduction. MIT
Press. Retrieved 10 12, 2022, from
[http://mmrc.amss.cas.cn/tlb/201702/W020170224608150244118.pdf](http://mmrc.amss.cas.cn/tlb/201702/W020170224608150244118.pdf)
<br>

Shor, P. W. (1997). Polynomial-Time Algorithms for Prime Factorization and
Discrete Logarithms on a Quantum Computer. SIAM Journal on Computing,
26 (5), 1484-1509. doi:doi.org/10.1137/S0097539795293172
<br>

<sup>31, 34</sup>
Singh, S. (2000). The Code Book: The Sience of Secrecy from Ancient Egypt to
Quantum Cryptography. First Anchor Books. Retrieved 10 25, 2022
<br>

<sup>49</sup>
Sullivan , B. (2010). Cryptographic Agility. Microsoft Corporation. Retrieved 11 12,
2022, from [http://media.blackhat.com/bh-us-](http://media.blackhat.com/bh-us-)
10/whitepapers/Sullivan/BlackHat-USA- 2010 - Sullivan-Cryptographic-Agility-
wp.pdf
<br>

<sup>39</sup>
Wang, L.-J., Zhanh, K.-Y., Wang, J.-Y., Cheng, J., Yang, Y.-H., & Tang, S.-B.
(2021). Experimental authentication of quantum key distribution with post-
quantum cryptography. npj Quantum Information, 7 , 1 - 7.
doi:10.1038/s41534- 021 - 00400 - 7
<br>

Wang, X., Yin, Y. L., & Yu, H. (2005). Finding Collisions in the Full SHA-1. Retrieved
11 09, 2022, from
https://www.iacr.org/archive/crypto2005/36210017/36210017.pdf
<br>

<sup>36, 47</sup>
Wiesmaier, A., Alnahawi, N., Grasmeyer, T., Geißler, J., Zier, A., Bauspieß, P., &
Heinemann, A. (2021). On PQC Migration and Crypto-Agility. Retrieved 11
09, 2022, from https://arxiv.org/ftp/arxiv/papers/2106/2106.09599.pdf
<br>

Young, P. (2019). Using Period Finding to Factor an Integer. Retrieved 11 15, 2022,
from https://young.physics.ucsc.edu/150/period.pdf
<br>

Zurek, W. H., & Wootters, W. (1982, October). A single quantum cannot be cloned.
Nature, 299(5886), 802-803. doi:doi:10.1038/299802a0



# CHAPTER: 8. SHOR ’ S ALGORITHM

## 8. SHOR ’ S ALGORITHM

Shor’s algorithm uses a combination of classical and quantum computing
operations.
The algorithm is applied when we have an 𝑁=𝑝∗𝑞 and want to find p and q.
The fundamental operations consist of 7 steps:

To solve the factorization problem, Shor’s algorithm transforms the problem into a
problem of period finding. Fortunately, it’s still impossible for classical computers to
solve even with this technique. Still, as we will see, it allows a quantum computer to
achieve exponential speed-up and solve the factorization. Therefore the solution
relies on the period 𝑟 to the function:^57
𝑓(𝑥)= 𝑎𝑥 (𝑚𝑜𝑑 𝑁) (8.1)^

Step 1: Choose 𝒂

To begin the operations, we need some integer 𝑎.
We can choose some random 𝑎 that is coprime to N (no common factor).
If gcd(𝑎,𝑁)≠ 1 , which is very unlikely. We have already found a solution.
Usually, gcd(𝑎,𝑁)= 1 , and we can continue.^58

Step 2: Find period 𝒓

We now have to solve 𝑎𝑟 (𝑚𝑜𝑑 𝑁) with a given N, chosen 𝑎 and unknown 𝑟.
This is the part where quantum computing comes in.
Without going into too much depth, Shor’s algorithm uses quantum phase estimation
to create a superposition of all 𝑟, and if we measure, we receive a state like the

following: φ=𝑠𝑟, where 𝑠 is a random integer between 0 and 𝑟− 1 and r is the period.

Extracting r is easy enough.^59

57
58 (Young, 2019, S. 1)^
59 (Young, 2019, S. 1)^
(Shor, 1997, S. 18)


Quantum phase estimation allows quantum computers to achieve exponential
speed-up compared to classical algorithms because finding 𝑟 would be challenging
for classical algorithms.

Before continuing, r must be tested to make sure it’s even and 𝑎

𝑟 2
+ 1 !≡
0 ( mod p q ). Otherwise, it would be divisible by N.
We know that 𝑎𝑟 Mod N= 1 , which means that (𝑎𝑟− 1 ) mod N= 0.
If r is even, we then get:

```
𝑎𝑟− 1 = (𝑎
```
```
𝑟 2
− 1 )(𝑎
```
𝑟 2
+ 1 ) (8.2)^
This means that the Greatest Common Divisor (gcd) of N and one of the factors

(𝑎

```
𝑟 2
− 1 )and (𝑎
```
```
𝑟 2
+ 1 ) are either p or q.^60
```
Step 3: Find 𝐠𝐜𝐝 (𝒂𝒓𝟐 − 𝟏,𝐍) 𝐚𝐧𝐝 𝐠𝐜𝐝 (𝒂𝒓𝟐 + 𝟏,𝑵)

Finding the gcd can quickly be done using Euclid’s algorithm, and the results will be
factors of N. Thus, we have the solution to the factoring problem.

### 60

```
(Chuang & Nielsen, 2010, S. 633)
```




