## 1.What are lifecycle block keywords like prevent_destroy, create_before_destroy, etc.?
Yes, in Terraform we use lifecycle blocks to control how resources are created, updated, or destroyed.

Some commonly used lifecycle arguments are:
prevent_destroy:
This prevents accidental deletion of critical resources. If someone tries to destroy the resource, Terraform will throw an error.

create_before_destroy:
This ensures that a new resource is created before the old one is destroyed. It helps in avoiding downtime, especially for resources like load balancers or instances.

ignore_changes:
This tells Terraform to ignore specific attribute changes, for example if some values are modified outside Terraform or updated dynamically.
These lifecycle rules help us manage infrastructure safely and avoid downtime or unintended changes.

⚡ Real Use Cases (say this 🔥)
✅ prevent_destroy

👉 Use for:
RDS databases
Production resources
✅ create_before_destroy

👉 Use for:
Load balancers
Auto Scaling resources
Zero downtime deployments

✅ ignore_changes
👉 Use for:
Auto-updated fields
External modifications

### 2. What are the different ways to pass input variables in Terraform?
Yes, in Terraform we can pass input variables in multiple ways.

First, using default values in the variable block. If a default is defined, Terraform uses it automatically.
Second, using a .tfvars file, where we define variable values in a separate file and pass it during execution.
Third, using command-line arguments with the -var flag while running terraform apply.
Fourth, using environment variables by prefixing them with TF_VAR_.
Fifth, through Terraform Cloud or CI/CD pipelines where variables are passed dynamically.
These methods provide flexibility depending on the use case and environment.





