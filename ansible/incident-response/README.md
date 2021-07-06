# Incident response

## Requirements

```console
[student1@ansible-1 incident-response]$ ansible-playbook whitelist_attacker.yaml 
...
[student1@ansible-1 incident-response]$
```

Go to checkpoint management server **SECURITY POLICIES** to to the policy named **asa-accept-...** and in the column **Track** change it to **Log**. After that **Publish** and **Install the policy**.

Install the following collections:

```console
[student1@ansible-1 ~]$ ansible-galaxy install ansible_security.ids_rule
- downloading role 'ids_rule', owned by ansible_security
- downloading role from https://github.com/ansible-security/ids_rule/archive/master.tar.gz
- extracting ansible_security.ids_rule to /home/student1/.ansible/roles/ansible_security.ids_rule
- ansible_security.ids_rule (master) was installed successfully
[student1@ansible-1 ~]$ ansible-galaxy install ansible_security.ids_rule_facts
- downloading role 'ids_rule_facts', owned by ansible_security
- downloading role from https://github.com/ansible-security/ids_rule_facts/archive/master.tar.gz
- extracting ansible_security.ids_rule_facts to /home/student1/.ansible/roles/ansible_security.ids_rule_facts
- ansible_security.ids_rule_facts (master) was installed successfully
[student1@ansible-1 ~]$ ansible-galaxy install ansible_security.ids_config
- downloading role 'ids_config', owned by ansible_security
- downloading role from https://github.com/ansible-security/ids_config/archive/master.tar.gz
- extracting ansible_security.ids_config to /home/student1/.ansible/roles/ansible_security.ids_config
- ansible_security.ids_config (master) was installed successfully
[student1@ansible-1 ~]$ ansible-galaxy collection install ibm.qradar
Process install dependency map
Starting collection install process
Installing 'ibm.qradar:1.0.3' to '/home/student1/.ansible/collections/ansible_collections/ibm/qradar'
[student1@ansible-1 ~]$ 
```





Lab has to meet requirements according to its guide.

Add snort rule:

```console
[student1@ansible ~]$ ansible-playbook incident_snort_rule.yml
```

Initiate attack:

```console
[student1@ansible ~]$ ansible-playbook sql_injection_simulation.yml
```

To verify the attack go to the snort machine:

```console
[ec2-user@snort ~]$ journalctl -u snort -f
-- Logs begin at Mon 2021-07-05 10:38:27 UTC. --
Jul 06 11:40:30 snort snort[19831]: [1:99000020:1] Attempted Web Attack [Classification: Web Application Attack] [Priority: 1] {TCP} 172.17.82.118:37184 -> 172.17.137.137:80
Jul 06 11:40:35 snort snort[19831]: [1:99000020:1] Attempted Web Attack [Classification: Web Application Attack] [Priority: 1] {TCP} 172.17.82.118:37188 -> 172.17.137.137:80
Jul 06 11:40:40 snort snort[19831]: [1:99000020:1] Attempted Web Attack [Classification: Web Application Attack] [Priority: 1] {TCP} 172.17.82.118:37192 -> 172.17.137.137:80
Jul 06 11:40:45 snort snort[19831]: [1:99000020:1] Attempted Web Attack [Classification: Web Application Attack] [Priority: 1] {TCP} 172.17.82.118:37196 -> 172.17.137.137:80
Jul 06 11:40:50 snort snort[19831]: [1:99000020:1] Attempted Web Attack [Classification: Web Application Attack] [Priority: 1] {TCP} 172.17.82.118:37200 -> 172.17.137.137:80
```

```console
[root@snort ~]# tail -f /var/log/snort/merged.log
User-Agent: curl/7.29.0
Host: 172.17.137.137
Accept: */*

h<�`�AZ�����Rv����dP��`�AZ`�AZ���-���;,%�
                                         E�7�@?�z�Rv����dP���剠
[H+[H�GET /web_attack_simulation HTTP/1.1
User-Agent: curl/7.29.0
Host: 172.17.137.137
Accept: */*
```
