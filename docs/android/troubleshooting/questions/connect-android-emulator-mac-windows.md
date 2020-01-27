---
title: Windows VM에서 Mac에서 실행 중인 Android 에뮬레이터에 연결할 수 있나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7B6752BB-8E4C-4690-B275-7E425A051F45
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/21/2018
ms.openlocfilehash: 49d1eea60f766f4cb61484a6e441506cf8f046ff
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725080"
---
# <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>Windows VM에서 Mac에서 실행 중인 Android 에뮬레이터에 연결할 수 있나요?

Windows 가상 머신에서 Mac에서 실행 되는 Android Emulator에 연결 하려면 다음 단계를 사용 합니다.

1. Mac에서 에뮬레이터를 시작 합니다.

2. Mac에서 `adb` 서버를 중지 합니다.

    ```bash
    adb kill-server
    ```

3. 에뮬레이터는 루프백 네트워크 인터페이스의 TCP 포트 2 개에서 수신 대기 합니다.

    ```bash
    lsof -iTCP -sTCP:LISTEN -P | grep 'emulator\|qemu'

    emulator6 94105 macuser   20u  IPv4 0xa8dacfb1d4a1b51f      0t0  TCP localhost:5555 (LISTEN)
    emulator6 94105 macuser   21u  IPv4 0xa8dacfb1d845a51f      0t0  TCP localhost:5554 (LISTEN)
    ```

    번호가 매겨진 포트는 `adb`연결 하는 데 사용 됩니다. [https://developer.android.com/tools/devices/emulator.html#emulatornetworking](https://developer.android.com/tools/devices/emulator.html#emulatornetworking)도 참조 하세요.

4. _옵션 1_: `nc`을 사용 하 여 포트 5555 (또는 원하는 다른 포트)에서 외부에서 받은 인바운드 TCP 패킷을 루프백 인터페이스의 홀수 번호 포트 (이 예제에서는**127.0.0.1 5555** )로 전달 하 고 아웃 바운드 패킷을 다른 방식으로 다시 전달 합니다.

    ```bash
    cd /tmp
    mkfifo backpipe
    nc -kl 5555 0<backpipe | nc 127.0.0.1 5555 > backpipe
    ```

    터미널 창에서 `nc` 명령이 실행 되 고 있으면 패킷이 예상 대로 전달 됩니다. 에뮬레이터를 사용 하 여 완료 한 후에는 터미널 창에서 Control-C를 입력 하 여 `nc` 명령을 종료할 수 있습니다.

    옵션 1은 일반적으로 옵션 2 보다 간단 합니다. 특히 **시스템 기본 설정 > 보안 & 개인 정보 보호 > 방화벽** 을 설정 합니다.

    _옵션 2_: `pfctl`을 사용 하 여 [공유 네트워킹](https://kb.parallels.com/en/4948) 인터페이스의 포트 `5555` (또는 원하는 다른 포트)에서 루프백 인터페이스의 홀수 번호 포트 (이 예제에서는`127.0.0.1:5555`)로 TCP 패킷을 리디렉션합니다.

    ```bash
    sed '/rdr-anchor/a rdr pass on vmnet8 inet proto tcp from any to any port 5555 -> 127.0.0.1 port 5555' /etc/pf.conf | sudo pfctl -ef -
    ```

    이 명령은 `pf packet filter` 시스템 서비스를 사용 하 여 포트 전달을 설정 합니다. 줄 바꿈이 중요 합니다. 복사 하 여 붙여 넣을 때 그대로 유지 해야 합니다. 또한 위 도선를 사용 하는 경우 *vmnet8* 에서 인터페이스 이름을 조정 해야 합니다. `vmnet8`는 VMWare Fusion의 *공유 네트워킹* 모드용 특수 *NAT 장치의* 이름입니다. [Vnic0](https://download.parallels.com/doc/psbm/en/Parallels_Server_Bare_Metal_Users_Guide/29258.htm)에 적절 한 네트워크 인터페이스가 있을 가능성이 높습니다.

5. Windows 컴퓨터에서 에뮬레이터에 연결 합니다.

    ```cmd
    C:\> adb connect ip-address-of-the-mac:5555
    ```

    "Ip 주소-mac"을 Mac의 IP 주소로 바꿉니다 (예: `ifconfig vmnet8 | grep 'inet '`에 나열). 필요한 경우 `5555`를 4 단계에서 원하는 다른 포트로 바꿉니다\. (참고: `adb`에 대 한 명령줄 액세스를 가져오는 한 가지 방법은 Visual Studio에서 android [ **> Android Adb 명령 프롬프트 > 도구**](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat) 를 사용 하는 것입니다.)

### <a name="alternate-technique-using-ssh"></a>`ssh`를 사용 하는 대체 기술

Mac에서 _원격 로그인_ 를 사용 하도록 설정한 경우 `ssh` 포트 전달을 사용 하 여 에뮬레이터에 연결할 수 있습니다.

1. Windows에 SSH 클라이언트를 설치 합니다. 한 가지 옵션은 [Windows 용 Git](https://git-for-windows.github.io/)를 설치 하는 것입니다. 그러면 **Git Bash** 명령 프롬프트에서 `ssh` 명령을 사용할 수 있습니다.

2. 위의 1-3 단계를 수행 하 여 에뮬레이터를 시작 하 고, Mac에서 `adb` 서버를 중지 하 고, 에뮬레이터 포트를 확인 합니다.

3. Windows에서 `ssh`를 실행 하 여 Windows의 로컬 포트 (이 예제에서는`localhost:15555`)와 Mac의 루프백 인터페이스에 있는 홀수 번호 에뮬레이터 포트 (이 예제에서는`127.0.0.1:5555`) 간에 양방향 포트 전달을 설정 합니다.

    ```cmd
    C:\> ssh -L localhost:15555:127.0.0.1:5555 mac-username@ip-address-of-the-mac
    ```

    `mac-username`를 `whoami`에 나열 된 Mac 사용자 이름으로 바꿉니다. `ip-address-of-the-mac`를 Mac의 IP 주소로 바꿉니다.

4. Windows에서 로컬 포트를 사용 하 여 에뮬레이터에 연결 합니다.

    ```cmd
    C:\> adb connect localhost:15555
    ```

    (참고: `adb`에 대 한 명령줄 액세스를 가져오는 한 가지 쉬운 방법은 [Visual Studio에서 **Android > Android Adb 명령 프롬프트 > android** ](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat)를 사용 하는 것입니다.)

약간의 주의 사항: 로컬 포트에 대해 포트 `5555`를 사용 하는 경우 에뮬레이터가 Windows에서 로컬로 실행 되 고 있다고 생각할 `adb`. 이로 인해 Visual Studio에서 문제가 발생 하지는 않지만 Mac용 Visual Studio 경우 시작 후 즉시 앱이 종료 됩니다.

### <a name="alternate-technique-using-adb--h-is-not-yet-supported"></a>`adb -H`를 사용 하는 대체 기술은 아직 지원 되지 않음

이론적으로는 `adb`의 기본 제공 기능을 사용 하 여 원격 컴퓨터에서 실행 되는 `adb` 서버에 연결 하는 것이 좋습니다 (예: [https://stackoverflow.com/a/18551325](https://stackoverflow.com/a/18551325)참조).
그러나 Xamarin Android IDE 확장은 현재 해당 옵션을 구성 하는 방법을 제공 하지 않습니다.

## <a name="contact-information"></a>联系信息

이 문서에서는 2016 년 3 월 현재 동작에 대해 설명 합니다. 이 문서에서 설명 하는 기술은 Xamarin 용 안정적인 테스트 제품군의 일부가 아니므로 나중에 중단 될 수 있습니다.

기술이 더 이상 작동 하지 않거나 문서에서 다른 오류가 발생 하는 경우 다음 포럼 스레드: [http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm](https://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm)에 대 한 토론에 자유롭게 추가 하세요.
감사합니다.
