# How to toggle panes in Zotero VSCode-style

Based on discussion here: <https://github.com/windingwind/zotero-actions-tags/discussions/169>

![Demo pane toggle in Zotero](https://github.com/user-attachments/assets/c7dab2cd-33fe-4a68-b8b6-91b5833745bf)

Install `Zotero Actions & Tags` extension from <https://github.com/windingwind/zotero-actions-tags>

In settings, add new actions of type `Script` with the following code snippets:

## Left Pane Toggle

*Suggestion: assign to `Cmd/Ctrl + B` to mimic VSCode behavior.*

```applescript
/**
 * Toggle Left Pane
 * @author [MuiseDestiny](https://github.com/MuiseDestiny), windingwind
 * @usage Assign a shortcut
 * @link https://github.com/windingwind/zotero-actions-tags/discussions/169
 * @see https://github.com/windingwind/zotero-actions-tags/discussions/169
 */
const Zotero = require("Zotero");
const Zotero_Tabs = require("Zotero_Tabs");
const document = require("document");

if (item) {
  return;
}

if (Zotero_Tabs.selectedType === "library") {
  const splitter = document.querySelector("#zotero-collections-splitter");
  if (splitter.getAttribute("state") == "collapsed") {
    splitter.setAttribute("state", "");
    return;
  } else {
    splitter.setAttribute("state", "collapsed");
    return;
  }
} else {
  Zotero.Reader.getByTabID(
    Zotero_Tabs.selectedID
  )._internalReader.toggleSidebar();
  return;
}
```


## Right Pane Toggle

*Suggestion: assign to `Cmd/Ctrl + Shift + B` to mimic VSCode behavior.*

```applescript
/**
 * Toggle Right Pane
 * @author [MuiseDestiny](https://github.com/MuiseDestiny), windingwind
 * @usage Assign a shortcut
 * @link https://github.com/windingwind/zotero-actions-tags/discussions/169
 * @see https://github.com/windingwind/zotero-actions-tags/discussions/169
 */
const Zotero = require("Zotero");
const Zotero_Tabs = require("Zotero_Tabs");
const document = require("document");
const DEFAULT_OPEN_PANE = "item";

if (item) {
  return;
}

const splitter = document.querySelector(Zotero_Tabs.selectedType === "library" ? "#zotero-items-splitter":"#zotero-context-splitter");

if (splitter.getAttribute("state") == "collapsed") {
splitter.setAttribute("state", "");
return;
} else {
splitter.setAttribute("state", "collapsed");
return;
}
```

## Toggle Both Panes

*Suggestion: I personally assigned it to `Cmd/Ctrl + D`*

```applescript
/**
 * Toggle Both Panes
 * @author [MuiseDestiny](https://github.com/MuiseDestiny), windingwind, with modifications
 * @usage Assign a shortcut to toggle both left and right panes simultaneously.
 * @link https://github.com/windingwind/zotero-actions-tags/discussions/169
 */

const Zotero = require("Zotero");
const Zotero_Tabs = require("Zotero_Tabs");
const document = require("document");

if (typeof item !== 'undefined' && item) {
  return;
}

// Toggle Right Pane
const rightSplitter = document.querySelector(Zotero_Tabs.selectedType === "library" ? "#zotero-items-splitter" : "#zotero-context-splitter");
if (rightSplitter) {
  const rightPaneState = rightSplitter.getAttribute("state");
  rightSplitter.setAttribute("state", rightPaneState === "collapsed" ? "" : "collapsed");
}

// Toggle Left Pane
if (Zotero_Tabs.selectedType === "library") {
  const leftSplitter = document.querySelector("#zotero-collections-splitter");
  if (leftSplitter) {
    const leftPaneState = leftSplitter.getAttribute("state");
    leftSplitter.setAttribute("state", leftPaneState === "collapsed" ? "" : "collapsed");
  }
} else {
  const reader = Zotero.Reader.getByTabID(Zotero_Tabs.selectedID);
  if (reader && reader._internalReader) {
    reader._internalReader.toggleSidebar();
  }
}

return;
```
