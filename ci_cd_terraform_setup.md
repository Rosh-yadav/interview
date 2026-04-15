---

# 🧱 1. Connect Terraform with AWS

### ✅ Step 1: Create IAM User in Amazon Web Services

* Go to IAM → Create User
* Enable **Programmatic Access**

### Permissions:

* For practice: `AdministratorAccess`
* In real projects: **least privilege policy**

---

### ✅ Step 2: Get Credentials

You’ll get:

* Access Key
* Secret Key

---

### ✅ Step 3: Configure in Terraform

Option 1 (local setup):

```bash
aws configure
```

Option 2 (in code – not recommended for real projects):

```hcl
provider "aws" {
  region     = "ap-south-1"
  access_key = "YOUR_KEY"
  secret_key = "YOUR_SECRET"
}
```

👉 Best practice: use **environment variables**

```bash
export AWS_ACCESS_KEY_ID=xxx
export AWS_SECRET_ACCESS_KEY=xxx
```

---

# 🔄 2. Connect GitHub/GitLab → Jenkins

### Tools:

* Jenkins
* GitHub or GitLab

---

## ✅ Step 1: Setup Jenkins

* Install Jenkins on EC2
* Install plugins:

  * Git
  * Pipeline
  * Credentials

---

## ✅ Step 2: Connect Git Repo to Jenkins

### In Jenkins:

* Create Pipeline Job
* Add Git repo URL

### Authentication:

* Use:

  * GitHub PAT (Personal Access Token)
  * OR SSH key

Store in Jenkins **Credentials Manager**

---

## ✅ Step 3: Setup Webhook (Trigger 🔥)

In GitHub/GitLab:

* Add webhook:

```
http://<jenkins-url>/github-webhook/
```

👉 Now:
**Code push → Jenkins pipeline auto trigger**

---

# 🔐 3. How Jenkins Connects to AWS

This is VERY important 🔥

---

## ✅ Option 1 (Basic – Interview Safe)

* Store AWS keys in Jenkins Credentials:

  * Access Key
  * Secret Key

* Use in pipeline:

```groovy
withCredentials([aws(credentialsId: 'aws-creds')]) {
    sh 'terraform apply'
}
```

---

## ✅ Option 2 (Best Practice – Real World)

👉 Use **IAM Role (No keys)**

* Attach role to EC2 (where Jenkins runs)
* Jenkins automatically gets permissions

👉 More secure (no hardcoded keys)

---

# ☁️ 4. Azure Setup (Similar Concept)

For Microsoft Azure:

### Create Service Principal:

```bash
az ad sp create-for-rbac
```

You’ll get:

* client_id
* client_secret
* tenant_id

Use in Terraform:

```bash
export ARM_CLIENT_ID=xxx
export ARM_CLIENT_SECRET=xxx
export ARM_TENANT_ID=xxx
export ARM_SUBSCRIPTION_ID=xxx
```

---

# 🚀 5. How Deployment Actually Happens (Full Flow)

👉 This is your **INTERVIEW GOLD ANSWER**

---

### Step-by-step Flow:

1. Developer pushes code → GitHub

2. Webhook triggers Jenkins

3. Jenkins pipeline starts

4. Jenkins:

   * Pulls code
   * Runs Terraform:

     * `terraform init`
     * `terraform plan`
     * `terraform apply`

5. Terraform uses:

   * AWS IAM / Azure Service Principal
   * Creates or updates infrastructure
---

---

# 🚀 1. Overall Flow (Understand First 🔥)

👉
“Developer writes Terraform code → pushes to repo → webhook triggers pipeline → pipeline runs terraform init/plan/apply → infrastructure is created/updated.”

---

# 🧱 2. What You Need

* Git repo → GitHub or GitLab
* CI/CD → Jenkins
* Cloud → Amazon Web Services
* Terraform installed on Jenkins server

---

# 🔐 3. Step-by-Step Setup (FROM SCRATCH)

---

## ✅ Step 1: Create Terraform Code

Example (`main.tf`):

```hcl id="d3azrz"
provider "aws" {
  region = "ap-south-1"
}

resource "aws_instance" "example" {
  ami           = "ami-xxxx"
  instance_type = "t2.micro"
}
```

Push this code to GitHub/GitLab.

---

## ✅ Step 2: Setup Jenkins

* Install Jenkins on EC2
* Install plugins:

  * Git
  * Pipeline
  * Credentials

---

## ✅ Step 3: Connect Git Repo to Jenkins

In Jenkins:

* Create **Pipeline Job**
* Add your repo URL (GitHub/GitLab)

### Authentication:

* Use PAT (token) or SSH key
* Store in Jenkins **Credentials**

---

## ✅ Step 4: Setup Trigger (Webhook 🔥)

In GitHub/GitLab:

* Go to Webhooks
* Add:

```id="o3xj3j"
http://<jenkins-url>/github-webhook/
```

👉 Now:
**Every push → Jenkins auto triggers**

---

## ✅ Step 5: Configure AWS Access

In Amazon Web Services:

* Create IAM User or Role
* Give permissions (EC2, VPC, etc.)

### Store in Jenkins:

* Add AWS Access Key & Secret Key in **Credentials**

---

## ✅ Step 6: Create Jenkins Pipeline

Create `Jenkinsfile` in repo:

```groovy id="z84wz3"
pipeline {
  agent any

  stages {

    stage('Checkout Code') {
      steps {
        git 'https://github.com/your-repo.git'
      }
    }

    stage('Terraform Init') {
      steps {
        sh 'terraform init'
      }
    }

    stage('Terraform Plan') {
      steps {
        sh 'terraform plan'
      }
    }

    stage('Terraform Apply') {
      steps {
        sh 'terraform apply -auto-approve'
      }
    }
  }
}
```

---

# 🔁 4. What Happens After Setup

1. You write Terraform code

2. Push to GitHub

3. Webhook triggers Jenkins

4. Jenkins:

   * Pulls code
   * Runs:

     * `terraform init`
     * `terraform plan`
     * `terraform apply`

5. Terraform connects to AWS → creates resources

---

---

# 🌍 6. GitLab Alternative (Same Concept)

If using GitLab:

* Use `.gitlab-ci.yml` instead of Jenkins

Example:

```yaml id="xv5e2i"
stages:
  - plan
  - apply

plan:
  script:
    - terraform init
    - terraform plan

apply:
  script:
    - terraform apply -auto-approve
```

---

