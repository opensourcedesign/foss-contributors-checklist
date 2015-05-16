require "rubygems"
require 'rake'
require 'yaml'
require 'time'
require 'open-uri'

SOURCE = "."
CONFIG = {
  'version' => "0.2.13",
  'includes' => File.join(SOURCE, "_includes"),
  'themes' => File.join(SOURCE, "_includes", "themes"),
  'layouts' => File.join(SOURCE, "_layouts"),
  'posts' => File.join(SOURCE, "_posts"),
  'post_ext' => "markdown",
  'theme_package_version' => "0.1.0"
}

# Path configuration helper
# module JB
#   class Path
#     SOURCE = "."
#     Paths = {
#       :layouts => "_layouts",
#       :themes => "_includes/themes",
#       # :theme_assets => "assets/themes",
#       # :theme_packages => "_theme_packages",
#       :posts => "_posts"
#     }

#     def self.base
#       SOURCE
#     end

#     # build a path relative to configured path settings.
#     def self.build(path, opts = {})
#       opts[:root] ||= SOURCE
#       path = "#{opts[:root]}/#{Paths[path.to_sym]}/#{opts[:node]}".split("/")
#       path.compact!
#       File.__send__ :join, path
#     end

#   end #Path
# end #JB

# Usage: rake post title="A Title" [date="2012-02-09"] [cat="Category"]
desc "Begin a new post in #{CONFIG['posts']}"
task :post do
  abort("rake aborted: '#{CONFIG['posts']}' directory not found.") unless FileTest.directory?(CONFIG['posts'])
  title = ENV['title'] || "new-post"
  slug = title.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '')
  begin
    date = (ENV['date'] ? Time.parse(ENV['date']) : Time.now).strftime('%Y-%m-%d')
    date_time = (ENV['date'] ? Time.parse(ENV['date']) : Time.now).strftime('%Y-%m-%d-%H-%M')
  rescue Exception => e
    puts "Error - date format must be YYYY-MM-DD, please check you typed it correctly!"
    exit -1
  end
  cat = ENV['cat'] || ""

  filename = File.join(CONFIG['posts'], "#{date}-#{slug}.#{CONFIG['post_ext']}")
  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end

  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: checklist"
    post.puts "date: #{date_time}"
    post.puts "title: \"#{title.gsub(/-/,' ')}\""
    post.puts "category: #{cat}"
    post.puts "example: "
    post.puts "example_title: "
    post.puts "read_more: "
    post.puts "read_more_title: "
    post.puts "---"
  end
end # task :post

desc "Update the header files from the OSD repository"
task :get_includes do
  github_username = 'opensourcedesign'
  github_repo = 'opensourcedesign.github.io'

  raw = open("https://raw.githubusercontent.com/#{github_username}/#{github_repo}/master/_includes/footer.html").read
  file = File.write(File.join(CONFIG['includes'], 'footer.html'), raw)
  raw = open("https://raw.githubusercontent.com/#{github_username}/#{github_repo}/master/_includes/header.html").read
  file = File.write(File.join(CONFIG['includes'], 'header.html'), raw)
end
