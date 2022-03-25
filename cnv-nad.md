# CNV Network Attachment Definition Example

This will enable a cnv-bridge of type tuning using brext bridge to enable VMs to connect to that bridge:

~~~bash
apiVersion: "k8s.cni.cncf.io/v1"
kind: NetworkAttachmentDefinition
metadata:
  name: brext
  annotations:
    k8s.v1.cni.cncf.io/resourceName: bridge.network.kubevirt.io/brext
spec:
  config: '{
    "cniVersion": "0.3.1",
    "name": "brext",
    "plugins": [
      {
        "type": "cnv-bridge",
        "bridge": "brext"
      },
      {
        "type": "tuning"
      }
    ]
  }'
~~~
