---
title: "Who Authorizes the Agent? Identity and Access for Autonomous AI"
date: 2026-06-02 09:00:00 -0400
categories: [blog]
tags: [ai-agents, identity, azure]
excerpt: "As enterprises move from AI chatbots to agents that take real actions, the hardest question is no longer what the agent can say, but who authorized what it does."
header:
  overlay_color: "#08142C"
  overlay_filter: "0.18"
  overlay_image: /assets/images/revamp/hero-ai.svg
---

The shift from chatbots to agents changes the security question entirely. A chatbot reads and answers. An agent acts. It calls APIs, edits records, files tickets, and sometimes moves money. The moment an AI system can take an action, the enterprise has to answer a question most teams have quietly skipped: who authorized this, and under whose authority did it run?

In practice, most teams hand the agent a single service credential with broad scope because it is the fastest path to a working demo. That convenience collapses every user's intent into one identity. When the agent issues a refund or updates a customer record, the audit log shows the agent, not the person who asked for it. That gap is harmless in a prototype and genuinely dangerous in production, because both least privilege and accountability disappear at the same time.

A better model treats the agent as a delegate rather than a principal. In Azure terms, that means carrying the calling user's identity through the agent using on-behalf-of flows in Microsoft Entra, so downstream calls run with the permissions of the actual person instead of a broadly scoped service account. The agent becomes a constrained actor that can do only what the requesting user was already allowed to do, no more.

Delegation alone does not cover autonomous steps. When an agent plans a multi-step task, each tool call should pass through an authorization checkpoint that understands the action, the resource, and the risk. Reading a calendar is low risk. Deleting a customer or approving a payment is not. The sensible pattern is to treat high-risk actions as requiring either explicit human approval or a scoped, time-limited token issued for that single step and nothing more.

This is where engineering discipline shows up. You need a policy layer that sits between the agent's intent and the systems it touches, an audit trail that records the human, the agent, the action, and the justification together, and managed identities so no long-lived secret ever lives inside the agent runtime. Azure gives you the primitives, including Entra, managed identities, and conditional access, but the wiring between them is yours to design and own.

The companies that succeed with agents will not be the ones with the cleverest prompts. They will be the ones who can answer, for every action an agent takes, exactly who allowed it and why. Build that answer into your architecture now, before an over-privileged agent makes the decision for you.
