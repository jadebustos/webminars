# Incident response

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
