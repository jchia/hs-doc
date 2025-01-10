# hs-doc
Project for building local haddock and hoogle for some packages

On Debian 12, some development packages need to be installed first (as root), as listed in
`debian-requirements.txt`:

```bash
$ apt install libasound2-dev libblas-dev libbz2-dev libcairo2-dev libdbusmenu-gtk3-dev libgirepository1.0-dev libglew-dev libglib2.0-dev libgsl-dev libicu-dev liblapack-dev liblzma-dev libmariadb-dev libmysqlclient-dev libpcap-dev libpcre3-dev libpq-dev libsdl2-dev libsdl2-gfx-dev libsdl2-image-dev libsdl2-mixer-dev libsdl2-ttf-dev libxft-dev libxrandr-dev libxss-dev libzstd-dev pkgconf protobuf-compiler
```
Make sure that `hoogle` has been installed to somewhere on your `PATH`.
Then run these commands to build the haddock and hoogle DB:
```bash
$ stack haddock # Add "-j8 --ghc-options -j8" for possibly more parallel processing.
$ stack hoogle generate -- --local
```
To run the local hoogle on port 8000:
```bash
$ stack hoogle server -- --local --port 8000
```

There is an issue that may cause hoogle to miss documentation for built-in packages like `base` and `binary`.
One workaround is to hack the associated `.conf` files as described in
[this](https://github.com/ndmitchell/hoogle/issues/420#issuecomment-1911916738) issue comment.

With GHC 9.8, various amazonka have trouble being processed by haddock. The workaround is to do this before
the main haddock run:
```bash
$ stack haddock "--optghc -XDuplicateRecordFields" amazonka-s3 amazonka-sso amazonka-sts
```
