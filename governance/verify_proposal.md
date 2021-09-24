# How to verify a Timelock proposal

Timelock proposals are identified by a 32-byte hash, such as: `6B1705CD4D1F0471C2BA7D67A30441D998CA4A5BC5F68E784B1538E883BA4AFA`

We will decode a proposal from its hash using 3 tools: [etherscan](etherscan.io), the [4byte function signature database](https://www.4byte.directory/signatures), and the [adibas03](https://adibas03.github.io/online-ethereum-abi-encoder-decoder/#/decode) abi decoder.

## Find the calldata

Go to the [Timelock contract in Etherscan](https://etherscan.io/address/0xA5E64f55f1d7244475Ee3842E06295c1973482eD), and in the [events](https://etherscan.io/address/0xA5E64f55f1d7244475Ee3842E06295c1973482eD#events) tab, do a browser search for the proposal hash (the Etherscan event filtering is hard to use, regular browser search works fine).

Go to the [transaction in which the proposal was submitted](https://etherscan.io/tx/0x4a6c405fad393b24f0fd889bb8ae715b3fcca1f0a12c9ae079d072958c9dbbc7), click to see more, and decode the input data.

You will obtain a series of **target** and **data** variables. 

## Decoding the target

Each target is a contract where the Timelock will execute the call encoded in data. Grab the target address and enter it in Etherscan, or search for it in the Yield docs.

<code>[0x5a90358Ec01c551555536a1B2bd9DBFD733f61BA](https://etherscan.io/address/0x5a90358Ec01c551555536a1B2bd9DBFD733f61BA)</code> is the ChainlinkMultiOracle, we find.

Now, let’s decode the call.

## Decoding function calls

We are going to decode this data:
```
0xef532f2e3032000000000000000000000000000000000000000000000000000000000000000000000000000000000000a0b86991c6218b36c1d19d4a2e9eb0ce3606eb483030000000000000000000000000000000000000000000000000000000000000000000000000000000000000c02aaa39b223fe8d0a0e5c4f27ead9083c756cc2000000000000000000000000986b5e1e1755e3c2440e960477f25201b0a8bbd4
```

Take the 0x and the first eight characters and paste them in the [4byte signature database](https://www.4byte.directory/signatures/?bytes4_signature=ef532f2e). You will get the function signature.

Now we know that the Timelock is going to set some sources in the ChainlinkMultiOracle, let’s dig in.

## Decoding parameters

Go to the [adibas03 abi encoder/decoder](https://adibas03.github.io/online-ethereum-abi-encoder-decoder/#/decode), choose to decode, and paste the parameters you obtained from 4byte into the Argument Types field.

To get the function arguments, take the original data field from etherscan, and remove the 8 characters that we entered into 4byte. So from this:
```
0xef532f2e3032000000000000000000000000000000000000000000000000000000000000000000000000000000000000a0b86991c6218b36c1d19d4a2e9eb0ce3606eb483030000000000000000000000000000000000000000000000000000000000000000000000000000000000000c02aaa39b223fe8d0a0e5c4f27ead9083c756cc2000000000000000000000000986b5e1e1755e3c2440e960477f25201b0a8bbd4
```

To this:
```
0x3032000000000000000000000000000000000000000000000000000000000000000000000000000000000000a0b86991c6218b36c1d19d4a2e9eb0ce3606eb483030000000000000000000000000000000000000000000000000000000000000000000000000000000000000c02aaa39b223fe8d0a0e5c4f27ead9083c756cc2000000000000000000000000986b5e1e1755e3c2440e960477f25201b0a8bbd4
```

And then paste that in the Encoded data field from the decoder, you will obtain the decoded arguments.

You can go back to the [ChainlinkMultiOracle code](https://etherscan.io/address/0x5a90358Ec01c551555536a1B2bd9DBFD733f61BA#code) to get more information on what the parameters are.

The Timelock is asking the ChainlinkMultiOracle to set a new source:
 - BaseId: 0x303200000000 (USDC)
 - Base contract: [0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48](https://etherscan.io/address/0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48)
 - QuoteId: 0x303000000000 (ETH)
 - Quote contract: [0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2](https://etherscan.io/address/0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2)
 - Data source: [0x986b5E1e1755e3C2440e960477f25201B0a8bbD4](https://etherscan.io/address/0x986b5E1e1755e3C2440e960477f25201B0a8bbD4)

**Note:** Often, a proposal will be for grantRole, grantRoles, revokeRole, or revokeRoles. The role is a 4-byte parameter, such as `0xef532f2e`. We can enter this role in the [4byte database](https://www.4byte.directory/signatures/?bytes4_signature=0xef532f2e), and we will know which permission is being given (`setSources` in the example).
