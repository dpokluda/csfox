# Sample specification and runtime direction

The initial solution and projects are in place; see the [repository structure](../README.md#project-structure).
The [public samples specification](001-public-samples/spec.md) is ready for implementation planning.
It covers an independently authored vocabulary quiz and simple interest calculation, with synthetic fixtures and expected results.

Use the sample contracts to decide which runtime behavior to implement rather than design every possible FoxPro subsystem in advance.
The candidate work below has not yet been turned into feature specifications or implementation tasks.

## Do we need a database engine?

We need database behavior, but not a general-purpose database system or a separate database server.
FoxPro programs work with tables, a current record, field values, and indexes.
A small data compatibility component must eventually reproduce the operations used by the samples.
That is database-engine functionality in a limited sense, not a reason to build a complete engine first.

For the quiz, start with the declared DBF format, field values, sequential record navigation, and end-of-table behavior.
For the interest sample, add the declared index format and indexed lookup behavior.
The supplied index must be meaningful; silently ignoring it or substituting an unrelated lookup is not evidence of index compatibility.
Keep the first sample targets read-only.
Writing records, maintaining indexes after updates, locking, transactions, and wider format support can wait for examples that need them.

Planning should assess existing libraries before deciding what to implement ourselves.
A dependency must preserve the required file and program semantics and work on Windows, macOS, and Linux.
Replacing DBF data with a different storage system is not automatically equivalent to supporting the original programs.
No library or storage-backend choice is made here.

## Do we need an interpreter?

Yes: executing existing source programs requires an implementation of their language behavior.
For this experiment, a small interpreter is the intended starting point, not a compiler, bytecode format, or virtual-machine framework.
It needs only the expressions, values, variables, control flow, commands, and functions required by the chosen examples.
Unsupported constructs must produce useful diagnostics rather than be skipped.

Keep language execution, file access, and terminal interaction understandable as separate responsibilities.
That does not require separate projects, plugin systems, or interfaces for every operation.

## Proposed runtime work after sample authoring

1. **Execute a minimal program.** Load a short PRG, evaluate a small set of expressions and variables, follow basic control flow, and produce text output.
   Include source-location diagnostics for unsupported or invalid input.
   No database access is needed for this first executable milestone.
2. **Run the vocabulary quiz end to end.** Add only the table reading, record navigation, text-screen output, and keyboard input its cases require.
   A correct quiz session matters more than broad but disconnected language or database coverage.
3. **Run the interest sample end to end.** Add the required indexed lookup, date behavior, numeric calculations, and explicit rounding.
   Use the independently calculated reference cases to expose semantic differences.
4. **Expand from additional programs.** Add writes, more file variants, richer screens, character sets, procedure behavior, or other capabilities when a concrete example needs them.

Every runnable milestone must account for all three target operating systems.
Keep deterministic language/data checks separate from the host-specific checks needed for files, keyboard input, and terminal behavior.
Do not treat one platform's results as proof that the other platforms work.

## Decisions for the next planning steps

The foundation is one .NET 10 console project and one MSTest project in a single solution.
Keep engine and host code together until a concrete need justifies a separate library.
The sample plan will choose the specific DOS FoxPro dialect, DBF/index variants, field layouts, source statements, input conventions, and fixture-generation method.
Those choices will give the later runtime specifications a bounded compatibility target.

Do not start with a general query engine, distributed database, graphical screen designer, native-extension loader, or cross-language VM.
Get the small source programs working first, then let new requirements justify further machinery.
