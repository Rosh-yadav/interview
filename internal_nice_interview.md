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
