def merge_dicts(x, y):
  z = x.copy()
  z.update(y)
  return z

CONFIG_H = """
#ifndef SOUNDIO_CONFIG_H
#define SOUNDIO_CONFIG_H

#define SOUNDIO_VERSION_MAJOR 1
#define SOUNDIO_VERSION_MINOR 1
#define SOUNDIO_VERSION_PATCH 0

#define SOUNDIO_VERSION_STRING \\"1.1.0\\"

#endif
"""

genrule(
  name = 'config.h',
  out = 'config.h',
  cmd = 'echo "' + CONFIG_H + '" > $OUT',
)

macos_sources = [
  'src/coreaudio.c',
]

macos_compiler_flags = [
  '-DSOUNDIO_HAVE_COREAUDIO',
]

macos_exported_linker_flags = [
  '-framework', 'AudioToolbox',
  '-framework', 'CoreAudio',
  '-framework', 'CoreFoundation',
]

linux_exported_linker_flags = [
  '-lpthread',
  '-lpulse'
]

linux_sources = [
  'src/pulseaudio.c',
]

linux_compiler_flags = [
  '-DSOUNDIO_HAVE_PULSEAUDIO',
]

windows_sources = [
  'src/wasapi.c',
]

windows_compiler_flags = [
  '-DSOUNDIO_HAVE_WASAPI',
]

cxx_library(
  name = 'libsoundio',
  header_namespace = '',
  exported_headers = subdir_glob([
    ('', 'soundio/*.h'),
  ]),
  headers = merge_dicts(subdir_glob([
    ('src', '*.h'),
  ]), {
    'config.h': ':config.h',
  }),
  srcs = [
    'src/channel_layout.c',
    'src/dummy.c',
    'src/os.c',
    'src/ring_buffer.c',
    'src/soundio.c',
    'src/util.c',
  ],
  platform_srcs = [
    ('default', macos_sources),
    ('^macos.*', macos_sources),
    ('^linux.*', linux_sources),
    ('^windows.*', windows_sources),
  ],
  platform_compiler_flags = [
    ('default', macos_compiler_flags),
    ('^macos.*', macos_compiler_flags),
    ('^linux.*', linux_compiler_flags),
    ('^windows.*', windows_compiler_flags),
  ],
  exported_platform_linker_flags = [
    ('default', macos_exported_linker_flags),
    ('^linux', linux_exported_linker_flags),
    ('^macos.*', macos_exported_linker_flags),
  ],
  visibility = [
    'PUBLIC',
  ],
)
