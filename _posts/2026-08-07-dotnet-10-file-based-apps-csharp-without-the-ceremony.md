---
title: "File-Based Apps in .NET 10: C# Without the Ceremony"
date: 2026-08-07 09:00:00 -0400
categories: [blog]
tags: [csharp, dotnet, developer-productivity]
excerpt: "With .NET 10 you can run a single C# file with dotnet run app.cs, no project file required. That small change lets enterprise teams pull their scripts and utilities back into the language, tooling, and review process they already trust."
header:
  overlay_color: "#08142C"
  overlay_filter: "0.18"
  overlay_image: /assets/images/revamp/hero-cloud.svg
---

Every enterprise .NET shop has a shadow toolchain. The production services are C#, but the glue around them, including deployment helpers, data fixes, migration checks, and one-off diagnostics, tends to live in PowerShell, Bash, or Python. The reason was never preference. It was ceremony. Writing a ten-line utility in C# meant creating a project file, a solution entry, and a build step, so teams reached for whatever ran from a single file. .NET 10 removes that excuse with file-based apps: write `app.cs`, run `dotnet run app.cs`, and you have a working program with no csproj in sight.

The mechanics are simple and deliberately practical. A file-based app is plain C# in a single file, with top-level statements if you want them. Special directives at the top of the file declare what the project file used to: `#:package` pulls in a NuGet package, and `#:sdk` selects an SDK such as the web SDK for a minimal API. On Linux and macOS you can add a shebang line and mark the file executable, which puts C# on equal footing with any shell script in your operations toolkit.

The business case is consistency. When a script that touches production is written in Python by a C# team, it usually escapes the standards that govern everything else: no code review discipline, no analyzers, no shared libraries, no tests. File-based apps let the same utility live in the same repository, pass through the same pull request process, and reuse the same internal packages, including your Azure SDK clients and domain models. The team's strongest language is now also its scripting language, and the governance surface shrinks accordingly.

There is also a growth path, which is where this feature earns its place in an enterprise context. When a quick tool becomes a real one, `dotnet project convert` turns the single file into a conventional project, translating the directives into an equivalent csproj. Nothing is rewritten and nothing is thrown away. The prototype you sketched during an incident can graduate into a tested, deployed internal tool without a port to another stack.

Know where the boundary sits. File-based apps are built for utilities, prototypes, automation, and learning, not for long-lived services with complex build requirements. They also need the .NET 10 SDK on the machine that runs them, which is worth folding into your base images and build agents before the scripts start to spread.

The bigger lesson is that platform friction quietly decides your architecture. Teams did not choose fragmented scripting; the toolchain chose it for them, and .NET 10 corrects that. If you are consolidating your engineering stack on .NET and Azure and want the platform decisions to be deliberate rather than accidental, [book a free discovery call](/contact/) and we will help you get there.
