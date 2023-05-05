# This is not on the AUR.  It is loosely based on the 'emacs-git'
# package found there.  It is highly opinionated to only use stuff I
# need (or will most likely need).
#
# Package maintainer: Protesilaos Stavrou <info@protesilaos.com>
#
# About the choice of font backend, read this from the NEWS file of
# Emacs 28:
#
#     ** The Cairo graphics library is now used by default if present.
#     '--with-cairo' is now the default, if the appropriate development
#     files are found by 'configure'.  Building with Cairo is known to cause
#     some problems with bitmap fonts.  This may require you to adjust your
#     font settings, or to build with Xft support instead.
#
#     Note also that 'FontBackend' settings in ".Xdefaults" or
#     ".Xresources", or 'font-backend' frame parameter settings in your init
#     files, may need to be adjusted, as 'xft' is no longer a valid backend
#     when using Cairo.  Use 'ftcrhb' if your Emacs was built with HarfBuzz
#     text shaping support, and 'ftcr' otherwise.  You can determine this by
#     checking 'system-configuration-features'.  The 'ftcr' backend will
#     still be available when HarfBuzz is supported, but will not be used by
#     default.  We strongly recommend building with HarfBuzz support.  'x' is
#     still a valid backend.
#
#     ** 'configure' now warns about building with libXft support.
#     libXft is unmaintained, and causes a number of problems with modern
#     fonts including but not limited to crashes; support for it may be
#     removed in a future version of Emacs.  Please consider using
#     Cairo + HarfBuzz instead.
#
#     ** 'configure' now warns about not using HarfBuzz if using Cairo.
#     We want to encourage people to use the most modern font features
#     available, and this is the Cairo graphics library + HarfBuzz for font
#     shaping, so 'configure' now recommends that combination.


pkgname="emacs-git"
pkgver=30.0.50.166130
pkgrel=1
pkgdesc="GNU Emacs.  Current development target."
arch=('x86_64')
url="http://www.gnu.org/software/emacs/"
license=('GPL3')
depends=('harfbuzz' 'libgccjit' 'dbus' 'hicolor-icon-theme' 'libxinerama' 'libxfixes' 'lcms2' 'xaw3d' 'libxrandr' 'libxi' 'libsm' 'xcb-util' 'libxcb' 'cairo' 'tree-sitter' 'sqlite3')
makedepends=('git')
provides=('emacs')
conflicts=('emacs')
source=("emacs-git::git+https://git.savannah.gnu.org/git/emacs.git")
options=(!strip)
install=emacs-git.install
b2sums=('SKIP')

pkgver()
{
    cd "$srcdir/emacs-git"

    printf "%s.%s" \
           $(grep AC_INIT configure.ac | \
                 awk -F',' '{ gsub("[ \\[\\]]","",$2); print $2 }') \
           $(git rev-list --count HEAD)
}

# There is no need to run autogen.sh after first checkout.
# Doing so, breaks incremental compilation.
prepare()
{
    cd "$srcdir/emacs-git"
    [[ -x configure ]] || ( ./autogen.sh git && ./autogen.sh autoconf )
    mkdir -p "$srcdir/emacs-git/build"
}

build()
{
    cd "$srcdir/emacs-git/build"

    local _conf=(
        --prefix=/usr
        --sysconfdir=/etc
        --libexecdir=/usr/lib
        --localstatedir=/var
        --mandir=/usr/share/man
        --without-gif
        --without-tiff
        --without-rsvg
        # --without-modules
        --without-gsettings
        --without-xinput2
        --without-compress-install
        --without-toolkit-scroll-bars
        --without-xaw3d
        --without-gpm
        --without-selinux
        --without-gconf
        --with-native-compilation=yes
        --with-xaw3d
		--with-cairo
		--with-harfbuzz
        --with-sound=no
        --with-tree-sitter
		--with-json
        --with-x-toolkit=lucid
    )

    ../configure "${_conf[@]}"
    make
}

package()
{
    cd "$srcdir/emacs-git/build"

    make DESTDIR="$pkgdir/" install

    # fix user/root permissions on usr/share files
    find "$pkgdir"/usr/share/emacs/ | xargs chown root:root

    # fix permssions on /var/games
    mkdir -p "$pkgdir"/var/games/emacs
    chmod 775 "$pkgdir"/var/games
    chmod 775 "$pkgdir"/var/games/emacs
    chown -R root:games "$pkgdir"/var/games
}
