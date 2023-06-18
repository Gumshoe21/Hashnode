---
title: "Understanding git diff Output"
seoTitle: "Understanding git diff Output"
seoDescription: "git diff output can be extremely confusing. This article seeks to demystify the command's output syntax."
datePublished: Sun Jun 18 2023 21:28:38 GMT+0000 (Coordinated Universal Time)
cuid: clj1xwgdv000209lbhssw5g5e
slug: understanding-git-diff-output
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/wX2L8L-fGeA/upload/e97ac92078f6fb93e818f123b252964d.jpeg
tags: programming-blogs, github, version-control, git, technical-writing-1

---

`git diff` can be a confusing command to understand. In this tutorial, I aim to demystify the command by clarifying its ambiguities and clearly outlining its syntax so you never have to go "huh?" when it comes to `git diff` ever again.

# What is a Diff?

In Git, a diff shows the changes between two different states of your repository. These are some common comparisons that can be made using the `git diff` command:

* The working tree (or working directory) and the index (the staging area)
    
* The working tree and another tree
    
* The main branch and a feature branch
    
* The local repository and the remote repository
    

A good way to internalize the purpose of `git diff` is to remember that it is meant to show us the *difference* between two specific states of your project at different stages. If you wanted to know the *difference* or *diff* between your working directory and your staging area, you would use `git diff` to portray this *difference*.

As I've already stated, you can use `git diff` to show the changes between the different states of your repository. For the sake of this tutorial's main purpose, which is to clarify the `git diff` output syntax, I will demonstrate a basic example that uses `git diff` without arguments. In this form, `git diff` shows changes between the working tree and the index, also known as the working directory and staging area, respectively.

# Code-Along Example

For this example, I urge you to code along, but it is not required.

## Step 1: Create A New Git Repository

* From anywhere on your computer (this example will assume we are starting from our home directory), run the following commands, which will:
    
    * Create a new directory named `git_diff_tutorial`.
        
    * cd into that directory.
        
    * Initialize a new Git repository.
        

```bash
mkdir git_diff_tutorial
cd git_diff_tutorial
git init
```

## Step 2: Create a New File and Add Content

* Run the following commands, which will:
    
    * Create a new file, `a.txt`.
        
    * Add content to `a.txt`.
        

```bash
# From within ~/git_diff_tutorial, run these commands:
touch a.txt
echo "This is the change in the staging area." > a.txt
```

## Step 3: Add the Change to the Staging Area

* Run the following command, which will:
    
    * Add `a.txt` to the staging area:
        

```bash
git add a.txt
```

## Step 4: Add More Content to the File

* Now that you've added content (i.e. made changes) to the file and staged the file with said changes to the staging area, let's add some more content to the file, which will result in a *difference* between the version of the file as it exists in the staging area and the version of the file in the working directory:
    

```bash
# For the curious, -e enables interpretation of backslash escapes; this allows us to add an escape sequence for a newline (\n) so that we have two lines in our file.
echo -e "\nThis is the change in the working directory." >> a.txt
```

* Now, run `git status` so that we may see the current status of our working directory:
    

```bash
$ git status                                                             

On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   a.txt 

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   a.txt
```

* As you can see, we have two kinds of changes on our hands:
    
    1. Changes to be committed (i.e., changes you've made to the file and ran `git add` on). These changes are now in your *index*, also known as the staging area.
        
    2. Changes not yet staged for commit (i.e., any changes you've made to the file after the last `git add`). These changes are located in your *working tree*, also known as the working directory.
        

## Step 5: Run `git diff`

* We are now ready to run `git diff` to see our changes! Go ahead and run `git diff` without any arguments like so:
    

```bash
git diff
```

* Your output should look something like this:
    

```diff
diff --git a/a.txt b/a.txt
index 9db7f57..d3612ef 100644
--- a/a.txt
+++ b/a.txt
@@ -1 +1,2 @@
 This is the change added to the staging area
+This is the change in the working directory
```

* This is the meat of this tutorial, so let's dissect it.
    

# The Structure of `git diff` Output:

* Let's go line by line:
    

## Line 1 - The Header

```diff
diff --git a/a.txt b/a.txt
```

* `diff` - This indicates the command used to generate the diff.
    
* `--git` - This indicates that the diff was produced by the Git version control system.
    
* `a/a.txt b/a.txt` - This part represents the two versions of the file being compared.
    
    * `a/a.txt` - This is version A of the file. It is the "original" or "older" version of the file. In our scenario, this is the version of the file that's sitting in the *index* or staging area.
        
    * `b/a.txt` - This is version B of the file. It is the "new" or "modified" version of the file. In our scenario, this is the version of the file that's sitting in the *working tree* or working area.
        

## Line 2 - Metadata

```diff
index 9db7f57..d3612ef 100644
```

This line provides some metadata about the diff.

* `index` - Although "index" in the context of Git is a term that often refers to the staging area, in this context, it is being used in a similar way to how it's used in other systems that track changes, like databases or search engines. It's indicating that what follows is an indexed or key identifier of the data.
    
* `9db7f57..d3612ef` - These are the two SHA-1 hash identifiers that Git uses to refer to specific versions of the content. They are essentially "indexes" in Git's internal database of file contents, hence the preceding use of the term "index."
    
    * `9db7f57` - This SHA-1 hash identifies the "before" (version A) state of the file.
        
    * `..` - These two dots are used to represent a range from one state to another. You'll see this two-dot notation in various places while using Git. Think of it this way: "the difference between `9db7f57` and `d3612ef`" or "from to `9db7f57` and `d3612ef`."
        
    * `d3612ef` - This SHA-1 hash identifies the "after" (version B) state of the file.
        
* `100644` - This is the mode of the file in octal representation, indicating it's a normal, non-executable file. On the other hand, a mode of `100755` represents an executable file. For more information on modes, you can find an explanation in [Git Internals - Git Objects](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects) section of the Git documentation. Using the search function (Ctrl+f or Command+f on Mac) in your browser and typing "mode" should get you where you need to go.
    

## Line 3 - File Paths

```diff
--- a/a.txt
+++ b/a.txt
```

This line shows the file paths of the files before and after the change.

* `---` - These three signs are used as a matter of convention. The reason that three are used is to clearly distinguish the markers from content lines that might start with one or two minus or plus signs. For all intents and purposes, these three `-` signs represent the state of the file as it existed in its original state.
    
* `a/a.txt` - The original state of the file. The `a` before the `/` is an arbitrary identifier of the "before" state, and *does not* refer to a directory or path, even though it may seem so; it is simply a label that is used in the same way as it was in line 1.
    
* `+++` - These three `+` signs represent the state of the file as it exists after the changes.
    
* `b/a.txt` - The updated state of the file.
    

Essentially, these lines always refer to the same file, however, they represent *different* states of that file. You could say that `--- a/<file>` represents where the changes "come from", while `+++ b/<file>` represents where the changes "go to".

## Line 4 - Hunk Header

```diff
@@ -1 +1,2 @@
```

To me, this is the most confusing part of the entire output. Let's dissect it:

* `@@` - This marks the beginning (and later, the end) of the hunk header. It serves as a delimiter that Git uses to identify the hunk header.
    
* `-1` - This part indicates the changes in the original file (denoted by `-`). The `1` signifies that the changes begin from the first line. If, for instance, you see something like `-1,3`, it means that the changes start from the first line and span the next 3 lines in the original file. In this case, the `3` refers to the number of lines affected from the starting line onward.
    
* `+1,2` - This part indicates the changes in the updated file, as denoted by the `+` symbol. Here, the `1` indicates that the changes begin from the first line, and `2` signifies that from that starting point, there are 2 lines in the updated file, including the first line itself (thus, lines 1 and 2 are the two lines). If you encounter something like `+4,5`, it means the changes commence from the fourth line, and from there, the updated file includes a span of 5 lines (lines 4, 5, 6, 7, and 8). This span includes both the actual changes and the context around them in the new file.
    
* `@@` - This marks the end of the hunk header.
    

### Further Clarifying the Hunk Header

One crucial aspect of understanding hunks is that they include not only the changes to the file but also the ***context*** around those changes.

Remember that I indicated that the hunk in the updated file (`+1,2`) *starts* from the first line and includes 2 lines from that point? This hunk header doesn't imply that the first line was edited or added, but rather that the first line is the starting point for the *comparison* in this hunk.

When the hunk header says `+1,2`, it doesn't mean that actual changes/modifications to the file were made on both lines 1 and 2. Instead, it represents the section of the file, or the "hunk", that the changes are a part of, including both the *context* and the actual modifications. In our case, the change - an addition - is on line 2, but the hunk starts from line 1 to include some context.

At this point, you may be wondering why the hunk header isn't `+2,2` to indicate that the change starts and ends at line 2. The reason the hunk starts at line 1 is, again, to provide *context* (more on this below) for understanding the change; *context* enables you to see what the content looked like *before* the addition (the first line was `This is the change added to the staging area`), as well as see what the content looks like *after* the addition (`This is the change in the working directory`).

Another thing to be aware of is that the numbers delimited by the comma in `+1,2` do not mean "lines 1 to 2", but rather "starting from line 1, there are now 2 lines." It's about how many lines there are starting from the specified line, not a range of lines. So even if the actual addition was only the second line, `+1,2` just means there are 2 lines in the updated file starting from line 1. This is a huge source of confusion, so try to keep this in mind when reading the output.

## Lines 5 and 6 - Diff Lines (or Diff Content):

```diff
 This is the change added to the staging area.
+This is the change in the working directory.
```

Before we delve into the specific example we are dealing with, let's categorize the three types of diff lines:

1. **Context lines**: Lines that haven't changed between the two versions. These lines help you understand where in the file the changes are happening. In our example, `This is the change added to the staging area` is a context line. These lines are not preceded by any symbol.
    
2. **Deletion lines**: Lines that were present in the old version but are not present in the new version. These lines are preceded by a `-`. Our current example does not contain such lines.
    
3. **Addition lines**: Lines that are present in the new version but were not present in the old version. These lines are preceded by a `+`. In our example, `This is the change in the working directory` is an addition line.
    

The diff content essentially provides a before-and-after snapshot of your file. You can see the context (what parts of the file remained unchanged), the deletions (what parts were removed), and the additions (what parts were added). In other words, the diff content shows what was, what is, and what has changed between the two compared versions. Pretty powerful stuff, eh?

Let's try to interpret our diff lines further:

Going back to the hunk header, we can comprehend that the changes in the old file started at the first line and did not extend beyond that line (`-1`):

```diff
@@ -1 +1,2 @@
```

Having interpreted our hunk header, we can clearly observe the message it communicates:

```diff
 This is the change added to the staging area
+This is the change in the working directory
```

The changes in the old file *started* at the first line `This is the change added to the staging area` and did not extend beyond that line. The `This is the change in the working directory` line is not part of the original (old) file's content; it represents an addition to the new version of the file. Think of it this way: due to the fact that the `This is the change in the working directory` line didn't exist in the old version, there's no way for the changes to extend that far.

On the other hand, the new version *does* know about the `This is the change added to the staging area` line. In fact, it uses that line as context to describe where the new change has taken place. The `This is the change in the working directory` line is then an addition made in the new file, as shown by the `+` prefix. The hunk header tells us that starting from the first line in the new file, there are now two lines (`+1,2`), which includes the original context line and the newly added line.

Hence, the hunk header and the following lines together give us a complete picture of the difference between the old and the new file: the context where the change has happened, what was removed or modified from the old file, and what was added in the new file. This allows us to clearly understand the evolution of the file from its old state to its new state.

# Another Example

Let's look at a slightly more complex example that illustrates the inclusion of deletion lines. Suppose you have the following diff:

```diff
@@ -1,3 +1,3 @@
-This is the old first line
 This is the second line
-This is the old third line
+This is the new third line
```

The hunk header here is `@@ -1,3 +1,3 @@`. This means that Git started comparing from the first line in both versions of the file. Note that in both the original and new versions, there were 3 lines starting from the first line.

Now, you might be wondering why there are four lines shown in the hunk when the hunk headers imply changes that include only three lines in both the original (`-1,3`) and updated versions (`+1,3`) of the file. This discrepancy arises because the hunk includes a ***context line*** that is unchanged in both versions of the file.

Remember, the hunk is not just about the changes (additions and deletions), but about the entire section of the file that those changes impact, including the context around the changes.

Let's dissect each part in more detail. For convenience, I will include the addition (`+`) and deletion (`-`) symbols when referencing the diff lines.

### `-1,3`

The `-1,3` part of the hunk header indicates that starting from the first line of the original version of the file, there are 3 lines that are pertinent to understanding the changes that took place. These lines are:

1. `-This is the old first line` - This line was removed in the new version; hence, it is no longer part of the updated file. However, it is most certainly part of our old file, since it was present there.
    
2. `This is the second line` - This line is present in both versions of the file; therefore, it is a context line, providing information to understand the changes better. In the updated file, this has become the first line due to the removal of the old first line.
    
3. `-This is the old third line` - This line was removed in the new version of the file. However, this line was present in our old file.
    

Some things to note:

* `-This is the old first line` and `-This is the old third line` were both part of the original file but have been removed in the updated version. They are included in the diff output to demonstrate what content was deleted.
    
* The line `This is the second line`, while unchanged in both versions, is considered part of the 3 lines starting from the first line in the original version that help us understand what changes were made. This explains the `-1,3` in the hunk header.
    

In short, the original file, in this context, started from `-This is the old first line` and included 3 lines to fully understand the changes that took place:

* `-This is the old first line` (remember, the first line is inclusive)
    
* `This is the second line`,
    
* `-This is the old third line`
    

You may be asking, what isn't this line included: `+This is the new third line`? Well, that line is part of the new version, which our old version never even knew about! Again, if a line didn't exist in the old version of a file, there's no way for the changes to extend that far.

### `+1,3`

Looking at the new version of the file, the `+1,3` in the hunk header might seem confusing at first, because you might think it implies that three lines were added. However, remember that the numbers in the hunk header include both the changes and the context around them.

Here, `+1,3` tells us that from the first line of the new version of the file, there are 3 lines that are relevant to understand the changes that took place. These lines are:

1. `This is the second line` - This line is present in both versions of the file; therefore, it is a context line, providing information to understand the changes better. In the updated file, this has become the first line due to the removal of the old first line.
    
2. `-This is the old third line` - This line was removed in the new version.
    
3. `+This is the new third line` - This line was added in the new version.
    

Even though `This is the second line` was not changed, it is included in the hunk because it provides context for the change that did occur. And although `-This is the old third line` doesn't exist in the new version, it is included in the diff output to show that it was removed. So in total, there are 3 lines starting from the first line in the new version of the file that help us understand what changes were made.

Again, you may be asking, why does `-This is the old third line` appear within the scope of the new version's changes? That line was part of the old file, and it was removed in the new file, so why does it appear in the span of the new changes?

The reason `-This is the old third line` is shown in the span of the new changes is to convey the complete picture of the transformations that took place. It's included in the `git diff` output as it allows us to understand what exactly was removed to make way for the new content.

I cannot stress this enough: the goal of `git diff` is not only to show us the *new* lines but also to illustrate what lines were removed or replaced. This comprehensive view of alterations, both deletions and additions, is what enables us to fully comprehend the evolution from the old version to the new one.

So, in the new version, while it is true that `-This is the old third line` no longer exists, its appearance in the diff output is crucial to help us understand that it was a part of the original file but was subsequently removed in the new version. Context is key.

# Conclusion

And that's it. I hope you enjoyed this tutorial and were able to gain some insight into how `git diff` works. I encourage you to play around in your own Git repositories and experiment with `git diff` on your own. The more `git diff` outputs you read, the better you will become at comprehending its comparisons. Take care and happy coding!