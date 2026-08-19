Vagrant.configure("2") do |config|
  config.vm.box = "deargle/metasploitable2"
  config.ssh.username = "msfadmin"
  config.ssh.password = "msfadmin"
  config.ssh.insert_key = false

  config.vm.provider :libvirt do |libvirt|
    libvirt.nic_model_type = 'rtl8139'
    libvirt.management_network_name = 'vagrant-libvirt'
    libvirt.management_network_autostart = true
    # Keep the shared 'vagrant-libvirt' management network on `vagrant destroy`.
    # Without it, destroying any one lab VM deletes the network the other lab
    # VMs' domains still reference, and their next boot fails.
    libvirt.management_network_keep = true
  end

  config.vm.network :private_network,
      :ip => "192.168.56.102",
      :libvirt__network_name => "infosec-net",
      #:model_type => 'rtl8139',
      :libvirt__dhcp_enabled => false,
      :libvirt__host_ip => "192.168.56.101",
      # Do not destroy infosec-net when THIS vm is destroyed -- the other lab
      # VMs share it, and vagrant-libvirt deletes a network unless a RUNNING
      # domain is attached (merely defined-but-shut-off domains do not count).
      # NOTE: always_destroy is undocumented in the vagrant-libvirt README --
      # real in 0.12.2; re-verify it on any plugin upgrade.
      :libvirt__always_destroy => false,
      :autostart => true

 config.vm.synced_folder ".", "/vagrant", disabled: true
end
