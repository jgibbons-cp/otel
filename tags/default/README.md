Default Tags
--

The manifest ```default-tags.yaml``` has no tags set in it.  Therefore, it will use the default values Datadog assigns.  

Logs
--

```service: hello_otel_python```  
```source: hello_otel_python```  
  
Come from ```short_image:hello_otel_python``` by default
  
The log source is what assigns it to an integration pipeline to parse.  There is no integration for ```hello_otel_python``` therefore the log will not be parsed.  
  
APM
--

```service:unknown_service```
  
So, let's set them.  For reference, see [Universal Service Tagging](https://docs.datadoghq.com/getting_started/tagging/unified_service_tagging/?tab=kubernetes).  