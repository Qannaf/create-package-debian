# create-package-debian
## Step-By-Step guide how to create .deb file from your files

0. `sudo apt-get install devscripts build-essential lintian`
1. Create a directory with prebuild binaries, for example: **myproj_1.0.0.orig**
2. **myproj_1.0.0.orig** must contains a linux file structure, for exampel: binaries should be in `myproj_1.0.0.orig/usr/bin`, libraries in `usr/lib` or `usr/lib64` et cetera..
3. Create tar.gz from directory: `tar -zcvf myproj_1.0.0.orig.tar.gz myproj_1.0.0.orig`
4. `cd myproj_1.0.0.orig`
5. `mkdir debian`
6. `dch --create -v 1.0.0-1 --package myproj` - this command may ask you about preferred editor, select one
7. Write changelog respecting syntax:
 - asterisk (*) - what was changed, if new, just leave "initial release"
 - -- author, write your name and email
8. `echo "10" > debian/compat`
9. create file `control` in `debian`
 - Example:
 ```
Source: myproj
Maintainer: Qannaf AL-SAHMI <q.alsahmi@gmail.com>
Section: misc
Priority: optional
Standards-Version: 4.2.1
Build-Depends: debhelper (>=10), !!! write dependencies here in format: package-name (version_name), comma_separated (version_name)

Package: myproj
Architecture: !!! any (arch doesn't matter) or all or i386 (fox x86) or amd64 (for x86_64) https://www.debian.org/doc/manuals/maint-guide/dreq.html#control
Depends: ${shlibs:Depends}, ${misc:Depends}
Description: Write your
multiline description
 ```
 
 10. Write copyright to `debian/copyright`
 11. Paste to `debian/rules`:
 ```bash
 #!/usr/bin/make -f
%:
    dh $@
 ```
 **note: The last line should be indented by one TAB character, not by spaces. The file is a makefile, and TAB is what the make command wants**
 
 12. `mkdir debian/source && echo "3.0 (quilt)" debian/source/format`
 
 13. Now, in current root directory (**myproj_1.0.0.orig**), run: `debuild -us -uc`
 14. `cd .. && ls` 
 15. Done.
 
 For more information about errors and advanced configuration, read [official doc](https://www.debian.org/doc/manuals/maint-guide/dreq.html#control)
