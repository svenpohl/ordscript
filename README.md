# ordscript
Proposal of a Turing-complete Protocol for ordinals (Bitcoin-Inscriptions). Inspired by the BRC-20 protocol.

This proposal want's to start a public discussion:

Example-Inscriptions:

# Op-code "deploycode"
``` js
{
"p":"ordscript",
"op":"deploycode",
"version":"0.0.1",
"owner":"bc1p7d95end6905m9pskzvvlv3sn7uhrr7sr3rg0wrtedawtdw4r03asg9d0cs",
"id":"1",
"code":"0a0b0c0d0e0f10111213...fcfdfeff",
"entry":"4"
}
```

# Op-code "deploymem"
``` js
{
"p":"ordscript",
"op":"deploymem",
"id":"1",
"start":"0",
"end":"9",
"init":"00000000000000000000",
"owner":"bc1p7d95end6905m9pskzvvlv3sn7uhrr7sr3rg0wrtedawtdw4r03asg9d0cs"
}
```

Memory-blocks (defined by owner of the deploycode-incription) can only be extended by this owner himself.


# Op-code "exec"
``` js
{
"p":"ordscript",
"op":"exec",
"id":"1",
"caller":"bc1p7d95end6905m9pskzvvlv3sn7uhrr7sr3rg0wrtedawtdw4r03asg9d0cs",
"entry":"2"
"parameter":"04050607ff223344efefefa0",
}
```


# Op-code "setmeta"
``` js
{
"p":"ordscript",
"op":"setmeta",
"owner":"bc1p7d95end6905m9pskzvvlv3sn7uhrr7sr3rg0wrtedawtdw4r03asg9d0cs",
"id":"1",
"meta":"upgrade to v.0.0.2"
}
```

For protocoll-updates in the future. The circumstances will change in time, 
so maybe a ordscript-version will only support 2^32 Bytes of memory. Maybe the ordscript-community
will decide to find another consensus. So the first here suggested "setmeta" command is
"upgrade to v.0.0.2"

# Comment and description
Basic concept: code, memory-definition and execution are the basic inscription-opcodes. The state of the memory-block will be handled offchain, but code-definition and exec-calls will be settled 100% onchain (similar to the BRC-20 protocol) 

In contrast to "just hashing an offchain-interpreter" will the code-base and all signatures be handled onchain over the protocol. A invalid call (like transfer of a token) will be refused by the community.

This is just a basic concept without a full specification of the byte-interpreter. Another (extended) variant of this concept is to force the contract owner to validate the code-interpretation and publish an inscription of the full updated memory blocks (new memory-block-inscription simply will replace oder ones). There is no need to insist on such validation, because of the immutability of the deploycode-statement and a full history of signed exec-calles makes if possible for every node to calculate the latest state of the "ordscript - smartcontract". Example: If an user have send all his "FROG-Token" (represented by an Ordscript smartcontryt), the network will know and have consens of exactly the last balance ( zero ) and will refuse any additional "doublespend" (also similar to the BRC-20 protocol).

The ordscript - protocoll could start with a simple opcode-interpreter (only bytes and integer values) and later allow additional opcodes (floating-point variables, sha256-hashing, ZK-snarks, external contract calls, notifications...). 

Discussion open - have fun
(Sven P.)







