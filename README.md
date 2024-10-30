# vFHE.github.io

# Verifiable Fully Homomorphic Encryption: Bridging Computational Integrity with Encrypted Computations

## Introduction
Fully Homomorphic Encryption (FHE) is a groundbreaking cryptographic primitive that enables computations to be performed directly on encrypted data without requiring decryption. This capability preserves data confidentiality throughout the computation process, ensuring that sensitive information remains secure even when processed by untrusted servers. However, while FHE safeguards data privacy, it does not inherently guarantee the correctness of the computations performed. In critical applications such as healthcare diagnostics, incorrect computations can have severe, even life-threatening, consequences.
We explore the necessity of integrating verifiability into FHE schemes to ensure both data confidentiality and computational integrity. We examine the mathematical foundations of various verifiability techniques and analyze how these foundations influence their compatibility with FHE. Additionally, we present an exemplary construction of a Verifiable Fully Homomorphic Encryption (vFHE) scheme using lattice-based Succinct Non-interactive Arguments of Knowledge (SNARKs).
Motivation

## Limitations of Traditional FHE Schemes
FHE schemes like Brakerski-Gentry-Vaikuntanathan (BGV), Brakerski-Fan-Vercauteren (BFV), and Torus FHE (TFHE) have revolutionized secure computation by allowing arbitrary computations on encrypted data without decryption. These schemes operate over polynomial rings, with security based on the hardness of lattice problems such as Learning With Errors (LWE) and Ring-LWE. The reliance on lattice-based cryptography provides strong security guarantees against both classical and quantum attacks.
Despite their advantages in preserving data confidentiality, traditional FHE schemes lack mechanisms to verify the correctness of computations performed by untrusted servers. This absence of computational integrity can lead to critical issues, especially when the accuracy of results is as important as the confidentiality of the data.
The Need for Verifiable Computations
While FHE ensures that data remains confidential during computation, it does not guarantee that untrusted servers perform computations correctly. In critical applications like medical diagnostics, incorrect computations can have dire consequences.
Example Scenario: A healthcare provider outsources the computation of a cancer detection algorithm on encrypted medical images to a cloud server. Although FHE keeps patient data confidential, there is no assurance that the server executes the computation correctly. Servers might return incorrect results due to hardware malfunctions or software bugs. Inaccurate computations could lead to misdiagnoses—either failing to detect cancer when it is present (false negatives) or incorrectly diagnosing cancer when it is not present (false positives). Both outcomes can have severe implications for patient health.
Therefore, it is crucial to enhance FHE schemes with verifiability to ensure both data confidentiality and computational integrity. Verifiable Fully Homomorphic Encryption (vFHE) allows clients to verify that the server has performed computations correctly on the encrypted data without compromising data confidentiality. This ensures that the healthcare provider can trust the results returned by the server, making vFHE indispensable in scenarios where correctness is paramount.

# On Verifiable FHE (FHE with Verifiable Computation)
A practical vFHE scheme should satisfy the following properties:
- Correctness: Homomorphic operations on encrypted data must produce ciphertexts that decrypt to the correct result.
- Security: The scheme should be semantically secure under chosen-ciphertext attacks (IND-CCA1 or IND-CCA2) to prevent key-recovery attacks via decryption queries.
- Verifiability: Clients must be able to verify that the server has performed computations correctly, even if the server behaves maliciously.
- Efficiency: The verification process should introduce minimal computational and communication overhead.
Without chosen-ciphertext security, an adversary could manipulate ciphertexts and use the decryption oracle to extract the secret key, compromising the entire system. Therefore, integrating verifiability into FHE not only ensures computational integrity but also enhances security against active attacks.

## Comparison of Different Verifiability Techniques
Enhancing FHE with verifiability involves integrating proof systems that allow verification of computations on encrypted data. The mathematical foundations of these verifiability techniques directly influence their compatibility with FHE schemes.

## Incompatible Techniques
Certain cryptographic methods are unsuitable for vFHE due to their nature:
- Message Authentication Codes (MACs) and Digital Signatures: Designed for data integrity and authenticity of plaintext messages, they cannot verify computations on encrypted data without decryption. They lack the homomorphic properties necessary for verifying arbitrary computations on encrypted data.
- Trusted Execution Environments (TEEs): Depend on hardware-based trust, introducing additional trust assumptions that may not align with threat models where servers are untrusted or potentially compromised.

## Compatible Verifiability Techniques
We focus on cryptographic proof systems like Succinct Non-interactive Arguments of Knowledge (SNARKs) and Scalable Transparent Arguments of Knowledge (STARKs), which can verify arbitrary computations.

## Mathematical Foundations and Compatibility
The compatibility of verifiability techniques with FHE schemes is heavily influenced by their underlying mathematical structures:
FHE Schemes: Operate over polynomial rings Rq=Zq[x]/(f(x))R_q = \mathbb{Z}_q[x]/(f(x))Rq​=Zq​[x]/(f(x)), with security based on lattice problems (e.g., LWE, Ring-LWE).
Verifiability Techniques: Use mathematical constructs (fields, rings, lattices) that may or may not align with those of FHE schemes.
Pairing-Based SNARKs
Mathematical Foundations: Utilize bilinear pairings over elliptic curves defined over finite fields Fq\mathbb{F}_qFq​.
Bilinear Pairing Function: e:G1×G2→GT​, where G1,G2,GT​ are cyclic groups of prime order q.
Properties:
Bilinearity: e(aP,bQ)=e(P,Q)^{ab}for P∈G1,Q∈G2,a,b∈Zq.
Non-degeneracy: e(P,Q)≠1,if P and Q are generators.
Compatibility with FHE: Low.
Reason: FHE schemes are based on polynomial rings and lattices, not elliptic curves or finite fields. The mathematical mismatch makes integration inefficient and complex.
Influence: The inability to directly map computations over polynomial rings to operations over elliptic curves hinders compatibility. Techniques like encoding ring elements into elliptic curve points are inefficient and impractical for large-scale computations.
Field-Based SNARKs
Mathematical Foundations: Operate over finite fields Fq​.
Arithmetic Circuits: Computations are represented as circuits with operations in Fq​.
Compatibility with FHE: Moderate.
Reason: While FHE schemes use rings, they can be related to fields if the ring modulus is prime. However, mapping ring operations to field operations introduces overhead.
Influence: The need to adapt polynomial ring computations to finite field arithmetic affects efficiency. Specialized techniques are required to represent ring elements and operations within field-based SNARKs.
Ring-Based SNARKs
Mathematical Foundations: Operate over rings, specifically polynomial rings Rq=Zq[x]/(f(x)).
Arithmetic Circuits over Rings: Computations use ring operations directly.
Compatibility with FHE: High.
Reason: Direct alignment with the mathematical structures of FHE schemes.
Influence: Enables efficient proof generation and verification by leveraging the same ring operations used in FHE. Homomorphic properties can be directly utilized within the proof system without additional encoding or conversion.
Lattice-Based SNARKs
Mathematical Foundations: Based on lattice problems like LWE and Short Integer Solutions (SIS).
Lattices: Discrete additive subgroups of Rn, defined by integer linear combinations of basis vectors.
Commitment Schemes: Use lattice-based commitments, e.g., Com(m;r)=A⋅s+emod  q, where A is a public matrix, sss is a secret vector, and eee is an error term.
Compatibility with FHE: High.
Reason: Shared security assumptions and mathematical structures with FHE schemes.
Influence: Facilitates seamless integration, as both the FHE and the verifiability techniques operate within the same lattice framework. Computations over lattices in the FHE scheme can be directly related to those in the SNARK, enabling efficient proof generation and verification.
STARKs
Mathematical Foundations: Utilize polynomial commitment schemes and cryptographic hash functions.
Probabilistically Checkable Proofs (PCPs): Allow verification of computations via random sampling.
FRI Protocol: Fast Reed-Solomon Interactive Oracle Proofs of Proximity, tests proximity to low-degree polynomials over finite fields.
Compatibility with FHE: Moderate.
Reason: STARKs do not exploit specific algebraic structures of FHE schemes but can work over general fields.
Influence: Lack of mathematical alignment leads to larger proof sizes and increased computational overhead. While STARKs are flexible, they do not benefit from the efficiencies that arise from shared mathematical structures with FHE schemes.
Summary of Compatibility
High Compatibility: Ring-based and lattice-based SNARKs.
Moderate Compatibility: Field-based SNARKs and STARKs.
Low Compatibility: Pairing-based SNARKs.
Influence of Mathematical Structures on Compatibility
The mathematical structures underlying verifiability techniques determine how naturally they integrate with FHE schemes:
Shared Structures: Techniques that use rings or lattices align with FHE schemes, enabling direct mapping of operations and efficient integration.
Different Structures: Techniques based on elliptic curves or general fields require additional layers of abstraction or conversion, introducing inefficiencies and increasing computational overhead.
Case Study: Exemplary Construction of a Verifiable FHE Scheme
To illustrate the integration of verifiable computation with FHE, we consider an example using a lattice-based SNARK in a healthcare scenario.
Scenario: A healthcare provider wants to outsource the computation of a cancer detection algorithm on encrypted medical images to a cloud server. The provider needs to ensure that the server performs the computation correctly without accessing sensitive patient data.
Construction Steps
Key Generation:
The healthcare provider generates:
FHE Key Pair: Secret key sk and public key pk for the FHE scheme (e.g., BFV or BGV).
SNARK Parameters: Proving key pk_{SNARK}​ and verification key vk_{SNARK}​ for the lattice-based SNARK.
Note: The SNARK setup may involve a trusted setup phase, which can be mitigated using universal or updatable setups.
Circuit Definition:
Both parties agree on the arithmetic circuit C representing the cancer detection algorithm.
The circuit is expressed over the polynomial ring Rq=Zq[x]/(f(x)), matching the FHE scheme's structure.
Encryption:
The healthcare provider encrypts the medical images mmm using pk: c=Enc_{pk}(m)
The ciphertext ccc is sent to the cloud server.
Homomorphic Computation:
The cloud server performs the homomorphic evaluation of C on c: c′=EvalC(c)
This results in a ciphertext c′ encrypting C(m), the output of the cancer detection algorithm.
Proof Generation:
The server generates a proof π using the lattice-based SNARK: π=Prove_{pk}_{SNARK}(C,c,c′)
The proof attests that c′ is correctly computed from ccc using C, without revealing any information about mmm or intermediate values.
Result and Proof Transmission:
The cloud server sends c′ and π back to the healthcare provider.
Verification:
The provider verifies the proof using vk_{SNARK}​: Verify_{vk}_{SNARK}(C,c,c′,π)
Successful verification assures that the computation was performed correctly.
Decryption:
Upon successful verification, the provider decrypts c′ using sk: C(m)=Dec_{sk}(c′)
The decrypted result C(m) provides the diagnosis.
Advantages
Data Confidentiality: Patient data remains encrypted throughout the process.
Computational Integrity: The provider can verify the correctness of the computation.
Efficiency: Shared mathematical structures minimize computational and communication overhead.
Influence of Mathematics on Compatibility
Direct Mapping: The use of polynomial rings and lattices in both the FHE scheme and the SNARK allows for direct mapping of operations, reducing overhead.
Efficient Proofs: Lattice-based commitments and proofs are naturally integrated into the computation, enhancing efficiency.
Security Alignment: Shared security assumptions (e.g., hardness of lattice problems) ensure consistent security guarantees.
(Optional) What if We Use zkSNARK Instead of SNARK?
Using zero-knowledge SNARKs (zkSNARKs) can provide additional privacy benefits when the server has private inputs or wants to keep the computation itself confidential.
Benefits of Using zkSNARKs
Circuit Privacy: The server's computation details, such as proprietary algorithms or secret parameters, remain confidential.
Private Server Inputs: zkSNARKs allow the server to prove correctness without revealing its inputs.
Enhanced Privacy: Both client data and server computations are protected.
Considerations
Increased Computational Overhead: The zero-knowledge property adds complexity to proof generation and verification.
Trusted Setup: zkSNARKs often require a trusted setup, introducing potential vulnerabilities if not properly managed.
Mathematical Alignment: The underlying mathematics must still align with the FHE scheme for efficient integration.
Influence of Mathematics on Compatibility
Same Mathematical Structures: Using lattice-based zkSNARKs maintains compatibility with the FHE scheme.
Efficiency Trade-offs: The additional zero-knowledge property may introduce overhead, but mathematical alignment mitigates inefficiencies.
# Conclusion
Integrating verifiability into FHE schemes is essential for applications where computational integrity is as critical as data confidentiality. The mathematical foundations of verifiability techniques directly influence their compatibility with FHE schemes:
High Compatibility: Techniques like ring-based and lattice-based SNARKs share mathematical structures with FHE schemes, enabling efficient integration.
Moderate to Low Compatibility: Techniques based on different mathematical foundations (e.g., pairing-based SNARKs) face challenges due to the need for complex mappings or conversions.
# Key Takeaways:
Mathematical Alignment Is Crucial: Verifiability techniques must align with the mathematical structures of FHE schemes to achieve efficiency and practicality.
Security and Efficiency Balance: The choice of verifiability technique affects both the security and performance of the vFHE system.
Application Requirements Dictate Choices: Specific needs, such as server-side privacy or resource constraints, influence the selection of verifiability methods.
By carefully selecting verifiability techniques that align with the mathematical foundations of FHE schemes, we can develop practical vFHE systems that ensure both data confidentiality and computational integrity. This synergy is vital for secure deployment in real-world applications where trust in computational results is as crucial as data privacy.
