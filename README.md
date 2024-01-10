# About

Kernel modules for various webOS versions/SoCs. Mostly serial drivers. Let me know if you need any other ones built.


# Donations

Donate via [Ko-fi](https://ko-fi.com/throwaway96) or [GitHub](https://github.com/sponsors/throwaway96)
if you appreciate my work or want your request bumped to the top of my to-do list.


# Source

Download kernel source and toolchain packages from the [LG Open Source](https://opensource.lge.com/) site.


# Automatically loading at boot

While you can manually load these drivers with `insmod`, people usually prefer to have them loaded automatically on each boot.

The following is an example of using the Homebrew Channel init system to load the `ch341.ko` driver, assuming it is located in `/home/root`:

```sh
cat >/home/root/insmod_serial.sh <<'__EOF__'
#!/bin/sh

insmod /home/root/ch341.ko
__EOF__
chmod +x /home/root/insmod_serial.sh
ln -s /home/root/insmod_serial.sh /var/lib/webosbrew/init.d/10-insmod_serial
```
*If you are using a different driver, just replace `ch341.ko` with its
  filename.*

This will create a script named `/home/root/insmod_serial.sh` and a symbolic link to it in `/var/lib/webosbrew/init.d`.

If you want to temporarily stop the module from being loaded, you can delete the link:
```sh
rm /var/lib/webosbrew/init.d/10-insmod_serial
```

To re-enable it, create the link again:
```sh
ln -s /home/root/insmod_serial.sh /var/lib/webosbrew/init.d/10-insmod_serial
```
