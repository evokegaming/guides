# Merge requests
## Introduction
Since introducing GitLab to our workflow, we've been using it as our primary
tool for reviewing code.  
While issues still reside in Redmine and the actual merging/deploying is handled
outside of GitLab, commenting and discussing code is handled through GitLab.

While there has been discussion about altering the workflow to utilize GitLab's
various features more extensively, for now you should only consider it being
something that rests on top of your everyday work in Redmine.

## Prerequisite
The following instructions assume you have a working GitLab account. If you
don't yet have access to the web interface of GitLab, contact Operations.

## Lifecycle of a merge request
### Creation procedure
Let's assume you've worked on an issue enough to set the status to "Feedback",
which would put it into the feedback queue.

Before you set the status to "Feedback", make sure that there's a Merge Request
(henceforth referred to as "MR") available in GitLab.

These are the steps necessary:

1. Log in to GitLab.
2. When logged in, you should arrive at the Dashboard (as written in the top
   left). Click "Projects" in the menu to the left.
3. Choose the project which you wish to submit a MR for. Most of the time, this
   will be "sparkle".
4. Click "Merge Requests" in the menu to the left.
5. Click the green "New Merge Request" button, located in the top right part of
   the page.
6. Here you'll need to provide the **source** branch and the **target** branch.
   The source branch should be the branch which you wish to have reviewed, while
   the target branch should be what you want to merge into.  
   Unless you've been asked to set it to something else, you'll want the target
   branch to be set to `master`.
7. Press the blue "Compare branches" button.
8. Enter title and description, following the guidelines written in the
   **Title and Description guidelines** section.  
   Set no label, this will be done by someone else.  
   Set no assignee, unless you've been asked to.
9. At this point you can review the MR one final time before pressing the
   "Submit merge request" button.

This should take you to the page with the now created MR.  
All that is necessary from here is to post a link to the MR over in the
corresponding Redmine issue.

#### Title and Description guidelines
The title should always look something like this:

    #12345: Adjust constant in warpcore configuration to prevent overheating

Notice the issue number at the start, including the `#` character. This text
will automatically be linked to the Redmine issue, which makes it easy to access
the discussion in Redmine.  
Following the issue number should be general description of what the MR changes.

The description should include the following:

* What the MR changes, being a bit more descriptive than the title.
* If there's something you're unsure of how it should be done, point this out so
  that the reviewer can help you out.
* If necessary, include information that would help the reviewer understand
  *why* you made certain changes. This information should already be present in
  the form of [well written commit messages](../../style/git), but it doesn't
  hurt including this in the MR description.
* If there are parts of the changeset that can be ignored (you added a library
  to vendor, resulting in thousands of lines of code), you should point this out
  so the reviewer doesn't waste time sifting through vendor code.
* If the merge will require some additional deploy instructions, include these
  so that they too can be reviewed.

### Working with merge requests
#### Git
The process of working and commiting when working in conjunction with MRs
doesn't differ much from how work has been handled when we only used Redmine,
but there's one major difference:

Due to how GitLab handles comments on specific commits/lines, it's advised to
use fixup commits that are squashed by the end of the life cycle of the MR.

What does this mean?

Let's say you've submitted your MR and the reviewer wants you to change
something in a previous commit. If you previously had the habit of creating
a commit, rebasing and squashing right away: refrain from doing so. The
preferred way of doing things in this system [is to create fixup commits][fixup]
that are squashed at the very end of the life cycle of the MR, so pretty much
when the reviewer approves the MR for merging.

  [fixup]: http://fle.github.io/git-tip-keep-your-branch-clean-with-fixup-and-autosquash.html

Basically, prefer not to rewrite history, until the MR is about to be merged.

If there's no need to rewrite history, for instance if you start adding new
features after the MR is submitted, just keep working as you always have.

#### The merge request interface
If you make an adjustment (e.g. to address a comment that was left by
a reviewer) and push the resulting code, this will immediately be reflected in
the MR. This way, the reviewer will always see the latest version of the branch
in the MR.

Additionally, commenting on specific lines in the code is very easy, so feel
free to use the feature whenever applicable. To comment on a line, press the
"Changes" tab near the top of the MR, then press a line number in one of the
changed files to open up a comment box.

Everywhere where you can leave a comment, [GitLab flavoured Markdown][1] is
used. While it's helpful if you glance through the entire document, if you only
take away one thing from it, it should be that you can turn any text into
monospaced by surrounding it with backticks (or grave accents, as they're also
called: `` ` ``). If you're talking about `SomeClass.with.some_method`, things
become easier to if you surround it with backticks.

  [1]: http://doc.gitlab.com/ce/markdown/markdown.html

### Closing a merge request
When the reviewer considers the code ready to merge, he'll say so, and a label
with the name `ready-to-merge` will be applied, indicating that the branch is
ready to merge and deploy.
Don't worry about applying the label, one of the developers with merge/admin
rights will handle it.

Once it has been said in the MR that it's ready for merging, write the same in
Redmine (if the reviewer hasn't done it already).

Do not close the MR, it will be closed automatically when the branch is merged
into `master` (or whatever other branch was set as the target branch).

## Finding merge requests
As stated at the end of the "Creation procedure" section, if an issue in
Redmine has code that should be reviewed, a link to the corresponding MR in
GitLab *must* be posted. This link will probably be the easiest way to find the
MR in GitLab.

You can also go to GitLab and simply search for the issue number (`#` character
not necessary) in the search bar located in the top right.
