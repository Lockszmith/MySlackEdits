# Understanding `git` Conflict Resolution by Example

Below is an attempt to document a complete transcript of `git` operations with the purpose to demonstrate how _branching_, _merging_ and _rebasing_ work,
with a focus on what is `--theirs` and `--ours` in different situations.

> Sequences start with ### and the commit title. It will contain all of
> the commands included in the commit.

\* **NOTE**: On a new machine, start with [Git Initial configuraiton](#git-initial-configuraiton) below

```bash
### Initial commit
> git init
> code README.md
#>>> Create this README.md file with all of its content
> git add README.md
> code source-code.src
#>>> Add initial source-code.src lines
```

<details><summary> <code>source-code.src</code> content <small><sup>-🖱️</sup><sub> click to expand</sub></small></summary>

```txt
# some comment
# another comment

################################
commit 01 -- HEAD on main
commit 02
commit 03
commit 04
commit 05
commit 06
commit 07
commit 08
commit 09
commit 10
commit 11
commit 12
```

</details>

```bash
#>>> Set commit HEAD indicator to commit 1
> git add source-code.src
> git commit -m "Initial commit (1)"
[main (root-commit) _______] Initial commit
 2 files changed, 93 insertions(+)
 create mode 100644 README.md
 create mode 100644 source-code.src

### Adding FeatureA
> git switch -c "FeatureA"
Switched to a new branch 'FeatureA'

> code source-code.src
#>>> Add First feature A into source-code
#>>> Move commit HEAD indicator to commit 2
```

<details><summary> <code>source-code.src</code> content <small><sup>-🖱️</sup><sub> click to expand</sub></small></summary>

```txt
# some comment
# another comment

Source code line 1 for Feature A
Source code line 2 for Feature A
Source code line 3 for Feature A
Source code line 3 for Feature A with a typo
Source code line 5 for Feature A

################################
commit 01 -- FROM
commit 02 -- HEAD on FeatureA
commit 03
commit 04
commit 05
commit 06
commit 07
commit 08
commit 09
commit 10
commit 11
commit 12
```

</details>

```bash
> git add source-code.src
> git commit -m "Added FeatureA (with a typo) (2)"
[FeatureA _______] Added FeatureA (with a typo) (2)
 1 file changed, 8 insertions(+), 2 deletions(-)

> git switch main
Switched to branch 'main'

> git merge FeatureA --ff-only
Updating _______.._______
Fast-forward
 source-code.src | 10 ++++++++--
 1 file changed, 8 insertions(+), 2 deletions(-)

# Show list of changes in interactive tool
> git difftool main FeatureA
# difftool Keyboard cheat-sheet:
#  ],c - Next difference                         # Folding:
#  [,c - Prev difference                         #   z,o/O - Unfold (Open)
#                                                #   z,c/C - Fold   (Close)
#  d,o - Diff Obtain (pull from other)           #   z,a/A - Toggle
#  d,p - Diff Put    (push  to  other)           #   z,v   - Open folds for this line
#                                                #   z,M   - Fold All
#  :diffupdate - Re-scan files for differences   #   z,R   - UnFold All
#                                                #   z,m   - Fold more (foldlevel += 1)
#  Z,Q - Quit pane (discard changes)             #   z,r   - Fold less (foldlevel -= 1)
#  :qa - Quit all  (discard changes)             #   z,x   - Update folds
#
# Show list of changes (non-interactive)
> git diff main FeatureA --name-status
> code source-code.src
#>>> Move commit HEAD indicator to commit 3
```

<details><summary> <code>source-code.src</code> content <small><sup>-🖱️</sup><sub> click to expand</sub></small></summary>

```txt
# some comment
# another comment

Source code line 1 for Feature A
Source code line 2 for Feature A
Source code line 3 for Feature A
Source code line 3 for Feature A with a typo
Source code line 5 for Feature A

################################
commit 01
commit 02 -- FROM
commit 03 -- HEAD
commit 04
commit 05
commit 06
commit 07
commit 08
commit 09
commit 10
commit 11
commit 12
```

</details>

```bash
> git add source-code.src
> git commit -m "Additional commit after merging FeatureA into main branch (3)"
[main _______] Additional commit after merging FeatureA into main branch (3)
 1 file changed, 2 insertions(+), 2 deletions(-)

### Add FeatureB
> git switch -c FeatureB
Switched to a new branch 'FeatureB'
> code source-code.src
#>>> Add Feature B into source-code
#>>> Move commit HEAD indicator to commit 4
```

<details><summary> <code>source-code.src</code> content <small><sup>-🖱️</sup><sub> click to expand</sub></small></summary>

```txt
# some comment
# another comment

Source code line 1 for Feature A
Source code line 2 for Feature A
Source code line 3 for Feature A
Source code line 3 for Feature A with a typo
Source code line 5 for Feature A

Source code line 1 for Feature B
Source code line 2 for Feature B
Source code line 3 for Feature B
Source code line 3 for Feature B
Source code line 5 for Feature B

################################
commit 01 -- HEAD
commit 02
commit 03
commit 04
commit 05
commit 06
commit 07
commit 08
commit 09
commit 10
commit 11
commit 12
```

</details>

```bash
> git add source-code.src
> git commit -m "Added FeatureB (4)"
[FeatureB _______] Added FeatureB (4)
 1 file changed, 8 insertions(+), 2 deletion(-)

> code source-code.src
#>>> Move commit HEAD indicator to commit 5
> git add source-code.src
> git commit -m "Additional commit in FeatureB (5)"
[FeatureB _______] Additional commit in FeatureB (5)
 1 file changed, 2 insertions(+), 2 deletions(-)

### Add FeatureC (simulating parallel work to FeatureB)
> git switch main
> git branch -c FeatureC
Switched to a new branch 'FeatureC'

> code source-code.src
#>>> Fix typo in source-code.src (FeatureA)
#>>> Move commit HEAD indicator to commit 6
> git add source-code.src
> git commit -m "FeatureA code fixed (6)"
[FeatureC _______] FeatureA code fixed
 1 file changed, 3 insertions(+), 3 deletions(-)

> code source-code.src
#>>> Move commit HEAD indicator to commit 7
> git add source-code.src
> git commit -m "Additional commit in FeatureC (7)"
[FeatureC _______] Additional commit in FeatureC (7)
 1 file changed, 2 insertions(+), 2 deletions(-)
```

## Before we continue

The `git log` command allows to explore the tree, the switches below
draws the graph of the commit tree with relevant branches, tags and HEAD.

Because we're going to explore merge and rebase in the next steps, we'll
'save out spot' using a tag first, then display the tree using `git log`.

```bash-console
> git switch main
Switched to branch 'main'
> git tag before-merge # will be used later to reset to this position
> git log  --abbrev-commit --decorate --graph '--format=%>|(12)%C(yellow)%h%C(reset) %C(blue)[%><(14)%ar] %C(reset)%C(white)%s%<(1,trunc)% b%-(trailers:key=x-always-empty)%C(reset) %C(dim white)- %an%C(reset) %C(auto)%d%C(reset) ' --all
```

```
*    _______ [___ ago] Additional commit in FeatureC (7)   - <Author Name>  (HEAD -> main, tag: before-merge)
*    _______ [___ ago] FeatureA code fixed (6)   - <Author Name>
| *  _______ [___ ago] Additional commit in FeatureB (5)   - <Author Name>  (FeatureB)
| *  _______ [___ ago] Added FeatureB (4)   - <Author Name>
|/
*    _______ [___ ago] Additional commit after merging FeatureA into main branch (3)   - <Author Name>  (FeatureC)
*    _______ [___ ago] Added FeatureA (with a typo) (2)   - <Author Name>  (FeatureA)
*    _______ [___ ago] Initial commit (1)   - <Author Name>
```

## merge

When doing a `merge`, the following sequence will happen:

```bash-console
> git switch main
Already on 'main'

# If not "Already on 'main'", or you want to repeat this, you can do:
> git reset --hard before-merge
HEAD is now at _______ Additional commit in FeatureC (7)

> git merge FeatureB --ff-only
hint: Diverging branches can't be fast-forwarded, you need to either:
hint:
hint:   git merge --no-ff
hint:
hint: or:
hint:
hint:   git rebase
hint:
hint: Disable this message with "git config advice.diverging false"
fatal: Not possible to fast-forward, aborting.

> git merge FeatureB --no-ff --no-rerere
# Changes caused by the relevant merge strategies will be staged
# and a commit initiated.
# You will be prompted with a commit message and exiting the
# editor with ('ZZ' or ':wq' in vim) or without (`ZQ` or `:q`)
# would commit the changes.
#
# Exiting by cancelling (':cq!' in vim), or deleting all
# lines before saving will allow you to unstage/edit files
# before committing.

# Output below is in case of cancelling the commit:
Auto-merging source-code.src
error: there was a problem with the editor 'vi'
Not committing merge; use 'git commit' to complete the merge.

# To see all staged changes, run:
> git diff --staged                             
diff --git a/source-code.src b/source-code.src
index _______.._______ 100644
--- a/source-code.src
+++ b/source-code.src
@@ -7,11 +7,17 @@ Source code line 3 for Feature A
 Source code line 4 for Feature A (typo fixed)
 Source code line 5 for Feature A

+Source code line 1 for Feature B
+Source code line 2 for Feature B
+Source code line 3 for Feature B
+Source code line 3 for Feature B
+Source code line 5 for Feature B
+
 commit 01
 commit 02         (FeatureA)
 commit 03
 commit 04
-commit 05
+commit 05 -- HEAD (FeatureB)
 commit 06
 commit 07 -- HEAD (FeatureC)
 commit 08

# When ready to continue commit if you use `--no-edit`, the default
# merge message will be used, otherwise you'll be prompted to edit
# the commit message (or you can use `--message/-m`)
> git commit --no-edit
[main _______] Merge branch 'FeatureB'
# This is in effect commit 8, next commit will be 9

> code source-code.src
#>>> Move commit HEAD indicator to commit 9, indicate 8 as 'merged'
> git add source-code.src
> git commit -m "Commit on main after mreging FeatureB (9)"
[FeatureC _______] Commit on main after mreging FeatureB (9)
 1 file changed, 3 insertions(+), 3 deletions(-)

> git log  "--abbrev-commit" "--decorate" "--graph" "--format=%>|(12)%C(yellow)%h%C(reset) %C(blue)[%><(14)%ar] %C(reset)%C(white)%s%<(1,trunc)% b%-(trailers:key=x-always-empty)%C(reset) %C(dim white)- %an%C(reset) %C(auto)%d%C(reset) " "--all"
*    _______ [___ ago] Commit on main after mreging FeatureB (9)   - <Author Name>  (HEAD -> main)
*    _______ [___ ago] Merge branch 'FeatureB' ..   - <Author Name>
|\
| *  _______ [___ ago] Additional commit in FeatureB (5)   - <Author Name>  (FeatureB)
| *  _______ [___ ago] Added FeatureB (4)   - <Author Name>
* |  _______ [___ ago] Additional commit in FeatureC (7)   - <Author Name>  (tag: before-merge)
* |  _______ [___ ago] FeatureA code fixed (6)   - <Author Name>
|/
*    _______ [___ ago] Additional commit after merging FeatureA into main branch (3)   - <Author Name>  (FeatureC)
*    _______ [___ ago] Added FeatureA (with a typo) (2)   - <Author Name>  (FeatureA)
*    _______ [___ ago] Initial commit (1)   - <Author Name>
```

## rebase

Same scenario , this time, rebasing `FeatureB` onto `main`, and merging
into main branch at the end.

First reset the state before the merge:

```console
> git switch FeatureB
Switched to branch 'FeatureB'

# Next we'll reset 'main' back to 'before-merge' tag
> git branch -f main before-merge

> git rebase main
Auto-merging source-code.src
CONFLICT (content): Merge conflict in source-code.src
error: could not apply _______... Added FeatureB (4)
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
hint: Disable this message with "git config advice.mergeConflict false"
Recorded preimage for 'source-code.src'
Could not apply _______... Added FeatureB (4)

# The base is: commit 3    # SHA is     : cat .\.git\rebase-merge\stopped-sha
#                          # Content is : git cat-file -p "$(git rev-parse ":1:source-code.src")"
#                          #         or : git show "$(cat .\.git\rebase-merge\stopped-sha)~1:source-code.src"
git cat-file -p $(git rev-parse "$(git rev-parse "$(cat .\.git\rebase-merge\stopped-sha)~1"):source-code.src")
# --ours (HEAD) = commit 7 # git show REBASE_HEAD:source-code.src
#                          # HEAD here is from main
#                          # git show "HEAD:source-code.src"
# --theirs      = commit 4 # git show "$(cat .\.git\rebase-merge\stopped-sha)~1:source-code.src"

# Fix merge conflict on README.md option 1:
> git mergetool
# Fix merge conflich on README.md option 2 (because I already know this is safe):
> git checkout --ours -- README.md
> git commit --no-edit # --no-edit uses the default merge commit message
Recorded resolution for 'README.md'.
[main _______] Merge branch 'FeatureB'
# This is in effect commit 8, next commit will be 9
```

```bash-console
> git merge FeatureC
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Auto-merging source-code.src
CONFLICT (content): Merge conflict in source-code.src
Recorded preimage for 'README.md'
Recorded preimage for 'source-code.src'
Automatic merge failed; fix conflicts and then commit the result.

### Base/HEAD: commit 5 | Theirs: commit 7 | Ours: commit 5
### for git checkout --ours/--theirs -- README.md
### --ours   = Current branch state (main/FeatureB)
### --theirs = New branch being applied (FeatureC)

> git merge --abort
> git rebase --onto main main FeatureC --interactive # or > git switch FeatureC && git rebase main --interactive
pick _______ FeatureA code fixed
pick _______ Updated README on after code fix in FeatureC branche from main

Auto-merging README.md
CONFLICT (content): Merge conflict in source-code.src
error: could not apply _______... FeatureA code fixed
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
hint: Disable this message with "git config advice.mergeConflict false"
Recorded preimage for 'source-code.src'
Could not apply _______... FeatureA code fixed

> git status
interactive rebase in progress; onto _______ (GSz: commit 5)
Last command done (1 command done):
   pick _______ FeatureA code fixed (GSz: commit 6)
Next command to do (1 remaining command):
   pick _______ Updated README on after code fix in FeatureC branche from main (GSz: commit 7)
  (use "git rebase --edit-todo" to view and edit)
You are currently rebasing branch 'FeatureC' on '_______' (GSz: main).
  (fix conflicts and then run "git rebase --continue")
  (use "git rebase --skip" to skip this patch)
  (use "git rebase --abort" to check out the original branch)

Unmerged paths:
  (use "git restore --staged <file>..." to unstage)
  (use "git add <file>..." to mark resolution)
        both modified:   source-code.src

no changes added to commit (use "git add" and/or "git commit -a")

### Base: git rev-parse HEAD~1

### for git checkout "$(cat .\.git\rebase-merge\stopped-sha)~1"/--ours/--theirs -- <path-spec>
### Base: commit 2.5 | Theirs: commit 6 (FeatureC) | Ours: commit 4 (main/FeatureB)
### --ours   = Base branch state (main/FeatureB)
### --theirs = New branch being applied (FeatureC)
###
> 
```

## Git Initial configuraiton

```conosle
> git config --edit
```

Add the following content:

```ini
[init]
        defaultBranch = main
#[core]
#        excludesfile = <Path to my personal global .gitignore>
#[http]
#        sslBackend = schannel # On Windows - using this can solve issues
#[user]
#        name = Your Name
#        email = name@domain.tld
#[credential "helperselector"]
#        selected = manager # Recommended on Windows machines
#[credential "https://code.lksz.me"]
#        provider = generic
#[pull]
#        rebase = false
#[fetch]
#        prune = false
#[rebase]
#       autoStash = false
#       autosquash = true
#       updateRefs = false
#[rerere]
#        enabled = 1
#[diff]
#        guitool = vscode
#        tool = vimdiff
#[difftool "vimdiff"]
#        path = ~/scoop/apps/git/current/usr/bin/vimdiff.exe
#[difftool "vscode"]
#        path = C:/Program Files/Microsoft VS Code/Code.exe
#        cmd = \"C:/Program Files/Microsoft VS Code/Code.exe\" --new-window --wait --diff \"$LOCAL\" \"$REMOTE\"
#[merge]
#        guitool = vscode
#[mergetool "vimdiff"]
#        path = ~/scoop/apps/git/current/usr/bin/vimdiff.exe
#[mergetool "vscode"]
#        path = C:/Program Files/Microsoft VS Code/Code.exe
#        cmd = \"C:/Program Files/Microsoft VS Code/Code.exe\" --new-window --wait --merge \"$REMOTE\" \"$LOCAL\" \"$BASE\" \"$MERGED\"
```

<details> Remove this when done with the edit
<details><summary> <code>source-code.src</code> content <small><sup>-🖱️</sup><sub> click to expand</sub></small></summary>

```txt
# some comment
# another comment

Source code line 1 for Feature A
Source code line 2 for Feature A
Source code line 3 for Feature A
Source code line 3 for Feature A with a typo
Source code line 5 for Feature A

Source code line 1 for Feature B
Source code line 2 for Feature B
Source code line 3 for Feature B
Source code line 3 for Feature B
Source code line 5 for Feature B

################################
commit 01 -- HEAD
commit 02
commit 03
commit 04
commit 05
commit 06
commit 07
commit 08
commit 09
commit 10
commit 11
commit 12
```

</details>
</details>
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExMTUwNDkxNjksMjA3MDk1MTQ2NiwtOD
Y0Njg4ODgsMTIzNzMyMzc3NCwtMTc5MjQzMDc1NSwxMzA3Njgy
NDc2LC04MDMwNTY3NF19
-->