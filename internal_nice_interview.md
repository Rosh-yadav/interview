### do you know what is the difference between squash merge and Fast forward merge?

“We use squash merge mainly to avoid too many small or noisy commits in the main branch and keep it production-friendly.”

Yes, so both squash merge and fast-forward merge are strategies used to merge branches in Git, but they differ in how they handle commit history.
In a Fast-Forward Merge, if the target branch has no new commits, Git simply moves the branch pointer forward to the latest commit of the feature branch. This keeps all individual commits intact and maintains the complete history.

In contrast, a Squash Merge combines all the commits from the feature branch into a single commit before merging into the target branch. This results in a cleaner and more compact commit history.

So, fast-forward merge preserves full commit history, while squash merge simplifies it by creating a single commit.
In our project, we generally prefer squash merge for feature branches to keep the main branch history clean and readable.

### 🧠 Simple Way to Remember
Type	            What happens	                      History
Fast-forward  	Moves pointer forward	               Full history kept
Squash merge	   Combines commits into one	         Clean  history

#  2. have you worked on creating this branch policies into your reposit

Yes, I have worked on implementing branch policies in our repositories.

We usually configure branch protection rules for critical branches like main and develop.

Some of the key policies we enforce are:

- Pull Request (PR) is mandatory before merging code
- At least one or two code review approvals are required
- CI/CD pipeline checks must pass before merge (like build, test, lint checks)
- Direct push to main branch is restricted
- Commit history should follow standards (sometimes squash merge is enforced)

We also enable policies like:
- Resolving all comments before merging
- Preventing force push or branch deletion

This helps us maintain code quality, avoid breaking changes, and ensures a controlled and secure deployment process.
“These policies are tightly integrated with our CI/CD pipelines to ensure only validated and reviewed code reaches production.”

## 3. “What is the difference between git fetch and git pull?”
Yes, so both git fetch and git pull are used to get changes from a remote repository, but they work differently.

git fetch only downloads the latest changes from the remote repository and updates the local tracking branches, but it does not modify the working directory or current branch.
On the other hand, git pull not only fetches the changes but also automatically merges them into the current branch.
So, git fetch is safer because it allows us to review changes before merging, while git pull directly updates the working code.

## 4.  do you know the connection between that? Like how the secret is come flowing through the secret manager to your pipeline? How is that connection or how to setup this
# ✅ What you understood (partially correct)

You said:

> store secret → create IAM role → give permission → create access key → store in GitHub

✔️ This is **a working approach**, BUT
❗ it’s **NOT the best practice** (and interviewers expect you to know that)

---
# ❌ Problem in your approach

👉 You are doing this:

* Creating **Access Key + Secret Key**
* Storing them in **GitHub secrets**

⚠️ Issue:

* Long-lived credentials ❌
* Security risk if leaked ❌
* Manual rotation needed ❌

---
| Tool           | OIDC Support | Best Practice |
| -------------- | ------------ | ------------- |
| GitHub Actions | ✅ Native     | Use OIDC      |
| GitLab CI      | ✅ Supported  | Use OIDC      |
| Jenkins        | ❌ Not native | Use IAM Role  |

## 5. “What kind of checks does SonarQube perform?”
Yes, SonarQube performs static code analysis and checks multiple aspects of code quality.

It mainly focuses on:

- Code smells: These are maintainability issues like duplicate code, unused variables, or complex logic.

- Bugs: It identifies potential logical errors that may cause incorrect behavior in the application.

- Security vulnerabilities: It detects common security issues like hardcoded credentials, SQL injection risks, or insecure coding patterns.

- Code coverage: It integrates with testing tools to show how much of the code is covered by unit tests.

- Duplications: It checks for duplicate code blocks which impact maintainability.

- Maintainability, reliability, and security ratings: It gives overall ratings based on the quality of the code.

We usually define quality gates, and if the code does not meet the threshold, the pipeline fails and prevents merging or deployment.

🧠 Simple Way to Remember

👉
Bugs + Vulnerabilities + Code Smells + Coverage + Duplication

## 6 ❓ “What is Quality Gate?”
A Quality Gate is a set of conditions like minimum code coverage, no critical bugs, or no high-severity vulnerabilities. If the code fails these conditions, the pipeline is blocked.
## 7 ❓ “What is Code Smell?”
Code smell refers to maintainability issues in the code, like complex methods, duplicate code, or poor naming conventions.

## 8. “What is the difference between a Vulnerability and a Threat?”
Yes, vulnerability and threat are related but different concepts in security.

A vulnerability is a weakness or flaw in the system, application, or configuration that could be exploited. For example, hardcoded credentials, outdated libraries, or open ports.

A threat is a potential risk or actor that can exploit that vulnerability to cause harm. For example, a hacker, malware, or unauthorized access attempt.

So, in simple terms:
- Vulnerability = weakness
- Threat = something that can exploit that weakness

For example, if an application has a SQL injection flaw, that is a vulnerability. A hacker trying to exploit that flaw is the threat.

## 🔥 Real DevOps Example
Hardcoded AWS keys → Vulnerability
Someone using them to access your infra → Threat

## 9. “If a pipeline takes 30 minutes, how will you reduce the time?”

Yes, if a pipeline is taking around 30 minutes, I would first analyze which stages are consuming the most time and then optimize accordingly.

One key approach is parallelization. Independent stages like testing, linting, and security scans can be run in parallel instead of sequentially.

Second, I would implement caching. For example, caching dependencies, Docker layers, or build artifacts so that repeated builds don’t download or rebuild everything again.

Third, I would optimize the build process. This includes using smaller base images, multi-stage Docker builds, and avoiding unnecessary steps.

Fourth, I would reduce redundant steps by triggering pipelines only when relevant changes occur, such as running Terraform pipelines only when infrastructure code changes.

We can also use incremental builds or reuse previously built artifacts wherever possible.

Additionally, I would ensure efficient infrastructure by using faster runners or scaling runners dynamically.

Overall, by combining parallel execution, caching, and optimizing build steps, we can significantly reduce pipeline execution time.

### 10. So you mentioned multistage build here. So do you know why it is used in which scenarios?

Yes, multi-stage builds in Docker are used to optimize the final image size and improve security.

In a multi-stage build, we use multiple FROM statements. The first stage is used for building the application, where we include all build tools and dependencies. Then, in the final stage, we copy only the required artifacts from the build stage and exclude unnecessary files like compilers or development dependencies.
This helps in reducing the final image size and attack surface.

We mainly use multi-stage builds in scenarios where the application needs compilation or build steps, such as Java, Node.js, or Go applications.
For example, in a Java application, we use Maven to build the JAR file in the first stage, and then copy only the JAR into a lightweight runtime image in the final stage.
This improves performance, reduces image size, and makes deployments faster.

# Build stage
FROM node:18 AS build

WORKDIR /app
COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

# Runtime stage
FROM node:18-alpine

WORKDIR /app
COPY --from=build /app/dist ./dist

CMD ["node", "dist/index.js"]














