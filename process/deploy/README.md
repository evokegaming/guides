# Deploy process
## Preparation
1. Create a branch called `deploy-yyyy-mm-dd.n` from latest `master` where
   `yyyy-mm-dd` is the date of the deploy and `.n` is the deploy number for that
   day starting at 1.

2. No-ff merge each branch into the deploy branch, be sure to include the commit
   log in the merge commit message and be sure that the branch is sane by
   checking that it has been reviewed and has proper commits. Merge directly
   from the remote tracking branch (`origin/...`) to avoid human error and to
   follow convention.

   If there is a merge conflict or potential semantic conflict, get the author
   to rebase on lastest `master`. If there is still a merge conflict or semantic
   conflict, see if conflicting branch can be moved to next deploy opportunity.
   If conflict resolution must happen, handle with care. Leave the conflicts
   section of the commit message as-is.

3. Run all tests.

4. If tests fail, start over and merge all branches except the bad one.

## At time of deploy
1. No-ff merge deploy branch to `master`. Merge from local branch, not from
   remote tracking branch (i.e. merge `deploy-yyyy-mm-dd.n`, not `origin/dito`)

2. Push `master`.

3. Tag the new `master` with an annotated tag bearing the same name as the
   deploy branch (i.e. `deploy-yyyy-mm-dd.n`. Always tag the commit that merges
   the deploy branch into master. Describe the deploy in the tag annotation
   according to below. Tag the deploy as soon as master has been pushed to
   capture the approximate time of the push in the tag timestamp.

4. Delete the deploy branch.

## Tag annotation
See existing tags for reference.

* **Regular deploy** is a planned regular deploy.
  Annotation is simply "Regular deploy", no further explanation is needed.

* **Extra deploy** is a planned extra deploy that has been known beforehand,
  such as a game release or content update. Annotation has subject
  "Extra deploy" and explains the reason for the extra deploy in the body.

* **Emergency deploy** is when things are burning and things need to be fixed
  fast. Annotation has subject line "Emergency deploy" and explains the reason
  for the emergency deploy in the body.

# Cheat sheet
## Create branch from latest master
    $ git fetch
    $ git checkout -b deploy-yyyy-mm-dd.n origin/master

## Proper merge of topic branch
    $ git merge --no-ff --log=2000 origin/foo/123-topic

Log option can be set in config to avoid having to `--log=2000` (the number 2000
is to have something high enough to include all commits merged):

    $ git config --global merge.log 2000

## Proper merge of deploy branch
    $ git merge --no-ff --log=2000 deploy-yyyy-mm-dd.n

## Delete a branch
    $ git branch -d deploy-yyyy-mm-dd.n

If you happen to have pushed the deploy branch, also delete from remote:

    $ git push origin :deploy-yyyy-mm-dd.n

## Create and push a tag
    $ git tag -a deploy-yyyy-mm-dd.n
    <write proper tag annotation, save and close>
    $ git push origin refs/tags/deploy-yyyy-mm-dd.n

## Push all tags
To make all local tags reflect what is on the remote, delete all local tags and
fetch all tags anew from the remote:

    $ git tag | xargs git tag -d && git fetch --tags

If there are no local tags that are not to exist on the remote (e.g. stale tags
since the early days), pushing all tags may be easier than pushing a single one:

    $ git push --tags

## Delete remote tag

If an erroneous tag was pushed, remove it as fast as possible before others have
fetched it, as they will not get the updated one unless they explicitly `git
fetch --tags`.

    $ git push origin :refs/tags/deploy-yyyy-mm-dd.n

Be sure to remove the local tag as well.

    $ git tag -d deploy-yyyy-mm-dd.n

### Tag annotation examples
    $ git fetch --tags
    $ git tag | grep deploy- | xargs git show --format=""

# Emergencies
**Keep calm and take your time**. Even if things are burning, spending a few
minutes extra to do things properly is worth it. Make haste slowly to avoid
stupid mistakes.

Emergency deploys are to be handled **just as any other deploy**. This means
create a deploy branch, and follow the above procedure. Do not forget to
increment the deploy number (`.n` of `deploy-yyyy-mm-dd.n`), as this is a new
deploy.

This also applies to the integration master's own branches. Have them reviewed
and merge from `origin/foo/123-topic`, not from the local branch.

The only part that may be postponed (but not skipped) until after the deploy is
the test suite, but only if the deploy needs to happen so fast that there really
is no time to run it.

## Reverting the changes from a whole topic branch
When reverting a merge commit where a topic branch has been merged into a deploy
branch, no topic branch should be created. In this case, revert the merge
directly on the new deploy branch using:

    $ git revert -m 1 54492a6  # 54492a6 is the merge commit for the bad branch

Reversion commits have issue reference `#8358` and an explanation of why the
reversion is needed. Leave the information that Git adds as-is and add the
explanation and issue reference beneath it.

Side note: When a merge commit is reverted, this does not mean history is
changed. As far as Git is concerned, nothing changes the fact that the branch
has been merged. The best way to add the same changes (with fix) again is to ask
the developer to create a new branch based on the latest `master` that contains
both the changes that were reverted and the fix, just as if it were any other
topic branch. Reversions of reversions are possible but ugly so let's avoid
them.
