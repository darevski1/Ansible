servers=[
    {
        :hostname => "ansible",
        :ip => "192.168.56.10",
        :box => "ubuntu/jammy64",
        :ram => 712,
        :cpu => 1
    },
        {
        :hostname => "web",
        :ip => "192.168.56.11",
        :box => "ubuntu/jammy64",
        :ram => 712,
        :cpu => 1
    },
    {
        :hostname => "db",
        :ip => "192.168.56.12",
        :box => "ubuntu/jammy64",
        :ram => 712,
        :cpu => 1
    }
    ]

    Vagrant.configure(2) do |config|
        servers.each do |machine|
            config.vm.define machine[:hostname] do |node|
                node.vm.box = machine[:box]
                node.vm.hostname = machine[:hostname]
                node.vm.network "private_network", ip: machine[:ip]
                node.vm.provider "virtualbox" do |vb|
                    vb.customize ["modifyvm", :id, "--memory", machine[:ram]]
                end
            end
        end
    end