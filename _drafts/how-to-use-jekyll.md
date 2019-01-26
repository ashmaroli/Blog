---
layout: post
#permalink: /:categories/:year/:month/:day/:title:output_ext
---

#Preparation: [Install Ruby][ruby-instal]
{% highlight ruby %}
brew install rbenv ruby-build
# Add rbenv to bash so that it loads every time you open a terminal
echo 'if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi' >> ~/.bash_profile
source ~/.bash_profile
# Install Ruby
rbenv install 2.5.3
rbenv global 2.5.3
{% endhighlight %}

2. Check Ruby and Gem `ruby -v` `gem -v`, make sure they are installed correctly.
3. Create a new folder in terminal.
4. cd to this new folder.
5. Install Jekyll. `gem install jekyll bundler`
6. Check Jekyll `jekyll -v`
7. Create a new site `jekyll new my-new-site-name`
8. cd to my-new-site-name and `bundle exec jekyll serve` (the first time run this), later use `jekyll serve` instead.
   Copy the Server address, and open it in a browser. Now the site can run on local machine.
9. Hosting the site on Github: Create a new empty repository in my github account. Don't check the box `initialize this repository with a README`.
10. In terminal type `git init`(initialize) `git checkout -b gh-pages` `git status` `git add .` `git commit -m "my message"`
`git add remote origin "a url can be found in your empty repository"`(connect github)
`git push origin gh-pages`(upload files)
11. See more details about jekyll:[Video Walkthroughs][jekyll-video], [Doc][jekyll-doc], [Cheatsheet][jekyll-cheatsheet].

[ruby-instal]: https://gorails.com/setup/osx/10.14-mojave
[jekyll-video]:https://jekyllrb.com/tutorials/video-walkthroughs/
[jekyll-doc]:https://jekyllrb.com/docs/
[jekyll-cheatsheet]:https://devhints.io/jekyll
