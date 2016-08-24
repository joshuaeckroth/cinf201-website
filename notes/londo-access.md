---
title: Londo and MySQL access
layout: note
---

# Londo and MySQL access

We will use a server on Stetson campus known as "londo" for accessing
the database and submitting assignments. You can access londo from
anywhere but to do so you must use a tool like Putty or
SSH. Instructions are shown below for Windows and Mac systems.

For future reference, londo's address is 147.253.255.20

## Londo access

Most of your work with londo will use console access. For Windows, I recommend you use Putty. For Mac, you can use the builtin SSH tool.

### Windows

Putty is already installed on the school machines. [Download Putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) for your own machine if you wish (choose "putty.exe" link).

Launch Putty:

![Putty](/images/putty-1.png)

Type in londo's address:

![Putty](/images/putty-2.png)

The first time you connect, you will be asked if you trust the machine you are connecting to. Say "Yes."

![Putty](/images/putty-3.png)

Type your username and password:

![Putty](/images/putty-4.png)

The first time you connect, run: `tmux`. Every time you reconnect (for the rest of the semester), run `tmux attach` instead.

![Putty](/images/putty-5.png)

Now you are just where you left off from your last connection:

![Putty](/images/putty-6.png)

When you are done, just close the Putty window.

### Mac

Start the Terminal app and run `ssh username@147.253.255.20` where "username" is your username:

![Terminal](/images/ssh-1.png)

The first time you connect, you will be asked if you trust the machine you are connecting to. Type "yes", and then type your password.

![Terminal](/images/ssh-2.png)

The first time you connect, run: `tmux`. Every time you reconnect (for the rest of the semester), run `tmux attach` instead.

![Terminal](/images/ssh-3.png)

Now you are just where you left off from your last connection:

![Terminal](/images/ssh-4.png)

When you are done, just close the Terminal window.

## File transfer

You can write your files on your machine or on londo itself (in the console). If you write on your machine, you'll need to transfer files to londo. You may also wish to copy files out of londo for backup purposes.

### Windows

Use [FileZilla](https://filezilla-project.org/download.php?type=client) on Windows to transfer files to/from londo. Start FileZilla:

![FileZilla](/images/filezilla-1.png)

Type in the host (londo's address), your username and password, and port "22", and click "Quickconnect":

![FileZilla](/images/filezilla-2.png)

Find your local files on the left, and londo files on the right. Drag files back and forth to copy to/from londo:

![FileZilla](/images/filezilla-3.png)

### Mac

Use [Cyberduck](https://cyberduck.io/?l=en) on Mac to transfer files to/from londo. Use this address in the "Quick Connect" box (change the username):

```
sftp://jeckroth@147.253.255.20
```

![Cyberduck](/images/cyberduck-1.png)

The window will show londo files. Find your files in the window and drag/drop with the Finder on your Mac:

![Cyberduck](/images/cyberduck-2.png)


## Editing files

### Windows / Mac

Use a text editor, not Microsoft Word. Good text editors include:

- Windows: [Notepad++](https://notepad-plus-plus.org/), [Sublime Text](https://www.sublimetext.com/), [Atom](https://atom.io/)
- Mac: [Sublime Text](https://www.sublimetext.com/), [Atom](https://atom.io/), [TextWrangler](http://www.barebones.com/products/textwrangler/)

### Londo

You can edit your files directly on londo. I recommend `nano` for this purpose. Start `nano` like this:

```
nano [myfile.sql]
```

This is what nano looks like:

![nano](/images/nano.png)

Use Ctrl-key commands listed at the bottom of the window to exit (Ctrl-x), save your file (Ctrl-o), load a new file (Ctrl-R), etc.


## MySQL

Much of our work will take place in the "MySQL client," i.e., the tool that allows us to communicate with the database and run queries. The client is accessible on the londo console. Start the client by running `mysql`:

![MySQL client](/images/mysql-client-1.png)

Here is the startup screen:

![MySQL client](/images/mysql-client-2.png)

Typically, you'll start by "using" your own database:

![MySQL client](/images/mysql-client-3.png)

From there, you can run "select" queries, etc.

Quit the client by typing "exit".

## tmux

Whenever I use londo or other servers, I use tmux ("terminal
multiplexer"). tmux allows me to quit Putty or the Terminal and come
back later without disrupting what I was doing.

If you have never started tmux, then you'll need to run the `tmux`
command after connecting to londo. If you already have tmux running
(or at least you think you might), run `tmux attach` instead after
connecting.

Once inside tmux, you can do some fancy "window" operations:

- `Ctrl-b c` -- create a new window (windows are listed at the bottom of the screen)
- `Ctrl-b n` -- switch to "next" window
- `Ctrl-b p` -- switch to "previous" window
- `Ctrl-b [` -- freeze the window and scroll up/down with arrow keys or page-up/page-down; use `q` or `Escape` to unfreeze

You can also split a window into "panes":

- `Ctrl-b %` -- split the window vertically
- `Ctrl-b "` -- split the window horizontally
- `Ctrl-b o` -- switch to the other pane

Type "exit" in the console to close a window or pane.

## Homework submission

Before you start a new assignment, create a directory and cd (change directory) into it:

~~~
cd
mkdir -p cinf201/A01
cd cinf201/A01
~~~

Code your solution, then test it (if you wish):

~~~
cinf201-test A01
~~~

When you are ready to submit, run the submit command:

~~~
cinf201-submit A01
~~~

You will see output similar to the following:

~~~
CINF 201 homework tester and submitter
======================================
You are testing and submitting for homework A01.

Note, all files listed below will be submitted. Continue with these files?

>> 4.0K A01.sql

Enter y to continue, any other letter to quit: y

Testing...


CINF 201 homework tester
======================================
You are testing homework A01.

ok 1 - A01.sql exists
ok 2 - Output matches.
1..2


All tests passed!


All tests pass!

Submitting...done.

Here are all of your submissions. Note, only the last submission will be graded.

4.0K jeckroth-A01-2016-08-14_13:35:00.tar.gz
~~~

