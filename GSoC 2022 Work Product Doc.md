# Google Summer of Code 2022: Dubbo Thin SDK

![image](https://user-images.githubusercontent.com/56248584/202880083-0961edfa-0bee-498d-8f01-a6be834a79c8.png)



## Project Goal

The overall goal is to modify the Dubbo SDK to work in mesh mode and streamline unnecessary processes. 
At present, Dubbo and Service Mesh duplicate many common traffic management functions, such as load balancing, current limiting, and grayscale.

In Dubbo Mesh mode, Dubbo should retain the most basic RPC functions, and integrate these traffic governance capabilities into Sidecar.

Detail proposals is [here](https://github.com/apache/dubbo-awesome/blob/master/proposals/D3.1-thinsdk-sidecar-mesh.md#background).

## My Contributions

1. For service invocation, add the meshEnable configuration in ConsumerConfig to enable the mesh mode and add the providerPort in ReferenceConfig to specify the provider's service port. Modify the SDK, read the providerBy and providerPort in the configuration to determine the address of sending RPC. In the mesh environment, providerBy should be the serviceName of the provider.
2. Add the the unloadClusterRelated in ReferenceConfig to configure whether to unload the useless cluster related functions, such as load balancing and retries, to strip these traffic governance capabilities from Dubbo and sink them into sidecar.
3. For health checks, Istio comes with probes that align the life cycle of Dubbo and even the entire application with the life cycle of Pod. An active health check provided by Envoy removes unhealthy instances immediately and then joins the service route when the instance is healthy again. However, the readinessProbe implementation provided by Dubbo does not apply to mesh. If the registry is not configured in mesh mode, Dubbo's readinessProbe returns false, which needs to be modified here. 
4. Write samples of Dubbo mesh to guide users to quickly learn Dubbo mesh mode.
5. Replace kube-apiserver watch with informer. The current implementation of syncing endpoints, pods, services information is to use watch api. This is not as good as using the informer's list-watch.
6. As the keynote speaker of Dubbo micro service technology live broadcast service mesh practice: [teach you how to deploy Dubbo to Istio](https://www.bilibili.com/video/BV1HV4y1x7A9?spm_id_from=333.999.0.0&vd_source=6668b7f3025d083b8b0b10851dd834a8), I explained the deployment and principle of Dubbo 3 mesh to Dubbo developers.

## My Works Links

### Main Repository Pull Request

|  PR   | Description  |
|  ----  | ----  |
| [#10329](https://github.com/apache/dubbo/pull/10329)  | perf: remove duplicate code snippets in AbstractInterfaceConfig |
| [#10356](https://github.com/apache/dubbo/pull/10356)  | feat: add mesh enable config and process url in mesh mode |
| [#10498](https://github.com/apache/dubbo/pull/10498)  | feat: add unloadClusterRelated in ReferenceConfig which can unload the useless cluster related functions |
| [#10543](https://github.com/apache/dubbo/pull/10543)  | feat: replace kube-apiserver watch with informer |

### Product Demo
Write [samples of Dubbo mesh](https://github.com/apache/dubbo-samples/tree/master/dubbo-samples-mesh-k8s) to guide users to quickly learn Dubbo mesh mode and push it to [dubbo-samples](https://github.com/apache/dubbo-samples).

The final deployment of Dubbo mesh is as follows:

![image](https://user-images.githubusercontent.com/56248584/188522535-43fb1f2f-bc02-4c6d-b0bb-a7710c8863f3.png)

#### Related PR
|  PR   | Description  |
|  ----  | ----  |
| [#463](https://github.com/apache/dubbo-samples/pull/463) | feat: add Dubbo samples mesh k8s |
| [#482](https://github.com/apache/dubbo-samples/pull/482) | feat: add envoy health checks |
| [#484](https://github.com/apache/dubbo-samples/pull/484) | feat: add mesh mode in dubbo-samples-mesh-k8s |
| [#498](https://github.com/apache/dubbo-samples/pull/498) | feat: add server stream and bi stream demos in mesh example && recover readinessProbe |

### Product Document
- [docs: add relevant configuration description of Service Mesh Sidecar](https://github.com/apache/dubbo-website/pull/1425)
- [dubbo-samples-mesh-k8s doc](https://github.com/apache/dubbo-samples/blob/master/dubbo-samples-mesh-k8s/README.md)

### Technology Sharing
- [Dubbo Mesh deployment](https://www.bilibili.com/video/BV1HV4y1x7A9?spm_id_from=333.999.0.0&vd_source=6668b7f3025d083b8b0b10851dd834a8)

## Next

I will continue to participate in Dubbo mesh related contributions. According to [the next plan of Dubbo mesh](https://mp.weixin.qq.com/s/GF8i1fzY1I4bDSJDZh6aJQ), I will continue to participate in the development of Dubbo control plane and data plane.

Thank you for being a part of this fantastic summer.

—— Guoqing Cong
