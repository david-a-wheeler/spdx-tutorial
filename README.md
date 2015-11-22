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
* LGPL-3.0 : [GNU Lesser General Public License v3.0 only](https://spdx.org/licenses/LGPL-3.0.html)
* MIT : [The MIT license](https://spdx.org/licenses/MIT.html), aka the expat license.

Similarly, here are some [free cultural works](http://creativecommons.org/freeworks) licenses
(typically used for works other than software):

* CC-BY-3.0 : [Creative Commons Attribution 3.0](https://spdx.org/licenses/CC-BY-3.0.html)
* CC-BY-4.0 : [Creative Commons Attribution 4.0](https://spdx.org/licenses/CC-BY-4.0.html)
* CC-BY-SA-4.0 : [Creative Commons Attribution Share Alike 4.0](https://spdx.org/licenses/CC-BY-SA-4.0.html)
* CC0-1.0 : [Creative Commons Zero v1.0 Universal](https://spdx.org/licenses/CC0-1.0.html)

SPDX license identifiers never contain a space.  SPDX licenses use case-insensitive matching, however, people are encouraged to provide and display the mixed-case forms where reasonable.  In URLs people are encouraged to support both mixed-case and lower-case forms; if that's impractical, convert to lower case (to support case-insensitive matching).

The [SPDX license list](https://spdx.org/licenses/) has a much more complete list of SPDX license identifiers, and is occasionally updated with new ones. As of September 2015 it defines standard license identifiers for over 300 licenses. 

## SPDX license expressions

In some situations there isn't just one license, e.g., software might be offered with a choice between two or more licenses.
SPDX version 2.0 added support for SPDX *license expressions*, which let you clearly express this when you need to.

A SPDX license expression can be simple SPDX license identifier or a SPDX "user defined license reference" (aka LicenseRef).
A LicenseRef can be used if a license isn't already defined by a standard SPDX identifier.

SPDX license expressions also support a "+" suffix after a license identifier; the "+" means
"this license or any later version".  For example, you can express "GNU General Public License (GPL) version 2.0 or later"
using the SPDX license expression "GPL-2.0+".

You can also use three different operators to combine license expressions to create other license expressions:

* "OR": Recipients can choose between two or more licenses.
  For example, "(LGPL-2.1 OR MIT OR BSD-3-Clause)" means recipients can choose between any of those three licenses.
* "AND": Recipients are required to simultaneously comply with two or more licenses.
  For example, "(LGPL-2.1 AND MIT AND BSD-2-Clause)" means recipients must comply with all three licenses.
* "WITH": Add the collowing exception(s).  For example, "(GPL-3.0+ WITH Classpath-exception-2.0)"
  means recipients must comply with the GPL version 3.0 or later with the Classpath 2.0 license exception.
  See the [SPDX license exceptions list](https://spdx.org/licenses/exceptions-index.html) for the list of
  standard names for license exceptions.

There is an order of operations: "+", then "WITH", then "AND", then "OR".
This order can be overridden by parentheses, and if it's complex parentheses are good for clarity.
Any license expression that consists of more than one license identifier and/or LicenseRef should be 
encapsulated by parentheses.
SPDX expressions are case-insensitive, but by convention the operation names should be capitalized.

## SPDX Files

SPDX files are a way to capture and exchange license information.
The SPDX specification actually supports two formats: tag/value and RDF/XML.
They can do the same thing, but tag/value is easier to write, so that's what we'll use here.
A tag/value file is normally a sequence of lines, where each line is a tag name, a colon, a space, and its value.

There are many tags defined in the SPDX specification, but many of them may not apply to your specific situation.
Note that unlike license expressions, tag names are case-sensitive.
A few especially important tags are:

* SPDXVersion: The version of the spec used, normally "SPDX-2.0".
* DataLicense: The license for the license data itself (!); normally this is "CC0-1.0".
  Note that this is *not* the license for the software or data being packaged.
* Creator: Who or what created this SPDX file.  This is in one of 3 formats:
    - For a person: person name, optionally followed by email in parentheses.
    - For an organization: organization name, optionally followed by email in parentheses.
    - For a tool: toolidentifier-version
* PackageName: The full name of the package as given by Package Originator.
* PackageHomePage: The package's home page URL.
* PackageLicenseDeclared: The license identified in text in one or more files (for example a COPYING or LICENSE file)
  in the source code package.  This field is not intended to capture license information
  obtained from an external source, such as the package website.
   
For example, a SPDX file with the following lines states that this uses the SPDX 2.0 format (the current one),
the license information can be shared with everyone, this SPDX file was created by David A. Wheeler,
it describes the Foo package, and
the package maintainers declare that all the software in this package is released using the MIT license:

    SPDXVersion: SPDX-2.0
    DataLicense: CC0-1.0
    Creator: David A. Wheeler
    PackageName: Foo
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
describes the license you're using for just the software you're releasing, I suggest starting with the five lines listed
in the example above.
In those cases you don't really need tags like the "Created" datetime stamp (your version control system does that),
"DocumentName" (you can see what it is), "PackageDownloadLocation" (that changes all the time), and so on.

However, if you're exchanging larger files about many software components,
particularly if you're exchanging them without the software itself, then absolutely *do*
include all the mandatory tags. The mandatory tags we have not yet defined are
SPDXID, DocumentName, DocumentNamespace, Created, PackageDownloadLocation,
PackageVerificationCode, PackageLicenseConcluded, PackageLicenseInfoFromFiles, PackageCopyrightText,
FileName, FileChecksum, LicenseConcluded, LicenseInfoInFile, and FileCopyrightText.
Organizations that have to deal with many components and licenses
need that additional information when they do legal review, and that's what those tags are for.
If you are in that more complicated circumstance, see the SPDX site for information about tools that can help you.

The SPDX specification doesn't specify a file extension or file naming convention.
I personally recommend using ".spdx" for a SDPX file in tag-value format (as described here),
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

You can also refer to SPDX license expressions from source code, instead of trying to
embed large quantities of legalese in your source code headers.
The SPDX group have not endorsed a particular way to do this, however.
I recommend using a case-sensitive tag "SPDX-License-Expression" in a comment near the top of the file,
followed by the SPDX license expression.  For example, in a programming language that uses "#" as the
rest-of-line comment character, use this to express "this software is under GPL version 3.0 or later":

    # SPDX-License-Expression: GPL-3.0+

[Some](http://esr.ibiblio.org/?p=6867)
developers use "SPDX-License-Identifier" instead of "SPDX-License-Expression".
I'm glad they're using SPDX, but I think that particular tag name is a mistake,
because sometimes you want the full power of SPDX license expressions.
I think it'd be better if the name made it clear that full license expressions are allowed.

## License recommendations

I (David A. Wheeler) recommend that you primarily pick from one of the following SPDX license expressions,
since they are all very common and can be combined into larger works:

* MIT.  This is a simple permissive license, useful if you want people to do whatever they want with the software;
  it provides some simple legal protections for developers.
* Apache-2.0.  This is a permissive license but includes an express grant of patent rights from contributors to users;
  this may be useful if you have concerns about patents in the project.
* LGPL-2.0+.  This is a common weakly protective license, which ensures that those who get the executable of the
  software can also get the source code, but allows the software to be used in larger proprietary works.
* GPL-2.0+.  This is a common strongly protective license, which ensures that those who get the executable of the
  software can also get the source code.  Selecting "GPL-3.0+" is also a reasonable choice, but is incompatible
  with programs that are GPL-2.0 only.

The list above is extremely similar to the recommendations in GitHub's [ChooseALicense.com](http://choosealicense.com/).

Most of the BSD licenses are perfectly reasonable as permissive licenses.  A historical problem with them is that there
are many different licenses all called the "BSD license", and at least one of them
(with SPDX license identifier "BSD-4-Clause") is obsolete, incompatible with many other licenses, and
in my opinion often impractical to use at today's Internet scale due to its "obnoxious advertizing clause".
SPDX solves this problem of ambiguity; instead of saying "BSD license" (which is dangerously vague), you can use
precise SPDX license expressions.  For example, perfectly good BSD-style licenses include the
"BSD-3-Clause" and "BSD-2-Clause".  SPDX can make BSD-style licenses much easier for everyone to use and understand.

Of course, different people have different opinions about what license to use, and licenses
are often difficult to change later.
Picking a license depends on your beliefs and goals for a particular project.
Indeed, the same person is likely to
chose different licenses for different projects, depending on the goals of the project.
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
