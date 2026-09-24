# PP-SPEC-032: The Non-Blocking Requirement and the Valid-Proof Conditions

- Status: DRAFT (for review; not yet published)
- License: CC BY-ND 4.0
- DOI: [assign on publication via Zenodo]
- Supersedes: none
- Builds on: PP-SPEC-023 (Structural Independence and the Fidelity-at-Capture Requirement), PP-SPEC-024 (The Self-Attestation Oxymoron), PP-SPEC-027 (Proven Efficacy and the Third Axis)
- Date: 2026-09-24 (authored; public release deferred until after the PoC comment period closes 2026-10-30)

## Abstract

This document states, at the level of principle, two requirements a system must meet to produce a proof of efficacy that is both trustworthy and deployable: it must be non-blocking with respect to the tested party's production, and its verdict must satisfy the valid-proof conditions. It distinguishes these two properties, which are frequently conflated. A system can be non-blocking and still fail to be independent. This specification defines the requirements only. It does not specify the mechanism by which they are met.

## 1. Scope

This specification is doctrine. It defines requirements and the distinctions between them. It deliberately does not define topology, data formats, scoring procedures, capture mechanisms, or any other means of implementation. Those are out of scope here and are addressed elsewhere.

The requirements below are stated using the key words MUST, MUST NOT, SHOULD, and MAY as described in RFC 2119.

## 2. Definitions

- Tested party: the party whose product or control is the subject of a proof.
- Verifier: the party that runs a proof and renders its verdict.
- Sealer: the party that seals the verdict into a durable, verifiable record.
- Blocking (inline) verification: a design in which verification sits in the tested party's production path and can halt, delay, or gate production until verification completes.
- Non-blocking (out-of-band) verification: a design in which verification does not sit in the production path and cannot halt or gate production. Verification runs alongside production.
- Valid proof: a verdict that satisfies the conditions in Section 4.
- Independent sealer: a sealer that is neither the tested party nor a party the tested party controls.
- Pre-forensic capture: capture of the evidence of what occurred at the moment of occurrence, faithfully, before the evidence can be reshaped by the tested party.
- Fidelity-at-capture: the property that the evidence entering a proof faithfully represents what the executor actually did. Distinct from consistency-of-data, which concerns whether a record remains internally consistent after capture.
- Self-attestation: the condition in which the party being measured determines, in whole or in part, the outcome of its own measurement.

## 3. The non-blocking requirement

A proof system MUST NOT sit in the tested party's production path as a precondition of production. Production MUST proceed independently of the state of verification. If verification is delayed, degraded, or unavailable, production MUST continue unaffected.

Rationale: a control that halts production until its own verification catches up trades the tested party's availability for the verifier's bookkeeping. This is not acceptable to operating organizations and is not required in order to produce a valid proof. Out-of-band verification carries no such cost, because it was never a gate.

This requirement concerns availability only. It says nothing about whether the resulting verdict is trustworthy. See Section 5.

## 4. The valid-proof conditions

A verdict is a valid proof only if all of the following hold. If any one fails, the result MUST be rendered as an explicit invalid result, never as a passing verdict and never as a silent omission.

1. Independent sealer. The verdict MUST be sealed by an independent sealer. The tested party MUST NOT seal, or control the sealing of, a verdict about itself.
2. Pre-forensic capture. The evidence underlying the verdict MUST be captured faithfully at the moment of execution. The tested party MUST NOT be positioned to reshape that evidence before it is sealed.
3. Independent determination. The determination of the outcome MUST be made independently of the tested party. The tested party MAY define the surface of what is tested. The tested party MUST NOT define, in whole or in part, the criteria by which the outcome is judged.

Condition 3 admits a legitimate role for the tested party: it may say what its product is and what interface it exposes, so that a proof can reach it and so that the tested party will consent to be tested. It may not say what counts as passing.

## 5. The availability-independence distinction

Non-blocking (Section 3) and valid (Section 4) are independent properties. Conflating them is an error this specification exists in part to prevent.

- A system MAY be non-blocking and still fail every valid-proof condition. A verification that runs out-of-band but lets the tested party determine its own outcome is non-blocking and self-attesting.
- A system MAY be valid and blocking. Independence of the verdict says nothing about where verification sits relative to production.

Therefore a claim of non-blocking operation MUST NOT be presented, or reasonably understood, as a claim of independence. Availability and independence are separate axes and MUST be claimed separately.

## 6. The required pairing

A conformant proof system MUST be non-blocking on the tested party's production (Section 3) and MUST be fail-closed on the verdict (Section 4). Both. Neither alone is sufficient.

- Non-blocking without a valid verdict yields availability at the cost of trust.
- A valid verdict that blocks production yields trust at the cost of availability.

The pairing yields both: production is never held hostage to verification, and the verdict is never rendered when its conditions are not met.

## 7. Contrast with blocking and self-attesting patterns

Two design patterns fail the requirements above and are named here as patterns, not as organizations.

- The blocking pattern places verification inline and halts production until integrity catches up. It fails Section 3.
- The self-attesting pattern permits the tested party to determine its own outcome, whether by sealing its own record, by capturing its own evidence without independent fidelity, or by authoring the criteria of judgment. It fails Section 4. Cryptographic integrity of a self-generated record does not cure this: integrity proves a record was not altered after capture; it does not prove the record faithfully represented what occurred, nor that an independent party sealed it. See PP-SPEC-023.

A system may exhibit both patterns at once. Curing one does not cure the other.

## 8. Conformance

A system conforms to this specification if and only if:

1. It meets the non-blocking requirement of Section 3.
2. Every verdict it renders meets all valid-proof conditions of Section 4, or is rendered as an explicit invalid result.
3. It does not present non-blocking operation as independence (Section 5).
4. It satisfies the required pairing of Section 6.

Conformance is a property of the system's requirements, not of any particular implementation of them.

## 9. What this specification does not define

This specification does not define, and conformance does not require any particular choice of:

- deployment topology or the placement of components;
- evidence formats or the structure of captured signals;
- the procedure by which an outcome is determined from evidence;
- the capture mechanism by which fidelity-at-capture is achieved;
- any scoring formula or verdict-rendering algorithm.

These are matters of implementation and are addressed separately. Their omission here is deliberate.

## License

This work is licensed under the Creative Commons Attribution-NoDerivatives 4.0 International License (CC BY-ND 4.0). You may copy and redistribute this document in any medium or format for any purpose, provided you give appropriate credit and do not distribute modified material.
