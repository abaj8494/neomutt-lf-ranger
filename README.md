# Welcome

This is a repo which documents how I integrated the lf terminal file manager into neomutt for email attachments.

# Problem

When attaching files to an email in neomutt, you can only select one file at a time and navigating through directory structures is cumbersome. 

![]{img/problem.gif}

# Solution

Instead of using neomutt's file browser, use [lf](https://github.com/gokcehan/lf/) which will allow you to select multiple files. 

![]{img/solution.gif}

# Bonus Features

Due to lf's configurability you can use your pre-existing bindings to navigate through directories :)

# Usage

We will tell neomutt to run a bash script. This is defined in the muttrc with `macro compose A "<shell-escape>bash $HOME/.config/mutt/filepick<enter><enter-command>source $HOME/.config/mutt/multisel<enter><shell-escape>bash $HOME/.config/mutt/filepick clean<enter>" "Attach with your file manager"
`

This bash script, labelled `filepick` runs `lf`. From here, you tag the files you want to attach (with 't') and you quit lf (with 'q').

The script then checks your tags (which are defined in a file at `~/.local/share/lf/tags`), manipulates the text stream and outputs it to `~/.config/mutt/multisel`. This `multisel` file is then sourced by neomutt to attach your files.

# @ Nerds
Text stream manipulation is achieved through `sed`'s substitution capabilities, and it removes the last 2 characters from each line. The stream is passed through to `sed` with `printf` as opposed to `cat` and `echo` such that newline characters are preserved.


# Acknowledgements

This script was largely based off this person's [script](https://github.com/anufrievroman/neomutt-file-picker) to accomplish the same thing for `ranger` and `vifm`. 



