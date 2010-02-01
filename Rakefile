require "rake"
require "rake/testtask"
require "rake/gempackagetask"
require "rake/rdoctask"
require "rake/clean"

CLEAN << "pkg" << "doc" << "coverage" << ".yardoc"

task :default => :test

Rake::TestTask.new(:test) { |t| t.pattern = "test/*_test.rb" }
Rake::GemPackageTask.new(eval(File.read("friendly_id.gemspec"))) { |pkg| }
Rake::RDocTask.new do |r|
  r.rdoc_dir = "doc"
  r.rdoc_files.include "lib/**/*.rb"
end

begin
  require "yard"
  YARD::Rake::YardocTask.new do |t|
    t.options = ["--output-dir=doc"]
    t.options << "--files" << ["Guide.md", "Contributors.md", "Changelog.md"].join(",")
  end
rescue LoadError
end

begin
  require "rcov/rcovtask"
  Rcov::RcovTask.new do |r|
    r.test_files = FileList["test/**/*_test.rb"]
    r.verbose = true
    r.rcov_opts << "--exclude gems/*"
  end
rescue LoadError
end

namespace :rails do

  desc "Test FriendlyId gem inside Rails"
  task :test do
    rm_rf "fid"
    sh "rails --template extras/template-gem.rb fid"
    sh "cd fid; rake test"
  end
end

task :pushdocs do
  branch = `git branch | grep "*"`.chomp.gsub("* ", "")
  sh "git stash"
  sh "git checkout gh-pages"
  sh "cp -rp doc/* ."
  sh 'git commit -a -m "Regenerated docs"'
  sh "git push origin gh-pages"
  sh "git checkout #{branch}"
  sh "git stash apply"
end

namespace :ar2 do
  Rake::TestTask.new(:test) { |t| t.pattern = "test/active_record2/*_test.rb" }
end
