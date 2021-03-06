require 'rubygems'
require 'rake'
require 'rake/testtask'
require 'rake/rdoctask'
require 'rake/gempackagetask'
require 'rake/contrib/rubyforgepublisher'

PKG_NAME = 'quickbooks'
PKG_VERSION = "0.4.3"

PKG_FILES = FileList[
    "lib/**/*", "rspec/**/*", "[A-Z]*", "Rakefile", "doc/**/*"
]

# Make a console, useful when working on tests
desc "Generate a test console"
task :console do
   verbose( false ) { sh "irb -I lib -r 'quickbooks'" }
end

# Genereate the RDoc documentation
desc "Create documentation"
Rake::RDocTask.new("doc") { |rdoc|
  rdoc.title = "QuickBooks for Ruby"
  rdoc.rdoc_dir = 'doc'
  rdoc.rdoc_files.include('README')
  rdoc.rdoc_files.include('LICENSE')
  rdoc.rdoc_files.include('lib/**/*.rb')
}

# Generate the package
spec = Gem::Specification.new do |s|
  #### Basic information.
  s.name = PKG_NAME
  s.version = PKG_VERSION
  s.platform = Gem::Platform::RUBY
  s.description = s.summary = "A module to connect to QuickBooks through the QuickBooks SDK using WIN32OLE (other connection types to come)."

  #### Dependencies.
  s.add_dependency('formattedstring', '>= 0.1.0')
  s.add_dependency('hash_magic', '>= 0.1.0')
  
  #### Which files are to be included in this gem?  Everything!  (Except CVS directories.)
  s.files = PKG_FILES

  #### Load-time details: library and application (you will need one or both).
  s.require_path = 'lib'

  #### Documentation and testing.
  s.has_rdoc = true
  s.extra_rdoc_files = ["README", "LICENSE"]

  #### Author and project details.
  s.author = "Daniel Parker"
  s.email = "gems@behindlogic.com"
  s.homepage = "http://quickbooks.rubyforge.org"
  s.rubyforge_project = 'quickbooks'
end

Rake::GemPackageTask.new(spec) do |pkg|
  pkg.need_zip = false
  pkg.need_tar = false
end

desc "Report code statistics (KLOCs, etc) from the application"
task :stats do
  require 'code_statistics'
  CodeStatistics.new(
    ["Library", "lib"]
  ).to_s
end

desc "Publish new gem to Public Dropbox"
task :drop_gem => [:package] do
  `cp pkg/*.gem ~/Dropbox/Public`
end

desc "Default Task"
task :default => [ :drop_gem ] do
  # Run the unit tests
  # desc "Run all rspec tests (rake task not yet implemented!)"
end

desc "Publish new documentation"
task :publish => [:doc] do
  `scp -r doc/* dcparker@rubyforge.org:/var/www/gforge-projects/quickbooks`
end