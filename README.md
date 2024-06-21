# Use
This can be used to configure a redis cluster in 1 go with specific number fo processes of redis per sever.

# Clustering

Cluster needs to be created manually using the following command on any 1 machines:

```sh
redis-cli --cluster create nmr-mq-1:8000 nmr-mq-1:8001 nmr-mq-1:8002 nmr-mq-1:8003 nmr-mq-1:8004 nmr-mq-1:8005 nmr-mq-2:8000 nmr-mq-2:8001 nmr-mq-2:8002 nmr-mq-2:8003 nmr-mq-2:8004 nmr-mq-2:8005 nmr-mq-3:8000 nmr-mq-3:8001 nmr-mq-3:8002 nmr-mq-3:8003 nmr-mq-3:8004 nmr-mq-3:8005 --cluster-replicas 1
```

# UFW Rules

For basic ssh.

```sh
ufw allow ssh
```

For redis
```sh
ufw allow in on eno1 from any to any port 8000:8005 proto tcp comment 'redis client'
ufw allow in on eno1 from any to any port 8000:8005 proto udp comment 'redis client'
ufw allow in on eno2 from 172.16.11.1/24 to any port 8000:8005 proto tcp comment 'redis intercommunication'
ufw allow in on eno2 from 172.16.11.1/24 to any port 8000:8005 proto udp comment 'redis intercommunication'
ufw allow in on eno2 from 172.16.11.1/24 to any port 18000:18005 proto tcp comment 'redis intercommunication'
ufw allow in on eno2 from 172.16.11.1/24 to any port 18000:18005 proto udp comment 'redis intercommunication'
```

For RabbitMQ on the same nodes

```sh
ufw allow in on eno1 from any to any port 5673 proto tcp comment 'rmp haproxy' 
ufw allow in on eno1 from any to any port 15673 proto tcp comment 'rmp haproxy' 
ufw allow in on eno2 from 172.16.11.0/24 to any port 5672 comment 'rmq intercommunication'
ufw allow in on eno2 from 172.16.11.0/24 to any port 15672 comment 'rmq intercommunication'
ufw allow in on eno2 from 172.16.11.0/24 to any port 25672 comment 'rmq intercommunication'
```

For haproxy stats

```sh
ufw allow in on eno1 from any to any port 7000 proto tcp comment 'haproxy stats'
```

Defaults

```sh
ufw default deny
```