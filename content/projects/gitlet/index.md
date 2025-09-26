---
title: 'Gitlet'
date: 2021-12-01
draft: false
description: 'Gitlet Project Page'
slug: 'gitlet'
---

A version-control system similar to Git but designed to be simpler and smaller.

**Key functionalities:**

- **Saving file versions (committing):** Creates snapshots of entire directories, enabling restoration to previous states.
- **Restoring file versions (checking out):** Reverts files or entire commits to a chosen point in time.
- **Viewing history (log):** Displays a chronological list of commits made to the project.
- **Maintaining branches:** Creates divergent development paths within the project.
- **Merging branches:** Combines changes from different branches into a single branch.

## Differences from actual Git

1. **Flat Directory Structure:**
   Gitlet incorporates trees into commits but does not deal with subdirectories. This results in a "flat" directory structure where each repository contains only plain files without nested directories.

2. **Limited Merging:**
   Unlike Git, which supports merges with any number of parents, Gitlet limits merges to references with only two parents.

3. **Minimal Metadata:**
   Gitlet's metadata for commits is simplified, consisting only of a timestamp and a log message. A commit in Gitlet includes a log message, timestamp, a mapping of file names to blob references, and (for merges) a second parent reference.

## Detailed functionalities:

**Commands:**

- **init:** Creates a new Gitlet repository in the current directory.
- **add:** Stages a file for inclusion in the next commit.
- **commit:** Creates a new commit with the staged changes and a commit message.
- **rm:** Removes a file from the staging area and marks it for untracking in the next commit.
- **log:** Displays the commit history, starting from the current head commit and following the parent commit links.
- **global-log:** Shows information about all commits ever made, regardless of the current branch.
- **find:** Prints the unique identifiers of commits containing a specific message.
- **status:** Displays information about the current branch, staged files, removed files, modified files, and untracked files.

## Things I've Learned

Creating Gitlet deepened my grasp of version control systems and Object-Oriented Programming (OOP). Building a simplified Git taught me Java programming intricacies and reinforced my understanding of commits, branches, and merges.

Developing Gitlet honed my skills in modular software design, efficient file handling, and managing complex data structures. It prompted critical thinking about design trade-offs, emphasizing simplicity without sacrificing functionality.

In summary, Gitlet enhanced my technical proficiency and appreciation for version control and software design principles, showcasing the value of hands-on learning in software engineering.

**Disclaimer**

This project was developed for UC Berkeley's CS61B: Data Structures Course.
Source code can be provided on
<a class="button primary big" href="mailto:leechristopher722@gmail.com" target="_blank" >request</a>.
