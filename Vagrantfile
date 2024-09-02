# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  groups = {
    "postgresql" => ["node-1", "node-2", "node-3"],
    "pgpool2" => ["node-4", "node-5", "node-6"]
  }
  nodes = groups.values.flatten.uniq
  nodes.each_with_index do |hostname, index|
    config.vm.define hostname do |node|
      node.vm.box = "rockylinux/9"
      node.vm.synced_folder ".", "/vagrant", disabled: true
      node.vm.hostname = hostname + ".example.com"
      node.vm.network "private_network", ip: "192.168.56.#{101 + index}"
      if hostname.equal?(nodes.last)
        node.vm.provision "ansible" do |ansible|
          ansible.limit = "all"
          ansible.playbook = "playbook.yml"
          ansible.groups = groups
          ansible.extra_vars = {
            postgresql_version: "16", pgpool2_version: "4.5",
          }
        end
      end
    end
  end
end
