---
layout: post
title: Building a Portfolio with Jekyll
---

## So you need to build a portfolio...

I've spent most of my professional experience to date doing web development to some extent. I've been exposed to lots of frameworks, but that makes it hard to get started building a portfolio. I could use a full framework like Express or Ruby on Rails, or go to the dark side and new up a Wordpress site (total joke I would never stoop that low). But I find myself slapping together this site using Jekyll, a blog-aware, static site generator that a surprisingly high number of developers haven't heard of.

## Why use Jekyll?

Most sites out there are dynamic and odds are if you're a web developer, you will almost always need to accept user input, do something in the back-end and communicate back to the user. This is the overly boarish reason why you might use a web framework. But a portfolio doesn't need to be dynamic or accept user input. It also doesn't need support for user authentication or a complex SPA powered by a custom API. 

Your portfolio is a static site and Jekyll is often talked about as the most powerful static site generator out there. Oh, and it also happens to be the engine behind GitHub Pages, which means it's adoption is huge and you can use it to host any of your project's pages, blogs or websites from GitHub's servers _for free_.

## Alright, let's give this thing a shot

Jekyll is built in Ruby, and thus requires you have a full [Ruby](https://www.ruby-lang.org/en/downloads/) development environment with all headers and [RubyGems](https://rubygems.org/pages/download) installed (see Jekyll's [requirements](https://jekyllrb.com/docs/installation/#requirements)).

### Hello, World!

Once you've got a working Ruby environment set up, go ahead and create a new Jekyll site by doing the following:

```bash
### Install the Jekyll and Bundler gems through RubyGems
~ $ gem install jekyll bundler

### Create a new Jekyll site at ./portfolio
~ $ jekyll new portfolio

### Change into your new directory
~ $ cd portfolio

### Build and serve the site locally
~ $ bundle exec jekyll serve

### Now browse to http://localhost:4000
```

When you navigate to the port listed above in a browser, you should now see the following:
<!--TODO: Insert image here  -->

### Exploring our World

By creating a new Jekyll project from the terminal, you should get a directory structure like the following:

<!--TODO: Insert directory structure  -->

At it's core, Jekyll is simply a text transformation engine. Input text (in your favourite markup language) and outputs layout(s) files. For a deeper dive into the directory structure and full descriptions of what each file/directory is for, view the [Jekyll docs](https://jekyllrb.com/docs/structure/).

For the sake of this tutorial, let's get a hands-on view of what we have. 

#### _config.yml
Jekyll uses a _config.yml_ file for configuration. Go ahead and fill in the title, email, description and social handles. Upon making changes, stop serving the site and rebundle and serve it by executing `bundle exec jekyll serve` as before. Note that this is only necessary when configuration changes are made. Verify your changes are reflected in the browser.

#### Posts
One of Jekyll's primary aspects is that it is "blog aware". This means blogging was baked into Jekyll's functionality. You can publish and maintain a blog simply by managing a folder of text-files on your computer. This removes all of the hassle of maintaining a database and web-based CMS systems.

The `_posts` folder is where the blog posts live. Natively, they can be Markdown or HTML but other formats can be used with the proper converter. All posts must have a YAML Front Matter and will be converted from their source formats into an HTML page that becomes part of your static site. In your browser, you may have seen the _Welcome to Jekyll!_ post. Take a look at that post in the `_posts` directory to build an understanding of how it's generated. 

As I write this blog post, I am doing it directly in the portfolio I'm creating. If you'd like to work along and try creating a post, simply create a new `.md` or `.html` file in the `_posts` directory with the following name format: `YEAR-MONTH-DAY-title.MARKUP`. We'll get into layouts and tweaking the format of this post and others using Front Matter later. If you've created a new post, you should see it both on the index/root page of your site and in the details view.

### Front Matter
<!--TODO: Word definition formatting  -->
Front Matter: _the pages, such as the title page and preface, that precede the main text of a book._

In a Jekyll setting, Front Matter tells Jekyll that this file is special and to treat the values between the triple-dashed lines as configuration.

Add the following YAML front matter block to the post you created:

```
---
layout: post
title: Your new dank post
---
```

By adding the predefined `layout` variable to your front matter, you'll notice the new post's styling become beautiful, just like the _Welcome to Jekyll!_ post.

### Themes and Layouts

By default, your portfolio is using the minima theme (check `_config.yml` to verify). As we just saw, adding `layout: post` to the front matter seems to add magical styling and render the post inside our site (the navbar is still visible at the top).

What actually happens under the hood is Jekyll sees the `post` layout defined, and takes all the content from the file, and renders it as _content_ inside the `post` layout. 

But where does that layout live? How can I make edits to it if I don't like the layout? I highly advise you give the [documentation on themes](https://jekyllrb.com/docs/themes/) a read, but at a high level: some of the site’s directories (such as the `assets`, `_layouts`, `_includes`, and `_sass` directories) are stored in the theme’s gem, hidden from your immediate view. Yet all of the necessary directories will be read and processed during Jekyll’s build process.

To override the theme's default `post` layout, we need to make a copy in a `_layouts` folder under our root directory: `_layouts/post.html`.

1. Locate the current theme by executing `bundle show minima`.
1. Using the returned location, make a copy of the file into the location highlighted above.
1. You do you. Make all the modifications you want. For this tutorial, I will be sticking with the default layout.

## This is cool, but I'm building a portfolio

_Enough of this blogging crap_ the crowd screamed... _I came here to build a portfolio_.

<!-- If we look at `index.md`, the entry point to our website, all that's defined is the font matter `layout` variable. Let's go ahead and change it to `layout: default`. 

Go ahead and create the directory and file:`_layouts/default.html`

To start, I'll copy the `default` layout for the minima theme using the steps outlined above. For the lazy, go ahead and copy-paste this:

```html
{% raw %}<!DOCTYPE html>
<html lang="{{ page.lang | default: site.lang | default: "en" }}">
  {% include head.html %}
  <body>
    {% include header.html %}
    <main class="page-content" aria-label="Content">
      <div class="wrapper">
        {{ content }}
      </div>
    </main>
    {% include footer.html %}
  </body>
</html>{% endraw %}
```

Now, if you refresh you browser you should notice **that nothing changed**! This is expected, after all we are using the same layout. -->

Before we start making changes to the layout, let's break apart what we want our portfolio to have.

- A top level navbar to switch between a portfolio (our site's home page), a blog (if you want one) and an about page
- A downloadable version of a resume
- Sections on the portfolio page for all our different chunks of information (ie: work experience, education, projects, etc.)
- A little bit of sexiness through themes/styling

With our goals in mind, let's tackle them one at a time.

### Navigation

#### Breaking up pages

Before we make changes to the navigation, let's go ahead and make our blog an explicit **page** in our application. We will start by basing this page off of the `about.md` page located in our root directory. The important things to note are that the `about.md` page specifies `layout: page`, a `title` and a `permalink` in the front matter.

Create a `blog.md` page with dummy content, it should look something like this:

```md
---
layout: page
title: Blog
permalink: /blog/
---

This is my dope-ass new blog
```

By simply adding this page, you should notice upon refreshing that _Blog_ now auto-magically appears in the navbar, and the `http://localhost:4000/blog/` URL brings you to your _dope-ass new blog_.

#### Modifying the existing plumbing

<!-- Take a look at the `default.html` layout which was copied over and exists in our project. There are two important pieces to explore:

`{% raw %}{% include header.html %}{% endraw %}`: uses the [include template](https://jekyllrb.com/docs/templates/#includes) and inserts the corresponding markup file wherever specified.

`{% raw %}{{ content }}{% endraw %}`: is the injected section after the front matter block in the markup file that _invoked_ the layout.

To override the _minima_ theme's markup for the navbar, let's do as we did for the `default.html` layout and copy it into our local `_includes` folder (you'll need to create this).

Note: again, it is vital that you name the new file with the [same name as the one you are overriding](https://jekyllrb.com/docs/themes/#overriding-theme-defaults). Alternatively, you can change the name of the include to your file name of choice.

`_includes/header.html`:

```html
{% raw %}<header class="site-header" role="banner">
  <div class="wrapper">
    {% assign default_paths = site.pages | map: "path" %}
    {% assign page_paths = site.header_pages | default: default_paths %}
    <a class="site-title" href="{{ "/" | relative_url }}">{{ site.title | escape }}</a>
    {% if page_paths %}
      <nav class="site-nav">
        <input type="checkbox" id="nav-trigger" class="nav-trigger" />
        <label for="nav-trigger">
          <span class="menu-icon">
            <svg viewBox="0 0 18 15" width="18px" height="15px">
              <path fill="#424242" d="M18,1.484c0,0.82-0.665,1.484-1.484,1.484H1.484C0.665,2.969,0,2.304,0,1.484l0,0C0,0.665,0.665,0,1.484,0 h15.031C17.335,0,18,0.665,18,1.484L18,1.484z"/>
              <path fill="#424242" d="M18,7.516C18,8.335,17.335,9,16.516,9H1.484C0.665,9,0,8.335,0,7.516l0,0c0-0.82,0.665-1.484,1.484-1.484 h15.031C17.335,6.031,18,6.696,18,7.516L18,7.516z"/>
              <path fill="#424242" d="M18,13.516C18,14.335,17.335,15,16.516,15H1.484C0.665,15,0,14.335,0,13.516l0,0 c0-0.82,0.665-1.484,1.484-1.484h15.031C17.335,12.031,18,12.696,18,13.516L18,13.516z"/>
            </svg>
          </span>
        </label>
        <div class="trigger">
          {% for path in page_paths %}
            {% assign my_page = site.pages | where: "path", path | first %}
            {% if my_page.title %}
            <a class="page-link" href="{{ my_page.url | relative_url }}">{{ my_page.title | escape }}</a>
            {% endif %}
          {% endfor %}
        </div>
      </nav>
    {% endif %}
  </div>
</header>{% endraw %}
```

From this file, it is clear that our newly added pages were auto-magically being piped in due to the for-loop that looks at the paths defined. We can also see that things like the `site-title` are grabbed from the `_config.yml` variables. Go ahead an make any changes you'd like. For myself, I'm editing the site title to just be my name.

Note: Don't forget to re-serve the page when making changes to the `_config.yml`. -->

Current status of our existing site:

- The root page has the content we'd ideally like for our _Blog_ page
- The _Blog_ page just has the empty dummy content we entered
- The _About_ page has the info it should (change at your own convenience)

Let's fix these in order.

Change the blog page to be an `html` page and with the following markup:

```html
{% raw %}---
layout: page
title: Blog
permalink: /blog/
---

<div class="home">
  <h1 class="page-heading">Posts</h1>
  <ul class="post-list">
    {% for post in site.posts %}
      <li>
        {% assign date_format = site.minima.date_format | default: "%b %-d, %Y" %}
        <span class="post-meta">{{ post.date | date: date_format }}</span>

        <h2>
          <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
        </h2>
      </li>
    {% endfor %}
  </ul>
  <p class="rss-subscribe">subscribe <a href="{{ "/feed.xml" | relative_url }}">via RSS</a></p>
</div>{% endraw %}
```

`index.md` is the page our site loads for it's root, and where we want our portfolio to exist. Noting that all this page does is define Front Matter that references `minima`'s `home` layout, let's go ahead and override that. As explained in the [Themes and Layouts](#themes-and-layouts) section of this post, go ahead and override the default home behaviiour by creating a new `home.md` file in the `_layouts` directory and temporarily fill it with dummy content.

```html
---
layout: default
---

<div class="home">
  <h1>Portfolio info goes here</h1>
</div>
```