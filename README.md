# Web stack

### Quick start

# Issue 1

## how to reproduce

```
openstack stack create -t web.yaml asg --wait
openstack stack output show asg scale_up_url
```

###Results

Returns the internal heat endpoint (heat-internal.au-east..)

###Expected:

Should return the external reachable endpoint

# Issue 2

## how to reproduce

1. Deploy stack
```
openstack stack create -t web.yaml asg --wait #manually assign a fip to the load balancer
```

2. Create load
```
wrk -t12 -c400 -d30s http://10.243.196.91
```

3. Check that the stack has scaled after 30s
```
 openstack stack output show asg current_size
```

###Results

Returns 1

###Expected:

Should return > 1

# Issue 3

1. Deploy stack (as a user)
```
openstack stack create -t web.yaml asg --wait #manually assign a fip to the load balancer
```

###Results

Error
```
2018-05-21 04:26:34Z [asg]: CREATE_IN_PROGRESS  Stack CREATE started
2018-05-21 04:26:34Z [asg.private_net]: CREATE_IN_PROGRESS  state changed
2018-05-21 04:26:35Z [asg.private_net]: CREATE_COMPLETE  state changed
2018-05-21 04:26:35Z [asg.subnet]: CREATE_IN_PROGRESS  state changed
2018-05-21 04:26:35Z [asg.allow_icmp]: CREATE_IN_PROGRESS  state changed
2018-05-21 04:26:36Z [asg.allow_icmp]: CREATE_COMPLETE  state changed
2018-05-21 04:26:36Z [asg.subnet]: CREATE_COMPLETE  state changed
2018-05-21 04:26:36Z [asg.allow_ssh]: CREATE_IN_PROGRESS  state changed
2018-05-21 04:26:37Z [asg.lb]: CREATE_IN_PROGRESS  state changed
2018-05-21 04:26:37Z [asg.allow_ssh]: CREATE_COMPLETE  state changed
2018-05-21 04:26:37Z [asg.jumpbox]: CREATE_IN_PROGRESS  state changed
2018-05-21 04:26:37Z [asg.allow_web]: CREATE_IN_PROGRESS  state changed
2018-05-21 04:26:38Z [asg.allow_web]: CREATE_COMPLETE  state changed
2018-05-21 04:26:38Z [asg.router]: CREATE_IN_PROGRESS  state changed
2018-05-21 04:26:38Z [asg.lb]: CREATE_COMPLETE  state changed
2018-05-21 04:26:39Z [asg.listener]: CREATE_IN_PROGRESS  state changed
2018-05-21 04:26:43Z [asg.router]: CREATE_COMPLETE  state changed
2018-05-21 04:26:43Z [asg.router_interface]: CREATE_IN_PROGRESS  state changed
2018-05-21 04:26:45Z [asg.router_interface]: CREATE_COMPLETE  state changed
2018-05-21 04:26:47Z [asg.listener]: CREATE_COMPLETE  state changed
2018-05-21 04:26:47Z [asg.pool]: CREATE_IN_PROGRESS  state changed
2018-05-21 04:26:50Z [asg.jumpbox]: CREATE_COMPLETE  state changed
2018-05-21 04:26:50Z [asg.pool]: CREATE_COMPLETE  state changed
2018-05-21 04:26:50Z [asg.asg]: CREATE_IN_PROGRESS  state changed
2018-05-21 04:27:11Z [asg.asg]: CREATE_COMPLETE  state changed
2018-05-21 04:27:11Z [asg.web_server_scaledown_policy]: CREATE_IN_PROGRESS  state changed
2018-05-21 04:27:11Z [asg.web_server_scaleup_policy]: CREATE_IN_PROGRESS  state changed
2018-05-21 04:27:11Z [asg.web_server_scaleup_policy]: CREATE_COMPLETE  state changed
2018-05-21 04:27:12Z [asg.web_server_scaledown_policy]: CREATE_COMPLETE  state changed
2018-05-21 04:27:12Z [asg.cpu_alarm_high]: CREATE_IN_PROGRESS  state changed
2018-05-21 04:27:12Z [asg.cpu_alarm_low]: CREATE_IN_PROGRESS  state changed
2018-05-21 04:27:15Z [asg.cpu_alarm_high]: CREATE_FAILED  ClientException: resources.cpu_alarm_high: You are not authorized to perform the requested action: identity:list_projects. (HTTP 403) (Request-ID: req-8e5abb42-7331-4421-9e4d-0b4cee5ecb04) (HTTP 500) (Request-ID: req-794698c1-7b80-44f2-a3ec-b822bd628ab3)
2018-05-21 04:27:15Z [asg]: CREATE_FAILED  Resource CREATE failed: ClientException: resources.cpu_alarm_high: You are not authorized to perform the requested action: identity:list_projects. (HTTP 403) (Request-ID: req-8e5abb42-7331-4421-9e4d-0b4cee5ecb04) (HTTP 500) (Request-ID: req-794698c1-7b80-
2018-05-21 04:27:15Z [asg.cpu_alarm_low]: CREATE_FAILED  ClientException: resources.cpu_alarm_low: You are not authorized to perform the requested action: identity:list_projects. (HTTP 403) (Request-ID: req-f333a159-8777-407b-aca0-2d0bb182dc97) (HTTP 500) (Request-ID: req-11e46e80-5d85-4c0a-b5d3-bd4407e91868)
2018-05-21 04:27:15Z [asg]: CREATE_FAILED  Resource CREATE failed: ClientException: resources.cpu_alarm_low: You are not authorized to perform the requested action: identity:list_projects. (HTTP 403) (Request-ID: req-f333a159-8777-407b-aca0-2d0bb182dc97) (HTTP 500) (Request-ID: req-11e46e80-5d85-4

 Stack asg CREATE_FAILED
```

###Expected:

Deploy succesfully
