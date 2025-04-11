			English README for package "oldrussian-ispell"

		Introduction

	The package "oldrussian-ispell" is a Russian language spelling
checker set (dictionary and affix file) implementing old (pre-1918) Russian
orthography. Since there are currently no software for working with old
Russian orthography, this package is a first attempt of this kind. It is
suitable for the program "ispell" or compatibles and is based on the package
"russian-ispell" by Alexander Lebedev (same version number) with changes
necessary to accomodate old Russian orthography. The package is under heavy
development and is distributed under GPL in the form of patches to the
package "rus-ispell". So far six books (total of 5 MB) were proofread using
this package and more testing and proofreading is needed.

		Installation of oldrussian-ispell

	The package is distributed as a patch to the existing package
"rus-ispell". The unmodified source of a compatible version of
"rus-ispell" is included into the distribution. Unpack the distribution
file "oldrus-ispell-XXX.tar.gz" to create a directory such as
"oldrus-ispell-XXX". Change to that directory and run "make unpack".
All necessary actions for unpacking and patching the "rus-ispell"
package will be performed automatically. If you don't have a free 40MB
of RAM on your computer, use the command "make LOWMEM=1 unpack".
	Then run "make dict" to build the dictionary with default options
(here you can also use LOWMEM=1). The dictionary hash files
"oldrussian.hash" and "oldrussian.aff" will be created and you can run
"make install" as root to install it system-wide.
	You can set various options by specifying variables for the "make"
command (run "make help" to list the available options). In this
version the following variables are supported:

	Variable	Its function
	
	YO=1		enable support of the letter "yo" (support is off by default)
	YOYE=1		enable side-by-side "yo" and "ye" (forces YO=1)
	NOFITA=1	remove support for "fita" (it is included by default)
	NOER=1		remove trailing "yer"'s (they are included by default)
	IZHITSA=1	require "izhitsa" (it is optional by default)
	CP1251=1	build dictionary in CP1251 encoding (KOI8-C by default)
	
	For example, if you run "make NOFITA=1 YOYE=1 dict", you will build a
version of the dictionary that uses the KOI8-C encoding, requires the ending
"yer"'s, does not have the letter "fita" and admits both "yo" and "ye"
spellings (e.g. both "vyodra" and "vedra" allowed).
	The letter "izhitsa" was seldom used, and therefore the default version of
the dictionary allows spelling variants with and without "izhitsa" (it was
replaced by the letter "i"). Specify "IZHITSA=1" to remove the no-izhitsa
spellings from the dictionary.
	The KOI8-C encoding is the default one, however the CP1251 variant is also
possible. Since the letters "yat'", "fita", "izhitsa" are not present in
CP1251, they are replaced by the Yugoslav letters "dye", "tshe" and the
Ukrainian "reverse e" which are graphically similar. The script
"koi8c2cp1251.sh" translates into this variant of the CP1251 encoding.
	If you wish to build several versions of the dictionary with different
options, you should run "make clean" before "make dict" every time, or else the
results may be incorrect.

		Other possibilities

	The Makefile supports several additional functions for dictionary work. Some of them rebuild the dictionary and therefore accept options such as NOER=1, YO=1 and so on.

	make unpack -- unpacking and patching (executed only once at the
very beginning). Accepts LOWMEM=1.
	make dict -- compile the dictionary (*.hash, *.aff). Requires about
30 MB of RAM and works slowly (optimizes the dictionary by removing
about 2000 redundant words). Accepts LOWMEM=1 and then requires only
about 5 MB of RAM and works quickly (but does not optimize the
dictionary).
	make install -- install the dictionary into /usr/lib/ispell/. Uses the result of "make dict" or accepts LOWMEM=1.
	make deb -- create a Debian package (uses results of a previous "make dict" or accepts LOWMEM=1 and other options)
	make dist -- create the patch distribution oldrus-ispell-XXX.tar.gz. Accepts LOWMEM=1.
	make distcheck -- check the patch distribution for correctness. Accepts LOWMEM=1.
	make hugelist -- create a full list of all word forms (the file
hugelist.koi). Uses results of "make dict" or accepts options.
	make dict.ez2 -- create a special packed list of word forms for the
package text_n2o.exe (accepts options)
	make text_n2o.exe -- update the archive text_n2o.exe (uses dict.ez2
or accepts options)
	make check_redundant -- create the list of redundant words without
flags (the file check_redundant). Accepts options.
	make check_dup -- create the list of repeated words (the file
check_dup). Accepts options.
	make sort -- sort all dictionaries again (useful after editing)
	make new-rus -- create an equivalent dictionary in contemporary
orthography (created in the subdirectory new-rus/, in which one can run
"make dict", "make hugelist" etc.)
	make check_new-rus -- prepare the list of words from rus-ispell
that are missing in the "new-rus" dictionary. Accepts LOWMEM=1.
	make clean -- delete all generated files (leave only sources)

		Additional programs

	In addition to the dictionary, the package "oldrus-ispell" includes some
auxiliary scripts: affixize, esq, excludelines, rus_new2old, rus_old2new,
showaffix, sortkoi8c, and update-dicts. The files esq, excludelines, rus_*, and
sortkoi8c are needed to compile the package from sources; the files affixize
and showaffix can be used to check the affix table; finally, the script
update-dicts helps to perform dictionary updates after A. Lebedev publishes
each update of the base package rus-ispell. Minimal documentation is included
in the scripts. These auxiliary scripts are not needed for the operation of
ispell with the compiled dictionaries.
	Another feature is a script for generating the new orthography dictionary
from the current version of the old orthography dictionary. This is useful for
comparison with the base dictionary package of A. Lebedev, because in our
version certain significant changes were made. Sources for this derivative
dictionary are generated in the directory "new-rus" by the command "make
new-rus". Compile the new dictionary the same way as the old one, using "make"
commands.
	The program "text_new2old.pl" can be used to help the conversion of texts
(word by word) from new orthography to old orthography. It needs a special
dictionary file "hugelistA.koi" which can be made by the command "make
hugelistA.koi". In case of several old orthography variants corresponding to
one new-orthography spelling, all variant spellings can be printed (usage
documented in program text). The program "text_new2old.pl" can be run also
under M$ Windows -- for this a self-extracting archive "text_n2o.exe" is
available.

		Differences from A. Lebedev's "rus-ispell"

	This package "oldrussian-ispell" differs from its parent package
"rus-ispell" in several aspects: support of the old orthography, expanded
possibilities to generate dictionary variants, and improved word coverage. The
latter follows from the paradigm I have adopted while working on this
dictionary: namely, word coverage should be increased whenever possible,
including rarely used forms, to aid spellchecking. In contrast, A. Lebedev
follows the opposite paradigm that to aid spellchecking the word coverage
should be restricted to the most frequently used word forms.

		The old Russian orthography in the 21st century, or How do I spell "Gulag"?

	A spelling dictionary of the old orthography must contain many words
introduced into the Russian language after 1917, such as "chekist" or "Gulag".
I followed the hypothetical natural development of the language: all new words
are spelled as if the reform of 1917 did not take place, according to the old
rules of word-building. For example, "yat'" is written in certain roots,
suffixes and endings, "fita" - only in words of Greek origin, and so on.

		Representation of the old Russian orthography

	The old Russian orthography differs from the modern one in several major
aspects: first, it uses the "obsolete" letters "Yat'", "Fita" and "I roman"
(which are encoded in the dictionary using the suggested "KOI8-C" charset);
second, it mandates a "Yer" ("hard sign") after trailing consonants; third, it
differs in some paradigms for nouns and adjectives. The letter "Yo" was rarely
used, partly because use of the letter "Yat'" resolves some important
ambiguities (e.g. "vsyo" -- "vse"), although its use was sometimes advocated.
Some texts use old orthography without trailing "Yer" or without "Fita".
Therefore the letters "Yo", "Fita" and the trailing "Yer" are made optional and
can be excluded from the dictionary at compile time.
	Both the main dictionary and the affix file of the modern Russian
spelling have to be modified to implement the old orthography. Note that the
new orthography is more streamlined (at the expense of obscuring the
morphology of the language) and it is much easier to convert from the old to
the modern orthography than in the opposite direction. The scripts
"rus_old2new.pl" and "rus_new2old.pl" attempt to do as much as can be done
automatically. The sorting script "sortkoi8c" was modified to include the
old letters.
	Since we need to use extra letters, only the KOI8-C character set is
supported. (See
"http://www.geocities.com/CapeCanaveral/Lab/5735/1/koi8-extended.html" for more
information about this proposed extension to KOI8-R encoding, including a
keyboard mapping and bitmapped fonts.) As Micro$oft has apparently no intention
to include the extra letters either in their character maps or in their Unicode
fonts, support of old orthography on DOS- and Windows-based machines (and by
extension under Macintosh) does not seem likely. (The letters Yat' and Fita are
present only in the Palatino font available in Windows 2000.) Under Unix,
however, typesetting of texts in old Russian orthography is already possible as
the latest LaTeX cyrillic fonts from Olga Lapko et al. fully support the old
Russian letters, and it is straightforward to create an input encoding for
KOI8-C. The package "xcyr" version 2.3 or newer (see above URL) provides a full
set of bitmapped fonts suitable for on-screen editing of texts in the old
orthography under X window system using the KOI8-C encoding. Compatibility with
existing software is achieved by simply relabeling these fonts as KOI8-R.

--
	Serge Winitzki (see above URL for contact information)
	January 2001.
