---
id: collect-261121-veille-ia/mikrotik/arborescence-des-pages-40
title: "Arborescence des pages"
domain: mikrotik
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["command", "console", "prompt", "login", "name", "commands", "line", "terminal"]
source: https://help.mikrotik.com/docs/spaces/ROS/pages/328134/Command+Line+Interface
source_anchor: ""
source_lines: [1, 179]
section: "GPT_OpenAI"
---
The console is used for accessing the MikroTik Router's configuration and management features using text terminals, either remotely using a serial port, telnet, SSH, console screen within WinBox, or directly using monitor and keyboard. The console is also used for writing scripts. This manual describes the general console operation principles. Please consult the Scripting Manual on some advanced console commands and on how to write scripts.

# Login Options

Console login options enable or disable various console features like color, terminal detection, and many other.

Additional login parameters can be appended to the login name after the '+' sign.

```
    login_name ::= user_name [ '+' parameters ]
    parameters ::= parameter [ parameters ]
    parameter ::= [ number ] 'a'..'z'
    number ::= '0'..'9' [ number ]
  
```
If the parameter is not present, then the default value is used. If the number is not present then the implicit value of the parameter is used.

Example: admin+ct80w - will disable console colors, disable auto detection and then set terminal width to 80.

| Param | Default | Implicit | Description | 
|---|---|---|---|
| **"w"** | auto | auto | Set terminal width | 
| **"h"** | auto | auto | Set terminal height | 
| **"c"** | on | off | disable/enable console colors | 
| **"t"** | off | off | Disable auto-detection of terminal capabilities | 
| **"e"** | on | off | Enables "dumb" terminal mode | 

# Banner and Messages

The login process will display the MikroTik banner and short help after validating the user name and password.

After the banner can be printed other important information, like **/system note** set by another admin, the last few critical log messages, demo version upgrade reminder, and default configuration description.

For example, the demo license prompt and last critical messages are printed

# Command Prompt

At the end of the successful login sequence, the login process prints a banner that shows the command prompt, and hands over control to the user.

Default command prompt consists of user name, system identity, and current command path />

For example, change the current path from the root to the interface then go back to the root

Use **up arrow** to recall previous commands (if this is a multiline command, then you can press **F8** in order to expand it) from command history (commands that added sensitive data, like passwords, will not be available in the history), **TAB** key to automatically complete words in the command you are typing, **ENTER** key to execute the command, **Control-C** to interrupt currently running command and return to prompt and **?** to display built-in help, in **RouterOS v7**, **F1** has to be used instead.

The easiest way to log out of the console is to press **Control-D** at the command prompt while the command line is empty (You can cancel the current command and get an empty line with **Control-C**, so **Control-C** followed by **Control-D** will log you out in most cases).

It is possible to write commands that consist of multiple lines. When the entered line is not a complete command and more input is expected, the console shows a continuation prompt that lists all open parentheses, braces, brackets, and quotes, and also trailing backslash if the previous line ended with **backslash**-white-space.

When you are editing such multiple line entries, the prompt shows the number of current lines and total line count instead of the usual username and system name.

line 2 of 3> :put (\

Sometimes commands ask for additional input from the user. For example, the command '`/password`' asks for old and new passwords. In such cases, the prompt shows the name of the requested value, followed by colon and space.

# Hierarchy

The console allows the configuration of the router's settings using text commands. Since there is a lot of available commands, they are split into groups organized in a way of hierarchical menu levels. The name of a menu level reflects the configuration information accessible in the relevant section.

For example, you can issue the `/ip route print` command:

Instead of typing `/ip route` path before each command, the path can be typed only once to move into this particular branch of the menu hierarchy. Thus, the example above could also be executed like this:

Each word in the path can be separated by **space** (as in the example above) or by "/"

Notice that the prompt changes in order to reflect where you are located in the menu hierarchy at the moment. To move to the top level again, type " / "

To move up one command level, type " .. "

You can also use **/** and **..** to execute commands from other menu levels without changing the current level:

# Item Names and Numbers

Many of the command levels operate with arrays of items: interfaces, routes, users, etc. Such arrays are displayed in similarly-looking lists. All items in the list have an item number followed by flags and parameter values.

To change the properties of an item, you have to use the set command and specify the name or number of the item.

## Item Names

Some lists have items with specific names assigned to each of them. Examples are interface or user levels. There you can use item names instead of item numbers.

You do not have to use the print command before accessing items by their names, which, as opposed to numbers, are not assigned by the console internally, but are properties of the items. Thus, they would not change on their own. However, there are all kinds of obscure situations possible when several users are changing the router's configuration at the same time. Generally, item names are more "stable" than the numbers, and also more informative, so you should prefer them to numbers when writing console scripts.

## Item Numbers

Item numbers are assigned by the print command and are not constant - it is possible that two successive print commands will order items differently. But the results of the last print commands are memorized and, thus, once assigned, item numbers can be used even after add, remove and move operations (since version 3, move operation does not renumber items). Item numbers are assigned on a per session basis, they will remain the same until you quit the console or until the next print command is executed. Also, numbers are assigned separately for every item list, so /`ip address print` will not change the numbering of the interface list.

You can specify multiple items as targets to some commands. Almost everywhere, where you can write the number of items, you can also write a list of numbers.

# General Commands

There are some commands that are common to nearly all menu levels, namely: **print, set, remove, add, find, get, export, enable, disable, comment, move.** These commands have similar behavior throughout different menu levels.

| Property | Description | 
|---|---|
| **add** | This command usually has all the same arguments as a set, except the item number argument. It adds a new item with the values you have specified, usually at the end of the item list, in places where the order of items is relevant. There are some required properties that you have to supply, such as the interface for a new address, while other properties are set to defaults unless you explicitly specify them. Common Parameters  Return Values  | 
| **edit** | This command is associated with the set command. It can be used to edit values of properties that contain a large amount of text, such as scripts, but it works with all editable properties. Depending on the capabilities of the terminal, either a full-screen editor or a single line editor is launched to edit the value of the specified property. | 
| **find** | The find command has the same arguments as a set, plus the flag arguments like disabled or active that take values yes or no depending on the value of the respective flag. To see all flags and their names, look at the top of the print command's output. The find command returns internal numbers of all items that have the same values of arguments as specified. | 
| **move** | Changes the order of items in the list. Parameters:  | 
| **print** | Shows all information that's accessible from a particular command level. Thus, `/system clock print` shows the system date and time,`/ip route print` shows all routes etc. If there\'s a list of items in the current level and they are not read-only, i.e. you can change/remove them (example of read-only item list is`/system history` , which shows a history of executed actions), then print command also assigns numbers that are used by all commands that operate with items in this list.Common Parameters:  | 
| **remove** | Removes specified item(-s) from a list. | 
| **set** | Allows you to change values of general parameters or item parameters. The set command has arguments with names corresponding to values you can change. Use ? or double Tab to see a list of all arguments. If there is a list of items in this command level, then the set has one action argument that accepts the number of items (or list of numbers) you wish to set up. This command does not return anything. | 

# Input Modes

It is possible to switch between several input modes:

- **Normal mode** - indicated by normal command prompt.
- **Safe mode** - safe mode is indicated by the word SAFE after the command prompt. In this mode, the configuration is saved to disk only after the safe mode is turned off. Safe mode can be turned on/off with**Ctrl+X or F4.** Read more >>
- **Hot-lock mode** - indicated by additional yellow >. Hot-lock mode autocompletes commands and can be turned on/off with**F7**

# Quick Typing

There are two features in the console that help entering commands much quicker and easier - the [**Tab**] key completions, and abbreviations of command names. Completions work similarly to the bash shell in UNIX. If you press the [**Tab**] key after a part of a word, the console tries to find the command within the current context that begins with this word. If there is only one match, it is automatically appended, followed by a space:

*/inte***[Tab]_** becomes **/interface _**

If there is more than one match, but they all have a common beginning, which is longer than that what you have typed, then the word is completed to this common part, and no space is appended:

*/interface set e***[Tab]_** becomes **/interface set ether_**

If you've typed just the common part, pressing the tab key once has no effect. However, pressing it for the second time shows all possible completions in compact form:

[admin@MikroTik] > interface set e[Tab]_
[admin@MikroTik] > interface set ether[Tab]_
[admin@MikroTik] > interface set ether[Tab]_
ether1 ether5
[admin@MikroTik] > interface set ether_

The **[Tab]** key can be used almost in any context where the console might have a clue about possible values - command names, argument names, arguments that have only several possible values (like names of items in some lists or name of the protocol in firewall and NAT rules). You cannot complete numbers, IP addresses, and similar values.

Another way to press fewer keys while typing is to abbreviate command and argument names. You can type only the beginning of the command name, and, if it is not ambiguous, the console will accept it as a full name. So typing:

[admin@MikroTik] > pi 10.1 c 3 si 100

equals to:

[admin@MikroTik] > ping 10.0.0.1 count 3 size 100

It is possible to complete not only the beginning, but also any distinctive sub-string of a name: if there is no exact match, the console starts looking for words that have string being completed as first letters of a multiple word name, or that simply contain letters of this string in the same order. If a single such word is found, it is completed at the cursor position. For example:

[admin@MikroTik] > interface x[TAB]_
[admin@MikroTik] > interface export _
[admin@MikroTik] > interface mt[TAB]_
[admin@MikroTik] > interface monitor-traffic _

# Console Search

Console search allows performing keyword search through the list of RouterOS menus and the history. The search prompt is accessible with the **[Ctrl+r]** shortcut. 

# Internal Chat System

RouterOS console has a built-in internal chat system. This allows remotely located admins to talk to each other directly in RouterOS CLI. To start the conversation prefix the intended message with the # symbol, anyone who is logged in at the time of sending the message will see it.

# List of Keys

| Key | Description | 
|---|---|
| Control-C | keyboard interrupt | 
| Control-D | log out (if an input line is empty) | 
| Control-K | clear from the cursor to the end of the line | 
| Control-U | clear from the cursor to the beginning of the line | 
| Control-X or F4 | toggle safe mode | 
| F7 | toggle hot-lock mode | 
| Control-R or F3 | toggle console search | 
| F6 | toggle cellar | 
| F1 | show context-sensitive help. | 
| Tab | perform line completion. When pressed a second time, show possible completions. | 
| # | Send a message to an internal chat system | 
| Delete | remove character at the cursor | 
| Control-H or Backspace | removes character before cursor and moves the cursor back one position. | 
| Control-\ | split line at cursor. Insert newline at the cursor position. Display second of the two resulting lines. | 
| Control-B or Left | move cursor backward one character | 
| Control-F or Right | move cursor forward one character | 
| Control-P or Up | go to the previous line. If this is the first line of input then recall previous input from history. | 
| Control-N or Down | go to the next line. If this is the last line of input then recall the next input from the history | 
| Control-A or Home | move the cursor to the beginning of the line. If the cursor is already at the beginning of the line, then go to the beginning of the first line of the current input | 
| Control-E or End | move the cursor to the end of the line. If the cursor is already at the end of the line, then move it to the end of the last line of the current input | 
| Control-L or F5 | reset terminal and repaint screen |
