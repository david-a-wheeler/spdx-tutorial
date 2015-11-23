# SPDX Tutorial

Software Package Data Exchange (SPDX) is a standard format for communicating the components, licenses,
and copyrights associated with software packages.
SPDX is easy to use for common cases,
and SPDX makes it easy for all software recipients to know what they're allowed to do (and not do).

This is a brief tutorial about SPDX.
To use SPDX you need to understand three basic constructs: SPDX license identifiers,
SPDX license expressions, and SPDX files.
This tutorial will also discuss how to use SPDX license expressions in source code files, and how to
get more information.

## SPDX license identifiers

SPDX license identifiers are the heart of SPDX.
A license identifier is a human readable short text string that uniquely identifies a license.
Here are some SPDX license identifiers (in alphabetical order) for some widely-used
[free](http://www.gnu.org/philosophy/free-sw.en.html)/libre/[open source software](https://opensource.org/osd-annotated)
licenses, followed by what the identifiers mean:

* AGPL-3.0 : [GNU Affero General Public License v3.0](https://spdx.org/licenses/AGPL-3.0.html)
* Apache-2.0 : [Apache License 2.0](https://spdx.org/licenses/Apache-2.0.html)
* BSD-2-Clause : [BSD 2-clause "Simplified" License](https://spdx.org/licenses/BSD-2-Clause.html)
* BSD-3-Clause :  [BSD 3-clause "New" or "Revised" License](https://spdx.org/licenses/BSD-3-Clause.html)
* GPL-2.0 : [GNU General Public License version 2.0 only](https://spdx.org/licenses/GPL-2.0.html)
* GPL-3.0 : [GNU General Public License version 3.0 only](https://spdx.org/licenses/GPL-3.0.html)
* LGPL-2.1 : [GNU Lesser General Public License v2.1 only](https://spdx.org/licenses/LGPL-2.1.html)
* LGPL-3.0 : [GNU Lesser General Public License v3.0 only](https://spdx.org/licenses/LGPL-3.0.html)
* MIT : [The MIT license](https://spdx.org/licenses/MIT.html), aka the expat license;
  note that this is different from the [X11](https://spdx.org/licenses/X11.html) license.

Similarly, here are some [free cultural works](http://creativecommons.org/freeworks) licenses
(typically used for works other than software):

* CC-BY-3.0 : [Creative Commons Attribution 3.0](https://spdx.org/licenses/CC-BY-3.0.html)
* CC-BY-4.0 : [Creative Commons Attribution 4.0](https://spdx.org/licenses/CC-BY-4.0.html)
* CC-BY-SA-4.0 : [Creative Commons Attribution Share Alike 4.0](https://spdx.org/licenses/CC-BY-SA-4.0.html)
* CC0-1.0 : [Creative Commons Zero v1.0 Universal](https://spdx.org/licenses/CC0-1.0.html)

SPDX license identifiers never contain a space.  SPDX licenses use case-insensitive matching, however, people are encouraged to provide and display the mixed-case forms where reasonable.  In URLs people are encouraged to support both mixed-case and lower-case forms; if that's impractical, convert to lower case (to support case-insensitive matching).

The [SPDX license list](https://spdx.org/licenses/) has a much more complete list of SPDX license identifiers, and is occasionally updated with new ones.
Not all licenses in the list are open source software (OSS) licenses, but the list does include the most popular OSS licenses.
As of September 2015 the SPDX license list defines standard license identifiers for over 300 licenses. 

## SPDX license expressions

In some situations there isn't just one license, e.g., software might be offered with a choice between two or more licenses.
SPDX version 2.0 added support for SPDX *license expressions*, which let you clearly express this when you need to.

A SPDX license expression can be simple SPDX license identifier or a SPDX "user defined license reference" (aka LicenseRef).
A LicenseRef can be used if a license isn't already defined by a standard SPDX license identifier.
We won't go into *how* you do that in this tutorial... but it's good to know you *can* do it.

SPDX license expressions also support a "+" suffix after a license identifier; the "+" means
"this license or any later version".  For example, you can express "GNU General Public License (GPL) version 2.0 or later"
using the SPDX license expression "GPL-2.0+".

You can also use three different operators to combine license expressions to create other license expressions:

* "OR": Recipients can choose between two or more licenses.
  For example, "(LGPL-2.1 OR MIT OR BSD-3-Clause)" means recipients can choose between any of those three licenses.
* "AND": Recipients are required to simultaneously comply with two or more licenses.
  For example, "(LGPL-2.1 AND MIT AND BSD-2-Clause)" means recipients must comply with all three licenses.
* "WITH": Add the following exception(s).  For example, "(GPL-3.0+ WITH Classpath-exception-2.0)"
  means recipients must comply with the GPL version 3.0 or later with the Classpath 2.0 license exception.
  See the [SPDX license exceptions list](https://spdx.org/licenses/exceptions-index.html) for the list of
  standard names for license exceptions.

There is an order of operations: "+", then "WITH", then "AND", then "OR".
This order can be overridden by parentheses, and if it's complex parentheses are good for clarity.
Any license expression that consists of more than one license identifier and/or LicenseRef should be 
encapsulated by parentheses.
SPDX expressions are case-insensitive, but by convention the operation names should be capitalized.

To me, license identifiers and license expressions are the real reasons to use SPDX.
However, both are less useful unless you can *put* their information somewhere.
So let's talk about two ways to do that: SPDX files and embedding license information in source files.

## SPDX Files

SPDX files are a way to capture and exchange license information.
The SPDX specification actually supports two formats: tag:value and RDF/XML.
They can do the same thing, but tag:value is easier to write, so that's what we'll use here.
A tag:value file is normally a sequence of lines, where each line is a tag name, a colon, a space, and its value.

There are many tags defined in the SPDX specification, but many of them may not apply to your specific situation.
Note that unlike license expressions, tag names are case-sensitive.
A few especially important or useful tags are:

* SPDXVersion: The version of the spec used, normally "SPDX-2.0".
* DataLicense: The license for the license data itself (!); normally this is "CC0-1.0".
  Note that this is *not* the license for the software or data being packaged.
* Creator: Who or what created this SPDX file (not the package creator).  This is in one of 3 formats:
    - For a person: person name, optionally followed by email in parentheses.
    - For an organization: organization name, optionally followed by email in parentheses.
    - For a tool: toolidentifier-version
* PackageName: The full name of the package as given by Package Originator.
* PackageOriginator: The person or organization from whom the package originally came. 
* PackageVersion: The version number of this particular version of the package (optional).
* PackageHomePage: The package's home page URL.
* PackageLicenseDeclared: The license identified in text in one or more files (for example a COPYING or LICENSE file)
  in the source code package.  This field is not intended to capture license information
  obtained from an external source, such as the package website.
   
For example, a SPDX file with the following lines states the following:
this package uses SPDX specification version 2.0 (the current version),
the license information can be shared with everyone,
it describes the Foo package created by David A. Wheeler, it has the given project home page, and
the package maintainers declare that all the software in this package is released using the MIT license:

    SPDXVersion: SPDX-2.0
    DataLicense: CC0-1.0
    PackageName: Foo
    PackageOriginator: David A. Wheeler
    PackageHomePage: https://github.com/david-a-wheeler/spdx-tutorial/
    PackageLicenseDeclared: MIT

There are many more tags you can use.  They are explained in the full
[SPDX specification](https://spdx.org/SPDX-specifications/spdx-version-2.0).

Now, a personal note.
Many of the tags and features in the SPDX file specification
are useful only when exchanging SPDX files that are isolated from their packages
so that you can describe large collections of packages.
In particular, the specification officially considers many tags mandatory, but I personally would interpret them only as
"mandatory" if you are trying to exchange SPDX files that are isolated from their packages.
If you are a package developer releasing software, and just want to include a short file that
describes the license you're using for just the software you're releasing, I suggest starting with the lines listed
in the example above (emphasizing PackageLicenseDeclared).
In those cases you don't really need tags like the "Created" datetime stamp (your version control system does that),
"DocumentName" (you can see what it is), "PackageDownloadLocation" (that changes all the time), and so on.

However, if you're exchanging larger files about many software components,
particularly if you're exchanging them without the software itself, then absolutely *do*
include all the mandatory tags.
Organizations that have to deal with many components and licenses
need that additional information when they do legal review, and that's what those tags are for.
If you are in that more complicated circumstance, see the SPDX site for information about tools that can help you.

The SPDX specification doesn't specify a file extension or file naming convention.
I personally recommend using ".spdx" for a SDPX file in tag:value format (as described here),
and using ".rdf" for a SDPX file in RDF+XML format.
When combining SDPX files together people often use the project name as the filename.
Personally, I think having a file named
"LICENSE.spdx" at the top level would be perfectly reasonable when including a SPDX file inside a single project.
To my knowledge SPDX (in either format) has not yet been formally registered as a MIME type.

If you're developing software or other copyrightable content, you still need to select a license and express
it in a way humans can understand.  For software, create a file named LICENSE or COPYING (possibly with a .md or
.txt extension) to provide human-readable text of the license.  Also create a SPDX file, as described above,
so programs can automatically process exactly what the license is, and humans can have a short and precise way
of representing the license of the software.

## SPDX license expressions in source code files

You can also refer to SPDX license expressions from within source code.
While embedding a copyright or license statement isn't necessary in copyright law,
there are advantages to including them.
For example, they make it easier for people to determine what they're allowed to do, and who to contact
if they need more... even if the file was quietly copied untracked to a different project.
The traditional way is annoying; it involves
embedding large quantities of legalese in your source code headers.
SPDX can help, because it can replace many lines of legal mumbo-jumbo, in every file, with a simple clear line of text.
I think best practice is to put it right under a
[simple copyright notice](http://ben.balter.com/2015/06/03/copyright-notices-for-websites-and-open-source-projects/).

I recommend using a case-sensitive tag "SPDX-License-Expression" in a comment near the top of the file,
followed by the SPDX license expression.
For example, in a programming language that uses "#" as the
rest-of-line comment character, use this:

    # Copyright [year project started] - [current year], [project founder] and the [project name] contributors
    # SPDX-License-Expression: [SPDX license expression]

This should be interpreted using the version of SPDX published when the license expression line
was added (or modified, if this line was modified later).
Modern version control software, like git, can easily provide this information
(in git use "git blame").

[Some](http://esr.ibiblio.org/?p=6867)
developers, and the [SPDX website](http://wiki.spdx.org/view/Technical_Team/SPDX_Meta_Tags),
recommend using "SPDX-License-Identifier:" instead of "SPDX-License-Expression:".
I find that prefix odd,
because I think the intent is that these be arbitrary SPDX license expressions (not just license identifiers).
I've contacted the SPDX maintainers to see if they really mean to use "SPDX-License-Identifier";
[stay tuned](https://bugs.linuxfoundation.org/show_bug.cgi?id=1330).

## License recommendations

I (David A. Wheeler) recommend that you primarily pick from one of the following SPDX license expressions,
since they are all very common and can be combined into larger works (they are
[GPL-compatible](http://www.dwheeler.com/essays/gpl-compatible.html) and
[mutually compatible](http://www.dwheeler.com/essays/floss-license-slide.html)):

* [MIT](https://spdx.org/licenses/MIT.html).
  This is a simple permissive license, useful if you want people to do whatever they want with the software;
  it provides some simple legal protections for developers.
* [Apache-2.0](https://spdx.org/licenses/Apache-2.0.html).
  This is a permissive license but includes an express grant of patent rights from contributors to users;
  this may be useful if you have concerns about patents in the project.
* [LGPL-2.1+](https://spdx.org/licenses/LGPL-2.0.html).
  This is a common weakly protective license, which ensures that those who get the executable of the
  software can also get the source code, but allows the software to be used in larger proprietary works.
* [GPL-2.0+](https://spdx.org/licenses/GPL-2.0.html).
  This is a common strongly protective license, which ensures that those who get the executable of the
  software can also get the source code.  Selecting "GPL-3.0+" is also a reasonable choice, but is incompatible
  with programs that are GPL-2.0 only.  GPL-2.0 only also has compatibility issues with
  [Apache-2.0](https://spdx.org/licenses/Apache-2.0.html).

The list above is extremely similar to the recommendations in GitHub's [ChooseALicense.com](http://choosealicense.com/).
If you're just completely confused and can't decide, start with [MIT](https://spdx.org/licenses/MIT.html);
you can release later versions (even if they include others' work) using a different license.

Most of the BSD licenses are perfectly reasonable as permissive licenses.
In particular, the licenses identified in SPDX as
[BSD-2-Clause (BSD 2-clause "Simplified" License)](https://spdx.org/licenses/BSD-2-Clause.html),
[BSD-3-Clause (BSD 3-clause "New" or "Revised" License)](https://spdx.org/licenses/BSD-3-Clause.html), and
[0BSD (BSD Zero Clause License)](https://spdx.org/licenses/0BSD.html)
are perfectly good permissive licenses (and the first two are widely used).
A historical problem with them is that there
are many different licenses all called the "BSD license", and at least one of them
(with SPDX license identifier "BSD-4-Clause") is obsolete, incompatible with many other licenses, and
in my opinion (and [others](http://www.gnu.org/philosophy/bsd.en.html))
it is often impractical to use at today's Internet scale due to its "obnoxious advertizing clause".
SPDX solves this problem of ambiguity; instead of saying "BSD license" (which is dangerously vague), you can use
a precise SPDX license expression.  SPDX can make licenses in this family much easier for everyone to use and understand.

I recommend that people *not* use [CC0-1.0](https://spdx.org/licenses/CC0-1.0.html) for software,
especially if you're not a government employee.
[CC0-1.0](https://spdx.org/licenses/CC0-1.0.html) essentially releases material to the public domain,
and thus fails to include disclaimers of warranty and merchantability.
These disclaimers provide a small amount of legal protection at no cost; you should use them
when releasing software.
Governments sometimes use CC0, but for them these legal protections are less important
(it's harder to sue governments).
I recommend using the [MIT](https://spdx.org/licenses/MIT.html) license instead for software.
Reasonable alternatives if you want *short* permissive software licenses include
[BSD-2-Clause (the BSD 2-clause "Simplified" License)](https://spdx.org/licenses/BSD-2-Clause.html),
[BSD-3-Clause (BSD 3-clause "New" or "Revised" License)](https://spdx.org/licenses/BSD-3-Clause.html),
[0BSD (BSD Zero Clause License)](https://spdx.org/licenses/0BSD.html), and
the [Unlicense](https://spdx.org/licenses/Unlicense.html).
The [CC0-1.0](https://spdx.org/licenses/CC0-1.0.html), [CC-BY-4.0](https://spdx.org/licenses/CC-BY-4.0.html),
and [CC-BY-SA-4.0](https://spdx.org/licenses/CC-BY-SA-4.0.html) licenses are just
fine for ordinary textual and artistic works.

Of course, different people have different opinions about what license to use, and licenses
are often difficult to change later.
Picking a license depends on your beliefs and goals for a particular project.
Indeed, the same person or organization is likely to
chose different licenses for different projects, depending on their goals for the project.
SPDX makes it possible to capture this licensing information in a precise and automated way.

## More information

For more information, visit the [SPDX website](http://spdx.org/).
Common destinations include the
[SPDX license list](https://spdx.org/licenses/) and the [SPDX specifications](http://spdx.org/spdx-specifications).

## About this tutorial

This tutorial was originally developed by [David A. Wheeler](http://www.dwheeler.com).
It is released under the Creative Commons Attribution License 3.0 Unported (CC-BY-3.0) license or later;
the 3.0 license is used for the SPDX specification.
In SPDX notation, this tutorial is released under "CC-BY-3.0+"; see the
[LICENSE.spdx](./LICENSE.spdx) file to see how we've expressed this.

Please propose changes (preferably as pull requests) at <https://github.com/david-a-wheeler/spdx-tutorial/>.
