# A simple app/library example in meson.

The goal is to see if meson can facilitate using a library that first checks if it is installed on the system, but if not, falls back to downloading and building it from source.

# The Problem

I currently that I use a `custom_target` to download the library source if it's not installed on the system (see `subprojects/libexample/meson.build`).  However, using this custom target is quite awkward since:

1. I cannot seem to add source files from the downloaded repo. Instead, I have to make another custom target that will copy files from the downloaded repo into another directory.

2. I cannot get a reference to the downloaded repo to add as an include directory.

> From nirbheek (June 20, 2017): https://github.com/mesonbuild/meson/issues/1855#issuecomment-309955330
>
>   I am not sure how we would make this easier to do in Meson. Will have to think about it.

# Proposed Solution

In the file `subprojects/libexample/meson.proposed_solution.build` I demonstrated what I believe to be a good way to express the semantics I'm looking for. To solve the problem of referencing files in a "custom target", I propose that you can call the `output` function on a custom target object to retrieve the file/directory object:

```
target = custom_target(...)

first_output = target.output(0)
second_output = target.output(1)
...
```

Next, I naturally assumed there was a method on a "file/directory" object I could call to make more file/directory objects relative to it, i.e.

```
target = custom_target(...)
mydir = target.output(0)

mydir.file('a-file-in-mydir')
```

Using these 2 new features, I was able to achieve the solution I was looking for.
