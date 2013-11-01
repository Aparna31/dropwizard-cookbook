require 'bundler/setup'
require 'rake'
require 'foodcritic'
require 'rspec/core/rake_task'


task :default => [:test]

RSpec::Core::RakeTask.new(:spec) do |t|
  t.pattern = "./test/spec{,/*/**}/*_spec.rb"
  t.ruby_opts = "-I./test/spec"
end

FoodCritic::Rake::LintTask.new

begin
  require 'kitchen/rake_tasks'
  Kitchen::RakeTasks.new
rescue LoadError
  puts '>>>>> Kitchen gem not loaded, omitting tasks' unless ENV['CI']
end

desc "Runs knife cookbook test against all the cookbooks"
task :knife_test do
  sh "knife cookbook test #{File.basename(File.expand_path("..", __FILE__))} -o ../."
end

desc "Run all tests"
task :test do
  [ :knife_test, :foodcritic, :spec ].each do |task|
    Rake::Task[task].execute
  end
end
