# -*- mode: ruby -*-
# vi: set ft=ruby :

BASEDIR = File.dirname(__FILE__)
WORKDIR = "#{BASEDIR}/work"

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"

  config.vm.hostname = "tfe.dev"

  config.vm.provider "virtualbox" do |vb, override|

    override.vm.network "private_network", ip: "203.0.113.11"

    vb.memory = "4000"

    data_disk_path = "#{WORKDIR}/data-vol.vdi"
    unless File.exist?(data_disk_path)
        vb.customize [
            "createhd",
            "--filename", data_disk_path,
            "--size", 15 * 1024,
        ]
    end

    vb.customize [
        "storageattach",
        :id,
        "--storagectl", "SCSI",
        "--port", 2,
        "--device", 0,
        "--type", "hdd",
        "--medium", data_disk_path,
    ]

  end

  config.vm.provision "shell", path: "tfe_provision.sh"
end
