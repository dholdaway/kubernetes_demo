Vagrant.configure("2") do |config|
    config.vm.define "k8s" do |k8s|
        k8s.vm.box = "ubuntu/trusty64"
        k8s.vm.network "private_network", ip: "192.168.0.251"
        k8s.vm.hostname = "k8s.example.com"
        k8s.vm.provider "virtualbox" do |v|
            v.memory = 1024
        end
    end
end
