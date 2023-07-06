# -*- mode: ruby -*-
# vi: set ft=ruby :

BRIDGE_NAME = "Bridge Name"
VAGRANTFILE_API_VERSION = "2"
BOX_IMAGE = "generic/ubuntu2204"
BOOT_Y = true
BOOT_N = false

USER = ""
PASS = ""
PROXY_HOST = ""
PROXY_PORT = ""
NO_PROXY = "127.0.0.1,localhost"

$script0 = <<SCRIPT
echo "http_proxy=http://#{PROXY_HOST}:#{PROXY_PORT}/" >> /etc/environment
echo "https_proxy=http://#{PROXY_HOST}:#{PROXY_PORT}/" >> /etc/environment
echo "no_proxy=#{NO_PROXY}" >> /etc/environment
SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.define "linuxHypervm", autostart: BOOT_Y	do |avm|

  avm.vm.provider "hyperv"
  avm.vm.box = BOX_IMAGE
  
  avm.vm.provider :hyperv do |hv|
    hv.ip_address_timeout = 360
    hv.maxmemory = 4096
    hv.vmname = 'linuxHypervm'
    hv.cpus = 2
    hv.enable_virtualization_extensions = true
    hv.linked_clone = true
    hv.enable_enhanced_session_mode = true
  end

  avm.vm.synced_folder ".", "/vagrant", type: "smb",
    smb_password: PASS, smb_username: USER

  avm.vm.network :private_network, bridge: BRIDGE_NAME
  avm.vm.network :forwarded_port, guest: 22, host: 10022, id: "ssh"
  
  avm.vm.provision "shell" do |s|
    s.inline = $script0
    s.args = []
  end

  avm.vm.provision "shell", path: "script\\provision.sh"

end

end
