---
title: SSH Key 설정 방법
published: 2023-11-07
description: '내 SSH는 안전하지 않다'
image: ''
tags: []
category: 'Guide'
draft: false 
---

SSH를 쓰다보면
![SSH Login](<./img/Pasted image 20231107132034.png>)
~~SSH 안에선 내가 인싸~~

게속해서 들어오는 무파별 접속 세레머니를 받을 수 있습니다!

비밀번호 방식으로 쓰다보면 언젠간 이렇게 받다가 뚫릴수도 있는데요.

그럼 이후 뚫리면 ~~이 서버는 이제 제껍니다.~~ 상황이 일어날 수 있습니다.

그래서 아주 작은 부분이라도 고치기 위해 SSH_KEY를 설정해보도록 하겠습니다.

## Client 설정

SSH는 기본적으로 설치되어 있으니 여러분이 여기 있으시니 시작해보도록 하겠습니다.

일단 키를 생성해볼건데요. 위 작업은 로그인 할 기기에서 하는 것 입니다.
```bash
bash:~$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/User/.ssh/id_rsa): [Return]
Enter passphrase (empty for no passphrase): [자동 로그인을 하려면 Return]
Enter same passphrase again: [위에서 입력한 값 다시 입력]
Your identification has been saved in /home/User/.ssh/id_rsa
Your public key has been saved in /home/User/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:Secret_KEY_SHA@%^ User@Host
The key's randomart image is:
+---[RSA 3072]----+
|                 |
|                 |
|                 |
|                 |
|                 |
|                 |
|                 |
|                 |
|                 |
+----[SHA256]-----+
```
.ssh 폴더에 가본다면 id_rsa, id_rsa.pub이 생겼습니다.
![File Explorer](<./img/Pasted image 20231107133013.png>)

    💡 다른 컴퓨터에서도 로그인하고 싶다고요?
    간단해요! 그냥 id_rsa을 다른 컴퓨터의 .ssh 폴더에 넣으세요!

## Host 설정
이제 Key를 Remote 서버로 옮길 작업을 해봅시다.

```bash
scp $HOME/.ssh/id_rsa.pub 적용할_유저_이름@호스트:id_rsa.pub
```

그리고 서버에 접속해보면 id_ras.pub이 SSH 기본 경로에 있을 겁니다.
> SSH의 기본 경로는 사용자의 Home 폴더입니다. 예시에선 Home 폴더라는 가정하에 작성합니다.

그리고 서버에서 이 id_rsa.pub를 `cat $HOME/id_rsa.pub >> $HOME/.ssh/authorized_keys`를 사용해 옮겨주세요.

이게 끝이 아니에요!
`sudo` 권한으로 `/etc/ssh/sshd_config`를 수정할거에요.
```bash
sudo nano /etc/ssh/sshd_config
```
여기선 nano로 예시를 들거에요.

![SSH Config](<./img/Pasted image 20231107134539.png>)
이 화면에서 Ctrl + W를 눌러주세요
`PasswordAuthentication yes`를 검색해 주석이 있다면 주석을 삭제하고 `PasswordAuthentication no`로 변경해주세요.

이후 Ctrl+X로 나간 후 일부 Ubuntu Server에선 숨겨진 sshd_config가 있어 수정해야해요.
```bash
sudo nano /etc/ssh/sshd_config.d/50-cloud-init.conf
```
`PasswordAuthentication yes`를 `PasswordAuthentication no`로 바꿔주세요

변경사항을 저장하기 위해 sshd를 재시작해주세요.
```bash
sudo systemctl restart sshd
```

## 접속하기

```bash
ssh -v -i ~/.ssh/id_rsa 적용_된_유저_이름@호스트
```

~/.ssh의 경로는 수정할 필요가 있을 수 있어요 윈도우 환경에선 C:\\Users\\사용자_이름\\.ssh 일거에요! 위 사항을 적용해주세요.

    💡 서버가 너무 많아서 뭐가 맞는 id_rsa가 뭔지 모르겠다고요?
    id_rsa의 위치나 이름을 바꿔도 작동합니다! 

여기로 이렇게 로그인하면 끝!