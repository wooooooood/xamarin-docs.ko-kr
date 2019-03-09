---
title: Windows VM에서 Mac에서 실행 중인 Android 에뮬레이터에 연결할 수 있나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7B6752BB-8E4C-4690-B275-7E425A051F45
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/21/2018
ms.openlocfilehash: 35bfdb92ccfffe54f0ca10dc001d8919703a5bd8
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668155"
---
# <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>Windows VM에서 Mac에서 실행 중인 Android 에뮬레이터에 연결할 수 있나요?

Mac에서 Windows 가상 머신에서 실행 중인 Android 에뮬레이터에 연결 하려면 다음 단계를 사용 합니다.

1.  Mac에서 에뮬레이터를 시작 합니다.

2.  Kill을 `adb` mac 서버:

    ```bash
    adb kill-server
    ```

3.  에뮬레이터는 루프백 네트워크 인터페이스 2 개의 TCP 포트에서 수신 대기 하는 참고:

    ```bash
    lsof -iTCP -sTCP:LISTEN -P | grep 'emulator\|qemu'

    emulator6 94105 macuser   20u  IPv4 0xa8dacfb1d4a1b51f      0t0  TCP localhost:5555 (LISTEN)
    emulator6 94105 macuser   21u  IPv4 0xa8dacfb1d845a51f      0t0  TCP localhost:5554 (LISTEN)
    ```

    홀수 번호 포트에 연결할 때 사용할 것 `adb`입니다. 참고 항목 [ https://developer.android.com/tools/devices/emulator.html#emulatornetworking ](https://developer.android.com/tools/devices/emulator.html#emulatornetworking)합니다.

4.  _옵션 1_: 사용 하 여 [`nc`](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/nc.1.html)
    앞으로 인바운드 TCP 패킷 수신 외부적으로 5555 포트에서 (또는 원하는 다른 모든 포트) 홀수 포트 루프백 인터페이스에 (**127.0.0.1 5555** 이 예제의)를 전달 하도록 아웃 바운드 패킷을 다시 다른 방법으로:

    ```bash
    cd /tmp
    mkfifo backpipe
    nc -kl 5555 0<backpipe | nc 127.0.0.1 5555 > backpipe
    ```

    으로 `nc` 터미널 창에서 명령을 실행 상태로 유지, 예상 대로 패킷을 전달 됩니다. 취소 하려면 터미널 창에서 컨트롤 + C를 입력할 수 있습니다는 `nc` 명령이 완료 되 면 에뮬레이터를 사용 하 여 합니다.

    (옵션 1 옵션 2, 보다 일반적으로 쉽습니다 경우에 특히 **시스템 기본 설정 > 보안 및 개인 정보 > 방화벽** 전환 됩니다.) 

    _옵션 2_: 사용 하 여 [`pfctl`](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/pfctl.8.html)
    포트에서 TCP 패킷 리디렉션할 `5555` (또는 원하는 다른 모든 포트)에 [공유 네트워킹](http://kb.parallels.com/en/4948) 홀수 포트 루프백 인터페이스에 대 한 인터페이스 (`127.0.0.1:5555` 이 예제의):

    ```bash
    sed '/rdr-anchor/a rdr pass on vmnet8 inet proto tcp from any to any port 5555 -> 127.0.0.1 port 5555' /etc/pf.conf | sudo pfctl -ef -
    ```

    이 명령은 포트 전달을 사용 하 여 설정 된 `pf packet filter` 시스템 서비스입니다. 줄 바꿈 하는 것은 중요 합니다. 복사-붙여넣기 경우 그대로 유지 해야 합니다. 인터페이스 이름을 조정 해야 *vmnet8* Parallels를 사용 하는 경우. `vmnet8` 특수의 이름인 *NAT 장치* 에 대 한 합니다 *공유 네트워킹* VMWare Fusion의 모드입니다. Parallels의 적절 한 네트워크 인터페이스는 가능성이 [vnic0](http://download.parallels.com/doc/psbm/en/Parallels_Server_Bare_Metal_Users_Guide/29258.htm)합니다.

5.  Windows 컴퓨터에서 에뮬레이터에 연결 합니다.

    ```cmd
    C:\> adb connect ip-address-of-the-mac:5555
    ```

    "Ip 주소-의-the-mac" Mac의 IP 주소를 사용 하 여로 나열으로 예를 들어 대체 `ifconfig vmnet8 | grep 'inet '`합니다. 필요한 경우 대체 `5555` 4 단계에서 원하는 다른 포트를 사용 하 여\. (참고: 명령줄 액세스 하려면 단방향 `adb` 를 통해 [ **도구 > Android > Android Adb 명령 프롬프트** ](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat) Visual Studio에서.)

### <a name="alternate-technique-using-ssh"></a>기술을 사용 하 여 대체 `ssh`

설정한 경우 _원격 로그인_ mac에서 사용할 수 있습니다 `ssh` 포트 전달 에뮬레이터에 연결 합니다.

1.  Windows에서 SSH 클라이언트를 설치 합니다. 설치 하는 한 가지 방법은 [Git에 대 한 Windows](https://git-for-windows.github.io/)합니다. 합니다 `ssh` 명령에서 사용할 수 있습니다 합니다 **Git Bash** 명령 프롬프트입니다.

2.  에뮬레이터를 시작, 종료 하려면 위의 1-3 단계를 수행 합니다 `adb` Mac 서버의 에뮬레이터 포트를 식별 하 고 있습니다.

3.  실행 `ssh` Windows의 로컬 포트 간의 양방향 포트 전달을 설정 하는 Windows에서 (`localhost:15555` 이 예제의) 및 Mac의 루프백 인터페이스의 홀수 번호 에뮬레이터 포트 (`127.0.0.1:5555` 이 예제의):

    ```cmd 
    C:\> ssh -L localhost:15555:127.0.0.1:5555 mac-username@ip-address-of-the-mac
    ```

    바꿉니다 `mac-username` 나열 된 대로 Mac 사용자 이름을 사용 하 여 `whoami`입니다. 대체 `ip-address-of-the-mac` mac의 IP 주소를 사용 하 여

4.  Windows에서 로컬 포트를 사용 하 여 에뮬레이터에 연결 합니다.

    ```cmd
    C:\> adb connect localhost:15555
    ```

    (참고: 손쉬운 방법을 가져오기 명령줄을 통해 액세스할 `adb` 를 통해 [ **도구 > Android > Android Adb 명령 프롬프트** Visual Studio에서](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat).)

작은 주의: 포트를 사용 하는 경우 `5555` 로컬 포트에 대해 `adb` 에뮬레이터 Windows에서 로컬로 실행 되는 것으로 생각할 것입니다. Visual Studio에서 문제가 발생 하지 않는이 이지만 Mac 용 Visual Studio에서 앱 시작 후 즉시 종료 합니다.

### <a name="alternate-technique-using-adb--h-is-not-yet-supported"></a>또 다른 방법은 사용 하 여 `adb -H` 아직 지원 되지 않습니다

이론적으로 사용 하는 다른 방법은 것 `adb`의에 연결 하는 기본 제공 기능을 `adb` 원격 컴퓨터에서 실행 중인 서버 (예를 들어 참조 [ https://stackoverflow.com/a/18551325 ](https://stackoverflow.com/a/18551325)).
하지만 Xamarin.Android IDE 확장 옵션을 구성 하는 방법을 현재 제공 하지 않습니다.

## <a name="contact-information"></a>연락처 정보

이 문서는 2016 년 3 월을 기준으로 현재 동작을 설명합니다. 이 문서에 설명 된 기술은 앞으로 손상 될 수 있도록 Xamarin에 대 한 안정적인 테스트 제품군의 일부가 아닙니다.

다음 포럼 스레드에 대 한 논의에 추가할 방법을 더 이상 제대로 작동 하는지 확인할 수 없습니다 또는 문서의 다른 실수 하는 경우 자유롭게: [ http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm ](http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm)합니다.
감사합니다.

