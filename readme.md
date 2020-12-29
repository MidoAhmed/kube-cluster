
# Create Kubernetes Cluster Using Kubeadm
 - 3 nodes (1 master, 2 workers) : Ubuntu 18.04, 4GB RAM and 2 vCPUs
 - Kubernetes v1.17.0
 - Docker v19.03
 - Ansible v2.10.3

## Prerequisites:

- Three servers running Ubuntu 18.04 with at least 2GB RAM and 2 vCPUs each. You should be able to SSH into each server as the root user with your SSH key pair.
        1 master and 2 workers
- Ansible installed on your local machine.
- git clone https://github.com/MidoAhmed/kube-cluster.git ~/kube-cluster

## Steps:

1. Creating a Non-Root User on All Remote Servers :
    * execute the playbook by locally running
    > $ ansible-playbook -i ~/kube-cluster/ansible/hosts.ini ~/kube-cluster/ansible/playbooks/initial.yml

2.  Installing Kubernetetes’ Dependencies
    * execute the playbook by locally running
    > $ ansible-playbook -i ~/kube-cluster/ansible/hosts.ini ~/kube-cluster/ansible/playbooks/kube-dependencies.yml

    - Installs Docker, the container runtime.
    - Installs apt-transport-https, allowing you to add external HTTPS sources to your APT sources list.
    - Adds the Kubernetes APT repository’s apt-key for key verification.
    - Adds the Kubernetes APT repository to your remote servers’ APT sources list.
    - Installs kubelet and kubeadm.
    - Installs kubectl on your master node.
    
 All system dependencies are now installed :)

3. Setting Up the Master Node

    * execute the playbook by locally running
    > $ ansible-playbook -i ~/kube-cluster/ansible/hosts.ini ~/kube-cluster/ansible/playbooks/master.yml

    * Verification: Check the status of the master node
    > $ ssh ubuntu@master_ip\
    > $ kubectl get nodes

You should see that the master in a Ready state, if it is smile ;)

4. Setting Up the Worker Nodes

    * execute the playbook by locally running
    > $ ansible-playbook -i ~/kube-cluster/ansible/hosts.ini ~/kube-cluster/ansible/playbooks/workers.yml
    
your cluster is now fully set up and functional, with workers ready to run workloads. 

5. Verifying the Cluster
    
    * let’s verify that the cluster is working as intended.
    > $ ssh ubuntu@master_ip\
    > $ kubectl get nodes

    | NAME        | STATUS      | ROLES       | AGE     | VERSION |
    | ----------- | ----------- | ----------- | --------|---------|
    | instance01  | Ready       | master      | 3h4m    | v1.17.0 |
    | instance02  | Ready       | none        | 4m26s   | v1.17.0 |
    | instance02  | Ready       | none        | 31s     | v1.17.0 |

Now that your cluster is verified successfully, 



## Resources:
- https://www.digitalocean.com/community/tutorials/how-to-create-a-kubernetes-cluster-using-kubeadm-on-ubuntu-18-04#step-2-%E2%80%94-creating-a-non-root-user-on-all-remote-servers
- https://jhooq.com/14-steps-to-install-kubernetes-on-ubuntu-18-04-and-16-04/
