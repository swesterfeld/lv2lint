# SPDX-FileCopyrightText: Hanspeter Portner <dev@open-music-kontrollers.ch>
# SPDX-License-Identifier: CC0-1.0

project('mapper.lv2', 'c', default_options : [
	'buildtype=release',
	'warning_level=3',
	'werror=false',
	'b_lto=true',
	'c_std=gnu11'])

build_tests = get_option('build-tests')

reuse = find_program('reuse', required : false)

add_project_arguments('-D_GNU_SOURCE', language : 'c')

cc = meson.get_compiler('c')

lv2_dep = dependency('lv2')

lib_deps = [
  lv2_dep
]

lib_incs = [
  include_directories('')
]

lib_srcs = [
  join_paths('src', 'mapper.c')
]

lib_args = [
  '-Wno-unknown-attributes',
  '-Wno-ignored-attributes'
]

mapper = declare_dependency(
  compile_args : lib_args,
  include_directories : lib_incs,
  dependencies : lib_deps,
  sources : lib_srcs)

if build_tests
  thread_dep = dependency('threads')
  m_dep = cc.find_library('m')

  test_deps = [mapper, thread_dep, m_dep]

  test_srcs = []
  test_srcs += join_paths('test', 'test.c')

  mapper_test = executable('mapper_test', test_srcs,
    dependencies : test_deps,
    install : false)

  nonrt = '0'
  rt = '1'
  seed = '1234567890'

  test(' 1 threads non-rt', mapper_test,
    args : ['1', nonrt, seed],
    timeout : 360,
    suite : ['memcheck'])
  test(' 2 threads rt', mapper_test,
    args : ['2', rt, seed],
    timeout : 360,
    suite : ['memcheck'])
  test(' 4 threads non-rt', mapper_test,
    args : ['4', nonrt, seed],
    timeout : 360,
    suite : ['memcheck'])
  test(' 8 threads rt', mapper_test,
    args : ['8', rt, seed],
    timeout : 360,
    suite : ['memcheck'])
  test('16 threads non-rt', mapper_test,
    args : ['16', nonrt, seed],
    timeout : 360,
    suite : ['memcheck'])
  test('32 threads rt', mapper_test,
    args : ['32', rt, seed],
    timeout : 360,
    suite : ['memcheck'])

  if reuse.found()
    test('REUSE', reuse, args : [
      '--root', meson.current_source_dir(),
      'lint'
    ])
  endif
endif
