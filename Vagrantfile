# vi:syntax=ruby

Vagrant.configure("2") do |config|

  hosts = {
    'node1' => '10.123.1.21',
    'node2' => '10.123.1.22',
    'node3' => '10.123.1.23'
  }

  packages = ["https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm"]
  services = []
  sync_dir = '/home/vagrant/sync'

  repo = nil
#  repo = <<SCRIPT
#  echo "[virt7-docker-common-candidate]
#name=virt7-docker-common-candidate
#baseurl=http://cbs.centos.org/repos/virt7-docker-common-candidate/x86_64/os/
#enabled=1
#gpgcheck=0" > /etc/yum.repos.d/Docker-Candidate.repo
#SCRIPT

  hosts.each do |host, ip|
    config.vm.define host do |h|
      h.vm.box = "centos/7"
      h.vm.network "private_network", ip: ip, virtualbox__intnet: true
      h.vm.hostname = host
      h.vm.synced_folder ".", sync_dir, type: "rsync", rsync__exclude: []
      # This is stupid, but Vagrant seems to not provide any way to override default excludes
      h.vm.synced_folder ".vagrant/", "#{sync_dir}/.vagrant/", type: "rsync"

      unless repo.nil?
        h.vm.provision "Candidate Repo", type: "shell" do |s|
          s.inline = repo
        end
      end
      packages.each do |package|
        h.vm.provision "Package #{package}", type: "shell" do |s|
          s.inline = "yum -y install #{package} || true"
        end
      end
      services.each do |service|
        h.vm.provision "Services", type: "shell" do |s|
          s.inline = "systemctl start #{service}; systemctl enable #{service}"
        end
      end

      if host == hosts.keys[hosts.length - 1] then
        h.vm.provision "Install Ansible", type: "shell" do |s|
          s.inline = 'yum -y install ansible'
        end
        h.vm.provision "Hack/Fix for Vagrant 1.8 not detecting Ansible 2.0", type: "shell" do |s|
          s.inline = '[[ ! -f $1 ]] || grep -F -q "$2" $1 || sed -i "/__main__/a \\    $2" $1'
          s.args = ['/usr/bin/ansible-galaxy', "if sys.argv == ['/usr/bin/ansible-galaxy', '--help']: sys.argv.insert(1, 'info')"]
        end 
        h.vm.provision "Ansible config", type: "shell" do |s|
          s.inline = "cat #{sync_dir}/ansible.cfg > /etc/ansible/ansible.cfg"
        end
        h.vm.provision "ansible_local" do |ansible|
          ansible.limit = "all"
          ansible.provisioning_path = "#{sync_dir}/ansible"
          ansible.inventory_path = "inventory"
          ansible.playbook = "kubernetes.yaml"
        end
      end
    end
  end
end
