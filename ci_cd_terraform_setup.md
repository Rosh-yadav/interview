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

