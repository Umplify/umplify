---
title: "The AI Bill of Materials: A New Line Item for Enterprise Procurement"
date: 2026-07-29 09:00:00 -0400
categories: [blog]
tags: [ai-governance, procurement, vendor-risk]
excerpt: "Enterprises already demand a software bill of materials from vendors to manage supply chain risk. AI vendors need the same discipline, and most cannot answer the questions yet."
header:
  overlay_color: "#08142C"
  overlay_filter: "0.18"
  overlay_image: /assets/images/revamp/hero-ai.svg
---

Ask a security team to approve a new software vendor and they will ask for a software bill of materials: what open source libraries are in the product, what versions, what known vulnerabilities. That practice took a decade to become standard, forced along by a string of supply chain incidents. AI vendors are now in the position open source vendors were in fifteen years ago. Enterprises buy an AI feature, embed it in a customer workflow, and cannot answer basic questions about what is actually running underneath it: which model, which version, trained on what, retrained how often, and what happens to the data sent to it. That gap is starting to matter at contract time, not just at audit time.

An AI bill of materials extends the same idea that made software bills of materials useful. It documents which foundation models power a product, whether they are first party or a wrapped third party API, what fine tuning or retrieval layers sit on top, what data was used to adapt the model, and where inference actually runs. It should also cover retention: does the vendor use customer prompts and outputs to train future models, and can that be turned off contractually rather than just in a settings toggle. None of this is exotic information. It is simply not standardized yet, so every enterprise buyer asks for it from scratch and every vendor answers differently, if at all.

The business case for demanding this document is concrete. A vendor swaps the underlying model without notice, and the behaviour your team validated during procurement quietly changes in production. A model gets deprecated and the vendor has no migration plan, leaving you exposed on a timeline you do not control. A regulator asks how a customer facing decision was made and the vendor cannot trace which model version produced it. Each of these has already happened to companies that assumed a stable AI product behind a stable API. An AI bill of materials does not prevent the underlying change, but it converts an invisible risk into a documented, trackable one that legal and procurement can price into the contract.

This is also where AI governance intersects with technical due diligence and vendor risk work more broadly. Acquirers evaluating a target's AI stack, insurers underwriting AI liability, and security teams reviewing a new SaaS purchase are all asking a version of the same question: what exactly are we relying on, and how would we know if it changed. Treating the AI bill of materials as a standing document rather than a one time procurement artifact means it stays current as the vendor's stack evolves, and becomes the reference point when something breaks.

Building the internal version of this practice does not require an industry standard to arrive first. Require it as a checklist item in vendor security reviews: model identity and version, training and fine tuning data provenance, retention and training opt out terms, inference location, and a defined process for material model changes. Apply the same discipline to any AI feature your own team ships, since your customers will eventually ask you the same questions you are now asking your vendors.

The enterprises that formalize this now will spend less time firefighting silent vendor changes later, and will negotiate from a position of specificity instead of trust. [Book a free discovery call](/contact/) if you want help building an AI vendor review process that holds up under real scrutiny.
