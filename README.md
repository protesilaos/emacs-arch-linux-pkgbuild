# Custom Emacs build for Arch Linux

This is just like every other package recipe for Arch.  We provide a
PKGBUILD and then `pacman` does the rest.

1. Clone this repository.
2. Open a shell in the root directory.
3. Run `makepkg -si`.  This synchronises the source, compiles it, and
   installs the resulting files using `pacman`.
4. Note that if you miss the part where you are prompted for escalated
   privileges, you do not need to rebuild the package.  Just check the
   directory for something like
   `emacs-git-30.0.50.165666-1-x86_64.pkg.tar.zst`.  Get its path and
   then do `pacman -U /path/to/package.pkg.tar.zst`.
5. To rebuild the package, simply re-run `makepkg -si`.  Sometimes you
   will get a build failure due to some massive change upstream.  In
   that case, run `makepkg -siC`.
6. Builds are done incrementally, meaning that every build after the
   first one is considerably faster.  Though the `-C` flag also does a
   clean build and thus takes longer.

## Why this and not the `emacs-git` from the AUR?

Use that.  I have not tested this thoroughly and do not recommend you
try it.  I simply wrote it as a way to learn how Emacs is built and
ultimately particularise my configuration of it.
