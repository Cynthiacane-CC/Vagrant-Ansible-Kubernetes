# # -*- mode: ruby -*-
# # vi: set ft=ruby :

# # All Vagrant configuration is done below. The "2" in Vagrant.configure
# # configures the configuration version (we support older styles for
# # backwards compatibility). Please don't change it unless you know what
# # you're doing.

# 1st install the guest_ansible pluggin: vagrant plugin install vagrant-guest_ansible because ansible does not work on a Windows control machine, so you basically have to:

IMAGE_NAME = "bento/ubuntu-16.04"
N = 2

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false #Vagrant will not automatically add a keypair to the guest.
    provisioner = Vagrant::Util::Platform.windows? ? :guest_ansible : :ansible 
    
    config.vm.provider "virtualbox" do |v|
        v.memory = 1024
        v.cpus = 2
    end
      
    config.vm.define "k8s-master" do |master|
        master.vm.box = IMAGE_NAME
        master.vm.network "private_network", ip: "10.0.2.10"#"192.168.50.10"
        master.vm.hostname = "k8s-master"
         # master.vm.network "public_network"
        master.vm.synced_folder ".", "/tmp/vagrant-shell", owner: "vagrant",
        group: "vagrant", :mount_options => ["dmode=777", "fmode=666"]
        master.vm.provision :guest_ansible do |ansible|
        
            ansible.playbook = "kubernetes-setup/master-playbook.yml"
            ansible.extra_vars = {
                node_ip: "10.0.2.10" #"192.168.50.10",
            }
        end
    end

    (1..N).each do |i|
        config.vm.define "node-#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "private_network", ip:"10.0.2.#{i + 10}"  #"192.168.50.#{i + 10}"
         #  node.vm.network "public_network"
            node.vm.hostname = "node-#{i}"
            node.vm.synced_folder ".", "/tmp/vagrant-shell", owner: "vagrant", group: "vagrant", :mount_options => ["dmode=777", "fmode=666"]
            node.vm.provision :guest_ansible do |ansible|
                ansible.playbook = "kubernetes-setup/node-playbook.yml"
                ansible.extra_vars = {
                    node_ip: "10.0.2.#{i + 10}", #"192.168.50.#{i + 10}",
                }
            
            
            end
        end
    end
end 















# IMAGE_NAME = "bento/ubuntu-16.04"
# N = 2

# Vagrant.configure("2") do |config|
#     config.ssh.insert_key = false

#     config.vm.provider "virtualbox" do |v|
#         v.memory = 1024
#         v.cpus = 2
#     end
      
#     config.vm.define "k8s-master" do |master|
#         master.vm.box = IMAGE_NAME
#         master.vm.network "private_network", ip: "192.168.50.10"
#         master.vm.hostname = "k8s-master"
#         master.vm.provision "ansible" do |ansible| # "ansible" do |ansible|
#             ansible.compatibility_mode = "2.0" # i added this line per the error
#             ansible.playbook = "kubernetes-setup/master-playbook.yml"
#             ansible.extra_vars = {
#                 node_ip: "192.168.50.10",
#             }
#         end
#     end

#     (1..N).each do |i|
#         config.vm.define "node-#{i}" do |node|
#             node.vm.box = IMAGE_NAME
#             node.vm.network "private_network", ip: "192.168.50.#{i + 10}"
#             node.vm.hostname = "node-#{i}"
#             node.vm.provision "ansible" do |ansible| # "ansible" do |ansible|
#                 ansible.compatibility_mode = "2.0"
#                 ansible.playbook = "kubernetes-setup/node-playbook.yml"
#                 ansible.extra_vars = {
#                     node_ip: "192.168.50.#{i + 10}",
#                 }
#             end
#         end
#     end
# end




