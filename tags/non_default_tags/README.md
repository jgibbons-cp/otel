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
  
OTEL Instrumented Application
--

```service:unified_service```. 
  
The tag comes from:  
  
```  
   metadata.labels  
     tags.datadoghq.com/service: "unified_service"  
  
   and
    
   spec.template.metadata.labels  
     tags.datadoghq.com/service: "unified_service"  
```
  
Datadog Instrumeted Application
--
  
If you want to test this, uncomment the ```command``` and ```args``` in the manifest. 

```service:unified_service```. 
  
The tag comes from:  
  
```  
        env:  
          - name: OTEL_RESOURCE_ATTRIBUTES  
            value: "service.name=unified_service"  
```  

