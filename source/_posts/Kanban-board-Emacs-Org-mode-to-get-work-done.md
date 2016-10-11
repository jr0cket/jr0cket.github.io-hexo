title: Kanban in Emacs Org-mode to get more work done
date: 2016-09-04 15:42:41
categories: emacs
tags:
- spacemacs
- emacs
---

{% img img-thumbnail /images/emacs-logo.png %}

A Kanban board is a way to visualise your work and help you get more work done.  You organise your work into tasks that need completeing and use the board to show the state of each card.  Kanban encourages you to get work finished before starting new work.

The amazing Emacs Org-mode can be used to create a very fast and easy to use Kanban board that is with you where ever you are.

**Update**: Using Org-mode doesnt give me everything I want from a Kanban board, but it was an interesting exersice.  For now, I am just sticking to [my list view of a Kanban board](http://jr0cket.co.uk/2013/08/configure-emacs-org-mode-to-manage-your-tasks.html.html).

> Org-mode is built into Emacs / Spacemacs so there is no need to install any packages or layers for any of the following.

<!-- more -->

# Designing a kanban board

The columns on your kanban board represent the state of work and represent your typical workflow.  You can represent the states as the most generic **todo, doing, done** workflow, or anything more specific that adds value to how you manage work.

I have been using kanban for a while, so I am using a five stage workflow: **planning, in progress, blocked, review, done**

* **planning** - work I'd like to do that needs organising so I can do it.
* **in progress** - what I am currently working on. I try and keep this to a minimum so I get things done
* **blocked** - things I've started working on but currently arent able to complete
* **review** - work I have completed. Check if there are any follow on tasks or lessons learnt
* **done** - things I have completed. Gives feeling of satisfaction

# Creating Org-mode stages

Its easy to create your own Org-mode stages, to represent the state of work in your Kanban board.  Please see my earlier article on [Configuring Emacs Org-Mode to Managing Your Tasks](http://jr0cket.co.uk/2013/08/configure-emacs-org-mode-to-manage-your-tasks.html.html)

# Create an Org-mode file

Create a new file by opening a new buffer `M-x find-files` and type in the new file name, ending in `.org`.  Any files with a `.org` filename extension will automatically set the Emacs major mode to Org-mode.

I use a file called `kanban.org` for my kanban board.

| Spacemacs                       | Emacs            |
| ---                             | ---              |
| `SPC f f`                       | `C-x C-f`        |
| `M-x spacemacs/helm-find-files` | `M-x find-files` |

# Create a kanban board

Lets assume you created a file called `kanban.org`.  Edit this file and create a table for the kanban board.  You can start creating ths manually by typing `|` for the table layout or use `M-x org-table-create` and enter the number of columns and rows for the table.  For example, to create for a table with 5 columns and 3 rows, you would speciify `5x3`

Add the names of the kanban board in the first row of the table.  If you did not use `M-x org-table-create` then add a header row with `M-x org-table-insert-hline`.

In my kanban board, this gives

[![Emacs Org-mode table as Kanban board](/images/emacs-kanban-org-mode-table.png)](/images/emacs-kanban-org-mode-table.png)


# Adding tasks to the kanban board

Each item on the board represents a task and we use the Org-mode interal link to jump from the board to the details of the task.  To create a link of the same name as the task, simply type the name inside double square brakets `[[]]`.

[![Emacs Org-mode table as Kanban board - task entry](/images/emacs-kanban-org-mode-table-item.png)](/images/emacs-kanban-org-mode-table-item.png)


# Moving the tasks across the board

Its easy enough to move the columns around with `Alt - <arrow-keys>` in org-mode, but there is not a single keybinding to move a cell.

To move the individual tasks between the columns use selective cut and paste:

* Move the cursor to the cell you want to move and use `C-c C-x C-w`
* Use `TAB` to move to the new cell
* Paste/Yank the value into the new cell using `C-c C-x C-y`


However, simply moving the task does not update the Org-mode stage.  As each task is a link, I can click on that link and I am taken to the task and can easily update the task stage to match the board.

It would be great if moving the tasks on the board updated the associated task stage and vice versa.

# El Kanban - updating the board from task stage changes

I found the [El Kanban](http://www.draketo.de/light/english/free-software/el-kanban-org-table) package that will updated the kanban board based on the task org-mode stages.  This uses the Org-mode table format directive that you run each time you want to update the board.

I installed this package and it did pull in my custom org-mode stages for the headers.  Unfortunately it did not pull in the tasks to the board, so I will either need to fix the package or find another solution.

Any suggestions are more than welcome.


**References**
* [Configuring Emacs Org-mode to manage your tasks](http://jr0cket.co.uk/2013/08/configure-emacs-org-mode-to-manage-your-tasks.html.html)
* [Emacs Org-mode Kanban pomodoro... oh my...](http://www.agilesoc.com/2011/08/08/emacs-org-mode-kanban-pomodoro-oh-my/) -  Posted on August 8, 2011 by Bryan Morris
* [El Kanban](http://www.draketo.de/light/english/free-software/el-kanban-org-table) - an org-mode table that updates based on task stages

Thank you.
[@jr0cket](https://twitter.com/jr0cket)
