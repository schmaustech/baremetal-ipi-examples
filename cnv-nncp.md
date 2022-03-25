# CNV NNCP Policy Example

This example will create a bridge brext off the interface of eno2 on every node in the cluster and give it an ipaddress of 192.168.0.2 (which is okay because these interfaces are not connected at the switch):

~~~bash
apiVersion: nmstate.io/v1
kind: NodeNetworkConfigurationPolicy
metadata:
  name: master-brext-eno2
spec:
  desiredState:
    interfaces:
      - name: brext
        description: brext with eno2
        type: linux-bridge
        state: up
        ipv4:
          address:
          - ip: 192.168.0.2
            prefix-length: 24
          enabled: true
        bridge:
          options:
            stp:
              enabled: false
          port:
            - name: eno2
~~~
