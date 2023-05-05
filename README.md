# ordscript
Proposal of a Turing-complete Protocol for ordinals (Bitcoin-Inscriptions)

Example-Inscriptions:

# Op-code "deploycode"

``` js
{
"p":"ordscript",
"op":"deploycode",
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
"end":"10",
"init":"00000000000000000000",
"owner":"bc1p7d95end6905m9pskzvvlv3sn7uhrr7sr3rg0wrtedawtdw4r03asg9d0cs",
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
"start":"0",
"end":"10",
"init":"00000000000000000000",
}
```

Basic concept: code, memory-definition and execution are the basic inscription-opcodes. The state of the memory-block will be handled offchain, but code-definition and exec-calls will be settled 100% onchain (similar to the BRC-20 protocol) 







