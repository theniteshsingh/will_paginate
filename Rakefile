require 'rake/rdoctask'

load 'spec/tasks.rake'

desc 'Default: run specs.'
task :default => :spec

desc 'Generate RDoc documentation for the will_paginate plugin.'
Rake::RDocTask.new(:rdoc) do |rdoc|
  rdoc.rdoc_files.include('README.rdoc', 'LICENSE', 'CHANGELOG.rdoc').
    include('lib/**/*.rb').
    exclude('lib/will_paginate/finders/active_record/named_scope*').
    exclude('lib/will_paginate/finders/sequel.rb').
    exclude('lib/will_paginate/view_helpers/merb.rb').
    exclude('lib/will_paginate/deprecation.rb').
    exclude('lib/will_paginate/core_ext.rb').
    exclude('lib/will_paginate/version.rb')
  
  rdoc.main = "README.rdoc" # page to start on
  rdoc.title = "will_paginate documentation"
  
  rdoc.rdoc_dir = 'doc' # rdoc output folder
  rdoc.options << '--inline-source' << '--charset=UTF-8'
  rdoc.options << '--webcvs=http://github.com/mislav/will_paginate/tree/master/'
end

desc %{Update ".manifest" with the latest list of project filenames. Respect\
.gitignore by excluding everything that git ignores. Update `files` and\
`test_files` arrays in "*.gemspec" file if it's present.}
task :manifest do
  list = `git ls-files --full-name -x *.gemspec -x website`.chomp.split("\n")
  
  if spec_file = Dir['*.gemspec'].first
    spec = File.read spec_file
    spec.gsub! /^(\s* s.(test_)?files \s* = \s* )( \[ [^\]]* \] | %w\( [^)]* \) )/mx do
      assignment = $1
      bunch = $2 ? list.grep(/^(test|spec)\//) : list
      '%s%%w(%s)' % [assignment, bunch.join(' ')]
    end
      
    File.open(spec_file, 'w') { |f| f << spec }
  end
  File.open('.manifest', 'w') { |f| f << list.join("\n") }
end

task :website do
  Dir.chdir('website') do
    %x(haml index.haml index.html)
    %x(sass pagination.sass pagination.css)
  end
end
