# SPDX Tutorial

Software Package Data Exchange (SPDX) is a standard format for communicating the components, licenses,
and copyrights associated with software packages.
SPDX is easy to use, and SPDX makes it easy for all software recipients to know what they're allowed to do (and not do).
This is a brief tutorial about SPDX.
To use SPDX you need to understand three basic constructs: SPDX license identifiers, SPDX license expressions, and SPDX files.

## SPDX license identifiers

SPDX license identifiers are the heart of SPDX.  A license identifier is a human readable short text string that uniquely identifies a license.  Here are some SPDX license identifiers (in alphabetical order) for some widely-used free/libre/open source software licenses, and what they mean:

* Apache-2.0 : [Apache License 2.0](https://spdx.org/licenses/Apache-2.0.html)
* BSD-2-Clause : [BSD 2-clause "Simplified" License](https://spdx.org/licenses/BSD-2-Clause.html)
* BSD-3-Clause :  [BSD 3-clause "New" or "Revised" License](https://spdx.org/licenses/BSD-3-Clause.html)
* CC-BY-3.0 : [Creative Commons Attribution 3.0]()
* GPL-2.0 : [GNU General Public License version 2.0 only](https://spdx.org/licenses/GPL-2.0.html)
* GPL-3.0 : [GNU General Public License version 3.0 only](https://spdx.org/licenses/GPL-3.0.html)
* LGPL-3.0 : [GNU Lesser General Public License v3.0 only](https://spdx.org/licenses/LGPL-3.0.html)
* MIT : [The MIT license](https://spdx.org/licenses/MIT.html), aka the expat license.

SPDX license identifiers never contain a space.  SPDX licenses use case-insensitive matching, however, people are encouraged to provide and display the mixed-case forms where reasonable.  In URLs people are encouraged to support both mixed-case and lower-case forms; if that's impractical, convert to lower case (to support case-insensitive matching).

The [SPDX license list](https://spdx.org/licenses/) has a much more complete list of SPDX license identifiers, and is occasionally updated with new ones.  SPDX can also reference licenses not on the list, but in many cases that isn't necessary.


## SPDX license expressions

In many situations a single license can't express everything you need to know.

TODO: "+", WITH, OR, AND.

## SPDX Files

TODO: tag-value and XML. Minimal

## SPDX expressions in a source code file

TODO: Note a variant of ESR's convention.

## About this tutorial

This tutorial was originally developed by [David A. Wheeler](http://www.dwheeler.com), and is released under the Creative Commons Attribution License 3.0 Unported (CC-BY-3.0) license, the same license used for the SPDX specification.  In SPDX notation, this is released under CC-BY-3.0.
