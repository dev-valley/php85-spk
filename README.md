## PHP 8.5.12-8 for Synology DSM 7.1+

Bug-fix release. **Recommended for everyone on 8.5.12-7 or earlier**, especially if PHP 8.5 sometimes
shows as **Stopped** in Package Center.

### Fixed

- **Package stops on its own, Web Station sites return 503** (reported on a DS720+ with DSM 7.4.1).
  Besides Web Station, the package runs its own PHP-FPM on `127.0.0.1:9085`. On some systems,
  something outside the package terminates that PHP-FPM. DSM then reports the whole package as stopped,
  and Web Station disables the PHP 8.5 backend for every profile: portals show **Abnormal** and sites
  return **503** until the package is started again.

  That PHP-FPM now runs under a small supervisor (`bin/php85-fpm-supervise`). If it's terminated by
  anything other than stopping the package, it's restarted within a few seconds and the package (and
  the Web Station backend) stays running. Each restart is logged with a timestamp in
  `/var/packages/php85/var/php85.log`. If PHP-FPM can't stay up, for example because of a broken
  `php-fpm.conf`, the retry interval backs off to at most 60 s. Stopping the package in Package Center
  works as before.

### Unchanged

- PHP 8.5.12, all 38 shared extensions, imagick/ImageMagick, `php.ini`, `php-fpm.conf` and the Web Station
  files are identical to 8.5.12-7. Only the package's service scripts changed.
- Your Web Station profiles and their extension selection are kept.

### Update

Package Center → **Manual Install** → choose the spk for your CPU and install it **over** the existing
package. Don't uninstall first. The package isn't signed, so DSM shows a warning about the publisher;
confirm it to continue.

| File | For |
|---|---|
| `php85_x64-7.1_8.5.12-8.spk` | Intel / AMD 64-bit (e.g. DS720+, DS920+, DS923+, DS1517+, RS-series) |
| `php85_aarch64-7.1_8.5.12-8.spk` | ARMv8 64-bit (rtd1296, rtd1619b, armada37xx), beta |
| `php85_armv7-7.1_8.5.12-8.spk` | ARMv7 32-bit (alpine, armada 370/375/38x/xp, monaco), beta |

See the [README](https://github.com/dev-valley/php85-spk#which-file-for-which-nas) for the full
platform list.

### SHA-256

```
4c4cfa0aadc44330209d439545a5c4182282b872a536beb2182eedd69d5bceca  php85_x64-7.1_8.5.12-8.spk
c872f6c02653d739c78015b871859b8a1044cd72c928716dce8b4bf00c6275d8  php85_aarch64-7.1_8.5.12-8.spk
8c934048b59925bc048ba78d546b0a52f1d7f6d50f33c4781cbc8cd2872e9543  php85_armv7-7.1_8.5.12-8.spk
```

### Still seeing the package stop?

Please open an [issue](https://github.com/dev-valley/php85-spk/issues) with the logs listed under
[Troubleshooting](https://github.com/dev-valley/php85-spk#troubleshooting) in the README.
