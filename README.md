# Ansible-Jenkins


## Local Images Creation - On VirtualBox
It can be used in Windows 10/11 (a bit difficult to setup), or you can use Debian/Ubuntu host environemnt.
1. Install git, VirtualBox
2. Install Vagrant, Ansible (Use Windows Subsystem for Linux 2)
3. Install some plugins in WSL2 to allow Ansible and Vagrant to access Windows VirtualBox (Google Search, also [this link https://slavid.github.io/2021/11/28/running-vagrant-ansible-windows-through-wsl2/#configuration ](https://slavid.github.io/2021/11/28/running-vagrant-ansible-windows-through-wsl2/#configuration) ).
4. Change into the project root folder.
5. Download required roles with the following command:
    ```
    ansible-galaxy install -r ./roles/requirements.yml
    ```
6. To create and Run the VM:
    ```
    vagrant up
    ```
7. To Stop the VM:
    ```
    vagrant halt
    ```
8. To Destroy the VM run (CAUTION: The Virtual Disk File will be deleted):
    ```
    vagrant destroy -f
    ```

## Browsing Jenkins
You can browse jenkins either locally on the VirtualBox Host computer or via Public URL.
1. Local HOST Computer:
    [http:localhost:8080/jenkins](http:localhost:8080/jenkins)
2. Public URL:
    [https://hexword.ca/jenkins](https://hexword.ca/jenkins)

## Cloud Image Creation - On Linode (NOT Being Implemented)
It can be used in Windows 10/11 (a bit difficult to setup), or you can use Debian/Ubuntu host environemnt.
1. Install Ansible, Terraform (Use Windows Subsystem for Linux 2)
2. Install some plugins in WSL2 for Ansible (Google Search, also [this link https://slavid.github.io/2021/11/28/running-vagrant-ansible-windows-through-wsl2/#configuration ](https://slavid.github.io/2021/11/28/running-vagrant-ansible-windows-through-wsl2/#configuration) )
3. Change into the project root folder.
4. Change into "tf-linode" subfolder
5. Run:
    ```
    terraform init
    terraform plan
    terraform apply -auto-approve
    ```
6. To Destroy run:
    ```
    terraform destroy -auto-approve
    ```