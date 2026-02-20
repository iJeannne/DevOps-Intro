# Lab 3 — CI/CD with GitHub Actions

## Platform
GitHub Actions

---

## Task 1 — First Workflow

### What I did
- Created workflow file in `.github/workflows/lab3.yml`
- Configured trigger on `push`
- Pushed changes to GitHub

### Workflow Run Link
https://github.com/iJeannne/DevOps-Intro/actions/runs/22233589166

### Concepts Learned
- Workflow — automation defined in YAML
- Job — group of steps running on a runner
- Step — individual command
- Runner — virtual machine executing workflow
- Trigger — event that starts workflow

### What triggered the workflow
Workflow runs automatically after pushing commit.

---

## Task 2 — Manual Trigger + System Info

### Manual Trigger
Added `workflow_dispatch` event to enable manual workflow execution.

Manual run is available via **Run workflow** button in Actions tab.

### System Information Collected
Runner information collected using:

- uname -a → OS info  
- nproc → CPU cores  
- free -h → memory  
- df -h → disk usage  

### Runner Environment Analysis
Workflow uses GitHub hosted runner ubuntu-latest (Linux VM).

Manual trigger differs from push trigger because:
- push → automatic
- workflow_dispatch → manual via UI