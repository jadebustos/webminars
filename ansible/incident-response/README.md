# Incident response

## Requirements

```console
[student1@ansible-1 incident-response]$ ansible-playbook whitelist_attacker.yaml 
...
[student1@ansible-1 incident-response]$
```
> In snort server we can see if attack was launched:
>
> ```console
> 
> ```

```console
[student1@ansible-1 incident-response]$ ansible-playbook add_snort_rule.yaml
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

## Preparations

So first we need to set up a snort rule to actually generate log entries:

```console
[student1@ansible-1 incident-response]$ ansible-playbook incident_snort_rule.yaml
...
[student1@ansible-1 incident-response]$
```

Generating suspicious traffic:

```console
[student1@ansible-1 incident-response]$ ansible-playbook sql_injection_simulation.yaml 
...
[student1@ansible-1 incident-response]$
```

We can check it in snort machine:

```console
[root@snort ~]# journalctl -u snort -f
-- Logs begin at Tue 2021-07-06 17:47:14 UTC. --
Jul 06 21:57:15 snort snort[15938]: [1:99000030:1] Attempted SQL Injection [Classification: Attempted Administrator Privilege Gain] [Priority: 1] {TCP} 172.17.240.14:59812 -> 172.17.16.148:80
Jul 06 21:57:20 snort snort[15938]: [1:99000030:1] Attempted SQL Injection [Classification: Attempted Administrator Privilege Gain] [Priority: 1] {TCP} 172.17.240.14:59814 -> 172.17.16.148:80
...
```

Sends logs to Qradar:

```console
[student1@ansible-1 incident-response]$ ansible-playbook incident_snort_log.yaml
...
[student1@ansible-1 incident-response]$
```