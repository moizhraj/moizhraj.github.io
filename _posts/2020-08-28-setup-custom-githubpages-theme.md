This article will guide you to quickly setup your custom Github pages theme using Jekyll

- Requirements and setup
- Install Jekyll 
- Setup and Build the theme using Jekyll

### Requirements and setup
We are using Ubuntu 20.04 and [Bundler][bundler-link] to install and run a Jekyll project. 

1. [Install Ruby][ruby-install-link] and verify if its configured correctly
```
ruby ---version
```
2. Install bundler
```
gem install bundler
```

### Install Jekyll
1. Create a new `Gemfile` (name it as Gemfile without any extension) and add following lines
```
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll_plugins
```
2. Install jekyll using bundler
```
bundle install
```

### Setup and Build the theme using jekyll
1. Run below command to setup a new jekyll theme project
```
bundle exec jekyll _3.3.0_ new GITHUB_USERNAME.github.io
```
2. Navigate to the newly created project
3. Run git init command to initialize a repository
```
$ git init
```
4. Create a new repository on github with name as `<git_username>.github.io`
5. Back in your terminal, connect your theme project with the remote repo
```
git remote add origin https://github.com/<git_username>.github.io
```
6. Test your theme project by running below command (site can be viewed by browsing http://localhost:4000/)
```
bundle exec jekyll serve
```
7. Add, Commit and push all your changes
```
git add .
git commit -m "Initial commit"
git push -u orgin main
```
8. Browse your github pages site using the same repo name i.e. <github_username>.github.io. Note that your repo needs to be public in order to work.

#### Jekyll plugins/gems can be updated using `bundle update`


[bundler-link]: http://bundler.io/
[ruby-install-link]: https://www.ruby-lang.org/en/downloads/x