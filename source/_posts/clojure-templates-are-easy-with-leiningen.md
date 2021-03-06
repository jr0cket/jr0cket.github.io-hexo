title: Clojure templates are easy with Leiningen
date: 2015-03-09 22:58:28
categories: clojure
tags:
- clojure
- coding
- leiningen
---

{% img img-thumbnail /images/clojure-logo.png %}

  Using templates to create your Clojure projects can save you a lot of setup time and ensure your team is using the same base configuration and dependencies.  There are [templates on Clojars.org](https://clojars.org/search?q=lein-template), however I'll show you how easy it is to create your own with [Leiningen](http://leiningen.org).

  I'll create a simple template based on the leiningen default template, adding a section in the project.clj to give a custom propmt when run in the repl.

> Templates used to be a Leinigen pluging called [lein-newnew](https://github.com/Raynes/lein-newnew) and its repo was the only doucmentation I found and was a little outdated.  The plugin is now part of Leiningen and there are a few [built in templates](https://github.com/technomancy/leiningen/tree/master/src/leiningen/new).  There is also information via `lein help new`.

> If you want to create a template in a more automatic way from a more complete project you created, take a look at the [lein-create-template](https://github.com/tcw/lein-create-template) Leinignen plugin.

<!-- more -->

## Creating templates

  A Clojure template is created in the same was as a Clojure project, however a template called `template` is used

```bash
    lein new template your-template-name
```

  I created a new template called `jr0cket-prompt`, so where you see this name in the following commands, substitue your own template name

```bash
    lein new template jr0cket-prompt
```

![](/images/leiningen-template-new-jr0cket-prompt.png)

> The documentation for _lein-newnew_ uses the `--to-dir` to specify the name of a directory to create the template in.  This is only useful if you want to give the directory a different name to the template name.


## Overview of the template

![Directory structure of jr0cket-prompt template](/images/leiningen-template-new-jr0cket-prompt-tree.png)

`project.clj` - this is the same as any other project.clj file, except the project name has `/lein-template` after it.  This allows leiningen find it on [Clojars.org](http://clojars.org).

`source/leiningen/new/clj_jr0cket_dojo_template.clj` - defines how a project is created from this template.  For example, defining which files the template generates and how it creates them.

`resources/leiningen/new/clj_jr0cket_dojo/` - this is where you put all the source & project files that make up your template, using tags where the name of a new project should be substituted.

## Configure the template definition

  My template will have a customised `project.clj file`.  The rest of the template is the same as the default Leiningen template. So I edited the `src/leiningen/new/jr0cket_prompt.clj` and added code to create the threee files for my project, in the correct paths.

>  The project contains a `project.clj` file containing my prompt modifications and the `core.clj` file for the `src` and `test` branch.

> The `sanitized` tag is used to change any `-` characters in the project name to `_` characters, so the directory names do not cause issues for Java.  The `render` specifies which file in the `resources` directory a new file is generated from.

![Leiningen template definition - files to be generated by the template](/images/leiningen-template-new-jr0cket-prompt-definition.png)

## Copy files into the template

  I now add the files to the `resources` directory that my new project files are generated from, configuring each file to substitute the namespace and any other project specific information.

  From a project I previosly created with `lein new`, I copied over a `project.clj` file along with a `core.clj` file for `src` and `core_test.clj` for `test` directories.  These files all reside under `resources/leiningen/new/jr0cket_prompt/`.

## Add placeholder names to the template

  When a new Clojure project is created with your template, in this case using `lein new jr0cket-prompt new-project`, the name needs to be substitued into the new Clojure files so they have the correct namespace for the project.  The new `project.clj` file also needs to use the name of the new Clojure project.

  I edited the `project.clj` file to add the custom prompt information and a placeholder for the new project name.

![Leiningen template - custom project.clj file](/images/leiningen-template-new-jr0cket-prompt-project.png)

  For the `src/project/core.clj` and `test/project/core.clj` I add the name tag to the namespace definition.

![Leiningen template - src core.clj file](/images/leiningen-template-new-jr0cket-prompt-core.png)

![Leiningen template - test core_test.clj file](/images/leiningen-template-new-jr0cket-prompt-core_test.png)


## Testing the template

  Build the template project into a `.jar` file using leiningen by running the following command within the template directory:

    lein jar

  Now change to the `target` directory and create a new project using leiningen.

```bash
  cd target
  lein new myproject-template new-project-name
```

> By changing into the target directory, you are placing the jar file of the template onto the Java class path and therefore making it avaialble to leiningen.

## Installing the template

  Once you are happy with the template, you can use it locally by installing it into your library cache - `~/.m2/repository/`.  From the root of the template project, run the following command:

    lein install

## Publishing the template

  Once the template is ready to share with others, you can publish the jar on [Clojars.org](http://clojars.org) using the following command from the root of the template project:

    lein deploy clojars

> You should clean the project and rebuild it before publishing to Clojars to make sure there are no testing files remaining - `lein clean ; lein jar`

## Summary

  This has been just the simplest template I could think of.  There are many useful [helper functions](https://github.com/technomancy/leiningen/blob/de06302d28cfadcced25067e6ac04480f18789c5/src/leiningen/new/templates.clj) as part of Leiningen templates

* [Lein-new project](https://github.com/Raynes/lein-newnew) - this project was merged into Leiningen.  The documentation gives a good overview, however there are smoe things that are less than ceear.
* [Leiningen example templates](https://github.com/technomancy/leiningen/tree/master/src/leiningen/new) - examples of how to define a template
* [Leiningen template helper functions](https://github.com/technomancy/leiningen/blob/de06302d28cfadcced25067e6ac04480f18789c5/src/leiningen/new/templates.clj) - useful functions you can call in your template definition

## Community templates of note

  Templates others have created can be [found on Clojars.org](https://clojars.org/search?q=lein-template).  For example, [Splat](https://github.com/james-henderson/splat) is a template to create a ClojureScript single page web applications by [James Hendersons](https://github.com/james-henderson).  Malcolm Sparks has templates for his [Modularity.org projects](http://modularity.org/).

  Got and create your own templates and contribute them back via Clojars.org.

Thank you.
[@jr0cket](https://twitter.com/jr0cket)
