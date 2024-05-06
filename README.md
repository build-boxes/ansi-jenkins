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
    - if reverse proxy is setup in your separate Web Server.....  
    [https://<<YOUR-DOMAIN-NAME>>/jenkins](https://<<YOUR-DOMAIN-NAME>>/jenkins)

## Cloud Image Creation - On Linode
It can be used in Windows 10/11 (a bit difficult to setup), or you can use Debian/Ubuntu host environemnt.
1. Install Ansible, Terraform (Use Windows Subsystem for Linux 2)
2. Install some plugins in WSL2 for Ansible (Google Search, also [this link https://slavid.github.io/2021/11/28/running-vagrant-ansible-windows-through-wsl2/#configuration ](https://slavid.github.io/2021/11/28/running-vagrant-ansible-windows-through-wsl2/#configuration) )
3. Change into the project root folder.
4. Download required roles with the following command:
    ```
    ansible-galaxy install -r ./roles/requirements.yml
    ```   
5. Change into "tf-linode*" subfolder
6. Run:
    ```
    terraform init
    terraform plan
    terraform apply -auto-approve
    ```
7. To Destroy run:
    ```
    terraform destroy -auto-approve
    ```
## Restore a Previous Backup (Optional)
You can optionaly also restore a previously backed up Jenkins-Home-Folder using this ansible script. Related Vars and secrets are highlighted below. The backed up filename.tar.gz should be available on the Ansible controller host. It will be uploded to the created (remote) VM by the script.
1. [https://github.com/build-boxes/ansi-jenkins/blob/main/vars/vars.yml#L30](https://github.com/build-boxes/ansi-jenkins/blob/main/vars/vars.yml#L30) Line 30 to Line 32.
2. [https://github.com/build-boxes/ansi-jenkins/blob/main/vars/secrets_shadow.yml#L22](https://github.com/build-boxes/ansi-jenkins/blob/main/vars/secrets_shadow.yml#L22) Line 22 and Line 23.


## TODO  
  
```
Terraform

https://www.linkedin.com/pulse/how-run-terraform-script-jenkins-step-by-step-guide-praveen-dandu
https://blog.digger.dev/how-to-run-terraform-in-jenkins/
https://spacelift.io/blog/terraform-jenkins

https://developer.hashicorp.com/terraform/install

https://github.com/robertdebock/ansible-role-terraform
```

```
"java -jar /opt/jenkins-cli.jar -s http://localhost:{{ jenkins_http_port }}{{ jenkins_url_prefix }} -auth {{ jenkins_admin_username }}:{{ jenkins_admin_password }}"


Get Jenkins-Crumb json:
curl -s -X GET http://localhost:8080/jenkins/crumbIssuer/api/json --user USERNAME:PASSWORD | python3 -c 'import sys, json; print(json.load(sys.stdin)["crumb"])'

curl -s -X GET http://localhost:8080/jenkins/crumbIssuer/api/json --user USERNAME:PASSWORD | jq '.crumb' | sed 's/\"//g'

curl -s -X GET http://localhost:8080/jenkins/crumbIssuer/api/json --user USERNAME:PASSWORD | jq ".crumb" | sed "s/\"//g"

Get Jenkins-Crumb XML:
curl -u "USERNAME:PASSWORD" -s --cookie-jar /tmp/cookies 'http://localhost:8080/jenkins/crumbIssuer/api/xml?xpath=concat(//crumbRequestField,":",//crumb)'
OR
JENKINS_CRUMB=$(curl -u "USERNAME:PASSWORD" -s --cookie-jar /tmp/cookies 'http://localhost:8080/jenkins/crumbIssuer/api/xml?xpath=concat(//crumbRequestField,":",//crumb)')

This one is working:
ACCESS_TOKEN=$(curl -u "USERNAME:PASSWORD" -H $(curl -u "USERNAME:PASSWORD" -s --cookie-jar /tmp/cookies 'http://localhost:8080/jenkins/crumbIssuer/api/xml?xpath=concat(//crumbRequestField,":",//crumb)') -s --cookie /tmp/cookies 'http://localhost:8080/jenkins/me/descriptorByName/jenkins.security.ApiTokenProperty/generateNewToken' --data 'newTokenName=AccessToken001' | jq -r '.data.tokenValue')
OR
ACCESS_TOKEN=$(curl -u "USERNAME:PASSWORD" -H $JENKINS_CRUMB -s --cookie /tmp/cookies 'http://localhost:8080/jenkins/me/descriptorByName/jenkins.security.ApiTokenProperty/generateNewToken' --data 'newTokenName=AccessToken001' | jq -r '.data.tokenValue')


echo $ACCESS_TOKEN

Consective Calls:
curl -u $JENKINS_USER:$ACCESS_TOKEN -H $JENKINS_CRUMB ..........

java -jar /opt/jenkins-cli.jar [-s URL] -auth USERNAME:$ACCESS_TOKEN who-am-i

```

```
TASK [Get Jenkins API crumb] ***************************************************
fatal: [debian]: FAILED! => {"changed": false, "msg": "Unsupported parameters for (ansible.legacy.uri) module: failed_when, register. Supported parameters include: attributes, body, body_format, ca_path, ciphers, client_cert, client_key, creates, decompress, dest, follow_redirects, force, force_basic_auth, group, headers, http_agent, method, mode, owner, remote_src, removes, return_content, selevel, serole, setype, seuser, src, status_code, timeout, unix_socket, unredirected_headers, unsafe_writes, url, url_password, url_username, use_gssapi, use_netrc, use_proxy, validate_certs (attr, password, user)."}

PLAY RECAP *********************************************************************
debian                     : ok=69   changed=30   unreachable=0    failed=1    skipped=59   rescued=0    ignored=0   

Ansible failed to complete successfully. Any error output should be
visible above. Please fix these errors and try again.

```