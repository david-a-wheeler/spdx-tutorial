# SPDX Tutorial

Software Package Data Exchange (SPDX) is a standard format
for communicating the components of software packages,
including information about them including licenses
copyrights.
It includes
several mechanisms that are especially useful for open source software.
SPDX is a formally-approved international standard
(as ISO/IEC 5962:2021), and its
[specification is freely available](https://spdx.dev/specifications/).
If you're developing open source software,
a developer who is considering the use of some open source software,
or someone who has to investigate if
software has acceptable licenses, SPDX may help.
SPDX is easy to use for common cases,
and SPDX makes it easy for all software recipients to know what
they're allowed to do (and not do).
SPDX was created by, and is maintained by, the Linux Foundation.
I recommend pronouncing SPDX as "speed-X"; some of its developers
pronounce each letter ("S-P-D-X").

[This is a brief tutorial about SPDX](#spdx-tutorial).
To use SPDX you need to understand three basic constructs:
[SPDX license identifiers](#spdx-license-identifiers),
[SPDX license expressions](#spdx-license-expressions), and [SPDX files](#spdx-files).
This tutorial will also discuss how to use
[SPDX license expressions in source code files](#spdx-license-expressions-in-source-code-files),
[license recommendations](#license-recommendations), and how to
[get more information](#more-information).

## SPDX license identifiers

SPDX license identifiers are the heart of SPDX.
A license identifier is a human readable short text string that uniquely identifies a license.
Here are some SPDX license identifiers (in alphabetical order) for some widely-used
[free](http://www.gnu.org/philosophy/free-sw.en.html)/libre/[open source software](https://opensource.org/osd-annotated)
licenses, followed by what the identifiers mean:

* AGPL-3.0-only : [GNU Affero General Public License v3.0](https://spdx.org/licenses/AGPL-3.0.html)
* Apache-2.0 : [Apache License 2.0](https://spdx.org/licenses/Apache-2.0.html)
* BSD-2-Clause : [BSD 2-clause "Simplified" License](https://spdx.org/licenses/BSD-2-Clause.html)
* BSD-3-Clause :  [BSD 3-clause "New" or "Revised" License](https://spdx.org/licenses/BSD-3-Clause.html)
* GPL-2.0-only : [GNU General Public License version 2.0 only](https://spdx.org/licenses/GPL-2.0-only.html)
* GPL-3.0-only : [GNU General Public License version 3.0 only](https://spdx.org/licenses/GPL-3.0-only.html)
* LGPL-2.1-only : [GNU Lesser General Public License v2.1 only](https://spdx.org/licenses/LGPL-2.1-only.html)
* LGPL-3.0-only : [GNU Lesser General Public License v3.0 only](https://spdx.org/licenses/LGPL-3.0-only.html)
* MIT : [The MIT license](https://spdx.org/licenses/MIT.html), aka the expat license;
  note that this is different from the [X11](https://spdx.org/licenses/X11.html) license.

Note that the `-only` suffix is used in the context of GPL only. At all other licenses without the suffix, it also means "only this version".

Similarly, here are some [free cultural works](http://creativecommons.org/freeworks) licenses
(typically used for works other than software):

* CC-BY-3.0 : [Creative Commons Attribution 3.0](https://spdx.org/licenses/CC-BY-3.0.html)
* CC-BY-4.0 : [Creative Commons Attribution 4.0](https://spdx.org/licenses/CC-BY-4.0.html)
* CC-BY-SA-4.0 : [Creative Commons Attribution Share Alike 4.0](https://spdx.org/licenses/CC-BY-SA-4.0.html)
* CC0-1.0 : [Creative Commons Zero v1.0 Universal](https://spdx.org/licenses/CC0-1.0.html)

SPDX license identifiers never contain a space.  SPDX licenses use case-insensitive matching, however, people are encouraged to provide and display the mixed-case forms where reasonable.  In URLs people are encouraged to support both mixed-case and lower-case forms; if that's impractical, convert to lower case (to support case-insensitive matching).

The [SPDX license list](https://spdx.org/licenses/) has a much more complete list of SPDX license identifiers, and is occasionally updated with new ones.
Not all licenses in the list are open source software (OSS) licenses, but the list does include the most popular OSS licenses.
As of September 2015 the SPDX license list defines
standard license identifiers for over 300 licenses. 

## SPDX license expressions

In some situations there isn't just one license, e.g., software might be
offered with a choice between two or more licenses.
SPDX version 2.0, released in 2015, added support for
SPDX *license expressions*, which let you clearly express choices
when you need to.

A SPDX license expression can be simple SPDX license identifier or a SPDX
"user defined license reference" (aka LicenseRef).
A LicenseRef can be used if a license isn't already defined by a standard
SPDX license identifier.
We won't go into *how* you do that in this tutorial... but it's good to
know you *can* do it.

SPDX license expressions also support a "+" suffix after a license
identifier; the "+" means "this license or any later version".
For example, you can express "Apache License 2.0 or later" using the SPDX license expression "Apache-2.0+".

There is a special case involving "+": GNU licenses (such as the GPL)
are handled specially ([since SPDX License List 3.0](https://spdx.org/news/news/2018/01/license-list-30-released)) and do not use the "+" any more.
For example, GPL-2.0-or-later means "GNU General Public License (GPL)
version 2.0 or later", instead of using "+".
The reason for this special case is
that the older expression "GPL-2.0" was not always used to mean
"only version 2", even though it should have been,
so that expression became ambiguous.
I suggest that if you know that software uses the GPL-2.0 license, but have
not determined if "or later" applies, you should
consider using the obsolete "GPL-2.0" marking.
On the other hand, if you're marking software you release yourself,
please provide a more precise license expression so that everyone
else will know exactly what they're allowed to do.

You can also use three different operators to combine license expressions
to create other license expressions:

* "OR": Recipients can choose between two or more licenses.
  For example, "(LGPL-2.1-only OR MIT OR BSD-3-Clause)" means recipients can choose between any of those three licenses.
* "AND": Recipients are required to simultaneously comply with two or more licenses.
  For example, "(LGPL-2.1-only AND MIT AND BSD-2-Clause)" means recipients must comply with all three licenses.
* "WITH": Add the following named exception.  For example, "(GPL-3.0-or-later WITH Classpath-exception-2.0)"
  means recipients must comply with the GPL version 3.0 or later with the Classpath 2.0 license exception.
  See the [SPDX license exceptions list](https://spdx.org/licenses/exceptions-index.html) for the list of
  standard names for license exceptions.

There is an order of operations: "+", then "WITH", then "AND", then "OR".
This order can be overridden by parentheses, and if it's complex
parentheses are good for clarity.
Any license expression that consists of more than one license identifier
and/or LicenseRef should be encapsulated by parentheses.
SPDX expressions are case-insensitive, but by convention the operation
names should be capitalized.

To me, license identifiers and license expressions
are the real reasons to use SPDX.
However, both are less useful unless you can *put* their information somewhere.

Most programming languages have a language library package manager,
and most systems have a system-level package manager;
each package manager has a package format.
In almost all of them there is a "license" field, and in most cases
you can simply use a SPDX license expression in that field.

There are also two other ways to capture this information:
SPDX files and embedding license information in source files.
Here I'll especially focus on how software developers can use them.

## SPDX Files

If you want to *generate* a full SPDX file that identifies all the
components in system, aka its "software bill of materials" (SBOM),
it's probably best to use a tool to generate SDPX files.
One example is [spdx-sbom-generator](https://github.com/spdx/spdx-sbom-generator))
That said, it's good to know what SPDX files contain, especially if
you want to do more than what existing tools do.

SPDX files are a way to capture and exchange information
about some software and its dependencies, such as license information,
in a way that is independent of the programming language(s) and package
manager(s) used.
The SPDX specification actually supports two formats: tag:value and RDF/XML.
They can do the same thing, but tag:value is easier to write, so that's
what we'll use here.
A tag:value file is normally a sequence of lines, where each line is a
tag name, a colon, a space, and its value.

There are many tags defined in the SPDX specification, but many of them may not apply to your specific situation.
Note that unlike license expressions, tag names are case-sensitive.
A few especially important or useful tags are:

* SPDXVersion: The version of the spec used, normally "SPDX-2.1".
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
this package uses SPDX specification version 2.1 (the current version),
the license information can be shared with everyone,
it describes the Foo package created by David A. Wheeler, it has the given project home page, and
the package maintainers declare that all the software in this package is released using the MIT license:

    SPDXVersion: SPDX-2.1
    DataLicense: CC0-1.0
    PackageName: Foo
    PackageOriginator: David A. Wheeler
    PackageHomePage: https://github.com/david-a-wheeler/spdx-tutorial/
    PackageLicenseDeclared: MIT

There are many more tags you can use.  They are explained in the full
[SPDX specification](https://spdx.org/specifications).

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
I personally recommend using ".spdx" for a SPDX file in tag:value format (as described here),
and using ".rdf" for a SPDX file in RDF+XML format
([this appears to be a best practice](https://bugs.linuxfoundation.org/show_bug.cgi?id=1329)).
When combining SPDX files together people often use the project name as the filename.
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
Embedding a copyright or license statement in every file
isn't required by copyright law, but
there are advantages to including them.
For example, they make it easier for people to determine what they're
allowed to do, and who to contact if they need more...
even if the file was quietly copied untracked to a different project.
The traditional way is annoying; it involves
embedding large quantities of legalese in your source code headers.
SPDX can help, because it can replace many lines of legal mumbo-jumbo, in every file, with a simple clear line of text.
I think best practice is to put it right under a simple copyright notice
(see [Ben Balter's article about copyright notices for open source projects](http://ben.balter.com/2015/06/03/copyright-notices-for-websites-and-open-source-projects/);
he focuses on project-wide notices, but the principles still apply).

The recommended way is to add a case-sensitive tag
"SPDX-License-Identifier: " in a comment near the top of the file,
followed by the SPDX license expression.
For example, in a programming language that uses "#" as the
rest-of-line comment character, use this:

    # Copyright [year file created] - [last year file modified], [project founder] and the [project name] contributors
    # SPDX-License-Identifier: [SPDX license expression]

This should be interpreted using the version of SPDX published
when the line was added (or modified, if this line was modified later).
Modern version control software, like git, can easily provide this information
(in git use "git blame").

Note that the prefix name is for a license identifier, but in fact
allows the more complex license expression.
This is a historical accident.
Older versions of SPDX only allowed license identifiers, so that's
the name in common use
(e.g., [here](http://esr.ibiblio.org/?p=6867)).
For backwards-compatibility, the SPDX folks have decided to
[keep the old name](https://bugs.linuxfoundation.org/show_bug.cgi?id=1330).
That's a little odd, but no big deal; the key is that there's a way
to provide the information.

## Recording license information in an OSS project

If you're developing open source software (OSS) you should *still*
include the full legal text of your license(s).
Include them at the top level of the project repository, with the
conventional all-upper-case filename *LICENSE* or *COPYING*
(optionally with a .txt or .md extension).

The full legal text is still important for humans to read.
If it's single simple license, great.  If it's complicated, you can
express all that complication as well.

What SPDX does is create a simple format for licenses that is
*machine-readable* (to help automation) and
*short* (to help humans quickly handle common cases).

## License recommendations

Make *sure* you include a license on any software you're releasing.
[Having no license does not mean you're opting out of copyright law](http://choosealicense.com/no-license/).
Generally speaking, around the world the absence of a license means that default copyright laws apply.
This normally means that nobody else may reproduce, distribute, or create derivative works from your work.
There are exceptions due to "Fair Use" (US) and "Fair Dealing" (Europe), but these are far weaker than you might think.
Before 1976 omitting a copyright statement meant there was no federal copyright in the US,
but that simply isn't true today.
[Simon Phipps' "Why all software needs a license"](http://www.infoworld.com/article/2839560/open-source-software/sticking-a-license-on-everything.html)
further explains why software needs a license.
The biggest common licensing mistake is failing to put a license on software at all.

Of course, different people have different opinions about what license to use, and licenses
are often difficult to change later.
Picking a license depends on your beliefs and goals for a particular project.
Indeed, the same person or organization may
chose different licenses for different projects, depending on their goals for the project.
SPDX makes it possible to capture this licensing information in a precise and automated way.

Since this is a personal essay, I can provide you with a few specific
recommendations - and why I recommend them.
That said, at this point these are personal recommendations,
I am not a lawyer, and I am not your lawyer... so treat them as such.

I (David A. Wheeler) recommend that you primarily pick from one of the following SPDX license expressions
when releasing open source software,
since all of these licenses are very common and can be combined into larger works (they are
[GPL-compatible](http://www.dwheeler.com/essays/gpl-compatible.html) and
[mutually compatible](http://www.dwheeler.com/essays/floss-license-slide.html)):

* [MIT](https://spdx.org/licenses/MIT.html).
  This is a simple permissive license, useful if you want people to do whatever they want with the software;
  it provides some simple legal protections for developers.
* [Apache-2.0](https://spdx.org/licenses/Apache-2.0.html).
  This is a permissive license but includes an express grant of patent rights from contributors to users;
  this may be useful if you have concerns about patents in the project.
* [LGPL-2.1-or-later](https://spdx.org/licenses/LGPL-2.1.html).
  This is a common weakly protective license, which ensures that those who get the executable of the
  software can also get the source code, but allows the software to be used in larger proprietary works.
  A plausible alternative for this purpose is [LGPL-3.0+](https://spdx.org/licenses/LGPL-3.0.html).
* [GPL-2.0-or-later](https://spdx.org/licenses/GPL-2.0.html).
  This is a common strongly protective license, which ensures that those who get the executable of the
  software can also get the source code.  Selecting "GPL-3.0-or-later" is also a reasonable choice, but is incompatible
  with programs that are GPL-2.0-only.  GPL-2.0-only also has compatibility issues with
  [Apache-2.0](https://spdx.org/licenses/Apache-2.0.html).

The list above is extremely similar to the recommendations in GitHub's [ChooseALicense.com](http://choosealicense.com/).
If you're just completely confused and can't decide, I recommend that you
start with [MIT](https://spdx.org/licenses/MIT.html);
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

The [CC0-1.0](https://spdx.org/licenses/CC0-1.0.html) license (aka "CC0") essentially releases the material to the public domain for purposes of only copyright.
The CC0 is generally fine for materials like documentation, but
I *strongly* recommend that you do *not* use CC0 to release software.
The CC0 is not approved as an open source software license by the open source
initiative (OSI), and the Free Software Foundation (FSF) warns that users be
cautious about software with this license.
It's *possible* to release software using the CC0,
but the CC0 creates a dangerous potential trap for software users.
Understanding the CC0 trap requires understanding the CC0.
The CC0 license states that as far as *copyright* laws go,
any recipient can do practically anything they want to do with it.
In jurisdictions where releasing to the public domain under copyright law isn't technically possible, the CC0 license does the best it can.
The CC0 also disclaims warranties (including warranties for fitness for a particular purpose) to the extent allowed under law (see [section 4(b)](https://creativecommons.org/publicdomain/zero/1.0/legalcode); my thanks to those who pointed me to this information).
There *are* some software projects (including those from the US government)
that are released under the CC0 license.
The problem, however, is that there are many laws that govern
software, *not* just copyright.
The CC0 license has *not* been approved by the Open Source Initiative
(OSI), however.
As the
[Creative Commons explains in their CC0 FAQ](https://wiki.creativecommons.org/wiki/CC0_FAQ#May_I_apply_CC0_to_computer_software.3F_If_so.2C_is_there_a_recommended_implementation.3F),
CC0 "expressly does *not* license or otherwise affect any
patent rights you may have".
I think patents should not apply to software (just like they don't apply to
books), but many jurisdictions allow this misuse.
As the
[FSF explains about CC0](https://www.gnu.org/licenses/license-list.html#CC0),
"For works of software [CC0] is not recommended, as CC0 has a term
expressly stating it does not grant you any patent licenses.
Because of this lack of patent grant, we encourage you to be careful
about using software under this license; you should first consider
whether the licensor might want to sue you for patent infringement.
If the developer is refusing users patent licenses, the program is
in effect a trap for users and users should avoid the program."
The CC0 also has a weaker defense against liability lawsuits
(it lacks a statement like
"in no event shall (authors or copyright holders) be held liable...").
Another license would be a far better choice,
As a result, I recommend using the [MIT](https://spdx.org/licenses/MIT.html) license instead of CC0 for software, as it's a small yet widely-used permissive
license.
The [MIT license expressly grants all necessary permissions, including copyright and patents](https://opensource.com/article/18/3/patent-grant-mit-license).
Reasonable alternatives if you want *short* permissive software licenses include
[BSD-2-Clause (the BSD 2-clause "Simplified" License)](https://spdx.org/licenses/BSD-2-Clause.html) and
[BSD-3-Clause (BSD 3-clause "New" or "Revised" License)](https://spdx.org/licenses/BSD-3-Clause.html),
and
[0BSD](https://opensource.org/license/0bsd/).
The [Apache License 2.0](https://spdx.org/licenses/Apache-2.0.html) is also
a reasonable choice as a permissive license, though it's longer and there are
some complications when combining it with GPL version 2.0.

[Creative Commons recommends not using Creative Commons licenses for software](https://wiki.creativecommons.org/wiki/Frequently_Asked_Questions#Can_I_apply_a_Creative_Commons_license_to_software.3F) other than the CC0 (and as noted above, even the CC0 has serious problems),
I agree with their assessment.
For example, their licenses do not address the issues of source code distribution or patents.
The [CC0-1.0](https://spdx.org/licenses/CC0-1.0.html), [CC-BY-4.0](https://spdx.org/licenses/CC-BY-4.0.html),
and [CC-BY-SA-4.0](https://spdx.org/licenses/CC-BY-SA-4.0.html) licenses are just
fine for other purposes, such as ordinary textual and artistic works.
SPDX does have license identifiers for the "no derivatives" and "non-commercial" Creative Commons licenses,
but note that by definition these are not OSS nor [free culture](https://creativecommons.org/freeworks) licenses.

I recommend in most cases *against* using the
[BSD-4-Clause (BSD 4-clause "Original" or "Old" License)](https://spdx.org/licenses/BSD-4-Clause.html) and
[EPL-1.0 (Eclipse Public License version 1.0)](https://spdx.org/licenses/EPL-1.0.html) licenses,
in part because they are not [GPL-compatible](http://www.dwheeler.com/essays/gpl-compatible.html).

You may disagree with my license recommendations, or a have a special circumstance.  That's okay.
SPDX won't decide a license for you, but SPDX does make it easy for everyone to know what license(s) were selected.

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

Please propose changes (preferably as pull requests) at
<https://github.com/david-a-wheeler/spdx-tutorial/#spdx-tutorial>.
