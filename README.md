# compile_flags.py

## What is this?

This is a simple script for [Meson](http://mesonbuild.com) C/C++ projects to generate a [compile_flags.txt](https://clangd.llvm.org/installation#compile_flagstxt) file for [clangd](https://clangd.llvm.org/).

It achieves this by extracting the compilation flags of a given target through Meson's introspect mechanism. This way, clangd can fall back to or rely solely on *compile_flags.txt* instead of *compile_commands.json* for static analysis.

The reason for doing this can be any. I personally prefer *compile_flags.txt*, as clangd with *compile_commands.json* tends to misfire a lot (e.g. causing a lot of false errors in headers or missing new files entirely) while simultaneously making it more resource intensive.

The *compile_flags.txt* approach is both simple and less buggy, with no real downsides as far as I can tell for Meson projects that aren't as granular about how source files should be compiled (usually, flags are target-wide or even project-wide).

## How do you use it?

Just copy the **compile_flags.py** script into your project; it includes the license text with it, so you don't have to carry over the entire repo. Then add the following to your *meson.build*:

```Meson
meson.add_postconf_script('compile_flags.py')
```

The *compile_flags.txt* file will be regenerated automatically everytime you reconfigure the project, using the compilation flags of the first defined target. You can optionally specify which target's flags to use:

```Meson
meson.add_postconf_script('compile_flags.py', 'foo')
```

 If you would like to generate multiple *compile_flags.txt* files, the script also optionally takes a third argument specifying where to write the extracted flags (relative to Meson's source root):

```Meson
meson.add_postconf_script('compile_flags.py', 'A', 'src/A/compile_flags.txt')
meson.add_postconf_script('compile_flags.py', 'B', 'src/B/compile_flags.txt')
```
