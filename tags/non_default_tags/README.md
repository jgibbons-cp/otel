Non-Default Tags
--

The manifest ```non-default-tags.yaml``` uses Datadog as well as an OTEL SDK tag.

OTEL and Datadog Log Tags
--

```service:unified_service```  
```source:python```  
  
The tags come from:  
  
```  
      annotations:  
        ad.datadoghq.com/container-one.logs: '[{"service":"unified-service", "source":"python"}]'
```  
  
The log will be parsed by the python pipeline.  
  
APM
--  

1) OTEL Instrumented Application  
  
I did not expose a port.  The server runs on 81.  To generate traces either expose the port and use a port-forward or:  
  
```  
$ kubectl exec -it non-default-tags-68f8f964cf-bn4ph -- bash  
Defaulted container "container-one" out of: container-one, container-two, datadog-init-apm-inject (init), datadog-lib-python-init (init)  
root@non-default-tags-68f8f964cf-bn4ph:/app# apt update  
...  
...  
root@non-default-tags-68f8f964cf-bn4ph:/app# apt install curl -y  
...  
...  
root@non-default-tags-68f8f964cf-bn4ph:/app# curl localhost:81  
Web App with Python Flask!root@non-default-tags-68f8f964cf-bn4ph:/app#  
``` 
  
```service:unified_service```. 
  
The tag comes from:  
  
```  
        env:  
          - name: OTEL_RESOURCE_ATTRIBUTES  
            value: "service.name=unified_service"  
```  

  
2) Datadog Instrumented Application  
  
If you want to test this, uncomment the ```command``` and ```args``` in the manifest (assumes you are using single-step instrumentation in your agent config). 

```service:unified_service```. 
  
The tag comes from:  
  
```  
   metadata.labels  
     tags.datadoghq.com/service: "unified_service"  
  
   and
    
   spec.template.metadata.labels  
     tags.datadoghq.com/service: "unified_service"  
```

