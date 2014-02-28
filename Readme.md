**kdevbox** is an ultra lightweight linux kernel development toolkit.

Point it to a kernel image and it will be executed inside a local KVM instance,
with a very small ramdisk as its root filesystem containing only Busybox.

I use this to hack on the Linux kernel with an extremely fast feedback cycle,
around ~20s.

## Dependencies

* a Linux box
* KVM: `apt-get install qemu-system-x86`
* busybox (statically compiled): `apt-get install busybox-static`
* cpio, to generate initrd image files: `apt-get install cpio`
* gzip
* bash

## Usage

1.  get a linux kernel source tree

1.  make changes

1.  build it (eg.: `make oldconfig && make`), and make sure it produced an
    `arch/x86/boot/bzImage` image file.

1.  run your kernel into a lightweight KVM instance with a very small
    ramdisk as the root filesystem containing only busybox:

    ```
    cd kdevbox
    ./run-kernel path/to/arch/x86/boot/bzImage
    ```

KVM will be started and the VM output (serial console) will be multiplexed to the
your terminal, via a terminal emulator implemented by QEMU. Use `C-a h` for a
list of available control commands (hint: `C-a x` terminates the VM). This is
nice because all your `printks` will be right there at your face. No need to
inspect log files.

All files inside the image directory will be available at `/` inside the VM. Feel
free to tweak `image/init` and add your own custom initialization tasks
(such as mounting filesystems, configuring network or loading modules).

## Credits

This is inspired on Nelson Elhage's blog post: [Lightweight Kernel Development
with KVM](https://blog.nelhage.com/2013/12/lightweight-linux-kernel-development-with-kvm/).

## License
<a rel="license" href="http://creativecommons.org/licenses/by/4.0/">
<img alt="Creative Commons License" style="border-width:0" src="http://i.creativecommons.org/l/by/4.0/88x31.png" /></a>

This work is licensed under a
<a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative
Commons Attribution 4.0 International License</a>.

Copyright 2014 Fabio Kung <fabio.kung@gmail.com>
