# webOS kernel drivers

## About

Kernel modules for various webOS versions/SoCs. Mostly serial drivers. Let me know if you need any other ones built.

## Donations

Donate via [Ko-fi](https://ko-fi.com/throwaway96) or [GitHub](https://github.com/sponsors/throwaway96)
if you appreciate my work or want your request bumped to the top of my to-do list.

## Source

Download kernel source and toolchain packages from the [LG Open Source](https://opensource.lge.com/) site.

## Finding the correct drivers

### Device info

If you don't know which directory to use, run the following commands on your TV to get the necessary information.

#### CPU architecture

```sh
uname -m
```

Can also be found in `/proc/cpuinfo`.

#### Kernel version

```sh
uname -r
```

#### SoC

```sh
nyx-cmd DeviceInfo query device_name
```

This Luna request may also work (depending on webOS version):

```sh
luna-send -q 'boardType' -n 1 'luna://com.webos.service.tv.systemproperty/getSystemInfo' '{"keys":["boardType"]}'
```

### SoC manufacturers

A non-exhaustive list of SoCs by manufacturer:

- LX: M14, H15, M16, M16P, M16PP, M16P3, M23, O18, O20, O20N, O22, O22N, O22N2, O22N3, O24, O24N

- Realtek (RTK): K2L, K2Lp, K3Lp, K5Lp, K6Lp, K6Hp, K7Lp, K8Ap, K8Lp, K8Hp, K8LpN, K8LpN2, K8Hpp, KF23F, K24, K25Lp, K24N

- MStar: LM14A, LM15U, M2, M2R, M3, M3R, LM18A, LM21A, LM21U, LM21AN, LM21ANN

- Mediatek (MTK): A5LR

## Automatically loading at boot

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
