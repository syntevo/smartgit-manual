# Preferences

## Commands

This page shows options which influence the executed Git (and Hg)
commands.

### Push

Only select the **Allow modifying pushed commits (e.g. forced-push)** if
you are sure you know Git well enough to understand the consequences of
forced push.

### Log

When **Contact gravatar.com to show images for the users** is selected,
a hash of the email address is generated and then gravatar.com is
contacted to request the belonging graphic which the user first has to
configure there.

## Git Executable

You can specify which **Git Executable** should be used by SmartGit.
Usually, the default executable which comes bundled with SmartGit is the
appropriate choice.


Version requirements


SmartGit is using the Git executable in many different ways, with
various options. Some of these options can easily be implemented as
"optional", others will be required for a decent implementation. Given
the required options, every new SmartGit version may come with an
increased *minimum required version* for the Git executable. Here is a
list of features witch are supported by SmartGit and partially
contribute to the minimum required version:

-   2.29: partial clone support
-   2.25: "--path-spec-from-file" support, for efficient operations on
    many files (affects all operations and thus is required!)
-   2.21: "--rebase-merges" option
-   2.14: stashing file selection



  

## Authentication

The **Authentication** page shows all credentials which SmartGit has
collected. Credentials can be selectively removed to force SmartGit
requesting new credentials when connecting to the repository again.

### SSH

SmartGit supports your system SSH client but also comes with a built-in
SSH client, in case you do not have an existing configuration.

-   **System SSH client**: if you already have a working SSH
    configuration, it's recommended to stick with this configuration,
    because this way changes to keys or passwords outside of SmartGit
    will automatically be picked up by SmartGit.



    When using the system SSH client, it's necessary that the system is
    configured in a way that it won't have to prompt for any additional
    user input on console: such prompts won't be processed by SmartGit
    and thus the Git operation will fail.



-   **Built-in SSH client**: if you do not have SSH set up and don't
    plan to have a shared SSH configuration between SmartGit and other
    tools, like Git command line client, using the built-in SSH client
    may be easier to configure and handle.



    The built-in SSH client requires the private key file in the
    PEM-format. Please see also the SSH-How-To.



### Password store

All passwords needed to access repositories can optionally be stored in
SmartGit's password store. This password store is located in the
`password` file, which can be found in SmartGit's settings directory
(see [Installation and Files](Installation-and-Files.md)).

The password store can be protected by a *master password*, and each
time you start SmartGit, this master password has to be entered as soon
as SmartGit tries to access the password store for the first time. The
entered password is kept in memory while the program runs, so you don't
have to enter it again for the rest of the current session. You may
choose **Don't use a master password** if you don't want the password
store to be protected with a master password. However, this option is
only recommended if you can make sure the master password file itself is
protected against unauthorized access.

### Master Password

By clicking on the **Change Master Password** button on the
**Authentication** page in the Preferences, you can set, reset or change
the master password. Use **Change master password** to *change* the
current password; this will preserve the stored passwords, but requires
that you supply the **Current Master Password**. Note that you don't
need to enter the **Current Master Password** if you are currently
working without a master password.

If you have forgotten the master password, select **Set new master
password**. In that case all previously stored passwords will be
discarded. Enter the **New Master Password** and **Retype New Master
Password**. When leaving both fields blank, you will continue to work
without a master password, i.e. as if having selected the option **Don't
use a master password** when you were asked to set the master password.

## Refresh

With **Detect Renames** enabled, SmartGit will detect renames of
*added*/*removed* and optionally also *untracked*/*missing* file pairs.

Select **Distinguish between content and EOL-only changes** to have
SmartGit distinguish between content and EOL-only changes, in addition
to the plain *modified* state which Git reports (in either case, files
which Git considers as *modified* will be reported). The EOL-change
information will be displayed in the **Files** table **State** If you do
not want Git to care about EOLs at all (that's usually the case if you
are on Windows), you might want to use
[core.autocrlf](.md)
or [text and eol
attributes](.md).

## Background Commands

Options on this page define what operations SmartGit might perform
automatically when it is in background.

### Local and Remote Changes

If **Detect Remote Changes** is selected, SmartGit will poll the remote
repositories of the favorite Git repositories in regular intervals for
changes. To avoid excessive overhead, by default only the lightweight
`git ls-remote` command is invoked, so you only get a *notification*
about changes. Note that this command will not notice if, for example,
the currently checked out feature branch has been merged and removed.

If **Fetch closed 'favorite' repositories** or **Fetch open repositories
when idle** is selected, SmartGit also will perform fetch-operations
which actually will *fetch* the changes from the remote repositories,
but can be more resource-hungry.

## Tools

On this page you can define external tools which can operate on certain
selections.



The **Tools** configuration is stored in `tools.xml` file in the
[settings directory](Installation-and-Files.md) and can handed over to
other developers/machines to share the tools configuration with them.



The *working directory* set when launching the tool will be the root
directory of the corresponding repository (which may also be a
submodule). When launching a tool on a set of files which belong to
different repositories, it will be the closest common directory of all
affected repositories.

When selecting **Can be used by the Open command**, SmartGit will
consider to use this tool when invoking **Open** (or **Open from Working
Tree**) in the **Files** view.

A couple of default tools are already preconfigured:

### Open File

This tool will invoke the system's default open command, e.g. to launch
a graphic viewer for `.png` files.

### Open in Terminal

This tool will open the selected directory in the terminal application.

### Format Patch

This tool will create a patch between either the selected commit/ref and
the working tree or the selected two commits/refs.

### Apply Patch

This tool will show a file open dialog and then apply the selected
patch.

### Fast-Forward Merge

This tool allows to right-click local, tracked branches in the
**Branches** view to perform a fast-forward merge for purely remote
changes, especially if you want to avoid having to check out the branch.

## Diff Tools



Diff tools are (GUI) viewers of diff results. Diff tools are not related
to the internal diff machinery of Git. If you want to customize the diff
machinery itself, have a look at [Git's 'diff.external' config
option](https://git-scm.com/docs/diff-config).



SmartGit comes with a built-in diff tool which will be used by default
to display results of various operations, like **Show Changes**. If you
prefer, you can configure external diff tools which should be used
instead. Following notable **Arguments** are available for an **External
diff tool**:

-   `${leftFile}` represents the left (or usually "old") version of the
    file
-   `${rightFile}` represents the right (or usually "new") version of
    the file
-   `${baseFile`} represents the *common base* version of the file;
    depending on the context from which the diff tool is invoked,
    the *common base* is defined as:
    -   the Index version, for a working-tree vs. Index comparison
    -   the HEAD version, for a working-tree vs. HEAD or a Index vs.
        HEAD comparison
    -   the version before the commit, for a single-commit comparison
    -   the *merge base* version of the two commits, when comparing two
        commits; this is similar to what SmartGit would display in case
        of conflicts

For certain files, like graphic files, there may be no appropriate
left-right diff tool available. In this case you can configure an
**External viewer** which has only a single **${file}** argument and
which will be launched by SmartGit two times: once for the old and once
for the new file.

By default, the list of tools is searched from top to bottom and the
first matching tool will be used. Alternatively, you can select **Show
chooser dialog if multiple entries match**.

## Conflict Solvers

SmartGit comes with a built-in conflict solver (three-way-merge) which
will be used by default when invoking **Query\|Conflict Solver**. If you
prefer, you can configure external three-way-merge tools which should be
used instead. Following notable **Arguments** are available for
an **External Conflict Solver**:

-   `${mergeFile}` represents the resulting conflicting file, as it's
    present in the working tree
-   `${leftFile}` represents Git's "ours" version of the file
-   `${rightFile}` represents Git's "theirs" version of the file
-   `${baseFile}` represents Git's "common" version of the file

For details on how Git is managing these file versions, refer to the
[Git manual](https://git-scm.com/book/en/v2/Git-Tools-Advanced-Merging).



**Example: Unity Smart Merge Tool**



To configure the UnityYAMLMerge, use
`merge -p ${baseFile} ${rightFile} ${leftFile} ${mergedFile}` as
**Arguments**. An example configuration could look like:

  
![](attachments/53215433/53215434.png)

Note, that a compatible 3-way merge tool has to be configured as
"fallback" for the Unity merge tool in order to complete the merge if
the automatic merging is not successful.



## Proxy

On this page you can configure the proxy to be used by SmartGit. The
proxy will be used exactly as configured for the check for new versions.
It will also be used for Git HTTP and HTTPs under certain conditions:

-   **Use following proxy** has been selected and
-   there is no `http.proxy` and no `https.proxy` configuration found in
    your `.gitconfig` and Git's system `config` and
-   the neither of following system environment variables is
    set: `HTTP_PROXY`, `HTTPS_PROXY`, `NO_PROXY`

  


