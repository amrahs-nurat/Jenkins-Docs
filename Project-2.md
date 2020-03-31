#### Deploying War file using Ansible with the help of Jenkins CI/CD

In this Project:

- Enable password less authentication b/w ansible and tomcat server
- Install "publish over ssh" plugin on jenkins server
- Write a playbook to copy jar/war file on to tomcat server
- Modify jenkins Jobs to copy Artifacts and initiate Ansible Playbook

In this Project i'm going to use three different servers (vms or cloud instances) with its proper configuration:

- For Jenkin Server
- For Tomcat Server
- For Ansible Server

##### Enable password less authentication b/w ansible and tomcat server
- Login to ansible server and create a new user "ansadmin"
    useradd ansadmin
- Add "ansadmin" user in visudo file and make an entry for this user
    ansadmin ALL=(ALL) NOPASSWORD: ALL
- Setting up password for user
    passwd ansadmin
- Similarly create a user in tomcat server as well.
    useradd ansadmin
- Add "ansadmin" user in visudo file and make an entry for this user
    ansadmin ALL=(ALL) NOPASSWORD: ALL
- Setting up password for user
    passwd ansadmin
__
- In ansible server, login as ansadmin user and generated the key
    ssh-keygen
    ssh-copy-id tomcat_server_ip
- Add tomcat server ip in /etc/ansible/hosts
    [webservers]
    tomcat_server_ip 
- Testing communication using 
    ansible all -m ping
##### Install "publish over ssh" plugin on jenkins server
- Login to jenkins server and install publish over ssh plugin 
    -> manage Plugin -> Available -> search for the plugin name
###### Write a playbook to copy jar/war file on to tomcat server
- In ansible server create a playbook, in /opt/playbooks/copyfile.yml
```
---
- hosts: all
  become: true
  tasks: 
    - name: copy jar/war onto tomcat servers
      copy:
          src: /opt/playbooks/webapp/target/webapp.war
          dest: /opt/apache-tomcat-8.5.32/webapps
```
##### Modify jenkins Jobs to copy Artifacts and initiate Ansible Playbook
- Add ansible server in jenkins so that it can initiate playbook
    -> configure system -> Ssh servers -> put information about the ansible server (Name,Hostname,username,password) -> Test connection
- Choose exiting created Jobs -> configure -> remove Post-Build Action 
- click on Post steps (This will only available when publish over ssh plugin installed on jenkins)
- select "send files or execute commands over ssh" 
    ssh_server(ansible server)
    source files (webapp/target/*.war)
    Remote Directory (//opt/playbooks) above source file will copied here along with define directory
- Again select "send files or execute commands over ssh" and put value in exec section 
    exec command (ansible-playbook /opt/playbooks/copyfile.yml)
- Apply and Save it, Build it


