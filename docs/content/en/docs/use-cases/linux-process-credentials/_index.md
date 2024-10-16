---
title: "Linux process credentials"
weight: 4
icon: "overview"
description: "Monitor Linux process credentials"
---

On Linux each process has various associated user, group IDs, capabilities,
secure management flags, keyring, LSM security that are used part of the
security checks upon acting on other objects. These are called the task
privileges or
[process credentials](https://www.kernel.org/doc/html/latest/security/credentials.html#task-credentials).

Changing the process credentials is a standard operation to perform privileged
actions or to execute commands as another user. The obvious example is
[sudo](https://www.sudo.ws/) that allows to gain high privileges and run commands
as root or another user. An other example is services or containers that can
gain high privileges during execution to perform restricted operations.

Process credentials monitoring is a good practice to identify programs
running with high privileges. Tetragon is able to observe process credentials at
different layers of the operating system. Each layer will generate a number of events,
where the upper layer that is the user space layer may generate a low number
of events, where the lower kernel layers could generate a high number of events.

Depending on the use case, users should chose the right layer at which they want to
monitor process credentials and apply the corresponding
[Tracing Policies]({{< ref "/docs/concepts/tracing-policy" >}}).

## Composition of Linux process credentials

### Traditional UNIX credentials

- Real User ID
- Real Group ID
- Effective, Saved and FS User ID
- Effective, Saved and FS Group ID
- Supplementary groups

### Linux Capabilities

- Set of permitted capabilities: a limiting superset for the effective
  capabilities.
- Set of inheritable capabilities: the set that may get passed across
  `execve(2)`.
- Set of effective capabilities: the set of capabilities a task is actually
  allowed to make use of itself.
- Set of bounding capabilities: limits the capabilities that may be inherited
  across `execve(2)`, especially when a binary is executed that will execute as
  UID 0.

### Secure management flags (securebits).

These govern the way the UIDs/GIDs and capabilities are manipulated and
inherited over certain operations such as `execve(2)`.

### Linux Security Module (LSM)

The [LSM framework](https://www.kernel.org/doc/html/latest/admin-guide/LSM/index.html)
provides a mechanism for various security checks to be hooked by new kernel
extensions. Tasks can have extra controls part of LSM on what operations they
are allowed to perform.



