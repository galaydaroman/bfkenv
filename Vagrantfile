# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant::Config.run do |config|
  config.vm.box = 'precise64'
  config.vm.box_url = 'http://files.vagrantup.com/precise64.box'

  config.ssh.forward_agent = true

  config.vm.forward_port 8080, 8080
  config.vm.forward_port 3000, 3000
  config.vm.forward_port 3306, 3306

  config.vm.share_folder 'pfr', "/home/vagrant/pfr", "../pfr"

  config.vm.customize ["modifyvm", :id, "--memory", 2248, "--cpus", 2]

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["cookbooks", "my-cookbooks"]

    chef.json = {
      :install => {
        :packages => %w(
          build-essential
          openssl
          libreadline6
          libreadline6-dev
          zlib1g
          zlib1g-dev
          libssl-dev
          libyaml-dev
          libsqlite3-dev
          sqlite3
          libxml2-dev
          libxslt-dev
          autoconf
          libc6-dev
          ncurses-dev
          automake
          libtool
          bison
          subversion
          libmysql-ruby
          libmysqlclient-dev
          libpq-dev
          postgresql-client-common
          rmagic
          imagemagick
        )
      },
      :rvm => {
        :user_installs => [ {
          :user         => 'vagrant',
          :upgrade      => 'stable',
          :default_ruby => '1.9.3',
          :rubies       => ['2.0.0'],
          :rvmrc        => {
            :rvm_always_trust_rvmrc_flag => 1,
            :rvm_install_on_use_flag     => 1,
            :rvm_trust_rvmrcs_flag       => 1,
          },
        } ]
      },
      :git => {
        :global => {
          "user.email" => "galayda.roman@gmail.com",
          "user.name" => "galaydaroman",
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
      }
    }

    chef.add_recipe('apt')
    chef.add_recipe('git')
    chef.add_recipe('git-config')
    chef.add_recipe('vim')
    chef.add_recipe('rvm::user')
    chef.add_recipe('gemrc')

    chef.add_recipe('mysql::server')
    chef.add_recipe('mysql::client')
    chef.add_recipe('xvfb')
    chef.add_recipe('firefox')
    chef.add_recipe('redis::source')

    chef.add_recipe('bashrc::user')
    chef.add_recipe('bash-profile')
    chef.add_recipe('bash-completion')
    chef.add_recipe('imagemagick')
    # chef.add_recipe('phantomjs')


  end
end
