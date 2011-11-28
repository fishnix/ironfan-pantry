# hadoop_cluster chef cookbook

Installs hadoop and sets up a high-performance cluster. Inspired by Tom White / Cloudera's hadoop-ec2 command line utilities.

## Overview

Installs Apache hadoop using the [Cloudera hadoop distribution (CDH)](http://archive.cloudera.com/docs/)

It comes with hooks for the Amazon cloud using EBS-backed instances.

For more details on the so, so many config variables, see

* [Core Hadoop params](http://archive.cloudera.com/cdh/3/hadoop/hdfs-default.html) (which often actually has things you'd think were in one of the following)
* [HDFS params](http://archive.cloudera.com/cdh/3/hadoop/hdfs-default.html) 
* [Map-Reduce params](http://archive.cloudera.com/cdh/3/hadoop/hdfs-default.html) 

### A couple helpful notes

Inspired by Tom White / Cloudera's hadoop-ec2 command line utilities.

Note that the secondarynamenode is NOT a redundant namenode. All it does is make periodic backups of the HDFS metadata.

### Author:
      
Author:: Joshua Timberman (<joshua@opscode.com>), Flip Kromer (<flip@infochimps.org>), much code taken from Tom White (<tom@cloudera.com>)'s hadoop-ec2 scripts and Robert Berger (http://blog.ibd.com)'s blog posts.

Copyright:: 2009, Opscode, Inc; 2010, 2011 Infochimps, In

## Attributes

* `[:cluster_size]`                   - Number of machines in the cluster (default: "5")
  - Number of machines in the cluster. This is used to size things like handler counts, etc.
* `[:apt][:cloudera][:force_distro]`  - Override the distro name apt uses to look up repos
  - Typically, leave this blank. However if (as is the case in Nov 2011) you are on natty but Cloudera's repo only has packages up to maverick, use this to override.
* `[:apt][:cloudera][:release_name]`  - Release identifier (eg cdh3u2) of the cloudera repo to use. See also hadoop/deb_version (default: "cdh3u2")
* `[:hadoop][:handle]`                - Version prefix for the daemons and other components (default: "hadoop-0.20")
  - Cloudera distros have a prefix most (but not all) things with. This helps isolate the times they say 'hadoop-0.20' vs. 'hadoop'
* `[:hadoop][:deb_version]`           - Apt revision identifier (eg 0.20.2+923.142-1~maverick-cdh3) of the specific cloudera apt to use. See also apt/release_name (default: "0.20.2+923.142-1~maverick-cdh3")
* `[:hadoop][:dfs_replication]`       - Default HDFS replication factor (default: "3")
  - HDFS blocks are by default reproduced to this many machines.
* `[:hadoop][:reduce_parallel_copies]` -  (default: "10")
* `[:hadoop][:tasktracker][:http_threads]` -  (default: "32")
* `[:hadoop][:jobtracker][:handler_count]` -  (default: "40")
* `[:hadoop][:namenode][:handler_count]` -  (default: "40")
* `[:hadoop][:datanode][:handler_count]` -  (default: "8")
* `[:hadoop][:compress_output]`       -  (default: "false")
* `[:hadoop][:compress_output_type]`  -  (default: "BLOCK")
* `[:hadoop][:compress_output_codec]` -  (default: "org.apache.hadoop.io.compress.DefaultCodec")
* `[:hadoop][:compress_mapout]`       -  (default: "true")
* `[:hadoop][:compress_mapout_codec]` -  (default: "org.apache.hadoop.io.compress.DefaultCodec")
* `[:hadoop][:log_retention_hours]`   -  (default: "24")
  - See [Hadoop Log Location and Retention](http://www.cloudera.com/blog/2010/11/hadoop-log-location-and-retention) for more.
* `[:hadoop][:java_heap_size_max]`    -  (default: "1000")
* `[:hadoop][:persistent_dirs]`       - 
* `[:hadoop][:scratch_dirs]`          - 
* `[:hadoop][:max_balancer_bandwidth]` -  (default: "1048576")
* `[:hadoop][:min_split_size]`        -  (default: "134217728")
* `[:hadoop][:s3_block_size]`         - fs.s3n.block.size (default: "134217728")
  - Block size to use when reading files using the native S3 filesystem (s3n: URIs).
* `[:hadoop][:hdfs_block_size]`       - dfs.block.size (default: "134217728")
  - The default block size for new files
* `[:hadoop][:max_map_tasks]`         -  (default: "3")
* `[:hadoop][:max_reduce_tasks]`      -  (default: "2")
* `[:hadoop][:java_child_opts]`       -  (default: "-Xmx2432m -Xss128k -XX:+UseCompressedOops -XX:MaxNewSize=200m -server")
* `[:hadoop][:java_child_ulimit]`     -  (default: "7471104")
* `[:hadoop][:io_sort_factor]`        -  (default: "25")
* `[:hadoop][:io_sort_mb]`            -  (default: "250")
* `[:hadoop][:extra_classpaths]`      - 
* `[:hadoop][:namenode][:run_state]` - 
* `[:hadoop][:namenode][:java_heap_size_max]` - 
* `[:hadoop][:secondarynn][:run_state]` - 
* `[:hadoop][:secondarynn][:java_heap_size_max]` - 
* `[:hadoop][:jobtracker][:run_state]` - 
* `[:hadoop][:jobtracker][:java_heap_size_max]` - 
* `[:hadoop][:datanode][:run_state]` - 
* `[:hadoop][:datanode][:java_heap_size_max]` - 
* `[:hadoop][:tasktracker][:run_state]` - 
* `[:hadoop][:tasktracker][:java_heap_size_max]` - 
* `[:groups][:hadoop][:gid]`          -  (default: "300")
* `[:groups][:supergroup][:gid]`      -  (default: "301")
* `[:groups][:hdfs][:gid]`            -  (default: "302")
* `[:groups][:mapred][:gid]`          -  (default: "303")
* `[:server_tuning][:ulimit][:hdfs]`  - 
* `[:server_tuning][:ulimit][:hbase]` - 
* `[:server_tuning][:ulimit][:mapred]` - 

## Recipes 

* `add_cloudera_repo`        - Add Cloudera repo to package manager
* `cluster_conf`             - Cluster Conf
* `datanode`                 - Datanode
* `default`                  - Base configuration for hadoop_cluster
* `doc`                      - Doc
* `ec2_conf`                 - Ec2 Conf
* `hadoop_webfront`          - Hadoop Webfront
* `hdfs_fuse`                - Hdfs Fuse
* `jobtracker`               - Jobtracker
* `namenode`                 - Namenode
* `pseudo_distributed`       - Pseudo Distributed
* `secondarynn`        - Secondarynn
* `tasktracker`              - Tasktracker
* `wait_on_hdfs_safemode`    - Wait On HDFS Safemode
## Integration

Supports platforms: debian and ubuntu

Cookbook dependencies:
* java
* apt
* runit
* mountable_volumes
* provides_service


## License and Author

Author::                Philip (flip) Kromer - Infochimps, Inc (<coders@infochimps.com>)
Copyright::             2011, Philip (flip) Kromer - Infochimps, Inc

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

> readme generated by [cluster_chef](http://github.com/infochimps/cluster_chef)'s cookbook_munger
