# STEP 2 REMOVE THE LINE BELOW
roles(['consul_role'])

# STEP 2 UNCOMMENT THE LINE BELOW - STEP 3 REMOVE THE LINE BELOW
# roles(['consul_role','patroni_role', 'pgbouncer_role'])

# STEP 3 UNCOMMENT THE LINE BELOW - SELECT THE APPROPRIATE REDIS ROLE
# roles(['consul_role','patroni_role', 'pgbouncer_role','redis_sentinel_role', '<redis_master_role|redis_replica_role>'])

#### COMMON

node_exporter['listen_address'] = '0.0.0.0:9100'

gitlab_rails['auto_migrate'] = false

#### CONSUL

consul['monitoring_service_discovery'] =  true

consul['configuration'] = {
   server: true,
   bootstrap_expect: 1, # Increase this values to the number of nodes of the cluster
   retry_join: %w(<IP>), # Add more nodes here
}

=begin STEP 2 REMOVE THIS LINE 
##### POSTGRES

postgresql['listen_address'] = '0.0.0.0'

patroni['postgresql']['max_replication_slots'] = X # Set to double the number of nodes.
patroni['postgresql']['max_wal_senders'] = X + 1 # Set to one more than the number of replication slots in the cluster.

consul['services'] = %w(postgresql)
consul['monitoring_service_discovery'] =  true

postgresql['pgbouncer_user_password'] = '<PGBOUNCER_PASSWORD_HASH>' #EDIT md5 value
postgresql['sql_replication_password'] = '<POSTGRESQL_REPLICATION_PASSWORD_HASH>' #EDIT md5 value
postgresql['sql_user_password'] = '<POSTGRESQL_PASSWORD_HASH>' #EDIT md5 value

patroni['username'] = '<PATRONI_API_USERNAME>' # EDIT (use the same username in all nodes)
patroni['password'] = 'PATRONI_API_PASSWORD' # EDIT (use the same password in all nodes)

# Replace XXX.XXX.XXX.XXX/YY with Network Addresses for your other patroni nodes
patroni['allowlist'] = %w(XXX.XXX.XXX.XXX/YY 127.0.0.1/32)

# Replace XXX.XXX.XXX.XXX/YY with Network Address
postgresql['trust_auth_cidr_addresses'] = %w(XXX.XXX.XXX.XXX/YY 127.0.0.1/32)

pgbouncer['databases'] = {
   gitlabhq_production: {
      host: "127.0.0.1",
      user: "<PGBOUNCER_USERNAME>", # EDIT
      password: '<PGBOUNCER_PASSWORD_HASH>' # EDIT md5 value
   }
}

postgres_exporter['listen_address'] = '0.0.0.0:9187'

=end #STEP 2 REMOVE THIS LINE

=begin REMOVE THIS LINE IN STEP 3
##### REDIS

sentinel['bind'] = '0.0.0.0'
sentinel['quorum'] = 2

redis['bind'] = 'NODE_IP' # EDIT

redis['port'] = 6379

#redis['master_port'] = 6379 # default

redis['password'] = '<REDIS_PRIMARY_PASSWORD>' # EDIT (use the same password in all nodes).
redis['master_password'] = '<REDIS_PRIMARY_PASSWORD>' # EDIT (use the same password in all nodes).

redis['master_name'] = '<gitlab-redis>' # EDIT (use the same name in all nodes).

redis['master_ip'] = '<MASTER_IP>' # EDIT

redis_exporter['listen_address'] = '0.0.0.0:9121'

=end #REMOVE THIS LINE IN STEP 3
