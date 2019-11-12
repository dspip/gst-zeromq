project('gst-zeromq', 'c', version: '1.0.0')

conf_data = configuration_data()
conf_data.set('GST_LICENSE', '"LGPL"')
conf_data.set('VERSION', '"@0@"'.format(meson.project_version()))
conf_data.set('PACKAGE', '"gst-zeromq"')

configure_file(
    output : 'config.h',
    configuration : conf_data)

inc = include_directories([
    'src/zeromq',
    'mybuild'
])

src = [
    'src/zeromq/gstzmqplugin.c',
    'src/zeromq/gstzmqsink.c',
    'src/zeromq/gstzmqsrc.c'
]

dep1 = dependency('gstreamer-1.0')
dep2 = dependency('gstreamer-base-1.0')
dep3 = dependency('gstreamer-controller-1.0')
dep4 = dependency('libzmq')

add_global_arguments('-DHAVE_CONFIG_H', language : 'c')

shared_library('gstzmq',
    sources: src,
    include_directories: inc,
    dependencies: [ dep1, dep2, dep3, dep4 ])