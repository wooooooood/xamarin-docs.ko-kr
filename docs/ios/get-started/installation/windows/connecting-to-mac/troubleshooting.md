---
title: 연결 문제 해결
description: 이 가이드에서는 연결 및 SSH 문제를 포함하여 새 연결 관리자를 사용하는 동안 발생할 수 있는 문제를 해결하는 단계별 지침을 제공합니다.
ms.topic: article
ms.prod: xamarin
ms.assetid: A1508A15-1997-4562-B537-E4A9F3DD1F06
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: d33f4ba5512985d62575885d44fdcebced8b61ed
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="connection-troubleshooting"></a>연결 문제 해결

_이 가이드에서는 연결 및 SSH 문제를 포함하여 새 연결 관리자를 사용하는 동안 발생할 수 있는 문제를 해결하는 단계별 지침을 제공합니다._

## <a name="log-file-location"></a>로그 파일 위치

- **Mac** – ~/Library/Logs/Xamarin-[MAJOR.MINOR]
- **Windows** – %LOCALAPPDATA%\Xamarin\Logs

로그 파일은 Visual Studio에서 **도움말 &gt; Xamarin &gt; Zip 로그**로 이동하여 찾을 수 있습니다.


## <a name="wheres-the-xamarin-build-host-app"></a>Xamarin 빌드 호스트 앱의 위치

오래된 Xamarin.iOS 버전의 Xamarin 빌드 호스트는 더 이상 필요 없습니다. 이제 Visual Studio는 원격 로그인을 통해 자동으로 에이전트를 배포하고 백그라운드에서 실행합니다. Mac 또는 Windows 컴퓨터에서 실행되는 추가 앱은 없습니다.


## <a name="troubleshooting-remote-login"></a>원격 로그인 문제 해결

> [!IMPORTANT]
> 다음 문제 해결 단계는 새 시스템에서 초기 설치 중에 발생하는 문제에 주로 사용됩니다.  이전에 특정 환경에서 연결을 성공적으로 사용했는데 연결이 갑자기 또는 일시적으로 중지되는 경우 대부분은 바로 다음 방법을 사용하여 문제를 해결할 수 있습니다. 
>   * 아래의 [기존 빌드 호스트 프로세스로 인한 오류](#errors)에 설명된 대로 남은 프로세스를 종료합니다. 
>   * [브로커, IDB, 빌드 및 디자이너 에이전트 지우기](#clearing)의 설명에 따라 에이전트를 지운 후 [MacBuildHost.local에 연결할 수 없습니다. 다시 시도하세요.](#tryagain) 아래의 설명에 따라 유선 연결을 사용하여 IP 주소를 통해 직접 연결합니다.  
> 위의 옵션 중 어떤 것도 문제 해결에 도움이 되지 않으면 [9단계](#stepnine)의 지침에 따라 새 버그 보고서를 작성합니다.

1. Mac에 호환되는 Xamarin.iOS 버전이 설치되어 있는지 확인합니다. Visual Studio 2017에서 이렇게 하려면 Mac용 Visual Studio의 **안정적인** 배포 채널에 있는지 확인합니다. Visual Studio 2015 이하인 경우 두 IDE에서 동일한 배포 채널에 있는지 확인합니다.
    * Mac용 Visual Studio인 경우 **Mac용 Visual Studio > 업데이트 확인...**으로 이동하여 **업데이트 채널**을 확인하거나 변경합니다.
    * Visual Studio 2015 이하인 경우 **도구 > 옵션 > Xamarin > 기타**에서 배포 채널을 확인합니다.

2. Mac에서 **원격 로그인**이 설정되었는지 확인합니다. **이 사용자만**에 대한 액세스를 설정하고, Mac 사용자를 목록 또는 그룹에 포함합니다.

    [![](troubleshooting-images/troubleshooting-image1.png "이 사용자 전용 액세스 설정")](troubleshooting-images/troubleshooting-image1.png#lightbox)

3. 방화벽이 SSH의 기본 포트인 포트 22를 통해 들어오는 연결을 허용하는지 확인합니다.

    [![](troubleshooting-images/troubleshooting-image2.png "방화벽이 포트 22를 통해 들어오는 연결을 허용하는지 확인")](troubleshooting-images/troubleshooting-image2.png#lightbox)

    **서명된 소프트웨어가 들어오는 연결을 수신하도록 자동으로 허용**을 해제하면 OS X은 연결 과정에서 들어오는 연결을 수신할 수 있도록 `mono-sgen` 또는 `mono-sgen32`를 허용해 줄 것을 요청하는 대화 상자를 표시합니다. 이 대화 상자에서 **허용**을 클릭합니다.

    [![](troubleshooting-images/troubleshooting-image4a.png "이 대화 상자에서 허용 클릭")](troubleshooting-images/troubleshooting-image4a.png#lightbox)

4. 해당 Mac에서 사용자 계정으로 로그인하여 GUI 세션이 활성화되었는지 확인합니다.

5. _전체 이름_이 아닌 _사용자 이름_으로 Mac에 연결합니다. 이렇게 하면 악센트 부호가 있는 문자가 포함된 전체 이름의 알려진 제한을 피할 수 있습니다.

    _사용자 이름_은 **Terminal.app**에서 `whoami` 명령을 실행하여 찾을 수 있습니다.

    예를 들어 아래 스크린샷에서 계정 이름은 **Amy Burns**가 아닌 **amyb**입니다.

    [![](troubleshooting-images/troubleshooting-image5a.png "터미널 앱에서 계정 이름 가져오기")](troubleshooting-images/troubleshooting-image5a.png#lightbox)


6. Mac에 사용하는 IP 주소가 올바른지 확인합니다. IP 주소는 Mac의 **시스템 기본 설정 > 공유 > 원격 로그인**에서 찾을 수 있습니다.

    [![](troubleshooting-images/troubleshooting-image17.png "시스템 기본 설정 앱의 IP 주소")](troubleshooting-images/troubleshooting-image17.png#lightbox)

7. Mac의 IP 주소를 확인한 후에는 Windows의 `cmd.exe`에서 해당 주소에 대한 `ping`을 시도합니다.

    ```
    ping 10.1.8.95
    ```
    
    ping이 실패하면 Windows 컴퓨터에서 해당 Mac을 _라우팅_할 수 없는 것입니다. 이 문제는 두 컴퓨터 간의 로컬 영역 네트워크 구성 수준에서 해결해야 합니다. 두 컴퓨터가 같은 로컬 네트워크에 있는지 확인합니다.

8. 다음으로, OpenSSH의 `ssh` 클라이언트가 Windows에서 Mac에 연결할 수 있는지 테스트합니다. 이 프로그램을 설치하는 한 가지 방법은 [Windows용 Git](https://git-for-windows.github.io/)를 설치하는 것입니다. 그런 후 **Git Bash** 명령 프롬프트를 시작하고 사용자 이름 및 IP 주소를 사용하여 해당 Mac으로 `ssh`를 시도할 수 있습니다.

    ```bash
    ssh amyb@10.1.8.95
    ```
    
<a name="stepnine" />

9. **8단계가 성공하면** 연결에 대해 `ls` 같은 간단한 명령을 실행해 볼 수 있습니다.

    ```bash
    ssh amyb@10.1.8.95 'ls'
    ```
    
    그러면 Mac의 홈 디렉터리에 있는 콘텐츠가 나열됩니다. `ls` 명령은 제대로 작동하지만 Visual Studio 연결이 실패하는 경우 Xamarin의 복잡성에 대한 [알려진 문제 및 제한 사항](#knownissues) 섹션을 확인하세요. 문제와 일치하는 내용이 없는 경우 [자세한 정보 표시 로그 파일 확인](#verboselogs)에 설명된 대로 [새 버그 보고서를 작성](https://bugzilla.xamarin.com/newbug)하여 첨부해주세요.

10. **8단계가 실패하면** Mac의 터미널에서 다음 명령을 실행하여 SSH 서버가 _모든_ 연결을 수락하는지 확인할 수 있습니다.

    ```bash
    ssh localhost
    ```
    
11. 8단계는 실패하지만 **10단계가 성공**하는 경우 네트워크 구성 때문에 Windows에서 Mac 빌드 호스트의 포트 22에 액세스할 수 없는 문제일 가능성이 높습니다. 가능한 구성 문제는 다음과 같습니다.

    - OS X 방화벽 설정에서 연결을 허용하지 않습니다. 3단계를 다시 확인해야 합니다.

        경우에 따라 시스템 환경 설정에 표시되는 설정이 실제 동작을 반영하지 않아 OS X 방화벽에 대한 앱별 구성의 상태가 올바르지 않을 수도 있습니다. 구성 파일(**/Library/Preferences/com.apple.alf.plist**)을 삭제하고 컴퓨터를 다시 부팅하면 기본 동작을 복원하는 데 도움이 될 수 있습니다. 파일을 삭제하는 한 가지 방법으로, Finder의 **이동 &gt; 폴더로 이동**에서 **/라이브러리/기본 설정**을 입력한 후 **com.apple.alf.plist** 파일을 휴지통으로 이동합니다.

    - Mac 컴퓨터와 Windows 컴퓨터 사이의 라우터 중 하나의 방화벽 설정이 연결을 차단합니다.

    - Windows 자체에서 원격 포트 22에 대한 아웃바운드 연결을 허용하지 않습니다. 이 상황은 흔치 않습니다. 아웃바운드 연결을 허용하지 않도록 Windows 방화벽을 설정할 수 있지만, 기본 설정은 모든 아웃바운드 연결을 허용하는 것입니다.

    - Mac 빌드 호스트는 `pfctl` 규칙을 통해 모든 외부 호스트에서 포트 22로 액세스하는 것을 허용하지 않습니다. 과거에 `pfctl`을 구성한 것을 알고 있지 않는 이상, 이 가능성은 높지 않습니다.

12. 8단계가 실패하고 **10단계도 실패하면** Mac의 SSH 서버 프로세스가 실행되고 있지 않거나 현재 사용자의 로그인을 허용하도록 구성되지 않았을 가능성이 있습니다. 이 경우 복잡한 다른 가능성을 조사하기 전에 2단계의 원격 로그인을 다시 한 번 확인해야 합니다.

<a name="knownissues" />

## <a name="known-issues-and-limitations"></a>알려진 문제 및 제한 사항

> [!NOTE]
> 이 섹션은 위의 8단계 및 9단계에서 설명한 대로 OpenSSH SSH 클라이언트를 사용하여 Mac 사용자 이름과 암호로 Mac 빌드 호스트에 성공적으로 연결한 경우에만 적용됩니다.

### <a name="invalid-credentials-please-try-again"></a>"잘못된 자격 증명입니다. 다시 시도하세요."

알려진 원인:

- **제한** – 이름에 악센트 부호가 포함된 경우 _전체 이름_ 계정을 사용하여 빌드 호스트에 로그인하려고 시도하면 이 오류가 발생할 수 있습니다. 이는 Xamarin이 SSH 연결에 사용하는 [SSH.NET 라이브러리](https://sshnet.codeplex.com/)의 제한 사항입니다. **해결 방법**: 위의 5단계를 참조하세요.

### <a name="unable-to-authenticate-with-ssh-keys-please-try-to-log-in-with-credentials-first"></a>"SSH 키를 사용하여 인증할 수 없습니다. 먼저 자격 증명으로 로그인해 보세요."

알려진 원인:

- **SSH 보안 제한** - 이 메시지는 Mac의 정규화된 경로 **$HOME/.ssh/authorized\_keys**의 파일 또는 디렉터리 중 하나에서 _기타_ 또는 _그룹_ 구성원에 대한 쓰기 권한이 설정되었음을 의미하는 경우가 가장 많습니다. **일반적인 해결 방법**: Mac의 터미널 명령 프롬프트에서 `chmod og-w "$HOME"` 명령을 실행합니다. 문제를 일으키는 파일 또는 디렉터리에 대한 세부 정보를 보려면 터미널에서 `grep sshd /var/log/system.log > "$HOME/Desktop/sshd.log"` 명령을 실행하고, 데스크톱에서 **sshd.log** 파일을 연 후 "인증 거부됨: 소유권 또는 모드가 잘못됨"을 찾아봅니다.

### <a name="trying-to-connect-never-completes"></a>"연결하는 중..."이 완료되지 않음

- **버그 [#52264](https://bugzilla.xamarin.com/show_bug.cgi?id=52264)** – **시스템 환경 설정 &gt; 사용자 &amp; 그룹**의 Mac 사용자에 대한 **고급 옵션** 상황에 맞는 메뉴의 **로그인 셸**이 **/bin/bash** 이외의 값으로 설정된 경우 Xamarin 4.1에서 이 문제가 발생할 수 있습니다. (Xamarin 4.2부터는 이 시나리오가 "연결할 수 없음" 오류 메시지로 연결됩니다.) **해결 방법**: **로그인 셸**을 **/bin/bash**의 원래 기본값으로 변경합니다.

<a name="tryagain" />

### <a name="couldnt-connect-to-macbuildhostlocal-please-try-again"></a>"MacBuildHost.local에 연결할 수 없습니다. 다시 시도하세요."

보고된 원인:

- **버그** - 일부 사용자가 Active Directory 또는 다른 디렉터리 서비스 도메인 사용자 계정을 사용하여 빌드 호스트에 로그인하려고 시도하면 이 오류 메시지와 함께 로그 파일에 "사용자에 대해 SSH를 구성하는 동안 예기치 않은 오류가 발생했습니다. 세션 작업 시간이 초과되었습니다."라는 구체적인 오류 메시지가 표시되는 것을 목격했습니다. **해결 방법:** 로컬 사용자 계정을 사용하여 빌드 호스트에 로그인합니다.

- **버그** - 일부 사용자가 [연결] 대화 상자에서 Mac 이름을 두 번 클릭하여 빌드 호스트에 연결하려고 시도하면 이 오류가 발생하는 것을 목격했습니다. **가능한 해결 방법**: IP 주소를 사용하여 [수동으로 Mac을 추가](~/ios/get-started/installation/windows/connecting-to-mac/index.md#manual-add)합니다.

- **버그 [#35971](https://bugzilla.xamarin.com/show_bug.cgi?id=35971)** – 일부 사용자가 Mac 빌드 호스트와 Windows 간에 무선 네트워크 연결을 사용하는 동안 이 오류를 경험했습니다. **가능한 해결 방법**: 두 컴퓨터를 유선 네트워크 연결로 이동합니다.

- **버그 [#36642](https://bugzilla.xamarin.com/show_bug.cgi?id=36642)** – Xamarin 4.0에서는 Mac의 **$HOME/.bashrc** 파일에 오류가 있을 때마다 이 메시지가 표시됩니다. (Xamarin 4.1부터는 **.bashrc** 파일의 오류가 더 이상 연결 프로세스에 영향을 주지 않습니다.) **해결 방법**: **.bashrc** 파일을 백업 위치로 이동합니다(또는 이 파일이 필요 없는 경우 파일을 삭제).

- **버그 [#52264](https://bugzilla.xamarin.com/show_bug.cgi?id=52264)** – **시스템 환경 설정 > 사용자 및 그룹**의 Mac 사용자에 대한 **고급 옵션** 상황에 맞는 메뉴의 **로그인 셸**이 **/bin/bash** 이외의 값으로 설정된 경우 이 오류가 발생할 수 있습니다. **해결 방법**: **로그인 셸**을 **/bin/bash**의 원래 기본값으로 변경합니다.

- **제한** – Mac 빌드 호스트가 인터넷에 액세스할 수 없는 라우터에 연결되어 있는 경우(또는 Windows 컴퓨터의 역방향 DNS 조회가 요청되면 Mac에서 사용 중인 DNS 서버의 시간이 초과되는 경우) 이 오류가 발생할 수 있습니다. Visual Studio가 SSH 지문을 검색하고 결국 연결에 실패할 때까지 약 30초가 걸립니다.

    **가능한 해결 방법**: **sshd\_config** 파일에 "UseDNS no"를 추가합니다. 변경하기 전에 이 SSH 설정에 대해 꼭 읽어보아야 합니다. [unix.stackexchange.com/questions/56941/what-is-the-point-of-sshd-usedns-option](http://unix.stackexchange.com/questions/56941/what-is-the-point-of-sshd-usedns-option) 예제를 참조하세요.

    다음 단계는 설정을 변경하는 방법 중 한 가지를 설명합니다. 단계를 완료하려면 Mac에서 관리자 계정으로 로그인해야 합니다.

    1. 터미널 명령 프롬프트에서 `ls /etc/ssh/sshd_config` 및 `ls /etc/sshd_config` 명령을 실행하여 **sshd\_config** 파일의 위치를 확인합니다. 나머지 단계에서는 "해당 파일 또는 디렉터리 없음" 오류 메시지를 반환하지 _않는_ 위치를 사용해야 합니다.

        [![](troubleshooting-images/troubleshooting-image18.png "터미널에서 `ls /etc/ssh/sshd_config` 및 `ls /etc/sshd_config` 실행")](troubleshooting-images/troubleshooting-image18.png#lightbox)

    3. 터미널에서 `cp /etc/ssh/sshd_config "$HOME/Desktop/"` 명령을 실행하여 데스크톱으로 파일을 복사합니다.

    4. 데스크톱의 텍스트 편집기에서 파일을 엽니다. 예를 들어 터미널에서 `open -a TextEdit "$HOME/Desktop/sshd_config"` 명령을 실행할 수 있습니다.

    5. 파일 맨 아래에 다음 줄을 추가합니다.

        ```
        UseDNS no
        ```
        
    6. 새 설정이 적용되도록 `UseDNS yes`를 표시하는 줄을 제거합니다.

    7. 파일을 저장합니다.

    8. 터미널에서 `sudo cp "$HOME/Desktop/sshd_config" /etc/ssh/sshd_config` 명령을 실행하여 편집된 파일을 원래 위치로 복사합니다. 암호를 요구하는 메시지가 표시되면 암호를 입력합니다.

    9. **시스템 환경 설정 &gt; 공유 &gt; 원격 로그인**에서 **원격 로그인**을 해제했다가 다시 설정하여 SSH 서버를 다시 시작합니다.

<a name="clearing" />

### <a name="clearing-the-broker-idb-build-and-designer-agents-on-the-mac"></a>Mac에서 브로커, IDB, 빌드 및 디자이너 에이전트 지우기

Mac 에이전트와 관련된 "설치", "업로드" 또는 "시작" 단계에서 로그 파일에 문제가 발생하는 경우 Visual Studio가 에이전트를 강제로 다시 시작하도록 **XMA** 캐시 폴더를 삭제하는 방법을 시도해 볼 수 있습니다.

1. Mac의 터미널에서 다음 명령을 실행합니다.

    ```bash
    open "$HOME/Library/Caches/Xamarin"
    ```
    
2. 컨트롤 키를 누른 상태로 **XMA** 폴더를 클릭하고 **휴지통으로 이동**을 선택합니다.

    [![](troubleshooting-images/troubleshooting-image8.png "XMA 폴더를 휴지통으로 이동")](troubleshooting-images/troubleshooting-image8.png#lightbox)

3. 또한 Windows에는 지우는 것이 더 좋은 캐시가 있습니다. Windows에서 관리자 권한으로 명령 프롬프트를 엽니다.

    ```
    del %localappdata%\Temp\Xamarin\XMA
    ```
    
## <a name="warning-messages"></a>경고 메시지

이 섹션에서는 출력 창 및 일반적으로 무시해도 되는 로그에 나타날 수 있는 몇 가지 메시지에 대해 설명합니다.

### <a name="there-is-a-mismatch-between-the-installed-xamarinios--and-the-local-xamarinios"></a>"...설치된 Xamarin.iOS와 로컬 Xamarin.iOS가 일치하지 않습니다."

Mac과 Windows를 동일한 Xamarin 배포 채널로 업데이트한 것을 확인했다면 이 경고를 무시해도 됩니다.

### <a name="failed-to-execute-ls-usrbinmono-exitstatus1"></a>"'ls /usr/bin/mono' 실행 실패: ExitStatus=1"

Mac에서 OS X 10.11(El Capitan) 이상을 실행 중이면 이 메시지를 무시해도 됩니다. OS X 10.11에서는 이 메시지가 표시되어도 아무 문제 없습니다. OS X 10.11의 `mono`에 대한 올바른 예상 위치인 **/usr/local/bin/mono**를 Xamarin에서도 검사하기 때문입니다.

### <a name="bonjour-service-macbuildhost-did-not-respond-with-its-ip-address"></a>"Bonjour 서비스 'MacBuildHost'가 IP 주소에 응답하지 않았습니다."

[연결] 대화 상자에 Mac 빌드 호스트의 IP 주소가 표시되는 한, 이 메시지를 무시해도 됩니다. 대화 상자에 IP 주소가 _없는_ 경우 [수동으로 Mac을 추가](~/ios/get-started/installation/windows/connecting-to-mac/index.md#manual-add)할 수 있습니다.

### <a name="invalid-user-a-from-101895-and-inputuserauthrequest-invalid-user-a-preauth"></a>"10.1.8.95의 사용자 a는 잘못된 사용자입니다." 및 "input\_userauth\_request: 잘못된 사용자 a [사전 인증]"

**sshd.log**를 살펴볼 때 이 메시지를 발견할 수 있습니다. 이러한 메시지는 정상적인 연결 프로세스의 일부입니다. Xamarin이 _SSH 지문_을 검색할 때 임시로 사용자 이름 **a**를 사용하기 때문에 이러한 메시지가 표시됩니다.

## <a name="output-window-and-log-files"></a>출력 창 및 로그 파일

Visual Studio가 빌드 호스트에 연결할 때 오류가 발생하면 출력 창 및 로그 파일 2곳에서 추가 메시지를 확인해야 합니다.

### <a name="output-window"></a>출력 창

출력 창은 시작하기에 가장 좋은 위치입니다. 출력 창에는 주 연결 단계 및 오류에 대한 메시지가 표시됩니다. 출력 창에서 Xamarin 메시지를 보려면:

1. 메뉴에서 **보기 > 출력**을 선택하거나 **출력** 탭을 클릭합니다.
2. **다음에서 출력 보기** 드롭다운 메뉴를 클릭합니다.
3. **Xamarin**을 선택합니다.

[![](troubleshooting-images/troubleshooting-image11.png "출력 탭에서 Xamarin 선택")](troubleshooting-images/troubleshooting-image11.png#lightbox)

### <a name="log-files"></a>로그 파일

출력 창의 정보가 문제를 진단하기에 충분하지 않은 경우 그 다음으로 살펴볼 위치는 로그 파일입니다. 로그 파일에는 출력 창에 나타나지 않는 추가 진단 메시지가 포함되어 있습니다. 로그 파일을 보려면:

1. Visual Studio를 시작합니다.

    > [!IMPORTANT]
    > **.svclogs**는 기본적으로 사용되지 않습니다. 이 파일에 액세스하려면 [버전 로그](~/cross-platform/troubleshooting/questions/version-logs.md#visual-studio-startup-verbose-logs) 가이드에 설명된 대로 자세한 로그를 사용하여 Visual Studio를 시작해야 합니다. 자세한 내용은 [활동 로그를 사용하여 확장 문제 해결](https://blogs.msdn.microsoft.com/visualstudio/2010/02/24/troubleshooting-extensions-with-the-activity-log/) 블로그를 참조하세요.

2. 빌드 호스트에 연결을 시도합니다.

3. Visual Studio에서 연결 오류가 발생하면 **도움말 > Xamarin > Zip 로그**에서 로그를 수집합니다.

    [![](troubleshooting-images/troubleshooting-image12.png "도움말에서 로그 수집 > Xamarin > Zip 로그")](troubleshooting-images/troubleshooting-image12.png#lightbox)

4. .zip 파일을 열면 아래 예제와 비슷한 파일 목록이 표시됩니다. 연결 오류에서 가장 중요한 파일은 **\*Ide.log** 및 **\*Ide.svclog** 파일입니다. 두 파일에는 동일한 메시지가 약간 다른 형식으로 포함되어 있습니다. **.svclog**는 XML이고, 메시지를 탐색하려는 경우에 유용합니다. **.log**는 일반 텍스트이고, 명령줄 도구를 사용하여 메시지를 필터링하려는 경우에 유용합니다.

    모든 메시지를 탐색하려면 **.svclog** 파일을 선택하여 엽니다.

    [![](troubleshooting-images/troubleshooting-image13.png "svclog 파일 선택")](troubleshooting-images/troubleshooting-image13.png#lightbox)

5. **Microsoft Service Trace Viewer**에서 **.svclog** 파일이 열립니다. 메시지를 스레드 단위로 검색하면서 관련 메시지 그룹을 살펴볼 수 있습니다. 스레드 단위로 검색하려면 **Graph** 탭을 선택한 다음, **레이아웃 모드** 드롭다운 메뉴를 클릭하고 **스레드**를 선택합니다.

    [![](troubleshooting-images/troubleshooting-image14.png "레이아웃 모드 드롭다운 메뉴를 클릭하고 스레드 선택")](troubleshooting-images/troubleshooting-image14.png#lightbox)

<a name="verboselogs" />

### <a name="verbose-log-files"></a>자세한 정보 표시 로그 파일

일반 로그 파일이 제공하는 정보가 문제를 진단하기에 충분하지 않은 경우 마지막 방법으로 자세한 정보 로깅을 사용하도록 설정합니다. 자세한 정보 로그는 버그 보고서에서도 선호되는 정보입니다.

1. Visual Studio를 종료합니다.

2. [**개발자 명령 프롬프트**](https://msdn.microsoft.com/en-us/library/ms229859(v=vs.110).aspx)를 시작합니다.

3. 명령 프롬프트에서 다음 명령을 실행하여 자세한 정보 로깅으로 Visual Studio를 시작합니다.

    ```bash
    devenv /log
    ```

4. Visual Studio에서 빌드 호스트에 연결을 시도합니다.

5. Visual Studio에서 연결 오류가 발생하면 **도움말 > Xamarin > Zip 로그**에서 로그를 수집합니다.

6. Mac의 터미널에서 다음 명령을 실행하여 SSH 서버에 있는 최근 로그 메시지를 데스크톱에 있는 파일로 복사합니다.

    ```bash
    grep sshd /var/log/system.log > "$HOME/Desktop/sshd.log"
   ```

자세한 정보 로그 파일이 제공하는 정보가 문제를 직접 해결하기에 충분하지 않은 경우 [새 버그 보고서를 작성](https://bugzilla.xamarin.com/newbug)하고 5단계의 .zip 파일과 6단계의 .log 파일을 첨부해 주세요.

## <a name="troubleshooting-build-and-deployment-errors"></a>빌드 및 배포 오류 문제 해결

이 섹션에서는 Visual Studio가 빌드 호스트에 성공적으로 연결된 후 발생할 수 있는 몇 가지 문제를 다룹니다.

### <a name="unable-to-connect-to-address1921681222-with-usermacuser"></a>"사용자 'macuser'로 주소 '192.168.1.2:22'에 연결할 수 없습니다."

알려진 원인:

- **Xamarin 4.1 보안 기능** - Xamarin 4.1 이상을 사용한 후 Xamarin 4.0으로 다운그레이드하면 이 오류가 _발생합니다_. 이 경우 오류와 함께 추가 경고 "개인 키는 암호화되었지만 암호가 비어 있습니다."가 표시됩니다. 이는 Xamarin 4.1의 새로운 보안 기능으로 인한 _의도적인_ 변경 내용입니다. **권장 해결 방법**: **%LOCALAPPDATA%\Xamarin\MonoTouch**에서 **id\_rsa** 및 **id\_rsa.pub**을 삭제한 후 Mac 빌드 호스트에 다시 연결합니다.

- **SSH 보안 제한** - 이 메시지와 함께 추가 경고 "기존 ssh 키를 사용하여 사용자를 인증할 수 없습니다."가 표시되는 경우 Mac의 정규화된 경로 **$HOME/.ssh/authorized\_keys**의 파일 또는 디렉터리 중 하나에서 _기타_ 또는 _그룹_ 구성원에 대한 쓰기 권한이 설정되었음을 의미하는 경우가 가장 많습니다. **일반적인 해결 방법**: Mac의 터미널 명령 프롬프트에서 `chmod og-w "$HOME"` 명령을 실행합니다. 문제를 일으키는 파일 또는 디렉터리에 대한 세부 정보를 보려면 터미널에서 `grep sshd /var/log/system.log > "$HOME/Desktop/sshd.log"` 명령을 실행하고, 데스크톱에서 **sshd.log** 파일을 연 후 "인증 거부됨: 소유권 또는 모드가 잘못됨"을 찾아봅니다.

### <a name="solutions-cannot-be-loaded-from-a-network-share"></a>네트워크 공유에서 솔루션을 로드할 수 없습니다.

솔루션이 로컬 Windows 파일 시스템 또는 매핑된 드라이브에 있는 경우에만 컴파일됩니다.

네트워크 공유에 저장된 솔루션은 오류를 throw 하거나 컴파일을 완전히 거부합니다. Visual Studio에서 사용되는 모든 **.sln** 파일은 로컬 Windows 파일 시스템에 저장해야 합니다.

이 문제 때문에 다음 오류가 throw 됩니다.

```bash
error : Building from a network share path is not supported at the moment. Please map a network drive to '\\SharedSources\HelloWorld\HelloWorld' or copy the source to a local directory.
```

관련 버그: [#36195](https://bugzilla.xamarin.com/show_bug.cgi?id=36195)

### <a name="missing-provisioning-profiles-or-failed-to-create-the-a-fat-library-error"></a>프로비전 프로필이 없거나 "the a fat 라이브러리를 만들지 못했습니다" 오류

Mac에서 Xcode를 시작한 후 Apple 개발자 계정에 로그인되었는지 그리고 iOS 개발 프로필이 다운로드되었는지 확인합니다.

[![](troubleshooting-images/troubleshooting-image7.png "Apple 개발자 계정에 로그인되었는지 및 iOS 개발 프로필이 다운로드되었는지 확인")](troubleshooting-images/troubleshooting-image7.png#lightbox)

### <a name="a-socket-operation-was-attempted-to-an-unreachable-network"></a>"연결할 수 없는 네트워크에서 소켓 작업을 시도했습니다."

보고된 원인:

- **향상된 기능 [#36118](https://bugzilla.xamarin.com/show_bug.cgi?id=36118)** - Visual Studio에서 IPv6 주소를 사용하여 빌드 호스트에 연결하는 경우 이 오류 때문에 성공한 빌드가 차단될 수 있습니다. (빌드 호스트 연결에서는 아직 IPv6 주소를 지원하지 않습니다.)

### <a name="xamarinios-visual-studio-plugin-fails-to-load-after-reinstallation-of-betaalpha-channel"></a>베타/알파 채널을 다시 설치한 후 Xamarin.iOS Visual Studio 플러그 인이 로드되지 않음

관련 버그 [#40781](https://bugzilla.xamarin.com/show_bug.cgi?id=40781).

Visual Studio가 MEF 구성 요소 캐시 새로 고침에 실패할 경우 이 문제가 발생할 수 있습니다. 그러한 경우 다음 Visual Studio 확장을 설치하면 도움이 될 수 있습니다. [https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd)

이렇게 하면 Visual Studio MEF 구성 요소 캐시를 삭제하여 캐시 손상으로 인한 문제가 해결됩니다.

<a name="errors" />

### <a name="errors-due-to-existing-build-host-processes-on-the-mac"></a>Mac에 있는 기존 빌드 호스트 프로세스로 인한 오류

경우에 따라 이전 빌드 호스트 연결의 프로세스가 현재 활성 연결의 동작을 방해할 수 있습니다. 기존 프로세스를 확인하려면 Visual Studio를 닫고 Mac의 터미널에서 다음 명령을 실행합니다.

```bash
ps -A | grep mono
```

[![](troubleshooting-images/troubleshooting-image10.png "Mac의 터미널에서 명령 실행")](troubleshooting-images/troubleshooting-image10.png#lightbox)

기존 프로세스를 종료하려면 다음 명령을 사용합니다.

```bash
killall mono
```

### <a name="clearing-the-mac-build-cache"></a>Mac 빌드 캐시 지우기

빌드 문제를 해결하고 동작이 Mac에 저장된 임시 빌드 파일과 관련되지 않게 하려면 빌드 캐시 폴더를 삭제합니다.

1. Mac의 터미널에서 다음 명령을 실행합니다.

    ```bash
    open "$HOME/Library/Caches/Xamarin"
    ```

2. 컨트롤 키를 누른 상태로 **mtbs** 폴더를 클릭하고 **휴지통으로 이동**을 선택합니다.

    [![](troubleshooting-images/troubleshooting-image9.png "mtbs 폴더를 휴지통으로 이동")](troubleshooting-images/troubleshooting-image9.png#lightbox)


## <a name="related-links"></a>관련 링크

- [Mac에 연결](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
- [XMA를 사용하여 Mac을 Visual Studio 환경에 연결(비디오)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
