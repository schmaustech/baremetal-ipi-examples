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


3. After installation of cluster we can verify the partition was set correctly:

    ```
    $ oc debug node/worker-0.kni20.schmaustech.com
    Starting pod/worker-0kni20schmaustechcom-debug ...
    To use host binaries, run `chroot /host`
    Pod IP: 192.168.0.213
    If you don't see a command prompt, try pressing enter.
    sh-4.4# chroot /host
    sh-4.4# lsblk
    NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    sda      8:0    0   120G  0 disk 
    |-sda1   8:1    0     1M  0 part 
    |-sda2   8:2    0   127M  0 part 
    |-sda3   8:3    0   384M  0 part /boot
    |-sda4   8:4    0 119.4G  0 part /sysroot
    `-sda5   8:5    0    65M  0 part 
    sdb      8:16   0   120G  0 disk 
    `-sdb1   8:17   0  97.7G  0 part /var
    sr0     11:0    1   913M  0 rom  
    sh-4.4# exit 
    exit
    sh-4.4# exit
    exit
    ```
