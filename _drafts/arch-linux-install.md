---
layout: post
title: "노트북에 아치 리눅스(Arch Linux) 설치하기"
comments: true
author: feynubrick
date: 2019-01-16
tags: [Linux, How-to]
---

# 아치 리눅스(Arch linux)?

## 리눅스
아치 리눅스가 어떤 것인지 알고 계신 분들이라면 리눅스에대한 설명은 필요없겠죠.
들어본 분들도 마찬가지일 것입니다.

사실 이건 핑계에 불과하고, 제가 리눅스에대해 잘 알지 못해 설명을 못하는 것이기도 합니다.
저는 2008년 우분투 08.04를 통해 리눅스를 처음 접했습니다.
학교 과제용으로 C언어를 사용해야했고, 조교님이 추천을 해주셔서 사용해본 것인데요.
우분투를 처음 부팅하고 들었던 아프리카 드럼소리가 아직도 귀에 생생하네요.
하지만 언제나 어찌어찌 사용만 했을 뿐, 리눅스를 제대로 이해할 엄두는 내지 못했습니다.
그 뒤로 대학원에 가면서 자연스레(?) centOS를 사용할 기회도 생겼습니다.
연구단 서버 관리를 맡게되었거든요...

아무 것도 모르는 상태에서 수십명이 사용하는 서버를 관리하는 일은 정말 지옥 같은 경험이었습니다.
이 서버는 웹서버는 아니었고, 시뮬레이션이나 분석 관련한 계산을 수행하는 8개의 노드를 가진 서버였습니다.
24시간 뜨겁게 돌아가야하는 계산 서버에 문제라도 생기면, 때와 장소를 가리지 않고 필사적인 구글링을 통해 가능한 빨리 문제를 해결해야했습니다.
문제가 해결되는 것을 보면서 처음에는 즐겁기도 했습니다만, 연구와 공부와 서버관리를 동시에 하다보니 점점 맛이가기 시작했습니다.

그래서인지 문제만 접했던 저에게 리눅스는 문제 그 자체로 보였습니다.
알 수 없는 이유로 생긴 문제를 이해하지 못하는 방법으로 해결하면서 이런 생각은 확신으로 굳어졌습니다.
"리눅스는 사람이 쓸 운영체제가 아니다. 맥이나 윈도우 같은 상용 운영체제만이 사람이 쓸 수 있는 운영체제다."

제가 다른 실험 그룹으로 가게 되면서 서버관리를 하지 않게 된 뒤로 리눅스는 거들떠 보지도 않았습니다.
시뮬레이션을 하거나 분석을 해야할 때 코딩용으로 사용하던 컴퓨터 역시 맥이나 윈도우였습니다.
개발자가 되기로 마음을 먹고 대학원을 나올 무렵에 리눅스가 다시 생각이 나기 시작하더군요.

제가 기초 과학 연구자가 되는 것을 포기하고 개발자가 되기로 결심한 것은 "미래"가 아니라 현재 세상에 기여할 수 있는 실용적인 일을 하고 싶다는 생각 때문이었습니다.
아무도 알 수 없고, 동료들의 리뷰조차 힘든 일은 더는 하고 싶지 않았습니다.
개발자의 일은 달라보였습니다.
어쨌든 모두가 읽을 수 있는 소스코드가 있었으니까요.

그러다 유투브에서 언젠가 봤던 FSF(Free Software Foundation)의 창립자인 리처드 스톨먼(Richard Stallman)의 [TEDx 발표](https://www.youtube.com/watch?v=Ag1AKIl_2GM)를 본 뒤로,
GNU/Linux의 철학에 깊은 공감을 하게 되었습니다.
바로 "자유"인데요.
소프트웨어를 사용하는 사람이 소스 코드를 보고, 공부하고, 원하는 대로 바꾸고, 배포할 자유.
그는 윈도우나 맥 같은 운영체제가 이 자유를 침해하고 있다고 주장하면서, GNU/Linux를 사용해야한다고 역설합니다.

저는 그의 말에 공감은 하지만, 이 제조회사들에게도 이유와 명분이 있다고 생각합니다.
확실히 자유가 "제한"되는 것은 맞지만, 그들이 있었기에 컴퓨터가 이 정도로 보급된 것이기 때문입니다.
컴퓨터를 보다 깊은 수준에서 이해하지 못해도 사용할 수 있는 소프트웨어도, 결국엔 이런 상용 운영체제가 있기 때문에 가능했습니다.

스톨먼의 말에서 가장 구미가 당겼던 것은 "소스 코드를 보고 공부할 자유"였습니다.
언제나 운영체제라는 엄청난 소프트웨어가 어떻게 작동하는지 궁금했었거든요.
개발자가 되기로 한 이상 이런 기회를 제공하는 소프트웨어를 제대로 사용하면서 공부하고 싶다는 생각이 강해졌습니다.
그래서 검색하다가 발견한 운영체제가 Arch Linux(아치 리눅스)였습니다.

## 아치 리눅스!

아치 리눅스는 어떻게 보면 황당한 운영체제입니다.
기본 설치를 마치면, 현대적인 운영체제에서 당연히 확인할 수 있는 GUI(Graphic User Interface)가 보이지 않기 때문입니다.
그저 검은 CLI(Command Line Interface)만 사용자를 반깁니다.

아치의 장점은 여기서 출발합니다.
정말 최소한의 것만 설치되어 있기 때문에 그 외의 나머지는 본인이 선택하고 만들어나가야 합니다.
공부를 안하고는 제대로 쓸 수 조차 없습니다.
그렇다고 공부를 알아서 하라는 무책임함을 보여주지는 않습니다.
아치 위키(Arch wiki)라는 방대하고 잘 정리된 문서로 그 책임을 다하고 있습니다.

사실 이 포스트의 주 내용인 설치과정도 이 아치 위키의 [설치 가이드](https://wiki.archlinux.org/index.php/installation_guide)를 보고 따라한 결과를 그저 적은 것일 뿐입니다.
이 글 보다는 설치 가이드를 보시는 것이 좋지만, 이 설치 가이드를 읽는 데 어려움이 있으신 분은 저의 경험을 통해 필요한 정보를 얻으셨으면 좋겠습니다.

# 설치 전 준비 단계

## 이 설치에 사용한 컴퓨터에 대해

제가 이 설치에 사용한 컴퓨터는 삼성전자의 2018년형 "노트북 9 올웨이즈"입니다.
그래픽 카드는 두가지가 들어가 있는데요.
인텔의 내장 그래픽과 엔비디아(NVIDIA)의 지포스(Geforce) MX150입니다.
윈도우에서 사용하면 별 고민없이 알아서, 사용하는 GPU를 결정하는 옵티머스(Optimus) 기술을 사용할 수 있지만 리눅스에서는 약간의 작업을 거쳐야 이 기술을 흉내낼 수 있습니다.
이 작업은 아래 설치 단계에서 설명할 것입니다.

## 설치 준비

여기서는 iso 파일을 다운로드 받아서 usb flash 드라이브에 부팅가능한 라이브 이미지를 만드는 것은 설명하지 않겠습니다.
워낙 많은 분들이 좋은 방법으로 설명을 많이 하셨기 때문입니다.

그래서 이 글에서는 이미 이 설치 usb가 준비되었고, 백업도 완료하고 usb로 부트를 완료했다고 가정하겠습니다.

물론 윈도우를 완전히 밀고 리눅스만 사용하겠다는 결심도 있으셔야합니다.
왜냐하면 듀얼 부팅이 아니라 아치 리눅스만 사용할 것이기 때문입니다.

## 부트 모드 확인하기

제 노트북은 UEFI 부트 모드를 사용합니다.
UEFI 모드로 부트된 것인지 아닌지 확인하려면 다음의 명령어를 사용합니다.

```
# ls /sys/firmware/efi/efivars
```

엄청나게 많은 파일이 확인된다면, UEFI 부트 모드로 부트되었다는 것입니다.

## 인터넷 연결

저는 와이파이를 사용하도록 하겠습니다.

### 드라이버 상태 확인하기
내 와이파이 카드에 필요한 드라이버가 로드되었는지 확인하려면 다음의 명령어를 사용합니다.

```
# lspci -k | grep -iA3 "network"
```

여기서 `k` 옵션은 `man` 명령어를 통해 살펴보니 

> "각 장치를 다루는 커널 드라이버는 물론 그것을 다룰 수 있는 커널 모듈을 보여줍니다."

라는군요.

위 명령어의 출력 결과는 다음과 같았습니다.

```
02:00.0 Network controller: Intel Corporation Wireless 8265 / 8275 (rev 78)
        Subsystem: Intel Corporation Wireless 8265 / 8275
        Kernel driver in use: iwlwifi
        Kernel modules: iwlwifi, wl
```

무선 인터페이스가 만들어졌는지 확인하려면 다음 명령어를 이용합니다.

```
# ip link
```

`wlp2s0`라는 것이 무선 장치처럼 보이네요.
이 인터페이스클 다음의 명령어로 켜봅니다.

```
# ip link set wlp2s0 up
```

결과로 나오는 메시지가 없습니다.
드라이버 모듈 이름인 `iwlwifi`가 어떤 상태인지 알아보려고 다음과 같이 해보았습니다.

```
# dmesg | grep iwlwifi
```

결과는 다음과 같습니다.

```
[    12.870378] iwlwifi 0000:02:00.0: enabling device (0000 -> 0002)
[    13.171346] iwlwifi 0000:02:00.0: loaded firmware version 36.e91976c0.0 op_mode iwlmvm
[    13.252395] iwlwifi 0000:02:00.0: Detected Intel(R) Dual Band Wireless AC 8265, REV=0x230
```

커널 모듈이 잘 로드 되었고 인터페이스가 잘 켜졌네요.

### connect to wifi network

`wifimenu`라는 것을 사용할 수도 있지만, 제 컴퓨터에서는 잘 작동되지 않아서 `netctl` 를 사용했습니다.
집 와이파이에 연결할 것이라 연결 이름을 "home"으로 하겠습니다.

다음의 명령어로 예제를 하나 복사해와서 내용을 고쳐보겠습니다.

```
# cp /etc/netctl/examples/wireless-wpa /etc/netctl/wifi_home
# vim /etc/netctl/wifi_home
```

`vim`이 익숙하지 않으면, `nano` 에디터를 사용하셔도 됩니다.
파일을 열면 여러 줄이 있는데요.
여기에 나온 값들만 적절하게 바꿔줍니다.

저의 경우 `ip link`로 확인한 인터페이스가 `wlp2s0` 였고,
집 와이파이의 ESSID(와이파이 이름)이 "home"이기 때문에 다음과 같이 바꿨습니다.
패스워드는 공개하지 않고, `password`라 쓰겠습니다.

```
...
Interface=wlp2s0
...
ESSID=home
...
Key=password
```

`netctl`을 시작합니다.

```
# netctl start wifi_home
```

에러가 나면 다음 명령어로 에러 내용을 확인합니다.

```
# journalctl -xe
```

에러 메시지가 다음과 같다면,

> "The interface of network profile 'wifi_home' is already up"

다음 명령어를 치고 다시 시도해봅니다.

```
# ip link set wlp2s0 down
```

에러가 안났으면, 다음 명령어로 연결을 확인합니다.

```
# ping google.com
```

연결이 잘 되었습니다.

## 시스템 시계 업데이트하기

다음 명령어를 사용하면 됩니다.

```
# timedatectl set-ntp true
```

## 파티션 나누기

현재 상태를 확인합니다.

```
# fdisk -l
```

파티션을 편집하려면, 다음의 명령어를 사용합니다.

```
# cfdisk /dev/nvme0n1
```

모든 파티션을 삭제하고 다음의 표에 나온 것처럼 새로운 파티션을 만들고 저장한 뒤 나옵니다.

| name | mount point | partition name | size     | file system         | format |
| ---- | ----------- | -------------- | -------- | ------------------- | ------ |
| boot | /boot       | nvme0n1p1      | 550M     | EFI system          | FAT32  |
| swap | none        | nvme0n1p2      | 2G       | Linux swap          | swap   |
| root | /           | nvme0n1p3      | 32G      | Linux root (x86-64) | ext4   |
| home | /home       | nvme0n1p4      | the rest | Linux home          | ext4   |

용량은 원하시는 대로 선택하셔도 되지만, 저는 Arch wiki의 가이드라인을 따라봤습니다.

다 완료되었으면, 다음 명령어로 현재 상태를 확인합니다.

```
# fdisk -l
```

## format the partitions

파티션을 만들었으니 그 공간을 어떻게 채울지 결정할 차례입니다.

위의 표에 나온대로 format을 설정합니다.

```
# mkfs.fat -F32 /dev/nvme0n1p1
# mkswap /dev/nvme0n1p2
# swapon /dev/nvme0n1p2
# mkfs.ext4 /dev/nvme0n1p3
# mkfs.ext4 /dev/nvme0n1p4
```

## mount the partition

이제 하드 드라이브에 아치를 설치하기 위해 마운트를 합니다.

```
# mount /dev/nvme0n1p3 /mnt
# mkdir /mnt/{boot,home}
# mount /dev/nvme0n1p1 /mnt/boot
# mount /dev/nvme0n1p4 /mnt/home
```

# 설치

## 미러(mirror) 선택하기
이 작업을 하지 않고 진행해도 좋지만, 다운로드 속도가 느려 설치에 시간이 굉장히 많이 소요됩니다.
설치 이후의 패키지 설치를 위해서라도 이 작업을 진행하는 것을 추천드립니다.

다음의 명령어로 mirrorlist를 준비합니다.

```
# cp /etc/pacman.d/mirrorlist{,.backup}
# cp /etc/pacman.d/mirrorlist{,.tmp}
# sed -i 's/^Server/#Server/' /etc/pacman.d/mirrorlist.tmp
```

mirrorlist에 올라있는 서버를 위에서부터 차례대로 사용하는 것이어서, 일단 모두 주석처리를 했습니다.
저는 특정 나라들의 서버를 주석 해제하는 방식으로 진행할 것인데요.

```
:sed -i 's/South Korea\n#/South Korea\r/' /etc/pacman.d/mirrorlist.tmp
```

다음의 나라들을 같은 방법으로 주석해제합니다.

- Japan
- Taiwan
- United States

> 팁: 카이스트 서버는 주석처리하시기 바랍니다. 사용해보니 문제가 많았습니다.

이제 `rankmirrors` 를 사용하기 위해 관련 패키지를 설치하고, 이를 이용해 빠른 순서대로 mirrorlist를 정렬합니다.

```
# pacman -Sy pacman-contrib
# rankmirrors -n 5 /etc/pacman.d/mirrorlist.tmp > /etc/pacman.d/mirrorlist
```

## 기본 패키지 설치하기

다음의 명령어로 마운트한 하드디스크에 아치와 패키지를 설치합니다.

```
# pacstrap /mnt base base-devel vim wpa_supplicant
```

- `vim`: 편집용 도구입니다.
- `wpa_supplicant`: 와이파이 연결에 필요한 `netctl`을 사용하는 데 필요합니다.

# 시스템 설정하기

더 진행하기 전에 우리가 사용했던 설정 파일을 하드디스크로 옮깁시다.

```
# cp {,/mnt}/etc/netctl/wifi_home
# cp {,/mnt}/etc/pacman.d/mirrorlist.tmp
# cp {,/mnt}/etc/pacman.d/mirrorlist.backup
```

## Fstab

`Fstab`이라는 것은 잘 알지 못하지만, 마운트 정보에 관한 것이라고만 알고 있습니다.
필요한 작업이니 수행합니다.

```
# genfstab -U /mnt >> /mnt/etc/fstab
```

## chroot

이제 설치된 하드디스크로 옮겨가봅시다.

```
# arch-chroot /mnt
```

## 시간대 설정

시간대 설정은 다음과 같이 합니다.

```
# ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
# hwclock --systohc
```

## 로케일 설정

저는 일단 영어로 로케일을 설정해보겠습니다.
`/etc/locale.gen` 파일을 열어 `en_US.UTF-8 UTF-8`를 코멘트 해제합니다.
그리고 다음 명령어로 로케일을 생성합니다.

```
# locale-gen
```

## 네트워크 설정

네트워크 이름은 `syarch`로 하기로 정했습니다.
다음과 같이 해줍니다.

```
# echo syarch > /etc/hostname
```

`/etc/hosts`도 다음과 같이 편집합니다.

```
127.0.0.1    localhost
::1          localhost
127.0.1.1    syarch.localdomain    syarch
```

## 루트 비밀번호 설정하고 사용자 계정 만들기

제 계정 이름은 "syoh"로 할 것입니다.

```
# passwd
# useradd -m -g users -G wheel -s /bin/bash syoh
# passwd syoh
# visudo
```

그리고 다음의 내용을 주석 해제합니다.

```
# %wheel ALL=(ALL) ALL
```

## 부트 로더 설정

grub을 많이 쓰긴하지만 여기서는 systemd에서 사용하는 bootctl을 사용하도록 하겠습니다.

```
# bootctl install
# vim /boot/loader/loader.conf
```
모든 내용을 삭제하고 다음 내용을 넣습니다.

```
default arch
timeout 2
```

`/boot/loader/entries/arch.conf` 파일을 만들고 다음 내용을 적은 뒤 저장합니다.

```
title Arch Linux
linux /vmlinuz-linux
initrd /initramfs-linux.img
options root=
```

여기서 필요한 것은 우리 루트 파티션의 "PARTUUID"입니다.
이것을 알기위해서는 `blkid`를 사용해야하는데요.
이 내용이 매우 길고 외우기 어렵기 때문에, 복사 붙여넣기 신공을 사용하도록 합시다.

```
# blkid >> /boot/loader/entries/arch.conf
```

이렇게 하면 blkid의 결과가 파일에 들어가게 되는데요.
편집기의 기능을 적절히 이용해 내용를 채우면 됩니다.

이 방법 말고 vim을 사용하는 방법이 있는데요.
다음과 같이 합니다.

커서를 `root=`의 끝에 두고, ESC를 눌러 명령 모드로 갑니다.
그리고 다음 명령어를 사용합니다.

```
:r !blkid
```

그 다음 다음 부분을 복사합니다.

```
PARTUUD="xxxxxxxx-......"
```

다 완성하면 다음과 같은 모습입니다.

```
title Arch Linux
linux /vmlinuz-linux
initrd /initramfs-linux.img
options root=PARTUUD=xxxxxxxx-...... rw
```

## 재부팅

모든 준비가 다 되었으니 재부팅을 시도합니다.

```
# exit
# umount -R /mnt
# reboot
```

# 설치 이후 과정

## 인터넷 연결

이전에 우리가 사용한 방법대로 인터넷에 연결합니다.

```
$ sudo netctl start wifi_home
```

## 인텔 프로세서 마이크로코드 문제 해결

인텔 프로세서에 어떤 문제가 있다고 합니다.
이 문제를 마이크로코드라고 부르는데요.
해결하려면 `intel-ucode` 패키지를 설치하면 됩니다.

다음과 같이 설치합니다.

```
$ sudo pacman -S intel-ucode
```

그리고 `/boot/loader/entries/arch.conf`을 다음과 같이 편집합니다.

```
title Archlinux
linux /vmlinuz-linux
initrd /intel-ucode.img
initrd /initramfs-linux.img
options root=PARTUUID=xxxxxxxx-...... rw
```

이전 내용과 다른 점은 `initrd /initramfs-linux.img` 윗 줄에 `initrd /intel-ucode.img`이 추가되었다는 것입니다.

## Xorg 설치

GUI를 사용하기 위해, 이를 가능하게 하는 "Xorg"를 설치합니다.

```
$ sudo pacman -S xorg xf86-input-libinput
```

- `xf86-input-libinput`: 입력 장치를 위한 라이브러리입니다.

위 명령어를 실행하면 여러 옵션을 물어보는데, 기본 선택지만 선택합니다.

## bumblebee 설치

bumblebee는 엔비디아의 옵티머스 기능을 흉내내서 평소에는 전력소모가 적은 내장 그래픽을 사용하고, 
필요할 때 외장 그래픽을 실행할 수 있는 프로그램입니다.

우선 드라이버가 커널에 잘 들어가 있는지 확인합니다.

```
$ lspci -k | grep -EA2 "(VGA|3D)"
```

두가지 그래픽 카드를 확인할 수 있습니다.

- intel UHD graphics 620
- Geforce MX150

`bumblebee`와 이것에 필요한 패키지를 설치합니다.

```
$ sudo pacman -S bumblebee mesa nvidia xf86-video-intel
```

이 bumblebee를 사용할 권한은 `bunblebee`라는 그룹에만 주도록 되어있습니다.
따라서 제 계정을 이 그룹에 추가하도록 합니다.
그리고 시작할 때 항상 실행되도록 bumblebeed.service를 활성화합니다.

```
$ sudo gpasswd -a syoh bumblebee
$ sudo systemctl enable bumblebeed.service
```

## 디스플레이 관리자 설치: lightdm

디스플레이 관리자는 로그인 관리자로도 불립니다.
GUI 환경이 처음 실행되고 로그인이 필요할 때 실행되기 때문인데요.
이 프로그램을 사용해 어떤 데스크탑 환경(Desktop Environment, DE)를 사용할지 정하게 됩니다.

많은 선택지가 있지만, 저는 `lightdm`이라는 프로그램을 사용하도록 하겠습니다.

```
$ sudo pacman -S lightdm lightdm-gtk-greeter
```

set the default greeter to `lightdm-gtk-greeter`
by modifying `/etc/lightdm/lightdm.conf`

```
[Seat:*]
...
greeter-session=lightdm-gtk-greeter
...
```

enable lightdm on systemd

```
$ sudo systemctl enable lightdm.service
```

## Xorg configuration check

make skeleton for xorg.conf

```
$ sudo Xorg :0 -configure
```

edit touchpad configuration

```
$ sudo ln -s /usr/share/X11/xorg.conf.d/40-libinput.conf /etc/X11/xorg.conf.d/40-libinput.conf
```

and edit `40-libinput.conf` file by following command and add the red-faced lines:

```
$ sudo vim /etc/X11/xorg.conf.d/40-libinput.conf
------------------------------------------------
...
Section "InputClass"
    Identifier "libinput touchpad catchall"
    ...
    Option "NaturalScrolling" "true"
    Option "Tapping" "on"
    ...
```

# install xfce

```
$ sudo pacman -S xfce4 xfce4-goodies xarchiver gvfs
```

and reboot
(gvfs for trash support, mounting removable media)

# install fonts

```
$ sudo pacman -S ttf-liberation
```

# install firefox

```
$ sudo pacman -S firefox
```

# install yay

```
$ sudo pacman -S git
$ git clone https://aur.archlinux.org/yay.git
$ cd yay
$ makepkg -si
```

# install google-chrome

```
$ yay -S google-chrome
```

# install fonts

```
$ sudo pacman -S terminus-font ttf-roboto noto-fonts ttf-liberation ttf-ubuntu-font-family adobe-source-code-pro-fonts
$ yay -S noto-fonts-cjk ttf-d2coding ttf-kopub ttf-nanum ttf-font-awesome-4
```

## font fallback 설정

```
$ mkdir ~/.config/fontconfig
$ vim ~/.config/fontconfig/font.conf
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
    <alias>
        <family>serif</family>
        <prefer>
            <family>Liberation Serif</family>
            <family>KoPubBatang</family>
        </prefer>
    </alias>

    <alias>
        <family>sans-serif</family>
        <prefer>
            <family>Liberation Sans</family>
            <family>Noto Sans CJK KR</family>
        </prefer>
    </alias>

    <alias>
        <family>monospace</family>
        <prefer>
            <family>Liberation Mono</family>
            <family>D2Coding</family>
        </prefer>
    </alias>
</fontconfig>
```

# install dropbox

```
$ yay -S dropbox
$ dropbox &
```

and login

# make chrome default browser

```
$ sudo pacman -S xdg-utils
$ xdg-settings set default-web-browser google-chrome.desktop
```

# hangul setting

```
$ sudo pacman -S python3
$ yay -S nimf
```

add the followings to `~/.xprofile`

```
export GTK_IM_MODULE=nimf
export QT4_IM_MODULE="nimf"
export QT_IM_MODULE=nimf
export XMODIFIERS="@im=nimf"
xmodmap -e 'remove mod1 = Alt_R'
xmodmap -e 'keycode 108 = Hangul'
xmodmap -e 'remove control = Control_R'
xmodmap -e 'keycode 105 = Hangul_Hanja'
```

to setting:

```
$ nimf-settings
```

# install network manager

```
$ sudo pacman -S networkmanager networkmanger-applet
$ sudo systemctl enable NetworkManager
```

and reboot

# install vscode

```
$ sudo pacman -S code
```

# vundle install and set vimrc

```
$ git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
$ vim
```

들어가서 `:PluginInstall`
`vimrc`는 dropbox에 저장해놓은 것에서 가져옴.

# screen 스크롤시 깨짐현상

```
/etc/X11/xorg.conf.d/20-intel.conf
Section "Device"
  Identifier  "Intel Graphics"
  Driver      "intel"
  Option      "TearFree" "true"
EndSection
```

# pulse audio

```
$ sudo pacman -S pulseaudio pavucontrol
```
