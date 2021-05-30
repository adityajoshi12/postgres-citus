# Installation

## Docker-Compose

1. Start docker container using compose file
   ```
   docker-compose up -f docker-compose.yml
   ```
2. Master Node is exposed on port 55432
3. Go inside the shell of master postgres instance and check the no of worker
   ```
   SELECT master_get_active_worker_nodes();
   ```
4. If want to increase the number of worker to 5
   ```
   docker-compose -p citus scale worker=5
   ```
5. Now again go inside the shell of master-node and check the number of workers
   ```
   SELECT master_get_active_worker_nodes();
   ```

## Usage

1. Create the database and tables
2. Create Distributed table for those tables which you want to shard.
   ```
   SELECT create_distributed_table('table_name','column_name');
   ```
3. In future if you want to reshard the data
   ```
   SELECT rebalance_table_shards('table_name');
   ```

## Docker Swarm

1. Create a docker swarm network.
2. create a overlay network.
   ```
   docker network create --driver=overlay --attachable test
   ```
3. Deploy the stack
   ```
   docker stack deploy -c docker-swarm.yaml test
   ```
4. check if all the service are running.
   ```
   docker service ls
   ```

## Usage

1. Go inside the shell of master postgres instance.
2. List all the available worker.
   ```
   SELECT master_get_active_worker_nodes();
   ```
3. All the worker nodes.

   ```
   SELECT * from citus_add_node('worker1', 5432);
   SELECT * from citus_add_node('worker2', 5432);

   ```

4. Again list all the available worker.
   ```
   SELECT master_get_active_worker_nodes();
   ```
5. Create the database and tables.
6. Create Distributed table for those tables which you want to shard.
   ```
   SELECT create_distributed_table('table_name','column_name');
   ```
7. In future if you want to reshard the data
   ```
   SELECT rebalance_table_shards('table_name');
   ```
