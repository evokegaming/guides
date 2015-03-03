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
* Subject kept as-is, even if it exceeds 50 characters
* Avoid merge conflicts whenever possible
* Conflicts section kept as-is
* Avoid fast-forwards (use option `--no-ff` for `git merge`)
* Lists all commits merged (use option `--log=<n>` for `git merge` or set the
  `merge.log` config option)
* No issue reference

### Reversion commits
* Subject kept as-is, even if it exceeds 50 characters
* Line saying which commit is reverted kept as-is
* Body explains reason for the reversion
* Include issue reference

## Commit contents
* Each commit introduces one logical change
    * Changes not described in the commit message do not belong in the commit
    * Multiple unrelated changes are not to be grouped in the same commit

* One logical change is not to be split over many commits
    * Squash changes that do not make sense on their own
    * Squash review fixes into the appropriate commit(s)

Do not combine changes of different type into the same commit. Examples of
change types are:

* Functionality changes
* Refactorings
* Whitespace fixes
* Typo fixes
* Additions of third-party code

Furthermore:

* Commits do not introduce whitespace errors
* Line-endings are LF (third-party files exempt)
* Single newline at EOF
* Commits adding third-party code must state from where the code was acquired as
  well as its version, and must add the third-party code as-is. Changes to
  third-party should be thought of as a last resort and belong in a separate
  commit.

## Branches
* Branch `master` contains the latest stable release
* Integration branches are namespaced `integration/`
* Individual words in branch names are separated by hyphen-minus (`-`)

### Personal branches
* Personal branches are namespaced `<name>/` where `<name>` is your firstname or
  known nickname.
* It is a good idea to push even unfinished work to your personal namespace
  frequently.
* Do not base work on someone else's personal branch unless explicitly told to
  do so.
* Personal branches that are sent for deploy follow the form
  `<name>/<issue>-<short-description>`, e.g. `charlemagne/123-promote-the-arts`.

## Workflow
* Do not rewrite another person's commit without permission
    * Makes it difficult to know whether or not that commit reflects what the
      original author wrote.

* Rebase personal branches before requesting merge
    * Makes it possible to spot potential problems early
    * Reduces risk of merge complications

* Do not work in isolation for long periods of time
    * Makes it hard to spot potential problems early
    * Increases risk of merge complications

* Do not create merge commits in personal branches
    * Makes the history unnecessarily complex
    * Personal branches should be simple and short-lived

* Delete remote branch when it has been merged
    * Superfluous entropy serves no purpose

## Tags
* All tags are annotated (`-a`)
* Individual words in tags are separated by hyphen-minus (`-`)
