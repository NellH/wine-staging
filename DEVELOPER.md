Compiling wine-compholio
========================

**Warning:** Please note that starting with wine-compholio 1.7.23 it is deprecated
to manually apply patches without using the Makefile. To avoid typical pitfalls for
package maintainers (like trying to use the patch commandline utility for binary patches
or not updating the patchlist) it is highly recommended to use the Makefile in order
to apply all patches. This ensures that the the correct patch utility is used, that
the list of patches is updated appropriately, and so on. Please note that it is still
possible to exclude patches if desired, take a look at the end of this document for
more details.

Instructions
------------

The following instructions (based on the
[Gentoo Wiki](https://wiki.gentoo.org/wiki/Netflix/Pipelight#Compiling_manually))
will give a short overview how to compile wine-compholio, but of course not explain
all details. Make sure to install all required Wine dependencies before proceeding.

As the first step please grab the latest Wine source:
```bash
wget http://prdownloads.sourceforge.net/wine/wine-1.7.23.tar.bz2
wget https://github.com/compholio/wine-compholio-daily/archive/v1.7.23.tar.gz
```
Extract the archives:
```bash
tar xvjf wine-1*.tar.bz2
cd wine-1*
tar xvzf ../v1.7.23.tar.gz --strip-components 1
```
And apply the patches:
```bash
make -C ./patches DESTDIR=$(pwd) install
```
Afterwards run configure (you can also specify a prefix if you don't want to install
wine-compholio system-wide):
```bash
./configure --with-xattr
```
Before you continue you should make sure that `./configure` doesn't show any warnings
(look at the end of the output). If there are any warnings, this most likely means
that you're missing some important header files. Install them and repeat the `./configure`
step until all problems are fixed.

Afterwards compile it (and grab a cup of coffee):
```bash
make
```
And install it (you only need sudo for a system-wide installation):
```bash
sudo make install
```

Excluding patches
-----------------

It is also possible to apply only a subset of the patches, for example if you're
compiling for a distribution where PulseAudio is not installed, or if you just don't
like a specific patchset. Please note that some patchsets depend on each other, and
requesting an impossible situation might result in a failure to apply all patches.

Lets assume you want to exclude the patchset in directory `DIRNAME`, then just invoke the
Makefile like this:
```bash
make -C ./patches DESTDIR=$(pwd) install -W DIRNAME.ok
```

Using the same method its also possible to exclude multiple patchsets. If you want to
exclude a very large number of patches, it is easier to do specify a list of patches
which should be included instead. To apply for example only the patchsets in directory
`DIRNAME1` and `DIRNAME2`, you can use:
```bash
make -C ./patches DESTDIR=$(pwd) PATCHLIST="DIRNAME1.ok DIRNAME2.ok" install
```