# Enable Partitioning at Deployment Time

1. Create a MachineConfig that sets 100GB /var partition on /dev/sdb.

   98-var-partition.yaml:
    
    ```
    apiVersion: machineconfiguration.openshift.io/v1
    kind: MachineConfig
    metadata:
      labels:
        machineconfiguration.openshift.io/role: worker
      name: 98-var-partition
    spec:
      config:
        ignition:
          version: 3.2.0
        storage:
          disks:
          - device: /dev/sdb 
            partitions:
            - sizeMiB: 100000
              startMiB: 0
             label: var
          filesystems:
            - path: /var
              device: /dev/disk/by-partlabel/var
              format: xfs
        systemd:
          units:
            - name: var.mount
              enabled: true
              contents: |
                [Unit]
                Before=local-fs.target
                [Mount]
                Where=/var
                What=/dev/disk/by-partlabel/var
                [Install]
                WantedBy=local-fs.target
    ```
    
2. After creating the MachineConfig, copy this file to $CLUSTERDIR/openshift

    ```
    cp 98-var-partition.yaml $CLUSTERDIR/openshift
