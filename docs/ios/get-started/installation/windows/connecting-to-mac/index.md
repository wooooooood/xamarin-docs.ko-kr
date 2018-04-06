---
title: Mac에 연결
description: Visual Studio용 Xamarin.iOS는 개발자가 Visual Studio IDE를 사용하여 Windows 컴퓨터에서 iOS 응용 프로그램을 만들고 빌드하고 디버그할 수 있게 해줍니다. 이 가이드에서는 Visual Studio용 Xamarin.iOS에서 제공하는 기능과 Mac 빌드 호스트에 연결하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 39DD7B3F-3E69-4E2A-B743-4C26AF613025
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 5a76c443521276a66e820fa0b1877ae2a4cce8f0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="connecting-to-the-mac"></a>Mac에 연결

_Visual Studio용 Xamarin.iOS는 개발자가 Visual Studio IDE를 사용하여 Windows 컴퓨터에서 iOS 응용 프로그램을 만들고 빌드하고 디버그할 수 있게 해줍니다. 이 가이드에서는 Visual Studio용 Xamarin.iOS에서 제공하는 기능과 Mac 빌드 호스트에 연결하는 방법을 설명합니다._

Visual Studio는 SSH를 통해 Mac에 연결하며, 이렇게 하면 다음과 같은 여러 가지 이점이 있습니다.

- Visual Studio는 빌드 에이전트를 직접 시작하고 제어할 수 있습니다. 수동으로 시작하고 중지해야 하는 사용자가 볼 수는 응용 프로그램은 더 이상 없습니다.

- Visual Studio의 새로운 연결 관리자는 Mac 빌드 호스트를 검색하고, 인증하고, 기억합니다.

- 모든 통신이 SSH를 통해 안전하게 터널링되므로 포트 22로 단일 포트만 연결하면 됩니다.

- 변경 사항이 발생하는 즉시 Visual Studio로 알림이 전송됩니다. 예를 들어 iOS 장치가 연결되면 도구 모음이 즉시 업데이트됩니다.

- Visual Studio의 여러 인스턴스를 동시에 연결할 수 있습니다.

- 연결은 개발에 방해가 되지 않습니다. 디버그하거나 iOS 디자이너를 사용할 때처럼 Mac이 필요한 작업을 수행할 때 연결을 요청할 뿐입니다.

Mac에 대한 연결은 브로커에 의해 제어되는 기능(예: iOS 디자이너 에이전트, 빌드 에이전트)의 여러 부분에 대한 여러 프로세스 구성됩니다. 이 브로커는 Visual Studio에 의해 제거 및 업데이트되며, 충돌이 발생할 독립 프로세스를 자동으로 다시 시작합니다.

아래 다이어그램은 Xamarin.iOS 개발 워크플로에 대한 간단한 개요를 보여줍니다.

[![iOS 개발 워크플로](images/xma2.png)](images/xma2.png#lightbox)

> [!IMPORTANT]
> Visual Studio는 프로젝트를 빌드하는 별도의 MSBuild 프로세스를 시작합니다. 이 프로세스에서는 Mac에 대한 새 연결을 설정합니다. 즉, Visual Studio가 빌드할 때 Windows에서 Mac으로 이어지는 두 개의 SSH 연결이 있습니다. [명령줄](#commandline)에서 빌드하면 MSBuild 프로세스가 하나만 생성됩니다. 다이어그램을 단순화하기 위해 모든 연결을 화살표로 표시했습니다.

## <a name="requirements"></a>요구 사항

Visual Studio용 Xamarin.iOS는 놀라운 결과를 가져다 줍니다. 개발자가 Visual Studio IDE를 사용하여 Windows 컴퓨터에서 iOS 응용 프로그램을 만들고 빌드하고 디버그할 수 있습니다. 단독으로는 불가능합니다. Apple의 컴파일러가 없으면 iOS 응용 프로그램을 만들 수 없으며, Apple의 인증서 및 코드 서명 도구 없이는 배포할 수 없습니다. 다시 말해서, Visual Studio용 Xamarin.iOS를 설치하려면 이러한 작업을 수행할 수 있도록 네트워크로 연결된 Mac OS X 컴퓨터(*호스트* 또는 *빌드 호스트*라고 함)에 연결해야 합니다. 구성이 완료되면 Xamarin 도구가 프로세스를 최대한 원활하게 만들어줍니다.

### <a name="system-requirements"></a>시스템 요구 사항

시스템 요구 사항은 [Windows에 Xamarin.iOS 설치](~/ios/get-started/installation/windows/index.md#system-requirements) 가이드에서 확인할 수 있습니다.


#### <a name="compatibility"></a>호환성

> [!IMPORTANT]
> Windows 컴퓨터는 Mac이 연결되는 것과 동일한 Xamarin.iOS 버전을 사용해야 합니다. 다음과 같은 방법으로 이를 확인할 수 있습니다.                                                    
>                                                                                                                 
> - **Visual Studio 2015 이하**: Mac용 Visual Studio와 동일한 [업데이트 채널](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/)에 있는지 확인합니다.
>                                                                                                                 
> - **Visual Studio 2017, 릴리스 버전**: Mac용 Visual Studio의 **안정적인** 채널에 있는지 확인합니다.
>                                                                                                                 
> - **Visual Studio 2017, 미리 보기 버전**: Mac용 Visual Studio **알파** 채널에 있는지 확인합니다. Visual Studio는 Xamarin.iOS SDK 및 Xcode가 존재하고 호환 가능한 버전이 있는지 여부를 검사하지 않습니다.
>   이 검사는 빌드 에이전트와 iOS 디자이너 에이전트에서 수행되며, 각각 빌드 오류와 디자이너 오류가 발생합니다.

### <a name="connecting-to-the-mac"></a>Mac에 연결

#### <a name="mac-setup"></a>Mac 설정

Mac 호스트를 설정하려면 Visual Studio용 Xamarin 확장과 Mac 호스트 간 통신을 설정해야 합니다. 이렇게 하려면 아래 단계에 따라 Mac에서 **원격 로그인**을 허용해야 합니다.

1. *스포트라이트*를 열고(**⌘-Space**) *원격 로그인*을 검색한 후 *공유* 결과를 선택합니다. 그러면 *공유* 패널에 *시스템 환경 설정*이 열립니다.

   [![원격 로그인에 대한 스포트라이트 검색](images/spotlight.png)](images/spotlight.png#lightbox)

2. 왼쪽에 있는 *서비스* 목록에서 *원격 로그인*을 선택하여 Visual Studio용 Xamarin이 Mac에 연결할 수 있도록 허용합니다.

   [![서비스 목록에서 원격 로그인 옵션 선택](images/sharing.png)](images/sharing.png#lightbox)

3. *모든 사용자*에 대한 액세스를 허용하도록 또는 Mac 사용자/그룹이 오른쪽에 있는 허용되는 사용자 목록에 포함되도록 *원격 로그인*을 설정합니다.

그 외에도, 기본적으로 서명된 응용 프로그램을 차단하도록 설정된 OS X 방화벽이 있는 경우 `mono-sgen`이 들어오는 연결을 받을 수 있도록 허용해야 합니다. 이 경우 들어오는 연결을 받을 수 있도록 허용하라는 경고 대화가 표시됩니다.

Mac에서 현재 세션이 열려 있는 경우 이제는 Visual Studio가 동일한 네트워크에 있다면 검색이 가능합니다.

Visual Studio가 Mac에서 에이전트를 시작하고 중지할 것이므로 사용자는 아무 것도 실행할 필요가 없습니다.

### <a name="windows-setup"></a>Windows 설치

Windows 컴퓨터에 Xamarin 도구를 [설치](~/ios/get-started/installation/windows/index.md)합니다.

### <a name="connecting-to-the-mac-build-host"></a>Mac 빌드 호스트에 연결

Mac 빌드 호스트에 연결하는 두 가지 방법이 있는데,

하나는 iOS 도구 모음에서 연결하는 방법입니다.

[![iOS 도구 모음](images/image1.png)](images/image1.png#lightbox)

또는 Visual Studio에서 **도구 > 옵션**으로 이동하여 **Xamarin > iOS 설정**을 선택한 후 **Xamarin Mac Agent 찾기** 단추를 클릭합니다.

[![Xamarin Mac Agent 찾기](images/image2.png)](images/image2.png#lightbox)

어떤 방법으로 탐색하든 아래 그림처럼 **Mac 에이전트** 대화 상자로 연결됩니다.

[![Mac 에이전트 대화 상자](images/image3.png)](images/image3.png#lightbox)

이전에 연결되어 알려진 컴퓨터로 저장된 컴퓨터 또는 *원격 로그인*에 사용할 수 있는 컴퓨터 전체 목록이 표시됩니다.

연결할 Mac을 두 번 클릭하여 연결합니다. Mac에 처음으로 연결하면 원격 연결을 허용하도록 Mac 사용자 자격 증명을 입력하라는 메시지가 표시됩니다.

[![Mac 사용자 자격 증명 입력](images/image4.png)](images/image4.png#lightbox)

에이전트는 자격 증명을 사용하여 Mac에 대한 새 SSH 연결을 만듭니다. 연결이 성공하면 SSH 키가 생성되고 해당 Mac의 `authorized_keys` 파일에 [등록](#commandline)됩니다. 그 다음 연결부터는 에이전트가 사용자 이름 및 키 파일을 사용하여 가장 최근에 연결한 알려진 빌드 호스트에 연결합니다.

> [!NOTE]
> 자격 증명을 입력할 때 _전체 이름_이 아닌 _사용자 이름_을 사용해야 합니다.  이것은 터미널에서 `whoami` 명령을 사용하여 확인할 수 있습니다.  예를 들어 아래 스크린샷에서 계정 이름은 **Amy Burns**가 아닌 **amyb**입니다.
>
> ![터미널 앱에서 사용자 이름 찾기](images/image5.png)

연결이 성공하면 아래 그림처럼 호스트 선택 대화 상자에 표시되고 그 옆에 **연결됨** 아이콘이 표시됩니다.

[![옆에 [연결됨] 아이콘이 표시된 호스트 선택 대화](images/image6.png)](images/image6.png#lightbox)

한 번에 하나의 Mac만 연결할 수 있습니다.

목록의 각 컴퓨터는 연결되어 있든 그렇지 않든 마우스 오른쪽 단추를 클릭하면 필요에 따라 **연결**, **연결 끊기** 또는 **이 Mac 지우기**를 수행할 수 있는 상황에 맞는 메뉴를 표시합니다.

[![연결, 연결 끊기 또는 이 Mac 지우기 메뉴](images/image7.png)](images/image7.png#lightbox)

**이 Mac 지우기**를 선택하면 연결 시 자격 증명을 다시 입력해야 합니다.

<a name="manual-add"/>

### <a name="manually-adding-a-mac"></a>수동으로 Mac 추가

상황에 따라 [호스트 선택] 대화 상자에 mDNS 이름이 표시되지 않는 경우 Mac을 수동으로 추가할 수 있습니다. 이렇게 하려면 아래 단계를 수행합니다.

1. Mac에서 **시스템 기본 설정 > 공유 > 원격 로그인**으로 이동하여 Mac의 IP 주소를 확인합니다.

   [![시스템 기본 설정의 Mac IP 주소](images/image8.png)](images/image8.png#lightbox)

   또는 명령줄을 선호하는 경우 터미널에 `ipconfig getifaddr en0`을 입력하여 IP 주소를 확인할 수 있습니다(연결 유형에 따라 변수는 `en1`, `en2` 등일 수 있음).

   [![터미널 앱의 IP 주소](images/image9.png)](images/image9.png#lightbox)

2. Visual Studio로 돌아가서 [호스트 선택] 대화 상자에서 **Mac 추가...**를 선택합니다.

   [![호스트 선택 대화 상자](images/image10.png)](images/image10.png#lightbox)

3. [Mac 추가] 대화 상자에 Mac IP 주소를 입력하고 **추가**를 클릭합니다.

   [![[Mac 추가] 대화 상자에 Mac IP 주소 입력](images/image11.png)](images/image11.png#lightbox)

4. 마지막으로, Mac 관리자 계정의 사용자 이름(전체 이름 아님) 및 해당 암호를 입력합니다.

   [![사용자 이름 및 암호 입력](images/image12.png)](images/image12.png#lightbox)

**로그인**을 클릭하면 Visual Studio가 SSH를 사용하여 Mac 컴퓨터에 로그인하고 이 Mac을 알려진 컴퓨터로 추가합니다.

<a name="commandline"/>

### <a name="command-line-support"></a>명령줄 지원

에이전트는 명령줄에서도 Xamarin.iOS 구성을 빌드할 수 있습니다.  이 방법을 사용하려면 MSBuild에 다음과 같은 필수 매개 변수를 전달해야 합니다.

- `ServerAddress` - Mac 서버의 IP 주소입니다.

- `ServerUser` - Mac 서버에 로그인하는 데 사용되는 사용자 이름(전체 이름 아님)입니다.

- `ServerPassword` – Mac 호스트에 로그인하는 데 사용되는 암호입니다(선택 사항).

`ServerPassword` 매개 변수는 필수가 아닙니다.

그 대신, Visual Studio 또는 명령줄을 사용하여 암호가 처음으로 전달되면 해당 Windows, Mac 및 사용자 구성에 대한 키 쌍이 생성되고 나중에 사용할 수 있도록 Windows 컴퓨터에 저장됩니다. 위치는 **%localappdata%\Xamarin\MonoTouch\id_rsa**입니다.  `ServerPassword` 매개 변수를 전달하지 않으면 `id_rsa` 키 파일이 인증에 사용됩니다.

아래는 **xamUser** 계정과 **mypassword** 암호를 사용하여 Mac 10.211.55.2에 연결하는 명령 예제입니다.

```bash
C:\samples\App1>msbuild App1.sln /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhoneSimulator /p:ServerPassword=mypassword
```

### <a name="summary"></a>요약

이 문서에서는 Visual Studio를 사용하여 Xamarin.iOS 앱을 빌드할 수 있도록 Visual Studio와 iOS 빌드 및 디자이너 도구 사이의 연결을 살펴보았습니다.

### <a name="related-links"></a>관련 링크

- [설치](~/ios/get-started/installation/windows/index.md)
- [연결 문제 해결](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [XMA를 사용하여 Mac을 Visual Studio 환경에 연결(비디오)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
