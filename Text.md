# Overview and History

The original development of the RDKit was done within the company Rational Discovery for use in their software tools and internal research programs. The toolkit was released as an open-source project managed by GL in 2006. Since becoming open source, development of the RDKit has continued, with regularly scheduled releases (currently twice a year) and contributions coming from the Novartis Institutes for Biomedical Research (NIBR) as well as other members of the continuously growing RDKit community.

## License

The RDKit is released under the terms of the BSD license. This is one of the more permissive of the standard open-source licenses and allows use of the toolkit in both open- and closed-source projects. This openness to commercial usage is a key principle of the project. At the time of its inception no open-source cheminformatics packages were available that would have allowed the original RDKit authors to use them in proprietary software. By releasing the RDKit under the BSD license, we have changed that. 

## Details

The central website is http://www.rdkit.org
The source code is managed using the git version control system (xxx cite) and hosted in GitHub: https://github.com/rdkit/rdkit. Standard GitHub functionality is used to manage releases and track bugs and feature requests. In order to facilitate citation of specific versions of the toolkit, a DOI is automatically assigned to each RDKit release using an integration with the Zenodo service (https://zenodo.org). For example, the DOI http://doi.org/10.5281/zenodo.556226 corresponds to the 2017.03.1 release of the RDKit.

Stats on code size, including comments and tests: ~245K lines of C++, ~100K lines of Python

## Basics

The core data structures and algorithms of the RDKit are written in standard C++, making heavy use of C++'s Standard Template Library (STL) and the Boost libraries (https://www.boost.org). The use of C++ for the core gives us both performance and tremendous flexibility: it is straightforward to provide access to the functionality from other programming languages. As of this writing, the core RDKit is useable from the programming languages Python, Java, and C#. It can also be used from within the relational database PostgreSQL and the data-mining tool KNIME; these integrations will be discussed in more detail below. New algorithms are often prototyped and tested in Python, a very productive language, and then later moved into the C++ core. The extra work associated with the translation is normally justified both by performance gains and by the ability we gain to use the new functionality in other languages and systems.

The RDKit code base is covered by extensive unit- and regression-tests. These tests provide a safety net of sorts when moving to a new system or compiler or when refactoring existing code. Use of the GitHub integrations with the Travis CI (https://travis-ci.org/) and Appveyor (https://www.appveyor.com/) services provides continuous integration support: each code change in GitHub triggers a re-build of the toolkit and the running of the tests. We expect the master version of the RDKit to always pass its tests and be useable (though not in a production setting), so these builds should always succeed and the tests should always pass.

API documentation is provided using doxygen (http://doxygen.org) for the C++ and epydoc (http://epydoc.sourceforge.net/) for the python code. Additionally, the toolkit is distributed with an sizeable "getting started" guide for use from Python (http://rdkit.org/docs/GettingStartedInPython.html), a "theory manual" documenting things like the aromaticity model and extensions to standard file formats (http://rdkit.org/docs/RDKit_Book.html), and a "cookbook" providing recipes for accomplishing common tasks with the RDKit (http://rdkit.org/docs/Cookbook.html). The documentation is built using Python's excellent Sphinx system (http://www.sphinx-doc.org/) and the code samples in the documentation are part of the test suite. This helps prevent the "doc rot" problem that often afflicts software documentation where the documentation is not updated at the same time the code itself is.
One feature that distinguishes the RDKit from most other cheminformatics toolkits is its strong emphasis on chemical correctness. By default, the molecule input functions apply a "sanitization" step to the molecules. Inputs that cannot be represented with electron octets on main-group elements are rejected with a sanitization error. This can, of course, be disabled, but the default behavior helps ensure that the molecules being used in further calculations are chemically reasonable.

## Releases

A major version of the RDKit is released every six months: one release in March/April and the second in September/October. In between major releases bugfix versions are released about every month. These minor releases contain no new functionality and are provided to allow RDKit users to easily stay up to date with bug fixes without having to wait six months for a major release or take the risk of running non-released code.

## Functionality overview

The RDKit provides a very broad range of cheminformatics functionality. The documentation includes a comprehensive list, here we just provide a brief summary of functionality that is either particularly important or where the RDKit provides unique capabilities.

- Support for chemical reactions, including applying reactions to generate virtual molecules, searching sets of reactions, and generating fingerprints for reactions. (XXX cite Nadine)
- Conformation generation using a distance geometry algorithm that can optionally include information derived from experimental crystal structures. This has been shown to be quite effective at providing diverse conformations and reproducing crystal structures (XXX cite ETKDG paper and the Platinum paper).
- Force field optimization using UFF (XXX cite) or MMFF94/MMFF94S (XXX cite MMFF paper and Paolo's paper). The force field implementation includes an implementation of constraints.
-
Just highlights
- conformer generation with citations to JP, Sereina, and Johannes.
- reactions
- fingerprints
- mcs
- canonicalization?

## Contrib

In order to provide an easy distribution mechanism for smaller projects from the community, the RDKit distribution includes a Contrib directory, the "Contrib dir".  These contributions, often derived from the supplementary material of publications, are not reviewed or tested as carefully as the RDKit core code, but still provide useful tools or pieces of functionality that can be integrated into other software.

## KNIME nodes

An RDKit sub-project provides a set of open-source nodes for the open-source KNIME Analytics Platform (http://www.knime.org). These nodes, developed as a collaboration between the Novartis Institutes for BioMedical Research and KNIME.com (XXX cite UGM presentation) provide a broad spectrum of chemical functionality for use in KNIME workflows. This includes descriptor calculation, substructure search/filtering, chemical reactions, conformation generation, maximum common substructure, etc. The RDKit nodes also take advantage of KNIME's extension point architecture to provide chemical aggregation options (MCS-based) in the "Group By" node, easy usage of RDKit molecules and reactions from within KNIME's Python integration, and access to the full capabilities  of the RDKit Java wrappers from within the "Java Snippet" nodes.
The RDKit KNIME nodes are part of KNIME's "trusted community" nodes (XXX cite KNME community site) and are available as a default installation option with the KNIME distribution.
The core RDKit library is also used to provide chemical functionality within other KNIME community projects, including the Vernalis nodes (XXX cite) and the Erlwood nodes (XXX cite).

## PostgreSQL cartridge

The RDKit provides "cartridge" functionality that adds chemical data types and operations to PostgreSQL, a widely used and very extensible open-source relational database (XXX cite). The cartridge extends PostgreSQL to add data types for molecules, reactions, and both bit-vector and count-based fingerprints as well as functions for a variety of cheminformatics tasks like substructure searching, calculating common descriptors, generating molecular and reaction fingerprints, and generating molecular depictions as SVG . The new data types, when possible, integrate with PostgreSQL's indexing system to speed up substructure and reaction searching as well as similarity searches.

## don't forget

- JavaScript versions
- use in commercial software?




# The RDKit community
- mailing list
- LinkedIn group
- user group meetings
- contrib dir
- use in other projects
- tutorials

# Support

Aside from the usual "use your favorite search engine to find a solution/answer" the primary support mechanisms for the RDKit asking questions on either one of the mailing lists or using the GitHub issue tracker. The RDKit community is both friendly and responsive and most questions are answered within a few days. For groups where this isn't sufficient or who would like to be able to get support in a non-public forum, commercial support contracts are offered by T5 Informatics GmbH (http://www.t5informatics.com).

# Editorial comment to users of open-source software

Don't forget to let the authors and community of the software you use know that you are using it. Feedback from users is good for motivation and can help authors justify ongoing contributions. This may take the form of a citation in a paper, a post to a mailing list, or maybe just a personal email if the others don't work.
The database cartridge

Knime integration
	• Functionality

		• 2d

			• Supported formats for I/o
			• Vf2 for substructure
			• Transformations
			• Reaction handling
			• Chirality
			• Inchi integration
			• Depiction and constrained depiction 
			•
		• 3d

			• Distance-geometry based conformation generation
			• Uff implementation
			• MMFF94/s implementation
			• Constrained conformation generation
			• Cite JP's paper on performance
			• Rigid alignment to minimize rmsd
			• Shape scoring based on volume overlap
			•
		• Descriptors

			• Table with citations
		• Fingerprints

			• Morgan algorithm and similarities to the ECFP and FCFP
			• MACCS public keys
			• Atom pairs and topological torsions
			• Topological: rdkit, layered, layered2
			• Electro topological state (python only)
			• Most fingerprinters take optional atom invariants and/or allow limiting the atoms that contribute
			• Avalon Fp integration
		• Learning

			• Hierarchical clustering (include algorithms)
			• Bagged decision trees / random forests with truncated trees and tricks for handling unbalanced datasets
		• Misc

			• Information theory
			• Diversity picking
			•
		• Knime nodes

			• Molecule input and output
			• Substructure search, tagging, and highlighting
			• Fingerprinting
			• Descriptors
			• Diversity picking
			• Fragmentation
			• R group decomposition
			• Coordinate generation, 2d and 3d
			•
		• The cartridge

			• Molecule and fingerprint types
			• Index integration for fast substructure and similarity search
			• Descriptors
			• Inchi and inchi keys
	• Details

		• Code

			• Useage of boost
			• Lots of testing
			• Favor implementation in C++ because of the write once -  use everywhere benefit
		• Documentation

			• API docs for python and C++
			• Getting started in python doc
			• Cookbook
			• Cartridge overview
		• Web presence

			• Rdkit.org

				• Links to other sites
				• Documentation
				• Additional documents
		•
	• Community 

		• Contrib dir : place to collect small, self-containd contributions that are useful to the broader community. For example implementations of things from papers.
		• Contributions to core

			• MCS
			• MACCS keys and mdlvalence.h
		• Mailing lists
		• UGM
		• Other things out there:

			• Cinfony from Noel O'Boyle
			• Bisquit from the Dutch guys
			• USRCAT and the other thing from Adrian S.
			• Chemfp
			• AZ Orange

# Copyright
Copyright (C) 2017 Greg Landrum

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).
