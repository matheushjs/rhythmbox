gnome = import('gnome')

make_tab_command = ['sed', '-e', 's/^/"/', '-e', 's/$/",/', '@INPUT@']

authors_tab = custom_target('AUTHORS.tab',
  input: '../AUTHORS', output: 'AUTHORS.tab',
  command: make_tab_command, capture: true
)

maintainers_tab = custom_target('MAINTAINERS.tab',
  input: '../MAINTAINERS', output: 'MAINTAINERS.tab',
  command: make_tab_command, capture: true
)

maintainers_old_tab = custom_target('MAINTAINERS.old.tab',
  input: '../MAINTAINERS.old', output: 'MAINTAINERS.old.tab',
  command: make_tab_command, capture: true
)

documenters_tab = custom_target('DOCUMENTERS.tab',
  input: '../DOCUMENTERS', output: 'DOCUMENTERS.tab',
  command: make_tab_command, capture: true
)

shell_headers = [
  'rb-shell.h',
  'rb-shell-player.h',
  'rb-shell-preferences.h',
  'rb-playlist-manager.h',
  'rb-removable-media-manager.h',
  'rb-history.h',
  'rb-play-order.h',
  'rb-track-transfer-batch.h',
  'rb-track-transfer-queue.h'
]

install_headers(shell_headers, subdir: 'rhythmbox/shell')

resources = gnome.compile_resources('rb-resources', 'rhythmbox.gresource.xml',
  source_dir: ['../data/'],
  dependencies: playlists_xml)

shell_sources = [
  authors_tab,
  documenters_tab,
  maintainers_tab,
  maintainers_old_tab,
  'rb-application.c',
  'rb-history.c',
  'rb-play-order.c',
  'rb-play-order-linear.c',
  'rb-play-order-linear-loop.c',
  'rb-play-order-queue.c',
  'rb-play-order-random.c',
  'rb-play-order-random-by-age.c',
  'rb-play-order-random-by-age-and-rating.c',
  'rb-play-order-random-by-rating.c',
  'rb-play-order-random-equal-weights.c',
  'rb-play-order-shuffle.c',
  'rb-playlist-manager.c',
  'rb-removable-media-manager.c',
  'rb-shell.c',
  'rb-shell-clipboard.c',
  'rb-shell-player.c',
  'rb-shell-preferences.c',
  'rb-task-list.c',
  'rb-track-transfer-batch.c',
  'rb-track-transfer-queue.c',
  resources
]

shell_c_args = [
  '-DGNOMELOCALEDIR="' + get_option('datadir') + '/locale"',
  '-DG_LOG_DOMAIN="Rhythmbox"',
  '-DPIXMAP_DIR="' + get_option('datadir') + '/pixmaps"',
  '-DSHARE_DIR="' + get_option('datadir') + '"',
  '-DDATADIR="' + get_option('datadir') + '"',
  '-DLIBDIR="' + get_option('libdir') + '"',
  '-D__EXTENSIONS__',
]

rhythmbox_core_dependencies = [ mediaplayerid_dep, rhythmdb_dep,
  rbbackends_dep, rbpodcast_dep, rbwidgets_dep, sources_dep]

rhythmbox_core_include_directories = [
  pluginsinc, sourcessyncinc, include_directories('.'),
]

librhythmbox_core = shared_library('rhythmbox_core',
  shell_sources + sourcesync_sources,
  c_args: shell_c_args,
  include_directories: rhythmbox_core_include_directories,
  dependencies: rhythmbox_core_dependencies,
  install: 'true',
  link_args: ['-export-dynamic'],
  version: '@0@.@1@.@2@'.format(RHYTHMBOX_CORE_CURRENT, RHYTHMBOX_CORE_REVISION, RHYTHMBOX_CORE_AGE)
)

rhythmbox_core_dep = declare_dependency(
  link_with: librhythmbox_core,
  dependencies: rhythmbox_core_dependencies,
  include_directories: rhythmbox_core_include_directories,
)

rhythmbox_dependencies = [rhythmbox_core_dep, gobject_introspection,
  gio, gio_unix, libsoup, tdb, json_glib]

if gdk_targets.contains('x11')
  rhythmbox_dependencies += [x11]
endif

if enable_python
  rhythmbox_dependencies += [pygobject]
endif

executable('rhythmbox', 'main.c',
  dependencies: rhythmbox_dependencies,
  include_directories: [ pluginsinc ],
  link_args: ['-export-dynamic', '-Wno-undef'],
  install: true,
  install_rpath: rpath
)
