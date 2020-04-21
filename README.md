# Playbooks for simplify some taks with local-storage in OpenShift Container Storage

## Introduction 
Those ansible playbooks provides an intereactive menu for storage operations.

WARNING: Those playbooks are personal tools and don't recommended and supported in production clusters.

## What those playbook can do for you?
The playbooks will offer the following interactive menu:
    
    Hey there, what do you want to do?
    1=Listing OSD information deviceset|pv|pvc|host
    2=Replace a failed OSD (interactive)
    3=List device-byid for Create/Update loca-storage CR
    [0]:

## Testing environment
Following the cluster for testing the playbooks:
* OCP Cluster v4.3 with 3 masters
* IPI deployment (fuly automated with OpenShift machinesets)
* OCS v4.3 operator with loca-storage (Techpreview)
* OCS i3.8xlarge AWS instances with 4 local NVMes with 1.8TB size each


    [ctorres-redhat.com@clientvm 130 ~/deploy/tools/ocs-osd-manager]$ oc get machines
    NAME                                                   PHASE     TYPE         REGION         ZONE            AGE
    cluster-milano-9521-8xh7k-master-0                     Running   m5.xlarge    eu-central-1   eu-central-1a   3d6h
    cluster-milano-9521-8xh7k-master-1                     Running   m5.xlarge    eu-central-1   eu-central-1b   3d6h
    cluster-milano-9521-8xh7k-master-2                     Running   m5.xlarge    eu-central-1   eu-central-1c   3d6h
    cluster-milano-9521-8xh7k-worker-eu-central-1a-svlkq   Running   m5.4xlarge   eu-central-1   eu-central-1a   3d6h
    cluster-milano-9521-8xh7k-worker-eu-central-1b-g7lfr   Running   m5.4xlarge   eu-central-1   eu-central-1b   3d6h
    cluster-milano-9521-8xh7k-worker-eu-central-1c-r54sg   Running   m5.4xlarge   eu-central-1   eu-central-1c   3d5h
    ocs-worker-eu-central-1a-hlppq                         Running   i3.8xlarge   eu-central-1   eu-central-1a   48m
    ocs-worker-eu-central-1b-kfv4k                         Running   i3.8xlarge   eu-central-1   eu-central-1b   48m
    ocs-worker-eu-central-1c-kwfs6                         Running   i3.8xlarge   eu-central-1   eu-central-1c   48m

## Let's doing some testing

Use case: I want to discovery my local devices with device-byid path

    [ctorres-redhat.com@clientvm 130 ~/deploy/tools/ocs-osd-manager]$ ansible --version
    ansible 2.8.8
    config file = /etc/ansible/ansible.cfg
    configured module search path = [u'/home/ctorres-redhat.com/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
    ansible python module location = /usr/lib/python2.7/site-packages/ansible
    executable location = /usr/bin/ansible
    python version = 2.7.5 (default, May 31 2018, 09:41:32) [GCC 4.8.5 20150623 (Red Hat 4.8.5-28)]
    [ctorres-redhat.com@clientvm 0 ~/deploy/tools/ocs-osd-manager]$ ansible-playbook osd_manager.yml
    [WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'

    Hey there, what do you want to do?
    1=Listing OSD information deviceset|pv|pvc|host
    2=Replace a failed OSD (interactive)
    3=List device-byid for Create/Update loca-storage CR
    [0]: 3

    PLAY [Playbook for Ceph OSD mapping in OpenShift] ************************************************************************************************************************************************************************

    TASK [Gathering Facts] ***************************************************************************************************************************************************************************************************
    ok: [localhost]

    TASK [Get device->pv->pvc->host information] *****************************************************************************************************************************************************************************
    skipping: [localhost]

    TASK [debug] *************************************************************************************************************************************************************************************************************
    skipping: [localhost]

    TASK [Clean files] *******************************************************************************************************************************************************************************************************
    skipping: [localhost] => (item=./file_tmp_1)
    skipping: [localhost] => (item=./file_tmp_2)
    skipping: [localhost] => (item=./file_tmp_3)

    TASK [pause] *************************************************************************************************************************************************************************************************************
    skipping: [localhost]

    TASK [Check OSD status] **************************************************************************************************************************************************************************************************
    skipping: [localhost]

    TASK [Warning print OSD status up] ***************************************************************************************************************************************************************************************
    skipping: [localhost]

    TASK [Warning wrong osd] *************************************************************************************************************************************************************************************************
    skipping: [localhost]

    TASK [Check pg status] ***************************************************************************************************************************************************************************************************
    skipping: [localhost]

    TASK [pause] *************************************************************************************************************************************************************************************************************
    skipping: [localhost]

    TASK [Remove and clean osd {{ osd_id.user_input }}] **********************************************************************************************************************************************************************
    skipping: [localhost]

    TASK [debug] *************************************************************************************************************************************************************************************************************
    skipping: [localhost]

    TASK [Clean files] *******************************************************************************************************************************************************************************************************
    skipping: [localhost] => (item=./file_tmp_1)
    skipping: [localhost] => (item=./file_tmp_2)
    skipping: [localhost] => (item=./file_tmp_3)

    TASK [Collect OCS workers] ***********************************************************************************************************************************************************************************************
    changed: [localhost]

    TASK [Collect device by-id and size] *************************************************************************************************************************************************************************************
    changed: [localhost]

    TASK [Collect device by-id] **********************************************************************************************************************************************************************************************
    changed: [localhost] => (item=ip-10-0-141-44.eu-central-1.compute.internal)
    changed: [localhost] => (item=ip-10-0-157-251.eu-central-1.compute.internal)
    changed: [localhost] => (item=ip-10-0-163-72.eu-central-1.compute.internal)

    TASK [Print to screen config file ./local-storage-block.yaml] ************************************************************************************************************************************************************
    changed: [localhost]

    TASK [debug] *************************************************************************************************************************************************************************************************************
    ok: [localhost] => {
        "msg": [
            "apiVersion: local.storage.openshift.io/v1",
            "kind: LocalVolume",
            "metadata:",
            "  name: local-block",
            "  namespace: local-storage",
            "spec:",
            "  nodeSelector:",
            "    nodeSelectorTerms:",
            "    - matchExpressions:",
            "        - key: cluster.ocs.openshift.io/openshift-storage",
            "          operator: In",
            "          values:",
            "          - \"\"",
            "  storageClassDevices:",
            "    - storageClassName: localblock",
            "      volumeMode: Block",
            "      devicePaths:",
            "        - /dev/disk/by-id/nvme-Amazon_EC2_NVMe_Instance_Storage_AWS1032495F0D628878C # nvme0n1    1769 GB    ip-10-0-141-44.eu-central-1.compute.internal",
            "        - /dev/disk/by-id/nvme-Amazon_EC2_NVMe_Instance_Storage_AWS6032495F0D628878C # nvme1n1    1769 GB    ip-10-0-141-44.eu-central-1.compute.internal",
            "        - /dev/disk/by-id/nvme-Amazon_EC2_NVMe_Instance_Storage_AWS1977293BF68DDE42D # nvme2n1    1769 GB    ip-10-0-141-44.eu-central-1.compute.internal",
            "        - /dev/disk/by-id/nvme-Amazon_EC2_NVMe_Instance_Storage_AWS6977293BF68DDE42D # nvme3n1    1769 GB    ip-10-0-141-44.eu-central-1.compute.internal",
            "        - /dev/disk/by-id/nvme-Amazon_EC2_NVMe_Instance_Storage_AWS1C2B4567DAD58239A # nvme0n1    1769 GB    ip-10-0-157-251.eu-central-1.compute.internal",
            "        - /dev/disk/by-id/nvme-Amazon_EC2_NVMe_Instance_Storage_AWS6C2B4567DAD58239A # nvme1n1    1769 GB    ip-10-0-157-251.eu-central-1.compute.internal",
            "        - /dev/disk/by-id/nvme-Amazon_EC2_NVMe_Instance_Storage_AWS1EDE35EF918811545 # nvme2n1    1769 GB    ip-10-0-157-251.eu-central-1.compute.internal",
            "        - /dev/disk/by-id/nvme-Amazon_EC2_NVMe_Instance_Storage_AWS6EDE35EF918811545 # nvme3n1    1769 GB    ip-10-0-157-251.eu-central-1.compute.internal",
            "        - /dev/disk/by-id/nvme-Amazon_EC2_NVMe_Instance_Storage_AWS10FDCEE1E2D1EC57B # nvme0n1    1769 GB    ip-10-0-163-72.eu-central-1.compute.internal",
            "        - /dev/disk/by-id/nvme-Amazon_EC2_NVMe_Instance_Storage_AWS60FDCEE1E2D1EC57B # nvme1n1    1769 GB    ip-10-0-163-72.eu-central-1.compute.internal",
            "        - /dev/disk/by-id/nvme-Amazon_EC2_NVMe_Instance_Storage_AWS1881394BBC04782BC # nvme2n1    1769 GB    ip-10-0-163-72.eu-central-1.compute.internal",
            "        - /dev/disk/by-id/nvme-Amazon_EC2_NVMe_Instance_Storage_AWS6881394BBC04782BC # nvme3n1    1769 GB    ip-10-0-163-72.eu-central-1.compute.internal"
        ]
    }

    TASK [Clean files] *******************************************************************************************************************************************************************************************************
    changed: [localhost] => (item=./file_tmp_1)
    changed: [localhost] => (item=./file_tmp_2)

    PLAY RECAP ***************************************************************************************************************************************************************************************************************
    localhost                  : ok=7    changed=5    unreachable=0    failed=0    skipped=12   rescued=0    ignored=0
