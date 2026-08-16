---
title: "Azure Container Apps Dynamic Sessions: The Sandbox Enterprise AI Agents Need"
date: 2026-08-14 09:00:00 -0400
categories: [blog]
tags: [azure, ai, platform-engineering]
excerpt: "The moment an AI agent writes code, your architecture inherits an execution problem. Azure Container Apps dynamic sessions turn that problem into a managed, per-request isolation boundary."
header:
  overlay_color: "#08142C"
  overlay_filter: "0.18"
  overlay_image: /assets/images/revamp/hero-cloud.svg
---

There is a quiet moment in most enterprise AI projects where the architecture changes shape. The agent stops summarising documents and starts writing code. It generates a Python snippet to reconcile two financial extracts, or a query to reshape a customer dataset, and something has to run that code. Teams usually discover this requirement late, after the model work is done and the demo has already impressed someone senior. The question that follows is uncomfortable: where does untrusted, machine generated code execute inside your tenant?

The wrong answers are common. Some teams execute agent output inside the application process, which means a single prompt injection can reach connection strings, managed identity tokens, and every downstream system the app can talk to. Others spin up a long lived container pool and reuse it across requests, which quietly turns state left behind by one customer into an input for the next. Both patterns pass a demo and fail a security review.

Azure Container Apps dynamic sessions exist for exactly this gap. Each session is a Hyper-V isolated sandbox, allocated from a warm pool in well under a second, with its own filesystem and network boundary. You call an endpoint, pass code, get output, and the session is destroyed. There is no cluster to size, no node pool to patch, and no shared runtime accumulating residue between invocations. The code interpreter flavour ships with a Python runtime and common data libraries preinstalled; custom container sessions let you bring your own image when the agent needs a specific toolchain.

The business value is not the sandbox itself. It is that isolation stops being a design debate and becomes a platform default. When every code execution is disposable and identity scoped, you can let agents do genuinely useful work, data reconciliation, ad hoc analysis, file transformation, without expanding your blast radius each time. Your security team gets a boundary they can reason about, and your engineers stop writing bespoke containment logic that will drift out of date within two releases.

The design decisions that matter are the ones around the sandbox. Sessions are ephemeral, so anything the agent needs must be passed in or mounted deliberately, which forces a healthy conversation about what data an agent is actually entitled to see. Session identifiers should map to a user or tenant so that pooling never crosses a trust line. And execution results still need evaluation, because isolated code can be wrong just as easily as it can be dangerous.

If your roadmap includes agents that act rather than answer, the execution layer is not a detail to solve later. Decide where generated code runs before you decide which model writes it.
