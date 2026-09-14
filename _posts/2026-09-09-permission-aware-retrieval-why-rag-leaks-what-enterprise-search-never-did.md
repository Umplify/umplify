---
title: "Permission Aware Retrieval: Why RAG Leaks What Enterprise Search Never Did"
date: 2026-09-09 09:00:00 -0400
categories: [blog]
tags: [rag, security, ai-governance]
excerpt: "A retrieval pipeline that ignores document permissions turns every AI assistant into a data exfiltration path. The fix belongs in the index and the query, not in a prompt asking the model to be discreet."
header:
  overlay_color: "#08142C"
  overlay_filter: "0.18"
  overlay_image: /assets/images/revamp/hero-ai.svg
---

Enterprise search solved a hard problem years ago: a user should only see results they are allowed to open. SharePoint, Confluence, and every serious intranet search engine enforce this at query time, trimming results against the caller's identity before a single link is rendered. Retrieval augmented generation quietly discards that discipline. The typical pipeline crawls a document corpus with a privileged service account, chunks everything into a vector index, and answers questions from whatever is semantically closest. The model never sees a permission, so it cannot respect one. The first time an analyst asks about compensation bands and gets a fluent summary of the executive pay file, the problem becomes visible, and by then it has usually been live for months.

The failure is more dangerous than a search leak because the model synthesizes. A search engine that overshares returns a link the user might not click. A language model that overshares returns the sensitive content itself, paraphrased and stripped of the document title that would have warned anyone. It can also combine fragments from several restricted sources into a single confident paragraph, which is precisely the kind of aggregation that data classification policies were written to prevent. Prompt instructions to "avoid confidential information" do nothing here, because confidentiality is a property of the source record and the requesting user, not something the model can infer from text.

The correct design carries permissions into the index and enforces them at query time. Every chunk should be stored with the security identifiers of the groups and users allowed to read its parent document, and the query should be filtered against the caller's resolved group membership before ranking, not after. The distinction matters: if the top twenty nearest chunks are all restricted, a post filter returns nothing while a pre filter returns the best twenty the user may actually see. Azure AI Search supports this directly through filterable security fields with Entra group resolution, and the same principle applies to any vector store that handles metadata filters well.

The part most teams miss is permission drift. People move teams, projects close, documents get reclassified. An index built from a nightly crawl carries yesterday's permissions, so a user removed from a group in the morning can still retrieve that group's documents until the next crawl runs. Treat access control lists as data with freshness requirements: incremental indexers that pick up permission changes alongside content changes, short crawl intervals for sensitive libraries, and an explicit staleness window agreed for each source.

Some source systems cannot express permissions the index can consume, because access is enforced in application logic rather than a readable ACL. For those, the safer pattern is retrieval through the application's own API on behalf of the user with delegated identity, instead of a bulk export into a shared index. It costs latency and some retrieval quality, but it inherits an authorization model that has already been audited rather than reconstructing one that has not.

Permission aware retrieval is the difference between an assistant that can be rolled out to the whole company and one that stays confined to a pilot group with clean data. Umplify builds retrieval architectures on Azure where authorization is enforced in the index and the query path rather than hoped for in the prompt; if your assistant has access to more than its users should, book a free discovery call and we will map the fix together.
