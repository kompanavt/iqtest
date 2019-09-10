# Install minio cluster & nginx reverse proxy with load balancing using vagrant and ansible

## Requirements

Vagrant

## To run

1. You must set the ip adresses in `Vagrantfile` and also in the ansible group vars file (see the `group_vars/all.yml`)
2. Set the `minio_access_key` and `minio_secret_key` in group_vars (optional).
3. `vagrant up`
4. login to any ip address from node list using `http://ip_address/` or domain name if you setting it.

## Notes

Number of nodes: 3

Every node will configure automatically using vagrant ansible_local.

***

For managing minio you must login to any node (`vagrant ssh node1` for example) and download the MinIO Client:
```yaml
ansible localhost -m shell -a "wget -P /home/vagrant/ https://dl.min.io/client/mc/release/linux-amd64/mc"
ansible localhost -m file -a "dest=/home/vagrant/mc mode=775"
```
Creating an alias. Alias is simply a short name to your MinIO service.
If you changed `minio_access_key` and `minio_secret_key` you must change last two params.
```yaml
ansible localhost -m shell -a "/home/vagrant/mc config host add node1 http://node1:9000 9e22b2ee283109ab44b3ddeb56f9ed7a 9da30539af3639c600c6256f7691750a581c36c2"
```
Creating user and set the read-write permissions:
```yaml
ansible localhost -m shell -a "/home/vagrant/mc admin user add node1 user1 devuser123"
ansible localhost -m shell -a "/home/vagrant/mc admin policy set node1 readwrite user=user1"
```
Make a bucket:
```yaml
ansible localhost -m shell -a "/home/vagrant/mc mb node1/test"
```
and upload the test file:
```yaml
ansible localhost -m shell -a "/home/vagrant/mc cp 'test (1).jpg' node1/test"
```
