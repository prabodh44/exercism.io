ENV['NEWRELIC_ENABLE'] = "false"

$:.unshift File.expand_path("./../lib", __FILE__)
Dir.glob("lib/tasks/*.rake").each { |r| import r }

require 'rake/testtask'

Rake::TestTask.new do |t|
  require 'bundler'
  Bundler.require
  t.pattern = "test/**/*_test.rb"
end

namespace :db do
  desc "migrate your database"
  task :migrate do
    require 'bundler'
    Bundler.require
    require_relative 'lib/db/connection'
    DB::Connection.establish
    ActiveRecord::Migrator.migrate('./db/migrate')
  end
end

desc "Run each test file independently"
namespace :test do
  task :each do
    files = Dir.glob('test/**/*_test.rb')
    failures = files.reject do |file|
      system("ruby #{file}")
    end

    puts "FAILURES IN:"
    failures.each do |failure|
      puts failure
    end
  end
end

task default: :test
