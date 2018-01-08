---
title: "Working with Files and Directories"
teaching: 30
exercises: 15
questions:
- "How can I view and search file contents?"
- "How can I create, copy and delete files and directories?"
- "How can I control who has permission to modify a file?"
- "How can I repeat recently used commands?"
objectives:
- View, search within, copy, move, and rename files. Create new directories.
- Use wild cards (`*`) to perform operations on multiple files.
- Use the `history` command to view and repeat recently used commands.
keypoints:
- "You can view file contents using `less`, `cat`, `head` or `tail`."
- "The commands `cp`, `mv`, and `mkdir` are useful for manipulating existing files and creating new directories."
- "You can view file permissions using `ls -a` and change permissions using `chmod`."
- "The `history` command and the up arrow on your keyboard can be used to repeat recently used commands."
---

## Working with Files

### Our data set: FASTQ files

Now that we know how to navigate around our directory structure, lets
start working with sequencing files. We did a sequencing experiment and 
have two results files, which are stored in our `untrimmed_fastq` directory. 

### Wild cards

Navigate to your `untrimmed_fastq` directory.

~~~
$ cd ~/dc_sample_data/untrimmed_fastq
~~~
{: .bash}

We are interested in looking at the FASTQ files in this directory. We can list
all files with the .fastq extension using the command:

~~~
$ ls *.fastq
~~~
{: .bash}

~~~
SRR097977.fastq  SRR098026.fastq
~~~
{: .output}

The `*` character is a wildcard character which stands for "everything". 
Thus, `*.fastq` matches every file that ends with `fastq`. 

This command: 

~~~
$ ls *977.fastq
~~~
{: .bash}

~~~
SRR097977.fastq
~~~
{: .output}

lists only the file that ends with `977.fastq`.

We can use the command `echo` to see how the wildcard character is intepreted by the
shell.

~~~
$ echo *.fastq
~~~
{: .bash}

~~~
SRR097977.fastq SRR098026.fastq
~~~
{: .output}

The `*` is expanded to include any file that ends with `.fastq`.

This command:

~~~
$ ls /usr/bin/*.sh
~~~
{: .bash}

~~~
/usr/bin/amuFormat.sh  /usr/bin/gettext.sh  /usr/bin/gvmap.sh
~~~
{: .output}

Lists every file in `/usr/bin` that ends in the characters `.sh`.

> ## Home vs. Root
> 
> The `/` character is another navigational shortcut and refers to your root directory.
> The root directory is the highest level directory in your file system and contains
> files that are important for your computer to perform its daily work, but which you usually won't
> have to interact with directly. In our case,
> the root directory is two levels above our home directory, so `cd` or `cd ~` will take you to `/home/dcuser`
> and `cd /` will take you to `/`, which is equivalent to `~/../../`. Try not to worry if this is confusing,
> it will all become clearer with practice.
{: .callout}

> ## Exercise
> Do each of the following tasks from your current directory using a single
> `ls` command for each.
> 
> 1.  List all of the files in `/usr/bin` that start with the letter 'c'.
> 2.  List all of the files in `/usr/bin` that contain the letter 'a'. 
> 3.  List all of the files in `/usr/bin` that end with the letter 'o'.
>
> Bonus: List all of the files in 'usr/bin' that contain the letter 'a' or the
> letter 'c'.
> 
> Hint: The bonus question requires a Unix wildcard that we haven't talked about
> yet. Trying searching the internet for information about Unix wildcards to find
> what you need to solve the bonus problem.
> 
> > ## Solution
> > 1. `ls /usr/bin/c*`
> > 2. `ls /usr/bin/*a*`
> > 3. `ls /usr/bin/*o`  
> > Bonus: `ls /usr/bin/*[ac]*`
> > 
> {: .solution}
{: .challenge}


## Examining Files

We now know how to switch directories, run programs, and look at the
contents of directories, but how do we look at the contents of files?

One way to examine a file is to print out all of the
contents using the program `cat`. 

Enter the following command from within the `untrimmed_fastq` directory: 

~~~
$ cat SRR098026.fastq
~~~
{: .bash}

This will print out all of the contents of the `SRR098026.fastq` to the screen.


> ## Exercise
> 
> 1. Print out the contents of the `~/dc_sample_data/untrimmed_fastq/SRR097977.fastq` file. What is the last line of the file? 
> 2.  From your home directory, and without changing directories,
> use one short command to print the contents of all of the files in
> the `~/dc_sample_data/untrimmed_fastq` directory.
> 
> > ## Solution
> > 1. The last line of the file is `TC:CCC::CCCCCCCC<8?6A:C28C<608'&&&,'$`.
> > 2. `cat ~/dc_sample_data/untrimmed_fastq/*`
> {: .solution}
{: .challenge}

`cat` is a terrific program, but when the file is really big, it can
be annoying to use. There's another way that we can look at files, and in this case, just
look at part of them. This can be particularly useful if we just want
to see the beginning or end of the file, or see how it's formatted.

The commands are `head` and `tail` and they let you look at
the beginning and end of a file, respectively.

~~~
$ head SRR098026.fastq
~~~
{: .bash}

~~~
@SRR098026.1 HWUSI-EAS1599_1:2:1:0:968 length=35
NNNNNNNNNNNNNNNNCNNNNNNNNNNNNNNNNNN
+SRR098026.1 HWUSI-EAS1599_1:2:1:0:968 length=35
!!!!!!!!!!!!!!!!#!!!!!!!!!!!!!!!!!!
@SRR098026.2 HWUSI-EAS1599_1:2:1:0:312 length=35
NNNNNNNNNNNNNNNNANNNNNNNNNNNNNNNNNN
+SRR098026.2 HWUSI-EAS1599_1:2:1:0:312 length=35
!!!!!!!!!!!!!!!!#!!!!!!!!!!!!!!!!!!
@SRR098026.3 HWUSI-EAS1599_1:2:1:0:570 length=35
NNNNNNNNNNNNNNNNANNNNNNNNNNNNNNNNNN
~~~
{: .output}

~~~
$ tail SRR098026.fastq
~~~
{: .bash}

~~~
+SRR098026.247 HWUSI-EAS1599_1:2:1:2:1311 length=35
#!##!#################!!!!!!!######
@SRR098026.248 HWUSI-EAS1599_1:2:1:2:118 length=35
GNTGNGGTCATCATACGCGCCCNNNNNNNGGCATG
+SRR098026.248 HWUSI-EAS1599_1:2:1:2:118 length=35
B!;?!A=5922:##########!!!!!!!######
@SRR098026.249 HWUSI-EAS1599_1:2:1:2:1057 length=35
CNCTNTATGCGTACGGCAGTGANNNNNNNGGAGAT
+SRR098026.249 HWUSI-EAS1599_1:2:1:2:1057 length=35
A!@B!BBB@ABAB#########!!!!!!!######
~~~
{: .output}

The `-n` option to either of these commands can be used to print the
first or last `n` lines of a file. 

~~~
$ head -n 1 SRR098026.fastq
~~~
{: .bash}

~~~
@SRR098026.1 HWUSI-EAS1599_1:2:1:0:968 length=35
~~~
{: .output}

~~~
$ tail -n 1 SRR098026.fastq
~~~
{: .bash}

~~~
A!@B!BBB@ABAB#########!!!!!!!######
~~~
{: .output}

## Creating, moving, copying, and removing

Now we can move around in the file structure, look at files, search files, and
redirect output to new files or to other commands. But what if we want to copy files or move
them around or get rid of them. Most of the time, you can do these sorts of file manipulations without the command line,
but there will be some cases (like when you're working with a remote computer like we are for this lesson) where it will be
impossible. You'll also find that you may be working with hundreds of files and want to do similar manipulations to all 
of those files. In cases like this, it's much faster to do these operations at the command line.

### Creating Directories

The `mkdir` command is used to make a directory. Enter `mkdir`
followed by a space, then the directory name you want to create.

~~~
$ mkdir backup
~~~
{: .bash}

### Moving / Renaming 

We can now move our backup file to this directory. We can
move files around using the command `mv`. 

~~~
$ mv SRR098026-copy.fastq backup
$ ls backup
~~~
{: .bash}
 
~~~
SRR098026-copy.fastq
~~~
{: .output}

The `mv` command is also how you rename files. Let's rename this file to make it clear that this is a backup.

~~~
$ cd backup
$ mv SRR098026-copy.fastq SRR098026-backup.fastq
$ ls
~~~
{: .bash}

~~~
SRR098026-backup.fastq
~~~
{: .output}


### Removing

To prove to ourselves that you no longer have the ability to modify this file, try deleting it with the `rm` command.

~~~
$ rm SRR098026-backup.fastq
~~~
{: .bash}

You'll be asked if you want to override your file permissions.

~~~
rm: remove write-protected regular file ‘SRR098026-backup.fastq’? 
~~~
{: .output}

If you enter `n` (for no), the file will not be deleted. If you enter `y`, you will delete the file. This gives us an extra 
measure of security, as there is one more step between us and deleting our data files.

Important: The `rm` file permanently removes the file. Be careful with this command. It doesn't
just nicely put the files in the Trash. They're really gone.

By default, `rm`, will not delete directories. You can tell `rm` to
delete a directory using the `-r` (recursive) option. Let's delete the backup directory
we just made. 

Enter the following command:

~~~
$ cd ..
$ rm -r backup
~~~
{: .bash}

This will delete not only the directory, but all files within the directory. If you have write-protected files in the directory, 
you will be asked whether you want to override your permission settings. 

> ## Exercise
>
> Do the following:  
> 1. Make sure that you have deleted your backup directory and all files it contains.  
> 2. Create a copy of each of your FASTQ files. (Note: You'll need to do this individually for each of the two FASTQ files. We haven't 
> learned yet how to do this
> with a wild-card.)  
> 3. Use a wildcard to move all of your backup files to a new backup directory.   
> 4. Change the permissions on all of your backup files to be write-protected.  
>
> > ## Solution
> >
> > 1. `rm -r backup`  
> > 2. `cp SRR098026.fastq SRR098026-backup.fastq` and `cp SRR097977.fastq SRR097977-backup.fastq`  
> > 3. `mkdir backup` and `mv *-backup.fastq backup`
> > 4. `chmod -w backup/*-backup.fastq`   
> > It's always a good idea to check your work with `ls -l backup`. You should see something like: 
> > 
> > ~~~
> > -r--r--r-- 1 dcuser dcuser 47552 Nov 15 23:06 SRR097977-backup.fastq
> > -r--r--r-- 1 dcuser dcuser 43332 Nov 15 23:06 SRR098026-backup.fastq
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

{% include links.md %}
