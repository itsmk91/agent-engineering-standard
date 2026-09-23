---
name: software-engineering-build-standard
description: "Use when writing or changing code in a software project: adding or extending a feature or command, starting a new application or project, refactoring or reorganizing a repository, changing architecture, persistence, databases, data models, state or target platforms, organizing tests or tooling, reducing technical debt, or reviewing engineering quality. Sizes the work as tiny, substantial or architectural and builds in module and state ownership, dependency direction, data contracts, tests, documentation and Git hygiene from the start, without over-engineering; tiny fixes get no ceremony. Not for explanation-only questions or mechanical documentation or copy edits. Explicit task scope, project-specific instructions, accepted ADRs and owner decisions take precedence."

2. Insert new lines directly above the line ### Existing project (§2, §34, §58, §63) (around line 53), keeping one blank line before it:
### Before the first edit (§18, §54)

In an existing Git project, record the starting point before changing any file: the base commit and the working-tree state, naming any changes that are not yours. For substantial or architectural work, also run the tests that cover the area and record the result, exactly as it ran; if they cannot run, record why. Tiny work records only the commit and working-tree state. This is the baseline the builder handoff reports later.
