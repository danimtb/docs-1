
Build automation
================

You can invoke CMake from your *conanfile.py* file and automate the build of your library/project.
Conan provides a ``CMake()`` helper that is useful to call ``cmake`` command both for creating conan packages
or automating your project build with the :command:`conan build .` command.

The ``CMake()`` helper will take into account your settings to automatically set definitions and generator according to your compiler,
build_type, etc.

.. seealso::

    Check the section :ref:`Building with CMake<cmake_reference>`.
