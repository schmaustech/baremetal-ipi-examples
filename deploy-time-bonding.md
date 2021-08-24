# Enable LACP Bonding at Deployment Time

1. Create the required network files.

   ifcfg-bond0:
    
    ```
    DEVICE=bond0
    TYPE=Bond
    NAME=bond0
    BONDING_MASTER=yes
    BOOTPROTO=dhcp
    ONBOOT=yes
    BONDING_OPTS="mode=4 miimon=100"
    ```
    
    ifcfg-ens3:
    
    ```
    TYPE=Ethernet
    DEVICE=ens3
    BOOTPROTO=none
    ONBOOT=yes
    MASTER=bond0
    SLAVE=yes

    ```
    
    ifcfg-ens4:
    
    ```
    TYPE=Ethernet
    DEVICE=ens4
    BOOTPROTO=none
    ONBOOT=yes
    MASTER=bond0
    SLAVE=yes
    ```
 
2. Get the base64 enconding of each file as follow

    ```
    cat ifcfg-bond0 | base64 -w0
    cat ifcfg-ens3 | base64 -w0
    cat ifcfg-ens4 | base64 -w0
    ```

3. Create the machineconfig yaml to include in the manifests. Ensure to add the base64 to the respective section.

    11-master-bond0.yaml:

    ```
    apiVersion: machineconfiguration.openshift.io/v1
    kind: MachineConfig
    metadata:
      creationTimestamp: null
      labels:
        machineconfiguration.openshift.io/role: master
      name: 11-master-bond0
    spec:
      config:
        ignition:
          config: {}
          security:
            tls: {}
          timeouts: {}
          version: 2.2.0
        networkd: {}
        passwd: {}
        storage: {
                "files": [
                    {
                        "path": "/etc/sysconfig/network-scripts/ifcfg-bond0",
                        "filesystem": "root",
                        "mode": 420,
                        "contents": {
                        "source": "data:;base64,<BASE 64 ENCODING>"
                        }
                    },
                    {
                        "path": "/etc/sysconfig/network-scripts/ifcfg-ens3",
                        "filesystem": "root",
                        "mode": 420,
                        "contents": {
                        "source": "data:;base64,<BASE 64 ENCODING>"
                        }
                    },
                    {
                        "path": "/etc/sysconfig/network-scripts/ifcfg-ens4",
                        "filesystem": "root",
                        "mode": 420,
                        "contents": {
                        "source": "data:;base64,<BASE 64 ENCODING>"
                        }
                    }     
                ]
            }
    ```
    
4. After creating the MachineConfig file, copy this file to $CLUSTERDIR/openshift

    ```
    cp 11-master-bond0.yaml $CLUSTERDIR/openshift
