---
title: 'Gitlet'
date: 2021-12-01
draft: false
description: 'A simplified version control system implementing core Git functionality in Java'
slug: 'gitlet'
---

A lightweight version control system that implements Git's core functionality from scratch in Java. Built as part of UC Berkeley's CS61B Data Structures course, Gitlet demonstrates practical application of data structures including trees, hash maps, and directed acyclic graphs to solve real-world problems.

## The Problem

Understanding how version control systems work under the hood is crucial for software engineers, yet most developers only interact with Git at a surface level. This project required building a functional VCS from the ground up, handling everything from object serialization to complex merge algorithms—all while maintaining efficiency and correctness.

## Key Features

**📁 Complete Version Control**  
Implements fundamental Git operations: init, add, commit, rm, status, log, and checkout. Each commit creates an immutable snapshot of the repository state, stored efficiently using SHA-1 hashing for content-addressable storage.

**🌳 Branching & Merging**  
Full support for creating, switching, and deleting branches. Implements a sophisticated merge algorithm that handles split points, conflict detection, and automatic merging when possible. The commit tree structure uses directed acyclic graphs to maintain branch relationships.

**⚡ Efficient Storage**  
Uses blob objects for file storage with content-based deduplication—identical files share the same blob regardless of commit or filename. Implements lazy loading and caching strategies to minimize disk I/O operations.

**🔍 History Tracking**  
Comprehensive logging with `log` (current branch history) and `global-log` (all commits). The `find` command searches commit messages across the entire repository history. Status command provides detailed staging area and working directory information.

## Technical Implementation

**Persistence Layer**  
Built a custom serialization system using Java's Serializable interface to persist repository state to disk. The `.gitlet` directory structure mirrors Git's design with separate folders for commits, blobs, and branches. Each object is stored with its SHA-1 hash as the filename, enabling O(1) lookups.

**Data Structures**

- **Commit Tree**: Directed acyclic graph implementation tracking parent-child relationships
- **Staging Area**: HashMap-based structure for tracking additions and removals
- **Blob Storage**: Content-addressable storage using SHA-1 hashing for deduplication
- **Branch Management**: Reference system pointing to commit SHAs

**Merge Algorithm**  
Implements a three-way merge that:

1. Finds the split point (latest common ancestor) using BFS traversal
2. Compares files across current branch, given branch, and split point
3. Automatically merges non-conflicting changes
4. Marks conflicts for manual resolution when both branches modify the same file

## Key Commands

**Repository Management:**

- `init` – Creates new repository with initial commit
- `add/rm` – Stages files for addition or removal
- `commit` – Creates immutable snapshot with parent reference

**History & Branches:**

- `log/global-log` – Views commit history
- `branch/rm-branch` – Creates or deletes branches
- `checkout` – Restores files or switches branches
- `reset` – Moves current branch to specified commit

**Advanced Operations:**

- `merge` – Three-way merge with conflict detection
- `find` – Searches commits by message
- `status` – Displays repository state

## Challenges & Solutions

**Challenge**: Implementing efficient file deduplication across commits  
**Solution**: Content-based addressing using SHA-1 hashes—identical files reference the same blob object regardless of name or location

**Challenge**: Finding the merge split point in complex branch histories  
**Solution**: BFS traversal of the commit DAG to find the latest common ancestor, handling multiple merge commits correctly

**Challenge**: Managing persistence without a database  
**Solution**: Custom serialization layer with careful file organization, using SHA-1 hashes as natural indexes for O(1) retrieval

---

**Note**: Source code available upon request as per UC Berkeley's academic integrity policies.  
<a class="button primary big" href="mailto:leechristopher722@gmail.com" target="_blank">Request Source Code</a>
