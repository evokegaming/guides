# Git style guide

## Commit message
### Content
The commit message serves to provide a technical description of the change
introduced by its commit. It should be possible to look at a commit message in
combination with its diff and quickly understand **what** was changed **where**,
**how** it was changed and **why**. Simple changes sometimes speak for
themselves. Complex changes need more explanation.

* Commits are a form of documentation
* Time spent writing costs less than the time spent reading

A commit message contains a **subject**, a **body**, and an **issue reference**.
The body may be omitted if the subject on its own describes the whole change.

#### Subject
The subject line of a commit message gives a short description of the change. It
explains **what** it changes and with few exceptions **where** the change is.

It uses the imperative mood:

* Use e.g. `Add`, `Fix`, `Change`, `Move`, `Improve`, `Refactor`
* Avoid e.g. `Adds`, `Added`, `Adding`

Prefixes for conceptually isolated subsystems may be used, such as `API: `

#### Body
The commit message body describes the current state (**why**):

* Use present tense, e.g. "Foo does not work"
* Avoid past tense, e.g. "Foo did not work"

It describes what the the commit does to change the current state (**what**):

* Match the subject and use imperative mood "Let's fix foo by doing bar"
* Or use a descriptive tone "This fixes foo by doing bar"

When including URLs, avoid using URL shortener services and use full original
URLs.

#### Issue reference
All commits except merge commits have an issue reference. In most cases there
will be only one referenced issue. In case the change touches multiple issues,
they may be added in numerical order separated by space like `refs #123 #456`.
If there is more than one issue reference, consider whether breaking the commit
into several makes more sense.

### Style
    Subject line has at most 50 characters

    A blank line separates the subject line and the body. The body is
    wrapped at 72 characters, no more and no less. This means that no line
    may be longer than 72 characters and no line is wrapped at less than 72
    characters.

    Separate your commit message into paragraphs if you have a lot of text
    in order to improve readability.

    - This is a bullet list
    - It uses hyphen-dashes and it is not indented
      - If you need to nest lists, indent with two spaces
      - Nesting sometimes impairs readability so use with caution
    - Use it for lists with more than one item

    Here is another list:

    - If an item in a bullet list has so much text that it exceeds 72
      characters, wrap it and indent like this.

    - When there are multi-line items in a list, add a blank line
      between items to improve readability.

    Here is an long URL, on its own line and not wrapped:
    http://example.com/my-very-long-url-that-is-not-wrapped-even-if-it-exceeds-72-characters/

    refs #123

### Merge commits
1. Subject kept as-is, even if it exceeds 50 characters
2. Avoid merge conflicts whenever possible
3. Keep the "Conflicts:" listing by uncommenting it
4. Avoid fast-forwards (use option `--no-ff` for `git merge`)
5. Lists all commits merged (use option `--log=<n>` for `git merge` or set the
   `merge.log` config option)
6. No issue reference

### Reversion commits
1. Subject kept as-is, even if it exceeds 50 characters
2. Line saying which commit is reverted kept as-is
3. Body explains reason for the reversion
4. Include issue reference

## Commit contents
1. Each commit introduces one logical change
    1. Changes not described in the commit message do not belong in the commit
    2. Multiple unrelated changes are not to be grouped in the same commit

2. One logical change is not to be split over many commits
    1. Squash changes that do not make sense on their own
    2. Squash review fixes into the appropriate commit(s)

Do not combine changes of different type into the same commit. Examples of
change types are:

* Functionality changes
* Refactorings
* Whitespace fixes
* Typo fixes
* Additions of third-party code

Furthermore:

1. Commits do not introduce whitespace errors
2. Line-endings are LF (third-party files exempt)
3. Single newline at EOF
4. Commits adding third-party code must state from where the code was acquired
   as well as its version, and must add the third-party code as-is. Changes to
   third-party should be thought of as a last resort and belong in a separate
   commit.

## Branches
1. Branch `master` contains the latest stable release
2. Integration branches are namespaced `integration/`
3. Individual words in branch names are separated by hyphen-minus (`-`)

### Personal branches
1. Personal branches are namespaced `<name>/` where `<name>` is your firstname
   or known nickname.
2. It is a good idea to push even unfinished work to your personal namespace
   frequently.
3. Do not base work on someone else's personal branch unless explicitly told to
   do so.
4. Personal branches that are sent for deploy follow the form
   `<name>/<issue>-<short-description>`, e.g.
   `charlemagne/123-promote-the-arts`.

## Workflow
1. Do not rewrite another person's commit without permission
    1. Makes it difficult to know whether or not that commit reflects what the
       original author wrote.

2. Rebase personal branches before requesting merge
    1. Makes it possible to spot potential problems early
    2. Reduces risk of merge complications

3. Do not work in isolation for long periods of time
    1. Makes it hard to spot potential problems early
    2. Increases risk of merge complications

4. Do not create merge commits in personal branches
    1. Makes the history unnecessarily complex
    2. Personal branches should be simple and short-lived

5. Delete remote branch when it has been merged
    1. Superfluous entropy serves no purpose

## Tags
1. All tags are annotated (`-a`)
2. Individual words in tags are separated by hyphen-minus (`-`)
