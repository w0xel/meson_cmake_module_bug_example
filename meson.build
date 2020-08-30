project('cmake_module_bug_example', 'c', version: '0.0.1')

cmake = import('cmake')
sub = cmake.subproject('example_subproject')
gen = sub.dependency('main')

executable('example', dependencies: [gen])
