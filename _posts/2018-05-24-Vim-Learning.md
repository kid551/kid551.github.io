---
layout: post
comments: true
title:  "Learn Vim Progressively"
excerpt: "Referenced from "
date:   2018-05-16
mathjax: true
---



>  TL;DR: You want to teach yourself vim (the best text editor known to human kind) in the fastest way possible. This is my way of doing it. You start by learning the minimal to survive, then you integrate all the tricks slowly.



[Vim](http://www.vim.org/) the Six Billion Dollar editor

> Better, Stronger, Faster.

Learn [vim](http://www.vim.org/) and it will be your last text editor. There isn’t any better text editor that I know of. It is hard to learn, but incredible to use.

I suggest you teach yourself Vim in 4 steps:

1. Survive
2. Feel comfortable
3. Feel Better, Stronger, Faster
4. Use superpowers of vim

By the end of this journey, you’ll become a vim superstar.

But before we start, just a warning. Learning vim will be painful at first. It will take time. It will be a lot like playing a musical instrument. Don’t expect to be more efficient with vim than with another editor in less than 3 days. In fact it will certainly take 2 weeks instead of 3 days.

## 1st Level – Survive

1. Install [vim](http://www.vim.org/)
2. Launch vim
3. DO NOTHING! Read.

In a standard editor, typing on the keyboard is enough to write something and see it on the screen. Not this time. Vim is in *Normal*mode. Let’s go to *Insert* mode. Type the letter `i`.

You should feel a bit better. You can type letters like in a standard editor. To get back to *Normal* mode just press the `ESC` key.

You now know how to switch between *Insert* and *Normal* mode. And now, here are the commands that you need in order to survive in *Normal* mode:

> - `i` → *Insert* mode. Type `ESC` to return to Normal mode.
> - `x` → Delete the char under the cursor
> - `:wq` → Save and Quit (`:w` save, `:q` quit)
> - `dd` → Delete (and copy) the current line
> - `p` → Paste
>
> Recommended:
>
> - `hjkl` (highly recommended but not mandatory) → basic cursor move (←↓↑→). Hint: `j` looks like a down arrow.
> - `:help <command>` → Show help about `<command>`. You can use `:help` without a `<command>` to get general help.

Only 5 commands. That is all you need to get started. Once these command start to become natural (maybe after a day or so), you should move on to level 2.

But first, just a little remark about *Normal mode*. In standard editors, to copy you have to use the `Ctrl` key (`Ctrl-c` generally). In fact, when you press `Ctrl`, it is as if all of your keys change meaning. Using vim in normal mode is a bit like having the editor automatically press the `Ctrl` key for you.

A last word about notations:

- instead of writing `Ctrl-λ`, I’ll write `<C-λ>`.
- commands starting with `:` end with `<enter>`. For example, when I write `:q`, I mean `:q<enter>`.

## 2nd Level – Feel comfortable

You know the commands required for survival. It’s time to learn a few more commands. These are my suggestions:

1. Insert mode variations:

   > - `a` → insert after the cursor
   > - `o` → insert a new line after the current one
   > - `O` → insert a new line before the current one
   > - `cw` → replace from the cursor to the end of the word

2. Basic moves

   > - `0` → go to the first column
   > - `^` → go to the first non-blank character of the line
   > - `$` → go to the end of line
   > - `g_` → go to the last non-blank character of line
   > - `/pattern` → search for `pattern`

3. Copy/Paste

   > - `P` → paste before, remember `p` is paste after current position.
   > - `yy` → copy the current line, easier but equivalent to `ddP`

4. Undo/Redo

   > - `u` → undo
   > - `<C-r>` → redo

5. Load/Save/Quit/Change File (Buffer)

   > - `:e <path/to/file>` → open
   > - `:w` → save
   > - `:saveas <path/to/file>` → save to `<path/to/file>`
   > - `:x`, `ZZ` or `:wq` → save and quit (`:x` only save if necessary)
   > - `:q!` → quit without saving, also: `:qa!` to quit even if there are modified hidden buffers.
   > - `:bn` (resp. `:bp`) → show next (resp. previous) file (buffer)

Take the time to learn all of these command. Once done, you should be able to do every thing you are able to do in other editors. You may still feel a bit awkward. But follow me to the next level and you’ll see why vim is worth the extra work.

## 3rd Level – Better. Stronger. Faster.

Congratulation for reaching this far! Now we can start with the interesting stuff. At level 3, we’ll only talk about commands which are compatible with the old vi editor.

### Better

Let’s look at how vim could help you to repeat yourself:

1. `.` → (dot) will repeat the last command,
2. N<command> → will repeat the command N times.

Some examples, open a file and type:

> - `2dd` → will delete 2 lines
> - `3p` → will paste the text 3 times
> - `100idesu [ESC]` → will write “desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu”
> - `.` → Just after the last command will write again the 100 “desu”.
> - `3.` → Will write 3 “desu” (and not 300, how clever).

### Stronger

Knowing how to move efficiently with vim is *very* important. Don’t skip this section.

1. N`G` → Go to line N

2. `gg` → shortcut for `1G` - go to the start of the file

3. `G` → Go to last line

4. Word moves:

   > 1. `w` → go to the start of the following word,
   > 2. `e` → go to the end of this word.
   >
   > By default, words are composed of letters and the underscore character. Let’s call a WORD a group of letter separated by blank characters. If you want to consider WORDS, then just use uppercase characters:
   >
   > 1. `W` → go to the start of the following WORD,
   > 2. `E` → go to the end of this WORD.






