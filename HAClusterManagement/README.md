# Topic 334: High Availability Cluster Management
##334.1 High Availability Concepts and Theory

**Weight: 5**

Description: Candidates should understand the properties and design approaches of high availability clusters.

Key Knowledge Areas:

- Understand the most important cluster architectures
- Understand recovery and cluster reorganization mechanisms
- Design an appropriate cluster architecture for a given purpose
- Application aspects of high availability
- Operational considerations of high availability

Terms and Utilities:

- [x] Active/Passive Cluster, Active/Active Cluster
- [x] Failover Cluster, Load Balanced Cluster
- [x] Shared-Nothing Cluster, Shared-Disk Cluster
- [x] Cluster resources
- [x] Cluster services
- [x] Quorum
- [x] Fencing
- [x] Split brain
- [ ] Redundancy
- [ ] Mean Time Before Failure (MTBF)
- [ ] Mean Time To Repair (MTTR)
- [ ] Service Level Agreement (SLA)
- [ ] Disaster Recovery
- [ ] Replication
- [ ] Session handling
----
### Cluster architecture

**Active/Active (A/A)**
In this type of cluster, all nodes are active at the same time. Thus, they are able to serve requests simultaneously and equally, each with independent workloads. When a failover is necessary, the remaining node is assigned and addcitional processing load, thus impacting the overall performance of the cluster negatively.

**Active/Passive (A/P)**
In this type of cluster, there is an active node and a passive node. The former handles all traffic under normal circumstances, while the latter just sits idle waiting to enter the scene during a failover, when it actually takes over the situation by servicing requests using its own resource until the other node comes back online.

**Shared-Memory Cluster**


**Shared-Disk Cluster**

A shared-disk cluster uses a remote pool for storage, basically a SAN or NAS solution. Problem I/O on disk been a bottleneck for scalability.

**Shared-Nothing Cluster**

In a shared-nothing cluster  things scale horizontally across nodes, memory and disk are managed locally and all communication goes through network. Only one node can be owner of a specific resource at a time.

**Failover Cluster**


**Load Balancer Cluster**
A farm of servers with the same function is the base of a load balancing cluster. To distribute the user request to several nodes, a load balancer is useful. It's going to check the utilization of all nodes and via algorithms decide to which node redirect the user request.

**Cluster resource**
The resources needed to be high available for the business continuity.Can be replicated to one or more nodes from the cluster. Pacemaker uses resources like disk and ip that needs to be available all the time.

**Cluster service**
THe software used to create a high available environment, it controls failover cluster activite on a single node. Pacemaker and Corosync.

### Vocabulary

- **Failover**: A premier on high availability and performance
In this category you could find **Failover Clusters**, where a group of nodes is going to maintain high availability of applications. If, at any moment, a node of this cluster fails, another node will deal with the request without any downtime.

- **Fencing**: isolating the malfunctioning nodes
- **Split brain**: preparing to avoid inconsistencies
- **Quorum**: scoring inside your cluster

## 334.2 Load Balanced Clusters

**Weight: 6**

Description: Candidates should know how to install, configure, maintain and troubleshoot LVS. This includes the configuration and use of keepalived and ldirectord. Candidates should further be able to install, configure, maintain and troubleshoot HAProxy.

Key Knowledge Areas:

- Understanding of LVS / IPVS
- Basic knowledge of VRRP
- Configuration of keepalived
- Configuration of ldirectord
- Backend server network configuration
- Understanding of HAProxy
- Configuration of HAProxy

Terms and Utilities:

- [ ] ipvsadm
- [ ] syncd
- [ ] LVS Forwarding (NAT, Direct Routing, Tunneling, Local Node)
- [ ] connection scheduling algorithms
- [ ] keepalived configuration file
- [ ] ldirectord configuration file
- [ ] genhash
- [ ] HAProxy configuration file
- [ ] load balancing algorithms
- [ ] ACLs


## 334.3 Failover Clusters

**Weight: 6**

Description: Candidates should have experience in the installation, configuration, maintenance and troubleshooting of a Pacemaker cluster. This includes the use of Corosync. The focus is on Pacemaker 1.1 for Corosync 2.x.

Key Knowledge Areas:

- Pacemaker architecture and components (CIB, CRMd, PEngine, LRMd, DC,STONITHd)
- Pacemaker cluster configuration
- Resource classes (OCF, LSB, Systemd, Upstart, Service, STONITH, Nagios)
- Resource rules and constraints (location, order, colocation)
- Advanced resource features (templates, groups, clone resources, multi-state resources)
- Pacemaker management using pcs
- Pacemaker management using crmsh
- Configuration and Management of corosync in conjunction with Pacemaker
- Awareness of other cluster engines (OpenAIS, Heartbeat, CMAN)

Terms and Utilities:

- [x] pcs
- [x] crm
- [x] crm_mon
- [x] crm_verify
- [x] crm_simulate
- [x] crm_shadow
- [x] crm_resource
- [x] crm_attribute
- [x] crm_node
- [x] crm_standby (convenience wrapper for crm_attribute)
- [ ] cibadmin
- [ ] corosync.conf
- [ ] authkey
- [ ] corosync-cfgtool
- [ ] corosync-cmapctl
- [ ] corosync-quorumtool
- [ ] stonith_admin
---
### Pacemaker architecture

**CIB** :  
CRMd
PEngine
LRMd
DC
STONITHd

### PCS commands

DC = Designated Controller

```SH
pcs cluster auth node1 node2
pcs cluster setup --name BallotsCluster node1 node2
pcs cluster start --all
pcs cluster status
pcs cluster stop
```



Double checking with corosync-cmapctl command. This will allow access to the cluster's object database (CIB) where can be seen properties and configurations of each node.

```SH
corosync-cmapctl | grep -Ei 'cluster''_name|members'
```

### Cluster Resource Management (CRM)

- crm_mon: provides a summary of cluster's current state
  - crm_mon --group-by-node --inactive
  - crm_mon --daemonize --as-html /var/www/html/index.html
  - crm_mon --as-xml
- crm_verify: verify syntax and conceptual errors on xml configuration files
  - crm_verify --live-check
  - crm_verify --xml-file <path>/file.xml
- crm_simulate: This tool will simulate actions in the cluster
- crm_shadow: Create a copy of the running cluster for testing
  - crm_shadow --create myShadow
  - crm_shadow --create-empty myShadow
  - crm_shadow --commit myShadow # apply this shadown configuration to the running cluster
  - crm_shadow --delete myShadow --force
- crm_resource: Perform tasks on available resources
  - crm_resource --list-agents ocf
  - crm_resource --list-agents ocf:Heartbeat
  - crm_resource --resource virtual_ip --move --node 02.centos7
  - crm_resource --resource virtual_ip --clear
  - crm_resource --resource virtual_ip --set-parameter target-role --meta --parameter-value Stopped
  - crm_resource --resource virtual_ip --set-parameter is-managed --meta --parameter-value false # cluster will not attempt to start or stop a resource
  - crm_resource --resource virtual_ip --cleanup --node 02.centos7 # will ignore the current state and attempt to recover the resource
- crm_attribute: Allows node attributes and cluster options to be queried, modified and deleted
  - crm_attribute --node 02.centos7 --name location --update myHomeLab
  - crm_attribute --node 02.centos7 --name localtion --query --quiet
  - crm_attribute --node 02.centos7 --name location --delete
  - crm_attribute --type crm_config --name cluster-delay --query # cluster delay info
- crm_node: Tool for displaying low-level node information
  - crm_node --list
  - crm_node --name
  - crm_node --name-for-id=01.centos7
  - crm_node --quorum   
  - crm_node --cluster-id
  - crm_node --partition
- crm_standby: Convenience wrapper for crm_attribute, turn on/off standby mode which will make the node not host cluster resource
  - crm_standby


### Setting up a virtual IP for the cluster

In order to set a virtual ip for the cluster, a resource on pacemaker needs to be created.

```shell
sudo pcs resource create virtual_ip ocf:heartbeat:IPaddr2 \
        ip=192.168.122.4 cidr_netmask=24 nic=enp0s3 op monitor interval=10s
```

**crm_verify** - checks configuration and XML well-formedness. With the options -V (--verbose) and -L (--live-check) you can list ERRORs and WARNING. ERRORs will not let the cluster start.

```shell
sudo crm_verify -L -V
```

You can also pass a xml file to be checked, ans also save it.

```SH
sudo crm_verify --xml-file=file.xml --verbose
sudo cat file.xml | crm_verify -p/--xml-pipe

sudo crm_verify -L -S/--save-xml=file.xml
   error: unpack_resources:     Resource start-up disabled since no STONITH resources have been defined
   error: unpack_resources:     Either configure some or disable STONITH with the stonith-enabled option
   error: unpack_resources:     NOTE: Clusters with shared data need STONITH to ensure data integrity
   Errors found during check: config not valid
```

The above message is saying that STONITH (Shoot the other Node in The Head) resource is not defined. In this case you could configure it or disable with the command:

```shell
sudo pcs property set stonith-enabled=false
```

### Pacemaker resources
**Classes**
- OCF
An extension of LSB, has some constraints about returning status to determine the correct status of a resource or service.
- LSB
This class deals with /etc/ini.d scripts.
- Systemd
This class deals with systemd units
- Upstart
This class deals with upstart scripts
- Service
A special class created for dealing with a set of  system services, like Systemd, Upstart and LSB. Useful when a cluster is composed by a set of different OSs.
- STONITH
For dealing with Shoot the Other Node In The Head situations.
- Nagios
Use a Nagios Pluging to deal with remote hosts.

### Awareness of other cluster engines

OpenAIS

Heartbeat

CMAN


## 334.4 High Availability in Enterprise Linux Distributions

**Weight: 1**

Description: Candidates should be aware of how enterprise Linux distributions integrate High Availability technologies.

Key Knowledge Areas:

- Basic knowledge of Red Hat Enterprise Linux High Availability Add-On
- Basic knowledge of SUSE Linux Enterprise High Availability Extension

Terms and Utilities:

- [ ] Distribution specific configuration tools
- [ ] Integration of cluster engines, load balancers, storage technology, cluster filesystems, etc.
