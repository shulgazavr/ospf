MACHINES ={
    :R1 => {
        :box_name => "ubuntu/focal64",
        :vm_name => "R1",
        :net => [
            {ip: '10.0.10.1', adapter: 2, netmask: "255.255.255.252", virtualbox_intnet: "R1-R2"},
            {ip: '10.0.12.1', adapter: 3, netmask: "255.255.255.252", virtualbox_intnet: "R1-R3"},
            {ip: '192.168.10.1', adapter: 4, netmask: "255.255.255.252", virtualbox_intnet: "N1"},
            {ip: '192.168.50.10', adapter: 5},
        ]
    },
    :R2 => {
        :box_name => "ubuntu/focal64",
        :vm_name => "R2",
        :net => [
            {ip: '10.0.10.2', adapter: 2, netmask: "255.255.255.252", virtualbox_intnet: "R1-R2"},
            {ip: '10.0.11.2', adapter: 3, netmask: "255.255.255.252", virtualbox_intnet: "R2-R3"},
            {ip: '192.168.20.1', adapter: 4, netmask: "255.255.255.252", virtualbox_intnet: "N2"},
            {ip: '192.168.50.11', adapter: 5},
        ]
    },
    :R3 => {
        :box_name => "ubuntu/focal64",
        :vm_name => "R3",
        :net => [
            {ip: '10.0.12.2', adapter: 2, netmask: "255.255.255.252", virtualbox_intnet: "R1-R3"},
            {ip: '10.0.11.1', adapter: 3, netmask: "255.255.255.252", virtualbox_intnet: "R2-R3"},
            {ip: '192.168.30.1', adapter: 4, netmask: "255.255.255.252", virtualbox_intnet: "N3"},
            {ip: '192.168.50.12', adapter: 5},
        ]
    }
}

Vagrant.configure("2") do |config|
    MACHINES.each do |boxname, boxconfig|
        config.vm.define boxname do |box|
            box.vm.box = boxconfig[:box_name]
            box.vm.host_name = boxconfig[:vm_name]

            if boxconfig[:vm_name] == "R3"
                box.vm.provision "ansible" do |ansible|
                    ansible.playbook = "ansible/provision.yml"
                    ansible.inventory_path = "ansible/hosts"
                    ansible.host_key_checking = "false"
                    ansible.limit = "all"
                end
            end

            boxconfig[:net].each do |ipconf|
                box.vm.network "private_network", ipconf
            end
        end
    end
end
        

