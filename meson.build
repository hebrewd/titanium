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

#subdir('titanium/po')
#podir = join_paths (meson.source_root (), 'titanium', 'po')

titanium_version = meson.project_version()

titanium_prefix = get_option('prefix')
titanium_bindir = join_paths(titanium_prefix, get_option('bindir'))
titanium_localedir = join_paths(titanium_prefix, get_option('localedir'))

titanium_conf = configuration_data()
titanium_conf.set('appid', application_id)
titanium_conf.set('bindir', titanium_bindir)

datadir = get_option('datadir')
#subdir('titanium-gtk/resources')

cargo_script = find_program('scripts/cargo.sh')
test_script = find_program('scripts/test.sh')

#subdir('titanium-data/src')
#subdir('titanium-downloader/src')
#subdir('titanium-gtk/src')

titanium_sources = files (
	'src/completers.rs',
	'src/bookmarks.rs',
	'src/webview/mod.rs',
	'src/webview/settings.rs',
	'src/app/browser.rs',
	'src/app/adblock.rs',
	'src/app/pass_filler.rs',
	'src/app/test_utils.rs',
	'src/app/bookmarks.rs',
	'src/app/hints.rs',
	'src/app/mod.rs',
	'src/app/file_chooser.rs',
	'src/app/copy_paste.rs',
	'src/app/dialog.rs',
	'src/app/config.rs',
	'src/app/user_agent.rs',
	'src/app/popup.rs',
	'src/app/paths.rs',
	'src/app/server.rs',
	'src/app/search_engine.rs',
	'src/app/download.rs',
	'src/app/url.rs',
	'src/message_server.rs',
	'src/stylesheet.rs',
	'src/download_view.rs',
	'src/download_list_view.rs',
	'src/errors.rs',
	'src/main.rs',
	'src/urls.rs',
	'src/commands.rs',
	'src/gio_ext.rs',
	'src/file.rs',
	'src/pass_manager.rs',
	'src/config_dir.rs',
	'src/clipboard.rs',
	'src/permission_manager.rs',
	'src/settings.rs',
	'src/popup_manager.rs',
	'src/download.rs',
)

subdir('titanium-web-extension/src')

cargo_release = custom_target('cargo-build',
                        build_by_default: true,
                        input: [
                                titanium_sources,
				web_extension_data,
                               ],
                        output: ['titanium'],
                        install: true,
                        install_dir: titanium_bindir,
                        console: true,
                        depends: titanium_resources,
                        command: [cargo_script,
                                  '@SOURCE_ROOT@',
                                  '@OUTPUT@',
                                  meson.build_root(),
                                  profile
                                 ])


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

