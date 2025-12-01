# vim9script Style Guide
## Introduction
This is inspired by [Google's Vimscript Style Guide](https://google.github.io/styleguide/vimscriptfull.xml). Similar to the original guide this is a casual version of the vim9script style guide. When submitting vim plugin code, you must adhere to these rules.

## Style Guide
### Documentation
Write documentation in .vim files in conformance with vimdoc standards.

### Whitespace
- Use two spaces for indents.
- Do not use tabs.
- Use spaces around operators except for arguments to commands.
- Do not introduce trailing whitespace.
  - You need not go out of your way to remove it.
- Indent continued lines by two tabs (4 spaces)
- Do not waste whitespace aligning common segments of similar commands. It is both difficult and expensive to maintain.

<style>
pre { white-space: pre-wrap; font-family: monospace; color: #f8f8f2; background-color: #1b1d1e; }
.Comment { color: #7e8e91; }
.PreProc { color: #a6e22e; }
.Statement { color: #f92672; font-weight: bold; }
.LineNr { color: #465457; background-color: #232526; padding-bottom: 1px; }
.Function { color: #a6e22e; }
.Operator { color: #f92672; }
.Normal { color: #f8f8f2; background-color: #1b1d1e; padding-bottom: 1px; }
</style>

<pre id='vimCodeElement'>
<span id="L1" class="LineNr">1 </span><span class="Comment"># bad</span>
<span id="L2" class="LineNr">2 </span><span class="Statement">command</span> <span class="Operator">-</span><span class="PreProc">bang</span> MyCommand  <span class="Function">call</span> myplugin#<span class="Normal">foo</span>()
<span id="L3" class="LineNr">3 </span><span class="Statement">command</span>       MyCommand2 <span class="Function">call</span> myplugin#<span class="Normal">bar</span>()
<span id="L4" class="LineNr">4 </span>
<span id="L5" class="LineNr">5 </span><span class="Comment"># good</span>
<span id="L6" class="LineNr">6 </span><span class="Statement">command</span> <span class="Operator">-</span><span class="PreProc">bang</span> MyCommand <span class="Function">call</span> myplugin#<span class="Normal">foo</span>()
<span id="L7" class="LineNr">7 </span><span class="Statement">command</span> MyCommand2 <span class="Function">call</span> myplugin#<span class="Normal">bar</span>()
</pre>

#### Line Continuations
- Prefer line continuations on semantic boundaries.
```
# good
command SomeLongCommandName
    call some#function()

# bad
command SomeLongCommandName call
    some#function()
```
- Don't use backslash to denote a line continuation.
```
# good
autocommand BufEnter <buffer>
    if !empty(val)
      some#function()
    endif

# bad
autocommand BufEnter <buffer>
    \ if !empty(val)
    \   some#function()
    \ endif
```

#### Comments
- Place a space after the `#` before the comment text.
```
# I am a comment line
```
- Do not use inline comments.
  - Where you would use an inline comment, put a line comment on the line above.
- When leaving blank lines in comments, include the `#` in the blank line.
```
# I am one continuous
#
# comment block
```

### Strings
Prefer single quoted strings. Specifically, in order of precedence:
- Always use single quotes for regular expressions
  - `'\s*'` is not the same as `"\s*"`
  - Single quotes will prevent the need for excessive backslashes.
  - Double quotes escape to one single quote in single quoted strings: `'example('')'` represents the string `example(')`
- If your string requires escaped characters (`\n`, `\t`, etc) use double quotes.
  - Escapes can not be expressed in single quoted strings.
  - Remember that `'\n'` in regex does not represent a newline, but rather `"\n"`. You only need to use double quotes when you want to embed the represented character itself (e.g. a newline) in the string.
 - If your string contains no escapes nor single quotes, use single quoted strings.
 - Most strings in vimscript are regexes, so this provides maximum consistency.
-  If your non-regex string contains single quotes but no double quotes, use double quotes.
  - Don't bother escaping strings if you don't have to.
- If your string contains both single and double quotes, use whichever quoting style requires less escaping.
  - Break ties in favor of single quotes.

### Built-in Functions
- Prefer long names of built ins. (i.e. `tabstop` over `ts`)