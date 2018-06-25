# Web stack

### Quick start

# Issue 1

## how to reproduce

```
openstack stack create -t web.yaml asg --wait
openstack stack output show asg scale_up_url
curl "https://heat.au-east-2.oc.company.com:8000/v1/signal/arn%3Aopenstack%3Aheat%3A%3Aab085c4458ef4aaf8fc4921f86a99094%3Astacks/asg/6b6d3598-b953-424c-bab7-7b9c8c2c6ff5/resources/web_server_scaleup_policy?Timestamp=2018-06-22T03%3A56%3A16Z&SignatureMethod=HmacSHA256&AWSAccessKeyId=937af7a45df74c66a86f3a761a2b9425&SignatureVersion=2&Signature=%2FW50inw%2Bh5fQDa%2B21DzUaSYHoohhgEAEvfEa%2FleD1bU%3D"
```

### Results

```
<ErrorResponse><Error><Message>User is not authorized to perform action</Message><Code>AccessDenied</Code><Type>Sender</Type></Error></ErrorResponse>⏎
```

### Expected:

Should succesfully scale up the instance

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

### Results

Returns 1

### Expected:

Should return > 1
