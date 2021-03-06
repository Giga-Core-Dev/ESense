# ESense
ESense - Erlang "IntelliSense" for Emacs

Dependencies:
=============

The Yaws (http://yaws.hyber.org/) HTML parsing module needs to be
installed. (You may need to add it to the code search path in your
.erlang file using code:add_path/1.)

ESense was tested with version 2.5.2 of erlang.el. It might not work
with earlier versions.


Installation
============

Put esense.el and esense-start.el into a directory which is in
your load-path and add

     (require 'esense-start)

to your initialization file.

Set the variable esense-indexer-program to point to the location
where esense.sh is installed:

     (setq esense-indexer-program "/path/to/esense.sh")



XEmacs issues
=============

The following features are not available on XEmacs:

    - tool-tips, documentation is shown in the resized echo area
      instead
    - completion display method 'frame is not available
    - during completion no pop up help is shown for the currently
      selected item


Preparations before first use
=============================

Generate index files for the sources with esense.sh:

    esense.sh <directory> [<index-directory>]

The script will process the given directory and stores index
information in <index directory> which defaults to $HOME/.esense
if not given.

You can also generate new index files after Emacs is started. If you
do issue an M-x esense-initialize command in Emacs, so that it knows
about the new index files.

You must generate index files for the OTP sources, because index files
for certain modules (e.g. erlang) need to exist for ESense to work.

If you want to generate index files from the official OTP HTML
documentation and also from the official sources then index the
sources first and then the HTML documentation. This way the index file
of modules which have HTML documentation will point to the HTML doc
and the rest will use the source files.


How it works
============
 
(In the text below M-<char> means pressing the Meta key and the given
character).

ESense uses a single activation key (by default F1) which does
different things depending on the context:
 
If the cursor is ON a symbol (function, record, macro) it shows the
documentation of the symbol in a tool tip.
 
If the cursor is at the end of a symbol it tries to complete the
symbol.
 
If the cursor is in a parameter list and the two conditions above
are not met then it shows the documentation of the entity the
parameter list belongs to in a tool tip.
 
If the modifier CTRL is also pressed then in contexts where
normally documentation is shown the definition of the symbol is
visited instead. The original location is put onto the tag stack
of etags, so M-* can be used to return to the starting location.

Meta+<activation key> jumps to an arbitrary module/function with
completion.

If the cursor is on an -include line the corresponding header file
is visited.

The index files are loaded from the disk on demand and they are cached
once loaded, so the first access may be slower, but the further ones
are instantaneous.


Using an OTP earlier than R10B
==============================

If you want to use an OTP version earlier than R10B then copy the beam
files of the missing modules to somewhere where esense.erl can find
them (for example, you can use the "compat" directory which is already
in esense.sh). To find out which modules are missing simply run
esense.sh and determine the name of the missing modules from the crash
messages.


Troubleshooting
===============

"Opening directory: No such file or directory" error when jumping to
the definition of a standard module
--------------------------------------------------------------------

Make sure the variable erlang-root-dir is set correctly.


Assertion failed when reading an index file
-------------------------------------------

Make sure you use the same version of esense.sh and esense.el.

For the indexer program (esense.sh) to work properly the Erlang shell
started should not have any initial output. In case the Erlang boot
sequence contains additional process start-ups, you may see progress
report printouts. You can eliminate them by telling the Erlang shell
to use the original start up sequence. 


-------------------------
                         
  Local Variables:       
  mode: text             
  mode: auto-fill        
  adaptive-fill-mode: t  
  End:                   
                         
-------------------------
