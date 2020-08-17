Vagrant.configure("2") do |config|
  config.vm.box = "deargle/metasploitable2"
  config.ssh.username = "msfadmin"
  config.ssh.password = "msfadmin"

  config.vm.network :private_network,
      :ip => "192.168.56.102",
      :libvirt__network_name => "infosec-net",
      :libvirt__dhcp_enabled => false,
      :libvirt__host_ip => "192.168.56.101",
      :autostart => true
end
