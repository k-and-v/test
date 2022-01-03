# -*- mode: ruby -*-
# vi: set ft=ruby :
# @k-and-v

# Префикс для LAN сети
LAN="10.1.4."
# Домен который будем использовать для всей площадки
DOMAIN=""
# Массив из хешей, в котором заданы настройки для каждой ВМ
invent=[
  {
    :name    => "k8s-master-1" + DOMAIN,
    :cpu     => "2",
    :memory  => "4096",
#   :gui     => true,
    :lan     => { ip: LAN + "36" },
    :frwd    => true,
    :prt_in  => "8080",
    :prt_out => "8086"
  },
  {
    :name    => "k8s-master-2" + DOMAIN,
    :cpu     => "2",
    :memory  => "4096",
#   :gui     => true,
    :lan     => { ip: LAN + "37" },
    :frwd    => true,
    :prt_in  => "8080",
    :prt_out => "8087"
  },
  {
    :name    => "k8s-ingress-1" + DOMAIN,
    :cpu     => "1",
    :memory  => "2048",
#   :gui     => true,
    :lan     => { ip: LAN + "39" },
    :frwd    => true,
    :prt_in  => "8080",
    :prt_out => "8089"
  },
  {
    :name    => "k8s-node-1" + DOMAIN,
    :cpu     => "2",
    :memory  => "4096",
#   :gui     => true,
    :lan     => { ip: LAN + "32" },
    :frwd    => true,
    :prt_in  => "8080",
    :prt_out => "8082"
  },
  {
    :name    => "k8s-node-2" + DOMAIN,
    :cpu     => "2",
    :memory  => "4096",
#   :gui     => true,
    :lan     => { ip: LAN + "33" },
    :frwd    => true,
    :prt_in  => "8080",
    :prt_out => "8083"
  }
]

Vagrant.configure("2") do |config|
#  config.vm.box = "debian/buster64"
  config.vm.box = "centos/7"
  config.vm.box_check_update = false

#   *** Поднимаем ВМ по списку из 'invent' ***
  invent.each do |machine|
    config.vm.define machine[:name] do |host|
      host.vm.provider "virtualbox" do |vb|
        vb.gui    = machine[:gui] || false
        vb.name   = machine[:name]
        vb.cpus   = machine[:cpu] || "1"
        vb.memory = machine[:memory] || "256"
      end

      host.vm.hostname = machine[:name]
      host.vm.network :private_network, machine[:lan] if machine.key? :lan
      host.vm.network :forwarded_port, guest: machine[:prt_in], host: machine[:prt_out], host_ip: "127.0.0.1" if machine.key? :frwd

##   *** Поднимаем Ansible на всех ВМ, роли подтягиваются из файла inventory ***
#      host.vm.provision :ansible_local do |ansible|
#        ansible.playbook          = "playbook.yaml"
#        ansible.become            = true
#        ansible.limit             = machine[:name]
#        ansible.inventory_path    = "inventory"
#        ansible.galaxy_roles_path = '/home/vagrant/.ansible/roles/'
#        ansible.galaxy_command    = 'ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path}'
#      end
    end
  end
end