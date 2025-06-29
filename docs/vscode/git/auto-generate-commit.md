# Auto Generate Commit

## 使用插件使git自动填写commit信息

用git管理博客或者文章的时候，更新或者修改了文章，只想快速提交而不想填提交信息，此时需要一个方便的自动生成提交信息的工具。

vscode插件git-automator可以实现这个功能，安装后，每次提交时，会自动填写提交信息。

有些插件生成的提交信息仅包含日期，那信息太少了，git的commit本身就带时间戳。git-automator生成的commit信息是：xxx file modify/create/delete。这对我来说足够方便了。

## 使用方法

> Usage:
>
> Add all edited files to Git and commit them
>
> 1. Hit Ctrl + Shift + A (PC) / Cmd + Shift + A (Mac).
> 2. Enter the commit message.
> 3. Press ENTER.
>
> Add ONLY the current file to Git and commit it
>
> 1. Hit Ctrl + Shift + Z (PC) / Cmd + Shift + Z (Mac).
> 2. Enter the commit message.
> 3. Press ENTER.
>
> Setup the auto-prefill for commit messages
>
> 1. Hit Ctrl + Shift + P (PC) / Cmd + Shift + P (Mac).
> 2. Look for Preferences: Open User Settings.
> 3. Look for gaac. settings to customize them.
>
> Push local commits
>
> 1. Hit Ctrl + Shift + X (PC) / Cmd + Shift + X (Mac).