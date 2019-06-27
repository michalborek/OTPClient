project(
    'OTPClient',
    'c',
    version: '1.5.1',
    license: 'GPL3+',
    meson_version: '>= 0.46.0'
)

cc = meson.get_compiler('c')

test_cflags = [
'-Wall', '-Wextra', '-O3', '-Wformat=2', '-Wmissing-format-attribute', '-fstack-protector-strong', '-Wundef', '-Wmissing-format-attribute',
'-fdiagnostics-color=always', '-Wstrict-prototypes', '-Wunreachable-code', '-Wchar-subscripts',
'-Wwrite-strings', '-Wpointer-arith', '-Wbad-function-cast', '-Wcast-align',
'-Werror=format-security', '-Werror=implicit-function-declaration', '-Wno-sign-compare', '-U_FORTIFY_SOURCE', '-D_FORTIFY_SOURCE=3'
]
if cc.get_id() == 'gcc'
    test_cflags += [ '-pie', '-fPIE' ]
endif

largs = [
    '-Wl,--no-add-needed', '-Wl,--as-needed', '-Wl,--no-undefined', '-Wl,-z,relro,-z,now'
]

lgcrypt_req_version = '>= 1.6.0'
lgcrypt_config = find_program('libgcrypt-config')
lgcrypt_version = run_command(lgcrypt_config, '--version').stdout().strip()
assert(lgcrypt_version.version_compare(lgcrypt_req_version), 'found gcrypt v' + lgcrypt_version + ', but v1.6.0 or higher is required.')

lgcrypt_cflags = run_command(lgcrypt_config, '--cflags').stdout().strip().split()
lgcrypt_libs = run_command(lgcrypt_config, '--libs').stdout().strip().split()
lgcrypt_dep = declare_dependency(
    compile_args: lgcrypt_cflags,
    link_args: lgcrypt_libs,
    version: lgcrypt_version
  )
lcotp_dep = dependency('cotp', version: '>= 1.2.1')
lpng_dep = dependency('libpng', version: '>= 1.2.0')
lzip_dep = dependency('libzip', version: '>= 1.0.0')
ljansson_dep = dependency('jansson', version: '>= 2.6.0')
lzbar_dep = dependency('zbar', version: '>= 0.10')
lgtk3_dep = dependency('gtk+-3.0', version: '>= 3.18.0')

# TODO: if zbar old then pass ZBAR_OLD_LIB
# TODO: if flatpak then pass USE_FLATPAK_APP_FOLDER

src = [
    'src/add-common.h',
    'src/common.h',
    'src/data.h',
    'src/db-misc.h',
    'src/file-size.h',
    'src/get-builder.h',
    'src/gquarks.h',
    'src/imports.h',
    'src/liststore-misc.h',
    'src/manual-add-cb.h',
    'src/message-dialogs.h',
    'src/otpclient.h',
    'src/parse-uri.h',
    'src/password-cb.h',
    'src/qrcode-parser.h',
    'src/treeview.h',
    'src/exports.h',
    'src/lock-app.h',
    'src/add-common.c',
    'src/andotp.c',
    'src/app.c',
    'src/authplus.c',
    'src/common.c',
    'src/db-misc.c',
    'src/edit-data.c',
    'src/file-size.c',
    'src/get-builder.c',
    'src/gquarks.c',
    'src/imports.c',
    'src/liststore-misc.c',
    'src/main.c',
    'src/manual-add-cb.c',
    'src/message-dialogs.c',
    'src/parse-data.c',
    'src/parse-uri.c',
    'src/password-cb.c',
    'src/qrcode-parser.c',
    'src/screenshot-add-cb.c',
    'src/select-photo-add-cb.c',
    'src/settings.c',
    'src/shortcuts.c',
    'src/treeview.c',
    'src/webcam-add-cb.c',
    'src/exports.c',
    'src/lock-app.c'
]

executable('otpclient', sources: src, dependencies : [ lgcrypt_dep, lcotp_dep, lpng_dep, lzip_dep, ljansson_dep, lzbar_dep, lgtk3_dep ],
            link_args: largs)

install_data('data/com.github.paolostivanin.OTPClient.desktop', install_dir: 'share/applications')
install_data('data/com.github.paolostivanin.OTPClient.appdata.xml', install_dir: 'share/metainfo')

install_data('src/ui/otpclient.ui', install_dir: 'share/otpclient')
install_data('src/ui/shortcuts.ui', install_dir: 'share/otpclient')

foreach i : ['16', '24', '32', '48', '64', '128', '256', '512']
    install_data('data/icons/icon_x' + i + '.png', rename: 'com.github.paolostivanin.OTPClient.png)', install_dir: 'share/icons/hicolor/' + i + ' x' + i + '/apps')
endforeach