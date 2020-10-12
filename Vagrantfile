Vagrant.configure("2") do |config|
  config.vm.box = "deargle/metasploitable2"
  config.ssh.username = "msfadmin"
  config.ssh.password = "msfadmin"
  
  config.vm.provider :libvirt do |libvirt|
    libvirt.nic_model_type = 'rtl8139'
  end

  config.vm.network :private_network,
      :ip => "192.168.56.102",
      :libvirt__network_name => "infosec-net",
      #:model_type => 'rtl8139',
      :libvirt__dhcp_enabled => false,
      :libvirt__host_ip => "192.168.56.101",
      :autostart => true

 config.vm.synced_folder ".", "/vagrant", disabled: true
end
