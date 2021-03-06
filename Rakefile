require 'bundler'
env = (ENV['RACK_ENV'] || ENV['RAILS_ENV'] || :development).to_sym

Bundler.require(:default, env)
set :environment, env

require 'resque/tasks'
Dir["app/**/*.rb"].each {|file| require_relative file }
Mongoid.load!(File.join(__dir__, 'config', 'mongoid.yml'))

Resque.logger = Logger.new('log/resque.log','weekly')
Resque.logger.formatter = proc do |severity, datetime, progname, msg|
  "[#{datetime} ##{Process.pid}] #{progname} -- #{msg}\n"
end

desc 'Runs a develop webserver'
task :server do
  puts `rerun -- rackup --port 9292 config.ru`
end

desc 'Queue repository to fetch'
task :queue_repo, :repo do |t, args|
  repo = Repository.from_url(args[:repo])
  Resque.enqueue(FetchRepo, repo.id)
end

desc 'Complete changed_files for existing commits'
task :complete_changed_files do
  Repository.each do |repo|
    analyzer = RepoAnalyzer.new(repo)
    analyzer.complete_missing_changed_files
  end
end

desc 'Recalculate commit and repo scores for a repo'
task :recalculate_scores_for, :repo do |t, args|
  repo = Repository.from_url(args[:repo])
  puts repo.name
  report = repo.repo_report || repo.build_repo_report
  report.recalculate_score!
end

desc 'Feature repo'
task :feature, :repo do |t, args|
  repo = Repository.from_url(args[:repo])
  puts repo.name
  repo.featured = true
  repo.save
end

desc 'Unfeature repo'
task :unfeature, :repo do |t, args|
  repo = Repository.from_url(args[:repo])
  puts repo.name
  repo.featured = false
  repo.save
end

desc 'Recalculate commit and repo scores!'
task :recalculate_scores do
  puts "Recalculating scores..."
  Repository.each do |repo|
    puts repo.name
    report = repo.repo_report || repo.build_repo_report
    report.recalculate_score!
  end
end

desc 'Restart god workers'
namespace :queue do
  task :restart_workers => :environment do
    pids = Array.new

    Resque.workers.each do |worker|
      pids << worker.to_s.split(/:/).second
    end

    if pids.size > 0
      system("kill -QUIT #{pids.join(' ')}")
    end

    system("rm /var/run/god/resque-*.pid")
  end
end
