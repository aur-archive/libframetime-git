pkgname=libframetime-git
_name=libframetime
pkgver=0574b39
pkgrel=1
pkgdesc="A preloadable library, able to dump the frame times of any OpenGL application on Linux, on any driver."
url="https://github.com/clbr/libframetime/"
arch=('i686' 'x86_64')
license=('BSD') # i think
#depends=("sdl2")
#makedepends=("cmake")
#[ "$CARCH" = "x86_64" ] && depends=("lib32-sdl2" "sdl2")
options=('!libtool')
source=('git+https://github.com/clbr/libframetime/'
        "libframetime.sh")

pkgver() {
  cd "$_name"
  git describe --always | sed 's|libdrm.||g;s|-|.|g'
}

build() {
  #  export CFLAGS="-Og -ggdb"
  #  export CXXFLAGS="-Og -ggdb"
  cd "$_name"
  #./autogen.sh
  #./configure --prefix=/usr \

#if we are on 32 bit, only make 32 bit library
[ "$CARCH" = "i686" ] && (
  make
)

#if we are on 64 bit, make lib32-libframetime too
[ "$CARCH" = "x86_64" ] && (
  make
  mv libframetime.so libframetime64.so
  export "CFLAGS=-m32 $CFLAGS"
  make
  mv libframetime.so libframetime32.so
)
}

#check() {
#  cd "$_name"
#  make -k check
#}

package() {
  cd "$_name"
  [ "$CARCH" = "i686" ] && (
  install -d "$pkgdir/usr/lib/"
  install libframetime.so "$pkgdir/usr/lib/"
)

[ "$CARCH" = "x86_64" ] && (
   install -d "$pkgdir/usr/lib/"
   install -d "$pkgdir/usr/lib32/"
  install libframetime64.so "$pkgdir/usr/lib/libframetime.so"
  install libframetime32.so "$pkgdir/usr/lib32/libframetime.so"

)
  install -d "$pkgdir/usr/bin"
  install -m 755 "$srcdir/libframetime.sh" "$pkgdir/usr/bin/libframetime"
  install -m 755 "stats.awk" "$pkgdir/usr/bin/libframetime-stats.awk"
  #make DESTDIR="$pkgdir" install
}

md5sums=('SKIP'
         'dff512a0e0a2a04087ee9cccecfd9dce')
