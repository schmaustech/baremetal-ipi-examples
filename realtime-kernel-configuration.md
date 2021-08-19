# Enable RealTime Kernel at Deployment Time

1. Create a MachineConfig that sets kernelType to realtime.

   99-worker-kerneltype.yaml:
    
    ```
    apiVersion: machineconfiguration.openshift.io/v1
    kind: MachineConfig
    metadata:
      labels:
        machineconfiguration.openshift.io/role: "worker"
      name: 99-worker-kerneltype
    spec:
      config:
        ignition:
          version: 3.1.0
      kernelType: realtime
    ```
    
2. After creating the MachineConfig, copy this file to $CLUSTERDIR/openshift

    ```
    cp 99-worker-kerneltype.yaml $CLUSTERDIR/openshift
