#######
###
##
#
# Maintainer: Ff Tt <ft2011 [at] gmail [dot] com>
# thanks to: Mateus Rodrigues Costa <charles [dot] costar [at] gmail [dot] com>
# https://aur.archlinux.org/packages/chrome-remote-desktop/
#

pkgname=chrome-remote-desktop
pkgver=38.0.2125.9
pkgrel=2
pkgdesc="Allows you to securely access your computer over the Internet through Chrome."
url="https://chrome.google.com/webstore/detail/gbchcmhmhahfdphkhkmpfmihenigjmpp"
arch=("i686" "x86_64")
license=("BSD")
install=$pkgname.install
depends=("python2" "python2-psutil" "gconf" "gtk2" "nss"
         "xorg-xdpyinfo" "xorg-setxkbmap" "xorg-server-xvfb" "xorg-xauth")
source=("chrome-remote-desktop.service")
md5sums=("cde1758e875ff114cc8153edb7087d2a")
if [ "$CARCH" == i686 ]; then
  _arch=i386
  md5sums+=("5c62a61ebdfb974faafec56613e9c1f7")
elif [ "$CARCH" == x86_64 ]; then
  _arch=amd64
  md5sums+=("d5322560215d4569608b76390a4cdb4f")
fi
source+=(${pkgname}_${pkgver}_$_arch.deb::https://dl.google.com/linux/direct/${pkgname}_current_$_arch.deb)
 
package() {
  msg2 "Extracting data.tar.gz"
  bsdtar -xf data.tar.gz -C "$pkgdir/"
 
  msg2 "Patching Python script"
  sed -e '1 s/python/python2/' \
      -e '/^.*sudo_command =/ s/"gksudo .*"/"pkexec"/' \
      -e '/^.*command =/ s/s -- sh -c/s sh -c/' \
      -i "$pkgdir"/opt/google/chrome-remote-desktop/chrome-remote-desktop
 
  msg2 "Removing things that won't work"
  rm -R "$pkgdir"/etc/cron.daily/
  rm -R "$pkgdir"/etc/init.d/
  rm -R "$pkgdir"/etc/pam.d/
 
  msg2 "They forgot the LICENSE file, using the copyright file instead"
  install -Dm644 "$pkgdir"/usr/share/doc/$pkgname/copyright "$pkgdir"/usr/share/licenses/$pkgname/copyright
 
  msg2 "Adding a systemd user service"
  install -Dm644 "$srcdir"/$pkgname.service "$pkgdir"/usr/lib/systemd/user/$pkgname.service
 
  msg2 "Creating symlinks for chromium compatibility"
  mkdir -p "$pkgdir"/etc/chromium/native-messaging-hosts
  ln -sr "$pkgdir"/etc/opt/chrome/native-messaging-hosts/* "$pkgdir"/etc/chromium/native-messaging-hosts
}
