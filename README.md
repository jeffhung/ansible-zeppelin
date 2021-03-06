# Ansible Role: jeffhung.zeppelin

Ansible playbook for installing [Zeppelin](https://zeppelin.apache.org/) on CentOS/Debian Linux


## Installing

Install this playbook:

	ansible-galaxy install jeffhung.zeppelin

## Testing with Vagrant

After [installing](#installing) this playbook, you could create a working
folder with the following `Vagrantfile` and `site.yml` in folder root, to run
vagrant box locally:

`Vagrantfile`:

```
Vagrant.configure("2") do |config|
  config.vm.box = "centos/6"
# config.vm.box = "ubuntu/trusty64"
  config.vm.provider "virtualbox" do |v|
    v.memory = 1024 # Spark need much larger memory
  end

  if config.vm.box.start_with?("centos/")
    if Vagrant.has_plugin?("vagrant-sshfs")
      config.vm.synced_folder ".", "/vagrant", type: "sshfs"
    else
      config.vm.synced_folder ".", "/vagrant", type: "rsync"
    end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "site.yml"
  end

  # zeppelin console service port:
  config.vm.network :forwarded_port, host: 8080, guest: 8080
end
```

`site.yml`:

```
- hosts: default
  roles:
		- jeffhung.zeppelin
```

Start and stop Zeppelin:

```
sudo /opt/zeppelin/bin/zeppelin-daemon.sh start
sudo /opt/zeppelin/bin/zeppelin-daemon.sh stop
```


## Role Variables

### `zeppelin_version`

Default: `0.7.1`

Set which Zeppelin version to use.

Could be:

* `0.7.1`

### `zeppelin_environment`

Default: `devel`

Which environment to deploy.

Could be:

* `devel`: provision devel-time environment for developing with Zeppelin
* `build`: provision build-time environment for packaging Zeppelin

### `zeppelin_cache_dir`

Cache directory for holding downloaded/generated files.

The default is `/vagrant/files` so cache persist between boxes.


## License

BSD

