---
title: <span class='keypress'>alt-shift-l</span>
lang: en es pl
page_language: en
author: Craig Stuart Sapp
creation_date: 20 May 2018
last_updated:
ref: commands-alt-shift-l
tags: [all, commands]
sidebar: main_sidebar
keywords: interface commands layers
summary: "The <span class='keypress'>alt-shift-l</span> command adds an empty local comment line above the current line in the text editor."
permalink: /commands/alt-shift-l/index.html
---

Pressing <span class="keypress">alt-shift-l</span> will add an empty
local comment line above the current line, provided that the current
line contains spines (not a global comment, empty line, or exclusive
interpretation line).  The added local comment line will have the same
number of spine fields as the current line.

After the empty local comment line has been added, layout commands
can be added (or any other use you have for local comments).  Below
is an example of adding some text in the score through a layout
comment.


{% include image.html
	file="textlayout.gif"
	alt="adding a layout command."
	max-width="75%"
	caption="Adding a layout command after pressing alt-shift-l."
%}


