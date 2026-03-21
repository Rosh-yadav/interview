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

## 4. 



