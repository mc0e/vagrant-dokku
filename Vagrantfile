Vagrant::configure('2') do |config|
#  config.vm.define :box1, autostart: false do |box|
  config.vm.define :dokku do |box|
    box.vm.box      = 'trusty64'
    box.vm.box_url  = 'https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box'
    box.vm.hostname = 'dokku.localnet'

    # Provision
    box.vm.provision "shell" do |s|
      s.privileged = true

      s.inline = %{
        set -e
        # Set up apt-cache
#        echo 'Acquire::http { Proxy "http://172.17.41.1:3142"; };' > /etc/apt/apt.conf.d/30-apt-proxy

        # disable ipv6
        echo "net.ipv6.conf.all.disable_ipv6 = 1" >  /etc/sysctl.d/10-ipv6-disable.conf
        echo "net.ipv6.conf.default.disable_ipv6 = 1" >> /etc/sysctl.d/10-ipv6-disable.conf
        echo "net.ipv6.conf.lo.disable_ipv6 = 1" >> /etc/sysctl.d/10-ipv6-disable.conf
        sysctl -w net.ipv6.conf.all.disable_ipv6=1
        sysctl -w net.ipv6.conf.default.disable_ipv6=1
        sysctl -w net.ipv6.conf.lo.disable_ipv6=1
        sed -i 's/^GRUB_CMDLINE_LINUX_DEFAULT=.*/GRUB_CMDLINE_LINUX_DEFAULT="ipv6.disable=1 console=tty0"/' /etc/default/grub
        update-grub

        # disable unwanted services
        service puppet stop
        update-rc.d puppet disable
        service chef-client stop
        update-rc.d chef-client disable



        # To make sure packages are up to date
        export DEBIAN_FRONTEND=noninteractive
        apt-get update
        apt-get --yes --force-yes upgrade


        # Install dokku-alt
        sudo bash -c "$(curl -fsSL https://raw.githubusercontent.com/dokku-alt/dokku-alt/master/bootstrap.sh)"

        # echo deb https://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list
        # echo deb https://dokku-alt.github.io/dokku-alt / > /etc/apt/sources.list.d/dokku-alt.list

        # apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9
        # apt-key adv --keyserver pgp.mit.edu --recv-keys EAD883AF
        # apt-get update -y
        # apt-get install -y --force-yes dokku-alt
      }
    end
  end
end
