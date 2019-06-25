CppInfo
========

Abstraction over the package information (libraries, executables, system dependencies...) declared to be used by the consumers of a
package. This infomration will be used by the generators in order to provide the right information to consumers.

Constructor
-----------

.. code-block:: python

    class CppInfo(root_folder)

Parameters:
    - **root_folder** (Required): Root folder of the package so the absolute paths can be calculated.

Attributes
----------

name
++++

**Defaulted to**: ``None``

Name of the package.

rootpath
++++++++

**Defaulted to**: Initialized in constructor with ``root_folder`` parameter.

Absolut path to the root folder of the package.

sysroot
+++++++

MISSING

includedirs
+++++++++++

**Defaulted to**: ``["include"]``

List of include directories for the package.

libdirs
+++++++

**Defaulted to**: ``["lib"]``

List of library directories for the package.

resdirs
+++++++

**Defaulted to**: ``["res"]``

List of resources directories for the package.

bindirs
+++++++

**Defaulted to**: ``["bin"]``

List of binary directories for the package.

builddirs
+++++++++

**Defaulted to**: ``[""]``

List of build directories for the package.

srcdirs
+++++++

**Defaulted to**: ``[]``

List of sources directories for the package.

defines
+++++++

**Defaulted to**: ``[]``

List of definitions for the package.

cflags
+++++++

**Defaulted to**: ``[]``

List of C compiler flags for the package.

cxxflags
++++++++

**Defaulted to**: ``[]``

List of C++ compiler flags for the package.

cppflags
++++++++

**[DEPRECATED]**: Use ``cxxflags`` instead.

**Defaulted to**: ``[]``

List of C++ compiler flags for the package.

sharedlinkflags
+++++++++++++++

**Defaulted to**: ``[]``

List of linker flags  for shared libs of the component.

exelinkflags
++++++++++++

**Defaulted to**: ``[]``

List of linker flags for executables of the component.

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

components
++++++++++

**Defaulted to**: ``[]``

List of names of the different components in this package.

components
++++++++++

**Defaulted to**: ``[]``

Dictionary with names names of the different components as keys and a ``Component`` object as value.
This models the different components a package might contain.

libs
++++

**Defaulted to**: ``[]``

List of library names in the correct link order.
This attribute can be set as long as ``components`` are not used.

exes
++++

**Defaulted to**: ``[]``

List of executable names.
This attribute can be set as long as ``components`` are not used.

system_deps
+++++++++++

**Defaulted to**: ``[]``

List of system dependencies.
This attribute can be set as long as ``components`` are not used.

Methods
-------

Operator []
+++++++++++

Creates and retrieve the value of the component with its name used as key.


class _CppInfo(object):
    """ Object that stores all the necessary information to build in C/C++.
    It is intended to be system independent, translation to
    specific systems will be produced from this info
    """
    def __init__(self):
        self.name = None
        self._system_deps = []
        self.rootpaths = []
        self.defines = []  # preprocessor definitions
        self.cflags = []  # pure C flags
        self.cxxflags = []  # C++ compilation flags
        self.sharedlinkflags = []  # linker flags
        self.exelinkflags = []  # linker flags
        self.rootpath = ""
        self.sysroot = ""
        self.version = None  # Version of the conan package


class CppInfo(_CppInfo):
    """ Build Information declared to be used by the CONSUMERS of a
    conans. That means that consumers must use this flags and configs i order
    to build properly.
    Defined in user CONANFILE, directories are relative at user definition time
    """
    def __init__(self, root_folder):
        super(CppInfo, self).__init__()
        self.rootpath = root_folder  # the full path of the package in which the conans is found
        self._default_dirs_values = {
            "includedirs": [DEFAULT_INCLUDE],
            "libdirs": [DEFAULT_LIB],
            "bindirs": [DEFAULT_BIN],
            "resdirs": [DEFAULT_RES],
            "builddirs": [DEFAULT_BUILD],
            "srcdirs": []
        }
        self.includedirs.extend(self._default_dirs_values["includedirs"])
        self.libdirs.extend(self._default_dirs_values["libdirs"])
        self.bindirs.extend(self._default_dirs_values["bindirs"])
        self.resdirs.extend(self._default_dirs_values["resdirs"])
        self.builddirs.extend(self._default_dirs_values["builddirs"])
        # public_deps is needed to accumulate list of deps for cmake targets
        self.public_deps = []
        self.configs = {}

    def __getitem__(self, key):
        if self._libs or self._exes:
            raise ConanException("Usage of Components with '.libs' or '.exes' values is not allowed")
        self._check_and_clear_dirs_values()
        if key not in self._components:
            self._components[key] = Component(key, self.rootpath)
        return self._components[key]