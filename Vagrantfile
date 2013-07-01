# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant::Config.run do |config|
  config.vm.box = "precise32-bfk"
  config.vm.box_url = "http://files.vagrantup.com/precise32.box"

  config.vm.network :hostonly, "33.33.33.10"
  config.vm.share_folder "projects", "/projects", "/projects", :nfs => true

  config.vm.customize ["modifyvm", :id, "--memory", 4096, "--cpus", 2]
  config.ssh.forward_agent = true

  config.vm.forward_port 3000, 3000
  config.vm.forward_port 3306, 3306

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["cookbooks", "site-cookbooks"]
    chef.add_recipe('apt')
    chef.add_recipe('mysql::server')
    chef.add_recipe('mysql::client')
    chef.add_recipe('rvm::user')
    chef.add_recipe('gemrc')
    chef.add_recipe('git')
    chef.add_recipe('vim')
    #chef.add_recipe('xvfb')
    #chef.add_recipe('firefox')
    #chef.add_recipe('imagemagick')
    #chef.add_recipe('phantomjs')
	
    chef.add_recipe('redis::source')
    chef.add_recipe('bashrc::user')
    chef.add_recipe('bash-profile')
    chef.add_recipe('bash-completion')
    chef.add_recipe('git-config')
 
    # You may also specify custom JSON attributes:
    #chef.json = { :mysql_password => "foo" }
    chef.json = {
      :rvm => {
        :user_installs => [ {
          :user   => 'vagrant',
          :rvmrc => {
            :rvm_always_trust_rvmrc_flag => 1,
            :rvm_install_on_use_flag     => 1,
            :rvm_trust_rvmrcs_flag       => 1,
          },
        } ]
      }, 
      :bashrc => {
        :user_installs => [ {
          :user   => 'vagrant'
        } ]
      },
      "bash-profile" => {
        :user => 'vagrant', 
        :custom => [
	  "export APP_HOST='default'",
	  "export HEADLESS=1"
        ]
      },
      :git => {
        :global => {
          "user.email" => "vprokopchuk@gmail.com", 
          "user.name" => "\"Valeriy Prokopchuk\"", 
          "core.editor" => "vim", 
          "help.autocorrect" => 1,
          "color.ui" => "true"

        },
        :user => 'vagrant'
      },
      :mysql => {
	:server_root_password => '',
	:server_debian_password => '',
	:server_repl_password => '',
        :socket => '/var/run/mysqld/mysqld.sock',
        :client => {
	  :packages => ['libmysqlclient-dev']
	}
      } 
    } 
  end
end
