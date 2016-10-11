title: spacemacs - adding custom snippets to yasnippet
date: 2016-07-23 13:23:46
categories: emacs
tags:
- emacs
- spacemacs
- yasnippets
---

{% img img-thumbnail /images/spacemacs-logo.png %}

Using yasnippet saves time by avoiding the need to write boilerplate code and minimising other commonly typed content.  YASnippet contains mode-specific snippets that expand to anything from a simple text replacement to a code block structure that allows you to skip through parameters and other sections of the code block.  See YASnippet in action in this [Emacs Yasnippet video](https://www.youtube.com/watch?v=-4O-ZYjQxks).

To use a specific snippet simply type the alias and press `M-/`.  For example, in html-mode typing `div` and pressing `M-/` expands to `<div id="▮" class="▯">▯</div>` and places the cursor so you can type in the `id` name, then `TAB` to the `class` name, finally `TAB` to the contents of the div.

You can also combine yasnippets with autocompletion select snippets from the autocompletion menu.

Spacemacs has lots of snippets for most of the languages and modes it supports.  However, YASnippets also uses a simple template system in plain text, so its pretty easy to learn.  Lets look at how to add your own snippets with Spacemacs.

> In regular Emacs, yasnippets expand funciton is usually bound to `TAB`, but that key is used already in Spacemacs so `M-/` is used instead.
> If you just want text replacement you can also use [Emacs Abbrev mode](http://ergoemacs.org/emacs/emacs_abbrev_mode.html).

<!-- more -->

# Adding your private snippets to Spacemacs

The easiest place to add your own snippet definitions is in the `~/.emacs.d/private/snippets` directory.  Under this directory structure you should create a folder named after the relevant mode for your snippets, eg `markdown-mode`.  Inside this mode folder, create files whos names are based on the snippet alias you wish.

So for a work in progress snipped called `wip` in markdown mode I created `~/.emacs.d/private/snippets/markdown-mode/wip` file.

You need to load this new snippet into Spacemacs by either restarting or using the command `M-x yas-load-snippet-buffer` command in the buffer of the new snippet you have just written.  Ths snippet with then work within any markdown mode buffer.

## Managing your snippets

Although the private snippets directory is easy to use, it is not under version control.  So although its not over-riddend by Spacemacs it also means your private snippets are not backed up anywhere.

If you use the `~/.spacemacs.d/snippets/modename-mode/` directory structure for your snippets then you can version them with Git or similar versioning tools.


# How to write a snippet

Typically each snippet template is contained in its own file, named after the alias of the snippet.  So a snippet called `wip` will be in a filename wip, in a directory named after the relevant Emacs mode.

The basic structure of a snippet template is:

```
#key : the name of the snippet you type
#name : A description of the snippet (this shows in autocompletion menu too)
#contributor: John Stevenson <john@jr0cket.co.uk>
# --
Add the content you want to replace the snippet name with when it expands
```
The content can be anything, simple text or more usefully a code strucuture with placeholders for tab stops.  You can even include Emacs lisp (elisp) code in there too.


## Example: Simple text replacement

I use markdown mode for writing a lot of content, especially for technical workshops.  As I am developing these workshops its useful to highlight which sections are still work in progress.  Rather than type the common message I use, I've created a simple snippet called `wip`.

```
#key : wip
#name : WorkInProgress
#contributor: John Stevenson <john@jr0cket.co.uk>
# --
> **Fixme** work in progress
```

When you expand this snippet with `M-/` then the snippet name is replaced by the content.

## Example: Using tab stops

Lets look at an existing snippet called `form` in the `html-mode`.  This expands into a html form, but also helps you jump from method, id, action and content.
```
#contributor : Jimmy Wu <frozenthrone88@gmail.com>
#name :<form method="..." id="..." action="..."></form>
# --
<form method="$1" id="$2" action="$3">
  $0
</form>
```

This snippet is the same as the simpler example, except we have added **tab stops** using the `$` sign and a number.  When you expand this snippet, the snippet name is replaced by the content as usual but the cursor is placed at the first tab stop `$1`.  Each time you press `TAB` you move to the next tab stop.

`$0` is our exit point from the snippet, so pressing `TAB` reverts to the usual behaviour outside of YASnippet.


# Creating a snippet from existing text

A really fast way of creating a new snippet is to use a finished version of what you would like the snippet to expand to.  For a simple text replacement you just hightlight all the text and call `helm-yas-create-snippet-on-region`, save the snippet and you are done.

For a code structure with tab stops, simply hightlhght a completed code stucture, call `helm-yas-create-snippet-on-region` and edit the body of your snippet to replace the specific names and values with tab stop placeholders, `$1` `$2`, `$3`, etc.


## Example: Create a simple text replacement

When I write blogs I include a image thumbnail that gives a visual clue as to the topic of the article.  Rather than type this in I created a snippet.

First I mark the text I want my new snippet to expand too, in this example: ** {% raw %} {% img img-thumbnail /images/spacemacs.png %} {% endraw %} **.

Then I call the function `helm-yas-create-snippet-on-region`.  This prompts me for the mode for the snippet, in this case markdown-mode, then prompts for the location for the snippet file, `~/.emacs/private/snippets/markdown-mode/imgtmb-spacemacs`.  A new buffer is created with my snippet already filled in.

```
# -*- mode: snippet -*-
#name : imgtmb-spacemacs
#key : imgtmb-spacemacs
#contributor : jr0cket
# --
{% img img-thumbnail /images/spacemacs.png %}

```

The new snippet buffer already has the name and key values populated from the filename I gave for the snippet, `imgtmb-spacemacs`.  The snippet body is also populated automatically from the text I had highlighted.  So all I need to do is save the new snippet and try it out.

# Testing your snippets

Once you have written your snippet, you can quickly test it using `M-x yas-tryout-snippet`.  This opens a new empty buffer in the appropriate major mode and inserts the snippet so you can then test it with `M-/`.

If you just want to try the snippet in an existing buffer, then use `M-x yas-load-snippet-buffer` to load this new snippet into the correct mode.  `M-x yas-load-snippet-buffer` does exactly the same except it kills the snippet buffer (prompting to save first if neccessary).

> There are no default keybindings for these commands in Spacemacs, so you could create a binding under `C-o`, for example `C-o C-s t` to try a snippet and `C-o C-s l` to load a snippet.


# Adding yas-snippets to autocompletion in Spacemacs

By adding the `autocompletion` layer in Spacemacs the YASnippets can be shown in the autocompletion menu as you type.

By default, snippets are not shown in the auto-completion popup, so set the variable `auto-completion-enable-snippets-in-popup` to `t`.

```elisp
(setq-default dotspacemacs-configuration-layers
              '((auto-completion :variables
                                 auto-completion-enable-snippets-in-popup t)))
```

# Summary

Find out more about YASnippets and autocompletion from the [Github repository for Spacemacs autocompletion layer](https://github.com/syl20bnr/spacemacs/tree/master/layers/auto-completion).

For more details and examples on writing your own snipplets, take a look at:
* [Emacs YASnippet video tutorial](https://www.youtube.com/watch?v=-4O-ZYjQxks)
* [Snippet development](https://joaotavora.github.io/yasnippet/snippet-development.html).
* [Adding YASnippets snippets](http://jotham-city.com/blog/2015/03/21/adding-yasnippets-snippets/)
* [Snippet expansion with YASnippet](http://cupfullofcode.com/blog/2013/02/26/snippet-expansion-with-yasnippet/index.html)


Thank you.
[@jr0cket](https://twitter.com/jr0cket)
