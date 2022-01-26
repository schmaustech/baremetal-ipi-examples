
# Enable LACP Bonding at Deployment Time (Network Manager)

1. Create the required network files:

ens3.nmconnection

```
[connection]
id=ens3
type=ethernet
interface-name=ens3
master=bond0
slave-type=bond
autoconnect=true

[ethernet]
mtu=9000
```

ens4.nmconnection

```
[connection]
id=ens4
type=ethernet
interface-name=ens4
master=bond0
slave-type=bond
autoconnect=true

[ethernet]
mtu=9000
```
bond0.nmconnection

```
[connection]
id=bond0
type=bond
interface-name=bond0
autoconnect=true
connection.autoconnect-slaves=1

[ethernet]
mtu=9000

[bond]
mode=802.3ad
miimon=100

[ipv4]
method=auto
dhcp-timeout=2147483647
never-default=true

[ipv6]
```

2. Get the base64 enconding of each file as follow

```
cat ens3.nmconnection | base64 -w0
cat ens4.nmconnection | base64 -w0
cat bond0.nmconnection | base64 -w0
```
3. Create the MachineConfig yaml to include in the manifests. Ensure to add the base64 to the respective file section.

11-worker-bond0.yaml:

```
---
apiVersion: machineconfiguration.openshift.io/v1
kind: MachineConfig
metadata:
  labels:
    machineconfiguration.openshift.io/role: worker
  name: 11-worker-bond0
spec:
  config:
    ignition:
      version: 3.2.0 
    storage:
      files:
      - contents:
          source: data:;base64,W2Nvbm5lY3Rpb25dCmlkPWJvbmQwCnR5cGU9Ym9uZAppbnRlcmZhY2UtbmFtZT1ib25kMAphdXRvY29ubmVjdD10cnVlCmNvbm5lY3Rpb24uYXV0b2Nvbm5lY3Qtc2xhdmVzPTEKCltldGhlcm5ldF0KbXR1PTkwMDAKCltib25kXQptb2RlPTgwMi4zYWQKbWlpbW9uPTEwMAoKW2lwdjRdCm1ldGhvZD1kaXNhYmxlZApkaGNwLXRpbWVvdXQ9MjE0NzQ4MzY0NwoKW2lwdjZdCg==
        path: /etc/NetworkManager/system-connections/bond0.nmconnection
        filesystem: root
        mode: 0600
      - contents:
          source: data:;base64,W2Nvbm5lY3Rpb25dCmlkPWVuczMKdHlwZT1ldGhlcm5ldAppbnRlcmZhY2UtbmFtZT1lbnMzCm1hc3Rlcj1ib25kMApzbGF2ZS10eXBlPWJvbmQKYXV0b2Nvbm5lY3Q9dHJ1ZQoKW2V0aGVybmV0XQptdHU9OTAwMAo= 
        path: /etc/NetworkManager/system-connections/ens3.nmconnection
        filesystem: root
        mode: 0600
      - contents:
          source: data:;base64,W2Nvbm5lY3Rpb25dCmlkPWVuczQKdHlwZT1ldGhlcm5ldAppbnRlcmZhY2UtbmFtZT1lbnM0Cm1hc3Rlcj1ib25kMApzbGF2ZS10eXBlPWJvbmQKYXV0b2Nvbm5lY3Q9dHJ1ZQoKW2V0aGVybmV0XQptdHU9OTAwMAo= 
        path: /etc/NetworkManager/system-connections/ens4.nmconnection
        filesystem: root
        mode: 0600
    systemd:
      units:
      - contents: |-
          [Unit]
          Description=Set bond client-id to the first port MAC each
          Before=ovs-configuration.service network-online.target kubelet.service crio.service node-valid-hostname.service NetworkManager.service
          
          [Service]
          Type=oneshot
          ExecStart=/usr/local/bin/set-client-id.sh
          
          [Install]
          WantedBy=network-online.target
        enabled: true
        name: set-client-id.service
```

4. After creating the MachineConfig file, copy this file to $CLUSTERDIR/openshift

```
cp 11-worker-bond0.yaml $CLUSTERDIR/openshift
```

# Enable LACP Bonding at Deployment Time (Legacy Network)

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

3. Create the MachineConfig yaml to include in the manifests. Ensure to add the base64 to the respective section.

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
