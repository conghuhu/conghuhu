# Google Summer of Code 2022: Dubbo Thin SDK

## Project Goal

The overall goal is to modify the Dubbo SDK to work in mesh mode and streamline unnecessary processes. 

## My Contributions

1. For service invocation, add the meshEnable configuration in ConsumerConfig to enable the mesh mode and add the providerPort in ReferenceConfig to specify the provider's service port. Modify the SDK, read the providerBy and providerPort in the configuration to determine the address of sending RPC. In the mesh environment, providerBy should be the serviceName of the provider. What's more, add the the unloadClusterRelated in ReferenceConfig to configure whether to unload the useless cluster related functions, such as load balancing and retries, to strip these traffic governance capabilities from Dubbo and sink them into sidecar.
2. For health checks, Istio comes with probes that align the life cycle of Dubbo and even the entire application with the life cycle of Pod. An active health check provided by Envoy removes unhealthy instances immediately and then joins the service route when the instance is healthy again. However, the readinessProbe implementation provided by Dubbo does not apply to mesh. If the registry is not configured in mesh mode, Dubbo's readinessProbe returns false, which needs to be modified here. 
3. Write samples of Dubbo mesh to guide users to quickly learn Dubbo mesh mode.
4. As the keynote speaker of Dubbo micro service technology live broadcast service mesh practice: teach you how to deploy Dubbo to istio, he explained the deployment and principle of Dubbo 3 mesh to Dubbo developers.

## My Works Links

