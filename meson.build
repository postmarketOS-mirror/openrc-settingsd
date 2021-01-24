project('openrc-settingsd', 'c',
  version : '1.0.1',
  default_options : ['warning_level=3'])

gnome = import('gnome')

glib_dep = dependency('glib-2.0')
gio_dep = dependency('gio-2.0')
gio_unix_dep = dependency('gio-unix-2.0')
dbus_dep = dependency('dbus-1')
polkit_dep = dependency('polkit-gobject-1')
libdaemon_dep = dependency('libdaemon')

sources = files([
  'src/bus-utils.c',
  'src/hostnamed.c',
  'src/localed.c',
  'src/main.c',
  'src/timedated.c',
  'src/utils.c',
  'src/copypaste/hwclock.c',
  'src/copypaste/util.c',
])

sources += gnome.gdbus_codegen('hostname1-generated',
  sources: 'data/org.freedesktop.hostname1.xml',
  interface_prefix : 'org.freedesktop.',
  namespace : 'OpenrcSettingsdHostnamed',
)
sources += gnome.gdbus_codegen('locale1-generated',
  sources: 'data/org.freedesktop.locale1.xml',
  interface_prefix : 'org.freedesktop.',
  namespace : 'OpenrcSettingsdLocaled',
)
sources += gnome.gdbus_codegen('timedate1-generated',
  sources: 'data/org.freedesktop.timedate1.xml',
  interface_prefix : 'org.freedesktop.',
  namespace : 'OpenrcSettingsdTimedated',
)

conf_data = configuration_data()
conf_data.set('SYSCONFDIR', '"' + get_option('prefix') / get_option('sysconfdir') + '"')
conf_data.set('DATADIR', '"' + get_option('prefix') / get_option('datadir') + '"')
conf_data.set('LIBEXECDIR', '"' + get_option('prefix') / get_option('libexecdir') + '"')
conf_data.set('PKGDATADIR', '"' + get_option('prefix') / get_option('datadir') / meson.project_name() + '"')
conf_data.set('PIDFILE', '"/var/run/' + meson.project_name() + '.pid"')
conf_data.set('ENV_UPDATE', '"/usr/sbin/env-update"')

conf_data.set('PACKAGE_STRING', '"' + meson.project_name() + ' ' + meson.project_version() + '"')

configure_file(output : 'config.h',
               configuration : conf_data)



executable('openrc-settingsd',
           sources,
           dependencies : [glib_dep, gio_dep, gio_unix_dep, dbus_dep, polkit_dep, libdaemon_dep],
           install : true)

