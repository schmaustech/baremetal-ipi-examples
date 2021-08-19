# Configure MTU, Geneve Port and/or IPSec at Deployment Time

1. Create the required manifest file below and note that IPSec can only be done at deploytime and cannot be done as a day 2 operation.

   cluster-network-03-config.yml:
    
    ```
    apiVersion: operator.openshift.io/v1
    kind: Network
    metadata:
      name: cluster
    spec:
      defaultNetwork:
        ovnKubernetesConfig:
          mtu: 1400
          genevePort: 6081
          ipsecConfig: {}
    ```
    
2. After creating the installation manifests, copy this file to $CLUSTERDIR/manifest

    ```
    cp cluster-network-03-config.yml $CLUSTERDIR/manifest
