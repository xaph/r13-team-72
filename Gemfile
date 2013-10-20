ruby '2.0.0'
source 'https://rubygems.org'

gem 'sinatra'
gem 'sinatra-flash'
gem 'sinatra-partial', :require => 'sinatra/partial'
gem 'sass'
gem 'compass'
gem 'rake'
gem 'resque'
gem 'mongoid', "~> 3.0.0"
gem 'bson_ext'
gem "octokit", "~> 2.0"
gem 'rugged', git: 'git://github.com/libgit2/rugged.git', branch: 'development', submodules: true
gem 'rubocop', :require => false

group :test do
  gem 'rspec'
  gem 'rack-test', :require => 'rspec/core/rake_task'
end

group :development do
  gem 'capistrano', '~> 2.15'
  gem 'rerun'
end

group :production do
  gem 'god'
end
