# Lab - Metasploitable2 vagrant

I made this repository so that it is easier to `git pull` down any changes to the Vagrantfile onto the kali lab machine.

## How I made it

Starting from the Metaspoitable2 download from rapid7:

* download the metasploitable2 vmdk

  ```
  wget http://downloads.metasploit.com/data/metasploitable/metasploitable-linux-2.0.0.zip
  unzip metasploitable-linux-2.0.0.zip && cd Metasploitable2-Linux
  ```
* Use virt-manager to create a new instance, using the `.vmdk` as a source disk. Boot the instance.
* Edit `/etc/fstab` and remove the line for `/dev/sda`. This should not be hard-coded -- if the vagrant
  box uses VirtIO, it will mount a disk as `/dev/vda` and metasploitable2 will complain on boot.
* use `visudo` to give `msfadmin` passwordless-sudo. This is required so that vagrant can provision the host
  with an additional network interface, as specified in this repo's Vagrantfile.
* shut down the instance.
* use `qemu-img convert` to convert the now-ready .vmdk to .qcow2 format. (vagrant-libvirt only supports qcow2 at time of writing.)

  ```bash
  qemu-img convert -f vmdk -O qcow2 Metasploitable.vmdk Metasploitable.qcow2
  ```
* use `vagrant-libvirt`'s `create_box.sh` convenient script to package the qcow2 into a vagrant box.

  ```bash
  wget https://raw.githubusercontent.com/vagrant-libvirt/vagrant-libvirt/master/tools/create_box.sh
  bash create_box.sh Metasploitable.qcow2
  ```
* optionally, add that box to vagrant, and test that it works, using the below Vagrantfile at a minimum.

  ```bash
  vagrant box add Metasploitable.box --name Metasploitable
  ```

  ```ruby    
  echo <<EOF > Vagrantfile
  Vagrant.configure("2") do |config|
      config.vm.box = "Metasploitable"
      config.ssh.username = "msfadmin"
      config.ssh.password = "msfadmin"
      config.ssh.insert_key = false

      config.vm.synced_folder ".", "/vagrant", disabled: true
  end
  EOF
  ```
* publish the box to vagrant cloud.

  ```bash
  vagrant cloud publish ...
  ```
  * Note: Since `vagrant package` wasn't used to package it, `VIRT_SYSPREP` was not ever
    called, so there is no concern about ssh-hostkeys having been removed, etc.
