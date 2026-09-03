---
title: "Approval Fatigue: When the Human in the Loop Stops Looking"
date: 2026-09-02 09:00:00 -0400
categories: [blog]
tags: [human in the loop, ai governance, workflow design]
excerpt: "A human approval step only controls risk while the human is actually reviewing. Most enterprise AI workflows are designed as if attention were free, and within weeks the reviewer is approving on reflex."
header:
  overlay_color: "#08142C"
  overlay_filter: "0.18"
  overlay_image: /assets/images/revamp/hero-ai.svg
---

Human-in-the-loop is the control most enterprise AI programs reach for first, and for good reason. A reviewer between the model and the consequence turns an unpredictable system into an acceptable one. The problem is what happens after launch. The model is right most of the time, the queue keeps filling, and the reviewer learns within a few weeks that clicking approve is almost always the correct answer. From that point the control exists on the architecture diagram and nowhere else.

This is not a discipline failure. It is the predictable response of a person asked to evaluate hundreds of near-identical items where the base rate of problems is low. Aviation, radiology, and security operations have all documented the same decay: vigilance drops sharply as the true positive rate falls, and the reviewer's real accuracy converges on the automation's. The output is a workflow that carries the cost of a human step and the risk profile of an unattended one, while the audit trail records that every item was reviewed.

The financial exposure is real. If a copilot drafts credit adjustments, contract clauses, or supplier payments and a reviewer approves them on reflex, the organization has not reduced its liability. It has attached a human signature to the model's errors, which is arguably worse than having none. Regulators and auditors increasingly ask not whether a review step exists but whether it is effective, and a ninety-nine percent approval rate with a median review time of four seconds is not a defensible answer.

The fix starts with treating reviewer attention as a scarce resource to be allocated rather than a free control to be sprinkled everywhere. Route the low-risk, high-confidence majority straight through with sampling-based monitoring, and reserve human review for the items where the model's confidence is low, the stakes are high, or the input falls outside the distribution the system was validated on. A reviewer who sees thirty hard cases a day stays sharp. One who sees six hundred easy ones does not.

Then instrument the review step itself. Time spent per item, approval rate by reviewer, override frequency, and agreement with a periodic blind re-review are all cheap to capture and tell you whether the control is still alive. Seeded test cases, where a known-bad item is deliberately placed in the queue, give you a direct measurement of catch rate rather than an assumption. When these metrics drift, the workflow needs redesign, not a reminder email asking people to pay closer attention.

A human approval step is a claim about risk, and claims need evidence. If your AI workflows include reviewers, the question worth asking this quarter is not whether they are in the loop but whether you can prove they are still looking. Umplify helps enterprise teams design and instrument AI workflows that hold up under audit; book a free discovery call to talk through yours.
