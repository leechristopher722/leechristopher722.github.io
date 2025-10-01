---
title: 'Git Fundamentals'
date: 2024-11-10
draft: false
description: 'Real lessons from learning Git the hard way'
slug: 'git'
tags: ['Git', 'Version Control', 'Developer Tools']
---

Let's be honest about Git: everyone pretends to understand it, but most of us are secretly terrified we'll accidentally delete the entire codebase with one wrong command. If you've ever stared at a merge conflict like it's written in ancient hieroglyphics, this post is for you.

## Why Git Feels Like Dark Magic

When I first encountered Git, the tutorials made it sound simple: "Just commit your changes and push!" What they didn't mention was the existential crisis I'd face when encountering my first detached HEAD state, or the panic of realizing I'd been committing to the wrong branch for three days.

The truth is, Git is conceptually simple but practically complex. It's like chess—you can learn how the pieces move in minutes, but mastering the game takes years.

## The Mental Model That Finally Clicked

After countless botched merges and accidental force pushes, here's the mental model that finally made Git make sense:

**Git is a time machine for your code.** Every commit is a snapshot in time. Branches are alternate timelines. Merging is combining timelines. Rebasing is rewriting history (with all the dangers that implies).

Once I started thinking this way, commands started making intuitive sense:

- `git checkout` = travel to a different point in time
- `git reset --hard` = forcefully travel back in time (destroying everything that came after)
- `git stash` = put current changes in a pocket dimension for safekeeping

## The Commands You Actually Need

Forget the 150+ Git commands. Here's what you'll use 95% of the time:

### The Daily Workflow

```bash
git status              # What's going on?
git add .              # Stage everything (or be specific)
git commit -m "message" # Save snapshot
git pull               # Get latest changes
git push               # Share your changes
```

### The "Oh No" Commands

```bash
git diff                    # What did I change?
git log --oneline -10      # What happened recently?
git checkout -- filename   # Undo changes to specific file
git reset HEAD~1           # Undo last commit (keep changes)
git reset --hard HEAD~1    # Nuclear option: destroy last commit
```

### The Collaboration Commands

```bash
git checkout -b feature-name  # Create new timeline
git merge main                # Bring in changes from main
git rebase main              # Alternative to merge (rewrites history)
git cherry-pick <commit-id>  # Steal one commit from another branch
```

## Branches: Your Safety Net

Here's what nobody told me about branches: they're free, disposable, and your best friend. Create branches liberally:

```bash
git checkout -b experiment/crazy-idea
# Go wild, break things
# If it works: merge it
# If it doesn't: delete it and pretend it never happened
```

The branching epiphany came when I realized branches aren't precious. They're scratch paper. Make a mess, try things out, throw them away if they don't work.

## The Merge Conflict Survival Guide

Merge conflicts aren't errors—they're Git asking for your help. Here's the systematic approach that turned panic into process:

1. **Don't panic** (easier said than done)
2. **Understand what happened**: Two people changed the same part of the same file
3. **Open the conflicted file** and look for:

```
<<<<<<< HEAD
Your version
=======
Their version
>>>>>>> branch-name
```

4. **Decide what to keep**: Sometimes it's yours, sometimes theirs, often both
5. **Remove the markers** and save
6. **Add and commit** the resolution

Pro tip: Use a merge tool like VS Code's built-in merger. It turns cryptic conflicts into visual choices.

## Rebasing: The Sharp Knife

Rebasing is Git's most powerful and dangerous feature. It literally rewrites history. Use it for:

- Cleaning up messy commit history before merging
- Keeping feature branches up-to-date with main
- Making your commits look like you knew what you were doing all along

```bash
# Interactive rebase to clean up last 3 commits
git rebase -i HEAD~3
```

But remember the golden rule: **Never rebase commits that exist outside your local repository**. If you've pushed it, don't rebase it (unless you really know what you're doing).

## The Mistakes That Taught Me the Most

### The Force Push Disaster

I once force-pushed to main and wiped out a colleague's entire day of work. Lesson learned:

- Never force push to shared branches
- If you must force push, use `--force-with-lease` (it's safer)
- Communicate with your team

### The Commit Message Regret

Early commits looked like:

```
"fix"
"updates"
"asdfasdf"
"finally works"
```

Now I follow this format:

```
type: brief description

Longer explanation if needed
- Bullet points for multiple changes
- Reference issue numbers (#123)
```

### The Binary File Bloat

Committed a 100MB test video. Repository became sluggish forever. Lessons:

- Use `.gitignore` religiously
- Think twice before committing binaries
- Large files belong in Git LFS or somewhere else entirely

## Git in the Real World

### The Pull Request Dance

Real development isn't just committing to main. It's:

1. Create feature branch
2. Make changes
3. Push branch
4. Open pull request
5. Address review comments
6. Squash and merge

### The Unwritten Rules

- Commit early and often (you can clean up history later)
- Write commit messages for your future confused self
- Pull before you push (avoid unnecessary conflicts)
- When in doubt, make a backup branch
- `git reflog` is your safety net—it tracks everything

## Advanced Techniques Worth Learning

### Stashing: Your Quick Save

```bash
git stash                  # Quick save current changes
git stash pop              # Restore and delete stash
git stash apply            # Restore but keep stash
git stash list             # See all stashes
```

### Bisect: Binary Search for Bugs

```bash
git bisect start
git bisect bad                 # Current commit is broken
git bisect good <commit-hash>  # This commit worked
# Git checks out commits for you to test
# Keep marking as good/bad until bug commit found
```

### Worktrees: Multiple Branches Simultaneously

```bash
git worktree add ../project-hotfix hotfix-branch
# Now you have two folders with different branches checked out
```

## The Path to Git Confidence

Git mastery isn't about memorizing commands—it's about understanding the model and knowing how to recover from mistakes. Here's the progression:

1. **Fear Stage**: Copy-pasting commands, praying nothing breaks
2. **Cautious Stage**: Understanding basic commands, still scared of rebasing
3. **Confident Stage**: Comfortable with branches, merging, and basic recovery
4. **Mastery Stage**: Rebasing interactively, bisecting bugs, teaching others

Most developers hover between stages 2 and 3, and that's perfectly fine.

## Final Thoughts

Git is a powerful tool that becomes less intimidating with practice. Every developer has Git horror stories—force pushing to main, losing work, spending hours on merge conflicts. These aren't failures; they're rites of passage.

The secret to Git confidence isn't avoiding mistakes—it's knowing how to recover from them. Keep a learning mindset, experiment in safe environments, and remember: someone, somewhere, has made a worse Git mistake than you're about to make.
