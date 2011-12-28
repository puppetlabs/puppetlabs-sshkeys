require 'rake'
require 'rake/clean'
require 'rubygems'
require 'rspec'
require 'rspec/core/rake_task'

CLEAN.include('pkg/*')

task :default => [:spec]

RSpec::Core::RakeTask.new do |t|
  t.pattern = 'spec/**/*_spec.rb'
  t.fail_on_error = true
end

desc "Build package"
task :build do
  sh "puppet-module build"
end
