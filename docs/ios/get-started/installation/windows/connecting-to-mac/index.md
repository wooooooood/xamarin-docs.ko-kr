---
title: Mac에 페어링
description: 이 가이드에서는 Mac에 페어링을 사용하여 Visual Studio 2017을 Mac 빌드 호스트에 연결하는 방법에 대해 설명합니다.
ms.prod: xamarin
ms.assetid: 39DD7B3F-3E69-4E2A-B743-4C26AF613025
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/16/2018
ms.openlocfilehash: adaa74e206b1e756398f1ef1a38f387082c1e8f5
ms.sourcegitcommit: dc6ccf87223942088ca926c0dadd5b5478c683cb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2018
---
# <a name="pair-to-mac"></a>Mac에 페어링

_이 가이드에서는 Mac에 페어링을 사용하여 Visual Studio 2017을 Mac 빌드 호스트에 연결하는 방법에 대해 설명합니다._

## <a name="overview"></a>개요

네이티브 iOS 응용 프로그램을 빌드하려면 Mac에서만 실행되는 Apple의 빌드 도구에 액세스해야 합니다. 이에 따라 Visual Studio 2017에서 네트워크에 액세스할 수 있는 Mac에 연결하여 Xamarin.iOS 응용 프로그램을 빌드해야 합니다.

Windows 기반 iOS 개발자가 생산적으로 작업할 수 있도록 Visual Studio 2017의 Mac에 페어링 기능은 Mac 빌드 호스트를 검색, 연결, 인증 및 기억합니다. 

Mac에 페어링을 사용하면 다음과 같은 개발 워크플로를 수행할 수 있습니다.

- 개발자는 Visual Studio 2017에서 Xamarin.iOS 코드를 작성할 수 있습니다.

- Visual Studio 2017은 Mac 빌드 호스트에 대한 네트워크 연결을 열고, 해당 컴퓨터의 빌드 도구를 사용하여 iOS 앱을 컴파일하고 서명합니다.

- Mac에서 별도의 응용 프로그램을 실행할 필요가 없습니다. Visual Studio 2017은 SSH를 통해 Mac 빌드를 안전하게 호출합니다.

- 변경이 발생하는 즉시 알림이 Visual Studio 2017로 보내집니다. 예를 들어 iOS 장치가 Mac에 연결되어 있거나 네트워크에서 사용할 수 있게 되면 iOS 도구 모음이 즉시 업데이트됩니다.

- Visual Studio 2017의 여러 인스턴스를 Mac에 동시에 연결할 수 있습니다.

- Windows 명령줄을 사용하여 iOS 응용 프로그램을 빌드할 수 있습니다.

> [!NOTE]
> 이 가이드의 지침을 수행하기 전에 먼저 다음 단계를 완료합니다. 
> 
> - Windows 컴퓨터에서 [Visual Studio 2017](~/cross-platform/get-started/installation/windows.md)을 설치합니다.
> - Mac에서 [Xcode](https://itunes.apple.com/us/app/xcode/id497799835?mt=12) 및 [Mac용 Visual Studio](https://docs.microsoft.com/visualstudio/mac/installation)를 설치합니다.
>
> Mac용 Visual Studio를 설치하지 않으려는 경우 Visual Studio 2017은 Xamarin.iOS 및 Mono를 사용하여 Mac 빌드 호스트를 자동으로 구성할 수 있습니다.
> 자세한 내용은 [자동 Mac 프로비전](#automatic-mac-provisioning)을 참조하세요.

## <a name="enable-remote-login-on-the-mac"></a>Mac에서 원격 로그인을 사용하도록 설정

Mac 빌드 호스트를 설정하려면 먼저 원격 로그인을 사용하도록 설정합니다.

1. Mac에서 [시스템 기본 설정]을 열고 **공유** 창으로 이동합니다.

2. **서비스** 목록에서 **원격 로그인**을 선택합니다.

    ![원격 로그인을 사용하도록 설정](images/sharing.png "원격 로그인을 사용하도록 설정")

    **모든 사용자**에 대한 액세스를 허용하도록 구성되어 있거나 Mac 사용자 이름 또는 그룹이 허용된 사용자 목록에 포함되어 있는지 확인합니다.

3. 메시지가 표시되면 macOS 방화벽을 구성합니다.

    들어오는 연결을 차단하도록 macOS 방화벽을 설정한 경우 `mono-sgen`에서 들어오는 연결을 받도록 허용해야 할 수도 있습니다. 이 경우 사용자에게 묻는 경고 메시지가 표시됩니다.

4. Windows 컴퓨터와 동일한 네트워크에 있으면 이제 Visual Studio 2017에서 Mac을 검색할 수 있습니다. Mac을 여전히 검색할 수 없는 경우 [수동으로 Mac 추가](#manually-add-a-mac)를 시도하거나 [문제 해결 가이드](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)를 살펴보세요.

## <a name="connect-to-the-mac-from-visual-studio-2017"></a>Visual Studio 2017에서 Mac에 연결

원격 로그인을 사용하도록 설정되었으므로 Visual Studio 2017을 Mac에 연결합니다.

1. Visual Studio 2017에서 기존 iOS 프로젝트를 열거나, **파일 > 새로 만들기 > 프로젝트**를 차례로 선택하고 iOS 프로젝트 템플릿을 선택하여 새 iOS 프로젝트를 만듭니다.

2. **Mac에 페어링** 대화 상자를 엽니다. 

    - iOS 도구 모음의 **Mac에 페어링** 단추를 사용합니다.

        ![Mac에 페어링 단추가 강조 표시된 iOS 도구 모음](images/ios-toolbar.png "Mac에 페어링 단추가 강조 표시된 iOS 도구 모음")

    - 또는 **도구 > iOS > Mac에 페어링**을 차례로 선택합니다.

    - **Mac에 페어링** 대화 상자에는 이전에 연결되어 있거나 현재 사용할 수 있는 Mac 빌드 호스트의 목록이 모두 표시됩니다.

        ![Mac에 페어링 대화 상자](images/pairtomac.png "Mac에 페어링 대화 상자")

3. 목록에서 Mac을 선택합니다. **연결**을 클릭합니다. 

4. 사용자 이름과 암호를 입력합니다.
    
    - 특정 Mac에 처음 연결하면 해당 컴퓨터에 대한 사용자 이름과 암호를 입력하라는 메시지가 표시됩니다.

        ![Mac에 대한 사용자 이름 및 암호 입력](images/auth.png "Mac에 대한 사용자 이름 및 암호 입력")

        > [!TIP]
        > 로그인하는 경우 전체 이름 대신 시스템 사용자 이름을 사용하세요.

    - Mac에 페어링은 이러한 자격 증명을 사용하여 Mac에 대한 새 SSH 연결을 만듭니다. 성공하면 Mac의 **authorized_keys** 파일에 키가 추가됩니다. 동일한 Mac에 대한 후속 연결은 자동으로 로그인됩니다.

5. Mac에 페어링은 Mac을 자동으로 구성합니다.

    [Visual Studio 2017 버전 15.6부터](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning) Visual Studio 2017은 필요에 따라 연결된 Mac 빌드 호스트에서 Mono 및 Xamarin.iOS를 설치하거나 업데이트합니다(Xcode는 여전히 수동으로 설치해야 함). 자세한 내용은 [자동 Mac 프로비전](#automatic-mac-provisioning)을 참조하세요.

6. 연결 상태 아이콘을 찾습니다.
    
    - Visual Studio 2017이 Mac에 연결되면, **Mac에 페어링** 대화 상자의 해당 Mac 항목에 현재 연결되어 있음을 나타내는 아이콘이 표시됩니다.

        ![연결된 Mac](images/connected.png "연결된 Mac")

      한 번에 하나의 Mac만 연결할 수 있습니다.

      > [!TIP]
      > **Mac에 페어링** 목록에서 Mac을 마우스 오른쪽 단추로 클릭하면 **연결...**, **이 Mac 지우기** 또는 **연결 끊기**를 허용하는 바로 가기 메뉴가 표시됩니다.
      >
      > ![Mac에 페어링 바로 가기 메뉴](images/contextmenu.png "Mac에 페어링 바로 가기 메뉴") 
      >
      > **이 Mac 지우기**를 선택하는 경우 선택한 Mac에 대한 자격 증명이 무시됩니다. 해당 Mac에 다시 연결하려면 사용자 이름과 암호를 다시 입력해야 합니다.

Mac 빌드 호스트에 성공적으로 연결되면 Visual Studio 2017에서 Xamarin.iOS 응용 프로그램을 빌드할 준비가 되었습니다. [Visual Studio용 Xamarin.iOS 소개](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md) 가이드를 살펴보세요.

Mac을 페어링할 수 없는 경우 [수동으로 Mac 추가](#manually-add-a-mac)를 시도하거나 [문제 해결 가이드](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)를 살펴보세요.

## <a name="manually-add-a-mac"></a>수동으로 Mac 추가

**Mac에 페어링** 대화 상자에 특정 Mac이 표시되지 않으면 수동으로 추가합니다.

1. Mac의 IP 주소를 찾습니다. 

    - Mac에서 **시스템 기본 설정 > 공유> 원격 로그인**을 차례로 엽니다.

        [![시스템 기본 설정 > 공유에 있는 Mac의 IP 주소](images/sharing-ipaddress.png "시스템 기본 설정 > 공유에 있는 Mac의 IP 주소")](images/sharing.png#lightbox)

    - 또는 명령줄을 사용합니다. 터미널에서 다음 명령을 실행합니다. 
   
        ```bash
        $ ipconfig getifaddr en0
        196.168.1.8
        ```

      네트워크 구성에 따라 `en0` 이외의 인터페이스 이름을 사용해야 할 수도 있습니다. 예를 들어 `en1`, `en2` 등이 있습니다.

2. Visual Studio 2017의 **Mac에 페어링** 대화 상자에서 **Mac 추가...** 를 선택합니다.

    [![Mac에 페어링 대화 상자의 Mac 추가 단추](images/addtomac.png "Mac에 페어링 대화 상자의 Mac 추가 단추")](images/addtomac-large.png#lightbox)

3. Mac의 IP 주소를 입력하고 **추가**를 클릭합니다.

    ![Mac의 IP 주소 입력](images/enteripaddress.png "Mac의 IP 주소 입력")

4. Mac에 대한 사용자 이름과 암호를 입력합니다.

    ![사용자 이름 및 암호 입력](images/auth.png "사용자 이름 및 암호 입력")

   > [!TIP]
   > 로그인하는 경우 전체 이름 대신 시스템 사용자 이름을 사용하세요.

5. **로그인**을 클릭하여 SSH를 통해 Visual Studio 2017을 Mac에 연결하고 알려진 컴퓨터 목록에 추가합니다.

## <a name="automatic-mac-provisioning"></a>자동 Mac 프로비전

[Visual Studio 2017 버전 15.6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning)부터 Mac에 페어링은 Xamarin.iOS 응용 프로그램을 빌드하는 데 필요한 소프트웨어인 Mono, Xamarin.iOS(Mac용 Visual Studio IDE가 아닌 소프트웨어 프레임워크) 및 다양한 Xcode 관련 도구(Xcode 자체가 아님)를 사용하여 Mac을 자동으로 프로비전합니다.

> [!IMPORTANT]
> - Mac에 페어링은 Xcode를 설치할 수 없으므로 Mac 빌드 호스트에 수동으로 설치해야 합니다. 이 호스트는 Xamarin.iOS 개발에 필요합니다.
> - 자동 Mac 프로비전을 사용하려면 Mac에서 원격 로그인을 사용하도록 설정하고 네트워크를 통해 Windows 컴퓨터에 액세스할 수 있어야 합니다. 자세한 내용은 [Mac에서 원격 로그인을 사용하도록 설정](#enable-remote-login-on-the-mac)을 참조하세요.

Visual Studio 2017이 [Mac에 연결](#connect-to-the-mac-from-visual-studio-2017)되면 Mac에 페어링에서 필요한 소프트웨어 설치/업데이트를 수행합니다.

### <a name="mono"></a>Mono

Mac에 페어링은 Mono가 설치되어 있는지 확인합니다. 설치되어 있지 않으면 Mac에 페어링에서 안정적인 최신 버전의 Mono를 다운로드하여 Mac에 설치합니다. 

진행 상황은 다음 스크린샷과 같이 다양한 프롬프트로 표시됩니다(확대/축소하려면 클릭).

||설치 확인|다운로드|설치
|---|---|---|---|
|Mono|[![모노 설치 누락](images/mono-missing.png "모노 설치 누락")](images/mono-missing-large.png#lightbox)|[![모노 다운로드](images/mono-downloading.png "모노 다운로드")](images/mono-downloading-large.png#lightbox)|[![모노 설치](images/mono-installing.png "모노 설치")](images/mono-installing-large.png#lightbox)|

### <a name="xamarinios"></a>Xamarin.iOS 

Mac에 페어링은 Windows 컴퓨터에 설치된 버전과 일치하도록 Mac에서 Xamarin.iOS를 업그레이드합니다.

> [!IMPORTANT]
> Mac에 페어링은 Mac의 Xamarin.iOS를 알파/베타 버전에서 안정적인 버전으로 다운그레이드하지 않습니다. Mac용 Visual Studio를 설치한 경우 [릴리스 채널](https://docs.microsoft.com/visualstudio/mac/update)을 다음과 같이 설정합니다.
> - Visual Studio 2017을 사용하는 경우 Mac용 Visual Studio에서 **안정적인** 업데이트 채널을 선택합니다.
> - Visual Studio 2017 Preview를 사용하는 경우 Mac용 Visual Studio에서 **알파** 업데이트 채널을 선택합니다.

진행 상황은 다음 스크린샷과 같이 다양한 프롬프트로 표시됩니다(확대/축소하려면 클릭).

||설치 확인|다운로드|설치
|---|---|---|---|
|Xamarin.iOS|[![Xamarin.iOS 설치 누락](images/xamios-missing.png "Xamarin.iOS 설치 누락")](images/xamios-missing-large.png#lightbox)|[![Xamarin.iOS 다운로드](images/xamios-downloading.png "Xamarin.iOS 다운로드")](images/xamios-downloading-large.png#lightbox)|[![Xamarin.iOS 설치](images/xamios-installing.png "Xamarin.iOS 설치")](images/xamios-installing-large.png#lightbox)|

### <a name="xcode-tools-and-license"></a>Xcode 도구 및 라이선스

Mac에 페어링은 Xcode가 설치되어 있고 해당 라이선스가 승인되었는지 여부도 확인합니다. Mac에 페어링은 Xcode를 설치하지 않지만 다음 스크린샷과 같이 라이선스 승인을 요구합니다(확대/축소하려면 클릭).

||설치 확인|라이선스 승인|
|---|---|---|
|Xcode|[![Xcode 설치 누락](images/xcode-missing.png "Xcode 설치 누락")](images/xcode-missing-large.png#lightbox)|[![Xcode 라이선스](images/xcode-license.png "Xcode 라이선스")](images/xcode-license-large.png#lightbox)|

또한 Mac에 페어링은 Xcode와 함께 배포되는 다양한 패키지를 설치하거나 업데이트합니다. 예:

- **MobileDeviceDevelopment.pkg**
- **XcodeExtensionSupport.pkg**
- **MobileDevice.pkg**
- **XcodeSystemResources.pkg**

이러한 패키지의 설치는 프롬프트 없이 빠르게 수행됩니다.

> [!NOTE]
> 이러한 도구는 macOS 10.9 현재로 [Xcode와 함께 설치](https://developer.apple.com/library/content/technotes/tn2339/_index.html)되는 Xcode 명령줄 도구와 다릅니다.

### <a name="troubleshooting-automatic-mac-provisioning"></a>자동 Mac 프로비전 문제 해결

자동 Mac 프로비전을 사용하는 데 문제가 발생하면 **%LOCALAPPDATA%\Xamarin\Logs\15.0**에 저장된 Visual Studio 2017 IDE 로그를 살펴보세요. 이러한 로그에는 효율적으로 오류를 진단하거나 지원을 받는 데 도움이 되는 오류 메시지가 포함될 수 있습니다.

## <a name="build-ios-apps-from-the-windows-command-line"></a>Windows 명령줄에서 iOS 응용 프로그램 빌드 

Mac에 페어링은 명령줄에서 Xamarin.iOS 응용 프로그램을 빌드하도록 지원합니다. 예:

```bash
C:\samples\App1>msbuild App1.sln /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhoneSimulator /p:ServerPassword=mypassword
```
위의 예제에서 `msbuild`에 전달된 매개 변수는 다음과 같습니다.

- `ServerAddress` – Mac 빌드 호스트의 IP 주소입니다.
- `ServerUser` – Mac 빌드 호스트에 로그인할 때 사용할 사용자 이름입니다.
  전체 이름이 아닌 시스템 사용자 이름을 사용합니다.
- `ServerPassword` – Mac 빌드 호스트에 로그인할 때 사용할 암호입니다.
 
> [!NOTE]
> Visual Studio 2017은 `msbuild`를 **C:\Program Files (x86)\Microsoft Visual Studio\2017\<Version>\MSBuild\15.0\Bin** 디렉터리에 저장합니다.

Mac에 페어링은 Visual Studio 2017 또는 명령줄에서 특정 Mac 빌드 호스트에 처음 로그인할 때 SSH 키를 설정합니다. 이러한 키를 사용하면 이후의 로그인에서 사용자 이름이나 암호가 필요하지 않습니다. 새로 만든 키는 **%LOCALAPPDATA%\Xamarin\MonoTouch**에 저장됩니다.

명령줄 빌드 호출에서 `ServerPassword` 매개 변수가 생략되면 Mac에 페어링은 저장된 SSH 키를 사용하여 Mac 빌드 호스트에 로그인하려고 합니다.

## <a name="summary"></a>요약

이 문서에서는 Visual Studio 2017 개발자가 Xamarin.iOS를 사용하여 네이티브 iOS 응용 프로그램을 빌드할 수 있도록 Mac에 페어링을 사용하여 Visual Studio 2017을 Mac 빌드 호스트에 연결하는 방법을 설명했습니다.

## <a name="next-steps"></a>다음 단계

- [연결 문제 해결](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [Xamarin Mac 빌드 에이전트 - Xamarin University 번개 강의](https://www.youtube.com/watch?v=MBAPBtxkjFQ)
- [Visual Studio용 Xamarin.iOS 소개](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)
