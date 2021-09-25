# VAGRANT + WEBSERVERS AND NGINX LOADBALANCER

### Structure
> Load balance <br />
> Webservers <br />
> Controller Node ( where Ansible is installed) <br/>
> Provider - Virtual box
> Provisioning - Ansible

### Basic Requirements

0. Install [Vagrant](https://www.vagrantup.com/)
0. Install Virtual Box

1. Steps to reproduce


0. Vagrant file :

    ```sh
        Vagrant.configure("2") do |config|
        config.vm.box = "ubuntu/ubuntu-18.04"

        config.vm.define "lb" do |machine|
            machine.vm.network "private_network", ip: "172.17.177.21"
            machine.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
        end

        config.vm.define "web1" do |machine|
            machine.vm.network "private_network", ip: "172.17.177.22"
        end

        config.vm.define "web2" do |machine|
            machine.vm.network "private_network", ip: "172.17.177.23"
        end

        config.vm.define "controller" do |machine|
            machine.vm.network "private_network", ip: "172.17.177.11"

            machine.vm.provision "ansible_local" do |ansible|
                ansible.playbook = "provisioning/playbook.yml"
                ansible.limit = "all"
                ansible.inventory_path = "provisioning/hosts"
                ansible.config_file = "provisioning/ansible.cfg"
            end

            machine.vm.synced_folder ".", "/vagrant", mount_options: [ "umask=077" ]
             end
            end
    ```

   
  
  ### Steps to run
  > Install Vagrant  
  > Install Virtual box https://www.virtualbox.org/wiki/Downloads vagrant box add ubuntu/bionic64
  0. Clone Git file
  1. Change Directory to the file on your machine, open terminal and run
  2.
  ```sh
      vagrant Up
   ```
  3. Once All installation is complete 
  4. Open Virtual Box and navigate to  the Load balancer machine
  5. From the Loadbalancer VM , Open terminal and run the command:
    ```sh
           curl -i 172.17.177.21
    ```
 6. On the Host Browser Load http://hello or 172.17.177.21
  
