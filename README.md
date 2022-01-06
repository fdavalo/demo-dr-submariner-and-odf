# Disaster Recovery with Submariner  and Openshift Data Foundation

## Demo pre-requisites

### _Submariner connectivity between clusters_

 - Use ACM for simple Submariner setup or use subctl CLI to install and join clusters
 - Submariner used for image snapshot transfer and remote image status

### _ODF asynchronous data replication between clusters (*1)_
  - ODF installed and instantiated on each cluster
  - Ceph Rbd Mirroring enabled between clusters (peer secret exchanged) 
  - Ceph Block Pool configured with mirroring (+ storage class)
  - Volume replication enabled on Ceph CSI
  - Volume replication class configured (snapshot scheduling)

### _Application deployed on Active/Passive mode_
  - PVC+PV created on primary clustered
  - Volume replication enabled for PVC
  - PVC+PV cloned on secondary cluster (same pool image)
  - Application objects deployed on both clusters (argoCD, …)
    - Pods using PVC on secondary cluster won’t be able to mount replicated volumes
  Use demo/primary and demo/secondary Kustomize example from    https://guithub.com/fdavalo/demo-dr-submariner-and-odf/example-odf-objects

*1: you can use RDRhelper after ODF instance is up, cf https://github.com/red-hat-data-services/RDRhelper

*1: see example of ODF configured objects https://guithub.com/fdavalo/demo-dr-submariner-and-odf/example-odf-objects/

*1: as an alternative, use operators (need ACM) to configure mirroring (multi-cluster orchestrator, …), cf demo DR with ACM and ODF

### _Check pre-requisites_

  - Ceph Block pool mirroring status are OK
  - PVC+PV are available in both clusters (with same pool image)
  - Volume replication instance (for each PVC) are OK (primary and secondary)
  - Application is ready (Up in primary, Down in secondary)

### _Switch Volume replication status_

  - On primary cluster, demote volume replication instances (-> secondary mode)
  - On secondary cluster, promote volume replication instances (-> primary mode)

### _Check Application failover_

  - PVC+PV created on primary clustered
