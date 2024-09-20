# PAM(Pluggable Authentication Modules)을 사용하여 비밀번호를 8자리 이상으로 규제하기 8️⃣⬇️🙅‍♂️

---
<br>

## 설정 순서 ⚙

### 1️⃣) 기존의 VM을 복제하여 새로운 VM을 만듭니다.

#### ❗❗ ${\textsf{\color{red}새로운 IP할당을 위해 MAC주소 정책의 설정을 모든 네트워크의 어댑터의 새 MAC주소 생성을 선택해줘야 합니다.}}$

<p align="left"><img src="https://github.com/user-attachments/assets/a4bd7949-c07a-48de-81d3-76c0908d070b"></p>

<br>

### 2️⃣) 기존의 VM과 IP가 똑같이 때문에 고정IP를 설정합니다.

<p align="left"><img src="https://github.com/user-attachments/assets/54e63a80-f24d-45ca-9aaf-16fd62e8dd5f"></p>

<br>

#### 00-installer-config.yaml 편집 명령어

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

#### 고정IP 설정
```ymal
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      addresses:
        - 10.0.2.19/24  # 변경된 고정 IP 주소
      routes:
        - to: default
          via: 10.0.2.1  # 게이트웨이
      nameservers:
        addresses:
          - 8.8.8.8
      dhcp4: false
```

<p align="left"><img src="https://github.com/user-attachments/assets/44e2b93b-d44f-43f3-af07-b6a934a5e84b"></p><br>

### 3️⃣) 고정IP 설정이 제대로 되었는지 확인합니다.

<p align="left"><img src="https://github.com/user-attachments/assets/1a203af3-b38c-4d65-9f49-b5e3cf03e550"></p><br>


### 4️⃣) 새로운 VM의 IP를 다른 호스트 포트번호로 포트포워딩 해줍니다.

<p align="left"><img src="https://github.com/user-attachments/assets/c86d6d44-f06c-4712-85d5-e8b4e2d05ca4"></p><br>

### 5️⃣) PAM library를 사용하기위해 libpam-pwquality 설치해줍니다.

```bash
sudo apt update
sudo apt install libpam-pwquality
```

<p align="left"><img src="https://github.com/user-attachments/assets/034e354f-0cc2-46e8-892d-9d668ddb19e6"></p><br>

### 6️⃣) 설치가 제대로 되었는지 확인해줍니다.

```bash
 ls /etc/security/
```

<p align="left"><img src="https://github.com/user-attachments/assets/ff44dacd-37c4-418a-abb4-efa4431f13d6"></p><br>

### 7️⃣) 비밀번호를 8자리 이상으로 설정하기 위해 minlen을 8로 설정해줍니다.

```bash
sudo nano /etc/security/pwquality.conf
```
<p align="left"><img src="https://github.com/user-attachments/assets/0c08a8f7-94f5-46bc-9c28-82a348e1f6eb"></p><br>

### 8️⃣) 설정이 제대로 되었는지 비밀번호변경을 통해 확인해줍니다.

```bash
passpd username
```
<p align="left"><img src="https://github.com/user-attachments/assets/404f4745-4820-4300-916b-0065b4aa876c"></p><br>

---


## 트러블슈팅 🎯

### dpkg 에러로 libpam-pwquality를 설치할 수 없는 상황 

<p align="left"><img src="https://github.com/user-attachments/assets/404f4745-4820-4300-916b-0065b4aa876c"></p><br>

### 해결법 ✨

```bash
sudo dpkg --configure -a
```

- #### 패키지 관리 도구인 dpkg가 이전 작업 중에 중단되어 현재 상태가 불완전하다는 것을 나타냅니다.
- #### 모든 패키지에 대해 아직 설정이 완료되지 않은 패키지들을 설정하거나 필요한 설정 작업을 수행합니다.
