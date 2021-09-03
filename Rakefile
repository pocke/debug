require "bundler/gem_tasks"
require "rake/testtask"

# for make test-bundled-gems
Gem.instance_variable_set(:@ruby, ENV['RUBY']) if ENV['RUBY']

Rake::TestTask.new(:test) do |t|
  t.libs << "test"
  t.libs << "lib"
  t.test_files = FileList["test/**/*_test.rb"]
end

require "rake/extensiontask"

task :build => :compile

Rake::ExtensionTask.new("debug") do |ext|
  ext.lib_dir = "lib/debug"
end

task :default => [:clobber, :compile, :test, 'README.md']

file 'README.md' => ['lib/debug/session.rb', 'lib/debug/config.rb',
                     'exe/rdbg', 'misc/README.md.erb'] do
  require_relative 'lib/debug/session'
  require 'erb'
  File.write 'README.md', ERB.new(File.read('misc/README.md.erb')).result
  puts 'README.md is updated.'
end

task :run => :compile do
  system(RbConfig.ruby, *%w(-I ./lib test.rb))
end

