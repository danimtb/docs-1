Component
=========

Abstraction over the different components a package may have inside, such as different libraries, executables, system dependencies...

Constructor
-----------

.. code-block:: python

    class Component(name, root_folder)

Parameters:
    - **name** (Required): Name of the component.
    - **root_folder** (Required): Root folder of the package so the absolute paths can be calculated.

Attributes
----------

name
++++

**Defaulted to**: Initialized in the constructor

Name of the component.

deps
++++

**Defaulted to**: ``[]``

List of names of the different components which this component depends on.

system_deps
+++++++++++

**Defaulted to**: ``[]``

List of names of system dependencies this component depends on.

includedirs
+++++++++++

**Defaulted to**: ``["include"]``

List of include directories for the component.

libdirs
+++++++

**Defaulted to**: ``["lib"]``

List of library directories for the component.

resdirs
+++++++

**Defaulted to**: ``["res"]``

List of resources directories for the component.

bindirs
+++++++

**Defaulted to**: ``["bin"]``

List of binary directories for the component.

builddirs
+++++++++

**Defaulted to**: ``[""]``

List of build directories for the component.

srcdirs
+++++++

**Defaulted to**: ``[]``

List of sources directories for the component.

defines
+++++++

**Defaulted to**: ``[]``

List of definitions for the component.

cflags
+++++++

**Defaulted to**: ``[]``

List of C compiler flags for the component.

cxxflags
++++++++

**Defaulted to**: ``[]``

List of C++ compiler flags for the component.

sharedlinkflags
+++++++++++++++

**Defaulted to**: ``[]``

List of linker flags  for shared libs of the component.

exelinkflags
++++++++++++

**Defaulted to**: ``[]``

List of linker flags for executables of the component.

lib
+++

**Defaulted to**: ``None``

Name of the library of the component (excludes usage of ``exe``).

exe
+++

**Defaulted to**: ``None``

Name of the executable of the component (excludes usage of ``lib``).

include_paths
+++++++++++++

**Read only**
**Defaulted to**: ``["<root_folder>/include"]``

List of absolute paths for include directories of the component. The absolute paths will be calculated from the values declared in
``includedirs`` attribute. If there are no contents inside the directory, it will be cleared from the list.

lib_paths
+++++++++

**Read only**
**Defaulted to**: ``["<root_folder>/lib"]``

List of absolute paths for library directories of the component. The absolute paths will be calculated from the values declared in
``libdirs`` attribute. If there are no contents inside the directory, it will be cleared from the list.

bin_paths
+++++++++

**Read only**
**Defaulted to**: ``["<root_folder>/bin"]``

List of absolute paths for binary directories of the component. The absolute paths will be calculated from the values declared in
``bindirs`` attribute. If there are no contents inside the directory, it will be cleared from the list.

build_paths
+++++++++++

**Read only**
**Defaulted to**: ``["<root_folder>/"]``

List of absolute paths for build scripts directories of the component. The absolute paths will be calculated from the values declared in
``builddirs`` attribute. If there are no contents inside the directory, it will be cleared from the list.

res_paths
+++++++++

**Read only**
**Defaulted to**: ``["<root_folder>/res"]``

List of absolute paths for build scripts directories of the component. The absolute paths will be calculated from the values declared in
``resdirs`` attribute. If there are no contents inside the directory, it will be cleared from the list.

src_paths
+++++++++

**Read only**
**Defaulted to**: ``[""]``

List of absolute paths for sources directories of the component. The absolute paths will be calculated from the values declared in
``srcdirs`` attribute. If there are no contents inside the directory, it will be cleared from the list.

Methods
-------

as_dict()
+++++++++

.. code-block

    def as_dict(self)

Returns the public contents of the object in a dictionary structure.
