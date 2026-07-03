---
title: "Chapter 3, Part 3 – Virtualization, Containers, and Cloud"
date: 2026-07-03
description: "Virtualization interfaces, VMM types, virtualization condition, containers, PlanetLab, and cloud services."
tags: [distributed-systems, virtualization, containers, cloud, chapter3]
toc: true
weight: 3
---

## Why virtualization matters

Virtualization is important in distributed systems because it supports:

- portability,
- code migration,
- isolation between components,
- isolation from failing or attacked components,
- flexibility when hardware changes faster than software.

Basic idea:

> Virtualization mimics an interface so software can run as if it had its own machine or environment.

---

## Interfaces that can be virtualized

The slides mention interfaces at different levels.

| Interface | Meaning |
|---|---|
| Instruction Set Architecture (ISA) | Machine instructions available to software. |
| System calls | Operations offered by the OS to programs. |
| Library/API calls | Higher-level programming interface. |

ISA contains:

- **privileged instructions:** should only be executed by the OS,
- **general instructions:** can be executed by normal programs.

---

## Ways of virtualization

| Type | How it works | Example idea |
|---|---|---|
| Process VM | Interpreter/emulator runs a separate instruction set on top of an OS. | Java Virtual Machine style. |
| Native VMM | Virtual machine monitor runs close to hardware with minimal OS support. | Bare-metal hypervisor. |
| Hosted VMM | VMM runs on top of a full OS and delegates work to it. | VirtualBox/VMware Workstation style. |

---

## Sensitive and privileged instructions

Important definitions:

| Instruction type | Meaning |
|---|---|
| Privileged instruction | Causes a trap if executed in user mode. |
| Nonprivileged instruction | Does not trap in user mode. |
| Control-sensitive instruction | Can change machine configuration. |
| Behavior-sensitive instruction | Its behavior depends on execution context. |
| Sensitive instruction | Control-sensitive or behavior-sensitive. |

---

## Condition for virtualization

Necessary condition:

> A VMM can be constructed if all sensitive instructions are a subset of privileged instructions.

Why?

Because if a sensitive instruction is executed by a guest OS, it must trap to the VMM so the VMM can control it.

Problem:

Some architectures have sensitive instructions that do not trap in user mode. Then the VMM cannot safely intercept them.

Solutions:

| Solution | Meaning |
|---|---|
| Emulate all instructions | Interpret every instruction. Correct but slow. |
| Binary translation/wrapping | Replace problematic instructions with safe VMM calls. |
| Paravirtualization | Modify guest OS so it avoids problematic instructions or uses hypercalls. |

---

## Containers

Containers are lighter than full VMs because they share the host kernel but isolate processes.

Main mechanisms:

| Mechanism | Meaning |
|---|---|
| Namespaces | Give processes their own view of identifiers such as process IDs, network, filesystem. |
| Union file system | Combine file-system layers; only top layer is writable. |
| Control groups | Limit resources such as CPU, memory, and I/O. |

Exam contrast:

| VM | Container |
|---|---|
| Virtualizes hardware/OS environment. | Isolates processes using same kernel. |
| Heavier, stronger isolation. | Lighter, faster startup. |
| Easier to run different OS kernels. | Harder/impossible to run different kernel. |

---

## PlanetLab example

PlanetLab is an example of using virtualization for shared distributed experiments.

Basic situation:

- different organizations contribute machines,
- many distributed applications share them,
- isolation is needed so experiments do not interfere.

Vserver:

- independent protected environment,
- has its own libraries and server versions,
- applications get collections of vservers across machines.

Slice:

- a distributed collection of vservers assigned to an experiment/application.

---

## VMs and cloud computing

Three common cloud service models:

| Model | Provides | User manages |
|---|---|---|
| IaaS | Infrastructure such as VMs, storage, networks. | OS, runtime, apps. |
| PaaS | Platform services such as runtime, databases, frameworks. | Application code/data. |
| SaaS | Complete applications. | Mostly configuration/use. |

In IaaS, the provider rents a VM instead of a physical machine. Multiple customers may share the same physical machine, while the VM provides isolation.

Important limitation:

VMs provide strong functional isolation, but performance isolation may not be perfect because customers still share physical hardware.

---

## Exam check

1. Why is virtualization useful in distributed systems?
2. Compare process VM, native VMM, and hosted VMM.
3. What is the virtualization condition involving sensitive and privileged instructions?
4. What are namespaces, union file systems, and control groups?
5. Compare IaaS, PaaS, and SaaS.
