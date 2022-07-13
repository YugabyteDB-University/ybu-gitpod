## Working with yugabyted 

### Single node
```
./yugabyted start

./yugabyted demo

./yugabyted demo connect


```

### Multiple node simple
./yugabyted start \
--advertise_address=127.0.0.1 \
--cloud_location=cloud1.region1.zone1 \
--fault_tolerance=zone 
--config
--tserver_flags="

./yugabyted start \
--advertise_address=127.0.0.2 \
--cloud_location=cloud1.region1.zone2 \
--fault_tolerance=zone \
--join=127.0.0.1

./yugabyted start \
--advertise_address=127.0.0.3 \
--cloud_location=cloud1.region1.zone3 \
--fault_tolerance=zone \
--join=127.0.0.1

./yugabyted configure --fault_tolerance=zone 

./yugabyted demo

./yugabyted demo connect

./yugabyted start --ui=true
7002
```
ENV HOST_LB1=127.0.0.1
ENV HOST_LB2=127.0.0.2
ENV HOST_LB3=127.0.0.3
ENV STORE=/var/ybdp
ENV YSQL_PORT=5433
ENV YCQL_PORT=9042
ENV WEB_PORT1=7001
ENV WEB_PORT2=7002
ENV WEB_PORT3=7003
ENV MASTER_WEB_PORT1=7001
ENV MASTER_WEB_PORT2=7002
ENV MASTER_WEB_PORT3=7002
ENV TSERVER_WEB_PORT1=8201
ENV TSERVER_WEB_PORT2=8202
ENV TSERVER_WEB_PORT3=8203

ENV YCQL_API_PORT=12000
ENV YSQL_API_PORT=13000


bin/yugabyted start \
--base_dir=/Users/seth/var/ybdp/ybd1 \
--listen=127.0.0.1 \
--tserver_webserver_port=8201 \
--tserver_flags=ysql_num_shards_per_tserver=1 \
--tserver_flags=ysql_beta_features=true \
--tserver_flags=webserver_port=8201 \
--master_flags=ysql_num_shards_per_tserver=1 \
--master_flags=replication_factor=3 \
--master_webserver_port=7001



bin/yugabyted start \
--base_dir=/Users/seth/var/ybdp/ybd2 \
--listen=127.0.0.2 \
--join=127.0.0.1 \
--tserver_webserver_port=8202 \
--tserver_flags=ysql_num_shards_per_tserver=1 \
--tserver_flags=ysql_beta_features=true \
--tserver_flags=webserver_port=8202 \
--master_flags=ysql_num_shards_per_tserver=1 \
--master_flags=replication_factor=3 \
--master_webserver_port=7002


bin/yugabyted start \
--base_dir=/Users/seth/var/ybdp/ybd3 \
--listen=127.0.0.3 \
--join=127.0.0.1 \
--tserver_webserver_port=8203 \
--tserver_flags=ysql_num_shards_per_tserver=1 \
--tserver_flags=ysql_beta_features=true \
--tserver_flags=webserver_port=8203 \
--master_flags=ysql_num_shards_per_tserver=1 \
--master_flags=replication_factor=3 \
--master_webserver_port=7003


 yugabyted start --base_dir=$STORE/ybd2 --listen=127.0.0.2 --join=127.0.0.1


      gp sync-done base
  - name: run-lb2
    before: |
      gp sync-await base
      yugabyted start --base_dir=$STORE/ybd2 --listen=127.0.0.2 --join=127.0.0.1
      gp sync-done lb2
  - name: run-lb3
    before: |
      gp sync-await lb2
      yugabyted start --base_dir=$STORE/ybd3 --listen=127.0.0.3 --join=127.0.0.1
  - name: shell
    command: |


 --tserver_flags="placement_cloud=cloud1,placement_region=region1,placement_zone=zone1,ysql_num_shards_per_tserver=1,ysql_beta_features=true,tserver_webserver_port=8201,webserver_port=8201,replication-factor=3" \
 --master_flags="placement_cloud=cloud1,placement_region=region1,placement_zone=zone1,ysql_num_shards_per_tserver=1,master_webserver_port=7001" 



--rpc_bind_addresses=127.0.0.2 --server_broadcast_addresses=127.0.0.2 --cql_proxy_bind_address=127.0.0.2:9042 --server_dump_info_path=/Users/seth/var/ybdp/ybd2/data/tserver-info --start_pgsql_proxy --pgsql_proxy_bind_address=127.0.0.2:5433 --tserver_enable_metrics_snapshotter=true --metrics_snapshotter_interval_ms=11000 --webserver_port=8202 --default_memory_limit_to_ram_ratio=0.6 --instance_uuid_override=288f0d97ac03415dba0359b52d5dc8c2 --start_redis_proxy=false --tserver_master_addrs=127.0.0.1:7101,127.0.0.2:7102 >/dev/null 2>&1 &




### Multiple node
```
./yugabyted start \
--tserver_webserver_port=8201 \
--master_webserver_port=7201 \
--webserver_port=7301 \
--advertise_address=127.0.0.1 \
--cloud_location=cloud1.region1.zone1 \
--fault_tolerance=zone \
--tserver_flags="rpc_bind_addresses=127.0.0.1:9101,server_broadcast_addresses=127.0.0.1:9101" \
--master_flags="master_addresses=127.0.0.1:7101,127.0.0.1:7102,127.0.0.1:7103,rpc_bind_addresses=127.0.0.1:7101,server_broadcast_addresses=127.0.0.1:7101" 

./yugabyted start \
--tserver_webserver_port=8202 \
--master_webserver_port=7202 \
--webserver_port=7302 \
--advertise_address=127.0.0.2:9102 \
--cloud_location=cloud1.region1.zone2 \
--fault_tolerance=zone \
--join=127.0.0.1:9101 \
--tserver-flags="tserver_master_addrs=127.0.0.1:7101,127.0.0.2:7102,127.0.0.3:7103,rpc_bind_addresses=127.0.0.1:9102,server_broadcast_addresses=127.0.0.1:9102" \
--master-flags="master_addresses=127.0.0.1:7101,127.0.0.2:7102,127.0.0.3:7103,rpc_bind_addresses=127.0.0.1:7102,server_broadcast_addresses=127.0.0.1:7102" \

./yugabyted start \
--tserver_webserver_port=8203 \
--master_webserver_port=7203 \
--webserver_port=7303 \
--advertise_address=127.0.0.3:9103 \
--cloud_location=cloud1.region1.zone3 \
--fault_tolerance=zone \
--join=127.0.0.1:9101 \
--tserver-flags="tserver_master_addrs=127.0.0.1:7101,127.0.0.2:7102,127.0.0.3:7103,rpc_bind_addresses=127.0.0.1:9103,server_broadcast_addresses=127.0.0.1:9103"
--master-flags="master_addresses=127.0.0.1:7101,127.0.0.2:7102,127.0.0.3:7103,rpc_bind_addresses=127.0.0.1:7103,server_broadcast_addresses=127.0.0.1:7103" \

./yugabyted configure --fault_tolerance=zone 

./yugabyted demo

./yugabyted demo connect

./yugabyted start --ui=true
7002
```


--tserver_webserver_port=8201 \
--master_webserver_port=7001 \

--tserver_webserver_port=8202 \
--master_webserver_port=7002 \

--tserver_webserver_port=8203 \
--master_webserver_port=7003 \