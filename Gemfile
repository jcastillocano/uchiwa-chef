source 'https://rubygems.org'

gem 'rake'
gem 'stove'

group :lint do
  gem 'rubocop', '~> 0.18'
  gem 'rubocop-checkstyle_formatter'
  gem 'foodcritic', '~> 3.0'
end

group :unit, :integration do
  gem 'berkshelf', '~> 4.0'
  gem 'rspec_junit_formatter'
end

group :unit do
  gem 'chefspec', '~> 4.0'
end

group :integration do
  gem 'test-kitchen', git: 'https://github.com/jcastillocano/test-kitchen.git'
  gem 'kitchen-vagrant', '~> 0.11'
  gem 'kitchen-ec2', git: 'https://github.com/jcastillocano/kitchen-ec2.git', branch: 'dedicated_tenancy'
  gem 'kitchen-transport-sshtar'
  gem 'serverspec', '~> 2.0'
end
