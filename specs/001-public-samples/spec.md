# Feature Specification: Initial Public Samples

**Feature Branch**: Not created; specification only.

**Created**: 2026-11-13

**Status**: Draft

**Input**: User description: "create couple of samples"
The selected samples are a vocabulary quiz and a simple interest calculation.

This feature authors reference programs, synthetic data, screen descriptions, and expected results.
The interactions below define the behavior those programs must have when executed by a compatible runtime.
Implementing that runtime is a separate feature; publishing these samples must not imply that CsFox can already execute them.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Follow a small vocabulary quiz (Priority: P1)

A sample reader can follow a short quiz that reads vocabulary items from a table, asks for answers on a text screen, and reports a score.
The example is small enough to understand without navigating a larger application.

**Why this priority**: It provides the first end-to-end target for program flow, table traversal, text output, and keyboard input.

**Independent Test**: Review the quiz source, seed records, and reference interaction cases without depending on the interest sample.
Check that the source and expected transcript agree; execute the same cases against CsFox when the required runtime support becomes available.

**Acceptance Scenarios**:

1. **Given** five vocabulary items and their expected answers, **When** all five correct answers are submitted, **Then** the reference case shows five questions, correct feedback, and a final score of five out of five.
2. **Given** the same items, **When** an answer is incorrect or blank, **Then** that submitted answer counts as incorrect, the expected answer is shown, and the quiz advances.
3. **Given** a correct answer with different letter case or surrounding spaces, **When** it is submitted, **Then** it is accepted as correct.
4. **Given** an unfinished quiz, **When** the user chooses the documented quit action, **Then** it ends with a score for submitted answers only and leaves the input data unchanged.

---

### User Story 2 - Calculate interest for a selected balance (Priority: P2)

A sample reader can follow a program that finds a fictional balance by its identifier, accepts a date range, and displays simple interest.
The output makes the selected balance, rate, elapsed days, and result visible.

**Why this priority**: It adds a concrete target for indexed lookup, dates, numeric expressions, validation, and rounding.

**Independent Test**: Review this sample's source, records, index expectations, and calculation cases independently of the quiz.
Recalculate the expected results without relying on CsFox or an original FoxPro installation.

**Acceptance Scenarios**:

1. **Given** a balance of 1000.00 at an annual rate of 3.65%, **When** the selected period is 2026-01-01 through 2026-01-11, **Then** the result shows 10 elapsed days and interest of 1.00.
2. **Given** the same balance and rate, **When** the period is 2024-02-28 through 2024-03-01, **Then** the result shows two elapsed days and interest of 0.20.
3. **Given** an identifier absent from the sample data, **When** it is entered, **Then** the program reports that no balance was found and allows another lookup or exit.
4. **Given** an invalid date or an end date before the start date, **When** calculation is requested, **Then** the program reports the problem and allows correction without showing a calculated result.

---

### User Story 3 - Recreate and inspect the sample fixtures (Priority: P3)

A contributor can understand the binary fixtures from human-readable definitions, recreate them, and verify their declared records and index behavior.
They can distinguish authored expectations from behavior actually observed in a runtime.

**Why this priority**: The samples must be auditable compatibility targets rather than opaque files whose intended behavior is unknown.

**Independent Test**: Follow each sample's fixture instructions and compare recreated records and lookup results with its declared expectations.
This does not require either sample to execute in CsFox.

**Acceptance Scenarios**:

1. **Given** a sample package, **When** a contributor inspects its documentation, **Then** every data file, field, record, index key, input sequence, and expected result is explained.
2. **Given** the human-readable fixture definitions and documented prerequisites, **When** the fixtures are recreated in a working directory, **Then** they contain the declared data and index mappings without changing committed inputs.
3. **Given** that no compatible runtime has yet executed a case, **When** the contributor reads its status, **Then** it is labeled as an expected result rather than verified compatibility.

### Edge Cases

- An empty vocabulary table produces a clear no-questions outcome, not a division-by-zero error.
- Quitting before submitting an answer reports zero answers submitted and does not count the unfinished question.
- Unknown lookup keys do not select an arbitrary record.
- Equal start and end dates, a zero balance, or a zero rate produce zero interest.
- Leap-day spans use the actual elapsed day count.
- A half-cent result follows the stated rounding rule; for example, 50.00 at 3.65% for one day produces 0.01.
- Negative balances or rates, missing files, and inconsistent index/data fixtures produce explicit diagnostics rather than plausible results.
- Expected results do not depend on host locale, the current date, or random ordering.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The feature MUST provide exactly two independently authored public samples: a vocabulary quiz and a simple interest calculation.
  Both MUST use synthetic data and redistribution-safe assets.
- **FR-002**: Each sample MUST include readable FoxPro PRG source, a populated DBF fixture, a documented text-screen layout and input flow, and expected interaction results.
- **FR-003**: The quiz MUST ask five fixture-defined questions in a documented deterministic order, accept one submitted answer per question, show feedback, and report a final or partial score.
  Answer comparison MUST ignore letter case and surrounding spaces.
- **FR-004**: The interest sample MUST use a supplied index to select a balance by its identifier, independently of physical record order.
  Its package MUST include a valid index fixture and human-readable expected key-to-record mappings.
- **FR-005**: The interest calculation MUST use non-compounding simple interest: principal multiplied by annual rate and actual elapsed days, divided by 365.
  The annual rate is a fraction for calculation; 3.65% means 0.0365.
  Elapsed days are the end date minus the start date, with zero days when the dates are equal.
  Round only the final interest to two decimal places, with a half-cent rounded upward for the supported nonnegative values.
- **FR-006**: Each sample MUST define explicit outcomes for its listed edge cases and provide a documented quit action.
- **FR-007**: Each package MUST describe its intended DOS FoxPro dialect, data and index formats, encoding, input conventions, and required language and runtime capabilities.
  These choices MUST be concrete before the sample implementation is accepted.
- **FR-008**: Each package MUST include human-readable fixture definitions and a reproducible procedure to create and inspect its fixtures without requiring an original FoxPro installation.
  Recreation MUST preserve declared records and index mappings; byte-for-byte identical incidental metadata is not required.
- **FR-009**: Sample execution and fixture preparation MUST leave committed inputs unchanged.
  Any writable working files MUST be separate disposable copies.
- **FR-010**: Expected results MUST be deterministic and suitable for later reuse as compatibility cases on Windows, macOS, and Linux.
  Execution evidence MUST name the runtime and platform actually used, without treating unexecuted cases as verified.
- **FR-011**: The feature MUST remain limited to sample authoring and fixture preparation.
  It MUST NOT implement a general database engine, interpreter, terminal framework, or screen designer merely to make the samples run.

### Key Entities *(include if feature involves data)*

- **Sample package**: Source program, fixture files and definitions, screen/input description, required capabilities, reference cases, and validation status.
- **Vocabulary item**: A prompt, expected answer, and position in the deterministic quiz sequence.
- **Balance record**: A unique identifier, nonnegative principal, and nonnegative annual interest rate.
- **Calculation case**: A selected balance, start and end dates, elapsed days, and expected rounded interest.
- **Reference interaction**: Ordered user inputs, expected visible output, termination behavior, and whether a runtime has actually verified the case.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Two complete sample packages are present, each with source, populated data, screen/input documentation, reference cases, and fixture instructions.
- **SC-002**: The quiz's documented cases cover all-correct completion, an incorrect answer, a blank answer, case/space normalization, an empty table, and early exit.
- **SC-003**: The interest cases cover normal and unknown-key lookup, ordinary and leap-day periods, zero results, final rounding, and invalid input.
  The ordinary ten-day case yields 1.00, the stated leap-day case yields 0.20, and the stated half-cent case yields 0.01.
- **SC-004**: A contributor can reconstruct every declared record and index mapping from the included definitions and verify them without CsFox execution.
- **SC-005**: All reference cases have explicit expected outcomes and an honest execution status; no unrun case is labeled as passed.
- **SC-006**: Fixture preparation leaves zero changes to committed inputs, and the documented sample programs do not intentionally update their seed data.

## Assumptions

- The user selected a vocabulary quiz and a simple interest calculation as the first two samples.
- Initial fixtures use ASCII-only text with an explicitly declared encoding to keep the first compatibility target bounded.
  Broader character sets and code-page behavior are later work, not claimed support.
- These are small, single-user, read-only examples.
  Persisted scores, balance updates, compounding, fees, concurrency, and financial-product rules are outside scope.
- Text screens are defined by the programs and documented layouts; importing screen-designer formats or native libraries is outside this feature.
- Date input and displayed numeric conventions will be explicit and consistent across hosts rather than inherited from the host locale.
- The [repository structure](../../README.md#project-structure) supplies the sample location and contributor workflow.
  Sample contracts and fixture definitions can be reviewed before runtime support exists.
- Reference-runtime access is optional evidence, not an assumed prerequisite.
  Without it, format and semantic claims must state their documentary or independently calculated basis.
