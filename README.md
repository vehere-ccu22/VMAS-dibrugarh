# VMAS-dibrugarh

Short Description : This cluster belongs to four nodes. 
1. one co-ordinating node ( 192.168.2.160 )
2. one master with ingest node (192.168.2.164 )
3. two data with master node (192.168.2.161 & 192.168.2.162 )


The changes taken to tune in all the nodes are as below :
 1. /etc/security/limits.conf
    elasticsearch - nofile 65536
    elasticsearch soft nproc 4096
    elasticsearch hard nproc 4096
    elasticsearch soft memlock unlimited
    elasticsearch hard memlock unlimited 
    
  2. /etc/sysconfig/elasticsearch
   # /etc/systemd/system/elasticsearch.service.d/override.conf.
   MAX_LOCKED_MEMORY=unlimited
  
  3. /etc/systemd/system/elasticsearch.service.d
  Create a file override.conf and mention the following lines
      [Service]
      LimitMEMLOCK=infinity
      
   4. /etc/fstab/
      UUID=d513fc76-54bb-432c-81b1-aeca5a70c4b7 /data xfs inode64,noatime,nodiratime 1 2
   
   5.  /usr/lib/systemd/system/elasticsearch.service
       # Specifies the maximum file descriptor number that can be opened by this process
        LimitNOFILE=65536
        # Specifies the maximum number of processes
        LimitNPROC=4096
        # Specifies the maximum size of virtual memory
        LimitAS=infinity
        # in elasticsearch.yml and 'MAX_LOCKED_MEMORY=unlimited' in /etc/sysconfig/elasticsearch
        LimitMEMLOCK=infinity
   
   The cron details : All the cron job are running from 192.168.2.164 except the del_indices, the del_indices running from 192.168.2.160.
   We will get all the details with the command crontab -L and to change them use command crontab -e.
   
   
