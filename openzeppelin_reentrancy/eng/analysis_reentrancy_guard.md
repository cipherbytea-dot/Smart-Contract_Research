# Deep Dive Analysis of OpenZeppelin ReentrancyGuard 🔐

**Author:** M. Afif Al Falah  
**Topic:** Smart Contract Security and Gas Optimization  

---

## The Concept: What is Reentrancy? 🧠

Reentrancy is a critical vulnerability where an attacker exploits the delay between checking a balance and updating the state.  
It violates the **Checks-Effects-Interactions** pattern.

---

## The Bank Teller Analogy 🏦

Imagine a bank teller (the smart contract) handling a withdrawal:

- **Step 1 (Check):** The teller checks if you have funds.  
- **Step 2 (Interact):** The teller hands you the cash.  
- **Step 3 (Effect):** The teller writes down your new balance.

---

## The Attack ⚠️

An attacker interrupts **Step 2**.

Before the teller can write down the new balance in **Step 3**, the attacker asks for the money again via a fallback function.  
Since the record has not been updated, the teller believes the funds are still there and hands out cash again.

This loops until the contract is drained.

---

## The Solution: `nonReentrant` Modifier 🛡️

OpenZeppelin uses a **mutex (mutually exclusive flag)** pattern:

- **Phase 1 (Check and Lock):**  
  Before executing logic, check if the status is `NOT_ENTERED`.  
  If yes, change status to `ENTERED` *(lock the door)*.

- **Phase 2 (Execute):**  
  Run the function logic.

- **Phase 3 (Unlock):**  
  Change status back to `NOT_ENTERED`.

---

## Gas Optimization: `uint256` vs `bool` ⛽

If we look at the source code, OpenZeppelin uses `uint256` constants instead of booleans.

```solidity
uint256 private constant NOT_ENTERED = 1;
uint256 private constant ENTERED = 2;
```

## Why specific numbers `1` and `2` instead of `true` or `false`?

### Reason A: Storage Slot Efficiency (Read vs Write)

- `uint256` (32 Bytes) occupies a full storage slot.  
  Writing to it is a simple `SSTORE` operation — it overwrites the entire slot *(Write-Only)*.

- `bool` (1 Byte) occupies a partial slot.  
  To change it, the EVM must:
  - `SLOAD` (Read) the slot first  
  - Ensure it does not corrupt other variables packed in the same slot  
  - Modify the bits  
  - Then `SSTORE`

**Verdict:**  
`uint256` avoids the extra `SLOAD`, making it cheaper for this specific use case.

---

### Reason B: The 1 and 2 Trick (Warm Access) 🔥

**Avoid 0 (Zero):**

Changing a storage value from `0` to `1` *(Zero → Non-Zero)* is very expensive in Ethereum  
(~20,000 Gas) because it initializes a new slot.

**Use 1 and 2:**

Changing from `1` to `2` *(Non-Zero → Non-Zero)* is much cheaper  
(~2,900 to 5,000 Gas) because the storage slot is already initialized or *warm*.

**Verdict:**  
By toggling between `1` and `2`, the contract avoids the heavy cost of initializing a zero-slot, keeping gas costs stable for users.

## 📚 Resources 

- Source smart contract (OpenZeppelin ReentrancyGuard)  
  [Open ReentrancyGuard.sol on GitHub](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/ReentrancyGuard.sol)

- Official OpenZeppelin Contracts repository  
  [OpenZeppelin Contracts Repository](https://github.com/OpenZeppelin/openzeppelin-contracts)