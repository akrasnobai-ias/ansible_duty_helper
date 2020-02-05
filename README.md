# One-off Ansible Playbooks

#### Prerequisites
* ansible >= 2.3

### Check Reporting Pipeline
`check-reporting-pipeline` playbook's intended to help with regular duty tasks such as ad-hoc reporting pipeline monitoring

To run the whole bunch of checks:
```
ansible-playbook check-reporting-pipeline.yml
```
Only 1st hour checks:
```
ansible-playbook check-reporting-pipeline.yml --tags h1
```
By default it checks `prod`, to run against `staging`:
```
ansible-playbook check-reporting-pipeline.yml -i staging
```

Sample output:
```
$ ansible-playbook check-reporting-pipeline.yml --tags h2

PLAY [HOUR_01] *******************************************************************************************************************************************************************************

PLAY [HOUR_02] *******************************************************************************************************************************************************************************

TASK [Setup] *********************************************************************************************************************************************************************************
ok: [p-client03.nj01]

TASK [DT logs are ready for yesterday] *******************************************************************************************************************************************************
ok: [p-client03.nj01]

TASK [DT logs are collected for 00th bucket] *************************************************************************************************************************************************
ok: [p-client03.nj01]


TASK [Event log reaggregation is done] *******************************************************************************************************************************************************
fatal: [p-client03.nj01]: FAILED! => {"changed": false, "cmd": ["hdfs", "dfs", "-test", "-f", "/user/thresher/status/ias/eventreagg/archive/201802070000"], "delta": "0:00:02.509775", "end":
"2018-02-08 03:08:30.689672", "msg": "non-zero return code", "rc": 1, "start": "2018-02-08 03:08:28.179897", "stderr": "", "stderr_lines": [], "stdout": "", "stdout_lines": []}

TASK [Event log reaggregation is in_progress] ************************************************************************************************************************************************
ok: [p-client03.nj01]

TASK [Agency lookups exist] ******************************************************************************************************************************************************************
ok: [p-client03.nj01]

TASK [Network lookups exist] *****************************************************************************************************************************************************************
ok: [p-client03.nj01]

TASK [Qlog merge&scored 23+ buckets] *********************************************************************************************************************************************************
ok: [p-client03.nj01]

TASK [Qlog: aggregated 23+ buckets] **********************************************************************************************************************************************************
ok: [p-client03.nj01]

PLAY RECAP ***********************************************************************************************************************************************************************************
p-client03.nj01            : ok=8    changed=0    unreachable=0    failed=1
```
As you may see `Event log reaggregation is done` check failed and further investigation is required.
