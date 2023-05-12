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
"code":"ordnopnopsetax00000001jmp00000031nopsetbx00000002setbx00000003addaxbxincraxdecrbxnotaxnotbxoraxbxnopnopend",
"functions": 
  [
  { name: 'save', address: 32, 
   params: [
           { name: 'name', type: 'string', address: 14, len: 10 }, 
           { name: 'id', type: 'uint32', address: 34, len: 4 } 
           ]
  }
],
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

# Command-Opcodes
Here a list of possible (still to be implemented) 27 Command-Opcodes for turing-complete instructions.

``` js
[
  { code: 'ord', length: 3, params: 0, example: 'ord' },
  { code: 'nop', length: 3, params: 0, example: 'nop' },
  { code: 'add', length: 7, params: 2, example: 'add ax bx' },
  { code: 'sub', length: 7, params: 2, example: 'sub ax bx' },
  { code: 'mul', length: 7, params: 2, example: 'mul ax bx' },
  { code: 'div', length: 7, params: 2, example: 'div ax bx' },
  { code: 'incr', length: 6, params: 1, example: 'incr ax' },
  { code: 'decr', length: 6, params: 1, example: 'decr ax' },
  { code: 'and', length: 7, params: 2, example: 'and ax bx' },
  { code: 'or', length: 6, params: 2, example: 'or ax bx' },
  { code: 'xor', length: 7, params: 2, example: 'xor ax bx' },
  { code: 'not', length: 5, params: 1, example: 'not bx' },
  { code: 'shl', length: 7, params: 2, example: 'shl ax 04' },
  { code: 'shr', length: 7, params: 2, example: 'shr ax 04' },
  { code: 'set', length: 13, params: 2, example: 'set ax 00000001' },
  { code: 'mov', length: 7, params: 2, example: 'mov ax bx' },
  { code: 'mva', length: 14, params: 3, example: 'mva 1 ax 0x00000002' },
  { code: 'mav', length: 14, params: 3, example: 'mav 4 ax 0x00000002' },
  { code: 'mvi', length: 6, params: 2, example: 'mvi 4 ax' },
  { code: 'miv', length: 6, params: 2, example: 'miv 4 bx' },
  { code: 'jmp', length: 11, params: 1, example: 'jmp 00000014' },
  { code: 'cmp', length: 7, params: 2, example: 'cmp ax bx' },
  { code: 'jie', length: 11, params: 1, example: 'jie 0000004d' },
  { code: 'jne', length: 11, params: 1, example: 'jne 0000004d' },
  { code: 'jig', length: 11, params: 1, example: 'jig 0000004d' },
  { code: 'jil', length: 11, params: 1, example: 'jil 0000004d' },
  { code: 'end', length: 3, params: 0, example: 'end' }
]

```


# Register:
```js

ax - 32 bit register
bx - 32 bit register
cx - 32 bit register
dx - 32 bit register
ix - 32 bit index register (e.g. for mvi)
cs - compare status, == 1, > 2, < 3

```

# Opcode "ord"
``` js
The entry-opcode of every ordscript program always begins with "ord". No Parameters.
```

# Opcode "nop"
``` js
Just "nop", no operation, only an increment of the pc (program counter) register to 3 bytes . No Parameters.
```

# Opcode "and"
``` js
Binary "and" operation: ax & bx -> result will be written in cx. Parameters: currently only ax and bx in this order
```

...

# Opcode "end"
``` js
The last-opcode "end" marks the end of every ordscript program. No Parameters.
```


# Script-example
Just a sequence of some ordscript opcodes (will be initialized in "deploycode":"code" as breakless code-buffer)

``` js
ord   nop nop  
set ax 00000001 
  jmp mark2 nop /* this is cool */ mark1: set bx 00000002  mark2: set bx 00000003      mark3:    
   add ax bx  final: incr ax   decr bx   not ax   not bx  or ax bx   nop   nop    
end     

without line breaks:
ordnopnopsetax00000001jmp00000031nopsetbx00000002setbx00000003addaxbxincraxdecrbxnotaxnotbxoraxbxnopnopend      
```




