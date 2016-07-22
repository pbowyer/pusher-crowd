# Rationale

pusher-crowd exists to detect and allow you to take action in the following situations:

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

# API Design ideas

The library is trying to cover a number of use-cases, which is making for an unwieldy API. I am thinking something fluent like:

```
// Most liberal
var pusher = new PusherCrowd().multipleWindowsPerUser().multipleTabsAllowed().multipleUsers();
// Most restrictive
var pusher = new PusherCrowd().oneWindowPerUser().oneTabPerWindow().oneUser();
pusher.run();
```

Helper classes/methods can be provided later to wrap common cases.

The defaults would be liberal (multiple windows, multiple tabs, multiple users) -- actually better to start strict, to prevent errors (ask for permission, not have to remember to reduce it). Methods to add and remove permissions are present, to allow for explicit statements and to add and remove as-needed.

## Handling Infractions

When a user opens an additional tab/window, or a second user visits the page(s), users of this library want to do something. They need to be able to register a handler that's called, and distinguish which infraction(s) have occurred.

I am thinking something like the Promises interface for these, as it looks much neater in code than passing in an object with methods/parameters to be called (are there methods in Javascript? as in `{ myCallback: function(data) { ... } }`..?). I don't know how to implement that; I guess they are async calls, but ones which occur multiple times. Something like:

```
pusher.run().onMultipleTabs(function(data) { alert('Oi, only 1 tab allowed!'); }).onSuccess(function(data) { ... };
```

### The constructor

We need to pass in:
* `pusher key`
* `authEndpoint` (URL)

## Implementation notes
```
oneWindow:
    All copies of this user need to have the same session ID

oneTabPerWindow:
    The session ID needs to be unique for every instance of this user

oneUser:
    The user ID appears max once
```

Filtering cannot be changed once `run()` is called, as it would change only in the current tab, and not in every window.
