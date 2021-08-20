# Day 2 NNCP Policy Examples

Example of creating a bridge off of vlan 1044 on bond0 interface:

~~~bash
apiVersion: nmstate.io/v1alpha1
kind: NodeNetworkConfigurationPolicy
metadata:
  name: worker-br-ext2-bond0-vlan1044
spec:
  nodeSelector:
    node-role.kubernetes.io/worker: ""
  desiredState:
    interfaces:
      - name: bond0.1044
        type: vlan
        state: up
        vlan:
          base-iface: bond0
          id: 1044
      - name: br-ext2
        description: br-ext2 with bond0.1044
        type: linux-bridge
        state: up
        ipv4:
          enabled: false
        bridge:
          options:
            stp:
              enabled: false
          port:
            - name: bond0.1044
~~~

Example of bridge off of another physical interface on all workers:

~~~bash
apiVersion: nmstate.io/v1alpha1
kind: NodeNetworkConfigurationPolicy
metadata:
  name: br-ext2
spec:
  nodeSelector:
    node-role.kubernetes.io/worker: ""
  desiredState:
    interfaces:
      - name: br-ext2
        description: Linux bridge with ens10 as a port
        type: linux-bridge
        state: up
        ipv4:
          enabled: false
        bridge:
          options:
            stp:
              enabled: false
          port:
            - name: ens10
~~~

Example of static IP off another physical interface on worker-0:

~~~bash
apiVersion: nmstate.io/v1beta1
kind: NodeNetworkConfigurationPolicy
metadata:
  name: static-ip
spec:
  nodeSelector:
    kubernetes.io/hostname: worker-0
  desiredState:
    interfaces:
    - name: ens10
      type: ethernet
      state: up
      ipv4:
        address:
        - ip: 192.168.1.20
          prefix-length: 24
        dhcp: false
        enabled: true
~~~
