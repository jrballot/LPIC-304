# Topic 335: High Availability Cluster Storage

## 335.1 DRBD / cLVM

**Weight: 3**

Description: Candidates are expected to have the experience and knowledge to install, configure, maintain and troubleshoot DRBD devices. This includes integration with Pacemaker. DRBD configuration of version 8.4.x is covered. Candidates are further expected to be able to manage LVM configuration within a shared storage cluster.

Key Knowledge Areas:

- Understanding of DRBD resources, states and replication modes
- Configuration of DRBD resources, networking, disks and devices
- Configuration of DRBD automatic recovery and error handling
- Management of DRBD using drbdadm
- Basic knowledge of drbdsetup and drbdmeta
- Integration of DRBD with Pacemaker
- cLVM
- Integration of cLVM with Pacemaker

Terms and Utilities:

- [x] Protocol A, B and C
- [x] Primary, Secondary
- [ ] Three-way replication
- [x] drbd kernel module
- [x] drbdadm
- [ ] drbdsetup
- [ ] drbdmeta
- [x] /etc/drbd.conf
- [x] /proc/drbd
- [ ] LVM2
- [ ] clvmd
- [ ] vgchange, vgs
---
### Distributed Replicated Block Device (DRDB)

Port 7789

**install**
DRDB isn't present in the official CentOS repository, so, in order to install DRDB we need to use ELRepo repository:

```sh
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
```

> TIP: Disable ELRepo by default and only anable it when Using
cp /etc/yum.repos.d/elrepo.repo /etc/yum.repos.d/elrepo.repo.ORG
sed -i "s/enabled=1/enabled=0/g" /etc/yum.repos.d/elrepo.repo
yum --enablerepo elrepo update
yum --enablerepo elrepo install -y drbd84-utils kmod-drbd84



```sh
yum install -y drbd84-utils kmod-drbd84
modprobe drbd
lsmod | grep -i drbd
```

**Partitioning**

```sh
parted /dev/sdb mklabel msdos
parted /dev/sdb mkpart p 0% 100%
pvcreate /dev/vdb1
vgcreate drbd_vg /dev/vdb1
lvcreate -n drbd_vol -l 100%FREE drbd_vg
```


#### Configuring DRBD

Simple configuration with drbd.

```SH
vim /etc/drbd.d/global_common.conf

global {
  usage-count no;
}

common{
  net {
    protocol C;
  }
}

```
usage-count no;   Statistic usage collected by DRBD project (yes,no,ask)


Replication modes (Asynchronous, Memory synchronous, Synchronous)
- protocol A: Asynchronous - Local write confirmed and packet placed in the TCP send buffer
- protocol B: Memory synchronous - Local write confirmed and packet has reached the peer node
- protocol C: Synchronous - Local and remote write confirmed

```SHELL
vim /etc/drbd.d/drbd0.res
resource drbd0 {
  disk /dev/drbd_vg/drbd_vol;
  device /dev/drbd0;
  meta-disk internal;

  on centos7-01 {
    address 192.168.122.242:7789;
  }

  on centos7-02 {
    address 192.168.122.243:7789;
  }

}
```

Creating metadata and initiating it

```sh
drbdadm create-md drbd0
drbdadm up drbd0
drbdadm primary --force drbd0
```


**/proc/drbd content**
**Before any configuration has been made**
```SH
[root@centos7-01 ~]# cat /proc/drbd
version: 8.4.11-1 (api:1/proto:86-101)
GIT-hash: 66145a308421e9c124ec391a7848ac20203bb03c build by mockbuild@, 2018-11-03 01:26:55
```
**After drbdadm up drbd0**
```sh
[root@centos7-01 ~]# cat /proc/drbd
version: 8.4.11-1 (api:1/proto:86-101)
GIT-hash: 66145a308421e9c124ec391a7848ac20203bb03c build by mockbuild@, 2018-11-03 01:26:55
 0: cs:WFConnection ro:Secondary/Unknown ds:Inconsistent/DUnknown C r----s
    ns:0 nr:0 dw:0 dr:0 al:8 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:10481308
```
**After drbdadm primary --force drbd0 (centos7-01)**
```sh
[root@centos7-01 ~]# drbdadm primary --force drbd0
[root@centos7-01 ~]# cat /proc/drbd
version: 8.4.11-1 (api:1/proto:86-101)
GIT-hash: 66145a308421e9c124ec391a7848ac20203bb03c build by mockbuild@, 2018-11-03 01:26:55
 0: cs:WFConnection ro:Primary/Unknown ds:UpToDate/DUnknown C r----s
    ns:0 nr:0 dw:0 dr:2120 al:8 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:10481308
```
**Everything alright**
```sh
[root@centos7-01 ~]# cat /proc/drbd
version: 8.4.11-1 (api:1/proto:86-101)
GIT-hash: 66145a308421e9c124ec391a7848ac20203bb03c build by mockbuild@, 2018-11-03 01:26:55
 0: cs:SyncSource ro:Primary/Secondary ds:UpToDate/Inconsistent C r-----
    ns:7785472 nr:0 dw:0 dr:7787592 al:8 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:2695836
        [=============>......] synced: 74.3% (2632/10232)M
        finish: 0:01:02 speed: 42,948 (36,208) K/sec
```

**Diskless problem**
This means that you probably did not created the drbd0 on the other node (drbdadm create-md drbd0)
```shell
[root@centos7-01 ~]# cat /proc/drbd
version: 8.4.11-1 (api:1/proto:86-101)
GIT-hash: 66145a308421e9c124ec391a7848ac20203bb03c build by mockbuild@, 2018-11-03 01:26:55
 0: cs:Connected ro:Primary/Secondary ds:UpToDate/Diskless C r-----
    ns:0 nr:0 dw:0 dr:2120 al:8 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:10481308
```



## 335.2 Clustered File Systems

**Weight: 3**

Description: Candidates should know how to install, maintain and troubleshoot installations using GFS2 and OCFS2. This includes integration with Pacemaker as well as awareness of other clustered filesystems available in a Linux environment.

Key Knowledge Areas:

- Understand the principles of cluster file systems
- Create, maintain and troubleshoot GFS2 file systems in a cluster
- Create, maintain and troubleshoot OCFS2 file systems in a cluster
- Integration of GFS2 and OCFS2 with Pacemaker
- Awareness of the O2CB cluster stack
- Awareness of other commonly used clustered file systems

Terms and Utilities:

- [ ] Distributed Lock Manager (DLM)
- [ ] mkfs.gfs2
- [ ] mount.gfs2
- [ ] fsck.gfs2
- [ ] gfs2_grow
- [ ] gfs2_edit
- [ ] gfs2_jadd
- [ ] mkfs.ocfs2
- [ ] mount.ocfs2
- [ ] fsck.ocfs2
- [ ] tunefs.ocfs2
- [ ] mounted.ocfs2
- [ ] o2info
- [ ] o2image
- [ ] CephFS
- [ ] GlusterFS
- [ ] AFS
