project(
  'titanium', 'rust',
  version: '0.4.6',
  license: 'GPLv3',
)

dependency('sqlite3', version: '>= 3.20')
dependency('openssl', version: '>= 1.0')
dependency('dbus-1')

dependency('glib-2.0', version: '>= 2.56')
dependency('gio-2.0', version: '>= 2.56')
dependency('gdk-pixbuf-2.0')
dependency('gtk+-3.0', version: '>= 3.24.7')
#dependency('libhandy-0.0', version: '>= 0.0.9')

dependency('gstreamer-1.0', version: '>= 1.12')
dependency('gstreamer-base-1.0', version: '>= 1.12')
dependency('gstreamer-audio-1.0', version: '>= 1.12')
dependency('gstreamer-video-1.0', version: '>= 1.12')
dependency('gstreamer-player-1.0', version: '>= 1.12')
dependency('gstreamer-plugins-base-1.0', version: '>= 1.12')
dependency('gstreamer-plugins-bad-1.0', version: '>= 1.12')
dependency('gstreamer-bad-audio-1.0', version: '>= 1.12')

cargo = find_program('cargo', required: true)
gresource = find_program('glib-compile-resources', required: true)
gschemas = find_program('glib-compile-schemas', required: true)
cargo_vendor = find_program('cargo-vendor', required: false)

#if get_option('profile') == 'development'
#  profile = '.Devel'
#  vcs_tag = run_command('git', 'rev-parse', '--short', 'HEAD').stdout().strip()
#  if vcs_tag == ''
#    version_suffix = '-devel'
#  else
#    version_suffix = '-@0@'.format (vcs_tag)
#  endif
#else
profile = ''
version_suffix = ''
#endif

application_id = 'io.github.titanium@0@'.format(profile)
i18n = import('i18n')
gnome = import('gnome')

#subdir('podcasts-gtk/po')
podir = join_paths (meson.source_root (), 'podcasts-gtk', 'po')

titanium_version = meson.project_version()

podcasts_prefix = get_option('prefix')
podcasts_bindir = join_paths(podcasts_prefix, get_option('bindir'))
podcasts_localedir = join_paths(podcasts_prefix, get_option('localedir'))

podcasts_conf = configuration_data()
podcasts_conf.set('appid', application_id)
podcasts_conf.set('bindir', podcasts_bindir)

datadir = get_option('datadir')
#subdir('podcasts-gtk/resources')

cargo_script = find_program('scripts/cargo.sh')
#test_script = find_program('scripts/test.sh')

#subdir('podcasts-data/src')
#subdir('podcasts-downloader/src')
#subdir('podcasts-gtk/src')

subdir('titanium-web-extension/src')

meson.add_dist_script(
  'scripts/dist-vendor.sh',
  meson.source_root(),
  join_paths(meson.build_root(), 'meson-dist', meson.project_name() + '-' + titanium_version)
)

test(
  'cargo-test',
  test_script,
  args: meson.build_root(),
  workdir: meson.source_root(),
  timeout: 3000
)
