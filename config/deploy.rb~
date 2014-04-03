require "bundler/capistrano"
require 'capistrano/ext/multistage'
require 'bundler/setup'
#load 'deploy/assets'

set :default_stage, "production"

set :application, "Resume"

set :repository,  "git@github.com:azhar-uddin/Resume.git"
set :scm, "git"
set :user, "rubyeffect1"
#set :bundle_gemfile, "app/Gemfile"

default_run_options[:pty] = true #if you don't do this you'll get a tty sudo error on EC2
default_run_options[:shell] = '/bin/bash'

ssh_options[:keys] = [File.join(ENV["HOME"], ".ssh", "id_rsa")]
#set :deploy_to, "/home/rubyeffect1/apps/charleston_grace_two" 
set :deploy_via, :remote_cache

set :use_sudo, false # don't need this on most setup 
set :keep_releases, 10  # only keep 10 version to save space 
set :copy_exclude, [".git",".gitignore","/app/config/database.yml"]
set :normalize_asset_timestamps, false

before "deploy:assets:precompile", "deploy:update_symlinks", "deploy:config_symlink"
after "deploy:create_symlink", "deploy:cleanup"

namespace :deploy do
  desc "Setup symlinks and updates"
  task :update_symlinks, :roles => :app, :except => { :no_release => true } do
    #run "ln -nfs #{deploy_to}/#{shared_dir}/config/database.yml #{release_path}/config/database.yml"
    run "ln -nfs #{deploy_to}/#{shared_dir}/system #{release_path}/public/system"    
  end
  
  task :config_symlink do
    run "cp #{deploy_to}/shared/config/database.yml #{release_path}/config/database.yml"
  end

  task :restart, :roles => :app, :except => {:no_release => true} do
    run "touch #{File.join(current_path, 'tmp', 'restart.txt')}"
  end
end


