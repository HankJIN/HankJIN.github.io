---
title:  "윈도우 10 개발환경 설정"
excerpt: "chocolatey와 PowerShell로 WSL2 부터 onMyzsh까지 세팅하기."

categories:
  - HOWTO
tags:
  - bash
  - shell
  - toy Project
last_modified_at: 2020-9-18 T17:30:00-05:00
toc: true
toc_sticky: true
---

---

<span style="color:grey; font-size:12px;">
 계속해서 윈도우 8.1을 사용하다 드디어 윈도우 10을 사용하게 됐다. 이런 설정방법은 한번 설정해두면 다음번에 해매지 않고 진행 가능하기에 이번 기회에 정리해둔다.
</span>

---
<br><br><br>

## 환경
 노트북 : 포맷 후 윈도우 10 Home으로 업데이트 된 상태  
 데스크탑 : 윈도우 10 pro가 설치된 기본 상태  
 __필수조건__ : BIOS세팅에서 가상화를 지원해야 됩니다.
<br><br>

# chocolatey
 __chocolatey__ 는 설치, 실행, zip파일 그리고 스크립트를 컴파일된 패키징으로 래핑하는 윈도우OS를 위한 소프트웨어 관리 자동화 도구이다.  
  어렵게 말했지만, CLI를 통해 윈도우 소프트웨어들을 설치, 삭제하기위한 도구로 볼 수 있다.  

## chocolatey 설치 
 [chocllatey 공식 홈페이지](https://chocolatey.org/install) 에서 나오는 Chocolatey Install:Individual에 있는 커맨드를 PowerShell에 입력하면 된다. 업데이트시마다 커맨드가 변경되는듯하다.<br> 반드시 Powershell을 관리자 권한으로 실행해야된다.  
현재 기준 다음과 같은 명령어로 chocolatey를 설치 가능하다.

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```
## chocolatey로 필수 파일 설치  
1. 설치하고자 하는 파일을 [링크](https://chocolatey.org/packages?q=)에서 검색을 한다.
2. 검색 결과중 원하는 버전의 설치명령어를 확인 후, __관리자 권한__ 이 있는 PowerShell에서 해당 명령어를 실행한다.

- 추천 설치 파일 목록
  ```powershell
  choco install vscode  #VisualStudioCode
  choco install git.install #git Hub
  choco install microsoft-windows-terminal
  #윈도우 터미널 : WSL, CMD, PowerShell 등을 지원하는 터미널 에뮬레이터이다.
  #개발시 윈도우로 CLI를 진행하고자 하므로 설치하자.
  choco install winrar
  choco install putty.install
  #putty : SSH을 지원하는 응용프로그램이다.
  #클라우드플랫폼의 리눅스를 컨트롤할때 매우 유용했다.
  choco install mingw
  # 자주쓰는 언어인 C와 C++를 컴파일 하기위한 도구.
  ```
<br><br>

# WSL 설치

> Windows SubSystem for Linux로 윈도우에서 리눅스를 구동할 수 있게 도와주는 도구이다. 현재 WSL2 버전이 나와있다.  
[WSL1과 WSL2의 차이](https://docs.microsoft.com/ko-kr/windows/wsl/compare-versions)  

## WSL 설치
 0. 바이오스에서 가상화를 설정한다.
 1. 관리자 권한으로 실행한 윈도우 터미널에서 PowerShell로 실행 후 다음을 입력한다.
    ```powershell
    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
    ```
    ![install WSL](https://user-images.githubusercontent.com/42710591/94368160-c5414300-011d-11eb-941e-814d7159f630.PNG)

 2. 가상머신플랫 폼 가능하도록 설정
    ```powershell
    dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
    ```
    위의 명령어를 입력 후, 재 부팅한다.  
    `wsl --set-default-version 2` 를 Powershell에 입력하여, WSL의 기본 실행버전을 wSL2로 변경해준다.
  3. MS스토어 에서 원하는 버전의 Linux 배포판을 선택한다. 이후 Ubuntu로 진행하였다.
  
  4. 재부팅 후, 설치된 Ubuntu를 실행하여, 초기 설정을 진행하고 종료한다.  
  ![ubuntu Instell](https://user-images.githubusercontent.com/42710591/94368164-c7a39d00-011d-11eb-90a2-0339ff33ab7b.PNG)
  (재부팅이 권장 사항은 아니지만, 바로 실행시 에러가 발생했다.)
  ![setting_ubuntuError](https://user-images.githubusercontent.com/42710591/94368162-c6727000-011d-11eb-8c23-e4e7cfd45bbd.PNG)
  5. PowerShell에서 다음을 진행한다.
      ```powershell
      wsl --list --verbose
      #배포에 해당되는 리눅스 버전을 확인한다.
      # 2로 표기 되어있으면 정상 아닌경우 다음 명령어 입력
      wsl --set-default-version 2
      #명령어로  WSL2을 기본아키텍처로 설정
      ```
   ![우분투 설치후 윈도우터미널](https://user-images.githubusercontent.com/42710591/94368161-c5d9d980-011d-11eb-9c84-808973b6a77b.png)
    우분투가 확인이 되면 정상적으로 설치!<br>  


## 터미널 커스터마이징

### 윈도우 터미널 커스터마이징
  0. 윈도우 터미널에서 `ctrl + ,` 혹은 설정으로 진입하여 setting.json파일을 연다.
  1. defaultProfile을 profile의 ubuntu의 guid로 변경
2. 컬러스키마 설정  
[terminal Spalsh](https://terminalsplash.com/)에서 원하는 색조합을 선택하고, 복사하여 shceme에 추가한다.  
추가한 scheme의 name속성을 확인하고, profile내 default에 "colorScheme":"scheme name"추가.  
![post_windowTerminal_JsonSetting](https://user-images.githubusercontent.com/42710591/94368163-c70b0680-011d-11eb-9a91-c38be7fab1ed.png)  
<br> 

### wsl2 Ubuntu 커스터마이징
0. windows terminal로 Ubuntu(WSL2)를 실행.
1. z셸 설치  
``sudo apt install zsh``  
Zsh는 bash, ksh, tcsh의 일부 기능을 포함하여 수많은 개선 사항이 갖추어진 확장형 본 셸이다.
2. oh my zsh 설치  
 ``sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"``  
위의 명령어는 [링크](https://ohmyz.sh/#install)에서 확인 가능하다.
3. powerlevel10k 설치  
 업데이트에 따라 버전이 변경 될 수 있으므로, [공식 git](https://github.com/romkatv/powerlevel10k)참조를 권장합니다.  
    1. 설치  
``git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k 
code ~/.zshrc ``  
    2. 폰트 설치  
[MesloLGS NF regular](https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Regular.ttf) / 
[MesloLGS NF bold](https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold.ttf) / 
[MesloLGS NF italic](https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Italic.ttf) / 
[MesloLGS NF bold italic](https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold%20Italic.ttf)
4. WSL2를 실행시켜 powerlevel10k의 설정을 진행하면, 아름다운 CLI창을 사용 가능하다.
5. vscode 터미널에서 WSL을 실행시에도 폰트 설정이 필요하므로 `ctrl + ,`로 설정에 진입. ``terminal.integrated.fontFamily``의 값을 MesloLgs NF로 설정한다.
<br><br><br><br><br>

## reference  
>https://chocolatey.org  
https://docs.microsoft.com/ko-kr/windows/wsl/install-win10  
https://ohmyz.sh/  
https://github.com/romkatv/powerlevel10k  