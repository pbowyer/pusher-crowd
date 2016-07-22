# Rationale

pusher-crowd exists to detect and help you respond to the following situations:

1. A user having the same page open in multiple tabs
2. Multiple users having the same page open in multiple tabs
3. A user/multiple users having multiple tabs open (of pages including your Javascript) 
    * really a special case of 1) using the same channel name across the site
4. A user being logged in in different browsers (so different session IDs) and having the same page open in each browser 
    * (also works with multiple users)
5. Multiple tabs / multiple windows with one user and multiple users - a combination where e.g. you want a sole instance of a page interactable with
    
## Why was it developed?

I took on a legacy application which stored data in the session, not the URL. If a user opened a new tab, data corruption could occur.

I then built a CRUD app where record locking wasn't specified (or within budget), but it turned out to be needed. This script lets me provide poor man's UI record locking

# Dependencies

You need an account with [Pusher](https://pusher.com), and an application created.

You need to be able to understand PHP, and translate the examples into whichever programming language/web framework you use.

# Usage

_[Fill with code examples before writing any code]_

## Scenario 1

* Albert and Brenda can both have the same page open in their browsers.
* Albert can have the page open in multiple browsers.
* Albert cannot have the page opened in multiple tabs.
* Brenda can have the page open in multiple browsers.
* Brenda cannot have the page opened in multiple tabs.

## Scenario 2
