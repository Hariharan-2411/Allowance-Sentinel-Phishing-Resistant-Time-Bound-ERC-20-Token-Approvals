
# Allowance Sentinel

### Phishing-Resistant, Time-Bound ERC-20 Token Approvals

Allowance Sentinel is a smart contract security layer designed to protect users from phishing and token-drain attacks caused by unlimited ERC-20 approvals.

In today’s DeFi ecosystem, users often grant unlimited token approvals to dApps for convenience. If a malicious contract gains access to that approval, it can drain the user’s entire token balance without further confirmation. Allowance Sentinel solves this problem by introducing controlled, time-bound, and amount-limited token grants.

Instead of approving each dApp directly, the user approves the Sentinel once. The Sentinel then enforces:

* Maximum spendable amount
* Expiration time (time-to-live)
* Explicit spender identity
* Global panic mode to freeze all spending

Only transactions that match an active grant are allowed to execute. Any attempt to overspend, spend after expiry, or spend during panic mode will automatically revert.

---

## Architecture Overview

The system consists of:

* **User Wallet** – Holds ERC-20 tokens
* **TestToken (ERC-20)** – Standard token implementation
* **Allowance Sentinel** – Enforcement layer between wallet and dApps
* **Grant System** – Stores amount + expiry per user
* **Panic Mode** – Emergency switch to block all spending

### Flow:

1. User deploys ERC-20 TestToken.
2. User deploys AllowanceSentinel.
3. User approves Sentinel as spender.
4. User creates a time-bound grant.
5. dApp calls `spendFrom()` via Sentinel.
6. Sentinel verifies grant rules before calling `transferFrom()`.

---

## Key Features

* Converts unlimited approvals into controlled permissions
* Automatic expiry of grants
* Prevents overspending beyond configured limits
* Emergency freeze mechanism (panic mode)
* Fully compatible with existing ERC-20 tokens
* No token custody — Sentinel only forwards validated transfers

---

## Smart Contract Functions

* `setGrant(spender, token, amount, validForSeconds)`
* `revokeGrant(spender, token)`
* `setPanicMode(bool)`
* `spendFrom(user, token, amount)`

---

## Security Benefits

* Limits potential loss from phishing attacks
* Prevents long-term abuse of approvals
* Provides fast emergency response
* Moves ERC-20 approvals closer to the principle of least privilege

---

## Experimental Deployment

The contracts were deployed and tested on the Sepolia testnet using:

* Solidity 0.8.x
* Remix IDE
* MetaMask
* Etherscan

Experiments demonstrated:

* Successful controlled spending under valid grants
* Reversion when panic mode is enabled
* Reversion after grant expiry
* Modest gas overhead compared to direct approvals

---

## Limitations

* Does not protect against private key compromise
* Requires trust in Sentinel implementation
* Introduces slight additional gas cost
* Current version supports simple (user, token) mapping

---

## Future Improvements

* Per-dApp grant policies
* User-friendly dashboard for active grants
* Integration with account abstraction wallets
* Dynamic risk-based grant sizing

---

## Conclusion

Allowance Sentinel offers a practical and backward-compatible way to reduce ERC-20 approval risk. By transforming unlimited approvals into time-bound and amount-limited grants, it significantly reduces the damage a single phishing interaction can cause.

This project demonstrates that safer defaults in DeFi are possible without modifying the ERC-20 standard itself.

---
