# Lab 2 Submission — Version Control & Advanced Git

## Task 1: Git Object Model Exploration

### Object Inspection Outputs
```bash
# 1. Commit object
# Команда: git log --oneline -1
c6f3b27 (HEAD -> feature/lab2) Add test file
# Команда: git cat-file -p c6f3b27
tree 04e0a73fdd426bdd4b8509426d363843b9a9a7f0
parent 04c8701055d8753ef4267d3a401ac1b6e6cc19ef
author iJeannnee <zhanna9628644@gmail.com> 1770645600 +0300
committer iJeannnee <zhanna9628644@gmail.com> 1770645600 +0300
gpgsig -----BEGIN SSH SIGNATURE-----
 U1NIU0lHAAAAAQAAADMAAAALc3NoLWVkMjU1MTkAAAAgTkB59RC4O4vBl4bts8123QKHyv
 2l4O2QZFCpdKdQH0EAAAADZ2l0AAAAAAAAAAZzaGE1MTIAAABTAAAAC3NzaC1lZDI1NTE5
 AAAQLqAdVrIXfrl5tcYAuLOBWzZ3VWu80cbs2kflDnN8FqzeOnWtig0VW7/FsahaCiaNI
 zd7VGxfDTu1KYHjrFw7AM=
 -----END SSH SIGNATURE-----

Add test file

# 2. Tree object
# Команда: git cat-file -p 04e0a73fdd426bdd4b8509426d363843b9a9a7f0
040000 tree 45035f66155ac1790db26de8f059c7a738151985	.github
100644 blob 6e60bebec0724892a7c82c52183d0a7b467cb6bb	README.md
040000 tree a1061247fd38ef2a568735939f86af7b1000f83c	app
040000 tree eb79e5a468ab89b024bd4f3ed867c6a3954fe1f0	labs
040000 tree d3fb3722b7a867a83efde73c57c49b5ab3e62c63	lectures
100644 blob 2eec599a1130d2ff231309bb776d1989b97c6ab2	test.txt

# 3. Blob object
# Команда: git cat-file -p 2eec599a1130d2ff231309bb776d1989b97c6ab2
Test content



Analysis

What represents each object type?

Commit: Stores snapshot metadata, including author, committer, date, and a pointer to the root tree.

Tree: Acts like a directory, mapping filenames to blob hashes or other trees.

Blob: Stores only the file data without any metadata or filenames.

How does Git store data? Git uses a content-addressable storage system where every object is identified by a SHA-1 hash of its contents.

## Task 2: Reset and Reflog Recovery

### Commands and Outputs

# 1. Creating history
git switch -c git-reset-practice
echo "First commit" > file.txt && git add file.txt && git commit -m "First commit"
echo "Second commit" >> file.txt && git add file.txt && git commit -m "Second commit"
echo "Third commit"  >> file.txt && git add file.txt && git commit -m "Third commit"

# 2. Reset operations
# Performing soft reset (moving HEAD to Second commit)
git reset --soft HEAD~1
# Performing hard reset (moving HEAD to First commit, discarding changes)
git reset --hard HEAD~1

# 3. History state after resets
git log --oneline
1b9396c (HEAD -> git-reset-practice) First commit
c6f3b27 (feature/lab2) Add test file

# 4. Recovery using reflog
git reflog
1b9396c (HEAD -> git-reset-practice) HEAD@{0}: reset: moving to HEAD~1
04330b7 HEAD@{1}: reset: moving to HEAD~1
2d5216c HEAD@{2}: commit: Third commit
04330b7 HEAD@{3}: commit: Second commit
1b9396c (HEAD -> git-reset-practice) HEAD@{4}: commit: First commit

# Restoring to Third commit state
git reset --hard 2d5216c
HEAD is now at 2d5216c Third commit

Analysis

Reset modes: --soft only moves the HEAD, keeping changes staged; --hard resets everything, including the working directory.

Recovery: Using git reflog, I was able to find the hash of the "lost" commit and restore it using git reset --hard <hash>.

Task 3: Visualize Commit History
Commit Graph

# git log --oneline --graph --all
* 02de7c7 (side-branch) Side branch commit
* 2d5216c (git-reset-practice) Third commit
* 04330b7 Second commit
* 1b9396c First commit
* c6f3b27 (HEAD -> feature/lab2) Add test file
| * 8f51648 (feature/lab1) docs: ignore .DS_Store files
| * 1a3f327 (origin/feature/lab1) docs: add lab1 submission with screenshots
|/  
* 04c8701 (origin/main, origin/HEAD, main) docs: add PR template
* d6b6a03 Update lab2
* 87810a0 feat: remove old Exam Exemption Policy
* 1e1c32b feat: update structure
* 6c27ee7 feat: publish lecs 9 & 10
* 1826c36 feat: update lab7
* 3049f08 feat: publish lec8
* da8f635 feat: introduce all labs and revised structure
* 04b174e feat: publish lab and lec #5
* 67f12f1 feat: publish labs 4&5, revise others
* 82d1989 feat: publish lab3 and lec3
* 3f80c83 feat: publish lec2
* 499f2ba feat: publish lab2
* af0da89 feat: update lab1
* 74a8c27 Publish lab1



Reflection

The graph visualization helps understand branch divergence and merges, making it clear where history splits and joins.

## Task 4: Tagging Commits

### Commands and Hashes

# Tag creation and push
git tag v1.0.0
git push origin v1.0.0

# Output:
# * [new tag]         v1.0.0 -> v1.0.0
#Commit hash:  c6f3b27

Tags act as permanent bookmarks in history. They are essential for versioning (marking software releases like v1.0.0), triggering automated CI/CD pipelines, and providing clear points for generating release notes.

## Task 5: git switch vs git checkout vs git restore

### Evidence of State

# Testing git switch
git switch -c cmd-compare
git branch
# * cmd-compare
#   feature/lab2
git switch feature/lab2

# Testing git restore
echo "This is a mistake" >> test.txt
git status
# modified:   test.txt
git restore test.txt
git status
# nothing added to commit but untracked files present

Analysis

Use git switch for branch operations (creating, switching). It is safer and clearer than the old checkout command.

Use git restore to discard local changes in files or unstage them. It's much more intuitive than using checkout for file recovery.

Avoid git checkout because it's "overloaded"—it tries to manage both branches and files, which often leads to accidental mistakes.

Task 6: GitHub Community
Engagement Analysis

Why starring matters: Starring signals appreciation to maintainers and helps the community discover high-quality projects.

Why following matters: Following developers builds a professional network and keeps you informed about trends and best practices in the industry.

Challenges & Solutions
During this lab, I encountered an issue where local changes to .DS_Store prevented me from switching branches. I solved this by adding .DS_Store to .gitignore and removing it from the Git index using git rm --cached.
