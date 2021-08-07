# -*- mode: ruby -*-
# vi: set ft=ruby :

boxes = {
  'netology1' => '10',
  'netology2' => '60',
  'netology3' => '90',
  'netology4' => '100',
  'netology5' => '110',
}

Vagrant.configure("2") do |config|
  config.vm.network "private_network", virtualbox__intnet: true, auto_config: false
  config.vm.box = "bento/ubuntu-20.04"

  boxes.each do |k, v|
    config.vm.define k do |node|
      node.vm.provision "shell" do |s|
        s.inline = "hostname $1;"\
          "ip addr add $2 dev eth1;"\
          "ip link set dev eth1 up;"\
          "apt -y install nginx;"
        s.args = [k, "172.28.128.#{v}/24"]
      end
    end
  end

end
