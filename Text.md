# The RDKit paper


# Overview and History

The original development of the RDKit was done within the company Rational Discovery for use in their software tools and internal research programs. The toolkit was released as an open-source project managed by GL in 2006. Since becoming open source, development of the RDKit has continued, with regularly scheduled releases (currently twice a year) and contributions coming from the Novartis Institutes for Biomedical Research (NIBR) as well as other members of the continuously growing RDKit community.

## License

The RDKit is released under the terms of the BSD license. This is one of the more permissive of the standard open-source licenses and allows use of the toolkit in both open- and closed-source projects. This openness to commercial usage is a key principle of the project. At the time of its inception no open-source cheminformatics packages were available that would have allowed the original RDKit authors to use them in proprietary software. By releasing the RDKit under the BSD license, we have changed that. 

## Details

The central website is http://www.rdkit.org
The source code is managed using the git version control system (https://git-scm.com/) and hosted in GitHub: https://github.com/rdkit/rdkit. Standard GitHub functionality is used to manage releases and track bugs and feature requests. In order to facilitate citation of specific versions of the toolkit, a DOI is automatically assigned to each RDKit release using an integration with the Zenodo service (https://zenodo.org). For example, the DOI http://doi.org/10.5281/zenodo.556226 corresponds to the 2017.03.1 release of the RDKit.

Stats on code size, including comments and tests: ~245K lines of C++, ~100K lines of Python

## Basics

The core data structures and algorithms of the RDKit are written in standard C++, making heavy use of C++'s Standard Template Library (STL) and the Boost libraries (https://www.boost.org). The use of C++ for the core gives us both performance and tremendous flexibility: it is straightforward to provide access to the functionality from other programming languages. As of this writing, the core RDKit is usable from the programming languages Python, Java, and C#. It can also be used from within the relational database PostgreSQL and the data-mining tool KNIME; these integrations will be discussed in more detail below. New algorithms are often prototyped and tested in Python, a very productive language, and then later moved into the C++ core. The extra work associated with the translation is normally justified both by performance gains and by the ability we gain to use the new functionality in other languages and systems.

The RDKit code base is covered by extensive unit and regression tests. These tests provide a safety net of sorts when moving to a new system or compiler or when refactoring existing code. Use of the GitHub integrations with the Travis CI (https://travis-ci.org/) and Appveyor (https://www.appveyor.com/) services provides continuous integration support: each code change in GitHub triggers a re-build of the toolkit and the running of the tests. We expect the master version of the RDKit to always pass its tests and be useable (though not in a production setting), so these builds should always succeed and the tests should always pass. XXX coverage analysis?

API documentation is provided using doxygen (http://doxygen.org) for the C++ and epydoc (http://epydoc.sourceforge.net/) for the python code. Additionally, the toolkit is distributed with an sizeable "getting started" guide for use from Python (http://rdkit.org/docs/GettingStartedInPython.html), a "theory manual" documenting things like the aromaticity model and extensions to standard file formats (http://rdkit.org/docs/RDKit_Book.html), and a "cookbook" providing recipes for accomplishing common tasks with the RDKit (http://rdkit.org/docs/Cookbook.html). The documentation is built using Python's excellent Sphinx system (http://www.sphinx-doc.org/) and the code samples in the documentation are part of the test suite. This helps prevent the "doc rot" problem that often afflicts software documentation where the documentation is not updated at the same time the code itself is.

One feature that distinguishes the RDKit from most other cheminformatics toolkits is its strong emphasis on chemical correctness. By default, the molecule input functions apply a "sanitization" step to the molecules. Inputs that cannot be represented with electron octets on main-group elements are rejected with a sanitization error. This can, of course, be disabled, but the default behavior helps ensure that the molecules being used in further calculations are chemically reasonable.

## Releases

A major version of the RDKit is released every six months: one release in March/April and the second in September/October. In between major releases, bugfix versions are released about every month. These minor releases contain no new functionality and are provided to allow RDKit users to easily stay up to date with bug fixes without having to wait six months for a major release or take the risk of running non-released code.

To make things easier for end users, we also produce binary releases of the RDKit. These are distributed using the conda package manager, part of the anaconda Python distribution (https://anaconda.org/anaconda/python). At the time of this writing (February 2018) for each RDKit release we provide binaries supporting Python versions 2.7, 3.5, and 3.6 for Linux, Windows (32 bit and 64 bit), and MacOS. Together with other scientific software packages (http://python3statement.org/), the RDKit will be deprecating support for Python 2.7 starting in 2018 with the goal of completely removing support for this unsupported legacy Python version by 2020. The plan for how this slow deprecation will be carried out has been announced on the rdkit-discuss mailing list (XXX URL).

Packages to allow easy installation of the RDKit on major Linux distributions like Debian, Ubuntu, Fedora, CentOS, and RedHat Enterprise Linux are also provided by the RDKit community. Due to the nature of the Linux distributions, these tend to lag behind the primary RDKit version.

## Functionality overview

The RDKit provides a very broad range of cheminformatics functionality. The documentation includes a comprehensive list, here we just provide a brief summary of functionality that is either particularly important or where the RDKit provides unique capabilities.

- A broad selection of chemical fingerprints for chemical similarity and substructure screening.  Many of the fingerprints are available as both bit vectors or count vectors.
- A collection of descriptors useful for building machine learning /QSAR models (http://rdkit.org/docs/GettingStartedInPython.html#list-of-available-descriptors).
- A multi-molecule maximum common substructure (MCS) implementation derived from the fmcs algorithm (https://doi.org/10.1186/1758-2946-5-S1-O6) that supports fuzzy MCS.
- Support for chemical reactions, including applying reactions to generate virtual molecules, searching sets of reactions, and generating fingerprints for reactions. (https://doi.org/10.1021/ci5006614 https://doi.org/10.1021/acs.jcim.6b00564)
- Conformation generation using a distance geometry algorithm that can optionally include information derived from experimental crystal structures (https://doi.org/10.1021/acs.jcim.5b00654). This has been shown to be quite effective at providing diverse conformations and reproducing crystal structures (https://doi.org/10.1021/acs.jcim.6b00613 https://doi.org/10.1021/acs.jcim.7b00221).
- Force field optimization using UFF (https://doi.org/10.1021/ja00051a040) or MMFF94/MMFF94S (https://doi.org/10.1186/s13321-014-0037-3). The force field implementation includes a number of flexible constraints.
- Unsupervised molecule-molecule alignment using the Open3DAlign algorithm (https://doi.org/10.1007/s10822-011-9462-9).
- Integration with the Pandas library (https://pandas.pydata.org) for working with tabular data and Jupyter interactive notebook (https://jupyter.org).  This integration enables the display of molecules and chemical reactions in Jupyter and substructure and similarity searching in Pandas data frames.

## Contrib

In order to provide an easy distribution mechanism for smaller projects from the community, the RDKit distribution includes a Contrib directory, the "Contrib dir".  These contributions, often derived from the supplementary material of publications, are not reviewed or tested as carefully as the RDKit core code, but still provide useful tools or pieces of functionality that can be integrated into other software.

## KNIME nodes

An RDKit sub-project provides a set of open-source nodes for the open-source KNIME Analytics Platform (http://www.knime.org). These nodes, developed as a collaboration between the Novartis Institutes for BioMedical Research and KNIME.com (https://files.knime.com/sites/default/files/inline-images/01_greg_landrum.pdf) provide a broad spectrum of chemical functionality for use in KNIME workflows. This includes descriptor calculation, substructure search/filtering, chemical reactions, conformation generation, maximum common substructure, etc. The RDKit nodes also take advantage of KNIME's extension point architecture to provide chemical aggregation options (MCS-based) in the "Group By" node, easy usage of RDKit molecules and reactions from within KNIME's Python integration, and access to the full capabilities of the RDKit Java wrappers from within the "Java Snippet" nodes.
The RDKit KNIME nodes are part of KNIME's "trusted community" extensions (https://www.knime.com/trusted-community-contributions) and are available as a default installation option with the KNIME distribution.
The core RDKit library is also used to provide chemical functionality within other KNIME community projects, including the Vernalis nodes (https://www.knime.com/book/vernalis-nodes-for-knime-trusted-extension), the Erlwood nodes (https://www.knime.com/community/erlwood), and the 3D-E-Chem nodes (https://www.knime.com/3d-e-chem-nodes-for-knime https://pubs.acs.org/doi/abs/10.1021/acs.jcim.6b00686)

## PostgreSQL cartridge

The RDKit provides "cartridge" functionality that adds chemical data types and operations to PostgreSQL, a widely used and very extensible open-source relational database (https://www.postgresql.org/). The cartridge extends PostgreSQL to add data types for molecules, reactions, and both bit-vector and count-based fingerprints as well as functions for a variety of cheminformatics tasks like substructure searching, calculating common descriptors, generating molecular and reaction fingerprints, and generating molecular depictions as SVG . The new data types, when possible, integrate with PostgreSQL's indexing system to speed up substructure and reaction searching as well as similarity searches.

# The RDKit community
The user and developer community around an open source toolkit is an integral part of its adoption and success.  Ways in which an open-source community contributes include providing code, documentation, examples, support for other users, bug reports and feature requests, and advocacy.

The RDKit community is linked electronically via:
- the mailing lists: https://sourceforge.net/p/rdkit/mailman/
- the GitHub project: https://github.com/rdkit
- a LinkedIn group: https://www.linkedin.com/groups/8192558
- the RDKit blog: https://rdkit.blogspot.com
- the RDKit twitter account @RDKit_org
- a Slack channel (invitation only due to the nature of Slack)

In the real world there are occasional informal gatherings, such as MeetUps, and annual RDKit User Group Meetings (UGMs) - 2017 marked the 6th UGM.

# Use in other software
The RDKit is primarily a toolkit, it is intended to provide cheminformatics functionality for use in other software. Because registration is not necessary to use the toolkit we don't have a complete picture of everywhere it is being used, but here are some recent examples we know of:

## Open Source
- [Samson Connect](https://www.samson-connect.net) - Software for adaptive modeling and simulation of nanosystems
- [mol_frame](https://github.com/apahl/mol_frame) - Chemical Structure Handling for Dask and Pandas DataFrames
- [RDKitjs](https://github.com/cheminfo/RDKitjs) - port of RDKit functionality to JavaScript
- [DeepChem](https://deepchem.io) - python library for deep learning for chemistry
- [mmpdb](https://github.com/rdkit/mmpdb) - Matched molecular pair database generation and analysis
- [CheTo](https://github.com/rdkit/CheTo) ([paper](http://pubs.acs.org/doi/10.1021/acs.jcim.7b00249))- Chemical topic modeling
- [OCEAN](https://github.com/rdkit/OCEAN) ([paper](http://pubs.acs.org/doi/abs/10.1021/acs.jcim.6b00067))- Optimized cross reactivity estimation
- [ChEMBL Beaker](https://github.com/mnowotka/chembl_beaker) - standalone web server wrapper for RDKit and OSRA
- [myChEMBL](https://github.com/chembl/mychembl) ([blog post](http://chembl.blogspot.de/2013/10/chembl-virtual-machine-aka-mychembl.html), [paper](http://bioinformatics.oxfordjournals.org/content/early/2013/11/20/bioinformatics.btt666)) - A virtual machine implementation of open data and cheminformatics tools
- [ZINC](http://zinc15.docking.org) - Free database of commercially-available compounds for virtual screening
- [Coot](https://www2.mrc-lmb.cam.ac.uk/personal/pemsley/coot/) - software for macromolecular model building, model completion and validation
- [PYPL](http://www.biochemfusion.com/downloads/#OracleUtilities) - Simple cartridge that lets you call Python scripts from Oracle PL/SQL.
- [shape-it-rdkit](https://github.com/jandom/shape-it-rdkit) - Gaussian molecular overlap code shape-it (from silicos it) ported to RDKit backend
- [WONKA](http://wonka.sgc.ox.ac.uk/WONKA/) - Tool for analysis and interrogation of protein-ligand crystal structures
- [OOMMPPAA](http://oommppaa.sgc.ox.ac.uk/OOMMPPAA/) - Tool for directed synthesis and data analysis based8 on protein-ligand crystal structures
- [DeepChem](http://deepchem.io/) - deep learning toolkit for drug discovery
- [RRDKit](https://github.com/pauca/RRDKit) - RDKit integration for R
- [chemfp](http://chemfp.com)
- [rdkit_ipynb_tools](https://github.com/apahl/rdkit_ipynb_tools) - RDKit Tools for the jupyter Notebook
- [chemicalite](https://github.com/rvianello/chemicalite) - SQLite integration for the RDKit
- [django-rdkit](https://github.com/rdkit/django-rdkit)

## Closed Source/Commercial
T5 Informatics, Cresset, Schroedinger, ADF. Need text from them? Just mention?

# Support

Aside from the usual "use your favorite search engine to find a solution/answer" the primary support mechanisms for the RDKit asking questions on either one of the mailing lists or using the GitHub issue tracker. The RDKit community is both friendly and responsive and most questions are answered within a few days. For groups where this isn't sufficient or who would like to be able to get support in a non-public forum, commercial support contracts are offered by T5 Informatics GmbH (http://www.t5informatics.com).

# Polite request to users of open-source software

Whenever possible please try to let the authors and community of the open-source software you use know that you are using it. Feedback from users is great for motivation and can help authors justify ongoing contributions. This may take the form of a citation in a paper, a post to a mailing list, or maybe just a personal email if the others don't work.

# Copyright
Copyright (C) 2017-2018 Greg Landrum

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).
