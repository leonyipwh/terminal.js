# terminal.js

Terminal.js is a rendering engine for vt100-like terminals.
It is written from scratch and supports most commonly used escape sequences.

[![Build Status](https://travis-ci.org/c3ks/terminal.js.png)](https://travis-ci.org/c3ks/terminal.js)

## Benefits

- it can be used both in nodejs and as a javascript library in the browser
- it avoids any dependencies like pty, html outputting etc so you can provide your own
- it does not use an html dom to store it's state, this makes it server state compatible
- clean separation of different escape codes
- has a test framework

## Usage
### TermWriter Usage

    var TermWriter = require('terminal').TermWriter;
    var TermBuffer = require('terminal').TermBuffer;

    var buffer = new TermBuffer(80,24);
    var terminal = new TermWriter(termBuffer);
    terminal.write("first test\n");
    // termWriter.buffer is accessible

### TermBuffer Usage

    var TermBuffer = require('terminal').TermBuffer;
    var buffer = new TermBuffer(80,24);
    buffer.inject("first test\n");

### TermDiff Usage

    var TermDiff = require('terminal').TermDiff;
    var TermBuffer = require('terminal').TermBuffer;
    var buffer1 = new TermBuffer(80,24);
    var buffer2 = new TermBuffer(80,24);
    var terminal1 = new TermWriter(buffer1);
    var terminal2 = new TermWriter(buffer2);
    var diff = new TermDiff(terminal1.buffer,terminal2.buffer);

## TermBuffer Functions

- createChar
- createLine
- getLine

- write
- escapeWrite
- inject
- lineFeed
- mvCur : moves to relative coordinates
- setCur : move to absolute coordinates
- mvTab
- tabSet
- tabClear
- saveCursor
- restoreCursor
- deleteCharacters
- eraseCharacters
- setScrollRegion
- eraseInDisplay
- eraseInLine
- deleteLines
- resetAttr
- chAttr
- insertBlanks
- resize
- insertLines
- toString
- setLed
- scroll
- eventToKey


## TermBuffer Object structure

- width: width of the terminal ``80``
- height: height of the terminal ``24``

- leds: ``{ '1': false, '2': false, '3': false, '4': false }``
- mode: ``{ cursor: true, appKeypad: false, wrap: true, insert: false, crlf: false, mousebtn: false, mousemtn: false, reverse: false, graphic: false }``

term.mode.crlf = true make a linefeed go the next line AND to the beginning of the line

- cursor: ``{ x: 14, y: 1 }``
- savedCursor: remembered location of previous cursor - `{ x: 0, y: 0 }``

- defaultBuffer: when a new terminal gets created it has this buffer ``[]``
- altBuffer: alternate to Default Buffer (for use in Xterm)
- escapeBuffer: TODO - defaults to ``null``

- tabs: ``[]``
- scrollRegion: ``[ 0, 24 ]``
- scrollBack: ``[]``

- lineAttr: ``{ doubletop: false, doublebottom: false, doublewidth: false }``
- defaultAttr: `` { fg: 15, bg: 0, bold: false, underline: false, blink: false, inverse: false }``

- buffer: `` [ ...arrays of lines... ]``
- line: ``line: [ ... arrays of chars... ] , attr:{}, changed: true }``
- char: ``{ chr: 'r', attr: { fg: 15, bg: 0, bold: false, underline: false, blink: false, inverse: false } }``

Note: a line can be sparse => ``line: [ , , , , , , { ...char .... } ]``

## TermBuffer structure (subject to change)

    { '0': { act: '+', rm: 0, line: [ ... ], attr: {}, changed: true }, # If the line was added
      '1': { act: 'c', rm: 0, line: [.....], attr: {}, changed: true }, # If the line was changed
      '2': { rm: ... },                                                 # If the line was removed

## Alternatives & Related
### Jquery/Server side:

- headless-terminal - <https://github.com/dtinth/headless-terminal>
- tty.js - <https://github.com/chjj/tty.js/> 
- jquery terminal emulator  - <https://github.com/jcubic/jquery.terminal>
- tty.js - <https://github.com/jondistad/tty.js>
- screen-buffer - <https://github.com/dtinth/screen-buffer>
- Terminal.js - <https://github.com/chenxiaoqino/Terminal.js>
- terminal.js - <https://github.com/wayneashleyberry/terminal.js>
- Terminal.js - <https://github.com/Tehnix/Terminal.js>
- Js-terminal - <https://github.com/realslimkarthik/js-terminal>

### Expose local shell in a browser:

- ttycast.js - <https://github.com/dtinth/ttycast>
- js.terminal - <https://github.com/svewag/js.terminal>
- Node-terminal-shark - <https://github.com/raadad/node-terminal-shark>

### Simulate a pseudo shell in browser:

- terminal.js is a dead simple JavaScript library for emulating a shell environment - <https://github.com/eosterberg/terminaljs>
- terminal.js - <https://github.com/wayneashleyberry/terminal.js>
- JUIx - <http://www.masswerk.at/jsuix/>
- Termlib - <http://www.masswerk.at/termlib/>

### Reference docs for vt100, csi , ... codes

- <http://www.vt100.net/docs/vt510-rm/contents>
- <http://www8.cs.umu.se/~isak/snippets/vt100.txt>
- <http://linux.about.com/library/cmd/blcmdl4_console_codes.htm>

## Remarks

- we should clone the defaultBuffer instead of equalling it (see reset function)
- attrCommited? always after changr of attr?
- this.attr = this.defaultAttr (again it should copy it not equal the object)
- Termdiff.diff() - should it not be an array instead of a hash?
