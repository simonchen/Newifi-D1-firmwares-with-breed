# 如何刷Breed
降级固件xCloudOS_newifi-d1_Build_v0.0.4.2100_beta_sign.bin
登录帐号root、密码（与web管理界面一致)
用ftp软件（或scp命令）上传breed-mt7621-newifi-d1.bin至/tmp目录中

参考：
https://blog.umu618.com/2019/11/24/umutech-breed/
先降级固件到xCloudOS_newifi-d1_Build_v0.0.4.2100_beta_sign.bin

用putty登录192.168.99.1，帐号密码与前一样
输入下面命令，目的是将/dev/mtd0 (uboot) 替换成breed，并与/dev/mtd1(2,3)结合生成新的固件，
最后重新刷入。
```
cd /tmp
wget https://breed.hackpascal.net/breed-mt7621-newifi-d1.bin --no-check-certificate
dd if=/dev/zero bs=1024 count=192 | tr "\000" "\377" >breed_192.bin
dd if=breed-mt7621-newifi-d1.bin of=breed_192.bin conv=notrunc
cat /tmp/breed_192.bin /dev/mtd1 /dev/mtd2 /dev/mtd4 >fullflash_with_breed.bin
mtd write fullflash_with_breed.bin fullflash
```
最后一步命令可能报错：（固件没降级，无法写入）
root@newifi:/tmp# mtd write fullflash_with_breed.bin fullflash
Could not open mtd device: fullflash
Can't open device for writing!

最后一步命令正常：
root@xCloud:/tmp# mtd write fullflash_with_breed.bin fullflash
Unlocking fullflash ...

Writing from fullflash_with_breed.bin to fullflash ...  [w]

试下用第三方工具刷 - Breed web控制台 ，引导选u-boot

## License

[MIT](https://github.com/P3TERX/Actions-OpenWrt/blob/main/LICENSE) © [**P3TERX**](https://p3terx.com)
