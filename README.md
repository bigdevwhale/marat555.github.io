# Marat Badykov Home Page

This is the code for the [Marat Badykov Home Page](http://marat555.github.io).

This is my home page based on [Yevgeniy Brikman Home Page](http://www.ybrikman.com).
Check out [Migrating from Blogger to GitHub Pages and launching the new ybrikman.com](http://www.ybrikman.com/blog/2015/04/20/migrating-from-blogger-to-github-pages/)
for background info.

# Quick start

 Install prerequisites.

```bash
sudo apt-get install ruby ruby-dev node.js zlib1g-dev
sudo gem install bundler
cd /path/to/this-repo
bundle install
```
Check out your blog before committing and pushing your changes.

```bash
bundle exec jekyll serve
```

Go to `http://localhost:4000` to test.

See the [Jekyll](http://jekyllrb.com/) and [GitHub Pages](https://pages.github.com/)
documentation for more info.

# Docker quick start

As an alternative to installing Ruby and Jekyll, if you're a user of
[Docker](https://www.docker.com/) and [Docker
Compose](https://docs.docker.com/compose/), you can run a Docker image of
marat555.github.io that has all the dependencies already setup for you.

```bash
git clone https://github.com/Marat555/marat555.github.io.git
cd marat555.github.io
docker-compose up
```

Go to `http://localhost:4000` to test.

# Technologies

1. Built with [Jekyll](http://jekyllrb.com/). This website is completely static
   and I use basic HTML or Markdown for everything.
1. Hosted on [GitHub Pages](https://pages.github.com/). I'm using the
   [GitHub Pages Gem](https://help.github.com/articles/using-jekyll-with-pages/)
   and only Jekyll plugins that are
   [available on GitHub Pages](https://help.github.com/articles/repository-metadata-on-github-pages/).
1. The design is loosely based on [Kasper](https://github.com/rosario/kasper),
   [Pixyll](http://pixyll.com/), and [Medium](https://medium.com/).
1. I used [Basscss](http://www.basscss.com/), [Sass](http://sass-lang.com/),
   [Font Awesome Icons](http://fortawesome.github.io/Font-Awesome/icons/),
   [Hint.css](http://kushagragour.in/lab/hint/),and
   [Google Fonts](https://www.google.com/fonts) for styling.
1. I used [jQuery](https://jquery.com/), [lazySizes](http://afarkas.github.io/lazysizes/),
   and [responsive-nav.js](http://responsive-nav.com/) for behavior.
1. I added [Disqus](https://disqus.com/websites/) as a commenting system.

# License

This code is released under the MIT License. See LICENSE.txt.
