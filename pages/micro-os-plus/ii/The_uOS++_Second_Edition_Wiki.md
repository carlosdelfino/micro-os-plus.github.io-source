---
layout: old-wiki-page
lang: en
permalink: /micro-os-plus/ii/The_uOS++_Second_Edition_Wiki/
title: The µOS++ Second Edition Wiki
author: Liviu Ionescu

date: 2015-09-28 11:07:03 +0000

---

The ***µOS++ SE*** (micro oh ɛs plus plus second edition) project is the second iteration of ***µOS++***, a [SourceForge hosted](http://sourceforge.net/projects/micro-os-plus/) open source, royalty-free, multi-tasking operating system intended for 16/32/64 bit embedded systems. It will be based on a simple preemptive scheduler specifically designed for ARM Cortex-M devices and also ported on AVR8 devices. It is written in a subset of C++11 and compiles with modern GNU GCC and LLVM Clang compilers. It uses templates for compile time polymorphism and inlines as much as possible, which leads to highly optimised code.

The authoritative µOS++ reference documents can be found in the project [documentation pages](http://micro-os-plus.sourceforge.net/doc), with additional documents in this Wiki. The source code itself, publicly available for browsing via the web [Git Browse](http://sourceforge.net/p/micro-os-plus/second/) interface is another good source of information. (Additional resources can be found in the SourceForge trackers mentioned in the [Support]({{ site.baseurl }}/micro-os-plus/ii/Support "wikilink") page). The configuration metadata is documented in the [XCDL wiki](http://xcdl.sourceforge.net/wiki/).

**Note:** This is currently a work in progress, for those interested in a functional version, please read [the µOS++ first edition wiki](http://micro-os-plus.sourceforge.net/old-wiki/).

Introduction
------------

The ***µOS++ SE*** is a major redesign of the ***µOS++*** project, adding a highly configurable build mechanism. Its design is centred on 32-bit ARM Cortex-M devices, but it is highly portable, with unit tests running on 64-bit POSIX platforms (like OS X and GNU/Linux).

Documentation
-------------

The ***µOS++ SE*** reference pages are created with a quite elaborate [Doxygen](http://www.doxygen.org/index.html) configuration and are available from [a separate site](http://micro-os-plus.sourceforge.net/doc).

<div style="text-align:center">
<a href="http://micro-os-plus.sourceforge.net/doc"><img src="{{ site.baseurl }}/assets/images/old/524px-Doxy.png" /></a>
</div>

Content can be browsed by classes (hierarchically grouped by namespaces, with inheritance and collaboration graphs), by modules (a logical/functional structure), by file names (the file system hierarchy), etc.

Style guides
------------

-   [Coding style (SE)]({{ site.baseurl }}/micro-os-plus/ii/Coding_style_(SE) "wikilink")
-   [Naming conventions (SE)]({{ site.baseurl }}/micro-os-plus/ii/Naming_conventions_(SE) "wikilink")
-   [Wiki pages style guide (SE)]({{ site.baseurl }}/micro-os-plus/ii/Wiki_pages_style_guide_(SE) "wikilink")
-   [Doxygen style guide (SE)]({{ site.baseurl }}/micro-os-plus/ii/Doxygen_style_guide_(SE) "wikilink")
-   [Deviations from standards (SE)]({{ site.baseurl }}/micro-os-plus/ii/Deviations_from_standards_(SE) "wikilink")

How to evaluate
---------------

-   [Prerequisites]({{ site.baseurl }}/micro-os-plus/ii/Prerequisites "wikilink")
-   [How to run the unit tests]({{ site.baseurl }}/micro-os-plus/ii/How_to_test "wikilink")
-   [Eclipse (install and config)]({{ site.baseurl }}/micro-os-plus/ii/Eclipse_(install_and_config) "wikilink")
-   [Development environment]({{ site.baseurl }}/micro-os-plus/ii/Development_environment "wikilink")

Links & References
------------------

-   [Standards and style]({{ site.baseurl }}/micro-os-plus/ii/References#standards-and-style "wikilink")
-   [Embedded operating systems]({{ site.baseurl }}/micro-os-plus/ii/References#embedded-operating-systems "wikilink")
-   [C/C++ language libraries]({{ site.baseurl }}/micro-os-plus/ii/References#cc-language--libraries "wikilink")
-   [TCP/IP libraries]({{ site.baseurl }}/micro-os-plus/ii/References#tcpip-libraries "wikilink")
-   [Testing]({{ site.baseurl }}/micro-os-plus/ii/References#testing "wikilink")
-   [Multi-tasking related links]({{ site.baseurl }}/micro-os-plus/ii/References#multi-tasking-related-links "wikilink")
-   [Miscellaneous]({{ site.baseurl }}/micro-os-plus/ii/References#miscellaneous "wikilink")
-   [Books]({{ site.baseurl }}/micro-os-plus/ii/References#books "wikilink")
-   [References notes]({{ site.baseurl }}/micro-os-plus/ii/References_notes "wikilink")

Miscellaneous
-------------

-   [Support]({{ site.baseurl }}/micro-os-plus/ii/Support "wikilink")
-   [Project history]({{ site.baseurl }}/micro-os-plus/ii/Project_history "wikilink")

MediaWiki links
---------------

Consult the [User's Guide](http://meta.wikimedia.org/wiki/Help:Contents) for information on using the wiki software.

-   [Configuration settings list](http://www.mediawiki.org/wiki/Manual:Configuration_settings)
-   [MediaWiki FAQ](http://www.mediawiki.org/wiki/Manual:FAQ)
-   [MediaWiki release mailing list](https://lists.wikimedia.org/mailman/listinfo/mediawiki-announce)

Final thoughts
--------------

Although not required by the license, I would be very happy if you could drop me a notice after reading these pages and eventually downloading any sources.

Any comments/critics/suggestions/etc will be highly appreciated.

Enjoy,

Liviu Ionescu <ilg@livius.net>
