desc 'Generate tags page'
task :tags do
  puts "Generating tags..."
  require 'rubygems'
  require 'jekyll'
  include Jekyll::Filters

  options = Jekyll.configuration({})
  site = Jekyll::Site.new(options)
  site.read_posts('')
  site.tags.sort.each do |tag, posts|
    html = <<-HTML
---
layout: tag_index
tag_name: "#{tag}"
title: Postings tagged "#{tag}"
---


    HTML
    File.open("tags/#{tag}.html", 'w+') do |file|
      file.puts html
    end
  end
  puts 'Done.'
end

desc 'Generate tag cloud'
task :tag_cloud do
  puts "Generating tag_cloud..."
  require 'rubygems'
  require 'jekyll'
  include Jekyll::Filters

  options = Jekyll.configuration({})
  site = Jekyll::Site.new(options)
  site.read_posts('')
  html = '<h4>Tags</h4><ul class="tag-cloud">'
  site.tags.sort_by{|k,v| -v.count}.slice(0,10).each do |tag, posts|
    html << <<-HTML
    <li><a href="/tags/#{tag}.html">#{tag}(#{posts.count})</a></li>
    HTML
  end
  html << '</ul>'

  File.open("_includes/tag_cloud.html", 'w+') do |file|
    file.puts html
  end
  puts 'Done.'
end

desc 'build'
task :build => [:tags, :tag_cloud]
