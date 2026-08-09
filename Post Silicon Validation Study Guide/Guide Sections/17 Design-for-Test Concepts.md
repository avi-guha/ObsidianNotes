# 17. Design-for-Test Concepts

[[Post Silicon Validation Study Guide|Back to study guide index]] | [[Guide Sections/16 Failure Analysis and Field Returns|Previous: 16. Failure Analysis and Field Returns]] | [[Guide Sections/18 AI-Assisted Validation|Next: 18. AI-Assisted Validation]]

## Scan chains

In test mode, flip-flops may be connected into shift registers.

This allows internal states to be:

- Controlled.
- Shifted in.
- Captured.
- Shifted out.
- Observed externally.

## ATPG

Automatic Test Pattern Generation creates patterns to detect modeled faults.

## Stuck-at fault

A signal behaves as though permanently fixed at zero or one.

## Transition fault

A node cannot transition quickly enough from:

- Zero to one.
- One to zero.

## MBIST

Memory Built-In Self-Test tests on-chip memories using hardware-generated patterns.

It may detect:

- Stuck bits.
- Coupling faults.
- Address faults.
- Transition faults.

## BISR

Built-In Self-Repair may replace defective memory rows or columns with redundant resources.

## ATE

Automated Test Equipment tests devices during manufacturing.

Production-test priorities:

- High fault coverage.
- Short test time.
- Reliable classification.
- Repeatability.
- Low cost per unit.

## Boundary scan

Boundary scan can test connections between chips on a PCB without requiring normal firmware execution.

## DFT terms in more detail

- **DFT:** Design for Test. Hardware features intentionally added so manufacturing and validation teams can test internal logic, memories, and board connections.
- **Scan chain:** A chain of flip-flops connected so internal state can be shifted in and out during test mode. This improves controllability and observability of logic that is otherwise hidden.
- **ATPG:** Automatic Test Pattern Generation. A tool creates input patterns that should detect modeled faults, such as stuck-at or transition faults.
- **Fault coverage:** The percentage of modeled faults that the test patterns can detect. Higher coverage reduces the chance of shipping defective parts.
- **Stuck-at fault:** A modeled defect where a signal behaves as if permanently tied to 0 or 1.
- **Transition fault:** A modeled timing-related defect where a node can switch but not quickly enough.
- **MBIST:** Memory Built-In Self-Test. On-chip hardware tests embedded memories without requiring external testers to directly access every memory bit.
- **BISR:** Built-In Self-Repair. Redundant rows or columns can replace defective memory locations, often using fuse or repair data.
- **ATE:** Automated Test Equipment. Production machines that apply patterns, measure responses, and classify devices quickly and repeatably.

## Why validation engineers care

DFT features are not only for factories. During bring-up and failure analysis, scan, MBIST, boundary scan, device IDs, repair status, and test modes can reveal whether a problem is in logic, memory, packaging, board assembly, or normal firmware configuration.

---
