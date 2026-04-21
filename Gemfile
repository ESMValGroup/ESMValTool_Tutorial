# frozen_string_literal: true

source 'https://rubygems.org'

git_source(:github) {|repo_name| "https://github.com/#{repo_name}" }

# Synchronize with https://pages.github.com/versions
ruby '>=2.5.3'

#  github-pages conflics with jekyll+liquid, but the site
#  builds well locally; github-pages gem is semi-deprecated
#  and should not be used as gem anymore, instead use via Actions
#  gem 'github-pages', group: :jekyll_plugins
gem 'jekyll', '~> 4.3'
gem 'csv'
gem 'bigdecimal'
