# Ansible kit

Easy manage fresh servers

## Usage

```sh
$ ansible-playbook secure-ssh.yml -i inventory/gitlab-runner.ini
$ ansible-playbook secure-ssh.yml -i inventory/app.ini
```

### Gitlab Runner

```sh
$ ansible-playbook gitlab-runner.yml -i inventory/gitlab-runner.ini
# postgres
$ sudo -u postgres psql -d template1
$ > CREATE USER runner WITH PASSWORD 'runner' CREATEDB;
$ > ALTER USER runner WITH SUPERUSER;
```

Login to the node after and register the runner with `gitlab-runner register`
```sh
$ useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
$ usermod -aG docker gitlab-runner
$ gitlab-runner register
# service
$ gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
$ gitlab-runner start
```

### Deployment

```sh
$ ansible-playbook docker-swarm.yml -i inventory/app.ini
$ docker login registry.gitlab.com
# first time deploy only postgres and http nginx to
# prepare for full deployment (load dump and get certs)
$ ansible-playbook deploy.yml -i inventory/app.ini
```

```sh
$ echo "" | docker secret create master_key -
$ docker exec <CONTAINER_ID> psql -U postgres -c 'create database "app_";'
```

### Certbot

```sh
$ ansible-playbook certbot.yml -i inventory/app.ini
```

### Auto deployments
Allow CI to login to production hosts.
```sh
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
## Bonus

Figure out how to make real ip addres apper in logs.
