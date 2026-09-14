source "https://rubygems.org"

# This matches the gem bundle GitHub Pages uses to build sites, so a local
# `bundle exec jekyll serve` renders the same way the live site will.
gem "github-pages", group: :jekyll_plugins

group :jekyll_plugins do
  gem "jekyll-relative-links"
  gem "jekyll-sitemap"
  gem "jekyll-seo-tag"
end

# Windows/JRuby workaround required by some Jekyll installs
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]
