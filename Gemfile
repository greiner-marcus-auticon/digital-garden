# frozen_string_literal: true

source "https://rubygems.org"

git_source(:github) {|repo_name| "https://github.com/#{repo_name}" }

gem "jekyll", "~> 4.3"
gem "jekyll-last-modified-at", git: "https://github.com/maximevaillancourt/jekyll-last-modified-at", branch: "add-support-for-files-in-git-submodules"
gem "webrick", "~> 1.8"
gem "nokogiri"
gem 'ffi', '>= 1.17.0'
# Avoid polling for changes on Windows
gem 'wdm', '>= 0.1.0' if Gem.win_platform?
