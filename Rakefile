require 'rspec/core/rake_task'

# syntax/lint checks: RuboCop & Foodcritic
namespace :lint do
  require 'rubocop/rake_task'
  require 'foodcritic'

  desc 'Run Ruby syntax/lint checks'
  RuboCop::RakeTask.new(:ruby) do |task|
    task.requires = ['rubocop/formatter/checkstyle_formatter']
    task.options = [
      '--no-color',
      '--require', 'rubocop/formatter/checkstyle_formatter',
      '--format', 'RuboCop::Formatter::CheckstyleFormatter',
      '--out', 'reports/checkstyle.xml'
    ]
    # don't abort rake on failure
    task.fail_on_error = false
  end

  desc 'Run Chef syntax/lint checks'
  FoodCritic::Rake::LintTask.new(:chef) do |task|
    task.options = {
      tags: ['~FC037'],
      fail_tags: ['any']
    }
  end
end

desc 'Run all syntax/lint checks'
task lint: ['lint:ruby', 'lint:chef']

# unit testing: ChefSpec
desc 'Run RSpec and ChefSpec unit tests'
RSpec::Core::RakeTask.new(:unit) do |rspec|
  rspec.rspec_opts = '--color  --format documentation --format html ' \
                     '--out reports/rspec_results.html --format RspecJunitFormatter ' \
                     '--out reports/rspec_results.xml'
end

# integration testing: Test Kitchen
namespace :integration do
  require 'kitchen'

  desc 'Run Test Kitchen integration tests with Vagrant'
  task :vagrant do
    Kitchen.logger = Kitchen.default_file_logger
    Kitchen::Config.new.instances.each do |instance|
      instance.test(:always)
    end
  end
end

namespace :package do
  require 'logify'
  require 'stove/mash'
  require 'stove/packager'
  require 'stove/cookbook'
  require 'stove/cookbook/metadata'
  desc 'Pretendo hacer algo con stove ...'
  task :local, :path do |_t, args|
    path = args[:path] || Dir.pwd
    puts "Path : #{path}"
    UNSET_VALUE = Object.new
    cookbook = Stove::Cookbook.new(Dir.pwd)
    puts "Cookbook name: #{cookbook.name}"
    puts "Cookbook version : #{cookbook.version}"
    File.open("#{path}/#{cookbook.name}-#{cookbook.version}.tar.gz", 'w') do |package|
      package.write(cookbook.tarball(true).read)
    end
  end
end

desc 'Publish package'
task package: ['package:local']

desc 'Run all integration tests'
task integration: ['integration:vagrant']

desc 'Run all travis tests'
task default: %w(lint unit integration)
