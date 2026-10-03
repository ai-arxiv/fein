# ADR-0001: FEIN Architecture

- Status: Proposed
- Owner: Samruddhi Shekatkar
- Date: 2026-10-03

## Context

FEIN (Financial Enterprise Intelligence Network) coordinates economic transactions between independent software systems owned by different organizations.
Each organization has its own data, risk policies, and governance rules. One organization cannot fully trust another organization to complete a transaction correctly.
A central system (Escrow) could solve some of these problems, but it would reduce the control that each organization has over its own systems.
FEIN therefore needs a clear architecture boundary. It must help organizations bind their commitments, verify delivered work, and coordinate settlement without taking control of their internal systems.

## Decision

The proposed FEIN architecture uses an external coordination mechanism.
FEIN does not replace or control the internal systems of the participating organizations.

### System Boundary

FEIN has three main responsibilities:

1. Bind the transaction commitments.
2. Coordinate delivery verification.
3. Coordinate conditional settlement.

Other functions remain outside FEIN.

### Sovereign Runtimes

Each organization keeps control of its own runtime.

This includes its:

- Data
- Logic
- Keys
- Compute resources
- Policies

FEIN does not require organizations to use a common internal runtime or execution system.

### Transaction Control Block

The Transaction Control Block (TCB) stores the main information about a transaction.
It contains important transaction information, such as:

- Transaction ID (TID)
- Contract hash
- Verifier identity
- How long the transaction remains valid
- Maximum allowed risk
- Buyer capital commitment
- Seller's delivery verification code

The final TCB serialization and wire format are not yet decided.

### Escrow

The proposed architecture uses an escrow system where 2 out of 3 parties must agree to release the funds.

The three parties are:

- Buyer
- Seller
- Selected independent verifier

The escrow mechanism prevents one party from releasing or taking the funds alone.

### Verification

An independent verifier checks the delivery against the agreed criteria.

The verifier also takes part in the escrow authorization.

The rules for verifier selection, governance, and dispute handling are not yet defined.

### Settlement Coordination

The Settlement Coordinator coordinates Delivery versus Payment (DvP).

After successful verification, the coordinator produces three outputs:

1. A delivery receipt.
2. A token or accounting log entry.
3. A fund transfer instruction.

The current architecture does not guarantee that these outputs and the external payment happen at the same time.

### Upstream Discovery and Negotiation

FEIN does not provide participant discovery or negotiation.

These activities happen before the FEIN transaction.

The documentation mentions technologies such as the NANDA Index and A2A as examples. 

### External Payment Rails

FEIN does not operate a payment rail.

Actual money movement uses an external payment rail.

The documentation mentions AP2 as an example. AP2 helps an AI agent make a payment on behalf of a user while proving that the agent was authorized to make that payment. It is not an approved FEIN architecture choice.

## Options Considered

### External Coordination — Selected

FEIN coordinates the transaction from outside the organizations' internal systems.

The organizations keep control of their own runtimes.

This option matches the current FEIN architecture and keeps the system boundary clear.

### Centralized Clearinghouse — Rejected

A central system could manage all the software and transaction details.

We did not choose this option because organizations would have less control over their own systems.

### Integrated Discovery and Payment — Rejected

FEIN could include its own discovery system and payment system.

This option was not selected because the current FEIN scope keeps discovery, negotiation, and actual money movement outside FEIN.

## Consequences

### Trade-offs

- Organizations must connect their internal systems to FEIN.
- The TCB must work with information received from systems used before FEIN.

### Risks

- FEIN depends on the selected verifier to perform verification correctly.
- FEIN depends on external payment rails for actual money movement.
- It is not yet confirmed that FEIN and the external payment system can complete the transaction together without any mismatch.

### Follow-up Work

- Define the final TCB format and commitment rules.
- Define how FEIN connects to external payment rails.
- Define verifier selection and dispute handling.
- Define the remaining settlement failure and recovery rules.



