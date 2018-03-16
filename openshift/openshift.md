
## Set DNS (master & node)
```
-- get hostname
hostname

-- set hostname
hostnamectl set-hostname cefcfco.com

-- set hosts
vi /etc/hosts
-- add this line to hosts
change-to-your-ip cefcfco.com
```

## Install required package (master & node)
```
yum update
yum install epel-release docker ansible git net-tools NetworkManager java-1.8.0-openjdk-headless "@Development Tools"

-- if ansible version less then 2.4.3 then update
ansible --version
rpm -Uvh https://releases.ansible.com/ansible/rpm/release/epel-7-x86_64/ansible-2.4.3.0-1.el7.ans.noarch.rpm

shutdown -r now
```

## Prepared NetworkManager (master & node)
```
-- Check NetworkManager state
systemctl | grep "NetworkManager.*running"

-- if not running
systemctl start NetworkManager
systemctl enable NetworkManager
```

## Prepared docker registry mirrors (master & node)
```
vi /etc/sysconfig/docker

-- change this line 
OPTIONS=' --selinux-enabled     --log-driver=journald  --signature-verification=False --registry-mirror=https://2h7decqf.mirror.aliyuncs.com'

systemctl restart docker
systemctl enable docker
```

## Prepared Openshift-Ansible hosts (master)
```
-- backup hosts
cp /etc/ansible/hosts /etc/ansible/hosts.bak
-- edit hosts
echo > /etc/ansible/hosts
vi /etc/ansible/hosts
```

## Install (master)
1. config ssh access master & all nodes
2. checkout openshift-origin
    ```
    git clone https://github.com/openshift/openshift-ansible.git
    git checkout release-3.9

    ```
3. install
    ```
    -- check the config file
    ls openshift-ansible/playbooks/deploy_cluster.yml

    -- install
    ansible-playbook -i /etc/ansible/hosts openshift-ansible/playbooks/prerequisites.yml -vvv
    ansible-playbook -i /etc/ansible/hosts openshift-ansible/playbooks/deploy_cluster.yml -vvv
    ```


## Config Admin User (master)
```
htpasswd -b /etc/origin/master/htpasswd admin 123.123a
oc adm policy add-cluster-role-to-user cluster-admin admin
systemctl restart origin-master-api
```

## Utility
```
-- Generate ssh key
ssh-keygen -t rsa

-- Copy ssh pub key from local to server (linux)
ssh-copy-id -i ~/.ssh/id_rsa.pub cefcfco.com

-- Copy ssh pub key from local to server (windows)
ssh user@lnxhost "umask 077; test -d .ssh || mkdir .ssh ; cat >> .ssh/authorized_keys || exit 1" < C:\Users\fengdu\.ssh\id_rsa_hyperV.pub

-- Correct the file on linux
vi ~/.ssh/authorized_keys

```

```
-- Copy folder from local to remote
scp -r /c/Users/fengdu/workspace/docker/openshift-ansible root@dev.cefcfco.com:

scp -r /c/Users/fengdu/workspace/doc/openshift/hosts root@feng.com:/etc/ansible/
```