require 'fileutils'

Vagrant.require_version ">= 1.6.0"

CLOUD_CONFIG_CORE01_PATH = File.join(File.dirname(__FILE__), "core01.user-data")
CONFIG = File.join(File.dirname(__FILE__), "config.rb")

# Attempt to apply the deprecated environment variable NUM_INSTANCES to
# $num_instances while allowing config.rb to override it
if ENV["NUM_INSTANCES"].to_i > 0 && ENV["NUM_INSTANCES"]
  $num_instances = ENV["NUM_INSTANCES"].to_i
end

if File.exist?(CONFIG)
  require CONFIG
end

Vagrant.configure("2") do |config|
  # always use Vagrants insecure key
  config.ssh.insert_key = false

  config.vm.box = "coreos-%s" % $update_channel
  if $image_version != "current"
      config.vm.box_version = $image_version
  end
  config.vm.box_url = "http://%s.release.core-os.net/amd64-usr/%s/coreos_production_vagrant.json" % [$update_channel, $image_version]

  config.vm.provider :virtualbox do |v|
    # On VirtualBox, we don't have guest additions or a functional vboxsf
    # in CoreOS, so tell Vagrant that so it can be smarter.
    v.check_guest_additions = false
    v.functional_vboxsf     = false
  end

  # plugin conflict
  if Vagrant.has_plugin?("vagrant-vbguest") then
    config.vbguest.auto_update = false
  end

  config.vm.hostname = "docker-registry"

  if $expose_docker_tcp
    config.vm.network "forwarded_port", guest: 2375, host: ($expose_docker_tcp), auto_correct: true
  end

  $forwarded_ports.each do |guest, host|
    config.vm.network "forwarded_port", guest: guest, host: host, auto_correct: true
  end

  config.vm.provider :virtualbox do |vb|
    vb.gui = $vb_gui
    vb.memory = $vb_memory
    vb.cpus = $vb_cpus
    vb.name = "coreos-docker-registry"
  end

  ip = "172.17.8.128"
  config.vm.network :private_network, ip: ip

  config.vm.provision :file, :source => "#{CLOUD_CONFIG_CORE01_PATH}", :destination => "/tmp/vagrantfile-user-data"
  config.vm.synced_folder "registry", "/registry", id: "core", nfs: true, mount_options: ['nolock,vers=3,udp']
  config.vm.provision :shell, :inline => "mv /tmp/vagrantfile-user-data /var/lib/coreos-vagrant/", :privileged => true

  config.vm.provision 'shell' do |s|
    s.path = 'provision.sh'
  end

end
