# Huckleberry REPORT

# Huckleberry **Lend bug report - A malicious user can steal all funds deposited into the protocol**

sevarity: **Critical** Impact: Direct theft of all user funds Possible Loss: All TVL **(highest ~\$300k in history, now 50k)**



## **Bug Description**

Huckleberry Lending protocol allows depositing and borrowing \$TOM, which is an intersting bearing asset. However, an malicious user can do as following to exploit the protocol with this. The following description explains how this works. (A & B are two contracts deployed by the malicious attacker).

- A buy some \$FINN
- (1) B flashloan (flashswap)  large amount of stable coin (e.g. USDC.m, ETH is also available)
- B deposit all the flashloaned assets as collateral
- **B borrow all assets in the lending pools.**
- A flashswap to get some \\$FINN from Huckleberry dex and deposit to get \​\$TOM
- Then do a loop:
  - A deposit \$TOM as collateral
  - B borrow \$TOM using the pre-deposited collateral
  - B transfer \$TOM to A
- Exit the loop until it reaches the borrow capacity of B.
- A transfer \$FINN directly to the \$TOM contract address. As the price oracle calculates the \$TOM as `exchangeRate * price_of_FINN`.(donate \$FINN to make price of \$TOM rise). **This will generate bad debts for B**) **THIS GREATLY INCREASES THE COLLATERAL VALUE OF A**.
- A liquidate B to get the collateral (and withdraw the underlying token) and liquidation incentives.
- A withdraw all \$TOM he can withdraw.
- A repay the flashloan of (1)

At the end of the exploit, B will have a huge bad debt. And A will get all the funds in the protocol.

## **Impact**

A malicious user can steal all funds deposited into the protocol. **(highest ~\$300k in history, now 50k)**

## **Risk Breakdown**

Difficulty to Exploit: Easy Weakness: Critical, price manipulation

## **Recommendation**

Please disable \$TOM from borrowing.

## **References**

cream finance exploit: https://mudit.blog/cream-hack-analysis/ 

0vix finance exploit: https://twitter.com/0vixProtocol/status/1653076307087966214?s=20