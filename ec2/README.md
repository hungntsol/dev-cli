# Amazon EC2

- [Install docker](#docker)
- [Install postgres](#postgresql)

## Docker

### Install docker engine

- Install docker from mirror: ```sudo yum install -y docker```
- Add ec2-user to docker group: ```sudo usermod -a -G docker ec2-user```
- Reload group: ```newgrp docker```
- Start docker.service:

    ```centos
    sudo systemctl enable docker.service
    sudo systemctl start docker.service
    ```

### Install docker-compose

- Install from Github: ```sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose```
- Add permission: ```sudo chmod +x /usr/local/bin/docker-compose```

## PostgreSQL

- Install postgreSQL:

    ```centos
    sudo yum update -y

    sudo tee /etc/yum.repos.d/pgdg.repo<<EOF
    [pgdg12]
    name=PostgreSQL 12 for RHEL/CentOS 7 - x86_64
    baseurl=https://download.postgresql.org/pub/repos/yum/12/redhat/rhel-7-x86_64
    enabled=1
    gpgcheck=0
    EOF

    sudo yum makecache
    sudo yum install -y postgresql12.x86_64 postgresql12-libs.x86_64 postgresql12-server.x86_64
    sudo /usr/pgsql-12/bin/postgresql-12-setup initdb
    sudo systemctl start postgresql-12.service
    ```

- Enable remote with postgres user:

    ```bash
    sudo su postgres
    psql -U postgres -c "CREATE ROLE postgres;"
    psql -U postgres -c "ALTER ROLE postgres WITH LOGIN;"
    psql -U postgres -c "ALTER USER postgres CREATEDB;"
    psql -U postgres -c "ALTER USER postgres WITH PASSWORD ${pass};"
    ```

- Enable connect from remote client:

  - Edit file `postgres.conf` which allow who can allow listen to postgreSQL

    - `listen_addresses` = '*'

  - Edit file `pg_hba.conf` which allow who can connect

- Setup streaming replication: (link replication: `https://www.highgo.ca/2019/11/07/streaming-replication-setup-in-pg12-how-to-do-it-right/`)

```centos
cd /etc/yum.repos.d
sudo perl -i.org -pe 's/\$releasever/7/g' pgpool-II-release-40.repo
rpm --import https://www.pgpool.net/yum/RPM-GPG-KEY-PGPOOL2
```
