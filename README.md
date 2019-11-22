# Compound tasks are ignoring `endsPattern` of dependent tasks

## Setup
`task1` and `task2` are both background tasks that simulate a watcher. They both exit after approximately 10 seconds.

`task1` has a matching `endsPattern`; whereas `task2` has a non-matching `endsPattern`.

`Launch w/task1` and `Launch w/task2` are both launch confiruations, each configured with its respective `preLaunchTask`.

## Observed behavior when debugging the launch configurations
As expected, when debugging `Launch w/task1`, the application is launched as soon as `task1`'s `endsPattern` is matched.

As expected, when debugging `Launch w/task2`, the application is launched only when`task2` exits (since it has no match for its `endsPattern`).

## Observed behavior when running the compound tasks
When running `compoundtask1`, it doesn't start untils its dependent `task1` exits. This is *unexpected*, since its `endsPattern` matches 10 seconds before the task exits.

When running `compoundtask2`, it doesn't start untils its dependent `task2` exits. This is *expected*, since its `endsPattern` does not match.

## Why it matters
Besides the inconsistent behavior, the observed behavior makes it impossible to debug a launch configuration that is dependent on *2 or more background tasks*.

The reason is that the only way to do this is to define a launch configuration with a `preLaunchTask` that references a compound task.

However, as demonstrated above, the compound task runs only when its dependent tasks exit, which means that the launch configuration never starts.
