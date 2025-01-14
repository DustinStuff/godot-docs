.. _doc_compiling_with_mono:

Compiling with Mono
===================

.. highlight:: shell

Requirements
------------

- Mono 5.12.0 or greater
- MSBuild
- NuGet
- pkg-config

You may need to import necessary certificates for NuGet to perform HTTPS requests. You can do this
with the following command (on Windows, you can run it from the Mono command line prompt):

::

    mozroots --import --sync

Environment variables
---------------------

By default, SCons will try to find Mono in the Windows Registry on Windows or via ``pkg-config`` on other platforms.
You can specify a different installation directory by passing the ``mono_prefix`` command-line option to SCons;
e.g.: ``scons [...] mono_prefix=%ProgramFiles%/Mono``.

This is the directory that contains the subdirectories ``include`` and ``lib``.

Enable the Mono module
----------------------

By default, the mono module is disabled for builds. To enable it you can pass the
option ``module_mono_enabled=yes`` to your SCons command.

Generate the glue
-------------------

The glue sources are the wrapper functions that will be called by managed methods. These source
files must be generated before building your final binaries. In order to generate them, first,
you must build a temporary Godot binary with the options ``tools=yes`` and ``mono_glue=no``:

::

    scons p=<platform> tools=yes module_mono_enabled=yes mono_glue=no

After the build finishes, you need to run the compiled executable with the parameter
``--generate-mono-glue`` followed by the path to an output directory. This path
must be ``modules/mono/glue`` in the Godot directory.

::

    <godot_binary> --generate-mono-glue modules/mono/glue

This command will tell Godot to generate the file ``modules/mono/glue/mono_glue.gen.cpp``.
Once this file is generated, you can build Godot for all the desired targets without the need to repeat this process.

``<godot_binary>`` refers to the tools binary you compiled above with the Mono module enabled.
Its exact name will differ based on your system and configuration, but should be of the form
``bin/godot.<platform>.tools.<bits>.mono``, e.g. ``bin/godot.x11.tools.64.mono`` or ``bin/godot.windows.tools.64.exe``.
Be especially aware of the **.mono** suffix! If you compiled Godot without Mono support previously,
you might have similarly named binaries without this suffix which can't be used to generate the Mono glue.

Notes
^^^^^
-  **Do not** build your final binaries with ``mono_glue=no``. This disables C# scripting.
   This option must be used only for the temporary binary that will generate the glue.
   Godot will print a warning at startup if it was built without the glue sources.
-  The glue sources must be regenerated every time the ClassDB registered API changes. That is, for example,
   when a new method is registered to the scripting API or one of the parameter of such a method changes.
   Godot will print an error at startup if there is an API mismatch between ClassDB and the glue sources.


Rebuild with Mono glue
----------------------

Once you have generated the Mono glue, you can build the final binary with ``mono_glue=yes``.
It's the default value for ``mono_glue`` so you can also omit it. You can build the Mono-enabled editor:

::

    scons p=<platform> tools=yes module_mono_enabled=yes mono_glue=yes

And Mono-enabled export templates:

::

    scons p=<platform> tools=no module_mono_enabled=yes mono_glue=yes

If everything went well, apart from the normal output SCons should have created the following files in the ``bin`` directory:

-  If you're not static linking the Mono runtime, the build script will place the Mono runtime shared library (``monosgen-2.0``) next
   next to the Godot binary in the output directory. Make sure to include this library when distributing Godot. When targeting Android,
   no extra steps are required, as this library is automatically copied ``#platform/android/java/libs`` and gradle takes care of the rest.
-  Unlike "classical" Godot builds, when building with the mono module enabled and depending of the target platform a data directory
   may be created both for the editor and for export templates. This directory is important for proper functioning and must be
   distributed together with Godot. More details about this directory in :ref:`Data directory<compiling_with_mono_data_directory>`.


Examples
--------

Example (Windows)
^^^^^^^^^^^^^^^^^
::

    # Build temporary binary
    scons p=windows tools=yes module_mono_enabled=yes mono_glue=no
    # Generate glue sources
    bin\godot.windows.tools.64.mono --generate-mono-glue modules/mono/glue

    ### Build binaries normally
    # Editor
    scons p=windows target=release_debug tools=yes module_mono_enabled=yes
    # Export templates
    scons p=windows target=debug tools=no module_mono_enabled=yes
    scons p=windows target=release tools=no module_mono_enabled=yes

Example (X11)
^^^^^^^^^^^^^
::

    # Build temporary binary
    scons p=x11 tools=yes module_mono_enabled=yes mono_glue=no
    # Generate glue sources
    bin/godot.x11.tools.64.mono --generate-mono-glue modules/mono/glue

    ### Build binaries normally
    # Editor
    scons p=x11 target=release_debug tools=yes module_mono_enabled=yes
    # Export templates
    scons p=x11 target=debug tools=no module_mono_enabled=yes
    scons p=x11 target=release tools=no module_mono_enabled=yes

.. _compiling_with_mono_data_directory:

Data directory
--------------

The data directory is a dependency for Godot binaries built with the mono module enabled. It contains files
that are important for the correct functioning of Godot. It must be distributed together with the Godot executable.
There is no data directory when targeting ``Android`` so the following information does not apply to that platform.

Export templates
^^^^^^^^^^^^^^^^

The name of the data directory for a export template differs based on the configuration it was built with.
The format is ``data.mono.<platform>.<bits>.<target>``, e.g. ``data.mono.x11.32.debug`` or ``data.mono.windows.64.release``.

In the case of export templates the data directory only contains Mono framework assemblies
and configuration files, as well as some shared library dependencies like ``MonoPosixHelper``.

This directory must be placed with its original name next to the Godot export templates.
When exporting a project, Godot will also copy this directory with the game executable but
the name will be changed to ``data_<APPNAME>``, where ``<APPNAME>`` is the application name
as specified in the project setting ``application/config/name``.

In the case of macOS, where the export template is compressed as a zip file, the
contents of the data directory can be placed in the following locations inside the zip:

+-------------------------------------------------------+---------------------------------------------------------------+
| ``bin/data.mono.<platform>.<bits>.<target>/Mono/lib`` | ``/osx_template.app/Contents/Frameworks/GodotSharp/Mono/lib`` |
+-------------------------------------------------------+---------------------------------------------------------------+
| ``bin/data.mono.<platform>.<bits>.<target>/Mono/etc`` | ``/osx_template.app/Contents/Resources/GodotSharp/Mono/etc``  |
+-------------------------------------------------------+---------------------------------------------------------------+

Editor
^^^^^^^^

The name of the data directory for the Godot editor will always be ``GodotSharp``.
The main structure of this directory has the following subdirectories:

- ``Api`` (optional)
- ``Mono`` (optional)
- ``Tools`` (required)

The ``Tools`` subdirectory contains tools required by the editor, like the ``GodotSharpTools`` assembly.

The ``Mono`` subdirectory is optional. It can be used to bundle the Mono framework assemblies and
configuration files with the Godot editor, as well as some shared library dependencies like ``MonoPosixHelper``.
This is important to avoid issues that might arise when the installed Mono version in the user's system may
not be the same as the one the Godot editor was built with. You can make SCons copy these files to
this subdirectory by passing the option ``copy_mono_root=yes`` when building the editor.

The ``Api`` directory is also optional. Godot API assemblies are not bundled with the editor by default.
Instead the Godot editor will generate and build them on the user's machine the first time they are required.
This can be avoided by generating and building them manually and placing them in this subdirectory.
If the editor can find them there, it will avoid the step of generating and building them again.

The following is an example script for building and copying the Godot API assemblies:

.. tabs::
 .. code-tab:: bash Bash

    DATA_API_DIR=./bin/GodotSharp/Api
    SOLUTION_DIR=/tmp/build_GodotSharp
    BUILD_CONFIG=Release
    # Generate the solution
    ./bin/<godot_binary> --generate-cs-api $SOLUTION_DIR
    # Build the solution
    msbuild $SOLUTION_DIR/GodotSharp.sln /p:Configuration=$BUILD_CONFIG
    # Copy the built files
    mkdir -p $DATA_API_DIR
    cp $SOLUTION_DIR/GodotSharp/bin/$BUILD_CONFIG/{GodotSharp.dll,GodotSharp.pdb,GodotSharp.xml} $DATA_API_DIR
    cp $SOLUTION_DIR/GodotSharpEditor/bin/$BUILD_CONFIG/{GodotSharpEditor.dll,GodotSharpEditor.pdb,GodotSharpEditor.xml} $DATA_API_DIR

 .. code-tab:: batch Batch

    set DATA_API_DIR=.\bin\GodotSharp\Api
    set SOLUTION_DIR=%Temp%\build_GodotSharp
    set BUILD_CONFIG=Release
    # Generate the solution
    .\bin\<godot_binary> --generate-cs-api %SOLUTION_DIR%
    # Build the solution
    msbuild %SOLUTION_DIR%\GodotSharp.sln /p:Configuration=%BUILD_CONFIG%
    # Copy the built files
    if not exist "%DATA_API_DIR%" mkdir %DATA_API_DIR%
    for %%I in (GodotSharp.dll GodotSharp.pdb GodotSharp.xml) do copy %SOLUTION_DIR%\GodotSharp\bin\%BUILD_CONFIG%\%%I %DATA_API_DIR%
    for %%I in (GodotSharpEditor.dll GodotSharpEditor.pdb GodotSharpEditor.xml) do copy %SOLUTION_DIR%\GodotSharpEditor\bin\%BUILD_CONFIG%\%%I %DATA_API_DIR%

The script assumes it's being executed from the directory where SConstruct is located.
``<godot_binary>`` refers to the tools binary compiled with the Mono module enabled.

In the case of macOS, if the Godot editor is distributed as a bundle, the contents of the data directory may be placed in the following locations:

+-------------------------------------------------------+---------------------------------------------------------------+
| ``bin/data.mono.<platform>.<bits>.<target>/Api``      | ``<bundle_name>.app/Contents/Frameworks/GodotSharp/Api``      |
+-------------------------------------------------------+---------------------------------------------------------------+
| ``bin/data.mono.<platform>.<bits>.<target>/Mono/lib`` | ``<bundle_name>.app/Contents/Frameworks/GodotSharp/Mono/lib`` |
+-------------------------------------------------------+---------------------------------------------------------------+
| ``bin/data.mono.<platform>.<bits>.<target>/Mono/etc`` | ``<bundle_name>.app/Contents/Resources/GodotSharp/Mono/etc``  |
+-------------------------------------------------------+---------------------------------------------------------------+
| ``bin/data.mono.<platform>.<bits>.<target>/Tools``    | ``<bundle_name>.app/Contents/Frameworks/GodotSharp/Tools``    |
+-------------------------------------------------------+---------------------------------------------------------------+

Targeting Android
-----------------

Compiling the Android export templates with Mono is a bit simpler than it is for the desktop platforms,
as there are no additional steps required after building. There is no need to worry about any
dependency like a data directory or the runtime shared library (when dynamically linking) as
those are automatically added to the gradle project.

**Important:** You need to manually specify the mono version with the ``MONO_VERSION`` environment variable.

Before building Godot you do need to cross compile the Mono runtime for the target architectures. The easiest
way to do this is to use the sdk Makefiles from the Mono repository. The following is an example bash script:

*Note: We plan to distribute prebuilt packages of the Mono runtime in the future so you don't have to build it yourself.*

.. code:: bash

    #!/bin/bash

    set -e;
    set -o pipefail;

    set -x;

    # You have to set the MONO_SOURCE_ROOT environment variable to point to the
    # Mono repository location in the file system before running this script.

    : ${MONO_SOURCE_ROOT:?Variable MONO_SOURCE_ROOT not set or empty}
    cd ${MONO_SOURCE_ROOT}

    # We're using the sdk makefiles distributed with Mono. In the future we may want to
    # write our own configuration to get rid of the stuff we don't need and reducing size.
    # We are not using the cross templates for now, so you can comment out the calls
    # to the AndroidCross* functions in '${MONO_SOURCE_ROOT}/sdks/builds/android.mk'.

    ANDROID_TOOLCHAIN_DIR=${ANDROID_TOOLCHAIN_DIR:-${HOME}/Android/Toolchain}
    ANDROID_TOOLCHAIN_CACHE_DIR=${ANDROID_TOOLCHAIN_CACHE_DIR:-${ANDROID_TOOLCHAIN_DIR}/android-archives}
    ANDROID_TOOLCHAIN_PREFIX=${ANDROID_TOOLCHAIN_PREFIX:-${ANDROID_TOOLCHAIN_DIR}/toolchains}

    # The Makefiles expect the Android SDK and NDK to be located at '${ANDROID_TOOLCHAIN_DIR}/sdk' and
    # '${ANDROID_TOOLCHAIN_DIR}/ndk' respectively. Godot uses '${ANDROID_TOOLCHAIN_DIR}/sdk/ndk-bundle'
    # for the NDK, so '${ANDROID_TOOLCHAIN_DIR}/ndk' can be setup as a symlink to that directory.

    if [ ! -d ${ANDROID_TOOLCHAIN_DIR}/sdk ]; then
        echo Directory not found ${ANDROID_TOOLCHAIN_DIR}/sdk
        exit 1
    fi

    if [ ! -d ${ANDROID_TOOLCHAIN_DIR}/ndk ]; then
        echo Directory not found ${ANDROID_TOOLCHAIN_DIR}/ndk
        exit 1
    fi

    export ANDROID_TOOLCHAIN_DIR ANDROID_TOOLCHAIN_CACHE_DIR ANDROID_TOOLCHAIN_PREFIX

    MAKE_NUM_JOBS=${MAKE_NUM_JOBS:-2}

    echo "
    DISABLE_IOS = 1
    DISABLE_MAC = 1
    DISABLE_WASM = 1
    DISABLE_WASM_CROSS = 1
    DISABLE_BCL = 1
    DISABLE_DESKTOP = 1
    DISABLE_LLVM = 1
    " > ${MONO_SOURCE_ROOT}/sdks/Make.config

    make -C sdks/builds provision-mxe
    make -C sdks/builds archive-android NINJA= IGNORE_PROVISION_ANDROID=1 IGNORE_PROVISION_MXE=1 -j ${MAKE_NUM_JOBS}

    # You can then pass to SCons: mono_prefix=${MONO_SOURCE_ROOT}/sdks/out/android-${TARGET_ARCH}-release

    set +x;

Command-line options
--------------------

The following is the list of command-line options available when building with the mono module:

- **module_mono_enabled**: Build Godot with the mono module enabled ( yes | **no** )

- **mono_glue**: Whether to include the glue source files in the build and define `MONO_GLUE_DISABLED` as a preprocessor macro ( **yes** | no )

- **mono_prefix**: Path to the Mono installation directory for the target platform and architecture

- **xbuild_fallback**: Whether to fallback to xbuild if MSBuild is not available ( yes | **no** )

- **mono_static**: Whether to link the mono runtime statically ( yes | **no** )

- **copy_mono_root**: Whether to copy the Mono framework assemblies and configuration files required by the Godot editor ( yes | **no** )
