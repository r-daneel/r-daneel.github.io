# Ansible Tips 1

1.1 Ansible and grep

Relying on failure of a task to impement playbook logic is risky, as a lot of things can happen outside of the expected path.

Failure of a command may not indicate the situation we expect.

Thus, it is safer to make the logic rely on information gathered that can be checked besides the possible failure of a command.

If possible, the command should NEVER fail (unless there is an error you want to stop on). But then you have `grep`.

Using grep to count items will return an error status in case it finds no matches. It will however output '0', along with a non-zero return code.

See examples below on how to work around that.

```yaml
---

- name: ansible best practice example 1.1.1 - wrong use of grep
  hosts: localhost
  gather_facts: false
  vars:
    username: "root"

  tasks:

    - name: search for '{{ username }}', task fails if not found but does not fail the run
      command: "grep ^{{ username }}: /etc/passwd"
      register: result
      ignore_errors: true
      changed_when: false

    - name: show output if we found one or more matches
      debug:
        var: result.stdout
      when: result is succeeded

- name: ansible best practice example 1.1.2 - better use of grep
  hosts: localhost
  gather_facts: false
  vars:
    username: "root"

  tasks:

    - name: count occurrences of '{{ username }}', tasks cannot fail, but we expect a match count
      command: "grep -c ^{{ username }}: /etc/passwd"
      register: result
      failed_when: false
      changed_when: false

    - name: show output if we found exactly one match (if we have 2, it's a problem)
      debug:
        var: result.stdout
      when: result.stdout == "1"

```
