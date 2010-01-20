task :tags do
  puts "Generating tags..."
  require 'rubygems'
  require 'jekyll'
  include Jekyll::Filters
    
  options = Jekyll.configuration({})
  site = Jekyll::Site.new(options)
  site.read_posts('')

  categories = site.posts.collect do |post|
    post.categories
  end.flatten.uniq!

  FileUtils.mkdir_p("tags")
  categories.each do |category|
    html =<<-END
---
layout: default
title: "Tag archive: #{category}"
---
  {% for post in site.categories.#{category} %}
  <div class="item">
    <div class="item_details">
      <h3><a href="{{ post.url }}">{{ post.title }}</a></h3>
      <h4>Posted  on {{ post.date | date_to_string }}</h4>
    </div>
    <div class="item_content">
      {{ post.content }}
    </div>
    <div class="item_meta">
      <span class="item_tags">
        Tags: 
        {% for category in post.categories %}
        <a href="/tags/{{ category }}.html" title="View posts tagged with &quot;{{ category }}&quot;">{{ category }}</a>{% if forloop.last != true %}, {% endif %}
        {% endfor %}
      </span>
    </div>
  </div>
  {% endfor %}
</ul>
END

    File.open("tags/#{category}.html", "w") do |f|
      f.write(html)
    end
  end
end

task :cloud do
  puts 'Generating tag cloud...'
  require 'rubygems'
  require 'jekyll'
  include Jekyll::Filters
  
  options = Jekyll.configuration({})
  site = Jekyll::Site.new(options)
  site.read_posts('')
  
  html =<<-END
---
layout: default
title: Tag cloud
---

<h1>Tag cloud</h1>

    END
    
  site.categories.sort.each do |category, posts|
    html << <<-END
      END
      
    s = posts.size
    font_size = 12 + (s*1.5);
    html << "<a href=\"/tags/#{category}.html\" title=\"Postings tagged #{category}\" style=\"font-size: #{font_size}px; line-height:#{font_size}px\">#{category}</a> "
  end

  File.open('tags.html', 'w+') do |file|
    file.puts html
  end
  puts 'Done.'
end

task :jekyll do
  sh "jekyll --pygments"
end

task :clean do
  sh "rm -r _site"
  sh "rm -r tags"
end

task :gen => ["clean", "tags", "jekyll"]


