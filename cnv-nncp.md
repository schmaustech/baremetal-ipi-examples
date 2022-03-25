


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
