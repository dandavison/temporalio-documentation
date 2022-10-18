---
id: signal
title: tctl workflow signal
sidebar_label: signal
description: How to Signal a Workflow Execution using tctl.
tags:
  - tctl
---

The `tctl workflow signal` command [Signals](/concepts/what-is-a-signal) a [Workflow Execution](/concepts/what-is-a-workflow-execution).

Workflows listen for Signals by their Signal name, and can be made to listen to one or more Signal names.
The Workflow below listens for instances of "HelloSignal":

```bash
tctl workflow start  --workflow-id "HelloSignal" --taskqueue HelloWorldTaskQueue --workflow-type HelloWorld --execution-timeout 3600 --input \"World\"
```

The Worker would return this output upon receiving the Signal:

```text
13:57:44.258 [workflow-method] INFO  c.t.s.javaquickstart.GettingStarted - 1: Hello World!
```

Signals can also be used to change variable values.

```bash
tctl workflow signal --workflow-id "HelloSignal" --name "updateGreeting" --input \"Hi\"
```

The output would change from the first Signal received.

```text
13:57:44.258 [workflow-method] INFO  c.t.s.javaquickstart.GettingStarted - 1: Hello World!
13:58:22.352 [workflow-method] INFO  c.t.s.javaquickstart.GettingStarted - 2: Hi World!
```

When a Signal is sent, an await condition is made to block any Signals that contain the same input value.
However, changing the greeting in our example unblocks it:

```bash
tctl workflow signal --workflow-id "HelloSignal" --name "updateGreeting" --input \"Welcome\"
```

Worker output:

```text
13:57:44.258 [workflow-method] INFO  c.t.s.javaquickstart.GettingStarted - 1: Hello World!
13:58:22.352 [workflow-method] INFO  c.t.s.javaquickstart.GettingStarted - 2: Hi World!
13:59:29.097 [workflow-method] INFO  c.t.s.javaquickstart.GettingStarted - 3: Welcome World!
```

Sending Signals does not require a running Worker.

```bash
tctl workflow signal --workflow-id "HelloSignal" --name "updateGreeting" --input \"Welcome\"
```

CLI output:

```text
Signal workflow succeeded.
```

The Signal request is queued inside the Temporal Server until the Worker is restarted.
If the given Signal contains the same input as before, the queued Signal will be ignored.

Complete the Workflow by sending a Signal with a "Bye" greeting:

```bash
tctl workflow signal --workflow-id "HelloSignal" --name "updateGreeting" --input \"Bye\"
```

Check that the Workflow Execution has been completed.

```bash
tctl workflow showid HelloSignal
```

## Modifiers

Signals are written as follows:

```bash
tctl workflow signal --workflow-id [modifiers]
```

The following modifiers control the behavior of the command.
Make sure to include required modifiers in all command executions.

- [--input](/tctl-next/modifiers#--input)
- [--input-file](/tctl-next/modifiers#--input-file)
- [--name](/tctl-next/modifiers#--name)
- [--run-id](/tctl-next/modifiers#--run-id)
- [--workflow-id](/tctl-next/modifiers#--workflow-id)