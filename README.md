# Terraform vSphere VM Creation & Ansible Domain Join

## Description
This project automates the creation of virtual machines (VMs) in a VMware environment using Terraform and configures these VMs to join a domain using an Ansible playbook with realm as a SSSD wrapper. It supports both Windows and Linux VMs. Additionally, a Jenkinsfile is included for automating the process of cloning this repository and executing the Terraform and Ansible scripts.

## Demo
![terraform-jenkins](https://github.com/techmatlock/terraform-jenkins-vsphere-vm/assets/2618095/a3e302ea-0471-40c9-bacd-55790cd1de13)

## Prerequisites
* Terraform <= v1.6.1
* Ansible <= 2.15.6
* VMware vSphere environment <= 8.0.1.00000
* Jenkins with these plugins: Active Choices, Git

## Installation
Fork this repository.

```
git clone https://github.com/techmatlock/terraform-jenkins-vsphere-vm.git
```

1. Create a new Jenkins job
2. Give your job a name and choose "Pipeline"
3. Paste the Jenkinsfile contents into the Script section located below Pipeline.

## Usage
You need three credentials set up in Jenkins and their ID's added to the Jenkinsfile.

![Screenshot 2023-11-19 at 5 21 40 PM](https://github.com/techmatlock/terraform-jenkins-vsphere-vm/assets/2618095/7e4fdc90-b1a4-470d-add4-17fe01df701e)

The Ansible playbook assumes you've already encrypted your files to hide sensitive variables.

I only encrypt the file "vault" in the path below (with the command ansible-vault encrypt vault):
* Ansible/group_vars/all/vault

## Configuration

![File tree](https://i.imgur.com/fkRKwn7.png)

* If you want to deploy more than one virtual machine, here's an example of modifying the virtual_machines map variable.
```
terraform init
terraform plan -var 'vm_domain=${params.DOMAIN}' \
               -var 'vsphere_network=${params.VSPHERE_NETWORK}' \
               -var 'vm_ipv4_netmask=$VSPHERE_NETMASK' \
               -var 'vm_ipv4_gateway=$VSPHERE_GATEWAY' \
               -var 'vsphere_compute_cluster=${params.COMPUTE_CLUSTER}' \
               -var 'vsphere_datastore=${params.DATASTORE}' \
               -var 'vsphere_folder=${params.ROOT_FOLDER}' \
               -var 'virtual_machines={vm1: {name: "${params.VM_NAME1}", ip: "${params.IP1}"},
                                       vm2: {name: "${params.VM_NAME2}", ip: "${params.IP2}"},
                                       vm3: {name: "${params.VM_NAME3}", ip: "${params.IP3}"}}' -out myplan
terraform apply --auto-approve myplan
```
## Contributing
Contributions to this project are welcome. Please fork the repository and submit a pull request.
