# Enable Hybrid Network Configuration at Deployment Time

1. Create the required manifest file below and note that Hybrid Network Configuration can only be done at deploytime and cannot be done as a day 2 operation.

   cluster-network-03-config.yml:
    
    ```
    apiVersion: operator.openshift.io/v1
    kind: Network
    metadata:
      name: cluster
    spec:
      defaultNetwork:
        ovnKubernetesConfig:
          hybridOverlayConfig:
            hybridClusterNetwork: 
            - cidr: 10.132.0.0/14
              hostPrefix: 23
            hybridOverlayVXLANPort: 9898 
    ```
    
2. After creating the installation manifests, copy this file to $CLUSTERDIR/manifest

    ```
    cp cluster-network-03-config.yml $CLUSTERDIR/manifest
