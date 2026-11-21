---
id: collect-261121-veille-ia/divers-tech/yavchn
title: "YAVCHN"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["story", "window", "view", "list", "article", "reader", "menu", "open"]
source: https://yavchn.parkscomputing.com/hn/?open=hn-49364559,reader-1&p.hn-49364559=maximized:0.06,0,0.55,1&r.reader-1=hn-49922569&p.reader-1=maximized:0.06,0.05,0.55,0.75
source_anchor: ""
source_lines: [1, 68]
section: "Divers_Tech"
---
#### Keyboard

      - `j` /`k`
- Move down and up the story list. The arrow keys scroll whatever has focus.
- `Enter`
- Read the marked story in the article reader.
- `[` /`]`
- Read the story above or below in the list, in the same article reader. Back returns to the story you came from.
- `p`
- Pin or unpin the marked story, which keeps it in Pinned.
- `n` /`N`
- Move to the next or previous top-level comment in the window in front.
- `c`
- Collapse or expand that comment.
- `f`
- Hide or show the story list.
- `Esc`
- Close a menu or this help.
- Access key `m`
- Go to the menu bar. Most browsers take it with `Alt` on Windows and Linux, and Safari with`Control` and`Option` .
- `?`
- Show this help.

#### Windows

      Each story opens in a window holding its article above its discussion; drag the bar between them to share the room differently. A window can be moved by its title bar, resized from any edge, snapped to a half or a corner by dragging it there, maximised, or minimised to the bar at the foot of the page. Use Window > New reader window to open an empty reader, or Story > Open in new reader window to open another reader for the current article. Docked readers keep their articles when you select another story from the sidebar. Minimized readers can be restored and reused for their site. A window's Previous and Next buttons read up and down the list in the same window.

      A link in a comment or an article to another Hacker News or Lobsters thread opens that thread in a window too. A link to a single HN comment opens the comment above its replies.

      While a story's window is in front, the Story and Discussion menus in the menu bar hold its commands: pinning, Previous and Next story, sorting, collapsing every thread, jumping to the first new comment. Each window also remembers where you were in its article and discussion, so a reload, or Back to a story that Next took you past, finds your place again. Closing a window forgets it.

      The whole arrangement lives in the address, so a bookmark or a shared link brings it back, and Back undoes the last change. Moving between Hacker News, Lobsters, their lists, Pinned and Find changes only the list, and leaves the windows open.

    
    
      #### The list

      The pin at the start of a row keeps the story in Pinned, and the cross at its end hides it. Pinned can be narrowed by words in the title, site or author, by source, and to the stories you haven't opened yet, and ordered by when you pinned them, by points or by comments; the filters are part of the address, so a filtered view can be bookmarked. Scroll past the end of the list to load more. Domain filters, in the View menu, hide every story from a site.

      Collections are named lists of stories. Story > Add to collection files the story in front into one or more of them, and the Collections feed shows them all or one at a time; the menu that chooses collections also creates, renames, and deletes them. A note is your own text on a story. Choose Add note in a story's toolbar to write one; it saves as you type. Rows with a note carry the note mark, and the Notes feed lists every noted story and searches the text of your notes.

    
    
      #### Browsing view

      View > Windowed and View > Classic select the browsing view and save your default in this browser. Window view reuses a reader for each feed. Classic view opens stories and applets as pages. Open as a page is a one-off action that does not change your saved default. Use the Windowed selector to return an article to a window. Direct page links always open as pages.

    
    
      #### Applets

      The Applets menu in the menu bar holds three tools, each a window of its own. **Replies to me** takes your Hacker News user name and lists the replies to your last thirty comments and stories, checking again every three minutes while it is open, and marking what is new since you last marked them read. **Look up a user** opens a profile on Hacker News or Lobsters, with their submissions and recent comments, as a commenter's name in any discussion does; the bar at the top of a profile looks up someone else in the same window, and Back returns to the one before. **Who is hiring?** filters the posts of HN's monthly hiring threads by the words you type.

      They read only what the sites publish to everyone, so none of them asks for a login, and your user name stays in this browser anonymously and is included in retained applet state when signed in.

    
    
      #### Find

      Find takes any link and lists every time it was submitted to Hacker News and Lobsters, so you can read each discussion of it.

    
    
      #### About

      YAVCHN never sees your Hacker News or Lobsters login. The discussion is fetched from each site's public API; to vote or reply, follow the link above the discussion, or the arrow beside a comment, to the source's own site. Without a YAVCHN account, your data stays in this browser. When signed in, pins, collections, notes, blocked domains, and retained reading state are stored with your account and synchronized across devices. Hidden stories and layout stay in this browser. The privacy policy has the details.

      Open source: github.com/paulmooreparks/yavchn. Built with PUDL.
