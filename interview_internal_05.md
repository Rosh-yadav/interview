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

## 2. 
