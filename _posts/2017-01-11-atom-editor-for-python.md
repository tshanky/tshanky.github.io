---
title: Atom editor for Python
author: tshanky
permalink: /2017/01/11/atom-editor-for-python/
modified:
excerpt: "Using the Atom editor for Python"
tags: []
---
Github’s [Atom](https://atom.io/) is a beautiful and customizable editor that can be leveraged gainfully for Python programming. Built on [Electron](http://electron.atom.io/), a technology for building cross platform desktop apps
with JavaScript, HTML, and CSS, Atom is, as Github says, “hackable”. Its support for plugins and add-ons via packages allows one to add customizations and newer capabilities beyond what comes out of the box.


## Atom Packages

The package system is at the heart of Atom’s design. Atom itself has a very lightweight core and most of its own default features, like the “Settings View”, are implemented via packages. The out of box comes with over 90 packages installed.


Newer packages can be searched and installed via the "Install" tab in the "Settings View". Look at Figure 1, which shows the "Install" tab.

<figure>
	<img src="/images/install_tab_in_the_settings_view.png"/>
	<figcaption>Figure 1: Atom Settings View Install Tab</figcaption>
</figure>

>These searchable Atom packages are published to the official Atom registry at [https://atom.io/packages](https://atom.io/packages).

### Installing using the command line

Atom comes with a command line utility named *apm*, short for atom package manager. Latest version of a package can be installed easily using the following command:

```
apm install <package_name>
```

This assumes you know the exact name of the package you want to install. If you are not sure of the exact name of the package then you can search for packages like so:

```
apm search <possible_package_name>
```

While installing the latest version of a package is desirable, you may need to install a specific older version. Its easy to install a specific version as follows:

```
apm install <package_name>@<package_version>
```

To view more information about a specific package, use:

```
apm view <package_name>
```

## Atom Packages for Python

PEP8 (short for Python Enhancement Proposal 8) is the style guide for Python code. Pythonic code is optimized for readability. Clean, explicit, and simple code structure, design, and layout is preferred over complexity. PEP257 specifies docstring conventions. Docstrings are string literals that appear as the first statement in a Python function, module, class, or method definition. It serves as code comments and is also accessible at runtime via the "__doc__" attribute. Atom's support for code linters can help you proactively check your code for errors and adherence to PEP8 and PEP257.

Linter is an Atom package that provides a unified and consistent api for all atom linter plugins. Start by installing this package:

```
apm install linter
```

Next, piggyback on the Python packages to do the heavy lifting. Install flake8, the wrapper around PyFlakes, pycodestyle, and Mc Cabe script. *pip* is the most popular and widely used package manager for Python and using it is very easy. To install flake8 and flake8-pep257, use pip as follows:

```
pip install flake8
pip install flake8-pep257
```
Now that flake8 is available on your system, install linter-flake8. The linter-flake8 package is a flake8 provider for linter. It makes it possible for Atom linter to leverage flake8 for checking Python code for style adherence. Install linter-flake8 like so:

```
apm install linter-flake8
```

## Automating Style Corrections

Linter with flake8's help surfaces style related errors and warnings but doesn't correct it for you. The python package autopep8 on the other hand autocorrects your errors. Install autopep8 via the command line as follows:

```
pip install autopep8
```
Formatting issues identified by PEP8 checkers are listed at [PEP8 Error Codes](https://pep8.readthedocs.io/en/latest/intro.html#error-codes). autopep8 is capable of fixing most of these. autopep8 can be used via the command line to fix errors like so:

```
autopep8 --in-place --aggressive --aggressive <filename>
```
Within an editor or a development environment its very convenient if the auto corrections are transparent and don't involve invoking utilities via the command line. A good point for auto correction if often when a source code file is saved after it is edited. Atom's atom-beautify package allows integration of autopep8 as the beautifier for Python code. It also offers the option to "Beautify on Save". Install atom-beautify as follows:

```
apm install atom-beautify
```
Look at Figure 2, which illustrates the configuration options for Python within atom-beautify settings.

<figure>
	<img src="/images/atom_beautify_configuration_options_for_pyton.png"/>
	<figcaption>Figure 2: atom-beautify configuration options for Python</figcaption>
</figure>
