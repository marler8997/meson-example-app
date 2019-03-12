project('libexample', 'c',
  version : '1.0.0',
)

download_repo = custom_target('download-repo',
  output:['meson-example-library-repo'],
  input:files('download-repo'),
  command:['@INPUT@', '@OUTPUT@'],
)

example_h = custom_target('example-h',
  output:['example.h'],
  input:download_repo,
  # THIS IS A HACK: for some reason there doesn't seem to be a way
  #                 to add include directories for generated headers
  command:['cp', '@INPUT@/example.h','@SOURCE_DIR@'],
  #command:['cp', '@INPUT@/example.h','@OUTPUT@'],
)
example_c = custom_target('example-c',
  output:['examplestuff.c'],
  input:download_repo,
  command:['cp', '@INPUT@/examplestuff.c','@OUTPUT@'],
)

libexample = library('libexample',
  example_c,
  example_h,
  install:true,
)
libexample_dep = declare_dependency(
  link_with : libexample,
)
