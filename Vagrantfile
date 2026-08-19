Vagrant.configure("2") do |config|
  config.vm.box = "security-assignments/metasploitable2"
  # Pinned so the build is reproducible and the box can be pre-added at a known
  # version before `vagrant up` runs. Vagrant merges a box's embedded
  # Vagrantfile at CONFIG-LOAD time, so a box that `vagrant up` fetches itself
  # contributes nothing on that run; pre-adding is what avoids that, and
  # pre-adding needs a version to ask for. 0.0.4 is what this lab has actually
  # been running -- it is what floating to "latest" resolved to on the
  # 2026-08-19 golden-image build.
  config.vm.box_version = "0.0.4"
  config.vm.box_url     = "https://storage.googleapis.com/security-assignments-vagrant-boxes/registry/metasploitable2/metadata.json"
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
