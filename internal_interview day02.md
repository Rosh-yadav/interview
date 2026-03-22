## 1.“Why do we use Alpine-based images?”
“We use Alpine images because they are lightweight, faster, and more secure due to minimal dependencies.”
### 2. How to write multi stage build pipeline?
## 🗣️ What you should SAY in interview

“In multi-stage builds, we use the AS keyword to name a stage, and then in the next stage we use COPY --from=<stage-name> to copy artifacts from the previous stage.”
⚡ Simple way to remember👉
Stage 1 → give name (AS build)
Stage 2 → use it (COPY --from=build)

## 3. “When do you use CMD and when do you use ENTRYPOINT?”
🎯 One-line answer
“Use ENTRYPOINT when the container must always run a specific command, and CMD when you want to provide a default command that can be overridden.”CMD for configurable parameters like ports or environment-specific arguments.”

## 4. can you write the command to how to build a image from a docker file
“docker build -t image-name:tag . is used to build an image from a Dockerfile.”
docker build -t my-app:1.0 -f Dockerfile.dev .

## 5. Do you know what are the different kinds of network options in a container?
## 6. Help me with your experince in kubernets ? 

I have hands-on experience in setting up and managing Kubernetes clusters, primarily on AWS using EKS, and also working with existing clusters.
My work includes deploying applications using Kubernetes manifests and Helm charts. I have created and managed core Kubernetes resources like Deployments, Services, ConfigMaps, and Secrets.
I have also worked on setting up Ingress controllers to enable external access to applications and handled end-to-end connectivity between services.
For deployments, I use Helm as a package manager to manage application releases and perform version upgrades and rollbacks.

## 7. “What are the required input variables to create an EKS cluster?”

First, we need the cluster name and the Kubernetes version.
Then, networking configuration is required, including VPC ID and subnet IDs where the cluster will be deployed.
We also need an IAM role for the EKS control plane, which allows EKS to manage AWS resources.
Next, we define node group configuration, which includes instance type, desired capacity, minimum and maximum number of nodes.
Additionally, we configure security groups to control inbound and outbound traffic.
Optionally, we may include configurations like endpoint access (public or private), logging, and tags.


## 8. How secret are stored in kubernetes or how to use external secret in secret manager ?

We first install the External Secrets Operator in the cluster.
Then we create a SecretStore resource which defines how to connect to AWS Secrets Manager.
After that, we create an ExternalSecret resource where we specify which secret to fetch.
The operator watches this resource, fetches the secret from AWS, and automatically creates a Kubernetes Secret.
Finally, we use that secret in our deployment as environment variables or volumes.




