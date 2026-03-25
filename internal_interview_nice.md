### 1. Issues with Jenkins agent how do you troubleshoot.

# ✅ 1. Disk Space Issue — What EXACTLY happened?

### 🔹 Problem

Jenkins stores everything inside:

```
/var/lib/jenkins/
```

Inside it:

```
workspace/   → project build files
jobs/        → build history, logs
```

Over time:

* Old builds + logs + artifacts = 💥 disk full

---

## 🔧 What you did (Explain like this in interview)

> "We found disk usage was high using `df -h`. Most of the space was consumed by `/var/lib/jenkins/workspace` and old build logs. So we manually cleaned old workspaces and then automated cleanup using scripts and log rotation."

---

# ✅ 2. Workspace Cleanup Script (VERY IMPORTANT 🔥)

You can say you automated it using cron.

### 🔹 Example Script:

```bash
#!/bin/bash

echo "Cleaning Jenkins workspace..."

find /var/lib/jenkins/workspace/ -type d -mtime +3 -exec rm -rf {} \;

echo "Cleanup completed"
```

### 🔹 What this does:

* Deletes folders older than **3 days**
* Prevents disk from filling again

---

### 🔹 Schedule using cron:

```bash
crontab -e
```

Add:

```bash
0 2 * * * /home/ec2-user/cleanup.sh
```

👉 Runs daily at 2 AM

---

💥 **Say this line in interview:**

> "We automated workspace cleanup using a cron-based script that deletes old files based on retention days."

---

# ✅ 3. What is Log Rotation & WHY it helps?

### 🔹 Problem:

Even if you clean workspace, Jenkins still stores:

* Build logs
* Console output
* Artifacts

👉 These are inside:

```
/var/lib/jenkins/jobs/
```

---

### 🔹 Log Rotation = Keep only limited builds

Instead of storing:
❌ 1000 builds
✅ Keep only last 10 builds

---

### 🔧 How to Configure (UI Method)

Go to:

* Job → Configure → **Discard Old Builds**

Set:

* Keep builds: 10
* Keep days: 7

---

### 🔹 Pipeline Example:

```groovy
pipeline {
    options {
        buildDiscarder(logRotator(
            daysToKeepStr: '7',
            numToKeepStr: '10'
        ))
    }
}
```

---

💥 **How it's connected to disk issue:**

> "Log rotation helps prevent disk issues by limiting stored build history and logs, so disk usage does not grow indefinitely."

---

# ✅ 4. Dynamic Scaling of Jenkins Agents (VERY IMPORTANT 🔥🔥)

This is where you can **stand out**.

---

## 🔹 What is Dynamic Scaling?

Instead of:
❌ Fixed agents (always running, wasting resources)

We use:
✅ Agents created ONLY when build runs
✅ Deleted after job finishes

---

## 🔹 How it's done?

Using:

* Kubernetes plugin (most common)
* Auto Scaling Groups (EC2-based)

---

## ✅ Case 1: Kubernetes-Based Jenkins Agents (Best Answer)

👉 Jenkins creates pods dynamically in EKS

---

### 🔧 Flow:

1. Job triggered
2. Jenkins requests Kubernetes
3. Pod (agent) is created
4. Job runs inside pod
5. Pod is deleted after completion

---

### 🔹 Configuration Steps:

1. Install Kubernetes plugin in Jenkins
2. Add Kubernetes cloud config:

   * API server
   * Credentials
3. Define Pod Template

---

### 🔹 Example Pipeline:

```groovy
pipeline {
  agent {
    kubernetes {
      label 'jenkins-agent'
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: docker
    image: docker:latest
    command:
    - cat
    tty: true
"""
    }
  }
  stages {
    stage('Build') {
      steps {
        sh 'echo Building...'
      }
    }
  }
}
```

---

💥 **Interview line:**

> "We used Kubernetes-based dynamic agents where Jenkins provisions pods on demand in EKS. These agents are ephemeral and get terminated after the job, which helps in efficient resource utilization and avoids issues like disk accumulation."

---

## ✅ Case 2: EC2 Auto Scaling (Alternative)

* Jenkins agents run on EC2
* Use Auto Scaling Group
* Scale based on:

  * CPU
  * Queue length

---

💥 Interview line:

> "We can also use EC2 Auto Scaling groups where agents scale based on demand, but Kubernetes-based agents are more efficient."

---

# 🎯 If they ask: "How do you schedule scaling?"

You say:

### 🔹 In Kubernetes:

* No manual scheduling needed
* Pods are created automatically per job

👉 BUT you can control:

* Resource limits
* Max pods

---

### 🔹 In EC2 Auto Scaling:

You can schedule like:

* Scale up at 9 AM
* Scale down at night

---

### 🔧 Example:

AWS Scheduled Scaling:

* Increase instances during peak hours
* Reduce during off-hours

---

💥 Answer:

> "In Kubernetes-based Jenkins agents, scaling is automatic based on job demand. For EC2-based agents, we can configure scheduled scaling using Auto Scaling policies."

---

# 🔥 FINAL INTERVIEW ANSWER (Combine Everything)

> "We faced a disk space issue in Jenkins agents where builds failed due to storage exhaustion. On investigation, we found workspace and build logs consuming space. We cleaned old files manually and automated it using a cron-based cleanup script. Additionally, we implemented log rotation to limit stored builds.
> For scalability, we used dynamic Jenkins agents with Kubernetes, where agents are provisioned as pods on demand and terminated after execution. This helped in better resource utilization and avoided persistent storage issues."

---
