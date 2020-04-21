# Playbooks for simplify some taks with local-storage in OpenShift Container Storage

## Introduction 
Those ansible playbooks provides an intereactive menu for storage operations.

## What those playbook can do for you?
The playbooks will offer the following interactive menu:
    
    Hey there, what do you want to do?
    1=Listing OSD information deviceset|pv|pvc|host
    2=Replace a failed OSD (interactive)
    3=List device-byid for Create/Update loca-storage CR
    [0]:

## Testing environment
Following the cluster for testing the playbooks:

1. OCP Cluster v4.3 with 3 masters

2. IPI deployment (fuly automated with OpenShift machinesets)

3. OCS v4.3 operator with loca-storage (Techpreview)

4. OCS i3.8xlarge AWS instances with 4 local NVMes with 1.8TB size each

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

## Let's have fun

Create an ansible playbook by using the role, following an example:

    ---
    - name: 'Using ansible rbd_ceph_performance role'
      hosts: clients
      become: true
      roles:
        - rbd_ceph_performance

Now let's share some screenshots with comments:

![1.](1.png)

![2.](2.png)

![3.](3.png)

While the benchmark is running you can monitor performance in real time  from RHCS v4 dashboard  

![4.](4.png)