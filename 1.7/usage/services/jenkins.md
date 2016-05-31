---
layout: page
post_title: Jenkins
menu_order: 0
---


Jenkins is a continuous integration and continuous delivery application. You can use Mesosphere DC/OS to deploy Jenkins on your cluster.

Jenkins on DC/OS allows you to scale your Jenkins cluster by dynamically creating and destroying Jenkins agents as demand increases or decreases and enables you to avoid the statically partitioned infrastructure typical of other Jenkins clusters.

To use Jenkins on DC/OS, you must provide a mount point to a shared file system on each of your DC/OS agents. There are a number of existing shared file system solutions, including NFS, HDFS (plus its NFS gateway), Ceph and more. The following instructions use NFS as the example.

## Installing Jenkins on DC/OS

#### Prerequisites

*   The DC/OS CLI must be [installed][1].

*   A mount point to a shared file system at the same path on each of your DC/OS agents. NFS guides are available for [Red Hat Enterprise Linux][2], [Ubuntu][3], and [CoreOS][4].

1.  Create a Jenkins JSON configuration file that specifies any instance or site-specific information, such as the framework name and the base path to the NFS share on the DC/OS or Mesos agent. See the [Configuration Reference][5] for a complete example.
    
    Save the file as `options.json`. This file is specified during DC/OS Jenkins installation.
    
        {
            "jenkins": {
                "framework-name": "jenkins-myteam",
                "host-volume": "/mnt/nfs/jenkins_data"
            }
        }
        
    
    By choosing a unique framework name, you can run multiple Jenkins instances on the same cluster, sharing the cluster’s resources among all Jenkins masters. Each Jenkins instance creates a subdirectory inside the directory that you specified for the host volume. In this example, the data is stored on the NFS server at `/mnt/nfs/jenkins_data/jenkins-myteam`.

2.  From the DC/OS CLI, install Jenkins with the `options.json` file specified:
    
        $ dcos package install jenkins --options=options.json
        
    
    The first time Jenkins runs, it populates the directory on your NFS share with a basic Jenkins configuration and a small set of plugins. If Marathon needs to restart the task on a different host, the container automatically mounts your existing data directory, including job configurations, build history and installed plugins. For future versions of DC/OS, we are planning add support for other distributed file systems and leverage the persistence primitives available in recent versions of Mesos.

3.  Verify that Jenkins is installed and healthy.
    
    *   From the DC/OS CLI, enter this command to show the installed packages:
        
            $ dcos package list
            
    
    *   From the DC/OS web interface, go to the Services tab and confirm that Jenkins is running at `/#/services/`. <!-- screenshot of web UI -->

4.  Open a browser and navigate to the Jenkins web interface at `http://<hostname>/service/jenkins`.

## Uninstalling Jenkins on DC/OS

1.  From the DC/OS CLI, uninstall Jenkins:
    
        $ dcos package uninstall jenkins
        

**Note:** You must manually clean up any job configurations and/or build data was stored on your NFS share.

 [1]: /usage/cli/install/
 [2]: https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Storage_Administration_Guide/ch-nfs.html
 [3]: https://help.ubuntu.com/14.04/serverguide/network-file-system.html
 [4]: https://coreos.com/os/docs/latest/mounting-storage.html#mounting-nfs-exports
 [5]: http://mesosphere.github.io/jenkins-mesos/docs/configuration.html