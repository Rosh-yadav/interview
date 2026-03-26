### 1. if an ansible playbook fails halfway, what are the options do you have to re-run?

"If an Ansible playbook fails halfway, we can re-run it safely because Ansible is idempotent. Additionally, we can use option
s like --start-at-task, --limit, or tags to resume execution from a specific point or host."

🔹 2. Start from Failed Task
ansible-playbook site.yml --start-at-task="Install Nginx"

🔹 3. Run on Specific Hosts
ansible-playbook site.yml --limit web1

👉 Useful when:

Failure happened on specific server only

If playbook has tags:

- name: Install packages
  tags: install

Run:

ansible-playbook site.yml --tags install

👉 Helps re-run only failed section

"If a playbook fails halfway, first I check the error and fix the issue. Then I can re-run the entire playbook since Ansible is idempotent.
If needed, I can resume from a specific task using --start-at-task, or target specific hosts using --limit. If the playbook uses tags,
I can re-run only the failed section."

## 2. 2. how we can reduce a particular docker image size?

"We can reduce Docker image size using multi-stage builds, lightweight base images like Alpine, minimizing layers, cleaning up caches, and excluding unnecessary files using .dockerignore."  minizning layer by using single command in or condition so to save RUN apt-get update && apt-get install -y nginx

👉 Combine commands into one layer

4. Clean Up Cache (VERY IMPORTANT 🔥)
RUN apt-get update && apt-get install -y curl \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

👉 Removes unnecessary cache files
"Smaller images improve build time, reduce storage usage, and speed up deployment in Kubernetes or CI/CD pipelines."
❌ What Was Missing in Your Answer
Cache cleanup
.dockerignore
Avoid copying unnecessary files
Combining RUN commands


### 3. How Kubernetes handles scaling and self-healing, the properties that it comes up. How does Kubernetes handle it?

"Kubernetes handles self-healing using Deployments and ReplicaSets, which ensure the desired number of pods are always running. If a pod fails, it is automatically recreated. It also uses liveness and readiness probes to detect failures. For scaling, Kubernetes supports manual scaling and automatic scaling using Horizontal Pod Autoscaler based on metrics like CPU usage."

### 4. 4. What to do for scaling? How does Kubernetes manage scaling? Like if there is increase in demand in terms of CPU, memory, or anything else ? Like resources, which are not sufficient enough. Basically, your 3-node or 4-node cluster is not enough ? And you want to schedule a port, but it is not able to schedule due to insufficient resources, and you need to increase the number of worker nodes. So how does Kubernetes handle all those situations?


"Kubernetes handles scaling at two levels: pod-level scaling and cluster-level scaling.
For increased demand like high CPU or memory usage, Kubernetes uses Horizontal Pod Autoscaler to scale pods.
If the cluster itself does not have enough resources to schedule new pods, Kubernetes uses Cluster Autoscaler to add more worker nodes dynamically."

## 🧠 Key Components to Mention
HPA → scales pods
Cluster Autoscaler → scales nodes
Scheduler → assigns pods to nodes

"Kubernetes scaling is multi-layered — HPA handles workload scaling, while Cluster Autoscaler ensures infrastructure scalability."

✅ Cluster Autoscaler
🔧 What it does

It automatically adds or removes worker nodes based on demand.
✅ Karpenter

👉 Alternative to Cluster Autoscaler
👉 Faster and more efficient in AWS
"The add-on used for node-level autoscaling in Kubernetes is Cluster Autoscaler, which works with cloud provider APIs to dynamically add or remove worker nodes based on pod scheduling requirements."

"Cluster Autoscaler scales nodes by adjusting Auto Scaling Groups, while Karpenter directly provisions nodes using AWS APIs. Karpenter is faster, more flexible, and cost-efficient because it selects optimal instance types dynamically, whereas Cluster Autoscaler depends on predefined node groups."


### 5. 5. I have some microservices that I want to host ? And they are already containerized, And if I come and ask you, what would you suggest? Should I use ECS to run my services for ECS?
"If the microservices are simple and we want minimal operational overhead, I would choose ECS. But if the architecture is complex, requires advanced orchestration, or needs Kubernetes features like Helm, autoscaling, or portability, I would go with EKS."

## 6. 6. What is the difference between CloudWatch and CloudTrail?
"CloudWatch is used for monitoring, logging, and alerting of AWS resources, while CloudTrail is used for auditing and tracking API activity in AWS."

## 7. How we will integrate the kubernetes with aws secret manager ?

Yes, we use AWS Secrets Manager as the source of truth.
We install External Secrets Operator in Kubernetes, which acts as a bridge between AWS and the cluster.
We configure a SecretStore to connect to AWS, and then define an ExternalSecret where we specify which secret to fetch.
The operator automatically retrieves the secret from AWS and creates a Kubernetes Secret.
Then we use that Kubernetes Secret in our deployment as environment variables or mounted volumes.
This way, we don’t manually manage secrets in Kubernetes and keep everything secure and centralized.

We store secrets in AWS Secrets Manager as the source of truth.
In Kubernetes, we install External Secrets Operator and configure a SecretStore to connect AWS.
Then we create an ExternalSecret where we define which secret to fetch.
The operator automatically retrieves the secret and creates a Kubernetes Secret.
Finally, we use that Kubernetes Secret in our deployment as environment variables or volumes.
This way, secrets are securely managed in AWS and dynamically synced to Kubernetes.

1. Install External Secrets Operator

2. Create SecretStore
   → tells HOW to connect AWS

3. Create ExternalSecret
   → tells WHAT secret to fetch

4. Operator reads ExternalSecret

5. Operator calls AWS Secrets Manager

6. Operator creates Kubernetes Secret

7. Pod uses it


