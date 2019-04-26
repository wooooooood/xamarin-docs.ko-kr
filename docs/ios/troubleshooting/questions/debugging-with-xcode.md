---
title: Xcode 사용 하 여 Xamarin.iOS 앱 디버깅
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5FDDEDB3-AEB9-4D9C-9F7B-FEFAA9AF0031
ms.technology: xamarin-ios
author: migueldeicaza
ms.author: miguel
ms.date: 02/22/2018
ms.openlocfilehash: e0127d4b24236d350e5fa967110316544c320d0f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61422475"
---
# <a name="debugging-xamarinios-apps-with-xcode"></a>Xcode 사용 하 여 Xamarin.iOS 앱 디버깅

Xcode를 사용 하 여 Xamarin.iOS 응용 프로그램의 일부 부분을 디버깅 하려는 시나리오가 있을 수 있습니다. .NET 코드를 디버깅할 수 없습니다, 하는 동안 네이티브 코드를 디버깅 하 고 Xcode에서 네이티브 시각화 도우미 중 일부를 사용 하는 일을 할 수 계속 합니다.

## <a name="walkthrough"></a>연습

Mac 용 Visual Studio에서 Xcode를 디버깅 하는 것에 대 한 기본 제공 지원이 있지만 이렇게 하려면 다음 단계를 사용할 수 있습니다.

1. Xamarin 앱에 있는 것과 동일한 번들 ID를 사용 하 여 Xcode iOS 앱을 만듭니다.
   
    - 열어 Xamarin.iOS 프로젝트의 번들 식별자를 찾을 수 있습니다 합니다 **Info.plist** 파일:

        ![Info.plist를 편집](debugging-with-xcode-images/vsmac-infoplist.png "Info.list 편집")

    - Xcode 프로젝트를 만들 때 또는 프로젝트의 대상을 선택 하 여 번들 식별자를 설정 합니다.

        ![Xcode에서 번들 식별자를 설정](debugging-with-xcode-images/xcode-bundle.png "Xcode에서 번들 식별자 설정")

2. 앱을 자동으로 시작 하는 대신 시작 될 때까지 기다리는 Xcode 프로젝트를 변경 합니다.

    - 열기는 **구성표 편집 패널** 선택 하 여 **제품 > 체계 > 구성표 편집** 또는 사용 하 여는 **cmd⌘ + <** 바로 가기 키입니다.

    - 선택 된 **실행** 구성표 및 하면 오른쪽 패널 표시 됩니다 **시작** 옵션입니다. 선택 **실행 파일이 시작 될 때까지 기다리는** 누릅니다 **닫기**합니다.

        ![실행 파일이 시작 될 때까지 기다리는](debugging-with-xcode-images/xcode-schemes.png "실행 파일이 시작 될 때까지 대기")

3. Xcode 프로젝트를 실행 합니다.

    이 장치에서 더미 Xcode 앱을 설치할 하지만 시작 하지 않습니다.

4. Xamarin 앱을 실행 합니다.

    Xcode는 시작할 때 Xamarin 앱에 연결 해야 합니다.

### <a name="caveats"></a>주의 사항

Xamarin.iOS 앱에 약간 변경 될 때마다 시작 해야 합니다. 그렇지 않으면 앱을 빌드할 필요가 없습니다 Mac 용 Visual Studio는 검색 *및* 가 이미 설치 하 고 Xcode 더미 응용 프로그램을 통해 설치 되지 않습니다.

## <a name="alternative---using-lldb"></a>대안-lldb를 사용 하 여

사용에 익숙한 경우 **lldb** 명령줄에서은 훨씬 간단한 솔루션입니다.

셸에서 다음 명령을 입력 합니다.

```bash
touch ~/.mtouch-launch-with-lldb
```

지침을 얻게 됩니다 합니다 **응용 프로그램 출력** 사용할 수 있는 창에서 수행할 작업, 하지만 기본적으로, 응용 프로그램을 실행할 때 **lldb** 응용 프로그램을 디버깅 하려면 명령줄에서.
