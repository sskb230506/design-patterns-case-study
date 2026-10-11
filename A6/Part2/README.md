# Part 2: Three Change Scenarios

## Overview
Part 2 introduces three targeted architectural evolutions into the Sarathi ride-sharing application. Each change scenario addresses a specific design smell identified in Part 1 and introduces an appropriate Gang of Four (GoF) design pattern backed by concrete unit tests and verification logs.

---

## Change Scenarios Breakdown

### CH01: Driver Status Lifecycle Management (State Pattern)
* **Assigned Lead:** Dilip
* **Reviewer:** Sai Sameer
* **Problem in Baseline:** Driver statuses are represented as loose strings (`"pending" | "approved" | "rejected"` in `Driver.ts`). Lifecycle transitions and status checks are performed via ad-hoc procedural statements without transition invariant validation.
* **Proposed Evolution:** Implement the **State Pattern** to represent driver lifecycle states (e.g., `PendingState`, `ApprovedState`, `OnDutyState`, `SuspendedState`). Each state enforces valid transitions and defines state-dependent operations.
* **Directory:** `changes/CH01/`

---

### CH02: Ride Cancellation & Side-Effect Coordination (Observer / Pub-Sub Pattern)
* **Assigned Lead:** Leela Shankar
* **Reviewer:** Kumar
* **Problem in Baseline:** When a ride is cancelled in `CancelBooking.ts`, the use case directly triggers socket notifications imperatively. Payment refund handling and audit logging are missing or coupled tightly inside procedural code.
* **Proposed Evolution:** Implement the **Observer / Pub-Sub Pattern** (Domain Events). Cancellation dispatches a `RideCancelledEvent` consumed by independent subscribers (`NotificationSubscriber`, `RefundPaymentSubscriber`, `DriverReallocationSubscriber`).
* **Directory:** `changes/CH02/`

---

### CH03: Multiple Ride Types & Dynamic Pricing (Strategy Pattern)
* **Assigned Lead:** Kumar
* **Reviewer:** Leela Shankar
* **Problem in Baseline:** `FareCalculatorService.ts` relies on hardcoded `if-else` branches switching on `BookingType.ONE_WAY` and `BookingType.ROUND_TRIP` with fixed multipliers (`PER_KM_RATE = 10`, `PER_DAY_RATE = 1000`). Adding new ride types or surge pricing violates the Open/Closed Principle (OCP).
* **Proposed Evolution:** Implement the **Strategy Pattern**. Introduce an `IPricingStrategy` interface with concrete strategies:
  * `StandardRidePricingStrategy`
  * `RoundTripDailyPricingStrategy`
  * `SurgeDynamicPricingStrategy`
  * `LuxuryRidePricingStrategy`
* **Directory:** `changes/CH03/`

---

## Directory Organization
* `changes/CH01/`: Implementation code, tests, and documentation for CH01.
* `changes/CH02/`: Implementation code, tests, and documentation for CH02.
* `changes/CH03/`: Implementation code, tests, and documentation for CH03.
* `code/`: Integrated evolved application code / unified patch diffs.
* `diagrams/`: UML class diagrams, statecharts, and sequence diagrams for all three scenarios.
* `evidence/`: Automated test run logs, terminal outputs, and regression test proofs.
* `llm/`: Prompts, outputs, and critical reviews for LLM-assisted refactorings.
