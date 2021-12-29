# Tested environment
# OS: macOS Montery
# Vagrant: 2.2.19
# Hypervisor: VMware Fusion 12.2.1
# Box: centos/8
# Required plugin:
# vagrant-hostmanager (1.8.9, global)
# vagrant-vmware-desktop (3.0.1, global)

VAGRANTFILE_API_VERSION = "2"

vm_env = {
  "node01" => { :cpus => 1, :mem => 2048, :image => "generic/centos8" },
#   "node02" => { :cpus => 1, :mem => 1024, :image => "generic/centos8" },
}

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

    # Automanage /etc/hosts
    if Vagrant.has_plugin?('vagrant-hostmanager')
        config.hostmanager.enabled = true
        config.hostmanager.manage_host = true
        config.hostmanager.manage_guest = false
        config.hostmanager.ignore_private_ip = false
        config.hostmanager.include_offline = true
    end

    vm_env.each_with_index do |(hostname, column), index|
        config.vm.define hostname do |node|
            node.vm.provider :vmware_fusion do |fusion, override|
                # Image
                config.vm.box = column[:image]
                
                # Virtual Machine
                override.vm.hostname = hostname
                override.vm.network :private_network
                
                # Hardware
                fusion.vmx["numvcpus"] = column[:cpus]
                fusion.vmx["memsize"] = column[:mem]
            end
            
            # Copy Vagrant ssh-config into localhost
            node.trigger.after :up do |up|
                up.info = "Creating SSH config at ~/.ssh/config.d/#{hostname}"
                up.run = {inline: "bash -c 'vagrant ssh-config #{hostname} > #{Dir.home}/.ssh/config.d/#{hostname}'"}
            end

            # Provision VM
            node.vm.provision 'ansible' do |ansible|
                ansible.playbook = 'main.yml'
            end

            # Remove generated Vagrant ssh-config from localhost
            node.trigger.after :destroy do |destroy|
                destroy.warn = "Removing SSH config at ~/.ssh/config.d/#{hostname}"
                destroy.run = {inline: "bash -c 'rm #{Dir.home}/.ssh/config.d/#{hostname}'"}
            end
        end
    end
end
