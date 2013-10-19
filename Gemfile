ruby '2.0.0'
source 'https://rubygems.org'

gem 'sinatra'
gem 'sinatra-flash'
gem 'sinatra-partial', :require => 'sinatra/partial'
gem 'sass'
gem 'compass'
gem 'rake'
gem 'capistrano', '~> 2.15'
gem "capistrano-resque", "~> 0.1.0", :require => false
gem 'resque'

group :test do
  gem 'rspec'
  gem 'rack-test', :require => 'rspec/core/rake_task'
end

group :development do
  gem 'rerun'
end

group :production do
  gem 'mysql2'
  gem 'therubyracer'
end

