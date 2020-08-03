# CoAP Digital Twin Example - WhiteLabel Digital Twin Framework

This project shows how to implement a Digital Twin for an CoAP Device trough the WLDT Framework.

The CoAP-to-CoAP built-in IoT worker is dedicated to the seamless mirroring 
of standard CoAP physical objects. The CoAP protocol through the use of CoRE Link Format
and CoRE Interface provides by default both 
resource discovery and descriptions. It is possible for example to easily 
understand if a resource is a sensor or an actuator and which RESTful 
operations are allowed for an external client. 
Therefore, a WLDT instance can be automatically attached 
to a standard CoAP object without the need of any additional information. 
As illustrated in the following example, the class ***Coap2CoapWorker*** implements 
the logic to create and keep synchronized the two counterparts using standard 
methods and resource discovery through the use of ``/.well-known/core'' URI in 
order to retrieve the list of available resources and and mirror the corresponding digital replicas. 

Executed steps are: 

1) The WLDT instance sends a GET on the standard ``/.well-known/core'' URI in order to retrieve the list of available resources; 
2) the object responds with available resources using CoRE Link Format and CoRE Interface; 
3) the worker creates for the twin the digital copy of each resource keeping the same descriptive 
attributes (e.g., id, URI, interface type, resource type, observability etc ...); 
4) when an external client sends a CoAP Request to the WLDT it will forward the 
request to the physical object with the same attributes and payload (if present); 
5) the physical device handles the request and sends the response back to the digital 
replica that forwards the response to the requesting external client. 
The worker implements also a caching system that can be enabled through the 
***Coap2CoapConfiguration*** class together with the information related to 
the physical object network endpoint (IP address and ports for CoAP and CoAPs).

The following example shows a WLDT implementation using the built-in CoAP to CoAP worker to automatically 
create a digital twin of an existing CoAP physical object.

```java  
Coap2CoapConfiguration coapConf = new Coap2CoapConfiguration();
coapConf.setResourceDiscovery(true);
coapConf.setDeviceAddress("127.0.0.1");
coapConf.setDevicePort(5683);

WldtEngine wldtEngine = new WldtEngine(new WldtConfiguration());
wldtEngine.addNewWorker(new Coap2CoapWorker(coapConf));
wldtEngine.startWorkers();
```
