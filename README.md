# README

## rmagick-memory

Test of RMagick with ImageMagick-6.9.12-90 memory leak

```
rvm use ruby-3.1.2@rmagick-memory --create
gem install rails
rails new rmagick-memory
cd rmagick-memory
bundle install
# open another terminal and watch memory consumption via htop or the like
rake rmagick
```

A rake task was created to test rmagick by iterating over the [Usage example](https://github.com/topfunky/gruff#usage) from gruff gem.

```
require 'gruff'

desc "This is the default task of rmagick"
task rmagick: 'rmagick:default'

namespace :rmagick do

  desc "Create a ton of images using rmagick and gruff"
  task :default => :environment do
    ENV['RAILS_ENV'] = "development" 
    
    1000.times do |x|
      g = Gruff::Line.new
      g.title = 'Wow!  Look at this!'
      g.labels = { 0 => '5/6', 1 => '5/15', 2 => '5/24', 3 => '5/30', 4 => '6/4',
                   5 => '6/12', 6 => '6/21', 7 => '6/28' }
      g.data :Jimmy, [25, 36, 86, 39, 25, 31, 79, 88]
      g.data :Charles, [80, 54, 67, 54, 68, 70, 90, 95]
      g.data :Julie, [22, 29, 35, 38, 36, 40, 46, 57]
      g.data :Jane, [95, 95, 95, 90, 85, 80, 88, 100]
      g.data :Philip, [90, 34, 23, 12, 78, 89, 98, 88]
      g.data :Arthur, [5, 10, 13, 11, 6, 16, 22, 32]
      g.write("#{Rails.root}/lib/assets/exciting#{x}.png")
    end
    
  end
  
end
```

This should create 1,000 images in your lib/assets directory but with RMagick 5.2.0 with ImageMagick-6.9.12-90 the process will consume memory until the process is killed by the OS, in my case Ubuntu 20 LTS.  However, using RMagick 5.2.0 with ImageMagick-6.9.12-89 and below the rake task works just fine and memory consumption plateaus out.

