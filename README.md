# Ansible Smart Restart

Example of how to deploy the same app with two different names and restart on
just the servers we specify.

App names could map to upstart configs, etc.

In real life since we may move apps around to different servers we use a shotgun
approach to stopping apps -- i.e. we attempt to stop every app on every server
that starts with a given prefix. But for startup, we use a rifle approach --
only start exactly the app we want on the server(s) we want.

Output should be as shown below.

```
$ ansible-playbook --connection=local -i hip.inv hip.yml

PLAY [Example of parameterized role and app startup] **************************

GATHERING FACTS ***************************************************************
ok: [broker3.com]
ok: [broker1.com]
ok: [data2.com]
ok: [broker2.com]
ok: [data1.com]
ok: [service1.com]
ok: [service2.com]
ok: [service4.com]
ok: [service3.com]
ok: [service5.com]
ok: [web.com]

TASK: [retina | redeploy with notify restart] *********************************
skipping: [data2.com]
skipping: [broker1.com]
skipping: [broker2.com]
skipping: [broker3.com]
skipping: [data1.com]
skipping: [service1.com]
skipping: [service2.com]
skipping: [service3.com]
skipping: [service4.com]
skipping: [service5.com]
changed: [web.com]

TASK: [retina | just some feedback here] **************************************
skipping: [data1.com]
skipping: [data2.com]
skipping: [broker3.com]
skipping: [broker1.com]
skipping: [broker2.com]
skipping: [service1.com]
skipping: [service2.com]
skipping: [service4.com]
skipping: [service3.com]
skipping: [service5.com]
ok: [web.com] => {
    "msg": "\"{u'changed': True, u'end': u'2015-02-24 07:35:43.563091', u'stdout': u'we just redeployed retinaOne', u'cmd': u'echo \"we"
}

TASK: [retina | redeploy with notify restart] *********************************
skipping: [broker1.com]
skipping: [broker3.com]
skipping: [broker2.com]
skipping: [service1.com]
skipping: [service2.com]
skipping: [service3.com]
skipping: [service5.com]
skipping: [service4.com]
skipping: [web.com]
changed: [data1.com]
changed: [data2.com]

TASK: [retina | just some feedback here] **************************************
skipping: [broker1.com]
skipping: [broker2.com]
ok: [data1.com] => {
    "msg": "\"{u'changed': True, u'end': u'2015-02-24 07:35:43.737690', u'stdout': u'we just redeployed retinaTwo', u'cmd': u'echo \"we"
}
skipping: [service1.com]
ok: [data2.com] => {
    "msg": "\"{u'changed': True, u'end': u'2015-02-24 07:35:43.737632', u'stdout': u'we just redeployed retinaTwo', u'cmd': u'echo \"we"
}
skipping: [service2.com]
skipping: [broker3.com]
skipping: [service4.com]
skipping: [service3.com]
skipping: [service5.com]
skipping: [web.com]

NOTIFIED: [retina | restart retinaOne] ****************************************
ok: [web.com] => {
    "msg": "restarting retinaOne"
}

NOTIFIED: [retina | restart retinaTwo] ****************************************
ok: [data1.com] => {
    "msg": "restarting retinaTwo"
}
ok: [data2.com] => {
    "msg": "restarting retinaTwo"
}

PLAY RECAP ********************************************************************
broker1.com                : ok=1    changed=0    unreachable=0    failed=0
broker2.com                : ok=1    changed=0    unreachable=0    failed=0
broker3.com                : ok=1    changed=0    unreachable=0    failed=0
data1.com                  : ok=4    changed=1    unreachable=0    failed=0
data2.com                  : ok=4    changed=1    unreachable=0    failed=0
service1.com               : ok=1    changed=0    unreachable=0    failed=0
service2.com               : ok=1    changed=0    unreachable=0    failed=0
service3.com               : ok=1    changed=0    unreachable=0    failed=0
service4.com               : ok=1    changed=0    unreachable=0    failed=0
service5.com               : ok=1    changed=0    unreachable=0    failed=0
web.com                    : ok=4    changed=1    unreachable=0    failed=0
```