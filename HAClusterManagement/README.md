## Topic 334: High Availability Cluster Management
334.1 High Availability Concepts and Theory

Weight: 5

Description: Candidates should understand the properties and design approaches of high availability clusters.

Key Knowledge Areas:

    Understand the most important cluster architectures
    Understand recovery and cluster reorganization mechanisms
    Design an appropriate cluster architecture for a given purpose
    Application aspects of high availability
    Operational considerations of high availability

Terms and Utilities:

    Active/Passive Cluster, Active/Active Cluster
    Failover Cluster, Load Balanced Cluster
    Shared-Nothing Cluster, Shared-Disk Cluster
    Cluster resources
    Cluster services
    Quorum
    Fencing
    Split brain
    Redundancy
    Mean Time Before Failure (MTBF)
    Mean Time To Repair (MTTR)
    Service Level Agreement (SLA)
    Disaster Recovery
    Replication
    Session handling


334.2 Load Balanced Clusters

Weight: 6

Description: Candidates should know how to install, configure, maintain and troubleshoot LVS. This includes the configuration and use of keepalived and ldirectord. Candidates should further be able to install, configure, maintain and troubleshoot HAProxy.

Key Knowledge Areas:

    Understanding of LVS / IPVS
    Basic knowledge of VRRP
    Configuration of keepalived
    Configuration of ldirectord
    Backend server network configuration
    Understanding of HAProxy
    Configuration of HAProxy

Terms and Utilities:

    ipvsadm
    syncd
    LVS Forwarding (NAT, Direct Routing, Tunneling, Local Node)
    connection scheduling algorithms
    keepalived configuration file
    ldirectord configuration file
    genhash
    HAProxy configuration file
    load balancing algorithms
    ACLs


334.3 Failover Clusters

Weight: 6

Description: Candidates should have experience in the installation, configuration, maintenance and troubleshooting of a Pacemaker cluster. This includes the use of Corosync. The focus is on Pacemaker 1.1 for Corosync 2.x.

Key Knowledge Areas:

    Pacemaker architecture and components (CIB, CRMd, PEngine, LRMd, DC, STONITHd)
    Pacemaker cluster configuration
    Resource classes (OCF, LSB, Systemd, Upstart, Service, STONITH, Nagios)
    Resource rules and constraints (location, order, colocation)
    Advanced resource features (templates, groups, clone resources, multi-state resources)
    Pacemaker management using pcs
    Pacemaker management using crmsh
    Configuration and Management of corosync in conjunction with Pacemaker
    Awareness of other cluster engines (OpenAIS, Heartbeat, CMAN)

Terms and Utilities:

    pcs
    crm
    crm_mon
    crm_verify
    crm_simulate
    crm_shadow
    crm_resource
    crm_attribute
    crm_node
    crm_standby
    cibadmin
    corosync.conf
    authkey
    corosync-cfgtool
    corosync-cmapctl
    corosync-quorumtool
    stonith_admin


334.4 High Availability in Enterprise Linux Distributions

Weight: 1

Description: Candidates should be aware of how enterprise Linux distributions integrate High Availability technologies.

Key Knowledge Areas:

    Basic knowledge of Red Hat Enterprise Linux High Availability Add-On
    Basic knowledge of SUSE Linux Enterprise High Availability Extension

Terms and Utilities:

    Distribution specific configuration tools
    Integration of cluster engines, load balancers, storage technology, cluster filesystems, etc.
