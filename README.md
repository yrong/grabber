# grabber

Grabber is a concurrent declarative web scraper and downloader.

## Features:
  * Simple tree-like JSON configuration
  * XPath and Regexp extractors
  * Parallel parsing and extraction
  * Parallel download
  * Ability to bail out early (e.g. for updating)
  * Fails fast on config errors, tolerates web errors
  * Follow, every, and single extraction modes
  * Multiple XPaths or Regexps per stage
  * Multi-grouped regexps with a separator (e.g. extract to CSV)
  * It's rather fast
  
## General:

Purely declarative web scrapper with parallel downloading. Intended to automate the common pattern found in websites: archive page pagination - post links - resource links. You should open unixporn.json example to see the structure of a target definition. Non-URL data extraction example is in golang.json.

There are two modes: follow - use for pagination, and every - use for link extraction. Then there are three actions that can be taken: print - will print the link to stdout, log - will log the link (either stdout or stdin, depending on the state of the -log flag), download - will send the link to the built-in parallel downloader (which will download only if the file doesn't already exist), and finally raw - this mode will print to stdout whatever was matched, without trying to resolve it into a full URL (this way you can use this software to extract pretty much anything).

The do actions can be chained in arbitrary manner and form a command path that will be executed linearly. You can define multiple targets in a single file. It will work with SSL connection however it will not do any verification.

This is by far the most involved piece of software that I have written in Golang, and although it's far from beauty, I had a lot of fun writing it.

Depends on gokogiri. Tested under Linux and FreeBSD.

Run `grabber -h` to see command-line options.

See `examples/` directory and consult the code to learn the format
of the config files.

Note that for `tumblr.json` you'll need to replace all occurrences of 
`{{name}}` with a proper account (subdomain) name and all occurrences of 
`{{paging}}` with the (XPath's text() operator) contents of what your target 
blog uses for 'next page' (or semantically equivalent). You may also notice 
that the format is already template-friendly, so you can easily write a script 
for generating per-blog templates.

The examples provided are certainly not exhaustive.

Advice:

Remember you can build your config iteratively by using the `log` command,
so that you make sure the current level works as it should before going
further.

When downloading:

For the first run set `bail` to `0` and use options `-quiet -stdout`,
you may also wish to pipe the output of the run to `tee log`.
Then inspect the output/logfile for any errors. If it looks ok set `bail` to
something reasonable e.g. if you have 10 assets per page set it to 20.

## Todo / Bugs

  * Needs testing 'in the wild'
  * Better documentation
  * Ability to use `Content-Disposition`
  * Full config parsing and error checking during load
  * Test suite

## Copyright

Copyright (c) 2014 Piotr S. Staszewski

Absolutely no warranty. See LICENSE.txt for details.
