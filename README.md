---
- name: Update Docker containers
  hosts: localhost
  become: true
  tasks:
    - name: Get list of running Docker containers
      shell: docker ps --format '{{.Names}}'
      register: running_containers
      changed_when: false

    - name: Update each Docker container
      docker_container:
        name: "{{ item }}"
        state: restarted
      loop: "{{ running_containers.stdout_lines }}"
