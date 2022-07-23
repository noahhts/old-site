---
layout: post
title:	My current setup for blogging
date:	2022-06-22
author:	Noah
description: How I write and publish articles in a seamless way using tools and software like Obsidian, GitHub Pages, Jekyll, and the terminal (command line on iTerm2).
ogimage: og-image.jpeg

---

## Obsidian
I use [Obsidian](https://obsidian.md/) as my main tool for both taking notes and actually writing. It's sort of my "second brain", and I think it just makes it easier to have most of my stuff in one place.

There's various reasons why I chose Obsidian (customizability, linking, local markdown files -> future proof and offline use). I may write a separate post on that eventually, but I would like to spend more time using it. There's already tons of content out there on Obsidian and similar tools like Roam Research and Logseq, but my best advice is to just try stuff out yourself.

In terms of writing articles/blog posts, I like to use Obsidian for its linking and being able to have multiple panes up side by side. Linking concepts and notes to each other helps me make some connections that can be useful points and ideas for my writing. Having multiple panes up lets you easily switch between the two and have your notes and actual writing side-by-side. I actually use the sliding panes plugin, inspired by [Andy Matuschak](https://twitter.com/andy_matuschak) and his [notes](https://notes.andymatuschak.org). It's useful if you want to keep panes of many notes open, so you can just scroll between them and the panes stay at a set width (it's like having your notes or index cards physically laid out on a table but digitally).

I also have a template for posts in my Obsidian, so I insert that whenever I create a new file for a blog post. It's simply for the metadata such as the layout (post vs page), title, date, etc. When I finish writing, the end product is a markdown file with metadata at the top and then the actual content below. It's inside a posts folder in my Obsidian vault.

## GitHub Pages & Jekyll
I use [GitHub Pages](https://pages.github.com/) and [Jekyll](https://jekyllrb.com/) for my personal website. It's free and the two services work very well together with my GitHub account. For more, check out Dan Romero's [post](https://danromero.org/how-this-website-works.html). I used the Jekyll theme he created and then edited it to my liking.

To publish a post, you just have to put the post (in markdown format) into a posts folder within the repo. While this is simple, it can be manually annoying to do if the file is in a separate directory (Obsidian vault). Since I'm getting back into [programming](https://eacxns.com/learning-programming.html) as well, I made a script for this.

## iTerm2
You can use whatever terminal you want, but I recommend [iTerm2](https://iterm2.com/) if you're on macOS.

Since I've been using Obsidian, any new notes and writing will be in the Obsidian directory/vault. But my blog posts had to be in their own folder within the repository for my website. Because of this, I wasn't sure if there was a seamless way to write and publish my posts. I could copy the contents from Obsidian into a new file in the file for the post and add all the metadata needed. Or I could already have the metadata in my Obsidian file (which I do using a blog-post template now) and copy that file into the `_posts` folder. Then I would have to execute all the git commands to publish the changes to my website (aka uploading or editing a post). Since I'm lazy, I didn't want to do any of that and knew there could be an easier way with technology, so I thought to write a script to do it for me.

I wanted to write a script to publish blog posts to:
1. even see if I can do this (I'm getting back into and learning programming)
2. keep using Obsidian as my markdown editor because of all the benefits of my notes, linking, sources, etc. 
3. not manually do all the tasks described above every time I wanted to publish or edit a post

After some thinking through it, and some trial and error, I did it. And it was actually really easy with the help of Google. I'm also currently going through [The Missing Semester](https://missing.csail.mit.edu/), and lecture 2 on [shell tools and scripting](https://missing.csail.mit.edu/2020/shell-tools/) helped. I probably only thought of this idea because I recently watched that lecture and went through the exercises.

I created a file: `touch publish.sh` and then edited it: `vim publish.sh`, and this is what that file contains now:
```bash
publish () {
	cd ~/Library/Mobile\ Documents/iCloud~md~obsidian/Documents/Obsidian\ Vault/posts
	# move into the posts folder in Obsidian Vault directory in iCloud Drive (so I can call this command from any location and not have to include the above path in the argument)

	cp $1 ~/path/to/website-repo/_posts/$1
	# copy first argument (the file/post) into the posts folder in my website repository

	cd ~/path/to/website-repo
	# move into the directory of my website repository, so I can complete git commands to publish changes


	git add --all
	git commit -m "publish $1"
	git push
}
```

```bash
publish () {
  cd ~/Library/Mobile\ Documents
  /iCloud~md~obsidian/Documents/
  Obsidian\ Vault/posts
  # move into the posts folder 
  # in Obsidian Vault directory 
  # in iCloud Drive (so I can 
  # call this command from any 
  # location and not have to 
  # include the above path in the 
  # argument)

  cp $1 ~/path/to/website-repo/
  _posts/$1
  # copy first argument (the file
  # /post) into the posts folder 
  # in my website repository

  cd ~/path/to/website-repo
  # move into the directory of my 
  # website repository, so I can 
  # complete git commands to 
  # publish changes

  git add --all
  git commit -m "publish $1"
  git push
}
```
It doesn't really matter the location of this file (I just put it in a folder called code for now).

To execute this script in our shell and (re)load the definition, type `source publish.sh`. Now the publish function has been defined in our shell and we can do `publish 2022-06-20-title.md` for example. This copies that file from your Obsidian Vault (or whatever location you have your writing in, using whatever text editor) into your website's repository and commits and pushes the post (or its edits) to GitHub.

*Edit: it seems that if you quit the terminal, you have to `source` again. Since I'm still new to this, I did some googling. The solution for me is to put `source ~/code/publish.sh` into `~/.bash_profile`. Then it will load the definition for every shell I think, and I don't have to type `source publish.sh` ever again.*

Jekyll requires the format of YYYY-MM-DD-title.md for posts, which is a bit long and annoying to type out with the date, so I'm not sure if there's an easier way to deal with this in general. Maybe it's possible to add stuff in the script, but found it easier for now to just make the title of the Obsidian file the same format.

So for this blog post, after I finished writing it in Obsidian, all I had to do was switch to iTerm and type `publish 2022-06-20-blogging-setup.md` (I copied the title from Obsidian to paste into the command and added '.md'). The post is now published on my website. Simple and easy as that.

## Conclusion
The system's not perfect (works well if you edit/change contents within the file, but if you change the file name, you have to separately remove the old one from the git repo). I haven't been using it for too long, so there may be changes (or not).

Let me know if there's any ways to improve this, and what tools and software you use for writing and your website. I'm always curious and would love to see what systems others use.

There's other great website options like [Ghost](https://ghost.org/) (which I've tried - read [this](https://balajis.com/set-up-a-paid-newsletter-at-your-own-domain/)) and Notion (w/ [Super](https://super.so/)), which seem really easy to use, but I also feel like I learned more about the technical side by using what I described above.
