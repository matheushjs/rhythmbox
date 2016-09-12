subdir('sync')

sources_headers = [
  'rb-source.h',
  'rb-streaming-source.h',
  'rb-source-search.h',
  'rb-display-page.h',
  'rb-display-page-group.h',
  'rb-display-page-tree.h',
  'rb-display-page-menu.h',
  'rb-display-page-model.h',
  'rb-browser-source.h',
  'rb-media-player-source.h',
  'rb-playlist-source.h',
  'rb-playlist-xml.h',
  'rb-auto-playlist-source.h',
  'rb-static-playlist-source.h',
  'rb-source-search-basic.h',
  'rb-device-source.h',
  'rb-transfer-target.h'
]

install_headers(sources_headers, subdir: 'rhythmbox/sources')

sources_sources = files(
  'rb-source.c',
  'rb-streaming-source.c',
  'rb-source-search.c',
  'rb-display-page.c',
  'rb-display-page-group.c',
  'rb-display-page-tree.c',
  'rb-display-page-menu.c',
  'rb-display-page-model.c',
  'rb-browser-source.c',
  'rb-library-source.c',
  'rb-media-player-source.c',
  'rb-playlist-source.c',
  'rb-auto-playlist-source.c',
  'rb-static-playlist-source.c',
  'rb-play-queue-source.c',
  'rb-missing-files-source.c',
  'rb-import-errors-source.c',
  'rb-source-search-basic.c',
  'rb-device-source.c',
  'rb-transfer-target.c'
)

sources_c_args = [
  '-DGNOMELOCALEDIR="' + get_option('datadir') + '/locale"',
  '-DG_LOG_DOMAIN="Rhythmbox"',
  '-DPIXMAP_DIR="' + get_option('datadir') + '/pixmaps"',
  '-DSHARE_DIR="' + get_option('datadir') + '"',
  '-DDATADIR="' + get_option('datadir') + '"',
]

if use_no_strict_aliasing
  sources_c_args += ['-fno-strict-aliasing']
endif

sources_dependencies = [totem_plparser, gstreamer_pbutils, gtk,
  librb_dep, rhythmdb_dep, rbbackends_dep, rbmetadata_dep,
  sourcesync_dep]

sources_lib = static_library('sources', sources_sources,
  c_args: sources_c_args,
  include_directories: [configinc, shellinc, sourcesinc, widgetsinc],
  dependencies: sources_dependencies,
  install: false,
  link_args: ['-export-dynamic']
)

sources_dep = declare_dependency(
  link_with: sources_lib,
  dependencies: sources_dependencies,
  include_directories: [configinc, include_directories('.')],
)
