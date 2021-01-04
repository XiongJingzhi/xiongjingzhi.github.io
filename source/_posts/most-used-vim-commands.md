---
title: most-used vim commands
date: 2019-12-08 15:44:36
tags: operation
---

## vim model 
1. normal model: 普通模式
2. insert model: 插入模式 
3. visual model: 视图模式

## vim grammar 
1. Nearly all commands can be preceded by a number for a repeat count: 量词 + 命令
2. `<Esc>` gets you out of any mode and back to command mode: escape 到normal模式 
3. Commands preceded by : are executed on the command line at the bottom of the screen: 通过: 运行命令
4. :help help with any command: 获取命令帮助

## navigation
1. Cursor movement: ←h ↓j ↑k l→ : 光标移动
2. Words: w (by punctuation)(下个单词头): 通过标点; W (by spaces): 通过空格; b(单词头); B; e(单词末); E;
3. Line: 0 start(本行头); ^ first non-whitespace(第一个非空字符); $ end of line(本行末); - first non-whitespace of upper line(上一行第一非空字符);  + first non-whitespace of lower line(下一行第一非空字符);
4. Paragraph: { previous blank line; } next blank line: 上下一个空格行
5. File: gg G 100G: (G end of file, gg start of file, number G absolute line) 
6. Marks: mx (set mark x); 'x go to mark; :marks view marks; '. go to position of last edit; '' go back to last point before jump. 
7. Scroll: Ctrl+F, Ctrl+B full screen; Ctrl+D, Ctrl+U half screen; Ctrt+E, Ctrl+Y scroll one line; zz zt zb screen postion;

## Edit 
1. u undo; Ctrl+r;
2. i insert text at cursor; I insert text at start of line; 
3. a append text after cursor; A end of line;
4. o open new line below; O above; 
5. r replace single character; R multiple;
6. cw change word; C change to end of line; cc change whole line;
7. ci (inside parentheses); eg: ci' ci) ci 

## todo