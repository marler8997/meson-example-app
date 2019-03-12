project('example-app', 'c')

libexample = dependency(
  'libexample',
  fallback : ['libexample', 'libexample_dep'],
)

executable('example-app',
    'example-main.c',
    dependencies : [libexample],
)
