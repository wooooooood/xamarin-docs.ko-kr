---
title: Xamarin.iOS에서 SiriKit 구현
description: 이 문서에서는 Xamarin.iOS 앱에 SiriKit 지원을 구현 하는 데 필요한 단계를 설명 합니다. 인 텐트 확장 및 인 텐트 UI 확장에 설명 합니다.
ms.prod: xamarin
ms.assetid: 20FFB981-EB10-48BA-BF79-40F37F0291EB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/03/2018
ms.openlocfilehash: 2c3bddc89348b46c9bba277580071cb8ac3d6943
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61435173"
---
# <a name="implementing-sirikit-in-xamarinios"></a>Xamarin.iOS에서 SiriKit 구현

_이 문서에서는 Xamarin.iOS 앱에 SiriKit 지원을 구현 하는 데 필요한 단계를 다룹니다._

새 ios 10, SiriKit Xamarin.iOS 앱을 iOS 장치에서 Siri 및 맵 앱을 사용 하 여 사용자가 액세스할 수 있는 서비스를 제공할 수 있습니다. 이 문서에서는 필요한 인 텐트 확장, 인 텐트 UI 확장 및 어휘를 추가 하 여 Xamarin.iOS 앱에 SiriKit 지원을 구현 하는 데 필요한 단계를 다룹니다.

Siri의 개념을 사용 하 여 작동 **도메인**, 관련된 작업에 대 한 작업 그룹을 알고 있어야 합니다. Siri를 사용 하 여 앱에 있는 각 상호 작용 다음과 같이 해당 알려진된 서비스 도메인 중 하나에 속합니다 해야 합니다.:

- 오디오 또는 비디오 호출 합니다.
- 승객을 예약 합니다.
- 달리기를 관리 합니다.
- 메시징입니다.
- 사진을 검색 합니다.
- 지불을 주고받지 설정 합니다.

SiriKit 보냅니다 확장 사용자 요청 하면 Siri의 관련 앱 확장의 서비스 중 하나에 **의도** 지 원하는 모든 데이터와 함께 사용자의 요청을 설명 하는 개체입니다. 앱 확장 한 다음 적절 한 생성 **응답** 개체를 지정 **의도**, 확장 요청을 처리할 수 있는 방법을 자세히 설명 합니다.

이 가이드는 기존 앱에 SiriKit 지원을 포함 하는 빠른 예제를 제공 합니다. 이 예제의 경우 가짜 MonkeyChat 앱 사용:

[![](implementing-sirikit-images/monkeychat01.png "MonkeyChat 아이콘")](implementing-sirikit-images/monkeychat01.png#lightbox)

MonkeyChat 사용자 친구의 연락 자체 책을 유지 하 고 각 화면 이름 (예: Bobo 예제)에 연결 된 화면 이름으로 각 friend 텍스트 채팅 보낼 수 있습니다.

## <a name="extending-the-app-with-sirikit"></a>SiriKit로 앱 확장

에 표시 된 대로 합니다 [SiriKit 개념 이해](~/ios/platform/sirikit/understanding-sirikit.md) 가이드 SiriKit 사용 하 여 앱을 확장 하는 관련 된 세 가지 주요 구성 요소는:

[![](implementing-sirikit-images/elements01.png "SiriKit 다이어그램을 사용 하 여 앱 확장")](implementing-sirikit-images/elements01.png#lightbox)

여기에는 다음이 포함됩니다.

1. **인 텐트 확장** -사용자가 응답을 확인, 요청을 처리할 수 하 고 실제로 사용자의 요청을 수행 하 고 작업을 수행 하는 앱을 확인 합니다.
2. **인 텐트 UI 확장** - *선택적*, Siri 환경에서 응답을 사용자 지정 UI를 제공 하 고 UI 앱을 가져올 수 및 사용자의 환경을 보강 하는 Siri에 브랜딩 합니다.
3. **앱** -Siri 작업을 지원 하기 위해 사용자 특정 어휘를 사용 하 여 앱을 제공 합니다. 

모든 앱에 포함 하는 단계 및 이러한 요소는 아래 섹션에서 자세히 설명 합니다.


## <a name="preparing-the-app"></a>앱 준비

SiriKit 확장은 기반으로, 단, 앱 확장을 추가 하기 전에 몇 가지 사항을 SiriKit 도입에 도움이 되도록 작업을 수행 하는 개발자.

### <a name="moving-common-shared-code"></a>일반적인 공유 코드를 이동합니다. 

먼저 개발자 간에 이동할 수는 공유 하는 일반적인 코드의 일부 앱 및 확장 중 하나로 _공유 프로젝트_하십시오 _이식 가능한 클래스 라이브러리 (Pcl)_ 또는 _네이티브 라이브러리_합니다.

모든 앱에서 수행 하는 작업을 수행할 수 있으려면 확장 해야 합니다. 샘플 MonkeyChat 앱에서 새 연락처를 추가할 연락처를 찾는 등의 측면에서 메시지를 보내고 메시지 기록을 검색 합니다.

공유 프로젝트, PCL 또는 네이티브 라이브러리에이 공통 코드를 이동 하 여 쉽게 한곳에서 해당 코드를 유지 관리 하 고 확장 하 고 부모 앱 제공는 균일 한 환경 및 기능 사용자에 대 한 확인 합니다.

MonkeyChat 예제 앱의 경우 데이터 모델 및 네트워크 및 데이터베이스 액세스와 같은 처리 코드를 네이티브 라이브러리 이동 됩니다.

다음을 수행합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac 용 Visual Studio를 시작 하 고 MonkeyChat 앱을 엽니다.
2. 솔루션 이름을 마우스 오른쪽 단추로 클릭 합니다 **Solution Pad** 선택한 **추가** > **새 프로젝트...** : 

    [![](implementing-sirikit-images/prep01.png "새 프로젝트 추가")](implementing-sirikit-images/prep01.png#lightbox)
3. 선택 **iOS** > **라이브러리** > **클래스 라이브러리** 을 클릭 합니다 **다음** 단추: 

    [![](implementing-sirikit-images/prep02.png "클래스 라이브러리를 선택 합니다.")](implementing-sirikit-images/prep02.png#lightbox)
4. 입력 `MonkeyChatCommon` 에 대 한 합니다 **이름** 을 클릭 합니다 **만들기** 단추: 

    [![](implementing-sirikit-images/prep03.png "MonkeyChatCommon 이름 입력")](implementing-sirikit-images/prep03.png#lightbox)
5. 마우스 오른쪽 단추로 클릭 합니다 **참조** 에서 기본 앱의 폴더를 **솔루션 탐색기** 선택한 **참조 편집...** . 확인 합니다 **MonkeyChatCommon** 프로젝트을 클릭 합니다 **확인** 단추: 

    [![](implementing-sirikit-images/prep05.png "MonkeyChatCommon 프로젝트 체크 인")](implementing-sirikit-images/prep05.png#lightbox)
6. 에 **솔루션 탐색기**, 기본 앱에서 네이티브 라이브러리 공용 공유 코드를 끕니다.
7. 끌어 MonkeyChat의 경우는 **DataModels** 하 고 **프로세서** 네이티브 라이브러리에 기본 앱에서 폴더: 

    [![](implementing-sirikit-images/prep06.png "솔루션 탐색기에서 DataModels 및 프로세서 폴더")](implementing-sirikit-images/prep06.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio를 시작 하 고 MonkeyChat 앱을 엽니다.
2. 솔루션 이름을 마우스 오른쪽 단추로 클릭 합니다 **솔루션 탐색기** 선택한 **추가** > **새 프로젝트...** .
3. 선택 **시각적 C#**   >  **공유 프로젝트** 을 클릭 합니다 **다음** 단추: 

    [![](implementing-sirikit-images/prep02.w157-sml.png "클래스 라이브러리를 선택 합니다.")](implementing-sirikit-images/prep02.w157.png#lightbox)
4. 입력 `MonkeyChatCommon` 에 대 한는 **이름** 을 클릭 합니다 **만들기** 단추입니다.
5. 마우스 오른쪽 단추로 클릭 합니다 **참조** 에서 기본 앱의 폴더를 **솔루션 탐색기** 선택한 **참조 편집...** . 확인 합니다 **MonkeyChatCommon** 프로젝트을 클릭 합니다 **확인** 단추: 

    [![](implementing-sirikit-images/prep05w.png "MonkeyChatCommon 프로젝트 체크 인")](implementing-sirikit-images/prep05w.png#lightbox)
6. 에 **솔루션 탐색기**, 공유 프로젝트에 기본 앱에서 일반적인 공유 코드를 끕니다.
7. 끌어 MonkeyChat, 경우는 **DataModels** 하 고 **프로세서** 네이티브 라이브러리에 기본 앱에서 폴더.

-----

네이티브 라이브러리에 이동 된 파일을 편집 및 라이브러리의 일치 하도록 네임 스페이스를 변경 합니다. 예를 들어 변경 `MonkeyChat` 에 `MonkeyChatCommon`:

```csharp
using System;
namespace MonkeyChatCommon
{
    /// <summary>
    /// A message sent from one user to another within a conversation.
    /// </summary>
    public class MonkeyMessage
    {
        public MonkeyMessage ()
        {
        }
        ...
    }
}
```

다음으로, 기본 앱으로 다시 이동 하 고 추가 `using` 문은 네이티브 라이브러리의 네임 스페이스에 대 한 어디서 나 앱을 사용 하 여 이동 된 클래스 중 하나:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;
using MonkeyChatCommon;

namespace MonkeyChat
{
    public partial class MasterViewController : UITableViewController
    {
        public DetailViewController DetailViewController { get; set; }

        DataSource dataSource;
        ...
    }
}
```

### <a name="architecting-the-app-for-extensions"></a>앱 확장에 대 한 설계

일반적으로 여러 의도에 앱을 등록 하 고 개발자는 적절 한 개수의 의도 확장에 대 한 앱의 아키텍처를 확인 해야 합니다.

앱에서 둘 이상의 의도 요구 하는 위치는 상황에서 개발자가 각 의도 대 한 별도 의도 확장이 만들거나 의도 확장 하나에 배치 하는 의도 처리의 모든 있습니다.

각 의도 대 한 별도 의도 확장을 만들려면를 선택 하는 경우 개발자 많은 양의 각 확장에 대 한 상용구 코드를 복제를 업데이트 하 고 많은 양의 프로세서 및 메모리 오버 헤드를 만듭니다 수 없습니다.

두 옵션 간에 선택 하려면 모든 의도 자연스럽 게 속할 함께 참조 하십시오. 예를 들어, 오디오 및 비디오 호출을 수행 하는 앱 유사한 작업을 처리 하는 대로 단일 의도 확장에서 모두 이러한 의도 포함 하려는 및 대부분의 코드 재사용을 제공할 수 있습니다.

모든 의도 하거나 의도 기존 그룹에 맞지 않는 그룹에 대 한 새 의도 확장을 포함 하는 앱의 솔루션에서 만듭니다.


### <a name="setting-the-required-entitlements"></a>필요한 자격 설정

SiriKit 통합을 포함 하는 Xamarin.iOS 앱을 올바른 자격을 설정 해야 합니다. 개발자는 이러한 필수 자격을 올바르게 설정 하지 않습니다, 경우 있습니다 됩니다을 설치 하거나 iOS 10 이후 요구 사항 이기도 실제 iOS 10 이상 하드웨어에서 앱을 테스트할 수 시뮬레이터 SiriKit을 지원 하지 않습니다.

다음을 수행합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 두 번 클릭 합니다 `Entitlements.plist` 파일을 **솔루션 탐색기** 을 편집용으로 엽니다.
2. 으로 전환 합니다 **원본** 탭 합니다.
3. 추가 합니다 `com.apple.developer.siri` **속성**설정 합니다 **형식** 에 `Boolean` 및 **값** 를 `Yes`: 

    [![](implementing-sirikit-images/setup01.png "Com.apple.developer.siri 속성 추가")](implementing-sirikit-images/setup01.png#lightbox)
4. 파일의 변경 내용을 저장합니다.
5. 두 번 클릭 합니다 **프로젝트 파일** 에 **솔루션 탐색기** 을 편집용으로 엽니다.
6. 선택 **iOS 번들 서명** 있는지 확인 합니다 `Entitlements.plist` 파일을 선택 합니다 **사용자 지정 자격** 필드: 

    [![](implementing-sirikit-images/setup02.png "사용자 지정 자격 필드에 Entitlements.plist 파일을 선택 합니다.")](implementing-sirikit-images/setup02.png#lightbox)
7. **확인** 단추를 클릭하여 변경 내용을 저장합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 두 번 클릭 합니다 `Entitlements.plist` 파일을 **솔루션 탐색기** 을 편집용으로 엽니다.
3. 추가 합니다 `com.apple.developer.siri` **속성**설정 합니다 **형식** 에 `Boolean` 및 **값** 를 `Yes`: 

    [![](implementing-sirikit-images/setup01w.png "Com.apple.developer.siri 속성 추가")](implementing-sirikit-images/setup01w.png#lightbox)
4. 파일의 변경 내용을 저장합니다.
5. 두 번 클릭 합니다 **프로젝트 파일** 에 **솔루션 탐색기** 을 편집용으로 엽니다.
6. 선택 **iOS 번들 서명** 있는지 확인 합니다 `Entitlements.plist` 에서 파일을 선택 합니다 **사용자 지정 자격** 필드입니다.

-----

완료 되 면 앱의 `Entitlements.plist` 파일은 다음과 같습니다 (의 편집기에서 열린 외부):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.siri</key>
    <true/>
</dict>
</plist>
```

### <a name="correctly-provisioning-the-app"></a>앱을 올바르게 프로 비전

Apple에 SiriKit 프레임 워크, SiriKit 구현 하는 모든 Xamarin.iOS 앱 주위에 엄격한 보안으로 인해 _해야_ 올바른 앱 ID 및 권한 부여 (위의 섹션 참조) 및 적절 한을 사용 하 여 서명 해야 합니다 프로 비전 프로필입니다.

Mac에서 다음을 수행 합니다.

1. 웹 브라우저에서로 이동 [ https://developer.apple.com ](https://developer.apple.com) 및 계정에 로그인 합니다.
2. 클릭할 **인증서**를 **식별자** 하 고 **프로필**합니다.
3. 선택 **프로 비전 프로필** 선택한 **앱 Id**를 클릭 합니다 **+** 단추입니다.
4. 입력 한 **이름을** 새 프로필에 대 한 합니다.
5. 입력 한 **번들 ID** Apple 다음의 권장 명명 합니다.
6. 아래로 스크롤하여를 **App Services** 섹션에서 **SiriKit** 을 클릭 합니다 **계속** 단추: 

    [![](implementing-sirikit-images/setup03.png "SiriKit 선택")](implementing-sirikit-images/setup03.png#lightbox)
7. 다음 설정을 모두 확인 **제출** 앱 id입니다.
8. 선택 **프로 비전 프로필** > **개발**를 클릭 합니다 **+** 단추를 선택 합니다 **Apple ID**, 누른 **계속**합니다.
9. 선택을 클릭 **모든**, 클릭 **계속**합니다.
10. 클릭 **모두 선택** 클릭 한 다음 다시 **계속**합니다.
11. 입력 한 **프로필 이름** Apple를 사용 하 여의 명명 제안, 클릭 **계속**합니다.
12. Xcode를 시작합니다.
13. Xcode 메뉴에서 선택 **기본 설정...**
14. 선택 **계정을**를 클릭 합니다 **세부 정보 보기...** 단추: 

    [![](implementing-sirikit-images/setup04.png "계정 선택")](implementing-sirikit-images/setup04.png#lightbox)
15. 클릭 합니다 **모든 프로필 다운로드** 왼쪽 아래 모서리에 단추: 

    [![](implementing-sirikit-images/setup05.png "모든 프로필 다운로드")](implementing-sirikit-images/setup05.png#lightbox)
16. 있는지 확인 합니다 **프로 비전 프로필** 만든 이상이 설치 된 Xcode 합니다.
17. Mac 용 Visual Studio에서 SiriKit 지원을 추가 하려면 프로젝트를 엽니다
18. 두 번 클릭 합니다 `Info.plist` 파일을 **솔루션 탐색기**합니다.
18. 있는지 확인 합니다 **번들 식별자** 위의 Apple Developer 포털에서 만든 값과 일치 합니다. 

    [![](implementing-sirikit-images/setup06.png "번들 식별자")](implementing-sirikit-images/setup06.png#lightbox)
18. 에 **솔루션 탐색기**를 선택 합니다 **프로젝트**합니다.
19. 프로젝트를 마우스 오른쪽 단추로 누르고 **옵션**합니다.
21. 선택 **iOS 번들 서명**를 선택 합니다 **서명 Id** 하 고 **프로 비전 프로필** 위에서 만든: 

    [![](implementing-sirikit-images/setup07.png "서명 Id 및 프로 비전 프로필 선택")](implementing-sirikit-images/setup07.png#lightbox)
22. **확인** 단추를 클릭하여 변경 내용을 저장합니다.

> [!IMPORTANT]
> 테스트 SiriKit 에서만 실제 iOS 10 하드웨어 장치 및 iOS 10에 없는 시뮬레이터입니다. SiriKit을 설치 하는 데 문제가 실제 하드웨어에서 Xamarin.iOS 앱을 사용 하는 경우는 필수 자격, 앱 ID, 식별자 서명 및 프로 비전 프로필 구성 되어 있는지 확인 올바르게 Apple 개발자 포털 및 Visual Studio에서 mac 용

### <a name="requesting-siri-authorization"></a>Siri 권한 부여를 요청합니다.

모든 사용자 특정 어휘를 추가 하는 앱 또는 인 텐트 확장 Siri 연결하기 전에 Siri를 액세스 하는 사용자 로부터 권한 부여를 요청 해야 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

앱의 편집 `Info.plist` 파일을 전환할를 **소스** 보기 및 추가 `NSSiriUsageDescription` Siri 및 앱은 사용 하는 방법을 설명 하는 문자열 값을 사용 하 여 키 데이터 형식에 전송 됩니다. 예를 들어 MonkeyChat 앱 "MonkeyChat 연락처 전송할 Siri" 이라는 있을 수도 있습니다.

[![](implementing-sirikit-images/request01.png "Info.plist 편집기에서 NSSiriUsageDescription")](implementing-sirikit-images/request01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

앱의 편집 `Info.plist` 파일을 추가 합니다 `NSSiriUsageDescription` Siri 및 앱은 사용 하는 방법을 설명 하는 문자열 값을 사용 하 여 키 데이터 형식에 전송 됩니다. 예를 들어 MonkeyChat 앱 "MonkeyChat 연락처 전송할 Siri" 이라는 있을 수도 있습니다.

[![](implementing-sirikit-images/request01w.png "Info.plist 편집기에서 NSSiriUsageDescription")](implementing-sirikit-images/request01w.png#lightbox)

-----

호출 된 `RequestSiriAuthorization` 메서드는 `INPreferences` 앱을 처음 시작할 때 클래스. 편집 합니다 `AppDelegate.cs` 고 클래스는 `FinishedLaunching` 다음과 같은 메서드 확인:


```csharp
using Intents;
...

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{

    // Request access to Siri
    INPreferences.RequestSiriAuthorization ((INSiriAuthorizationStatus status) => {
        // Respond to returned status
        switch (status) {
        case INSiriAuthorizationStatus.Authorized:
            break;
        case INSiriAuthorizationStatus.Denied:
            break;
        case INSiriAuthorizationStatus.NotDetermined:
            break;
        case INSiriAuthorizationStatus.Restricted:
            break;
        }
    });


    return true;
}
```

이 메서드는 처음으로 경고 Siri에 액세스 하도록 앱을 허용 하 라는 메시지가 표시 됩니다. 개발자를 추가 하는 메시지는 `NSSiriUsageDescription` 위에이 경고에 표시 되도록 합니다. 처음에 사용자 액세스를 거부 하는 경우 사용할 수 있습니다 합니다 **설정을** 앱을 앱에 대 한 액세스 권한을 부여 합니다.

언제 든 지 앱 Siri를 호출 하 여 액세스 하는 앱의 기능을 확인할 수는 `SiriAuthorizationStatus` 메서드는 `INPreferences` 클래스입니다.

### <a name="localization-and-siri"></a>지역화 및 Siri

IOS 장치에서 사용자가 다른 Siri를 시스템 기본 언어를 선택할 수 있습니다. 지역화 된 데이터를 사용할 때 앱을 사용 해야 합니다 `SiriLanguageCode` 메서드는 `INPreferences` Siri에서 언어 코드를 가져오는 클래스입니다. 예를 들어:

```csharp
var language = INPreferences.SiriLanguageCode();

// Take action based on language
if (language == "en-US") {
    // Do something...
}
```

### <a name="adding-user-specific-vocabulary"></a>사용자 특정 어휘를 추가합니다.

사용자 특정 어휘는 앱의 개별 사용자에 게 고유한는 단어 또는 구를 제공 하려고 합니다. 이러한 기본 앱 (앱 확장명 제외)에서 런타임에으로 제공 정렬된 된 목록의 시작 부분에 가장 중요 한 용어를 사용 하 여 사용자에 대 한 가장 중요 한 사용 우선 순위 정렬에 조건 집합입니다.

사용자 특정 어휘는 다음 범주 중 하나에 속해야 합니다.

- 연락처 이름 (연락처 프레임 워크에 의해 관리 됨)입니다.
- 사진 태그입니다.
- 사진 앨범 이름입니다.
- 운동 이름입니다.

사용자 지정 어휘도 등록 하는 용어를 선택할 때만 사용 약관을 선택 하는 앱에 익숙하지 않는 사람이 오해 될 수 있습니다. 되지 등록 일반적인 용어 "내 운동" 또는 "내 앨범" 등입니다. 예를 들어 MonkeyChat 앱 사용자의 주소록에 각 연락처와 연결 된 애칭 등록 됩니다.

앱을 호출 하 여 사용자 특정 어휘를 제공 합니다 `SetVocabularyStrings` 메서드의 `INVocabulary` 클래스 및 전달를 `NSOrderedSet` 기본 앱에서 컬렉션. 앱 항상 호출 해야는 `RemoveAllVocabularyStrings` 메서드 첫 번째 새로 추가 하기 전에 모든 기존 조건을 제거 합니다. 예를 들어:

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Foundation;
using Intents;

namespace MonkeyChatCommon
{
    public class MonkeyAddressBook : NSObject
    {
        #region Computed Properties
        public List<MonkeyContact> Contacts { get; set; } = new List<MonkeyContact> ();
        #endregion

        #region Constructors
        public MonkeyAddressBook ()
        {
        }
        #endregion

        #region Public Methods
        public NSOrderedSet<NSString> ContactNicknames ()
        {
            var nicknames = new NSMutableOrderedSet<NSString> ();

            // Sort contacts by the last time used
            var query = Contacts.OrderBy (contact => contact.LastCalledOn);

            // Assemble ordered list of nicknames by most used to least
            foreach (MonkeyContact contact in query) {
                nicknames.Add (new NSString (contact.ScreenName));
            }

            // Return names
            return new NSOrderedSet<NSString> (nicknames.AsSet ());
        }

        // This method MUST only be called on a background thread!
        public void UpdateUserSpecificVocabulary ()
        {
            // Clear any existing vocabulary
            INVocabulary.SharedVocabulary.RemoveAllVocabularyStrings ();

            // Register new vocabulary
            INVocabulary.SharedVocabulary.SetVocabularyStrings (ContactNicknames (), INVocabularyStringType.ContactName);
        }
        #endregion
    }
}
```

이 코드를 사용 하 여 해당 수 있습니다. 다음과 같이 호출할 수 있습니다:

```csharp
using System;
using System.Threading;
using UIKit;
using MonkeyChatCommon;
using Intents;

namespace MonkeyChat
{
    public partial class ViewController : UIViewController
    {
        #region AppDelegate Access
        public AppDelegate ThisApp {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do we have access to Siri?
            if (INPreferences.SiriAuthorizationStatus == INSiriAuthorizationStatus.Authorized) {
                // Yes, update Siri's vocabulary
                new Thread (() => {
                    Thread.CurrentThread.IsBackground = true;
                    ThisApp.AddressBook.UpdateUserSpecificVocabulary ();
                }).Start ();
            }
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion
    }
}
```

> [!IMPORTANT]
> Siri 힌트로 사용자 지정 어휘를 취급 하 고 최대한 용어의 만큼 통합 됩니다. 그러나 사용자 지정 어휘를 등록 해야 하므로 제한에 대 한 공간 _만_ 혼동 따라서 총 등록 된 용어를 최소로 유지 될 수 있는 용어입니다.

자세한 내용은 참조 하십시오 우리의 [사용자 특정 어휘 참조](~/ios/platform/sirikit/understanding-sirikit.md) 과 Apple [사용자 지정 어휘 참조 지정](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1)합니다.

### <a name="adding-app-specific-vocabulary"></a>앱 관련 어휘를 추가합니다.

앱 특정 어휘 모든 차량 형식 또는 운동 이름과 같은 앱의 사용자에 특정 단어 및 구를 알 수를 정의 합니다. 에 정의 된 응용 프로그램의 일부 이기 때문에 `AppIntentVocabulary.plist` 주 앱 번들의 일부로 파일입니다. 또한 이러한 단어와 구 지역화 해야 합니다.

앱 특정 용어는 다음 범주 중 하나에 속해야 합니다.

- 옵션을 재정의 합니다.
- 운동 이름입니다.

앱 특정 어휘 파일에는 두 개의 루트 수준 키 포함 됩니다.

- `ParameterVocabularies` **필요한** -앱의 사용자 지정 약관에 적용 되는 의도 매개 변수를 정의 합니다.
- `IntentPhrases` **선택적** -에서 정의 된 사용자 지정 용어를 사용 하 여 예제 구문이 포함 되어는 `ParameterVocabularies`합니다.

각 항목에는 `ParameterVocabularies` ID 문자열, 단어 및 용어에 적용 되는 의도 지정 해야 합니다. 또한 단일 용어 여러 의도 적용할 수 있습니다.

전체 목록을 허용 되는 값 및 필요한 파일 구조에 대 한 Apple의를 참조 하세요 [앱 어휘 파일 형식 참조](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1)합니다.

추가할를 `AppIntentVocabulary.plist` 앱 프로젝트에 파일에서 다음을 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 프로젝트 이름을 마우스 오른쪽 단추로 클릭 합니다 **솔루션 탐색기** 선택한 **추가** > **새 파일...**   >  **iOS**:

    [![](implementing-sirikit-images/plist01.png "속성 목록을 추가 합니다.")](implementing-sirikit-images/plist01.png#lightbox)
2. 두 번 클릭 합니다 `AppIntentVocabulary.plist` 파일을 **솔루션 탐색기** 을 편집용으로 엽니다.
3. 클릭 합니다 **+** 키를 추가 하려면 설정 합니다 **이름** 를 `ParameterVocabularies` 및 **형식** 를 `Array`:

    [![](implementing-sirikit-images/plist02.png "배열 형식과 ParameterVocabularies 이름을 설정합니다")](implementing-sirikit-images/plist02.png#lightbox)
4. 확장 `ParameterVocabularies` 클릭 합니다 **+** 단추를 설정 합니다 **형식** 에 `Dictionary`:

    [![](implementing-sirikit-images/plist03.png "사전 형식 설정")](implementing-sirikit-images/plist03.png#lightbox)
5. 클릭 합니다 **+** 새 키를 추가 하려면 설정 합니다 **이름** 에 `ParameterNames` 및 **형식** 에 `Array`:

    [![](implementing-sirikit-images/plist04.png "배열 형식과 ParameterNames 이름을 설정합니다")](implementing-sirikit-images/plist04.png#lightbox)
6. 클릭 합니다 **+** 사용 하 여 새 키를 추가 하는 **형식** 의 `String` 값을 사용할 수 있는 매개 변수 이름 중 하나입니다. 예를 들어 `INStartWorkoutIntent.workoutName`:

    [![](implementing-sirikit-images/plist05.png "INStartWorkoutIntent.workoutName 키")](implementing-sirikit-images/plist05.png#lightbox)
7. 추가 합니다 `ParameterVocabulary` 키를 `ParameterVocabularies` 사용 하 여 키를 **형식** 의 `Array`:

    [![](implementing-sirikit-images/plist06.png "배열 형식을 사용 하 여 ParameterVocabularies 키 ParameterVocabulary 키 추가")](implementing-sirikit-images/plist06.png#lightbox)
8. 사용 하 여 새 키를 추가 합니다 **형식** 의 `Dictionary`:

    [![](implementing-sirikit-images/plist07.png "사전 형식을 사용 하 여 새 키를 추가 합니다.")](implementing-sirikit-images/plist07.png#lightbox)
9. 추가 된 `VocabularyItemIdentifier` 사용 하 여 키를 **형식** 의 `String` 용어에 대 한 고유 ID를 지정:

    [![](implementing-sirikit-images/plist08.png "문자열 형식을 사용 하 여 VocabularyItemIdentifier 키를 추가 하 고 고유 ID를 지정 합니다.")](implementing-sirikit-images/plist08.png#lightbox)
10. 추가 된 `VocabularyItemSynonyms` 사용 하 여 키를 **형식** 의 `Array`:

    [![](implementing-sirikit-images/plist09.png "배열 형식을 사용 하 여 VocabularyItemSynonyms 키 추가")](implementing-sirikit-images/plist09.png#lightbox)
11. 사용 하 여 새 키를 추가 합니다 **형식** 의 `Dictionary`:

    [![](implementing-sirikit-images/plist10.png "사전 형식을 사용 하 여 새 키를 추가 합니다.")](implementing-sirikit-images/plist10.png#lightbox)
12. 추가 된 `VocabularyItemPhrase` 사용 하 여 키를 **형식** 의 `String` 앱에서 정의 하는 용어 및:

    [![](implementing-sirikit-images/plist11.png "문자열의 형식 및 앱을 정의 하는 용어를 사용 하 여 VocabularyItemPhrase 키 추가")](implementing-sirikit-images/plist11.png#lightbox)
13. 추가 된 `VocabularyItemPronunciation` 사용 하 여 키를 **형식** 의 `String` 및 용어의 음성 발음:

    [![](implementing-sirikit-images/plist12.png "문자열 형식 및 용어의 음성 발음 VocabularyItemPronunciation 키 추가")](implementing-sirikit-images/plist12.png#lightbox)
14. 추가 된 `VocabularyItemExamples` 사용 하 여 키를 **형식** 의 `Array`:

    [![](implementing-sirikit-images/plist13.png "배열 형식을 사용 하 여 VocabularyItemExamples 키 추가")](implementing-sirikit-images/plist13.png#lightbox)
15. 몇 가지 추가 `String` 용어의 사용 하 여 예제를 사용 하 여 키:

    [![](implementing-sirikit-images/plist14.png "예제에서는 용어를 사용 하 여 몇 가지 문자열 키를 추가 합니다.")](implementing-sirikit-images/plist14.png#lightbox)
16. 앱에서 정의 해야 다른 모든 사용자 지정 조건에 대해 위의 단계를 반복 합니다.
17. 축소 된 `ParameterVocabularies` 키입니다.
18. 추가 된 `IntentPhrases` 사용 하 여 키를 **형식** 의 `Array`:

    [![](implementing-sirikit-images/plist15.png "배열 형식을 사용 하 여 IntentPhrases 키 추가")](implementing-sirikit-images/plist15.png#lightbox)
19. 사용 하 여 새 키를 추가 합니다 **형식** 의 `Dictionary`:

    [![](implementing-sirikit-images/plist16.png "사전 형식을 사용 하 여 새 키를 추가 합니다.")](implementing-sirikit-images/plist16.png#lightbox)
20. 추가 된 `IntentName` 사용 하 여 키를 **형식** 의 `String` 예 의도 및:

    [![](implementing-sirikit-images/plist17.png "예를 들어 문자열의 형식 및 의도 사용 하 여 IntentName 키 추가")](implementing-sirikit-images/plist17.png#lightbox)
21. 추가 된 `IntentExamples` 사용 하 여 키를 **형식** 의 `Array`:

    [![](implementing-sirikit-images/plist18.png "배열 형식을 사용 하 여 IntentExamples 키 추가")](implementing-sirikit-images/plist18.png#lightbox)
22. 몇 가지 추가 `String` 용어의 사용 하 여 예제를 사용 하 여 키:

    [![](implementing-sirikit-images/plist19.png "예제에서는 용어를 사용 하 여 몇 가지 문자열 키를 추가 합니다.")](implementing-sirikit-images/plist19.png#lightbox)
23. 앱의 사용 예를 제공 하는 데 필요한 모든 의도 대 한 위의 단계를 반복 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 프로젝트 이름을 마우스 오른쪽 단추로 클릭 합니다 **솔루션 탐색기** 선택한 **추가 > 새 항목... > Apple > 속성 목록 > Info.plist**:

    [![](implementing-sirikit-images/plist01.w157-sml.png "새 Info.plist를 추가 합니다.")](implementing-sirikit-images/plist01.w157.png#lightbox)

2. 두 번 클릭 합니다 `AppIntentVocabulary.plist` 파일을 **솔루션 탐색기** 을 편집용으로 엽니다.
3. 클릭 합니다 **+** 키를 추가 하려면 설정 합니다 **이름** 를 `ParameterVocabularies` 및 **형식** 를 `Array`:

    [![](implementing-sirikit-images/plist02w.png "배열 형식과 ParameterVocabularies 이름을 설정합니다")](implementing-sirikit-images/plist02w.png#lightbox)
4. 확장 `ParameterVocabularies` 클릭 합니다 **+** 단추를 설정 합니다 **형식** 에 `Dictionary`:

    [![](implementing-sirikit-images/plist03w.png "사전 형식 설정")](implementing-sirikit-images/plist03w.png#lightbox)
5. 클릭 합니다 **+** 새 키를 추가 하려면 설정 합니다 **이름** 에 `ParameterNames` 및 **형식** 에 `Array`:

    [![](implementing-sirikit-images/plist04w.png "배열 형식과 ParameterNames 이름을 설정합니다")](implementing-sirikit-images/plist04w.png#lightbox)
6. 클릭 합니다 **+** 사용 하 여 새 키를 추가 하는 **형식** 의 `String` 값을 사용할 수 있는 매개 변수 이름 중 하나입니다. 예를 들어 `INStartWorkoutIntent.workoutName`:

    [![](implementing-sirikit-images/plist05w.png "INStartWorkoutIntent.workoutName 키")](implementing-sirikit-images/plist05w.png#lightbox)
7. 추가 합니다 `ParameterVocabulary` 키를 `ParameterVocabularies` 사용 하 여 키를 **형식** 의 `Array`:

    [![](implementing-sirikit-images/plist06w.png "배열 형식을 사용 하 여 ParameterVocabularies 키 ParameterVocabulary 키 추가")](implementing-sirikit-images/plist06w.png#lightbox)
8. 사용 하 여 새 키를 추가 합니다 **형식** 의 `Dictionary`:

    [![](implementing-sirikit-images/plist07w.png "사전 형식을 사용 하 여 새 키를 추가 합니다.")](implementing-sirikit-images/plist07w.png#lightbox)
9. 추가 된 `VocabularyItemIdentifier` 사용 하 여 키를 **형식** 의 `String` 용어에 대 한 고유 ID를 지정:

    [![](implementing-sirikit-images/plist08w.png "문자열 형식을 사용 하 여 VocabularyItemIdentifier 키를 추가 하 고 용어에 대 한 고유 ID를 지정 합니다.")](implementing-sirikit-images/plist08w.png#lightbox)
10. 추가 된 `VocabularyItemSynonyms` 사용 하 여 키를 **형식** 의 `Array`:

    [![](implementing-sirikit-images/plist09w.png "배열 형식을 사용 하 여 VocabularyItemSynonyms 키 추가")](implementing-sirikit-images/plist09w.png#lightbox)
11. 사용 하 여 새 키를 추가 합니다 **형식** 의 `Dictionary`:

    [![](implementing-sirikit-images/plist10w.png "사전 형식을 사용 하 여 새 키를 추가 합니다.")](implementing-sirikit-images/plist10w.png#lightbox)
12. 추가 된 `VocabularyItemPhrase` 사용 하 여 키를 **형식** 의 `String` 앱에서 정의 하는 용어 및:

    [![](implementing-sirikit-images/plist11w.png "문자열의 형식 및 앱을 정의 하는 용어를 사용 하 여 VocabularyItemPhrase 키 추가")](implementing-sirikit-images/plist11w.png#lightbox)
13. 추가 된 `VocabularyItemPronunciation` 사용 하 여 키를 **형식** 의 `String` 및 용어의 음성 발음:

    [![](implementing-sirikit-images/plist12w.png "문자열 형식 및 용어의 음성 발음 VocabularyItemPronunciation 키 추가")](implementing-sirikit-images/plist12w.png#lightbox)
14. 추가 된 `VocabularyItemExamples` 사용 하 여 키를 **형식** 의 `Array`:

    [![](implementing-sirikit-images/plist13w.png "배열 형식을 사용 하 여 VocabularyItemExamples 키 추가")](implementing-sirikit-images/plist13w.png#lightbox)
15. 몇 가지 추가 `String` 용어의 사용 하 여 예제를 사용 하 여 키:

    [![](implementing-sirikit-images/plist14w.png "예제에서는 용어를 사용 하 여 몇 가지 문자열 키를 추가 합니다.")](implementing-sirikit-images/plist14w.png#lightbox)
16. 앱에서 정의 해야 다른 모든 사용자 지정 조건에 대해 위의 단계를 반복 합니다.
17. 축소 된 `ParameterVocabularies` 키입니다.
18. 추가 된 `IntentPhrases` 사용 하 여 키를 **형식** 의 `Array`:

    [![](implementing-sirikit-images/plist15w.png "배열 형식을 사용 하 여 IntentPhrases 키 추가")](implementing-sirikit-images/plist15w.png#lightbox)
19. 사용 하 여 새 키를 추가 합니다 **형식** 의 `Dictionary`:

    [![](implementing-sirikit-images/plist16w.png "사전 형식을 사용 하 여 새 키를 추가 합니다.")](implementing-sirikit-images/plist16w.png#lightbox)
20. 추가 된 `IntentName` 사용 하 여 키를 **형식** 의 `String` 예 의도 및:

    [![](implementing-sirikit-images/plist17w.png "예를 들어 문자열의 형식 및 의도 사용 하 여 IntentName 키 추가")](implementing-sirikit-images/plist17w.png#lightbox)
21. 추가 된 `IntentExamples` 사용 하 여 키를 **형식** 의 `Array`:

    [![](implementing-sirikit-images/plist18w.png "배열 형식을 사용 하 여 IntentExamples 키 추가")](implementing-sirikit-images/plist18w.png#lightbox)
22. 몇 가지 추가 `String` 용어의 사용 하 여 예제를 사용 하 여 키:

    [![](implementing-sirikit-images/plist19w.png "예제에서는 용어를 사용 하 여 몇 가지 문자열 키를 추가 합니다.")](implementing-sirikit-images/plist19w.png#lightbox)
23. 앱의 사용 예를 제공 하는 데 필요한 모든 의도 대 한 위의 단계를 반복 합니다.

-----

> [!IMPORTANT]
> `AppIntentVocabulary.plist` 등록할 테스트에서 Siri를 사용 하 여 장치 개발 도중 Siri 사용자 지정 어휘를 통합 하기 위해 약간의 시간이 걸릴 수 있습니다. 결과적으로, 테스터 업데이트 되었을 때 앱 특정 어휘를 테스트 하기 전에 몇 분 정도 기다려야 할 수 있습니다.

자세한 내용은 참조 하십시오 우리의 [앱 특정 어휘 참조](~/ios/platform/sirikit/understanding-sirikit.md) 과 Apple [사용자 지정 어휘 참조 지정](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1)합니다.

## <a name="adding-an-intents-extension"></a>인 텐트 확장 추가

SiriKit을 채택 하도록 앱 준비 되었으므로 이제는 개발자 하나 (또는 이상) 인 텐트 확장 Siri의 통합에 필요한 의도 처리 하기 위해 솔루션을 추가 해야 합니다.

필요한 각 인 텐트 확장을 위한 다음 단계를 수행 합니다.

- Xamarin.iOS 앱 솔루션에는 인 텐트 확장 프로젝트를 추가 합니다.
- 인 텐트 확장 구성 `Info.plist` 파일입니다.
- 인 텐트 확장 기본 클래스를 수정 합니다.

자세한 내용은 참조 하십시오 우리의 [은 인 텐트 확장 참조](~/ios/platform/sirikit/understanding-sirikit.md) 과 Apple [인 텐트 확장 참조를 만드는](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CreatingtheIntentsExtension.html#//apple_ref/doc/uid/TP40016875-CH4-SW1)합니다.

### <a name="creating-the-extension"></a>확장 만들기

인 텐트 확장 솔루션에 추가 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 마우스 오른쪽 단추로 클릭 합니다 **솔루션 이름** 에 **Solution Pad** 선택한 **추가** > **새 프로젝트 추가...** .
2. 대화 상자에서 선택 **iOS** > **확장** > **의도 확장** 을 클릭 합니다 **다음** 단추: 

    [![](implementing-sirikit-images/intents05.png "의도 확장 선택")](implementing-sirikit-images/intents05.png#lightbox)
3. 그런 다음 입력을 **이름** 의도 확장을 클릭 합니다 **다음** 단추: 

    [![](implementing-sirikit-images/intents06.png "의도 확장에 대 한 이름을 입력 합니다.")](implementing-sirikit-images/intents06.png#lightbox)
4. 마지막으로 클릭 하 여 **만들기** 의도 확장 앱 솔루션에 추가 하려면 단추: 

    [![](implementing-sirikit-images/intents07.png "앱 솔루션에 의도 확장 추가")](implementing-sirikit-images/intents07.png#lightbox)
5. 에 **솔루션 탐색기**, 마우스 오른쪽 단추로 클릭 합니다 **참조** 새로 만든된 의도 확장의 폴더입니다. (즉 앱 위에서 만든) 공용 공유 코드 라이브러리 프로젝트의 이름을 확인 하 고 클릭 합니다 **확인** 단추: 

    [![](implementing-sirikit-images/intents08.png "공용 공유 코드 라이브러리 프로젝트의 이름을 선택 합니다.")](implementing-sirikit-images/intents08.png#lightbox)
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 마우스 오른쪽 단추로 클릭 합니다 **솔루션 이름** 에 **솔루션 탐색기** 선택한 **추가** > **새 프로젝트 추가...** .
2. 선택 대화 상자에서 **시각적 C# > iOS 확장 > 의도 확장** 을 클릭 합니다 **다음** 단추:

    [![](implementing-sirikit-images/intents05.w157-sml.png "의도 확장 선택")](implementing-sirikit-images/intents05.w157.png#lightbox)
3. 다음 입력을 **이름을** 의도 확장을 클릭 합니다 **확인** 단추입니다.
1. 에 **솔루션 탐색기**, 마우스 오른쪽 단추로 클릭 합니다 **참조** 은 새로 만든 인 텐트 확장의 폴더 선택한 **추가 > 참조**. (즉 앱 위에서 만든) 공용 공유 코드 라이브러리 프로젝트의 이름을 확인 하 고 클릭 합니다 **확인** 단추:

    [![](implementing-sirikit-images/intents08w.png "공용 공유 코드 라이브러리 프로젝트의 이름을 선택 합니다.")](implementing-sirikit-images/intents08w.png#lightbox)
    
-----

의도 확장명의 수에 대 한 이러한 단계를 반복 (기준 [확장에 대 한 앱을 설계](#architecting-the-app-for-extensions) 위의 섹션) 앱 해야 하는 합니다.

### <a name="configuring-the-infoplist"></a>Info.plist를 구성합니다.

각 앱의 솔루션을 추가 하는 인 텐트 확장에 대해 구성 되어야 합니다는 `Info.plist` 파일을 앱에서 작동 합니다.

모든 일반적인 앱 확장을 마찬가지로 앱의 기존 키 갖습니다 `NSExtension` 고 `NSExtensionAttributes`입니다. 인 텐트 확장 구성 해야 하는 두 가지 새로운 특성을 가지 있습니다.

[![](implementing-sirikit-images/intents01.png "구성 해야 하는 두 가지 새로운 특성")](implementing-sirikit-images/intents01.png#lightbox)

- **IntentsSupported** -필수 항목이 며 앱 의도 확장 프로그램에서 지원 하려는 의도 클래스 이름의 배열로 구성 됩니다.
- **IntentsRestrictedWhileLocked** -앱 확장의 잠금 화면 동작을 지정 하는 데는 선택적 키입니다. 앱 의도 확장 프로그램에서 사용 하 여에 로그인 해야 할 하려는 의도 클래스 이름의 배열을 이루어져 있습니다.

의도 확장을 구성 하 `Info.plist` 파일을에서 두 번 클릭 합니다 **솔루션 탐색기** 을 편집용으로 엽니다. 다음으로 전환 하는 **소스** 확인 한 후 확장을 `NSExtension` 및 `NSExtensionAttributes` 편집기에서 키:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](implementing-sirikit-images/intents02.png "편집기에서 NSExtension 및 NSExtensionAttributes 키")](implementing-sirikit-images/intents02.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents02w.png "편집기에서 NSExtension 및 NSExtensionAttributes 키")](implementing-sirikit-images/intents02w.png#lightbox)

-----

확장 된 `IntentsSupported` 키 및이 확장은 지 원하는 모든 의도 클래스의 이름을 추가 합니다. 예제 MonkeyChat 앱에 대 한 지원의 `INSendMessageIntent`:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](implementing-sirikit-images/intents09.png "INSendMessageIntent 키")](implementing-sirikit-images/intents09.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents09w.png "INSendMessageIntent 키")](implementing-sirikit-images/intents09w.png#lightbox)

-----

앱 사용자 지정된 의도 사용 하 여 차례로 확장 하 고 장치에 로그온는 필요에 따라 필요한 경우는 `IntentRestrictedWhileLocked` 키 및 액세스 제한 된 의도의 클래스 이름을 추가 합니다. 예제 MonkeyChat 앱에 대 한 사용자 로그인 해야 하므로 추가 했습니다 채팅 메시지를 보내려고 `INSendMessageIntent`:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](implementing-sirikit-images/intents10.png "추가한 INSendMessageIntent 키")](implementing-sirikit-images/intents10.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents10w.png "추가한 INSendMessageIntent 키")](implementing-sirikit-images/intents10w.png#lightbox)

-----


사용 가능한 의도 도메인의 전체 목록은, Apple의를 참조 하세요 [의도 도메인 참조](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)합니다.

### <a name="configuring-the-main-class"></a>기본 클래스를 구성합니다.

다음으로, 개발자 Siri에 의도 확장에 대 한 진입점으로 작동 하는 기본 클래스를 구성 해야 합니다. 하위 클래스 여야 합니다 `INExtension` 는 준수 하는 `IINIntentHandler` 위임 합니다. 예를 들어:

```csharp
using System;
using System.Collections.Generic;

using Foundation;
using Intents;

namespace MonkeyChatIntents
{
    [Register ("IntentHandler")]
    public class IntentHandler : INExtension, IINSendMessageIntentHandling, IINSearchForMessagesIntentHandling, IINSetMessageAttributeIntentHandling
    {
        #region Constructors
        protected IntentHandler (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override NSObject GetHandler (INIntent intent)
        {
            // This is the default implementation.  If you want different objects to handle different intents,
            // you can override this and return the handler you want for that particular intent.

            return this;
        }
        #endregion
        ...
    }
}
```

앱 의도 확장 기본 클래스에서 구현 해야 하는 독립 메서드가 하나는 `GetHandler` 메서드. 이 메서드가 SiriKit으로 의도 전달 되 고 앱에 반환 해야 합니다는 **의도 처리기** 지정된 의도의 형식과 일치 하는 합니다.

MonkeyChat 앱의 예는 한 가지 의도 처리 하므로 반환 하는 자체는 `GetHandler` 메서드. 개발자 의도 각 형식에 대 한 클래스를 추가 하 고 여기에 따라 인스턴스를 반환할 확장 둘 이상의 의도 처리 하는 경우는 `Intent` 메서드에 전달 합니다.

### <a name="handling-the-resolve-stage"></a>확인 단계를 처리합니다.

해결 단계 의도 확장은 명확 하 고 매개 변수 유효성 검사 전달 Siri에서 하 고 사용자의 대화를 통해 설정 되었습니다.

Siri에서 전송 하는 각 매개 변수에는 `Resolve` 메서드. 앱을 앱 사용자의 올바른 답을 사용 하려면 Siri의 도움말을 해야 할 수는 모든 매개 변수에 대해이 메서드를 구현 해야 합니다.

예제 MonkeyChat 앱의 경우 의도 확장에는 하나 이상의 받는 사람에 게 메시지를 전송 하도록 해야 합니다. 목록의 각 받는 사람에 대 한 다음 결과 가질 수 있는 연락처 검색 확장 해야 합니다.

- 정확히 하나의 일치 하는 연락처를 찾았습니다.
- 두 개 이상 일치 하는 연락처를 찾았습니다.
- 일치 하는 대화 상대가 없습니다 확인할 수 있습니다.

또한 MonkeyChat 메시지의 본문 내용이 필요합니다. 사용자가이 제공 되지 않으면 Siri 콘텐츠에 대 한 사용자에 게 해야 합니다.

의도 확장은 이러한 각 경우를 정상적으로 처리 해야 합니다.


```csharp
[Export ("resolveRecipientsForSearchForMessages:withCompletion:")]
public void ResolveRecipients (INSendMessageIntent intent, Action<INPersonResolutionResult []> completion)
{
    var recipients = intent.Recipients;
    // If no recipients were provided we'll need to prompt for a value.
    if (recipients.Length == 0) {
        completion (new INPersonResolutionResult [] { (INPersonResolutionResult)INPersonResolutionResult.NeedsValue });
        return;
    }

    var resolutionResults = new List<INPersonResolutionResult> ();

    foreach (var recipient in recipients) {
        var matchingContacts = new INPerson [] { recipient }; // Implement your contact matching logic here to create an array of matching contacts
        if (matchingContacts.Length > 1) {
            // We need Siri's help to ask user to pick one from the matches.
            resolutionResults.Add (INPersonResolutionResult.GetDisambiguation (matchingContacts));
        } else if (matchingContacts.Length == 1) {
            // We have exactly one matching contact
            resolutionResults.Add (INPersonResolutionResult.GetSuccess (recipient));
        } else {
            // We have no contacts matching the description provided
            resolutionResults.Add ((INPersonResolutionResult)INPersonResolutionResult.Unsupported);
        }
    }

    completion (resolutionResults.ToArray ());
}

[Export ("resolveContentForSendMessage:withCompletion:")]
public void ResolveContent (INSendMessageIntent intent, Action<INStringResolutionResult> completion)
{
    var text = intent.Content;
    if (!string.IsNullOrEmpty (text))
        completion (INStringResolutionResult.GetSuccess (text));
    else
        completion ((INStringResolutionResult)INStringResolutionResult.NeedsValue);
}
```

자세한 내용은 참조 하십시오 우리의 [해결 단계 참조](~/ios/platform/sirikit/understanding-sirikit.md) 과 Apple [Resolving 및 의도 참조 처리](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ResolvingandHandlingIntents.html#//apple_ref/doc/uid/TP40016875-CH5-SW1)합니다.

### <a name="handling-the-confirm-stage"></a>확인 단계를 처리합니다.

확인 단계를 사용 하는 의도 확장 모두 사용자의 요청을 수행 하는 정보를 확인 하는 위치는 것입니다. 앱의 모든 지원 세부 정보는에 대 한 발생 Siri를 확인할 수는 사용자를 사용 하 여 해당 이것이 의도 한 동작을 따라 확인 됩니다를 보내려고 합니다.

```csharp
[Export ("confirmSendMessage:completion:")]
public void ConfirmSendMessage (INSendMessageIntent intent, Action<INSendMessageIntentResponse> completion)
{
    // Verify user is authenticated and the app is ready to send a message.
    ...

    var userActivity = new NSUserActivity (nameof (INSendMessageIntent));
    var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Ready, userActivity);
    completion (response);
}
```

자세한 내용은 참조 하십시오 우리의 [의 확인 단계 참조](~/ios/platform/sirikit/understanding-sirikit.md)합니다.

### <a name="processing-the-intent"></a>의도 처리합니다.

여기서 의도 확장을 사용자의 요청을 수행 하 고 사용자는 알림을 받을 수 있도록 Siri에 다시 결과 전달 작업을 실제로 수행 하는 지점입니다.


```csharp
public void HandleSendMessage (INSendMessageIntent intent, Action<INSendMessageIntentResponse> completion)
{
    // Implement the application logic to send a message here.
    ...

    var userActivity = new NSUserActivity (nameof (INSendMessageIntent));
    var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, userActivity);
    completion (response);
}

public void HandleSearchForMessages (INSearchForMessagesIntent intent, Action<INSearchForMessagesIntentResponse> completion)
{
    // Implement the application logic to find a message that matches the information in the intent.
    ...

    var userActivity = new NSUserActivity (nameof (INSearchForMessagesIntent));
    var response = new INSearchForMessagesIntentResponse (INSearchForMessagesIntentResponseCode.Success, userActivity);

    // Initialize with found message's attributes
    var sender = new INPerson (new INPersonHandle ("sarah@example.com", INPersonHandleType.EmailAddress), null, "Sarah", null, null, null);
    var recipient = new INPerson (new INPersonHandle ("+1-415-555-5555", INPersonHandleType.PhoneNumber), null, "John", null, null, null);
    var message = new INMessage ("identifier", "I am so excited about SiriKit!", NSDate.Now, sender, new INPerson [] { recipient });
    response.Messages = new INMessage [] { message };
    completion (response);
}

public void HandleSetMessageAttribute (INSetMessageAttributeIntent intent, Action<INSetMessageAttributeIntentResponse> completion)
{
    // Implement the application logic to set the message attribute here.
    ...

    var userActivity = new NSUserActivity (nameof (INSetMessageAttributeIntent));
    var response = new INSetMessageAttributeIntentResponse (INSetMessageAttributeIntentResponseCode.Success, userActivity);
    completion (response);
}
```

자세한 내용은 참조 하십시오 우리의 [처리 단계 참조](~/ios/platform/sirikit/understanding-sirikit.md)합니다.

## <a name="adding-an-intents-ui-extension"></a>인 텐트 UI 확장을 추가합니다.

옵션은 인 텐트 UI 확장을 만들어 앱의 UI 및 Siri 환경에 대 한 브랜딩 기회를 제공 하 고 앱에 연결 된 것으로 생각 될 사용자를 확인 합니다. 이 확장을 사용 하 여 앱은 브랜드는 물론 visual 및 기타 기록 정보를 가져올 수 있습니다.

[![](implementing-sirikit-images/intentsui01.png "인 텐트 UI 확장 출력 예")](implementing-sirikit-images/intentsui01.png#lightbox)

인 텐트 확장 처럼 개발자 인 텐트 UI 확장에 대 한 다음 단계를 수행 합니다.

- Xamarin.iOS 앱 솔루션에는 인 텐트 UI 확장 프로젝트를 추가 합니다.
- 인 텐트 UI 확장 구성 `Info.plist` 파일입니다.
- 인 텐트 UI 확장 기본 클래스를 수정 합니다.

자세한 내용은 참조 하십시오 우리의 [은 인 텐트 UI 확장 참조](~/ios/platform/sirikit/understanding-sirikit.md) 과 Apple [사용자 지정 인터페이스 참조를 제공](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ProvidingaCustomInterface.html#//apple_ref/doc/uid/TP40016875-CH7-SW1)합니다.

### <a name="creating-the-extension"></a>확장 만들기

인 텐트 UI 확장 솔루션에 추가 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 마우스 오른쪽 단추로 클릭 합니다 **솔루션 이름** 에 **Solution Pad** 선택한 **추가** > **새 프로젝트 추가...** .
2. 대화 상자에서 선택 **iOS** > **확장** > **의도 UI 확장** 을 클릭 합니다 **다음** 단추: 

    [![](implementing-sirikit-images/intents11.png "의도 UI 확장을 선택 합니다.")](implementing-sirikit-images/intents11.png#lightbox)
3. 그런 다음 입력을 **이름** 의도 확장을 클릭 합니다 **다음** 단추: 

    [![](implementing-sirikit-images/intents12.png "의도 확장에 대 한 이름을 입력 합니다.")](implementing-sirikit-images/intents12.png#lightbox)
4. 마지막으로 클릭 하 여 **만들기** 의도 확장 앱 솔루션에 추가 하려면 단추: 

    [![](implementing-sirikit-images/intents13.png "앱 솔루션에 의도 확장 추가")](implementing-sirikit-images/intents13.png#lightbox)
5. 에 **솔루션 탐색기**, 마우스 오른쪽 단추로 클릭 합니다 **참조** 새로 만든된 의도 확장의 폴더입니다. (즉 앱 위에서 만든) 공용 공유 코드 라이브러리 프로젝트의 이름을 확인 하 고 클릭 합니다 **확인** 단추: 

    [![](implementing-sirikit-images/intents14.png "공용 공유 코드 라이브러리 프로젝트의 이름을 선택 합니다.")](implementing-sirikit-images/intents14.png#lightbox)
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 마우스 오른쪽 단추로 클릭 합니다 **솔루션 이름** 에 **솔루션 탐색기** 선택한 **추가** > **새 프로젝트 추가...**
2. 대화 상자에서 선택 **iOS** > **확장** > **의도 UI 확장** 을 클릭 합니다 **다음** 단추입니다.
3. 다음 입력을 **이름을** 의도 확장을 클릭 합니다 **확인** 단추입니다.
5. 에 **솔루션 탐색기**, 마우스 오른쪽 단추로 클릭 합니다 **참조** 새로 만든된 의도 확장의 폴더입니다. (즉 앱 위에서 만든) 공용 공유 코드 라이브러리 프로젝트의 이름을 확인 하 고 클릭 합니다 **확인** 단추입니다.
    
-----

### <a name="configuring-the-infoplist"></a>Info.plist를 구성합니다.

인 텐트 UI 확장 구성 `Info.plist` 파일을 앱에서 작동 합니다.

모든 일반적인 앱 확장을 마찬가지로 앱의 기존 키 갖습니다 `NSExtension` 고 `NSExtensionAttributes`입니다. 인 텐트 확장에는 구성 해야 하는 새로운 특성 다음과 같습니다.

[![](implementing-sirikit-images/intents03.png "구성 해야 하는 하나의 새 특성")](implementing-sirikit-images/intents03.png#lightbox)

**IntentsSupported** 필수 항목이 며 의도 확장 프로그램에서 지원 하 고 응용 프로그램 의도 클래스 이름의 배열로 구성 됩니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

의도 UI 확장을 구성 하 `Info.plist` 파일을에서 두 번 클릭 합니다 **솔루션 탐색기** 을 편집용으로 엽니다. 다음으로 전환 하는 **소스** 확인 한 후 확장을 `NSExtension` 및 `NSExtensionAttributes` 편집기에서 키:

[![](implementing-sirikit-images/intents04.png "편집기에서 NSExtension 및 NSExtensionAttributes 키")](implementing-sirikit-images/intents04.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

의도 UI 확장을 구성 하 `Info.plist` 파일을에서 두 번 클릭 합니다 **솔루션 탐색기** 을 편집용으로 엽니다. 확장 된 `NSExtension` 고 `NSExtensionAttributes` 편집기에서 키:

[![](implementing-sirikit-images/intents04w.png "편집기에서 사항 NSExtension 및 NSExtensionAttributes 키")](implementing-sirikit-images/intents04w.png#lightbox)

-----

확장 된 `IntentsSupported` 키 및이 확장은 지 원하는 모든 의도 클래스의 이름을 추가 합니다. 예제 MonkeyChat 앱에 대 한 지원의 `INSendMessageIntent`:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](implementing-sirikit-images/intents15.png "INSendMessageIntent 키")](implementing-sirikit-images/intents15.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents15w.png "INSendMessageIntent 키")](implementing-sirikit-images/intents15w.png#lightbox)

-----

사용 가능한 의도 도메인의 전체 목록은, Apple의를 참조 하세요 [의도 도메인 참조](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)합니다.

### <a name="configuring-the-main-class"></a>기본 클래스를 구성합니다.

Siri를 의도 UI 확장에 대 한 진입점으로 작동 하는 기본 클래스를 구성 합니다. 서브 클래스 해야 `UIViewController` 는 준수 하는 `IINUIHostedViewController` 인터페이스입니다. 예를 들어:

```csharp
using System;
using Foundation;
using CoreGraphics;
using Intents;
using IntentsUI;
using UIKit;

namespace MonkeyChatIntentsUI
{
    public partial class IntentViewController : UIViewController, IINUIHostedViewControlling
    {
        #region Constructors
        protected IntentViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }

        public override void DidReceiveMemoryWarning ()
        {
            // Releases the view if it doesn't have a superview.
            base.DidReceiveMemoryWarning ();

            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Public Methods
        [Export ("configureWithInteraction:context:completion:")]
        public void Configure (INInteraction interaction, INUIHostedViewContext context, Action<CGSize> completion)
        {
            // Do configuration here, including preparing views and calculating a desired size for presentation.

            if (completion != null)
                completion (DesiredSize ());
        }

        [Export ("desiredSize:")]
        public CGSize DesiredSize ()
        {
            return ExtensionContext.GetHostedViewMaximumAllowedSize ();
        }
        #endregion
    }
}
```

Siri 경과할를 `INInteraction` 클래스 인스턴스를 합니다 `Configure` 메서드의 `UIViewController` 의도 UI 확장 내에서 인스턴스.

`INInteraction` 개체는 세 가지 주요 확장에 대 한 정보를 제공 합니다.

1. 처리 중인 의도 하는 개체입니다.
2. 의도 응답 개체를 `Confirm` 고 `Handle` 의도 확장 메서드.
3. 앱 및 Siri 간의 상호 작용의 상태를 정의 하는 상호 작용 상태입니다.

`UIViewController` 인스턴스가 Siri와의 상호 작용에 대 한 원칙 클래스에서 상속 하므로 및 `UIViewController`, 모든 UIKit의 기능에 액세스할 수 합니다.

Siri가 호출 하는 경우는 `Configure` 메서드는 `UIViewController` 뷰 컨트롤러는 하거나 Siri Snippit 또는 지도 카드에서 호스팅할 수 없다는 뷰 컨텍스트를 전달 합니다.

Siri 앱 구성 앱 완료 되 면 보기의 원하는 크기를 반환 해야 하는 완료 처리기에도 전달 합니다.

### <a name="design-the-ui-in-ios-designer"></a>IOS 디자이너에서에서 UI 디자인

레이아웃은 인 텐트 UI 확장 사용자 iOS 디자이너 인터페이스입니다. 확장의 두 번 클릭 `MainInterface.storyboard` 파일을 **솔루션 탐색기** 을 편집용으로 엽니다. 모든 사용자 인터페이스를 구축 하 고 변경 내용을 저장 하려면 필요한 UI 요소를 끕니다.

> [!IMPORTANT]
> 와 같은 대화형 요소를 추가할 수 있지만 `UIButtons` 나 `UITextFields` 의도 UI 확장을 `UIViewController`, 이러한 엄격 하 게 사용할 수 없는 의도 UI에서 비 대화형으로 및 사용자 상호 작용 하는 일을 할 수 없습니다.

### <a name="wire-up-the-user-interface"></a>유선 전화 접속 사용자 인터페이스

IOS 디자이너에서에서 만든은 인 텐트 UI 확장의 사용자 인터페이스를 사용 하 여 편집 합니다 `UIViewController` 서브 클래스 및 재정의 `Configure` 같이 메서드:

```csharp
[Export ("configureWithInteraction:context:completion:")]
public void Configure (INInteraction interaction, INUIHostedViewContext context, Action<CGSize> completion)
{
    // Do configuration here, including preparing views and calculating a desired size for presentation.
    ...

    // Return desired size
    if (completion != null)
        completion (DesiredSize ());
}

[Export ("desiredSize:")]
public CGSize DesiredSize ()
{
    return ExtensionContext.GetHostedViewMaximumAllowedSize ();
}
```

### <a name="overriding-the-default-siri-ui"></a>기본 Siri UI를 재정의합니다.

인 텐트 UI 확장은 항상 UI의 맨 위에 있는 이름 및 앱 아이콘 같은 다른 Siri 내용과 함께 표시 되거나, 의도에 따라, 아래쪽 (예: 송신 또는 취소) 단추를 표시 될 수 있습니다.

Siri 메시징 등 기본적으로 사용자에 게 표시 되는 정보를 maps 앱을 앱에 맞는 하나를 사용 하 여 기본 환경을 바꿀 수 앱 바꿀 수 있는 몇 가지 인스턴스가 있습니다.

인 텐트 UI 확장 기본 Siri UI의 요소를 재정의 해야 하는 경우는 `UIViewController` 서브 클래스를 구현 해야 합니다는 `IINUIHostedViewSiriProviding` 인터페이스 및 특정 인터페이스 요소를 표시 하도록 옵트인 합니다.

다음 코드를 추가 `UIViewController` 하기가 Siri 의도 UI 확장을 이미 표시 되어 메시지 콘텐츠는 하위 클래스입니다.

```csharp
public bool DisplaysMessage {
    get {return true;}
}
```

### <a name="considerations"></a>고려 사항

Apple는 개발자 수행 된 다음 사항을 디자인 및 의도 UI 확장을 구현할 때 고려해 야 하는 것을 제안 합니다.

- **메모리 사용량이의 인식** -때문에 의도 UI 확장은 임시 이며 시스템 전체 앱을 사용 하는 보다 긴밀 하 게 메모리 제약 조건 적용 짧은 시간에 대 한 표시만 합니다.
- **최소 및 최대 뷰 크기를 고려해 야** -의도 UI 확장 모든 iOS 장치 유형, 크기 및 방향에 정상적으로 진행 되는지 확인 합니다. 또한 앱 Siri 다시 전송 하는 원하는 크기 부여할 못할 수 있습니다.
- **유연성 및 적응 레이아웃 패턴을 사용 하 여** -마찬가지로 UI는 모든 장치에서 잘 표시 되는지 확인 합니다.

## <a name="summary"></a>요약

이 문서에서는 SiriKit을 다루는 있으며 표시 하는 방법을 iOS 장치에서 Siri 및 맵 앱을 사용 하 여 사용자에 게 액세스할 수 있는 서비스 제공 하기 위해 Xamarin.iOS 앱에 추가할 수 있습니다.




## <a name="related-links"></a>관련 링크

- [ElizaChat 샘플](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [SiriKit 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [인 텐트 프레임 워크 참조](https://developer.apple.com/reference/intents)
- [인 텐트 UI 프레임 워크 참조](https://developer.apple.com/reference/intentsui)
- [의도 도메인 참조](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)
