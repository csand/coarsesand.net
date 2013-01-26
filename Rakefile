require 'erb'
require 'redcarpet'
require 'tilt'

def copy_task(dest, src, prereqs=[])
  file dest => prereqs do
    cp_r src, dest
  end
end

def build_task(dest, src)
  copy_task "build/#{dest}", src, 'build'
end

def sass_task(dest, src, prereqs=[])
  file dest => prereqs do
    sh "sass #{src} #{dest}"
  end
end

task :default => [
  'build',
  'build/screen.css',
  'build/normalize.css',
  'build/require.js',
  'build/requirejs_config.js',
  'build/index.html',
]
task :clobber do
  sh 'rm -r build/*'
end

directory 'build'
sass_task 'build/screen.css', 'src/styles/main.scss'
build_task 'normalize.css', 'lib/normalize-css/normalize.css'
build_task 'require.js', 'lib/require/require.js'
build_task 'requirejs_config.js', 'src/requirejs_config.js'
file 'build/index.html' do
  template = Tilt::ERBTemplate.new('src/index.erb')
  about    = Tilt::RedcarpetTemplate.new('src/about.md')
  File.open('build/index.html', 'w') do |file|
    file.write(template.render { about.render })
  end
  puts 'rendered build/index.html'
end
