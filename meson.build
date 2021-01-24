project('openrc-settingsd', 'c',
  version : '1.0.1',
  license : 'GPL-2.0-or-later',
  default_options : ['warning_level=3'])

gnome = import('gnome')

glib_dep = dependency('glib-2.0')
gio_dep = dependency('gio-2.0')
gio_unix_dep = dependency('gio-unix-2.0')
dbus_dep = dependency('dbus-1')
polkit_dep = dependency('polkit-gobject-1')
libdaemon_dep = dependency('libdaemon')
openrc_dep = dependency('openrc', required : get_option('openrc'))

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
conf_data.set('HAVE_OPENRC', openrc_dep.found())

configure_file(
  output : 'config.h',
  configuration : conf_data
)

executable(
  'openrc-settingsd',
  sources,
  dependencies : [
    glib_dep,
    gio_dep,
    gio_unix_dep,
    dbus_dep,
    polkit_dep,
    libdaemon_dep,
  ],
  install : true,
  install_dir : get_option('prefix') / get_option('libexecdir'),
)

install_data(
  'data/kbd-model-map',
  install_dir : get_option('prefix') / get_option('datadir') / meson.project_name(),
)

install_data(
  [
    'data/org.freedesktop.hostname1.xml',
    'data/org.freedesktop.locale1.xml',
    'data/org.freedesktop.timedate1.xml',
  ],
  install_dir : dbus_dep.get_pkgconfig_variable('interfaces_dir'),
)

conf_dbus_service = configuration_data()
conf_dbus_service.set('libexecdir', get_option('prefix') / get_option('libexecdir'))

dbussystemservicesdir = dbus_dep.get_pkgconfig_variable('system_bus_services_dir')
configure_file(
  input : 'data/org.freedesktop.hostname1.service.in',
  output : 'org.freedesktop.hostname1.service',
  configuration : conf_dbus_service,
  install_dir : dbussystemservicesdir,
)
configure_file(
  input : 'data/org.freedesktop.locale1.service.in',
  output : 'org.freedesktop.locale1.service',
  configuration : conf_dbus_service,
  install_dir : dbussystemservicesdir,
)
configure_file(
  input : 'data/org.freedesktop.timedate1.service.in',
  output : 'org.freedesktop.timedate1.service',
  configuration : conf_dbus_service,
  install_dir : dbussystemservicesdir,
)

install_data(
  'data/openrc-settingsd.conf',
  install_dir : get_option('prefix') / get_option('sysconfdir') / 'dbus-1' / 'system.d',
)

install_data(
  [
    'data/org.freedesktop.hostname1.policy',
    'data/org.freedesktop.locale1.policy',
    'data/org.freedesktop.timedate1.policy',
  ],
  install_dir : polkit_dep.get_pkgconfig_variable('actiondir')
)

install_man('data/openrc-settingsd.8')
