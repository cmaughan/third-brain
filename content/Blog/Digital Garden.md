---
date: 2021-07-27
tags:
  - blog
  - neuron
  - markdown
  - digital_garden
---

# Digital Garden
 
I have simple requirements for a digital garden.  It should be easy to edit using my favourite tools and trivial to deploy.  I want the content in the most simple form it can be, and easy to move around.  I want to edit with Vim keystrokes; and that goes for any document I ever edit!

Without going into details about previous attempts (wordpress, tiddlywiki, hugo, jekyll, etc.), here is a great system I've settled on.

All my blog and articles are trivial markdown files.  Links with `[[]]` square brackets between pages.  A little bit of front matter in the markdown files to add dates for blogs and the occasional tags.

Visual Studio code and Vim have great plugins for working with Markdown.  In particular Visual Studio Code lets me trivially jump between markdown links, and shows my inline images in the preview window.  These tools automatically install and work great; along with the Vim extension - a requirement for me.

[Neuron](https://neuron.zettel.page) is great as a static site generator, taking my markdown notes and turning them into a clean set of pages.  Github actions mean that all I have to do is `git push` my site to publish it.  I've customised it a bit with some JS script for adding social sharing links, etc.  And I have to add 4 lines of `<div>` elements to each blog page; only a minor annoyance and I could automate it too.

[Obsidian](https://obsidian.md) is an alternative editor, for when I want deep search and cool graph visualization of my pages.

At this point my only frustration is links to site images.  I can do bracket links, relative links just fine with great preview in VS Code, but the way github converts links doesn't work, so I need to make the link be 'static/myimage.png' for it to work.  This breaks the preview, which is a minor annoyance.

<div class="ui section divider"></div>
<section id="socialMediaLinks"></section>
<div class="ui section divider"></div>
<div id="disqus_thread"></div>