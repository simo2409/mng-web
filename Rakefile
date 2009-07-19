DEBUG = false
KINDS = ['html', 'css', 'js']
GENERATED_PATH = File.expand_path('./generated')

namespace :setup do
  desc 'Generates all directories needed'
  task :generate_directory_structure do
    ['workdir', GENERATED_PATH].each do |place|
      system("mkdir #{place}")
      cd place do
        KINDS.each do |kind|
          system("mkdir #{kind}") unless (kind == 'html' && place == GENERATED_PATH)
        end
      end
    end
  end
end

# Tasks to manage
namespace :mng do
  # If there is a <name page>.rb file the script assumes that it will generate a <name page>.html so it executes the .rb file and then look for the .html file
  desc 'Process and move all files present in workdir'
  task :process_all_files => :clean_generated do
    cd 'workdir' do
      # Managing html files
      
      KINDS.each do |kind|
        if kind == 'html' || File.exists?(kind)
          processed_files = []
          puts "Managing #{kind} files ... "
          cd kind do
            # Executing .rb files
            Dir.glob('*.rb').each do |ruby_file|
              file = ruby_file.split('.rb').first
              print "Executing #{kind}/#{ruby_file} ... " if DEBUG
              if system("ruby #{ruby_file}")
                puts 'DONE' if DEBUG
              else
                raise "There was an error during execution of '#{kind}/#{ruby_file}' script"
              end
              if File.exists?(file + '.' + kind)
                processed_files << file
              else
                raise "I can't find '#{kind}/#{file}.#{kind}' but I executed '#{kind}/#{ruby_file}'!"
              end
            end
            # Moving generated (and others) files to 'generated' directory
            Dir.glob("*.#{kind}") do |kind_file|
              print "Copying '#{kind}/#{kind_file}' ... " if DEBUG
              if processed_files.include?(kind_file)
                # Generated file, moves the generated file
                system("mv ./#{kind_file} #{GENERATED_PATH}/#{(kind == 'html' ? '' : kind + '/')}#{kind_file}")
              else
                # File not generated, preserve it for next processes
                system("cp ./#{kind_file} #{GENERATED_PATH}/#{(kind == 'html' ? '' : kind + '/')}#{kind_file}")
              end
              puts "DONE" if DEBUG
            end
          end
        end
      end
    end
  end
  
  desc 'Renames all files in generated to allow new files to be copied in it'
  task :clean_generated do
    puts "Preparing generated directory for new files ..."
    cd 'generated' do
      KINDS.each do |kind|
        if kind == 'html'
          Dir.glob('*.' + kind).each do |file|
            system("rm #{file}.old") if File.exists?(file + '.old')
            system("mv #{file} #{file}.old")
          end
        else
          if File.exists?(kind)
            cd kind do
              Dir.glob('*.' + kind).each do |file|
                system("rm #{file}.old") if File.exists?(file + '.old')
                system("mv #{file} #{file}.old")
              end
            end
          end
        end
      end
    end
  end
  
end

namespace :utils do
  task :generate_README_html_from_markdown do
    require 'rubygems'
    require 'RDiscount'
    s = RDiscount.new(File.read('README.markdown'))
    File.open('readme.html', 'w+') {|f| f.write(s.to_html) }
    system("open readme.html")
  end
end