#  Decentralization

## How decentralized is zkSync?

Technologically, decentralization is not a binary option but rather a continuum of design choices. The following rough scale can be used to asses a level of decentralization of a crypto project with regard to the ownership of assets:

1. Centralized custody (fully trusted): Coinbase
2. Collective custody (trust in the honest majority): sidechains
3. Non-custodial via fraud proofs (trust in the honest minority): optimistic rollups
4. Non-custodial, centrally operated (trustless): Argent
5. Multi-operator (trustless<sup>\*</sup>, weak censorship-resistance): Cosmos
6. Peer-to-peer (trustless<sup>\*</sup>, strong censorship-resistance): Ethereum, Bitcoin

Right now, **zkSync** is at level 4 of this scale. It is fully non-custodial with zero trust assumptions: the **zkSync** operator has no power to do anything with user's assets without explicit authorization. Users can always withdraw all of their funds to Ethereum without any cooperation from operator (even if the latter is completely shut down and unresponsive).

However, the daily operation of the **zkSync** network at the moment depends on the health of the computational service provider who generates zero-knowledge proofs for the blocks.

<span class="footnote"><sup>*</sup> For L1 networks, trustlessness is only guaranteed by the ability (and to the extent of that ability) of honest minority users to fork away from the network taken over by dishonest majority. For L2 protocols, this is not necessary, because users can ultimately rely on L1 to circumvent exit censorship.</span>

## Who operates zkSync now?

Currently, the computational service provider for the **zkSync** network is "Stichtig ZK Sync", a non-profit Dutch foundation registered under RSIN 861038204 in Kingsfordweg 151, 1043GR Amsterdam. The service will operated at break-even cost until the transition to full decentralization.

## Decentralizaton roadmap

Matter Labs is committed to developing **zkSync** into a fully decentralized protocol.

We outlined the decentralization roadmap in the [zkSync vision](https://medium.com/matter-labs/introducing-zk-sync-the-missing-link-to-mass-adoption-of-ethereum-14c9cea83f58) document. We intend to achieve it by introducing an independent consensus mechanism for block construction with two different roles: Validators and Guardians. (Note, that the security of **zkSync** will not be affected by introducing an additional consensus mechanism, since the final verification of state transition proofs still will be performed by a smart contract on the Ethereum mainnet.)
