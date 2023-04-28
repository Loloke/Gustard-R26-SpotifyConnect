# Gustard-R26-SpotifyConnect
How add (unofficial, probably unstable) Spotify Connect client to Gustard R26

## Requirements
- Linux computer (Raspberry pi is also ok) for changing the image password.

## Changing the root password on the stock image
- Download the latest image (eg. 1.4) from here: https://download.shenzhenaudio.com/#gustard_r26/1
- Extract the zip file
```
cd R26_Gustarender_1v4_upgrade
mkdir imagemount
gunzip R26_Release_1v4.img.gz
fdisk -l R26_Release_1v4.img
```
Output:
```
Disk R26_Release_1v4.img: 1,25 GiB, 1343225856 bytes, 2623488 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x215bbfe4

Device               Boot Start     End Sectors  Size Id Type
R26_Release_1v4.img1       2048 2623487 2621440  1,3G 83 Linux
```
The image contains a complete disk image (with partition table), so it cannot be directly mounted. For the setup, we need to remember it's Start Sector number, and sector size (2048 * 512)

We must use a loopback device to mount the partition we need.
- First, we need a /dev/loop device:
```
# sudo losetup
NAME       SIZELIMIT OFFSET AUTOCLEAR RO BACK-FILE                             DIO LOG-SEC
/dev/loop1         0      0         1  1 /var/lib/snapd/snaps/core20_1828.snap   0     512
/dev/loop4         0      0         1  1 /var/lib/snapd/snaps/snapd_18596.snap   0     512
/dev/loop2         0      0         1  1 /var/lib/snapd/snaps/lxd_22923.snap     0     512
/dev/loop0         0      0         1  1 /var/lib/snapd/snaps/core20_1852.snap   0     512
/dev/loop5         0      0         1  1 /var/lib/snapd/snaps/snapd_18933.snap   0     512
/dev/loop3         0      0         1  1 /var/lib/snapd/snaps/lxd_24322.snap     0     512
```
So, loop0-5 devices already used by system (Ubuntu 22.04), we can use loop6 for our purposes:
```
sudo losetup /dev/loop6 -o 1048576 R26_Release_1v4.img
sudo mount /dev/loop6 mountpoint/
```
And we're in!
