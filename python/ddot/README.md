Python Auto-Instrumentation
--

Simple Python Flask application auto-instrumented with OTEL.  
  
1) Dockerfile.otel - Dockerfile  
2) sample_flask_app_otel.py - simple, sample Flask application  
3) sample_flask_app_otel.yaml - Kubernetest manifest.  This can be used for OTEL or Datadog instrumentation as you can see by the tags.  You will need to change the image however as you can't instrument both at the same time and the current app is instrumented with OTEL - see Dockerfile.otel  So, if the application is instrumented with OTEL any automation for Datadog instrumntation will be inactive  
4) values_ddot.yaml - values file for the Datadog agent using the embedded OTEL collector/Datadog exporter  
5) collector-config.yaml - default collector/exporter however separated out as an example of how you pass a custom collector/exporter  
  
Agent, with embedded OTEL Collector/Datadog Exporter Deployment
--

1) Unless you plan on installing in the default namespace, create one:  
  
```  
   export NAMESPACE=<NAMESPACE>
   kubectl create ns $NAMESPACE  
```  
  
2) Create a secret:  
  
``` 
   # [API](https://app.datadoghq.com/organization-settings/api-keys)
   # and [application](https://app.datadoghq.com/organization-settings/application-keys) keys  
   export API_KEY=<API_KEY>  
   export APP_KEY=<APP_KEY>  
   kubectl create secret generic datadog-secret\  
      --from-literal api-key=$API_KEY\  
      --from-literal app-key=$APP_KEY -n $NAMESPACE  
```  
  
3) Deploy - tested on EKS on EC2  
  
```  
   helm upgrade dd-agent -f values_ddot.yaml\
     --set-file datadog.otelCollector.config=collector-config.yaml\
    datadog/datadog -i -n $NAMESPACE
```  
  
Deploy the application
--

1) Create a namespace if it will be deployed outside of ```default```  
  
```  
   export APP_NAMESPACE=<NAMESPACE>  
   kubectl create ns $APP_NAMESPACE  
```  
  
2) Edit the environment variable value for ```SERVICE_NAMESPACE``` in 
   ```sample_flask_app_otel.yaml``` if the agent is installed in a 
   namespace other then ```default```  

3) Deploy  
  
```kubectl apply -f sample_flask_app_otel.yaml -n $APP_NAMESPACE```  
  
Access the Application
--
  
1) Use a port-forward  
  
```kubectl port-forward deploy/sample-flask-app 33333:81 -n $APP_NAMESPACE```  
  
2) Navigate to it  
  
```http://localhost:33333```  
  
Access Traces
--
  
1) Trace Explorer in Datadog - OTEL traces  
  
```https://app.datadoghq.com/apm/traces```  
  
Access Traces - Datadog traces
--

1) This is for example only, you should not be able to overwrite the `CMD` in the 
container.  
  
In ```spec.template.spec.containers``` add the following under ```image:```:  
  
```  
        command: ["python3"]  
        args: ["sample_flask_app_otel.py"]  
```  
  
Hit is the same as above and see dd traces from the same image.  To switch, rebuild the container removing the OTEL instrumentation and the way the agent is configured it will auto-instrument.  
  
```  
CMD ["python3", "sample_flask_app_otel.py"]  
```  
  


  
