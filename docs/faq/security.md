# Security

[[toc]]

## Security overview

::: warning
**zkSync v1.0** is in alpha. Blockchains and zero-knowledge proofs are still experimental technologies with rapidly evolving attack vectors. **zkSync** relies on cutting-edge cryptography that has never been used in production before. While Matter Labs is rigorously following scientific and engineering best practices with regard to security, we can not provide 100% fault-free guarantee. Use **zkSync** at your own risk and do not put more money into it than you can afford to lose.
:::

[zkSync protocol design](https://github.com/matter-labs/zksync/blob/master/docs/protocol.md) document explicitly lists the protocol's cryptographic assumptions and security properties.

In a nutshell, the protocol's claim is that, given correct implementation and validity of cryptographic assumptions, funds placed into **zkSync** will have the same security guarantees as if they are held in an Ethereum account without any additional requirements on the user part. In particular:

- Users do not need to monitor the network.
- Private keys can be held in cold storage.
- Operators can not steal funds or corrupt the **zkSync** state in any way.
- Users can eventually withdraw their assets onto the mainnet, regardless of cooperation from **zkSync** operators.

Several mechanisms are used to fulfill these guarantees, discussed below.

### Validity proofs

**zkSync** is built on [zkRollup architecture](/faq/tech.html#zkrollup-architecture). This means, every single user transaction is verified by a smart contract on the Ethereum mainnet by means of verifying the proof of the validity of the block. Thus, no operator can ever move the system into an incorrect state or take users money.

See [this article](https://medium.com/starkware/validity-proofs-vs-fraud-proofs-4ef8b4d3d87a) for a further overview of the benefits of the validity proofs.

### Priority queue

In the ultimate emergency case of all operators being shut down or becoming unresponsive, the emergency exit mechanism ensures that users will keep control of their assets. It works as follows.

1. If transactions of a user are being for any reason ignored by the operator, an exit request can be submitted directly on mainnet into the **priority queue**.
2. Operators are obliged to process priority queue requests within a short time window (~1 week).
3. In case the operators fail to process the requests, the system enters **exodus mode** and every user can immediately exit all of their assets by making a direct transaction on the Ethereum mainnet.

### Upgrade mechanism

The version 1.0 of **zkSync** protocol comes with a contract upgrade mechanism in order to facilitate faster design iterations. However, users have a fundamental right to opt-out of a future upgrade. A new upgrade must be announced via the **zkSync** contract and all users get 4 week notice period to exit in case they don't like the changes.

::: warning
the beta will be deployed with 1 day notice period, which will soon be bumped up to 1 week, then to 2 weeks, and finally to 4 weeks. In the future, this opt-out mechanism will be replaced by a strict opt-in.
:::

## Cryptography used

Although **zkSync** is built on some of the most cutting-edge cryptography (such as PLONK and RedShift), we were very conservative with respect to security choices made in the protocol. Every component relies exclusively on well established cryptographic assumptions widely considered secure in the academic and professional security community.

### Primitives

|Primitive|Application|Cryptographic assumption|
|-|-|-| 
|[PLONK](https://eprint.iacr.org/2019/953)|Proof system (v1)|1, 3|
|[RedShift](https://eprint.iacr.org/2019/1400)|Proof system (v2)|1|
|[SHA256](https://en.wikipedia.org/wiki/SHA-2)|Hash function|1|
|[Rescue](https://eprint.iacr.org/2019/426.pdf)|Hash function|1, 2|
|[muSig](https://eprint.iacr.org/2018/068)|Signature scheme|1, 2, 3|

### Cryptographic assumptions

1. [Collision-resistance](https://en.wikipedia.org/wiki/Collision_resistance)
2. [Pseudo-randomness](https://en.wikipedia.org/wiki/Pseudorandomness)
3. [Elliptic curve DLP](https://en.wikipedia.org/wiki/Discrete_logarithm#Cryptography)

## Universal CRS setup

The version 1.0 of **zkSync** protocol is using the PLONK proof system which requires a "trusted setup" of a Common Reference String (CRS). In PLONK, this setup can be done once and be reused by any number of applications (this is called Universal CRS). If at least one participant deletes the entropy (randomness) used to provide their contribution, the setup is secure. Having universal and not application-specific setup significantly reduces trust assumptions, because larger number of prominent and respected members of the community have incentive to participate in it, and more scrutiny can be expected around the trusted setup ceremony.

Another big advantage of a universal CRS is that updates and bugfixes do not require their own trusted setup ceremonies (which are very difficult from the logistical and security perspectives).

Matter Labs [participated](https://www.aztecprotocol.com/ignition/participant/0x04a23ba68e4469061cd461e8b847e820d4ced948?timestamp=1587551054947) in the global Ignition trusted setup ceremony for PLONK on BN256 elliptic curve, coordinated by AZTEC protocol:

<table>
<tr>
    <td>Participation address</td>
    <td>0x04a23ba68e4469061cd461e8b847e820d4ced948</td>
</tr>
<tr>
    <td>Transcript hash</td>
    <td>0x1f6647d91a9e667173640b67b654cabc81ceee98d6100f259788afb34a3fc529</td>
</tr>
<tr>
    <td>Signature</td>
    <td>0x3f4ff7b9a6668c23c9ba45d73d1d9d0902b881191d97b307969b63f52296f2326d437ea04dd67a2ebe57a691025d7d31bb0dae88e8023a0d9b15ad599c3eb9351b</td>
</tr>

</table>

The ceremony ran from October 2019 until December 2019, with 176 participants from over 30 countries collaborating to compute a secure database of encrypted points, including [Vitalik Buterin](https://twitter.com/VitalikButerin/status/1225856246307311616) and other prominent members of the crypto community. Full ceremony transcript with the list of indviduals and organizations who claimed their contribution [is available here](https://www.aztecprotocol.com/ignition/). You can use [this script](https://github.com/matter-labs/ignition-verification) to verify the contributions of the listed participants.

Despite relying on a universal trusted setup, we argue that **zkSync** can be called a fully trustless protocol. The reason for this is that there are no systems without some form of a trusted setup. Most users do not personally verify and compile the source code of their wallets, full nodes, mining software, and certainly do not verify the circuits of the hardware all these things run upon. In our opinion, the ease of concealed collusion of developers and experts in these systems is much higher than in a trusted setup with hundreds of well-known participants. At the same time, the ongoing operation of **zkSync** requires zero trust from any party whatsoever, which is unique among all L2 scaling solutions.

Nonetheless, we encourage you to check the list of the contributors of the Ignition ceremony and make your own opinion on whether there is at least one trustworthy person or organisation among them. And in the future, we strive to eliminate trust assumptions altogether by embracing a transparent zero-knowledge proof system such as [RedShift](https://eprint.iacr.org/2019/1400) (developed by Matter Labs).

## Security audit

The audit by [ABDK Consulting](https://www.abdk.consulting/) for **zkSync** v1.0 protocol is complete, the results are [available here](https://zksync.io/zksync-1.0-audit.pdf). The audit scope includes the protocol design, applied cryptography, smart contracts and zero-knowledge circuits.
