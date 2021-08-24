# New-Relic-infrastructure-agent-installation
>To install the infrastructure monitoring agent in Linux, follow these instructions:
```
  - name: install config file
    ansible.builtin.lineinfile:
      path: /etc/newrelic-infra.yml
      line: 'license_key: YOUR_LICENSE_KEY'
      create: yes
```
>Add the infrastructure monitoring agent repository
```
  - name: Add NewRelic repository
    get_url:
      url: https://download.newrelic.com/infrastructure_agent/linux/yum/el/7/x86_64/newrelic-infra.repo
      dest: /etc/yum.repos.d/newrelic-infra.repo
      mode: '0644'
```
>Install the newrelic-infra package 
```
  - name: Install a nerelic-infra and nri-oracledb package
    yum:
      name:
        - newrelic-infra
        - nri-oracledb
      state: present
```
>Make sure that newrelic-infra service will be in start state
```
  - name: Start service newrelic-infra, if not started
    ansible.builtin.service:
      name: newrelic-infra
      state: started
```
>Change the file permission of docker-config.yml

```
  - name: Change file ownership, group and permissions
    ansible.builtin.file:
      path: /etc/newrelic-infra/integrations.d/docker-config.yml
      owner: root
      group: root
      mode: '0777'
```
>Rename the file oracledb-config.yml.sample to oracledb-config.yml
```
  - name: Copy file with owner and permissions
    ansible.builtin.copy:
      src: /etc/newrelic-infra/integrations.d/oracledb-config.yml.sample
      dest: /etc/newrelic-infra/integrations.d/oracledb-config.yml
      owner: root
      group: root
      mode: '0777'
```
>Restart the newrelic-infra service as we rename the oracledb-config.yml file
```
- name: Restart service newrelic-infra, in all cases
    ansible.builtin.service:
      name: newrelic-infra
      state: restarted
```
