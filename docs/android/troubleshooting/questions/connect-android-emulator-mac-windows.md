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
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "76725080"
---
# <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>Windows VM에서 Mac에서 실행 중인 Android 에뮬레이터에 연결할 수 있나요?

Windows 가상 머신에서 Mac에서 실행되는 Android Emulator에 연결하려면 다음 단계를 따르세요.

1. Mac에서 에뮬레이터를 시작합니다.

2. Mac에서 `adb` 서버를 중지합니다.

    ```bash
    adb kill-server
    ```

3. 에뮬레이터는 루프백 네트워크 인터페이스의 TCP 포트 2개에서 수신 대기하고 있습니다.

    ```bash
    lsof -iTCP -sTCP:LISTEN -P | grep 'emulator\|qemu'

    emulator6 94105 macuser   20u  IPv4 0xa8dacfb1d4a1b51f      0t0  TCP localhost:5555 (LISTEN)
    emulator6 94105 macuser   21u  IPv4 0xa8dacfb1d845a51f      0t0  TCP localhost:5554 (LISTEN)
    ```

    홀수 번호가 매겨진 포트는 `adb`에 연결하는 데 사용됩니다. [https://developer.android.com/tools/devices/emulator.html#emulatornetworking](https://developer.android.com/tools/devices/emulator.html#emulatornetworking) 항목을 참조하세요.

4. _옵션 1_: `nc`를 사용하여 외부에서 포트 5555(또는 원하는 다른 포트)에 수신된 인바운드 TCP 패킷을 루프백 인터페이스의 홀수 번호 포트(이 예에서는**127.0.0.1 5555**)로 전달하고 아웃바운드 패킷을 다른 방식으로 다시 전달합니다.

    ```bash
    cd /tmp
    mkfifo backpipe
    nc -kl 5555 0<backpipe | nc 127.0.0.1 5555 > backpipe
    ```

    터미널 창에서 `nc` 명령이 실행되고 있으면 패킷이 올바르게 전달됩니다. 에뮬레이터 사용을 완료한 후에는 터미널 창에서 Control-C를 입력하여 `nc` 명령을 종료할 수 있습니다.

    일반적으로 옵션 1이 옵션 2보다 간단합니다(특히 **시스템 기본 설정 > 보안 & 개인 정보 보호 > 방화벽**이 켜져 있는 경우).

    _옵션 2_: `pfctl`을 사용하여 [공유 네트워킹](https://kb.parallels.com/en/4948) 인터페이스에서 포트 `5555`(또는 원하는 다른 포트)의 TCP 패킷을 루프백 인터페이스의 홀수 번호 포트(이 예에서는`127.0.0.1:5555`)로 리디렉션합니다.

    ```bash
    sed '/rdr-anchor/a rdr pass on vmnet8 inet proto tcp from any to any port 5555 -> 127.0.0.1 port 5555' /etc/pf.conf | sudo pfctl -ef -
    ```

    이 명령은 `pf packet filter` 시스템 서비스를 사용하여 포트 전달을 설정합니다. 줄 바꿈이 중요합니다. 복사하여 붙여넣을 때 그대로 유지해야 합니다. 또한 위도선을 사용하는 경우 *vmnet8*에서 인터페이스 이름을 조정해야 합니다. `vmnet8`은 VMWare Fusion에서 *공유 네트워킹* 모드의 특수 *NAT 디바이스* 이름입니다. 위도선에서 적절한 네트워크 인터페이스는 [vnic0](https://download.parallels.com/doc/psbm/en/Parallels_Server_Bare_Metal_Users_Guide/29258.htm)일 가능성이 높습니다.

5. Windows 머신에서 에뮬레이터에 연결합니다.

    ```cmd
    C:\> adb connect ip-address-of-the-mac:5555
    ```

    "ip-address-of-the-mac"을 Mac의 IP 주소로 바꿉니다(예: `ifconfig vmnet8 | grep 'inet '`으로 나열). 필요한 경우 4단계에서 원하는 다른 포트로 `5555`를 바꿉니다\. (참고: `adb`에 대한 명령줄 액세스를 확보하는 한 가지 방법은 Visual Studio에서 [**도구 > Android > Android Adb 명령 프롬프트**](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat)로 이동하는 것입니다.)

### <a name="alternate-technique-using-ssh"></a>`ssh`를 사용하는 대체 기법

Mac에서 _원격 로그인_을 사용하도록 설정한 경우 `ssh` 포트 전달을 사용하여 에뮬레이터에 연결할 수 있습니다.

1. Windows에 SSH 클라이언트를 설치합니다. 한 가지 옵션은 [Windows용 Git](https://git-for-windows.github.io/)를 설치하는 것입니다. 그러면 **Git Bash** 명령 프롬프트에서 `ssh` 명령을 사용할 수 있습니다.

2. 위의 1-3단계를 따라 에뮬레이터를 시작하고 Mac에서 `adb` 서버를 중지하고 에뮬레이터 포트를 확인합니다.

3. Windows에서 `ssh`를 실행하여 Windows의 로컬 포트(이 예의 경우 `localhost:15555`)와 Mac 루프백 인터페이스의 홀수 번호 에뮬레이터 포트(이 예의 경우 `127.0.0.1:5555`) 간에 양방향 포트 전달을 설정합니다.

    ```cmd
    C:\> ssh -L localhost:15555:127.0.0.1:5555 mac-username@ip-address-of-the-mac
    ```

    `mac-username`을 `whoami`로 나열된 Mac 사용자 이름으로 바꿉니다. `ip-address-of-the-mac`를 Mac의 IP 주소로 바꿉니다.

4. Windows에서 로컬 포트를 사용하여 에뮬레이터에 연결합니다.

    ```cmd
    C:\> adb connect localhost:15555
    ```

    (참고: `adb`에 대한 명령줄 액세스를 확보하는 한 가지 쉬운 방법은 Visual Studio에서 [**도구 > Android > Android Adb 명령 프롬프트**](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat)로 이동하는 것입니다.)

한 가지 주의할 점은 로컬 포트에 포트 `5555`를 사용하는 경우 `adb`에서 에뮬레이터가 Windows에서 로컬로 실행되고 있다고 인식한다는 것입니다. 이로 인해 Visual Studio에서 문제가 발생하지는 않지만 Mac용 Visual Studio의 경우 시작 직후 앱이 종료됩니다.

### <a name="alternate-technique-using-adb--h-is-not-yet-supported"></a>`adb -H`를 사용하는 대체 기법은 아직 지원되지 않음

이론적으로는 `adb`의 기본 제공 기능을 사용하여 원격 머신에서 실행되는 `adb` 서버에 연결할 수도 있습니다(예: [https://stackoverflow.com/a/18551325](https://stackoverflow.com/a/18551325)).
그러나 Xamarin.Android IDE 확장은 현재 해당 옵션을 구성하는 방법을 제공하지 않습니다.

## <a name="contact-information"></a>연락처 정보

이 문서에서는 2016년 3월 현재 동작에 대해 설명합니다. 이 문서에서 설명하는 기법은 안정적인 Xamarin용 테스트 도구 모음의 일부가 아니므로 나중에 중단될 수 있습니다.

기법이 더 이상 작동하지 않거나 문서에서 다른 실수를 발견한 경우에는 포럼 스레드의 토론([http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm](https://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm))에 자유롭게 추가하세요.
감사합니다.
