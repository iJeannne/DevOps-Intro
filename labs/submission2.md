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

Task 2: Reset and Reflog Recovery
Commands and Outputs

bash
# [ВСТАВЬ ВЫВОД git log --oneline ПОСЛЕ ТВОИХ RESET]
# [ВСТАВЬ ВЫВОД git reflog]
Analysis

Reset modes: --soft only moves the HEAD, keeping changes staged; --hard resets everything, including the working directory.

Recovery: Using git reflog, I was able to find the hash of the "lost" commit and restore it using git reset --hard <hash>.

Task 3: Visualize Commit History
Commit Graph

# [ВСТАВЬ ВЫВОД git log --oneline --graph --all]

Reflection

The graph visualization helps understand branch divergence and merges, making it clear where history splits and joins.

Task 4: Tagging Commits
Commands and Hashes

bash
# [ВСТАВЬ ВЫВОД git tag v1.0.0 И git push origin v1.0.0]
Tag: v1.0.0

Commit hash: [ВСТАВЬ ХЭШ КОММИТА]

Task 5: git switch vs git checkout vs git restore
Evidence of State

bash
# [ВСТАВЬ ВЫВОДЫ git switch И git restore КОМАНД]
Analysis

Use git switch for branch operations (cleaner alternative to checkout).

Use git restore for file-level changes (to undo edits or unstage files).

Avoid git checkout because it's "overloaded" (does too many different things).

Task 6: GitHub Community
Engagement Analysis

Why starring matters: Starring signals appreciation to maintainers and helps the community discover high-quality projects.

Why following matters: Following developers builds a professional network and keeps you informed about trends and best practices in the industry.

Challenges & Solutions
During this lab, I encountered an issue where local changes to .DS_Store prevented me from switching branches. I solved this by adding .DS_Store to .gitignore and removing it from the Git index using git rm --cached.
