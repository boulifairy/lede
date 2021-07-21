#
# This is free software, lisence use MIT.
# 
# Copyright (C) 2019 P3TERX <https://p3terx.com>
# Copyright (C) 2020 KFERMercer <KFER.Mercer@gmail.com>
# 
# <https://github.com/KFERMercer/OpenWrt-CI>
#

name: OpenWrt-CI

on:
  push:
    branches: 
      - master
  # schedule:
  #   - cron: 0 20 * * *
  release:
    types: [published]

jobs:

  build_openwrt:

    name: Build OpenWrt firmware

    runs-on: ubuntu-latest

    if: github.event.repository.owner.id == github.event.sender.id

    steps:

      - name: Checkout
        uses: actions/checkout@v2
        with:
          ref: master

      - name: Space cleanup
        env:
          DEBIAN_FRONTEND: noninteractive
        run: |
          docker rmi `docker images -q`
          sudo rm -rf /usr/share/dotnet /etc/mysql /etc/php /etc/apt/sources.list.d
          sudo -E apt-get -y purge azure-cli ghc* zulu* hhvm llvm* firefox google* dotnet* powershell openjdk* mysql* php* android*
          sudo -E apt-get update
          sudo -E apt-get -y install build-essential asciidoc binutils bzip2 gawk gettext git libncurses5-dev libz-dev patch python3 unzip zlib1g-dev lib32gcc1 libc6-dev-i386 subversion flex uglifyjs gcc-multilib g++-multilib p7zip p7zip-full msmtp libssl-dev texinfo libglib2.0-dev xmlto qemu-utils upx libelf-dev autoconf automake libtool autopoint device-tree-compiler antlr3 gperf swig libtinfo5
          sudo -E apt-get -y autoremove --purge
          sudo -E apt-get clean
          
          - name: 同步最新的源码
            run: |
              git remote add openwrt https://github.com/coolsnowwolf/lede.git
              git pull openwrt master
              git remote -v
              
          - name: 全能推送插件
            run: |
              cd package
              mkdir openwrt-packages
              cd openwrt-packages
              git clone https://github.com/zzsj0928/luci-app-pushbot.git

          # sudo mkdir -p -m 777 /mnt/openwrt/bin /mnt/openwrt/build_dir/host /mnt/openwrt/build_dir/hostpkg /mnt/openwrt/dl /mnt/openwrt/feeds /mnt/openwrt/staging_dir
          # ln -s /mnt/openwrt/bin ./bin
          # mkdir -p ./build_dir
          # ln -s -f /mnt/openwrt/build_dir/host ./build_dir/host
          # ln -s -f /mnt/openwrt/build_dir/hostpkg ./build_dir/hostpkg
          # ln -s /mnt/openwrt/dl ./dl
          # ln -s /mnt/openwrt/feeds ./feeds
          # ln -s /mnt/openwrt/staging_dir ./staging_dir

          df -h

      - name: Update feeds
        run: |
          ./scripts/feeds update -a
          ./scripts/feeds install -a

      - name: Generate configuration file
        run: |
          rm -f ./.config*
          touch ./.config

          #
          # 在 cat >> .config <<EOF 到 EOF 之间粘贴你的编译配置, 需注意缩进关系
          # 例如:

          cat >> .config <<EOF
          CONFIG_TARGET_ramips=y
          CONFIG_TARGET_ramips_mt7620=y
          CONFIG_TARGET_ramips_mt7620_DEVICE_lenovo_newifi-y1=y
          CONFIG_LIBCURL_COOKIES=y
          CONFIG_LIBCURL_FILE=y
          CONFIG_LIBCURL_FTP=y
          CONFIG_LIBCURL_HTTP=y
          CONFIG_LIBCURL_NO_SMB="!"
          CONFIG_LIBCURL_OPENSSL=y
          CONFIG_LIBCURL_PROXY=y
          CONFIG_NODEJS_12=y
          CONFIG_NODEJS_ICU_NONE=y
          CONFIG_PACKAGE_6in4=y
          CONFIG_PACKAGE_SAMBA_MAX_DEBUG_LEVEL=-1
          CONFIG_PACKAGE_adbyby=y
          CONFIG_PACKAGE_antfs-mount=y
          CONFIG_PACKAGE_automount=y
          CONFIG_PACKAGE_autosamba=y
          CONFIG_PACKAGE_blkid=y
          CONFIG_PACKAGE_btrfs-progs=y
          CONFIG_PACKAGE_curl=y
          CONFIG_PACKAGE_e2fsprogs=y
          CONFIG_PACKAGE_frpc=y
          CONFIG_PACKAGE_frps=y
          CONFIG_PACKAGE_ip6tables=y
          CONFIG_PACKAGE_iputils-arping=y
          CONFIG_PACKAGE_ipv6helper=y
          CONFIG_PACKAGE_kmod-crypto-acompress=y
          CONFIG_PACKAGE_kmod-crypto-crc32c=y
          CONFIG_PACKAGE_kmod-fs-antfs=y
          CONFIG_PACKAGE_kmod-fs-btrfs=y
          CONFIG_PACKAGE_kmod-fs-exfat=y
          CONFIG_PACKAGE_kmod-fs-ext4=y
          CONFIG_PACKAGE_kmod-fs-vfat=y
          CONFIG_PACKAGE_kmod-ipt-nat6=y
          CONFIG_PACKAGE_kmod-iptunnel=y
          CONFIG_PACKAGE_kmod-iptunnel4=y
          CONFIG_PACKAGE_kmod-lib-crc16=y
          CONFIG_PACKAGE_kmod-lib-crc32c=y
          CONFIG_PACKAGE_kmod-lib-lzo=y
          CONFIG_PACKAGE_kmod-lib-raid6=y
          CONFIG_PACKAGE_kmod-lib-xor=y
          CONFIG_PACKAGE_kmod-lib-zlib-deflate=y
          CONFIG_PACKAGE_kmod-lib-zlib-inflate=y
          CONFIG_PACKAGE_kmod-lib-zstd=y
          CONFIG_PACKAGE_kmod-nf-nat6=y
          CONFIG_PACKAGE_kmod-nls-cp437=y
          CONFIG_PACKAGE_kmod-nls-iso8859-1=y
          CONFIG_PACKAGE_kmod-nls-utf8=y
          CONFIG_PACKAGE_kmod-scsi-core=y
          CONFIG_PACKAGE_kmod-sit=y
          CONFIG_PACKAGE_kmod-tun=y
          CONFIG_PACKAGE_kmod-usb-storage=y
          CONFIG_PACKAGE_kmod-usb-storage-extras=y
          CONFIG_PACKAGE_libatomic=y
          CONFIG_PACKAGE_libattr=y
          CONFIG_PACKAGE_libblkid=y
          CONFIG_PACKAGE_libcomerr=y
          CONFIG_PACKAGE_libcurl=y
          CONFIG_PACKAGE_libext2fs=y
          CONFIG_PACKAGE_liblzo=y
          CONFIG_PACKAGE_libminiupnpc=y
          CONFIG_PACKAGE_libmount=y
          CONFIG_PACKAGE_libnatpmp=y
          CONFIG_PACKAGE_libncurses=y
          CONFIG_PACKAGE_libnghttp2=y
          CONFIG_PACKAGE_libreadline=y
          CONFIG_PACKAGE_libsmartcols=y
          CONFIG_PACKAGE_libss=y
          CONFIG_PACKAGE_libstdcpp=y
          CONFIG_PACKAGE_lsblk=y
          CONFIG_PACKAGE_luci-app-adbyby-plus=y
          # CONFIG_PACKAGE_luci-app-bypass_INCLUDE_ShadowsocksR_Libev_Server is not set
          # CONFIG_PACKAGE_luci-app-bypass_INCLUDE_Shadowsocks_Libev_Server is not set
          # CONFIG_PACKAGE_luci-app-bypass_INCLUDE_Socks5_Proxy is not set
          # CONFIG_PACKAGE_luci-app-bypass_INCLUDE_Socks_Server is not set
          CONFIG_PACKAGE_luci-app-bypass_INCLUDE_V2ray_plugin=y
          CONFIG_PACKAGE_luci-app-diskman=y
          CONFIG_PACKAGE_luci-app-frpc=y
          CONFIG_PACKAGE_luci-app-frps=y
          CONFIG_PACKAGE_luci-app-jd-dailybonus=y
          CONFIG_PACKAGE_luci-app-pushbot=y
          # CONFIG_PACKAGE_luci-app-rclone_INCLUDE_fuse-utils is not set
          # CONFIG_PACKAGE_luci-app-rclone_INCLUDE_rclone-ng is not set
          # CONFIG_PACKAGE_luci-app-rclone_INCLUDE_rclone-webui is not set
          CONFIG_PACKAGE_luci-app-samba=y
          # CONFIG_PACKAGE_luci-app-ssr-plus_INCLUDE_ShadowsocksR_Libev_Client is not set
          # CONFIG_PACKAGE_luci-app-unblockmusic is not set
          CONFIG_PACKAGE_luci-app-zerotier=y
          CONFIG_PACKAGE_luci-i18n-adbyby-plus-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-frpc-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-frps-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-samba-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-zerotier-zh-cn=y
          CONFIG_PACKAGE_luci-proto-ipv6=y
          CONFIG_PACKAGE_node=y
          CONFIG_PACKAGE_odhcp6c=y
          CONFIG_PACKAGE_odhcp6c_ext_cer_id=0
          CONFIG_PACKAGE_odhcpd-ipv6only=y
          CONFIG_PACKAGE_odhcpd_ipv6only_ext_cer_id=0
          CONFIG_PACKAGE_parted=y
          CONFIG_PACKAGE_samba36-server=y
          # CONFIG_PACKAGE_shadowsocksr-libev-ssr-local is not set
          # CONFIG_PACKAGE_shadowsocksr-libev-ssr-redir is not set
          CONFIG_PACKAGE_smartmontools=y
          CONFIG_PACKAGE_terminfo=y
          CONFIG_PACKAGE_wsdd2=y
          CONFIG_PACKAGE_zerotier=y
          CONFIG_PARTED_READLINE=y
          CONFIG_PACKAGE_luci-app-pushbot=y
          EOF

          #
          # ===============================================================
          # 

          sed -i 's/^[ \t]*//g' ./.config
          make defconfig

      - name: Make download
        run: |
          make download -j8 || make download -j1 V=s
          rm -rf $(find ./dl/ -size -1024c)
          df -h

      - name: Compile firmware
        run: |
          make -j$(nproc) || make -j1 V=s
          echo "======================="
          echo "Space usage:"
          echo "======================="
          df -h
          echo "======================="
          du -h ./ --max-depth=1
          du -h /mnt/openwrt/ --max-depth=1 || true

      - name: Prepare artifact
        run: |
          mkdir -p ./artifact/firmware
          mkdir -p ./artifact/package
          mkdir -p ./artifact/buildinfo
          rm -rf $(find ./bin/targets/ -type d -name "packages")
          cp -rf $(find ./bin/targets/ -type f) ./artifact/firmware/
          cp -rf $(find ./bin/packages/ -type f -name "*.ipk") ./artifact/package/
          cp -rf $(find ./bin/targets/ -type f -name "*.buildinfo" -o -name "*.manifest") ./artifact/buildinfo/

      - name: Deliver buildinfo
        uses: actions/upload-artifact@v2
        with:
          name: OpenWrt_buildinfo
          path: ./artifact/buildinfo/

      - name: Deliver package
        uses: actions/upload-artifact@v2
        with:
          name: OpenWrt_package
          path: ./artifact/package/

      - name: Deliver firmware
        uses: actions/upload-artifact@v2
        with:
          name: OpenWrt_firmware
          path: ./bin/targets/

      - name: Upload release asset
        if: github.event == 'release'
        uses: svenstaro/upload-release-action@v2
        with:
          repo_token: ${{ secrets.YOURTOKEN }}
          file: ./artifact/firmware/*
          tag: ${{ github.ref }}
          file_glob: true
