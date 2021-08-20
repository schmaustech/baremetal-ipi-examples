
~~~bash
apiVersion: v1beta4
baseDomain: schmaustech.com
metadata:
  name: kni20
networking:
  networkType: OVNKubernetes
  machineCIDR: 192.168.0.0/24
compute:
- name: worker
  replicas: 2
controlPlane:
  name: master
  replicas: 3
  platform:
    baremetal: {}
platform:
  baremetal:
    provisioningNetworkCIDR: 172.22.0.0/24
    provisioningNetworkInterface: enp1s0
    apiVIP: 192.168.0.220
    ingressVIP: 192.168.0.221
    dnsVIP: 192.168.0.222
    bootstrapOSImage: http://192.168.0.246/images/rhcos-48.84.202106091622-0-qemu.x86_64.qcow2.gz?sha256=17868a1963ae2eae25fb16cb85871e08758066f5c5ee87276201c377cf25e418
    clusterOSImage: http://192.168.0.246/images/rhcos-48.84.202106091622-0-openstack.x86_64.qcow2.gz?sha256=6ab5c6413f275277ea90f7dfc66424ef14993941ba3a9f3a43955ab268e7d76d
    hosts:
      - name: master-0
        role: master
        bmc:
          address: ipmi://192.168.0.10:6221
          username: admin
          password: password
        bootMACAddress: 52:54:00:7e:08:45
        hardwareProfile: default
        bootMode: legacy
      - name: master-1
        role: master
        bmc:
          address: ipmi://192.168.0.10:6222
          username: admin
          password: password
        bootMACAddress: 52:54:00:b4:ce:9e 
        hardwareProfile: default
        bootMode: legacy
      - name: master-2
        role: master
        bmc:
          address: ipmi://192.168.0.10:6223
          username: admin
          password: password
        bootMACAddress: 52:54:00:70:2e:ed
        hardwareProfile: default
        bootMode: legacy
      - name: worker-0
        role: master
        bmc:
          address: ipmi://192.168.0.10:6224
          username: admin
          password: password
        bootMACAddress: 52:54:00:30:8d:61
        hardwareProfile: unknown
        bootMode: UEFISecureBoot
      - name: worker-1
        role: master
        bmc:
          address: ipmi://192.168.0.10:6225
          username: admin
          password: password
        bootMACAddress: 52:54:00:aa:8a:a8
        hardwareProfile: unknown
        bootMode: UEFISecureBoot
pullSecret: 'PULLSECRET-REDACTED'
sshKey: 'SSH-KEY-REDACTED'
imageContentSources:
- mirrors:
  - rhel8-ocp-auto.schmaustech.com:5000/ocp4/openshift4
  source: quay.io/openshift-release-dev/ocp-v4.0-art-dev
- mirrors:
  - rhel8-ocp-auto.schmaustech.com:5000/ocp4/openshift4
  source: registry.svc.ci.openshift.org/ocp/release
additionalTrustBundle: |
  -----BEGIN CERTIFICATE-----
  M22F7zCCA9egAwIBAgIUeecEs+U5psgJ0aFgc4Q5dGVrAFcwDQYJKoZIhvcNAQEL
  BQAwgYYxCzAJBgNVBAYTAlVTMRYwFAYDVQQIDA1Ob3J0aENhcm9saW5hMRAwDgYD
  VQQHDAdSYWxlaWdoMRAwDgYDVQQKDAdSZWQgSGF0MRIwEAYDVQQLDAlNYXJrZXRp
  bmcxJzAlBgNVBAMMHnJoZWw4LW9jcC1hdXRvLnNjaG1hdXN0ZWNoLmNvbTAeFw0y
  MTA2MDkxMDM5MDZaFw0yMjA2MDkxMDM5MDZaMIGGMQswCQYDVQQGEwJVUzEWMBQG
  A1UECAwNTm9ydGhDYXJvbGluYTEQMA4GA1UEBwwHUmFsZWlnaDEQMA4GA1UECgwH
  UmVkIEhhdDESMBAGA1UECwwJTWFya2V0aW5nMScwJQYDVQQDDB5yaGVsOC1vY3At
  YXV0by5zY2htYXVzdGVjaC5jb20wggIiMA0GCSqGSIb3DQEBAQUAA4ICDwAwggIK
  AoICAQC9exAg3Ie3N3mkrQKseyri1VP2IPTc+pUEiVCPisIQAhRUfHhPR1HT7EF7
  SwaxrWjpfh9aYBPDEF3uLFQvzDEJWCh5PF55jwn3aABFGKEhfVBKd+es6nXnYaCS
  8CgLS2qM9x4WiuZxrntfB16JrjP+CrTvlAbE4DIMlDQLgh8+hDw9VPlbzY+MI+WC
  cYues1Ne+JZ5dZcKmCZ3zrVToPjreWZUuhSygci2xIQZxwWNmTvAgi+CAiQZS7VF
  RmKjj2H/o/d3I+XSS2261I8aXCAw4/3vaM9aci0eHeEhLIMrhv86WycOjcYL1Z6R
  n55diwDTSyrTo/B4zsQbmYUc8rP+pR2fyRJEGFVJ4ejcj2ZF5EbgUKupyU2gh/qt
  QeYtJ+6uAr9S5iQIcq9qvD9nhAtm3DnBb065X4jVPl2YL4zsbOS1gjoa6dRbFuvu
  f3SdsbQRF/YJWY/7j6cUaueCQOlXZRNhbQQHdIdBWFObw0QyyYtI831ue1MHPG0C
  nsAriPOkRzBBq+BPmS9CqcRDGqh+nd9m9UPVDoBshwaziSqaIK2hvfCAVb3BPJES
  CXKuIaP2IRzTjse58aAzsRW3W+4e/v9fwAOaE8nS7i+v8wrqcFgJ489HnVq+kRNc
  VImv5dBKg2frzXs1PpnWkE4u2VJagKn9B2zva2miRQ+LyvLLDwIDAQABo1MwUTAd
  BgNVHQ4EFgQUbcE9mpTkOK2ypIrURf+xYR08OAAwHwYDVR0jBBgwFoAUbcE9mpTk
  OK2ypIrURf+xYR08OAAwDwYDVR0TAQH/BAUwAwEB/zANBgkqhkiG9w0BAQsFAAOC
  AgEANTjx04NoiIyw9DyvszwRdrSGPO3dy1gk3jh+Du6Dpqqku3Mwr2ktaSCimeZS
  4zY4S5mRCgZRwDKu19z0tMwbVDyzHPFJx+wqBpZKkD1FvOPKjKLewtiW2z8AP/kF
  gl5UUNuwvGhOizazbvd1faQ8jMYoZKifM8On6IpFgqXCx98/GOWvnjn2t8YkMN3x
  blKVm5N7eGy9LeiGRoiCJqcyfGqdAdg+Z+J94AHEZb3OxG8uHLrtmz0BF3A+8V2H
  hofYI0spx5y9OcPin2yLm9DeCwWAA7maqdImBG/QpQCjcPW3Yzz9VytIMajPdnvd
  vbJF5GZNj7ods1AykCCJjGy6n9WCf3a4VLnZWtUTbtz0nrIjJjsdlXZqby5BCF0G
  iqWbg0j8onl6kmbMAhssRTlvL8w90F1IK3Hk+lz0Qy8rqZX2ohObtEYGMIAOdFJ1
  iPQrbksXOBpZNtm1VAved41sYt1txS2WZQgfklIXOhNOu4r32ZGKas4EJml0l0wL
  2P65PkPEa7AOeqwP0y6eGoNG9qFSl+yArycZGWudp88977H6CcdkdEcQzmjg5+TD
  9GHm3drUYGqBJDvIemQaXfnwy9Gxx+oBDpXLXOuo+edK812zh/q7s2FELfH5ZieE
  Q3dIH8UGsnjYxv8G3O23cYKZ1U0iiu9QvPRFm0F8JuFZqLQ=
  -----END CERTIFICATE-----
~~~
