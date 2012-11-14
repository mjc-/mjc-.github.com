# Where our Bootstrap source is installed. Can be overridden by an
# environment variable.
BOOTSTRAP_SOURCE = File.expand_path("./bootstrap")

# Where to find our custom LESS file.
BOOTSTRAP_CUSTOM_LESS = 'bootstrap/less/custom.less'

def different?(path1, path2)
  require 'digest/md5'
  different = false
  if File.exist?(path1) && File.exist?(path2)
    path1_md5 = Digest::MD5.hexdigest(File.read path1)
    path2_md5 = Digest::MD5.hexdigest(File.read path2)
    (path2_md5 != path1_md5)
  else
    true
  end
end

task :bootstrap_css do |t|
  puts "Symlinking custom.less into bootstrap"
  sh "cd bootstrap/less && ln -s ../../custom.less"
  puts "Copying LESS files"
  Dir.glob(File.join(BOOTSTRAP_SOURCE, 'less', '*.less')).each do |source|
    target = File.join('src/bootstrap/less', File.basename(source))
    cp source, target if different?(source, target)
  end

  curdir = Dir.pwd
  puts "Compiling #{BOOTSTRAP_CUSTOM_LESS}"
  Dir.chdir(File.dirname(BOOTSTRAP_CUSTOM_LESS))
  custom_less = File.basename(BOOTSTRAP_CUSTOM_LESS)
  sh "lessc #{custom_less} > #{curdir}/src/bootstrap/css/bootstrap.min.css"
  Dir.chdir(curdir)
 end
 
task :bootstrap_js do
  require 'uglifier'
  require 'erb'

  template = ERB.new %q{
  <!-- AUTOMATICALLY GENERATED. DO NOT EDIT. -->
  <% paths.each do |path| %>
  <script type="text/javascript" src="/bootstrap/js/<%= path %>"></script>
  <% end %>
  }

  paths = []
  minifier = Uglifier.new
  Dir.glob(File.join(BOOTSTRAP_SOURCE, 'js', '*.js')).each do |source|
    base = File.basename(source).sub(/^(.*)\.js$/, '\1.min.js')
    paths << base
    target = File.join('src/bootstrap/js', base)
    if different?(source, target)
      File.open(target, 'w') do |out|
        out.write minifier.compile(File.read(source))
      end
    end
  end

  File.open('src/_includes/bootstrap.js.html', 'w') do |f|
    f.write template.result(binding)
  end
end

task :bootstrap => [:bootstrap_js, :bootstrap_css]

task :default => :jekyll

task :jekyll => :bootstrap do
  sh 'cd src ; jekyll --pygments . ../_out' 
end
