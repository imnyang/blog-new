---
title: Promox VE로 VM 생성하기 + Ubuntu Server 24.04 
published: 2024-06-05
description: 'Proxmox로 나만의 VM 생성하기'
image: 'img/preview.png'
tags: [Linux, Server, "Proxmox"]
category: 'Guide'
draft: false 
---

## 준비물

1. [Proxmox VE가 설치된 서버](/posts/proxmox-ve-install/)
2. [ISO 파일 (예시로는 우분투 서버)](https://mirror.imnyang.xyz/ubuntu-cd/24.04/ubuntu-24.04-live-server-amd64.iso)

![Proxmox VE Main Page](<./img/web/panel.png>)
저번에 우리는 Proxmox VE 설정을 끝냈습니다.
이번엔 VM 생성을 해보도록 하겠습니다.

## Proxmox VE에 ISO 다운받기

우리가 운영체제를 설치하려면 설치 디스크가 필요하듯이 VM도 똑같이 필요합니다.
그중 ISO라는 파일이 필요한데요. 여기선 [Ubuntu Server 24.04](https://mirror.imnyang.xyz/ubuntu-cd/24.04/ubuntu-24.04-live-server-amd64.iso)로 해보겠습니다.

먼저 Proxmox VE 패널로 접속해줍시다.

![Find disk](<./img/web/find-disk.png>)
여기서 ISO Image를 지원하는 디스크를 선택해주세요.
예시로 local로 하겠습니다.

![Download ISO](<./img/web/download-button.png>)
이 버튼을 클릭하고

`https://mirror.imnyang.xyz/ubuntu-cd/24.04/ubuntu-24.04-live-server-amd64.iso`

원하는 ISO 다이렉트 링크를 넣어주세요.

![Before Query](<./img/web/before-query.png>)

넣은 후 `Query URL` 버튼을 눌러주세요.

![After Query](<./img/web/after-query.png>)

이렇게 잘 나왔다면 `Download`를 눌러주세요.

![TASK OK](<./img/web/download.png>)

`TASK OK`가 출력되었다면 창을 닫아주세요.

Ubuntu Server 24.04 LTS 다운로드가 완료되었습니다.

## Proxmox VE에 VM 생성하기

![Create VM Button](<./img/gen/create-vm.png>)
우측 상단 위 이 버튼을 통해 생성합니다.

## Info

![Set Name](<./img/gen/set-info.png>)
원하는 VM ID와 Name을 설정해줍니다.
여기선 아래와 같은 설정으로 진행합니다.
| Name     | Value   |
| -------- | ------- |
| Node     | pve     |
| VM ID    | 101     |
| Name     | Ubuntu  |

## Set Bootable ISO

![Select ISO](<./img/gen/select-iso.png>)
우리가 아까 다운로드 받았던 ISO를 선택해줍시다.

### System

![System](<./img/gen/set-system.png>)
일부 설정은 운영체제에서 요구하는 경우 변경해주시길 바랍니다.
| Name     | Value   |
| -------- | ------- |
| Machine  | q35     |
| BIOS     | SeaBIOS (UEFI 필요시 변경 필요) |
| Add TPM  | False (필요시 변경 필요) |
| 나머지   | Default  |

### Disk

![Add Disk](<./img/gen/add-disk.png>)
디스크도 원하는 만큼 설정해주세요.

| Name     | Value   |
| -------- | ------- |
| Disk Size (GiB)  | 32 (필요시 변경 필요) |

### CPU

![CPU setting](<./img/gen/set-CPU.png>)

| Name     | Value   |
| -------- | ------- |
| Sockets  | 1 |
| Cores  | 1 |
| Type  | host |

### Memory

![Memory setting](<./img/gen/set-memory.png>)
메모리는 원하는 만큼 설정해주시길 바랍니다.

| Name     | Value   |
| -------- | ------- |
| Memory (MiB)  | 2048 |

### Network

![Network](<./img/gen/set-network.png>)
되도록이면 기본 값으로 설정해주세요. (만약 이 값을 건드리신다면 이미 뭘 하는지 알고 있을거라고 생각합니다.)

## Confirm
![Last](<./img/gen/confirm.png>)
Finish를 눌러주세요.

## 마무리

![Generated](<./img/gen/generated.png>)

생성되었습니다!
이제 Start를 누르고 Console에서 여러분의 VM을 접근하고 Ubuntu Server 24.04 LTS를 사용할 준비가 되었습니다.