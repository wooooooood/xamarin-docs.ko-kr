---
title: "SiriKit 구현"
description: "이 문서에 Xamarin.iOS 앱 SiriKit 지원을 구현 하는 데 필요한 단계에 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 20FFB981-EB10-48BA-BF79-40F37F0291EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 76787ecda1c2cd043b81482dcdbe3751d012ef74
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="implementing-sirikit"></a>SiriKit 구현

_이 문서에 Xamarin.iOS 앱 SiriKit 지원을 구현 하는 데 필요한 단계에 설명 합니다._



새로운 iOS 10 SiriKit iOS 장치에서 Siri와 지도 응용 프로그램을 사용 하는 사용자에 액세스할 수 있는 서비스를 제공 하도록 Xamarin.iOS 앱을 허용 합니다. 이 문서에서는 필요한 의도 확장, 의도 UI 확장 및 어휘를 추가 하 여 Xamarin.iOS 앱에 SiriKit 지원을 구현 하는 데 필요한 단계에 설명 합니다.

Siri의 개념과 작동 **도메인**, 그룹의 관련된 작업에 대 한 작업을 알고 있습니다. Siri와 응용 프로그램에 있는 각 상호 작용 다음과 같은 알려진된 서비스 도메인 중 하나에 해당 해야 합니다.

- 오디오 또는 비디오 호출 합니다.
- 안에서 예약 합니다.
- 체력 단련을 관리 합니다.
- 메시징입니다.
- 사진을 검색 합니다.
- 송신 하거나 수신 지불 합니다.

사용자가 앱 확장의 서비스 중 하나는 관련 된 Siri의 요청, SiriKit 확장 보냅니다는 **의도** 지원 데이터와 함께 사용자의 요청을 설명 하는 개체입니다. 앱 확장 한 다음 적절 한 생성 **응답** 개체에 대 한는 주어진 **의도**, 확장에서 요청을 처리할 수는 방법에 대해 자세히 설명 합니다.

이 가이드는 기존 앱에 SiriKit 지원을 포함 하는 빠른 예제를 제공 합니다. 이 예에서 사용할 예정 가짜 MonkeyChat 앱:

[![](implementing-sirikit-images/monkeychat01.png "MonkeyChat 아이콘")](implementing-sirikit-images/monkeychat01.png#lightbox)

MonkeyChat 유지 사용자의 친구의 연락처 자체 책, 각는 화면 이름 (예: Bobo 예:)와 관련 된 화면 이름 기준으로 텍스트 채팅 친구에 보낼 수 있습니다.

## <a name="extending-the-app-with-sirikit"></a>응용 프로그램을 SiriKit 확장

에 나와 있는 것 처럼는 [SiriKit 개념 이해](~/ios/platform/sirikit/understanding-sirikit.md) 가이드에서는 세 가지 주요 부분은 확장 SiriKit 사용 하 여 앱에 관련 된:

[![](implementing-sirikit-images/elements01.png "응용 프로그램을 SiriKit 다이어그램 확장")](implementing-sirikit-images/elements01.png#lightbox)

여기에는 다음이 포함됩니다.

1. **의도 확장** -사용자가 응답을 확인, 확인 하는 응용 프로그램 요청을 처리할 수 고 실제로 사용자의 요청을 충족 하는 작업을 수행 합니다.
2. **의도 UI 확장** - *Optional*, Siri 환경에서 응답을 사용자 지정 UI를 제공 하 고 UI 앱을 가져올 수 및 사용자의 경험을 보강 하기 위해 Siri에 브랜딩 합니다.
3. **응용 프로그램** -Siri 작업을 지원 하기 위해 특정 어휘 사용자와 응용 프로그램을 제공 합니다. 

모든 이러한 요소와 앱에서이 포함 하는 단계는 아래 섹션에서 자세히 설명 합니다.


## <a name="preparing-the-app"></a>응용 프로그램 준비

하지만 SiriKit 확장은 기반으로, 응용 프로그램에 모든 확장을 추가 하기 전에 몇 가지 개발자 SiriKit 단기간에 도움이 되도록 작업을 수행 해야 하는 합니다.

### <a name="moving-common-shared-code"></a>공용 공유 코드를 이동합니다. 

첫째, 개발자 간에 이동할 수을 공유 하는 일반적인 코드의 일부 응용 프로그램 및 확장명 중 하나로 _공유 프로젝트_, _이식 가능한 클래스 라이브러리 (PCLs)_ 또는 _네이티브 라이브러리_합니다.

확장 응용 프로그램을 수행 하는 작업을 모두 수행할 수 있게 될 해야 합니다. 샘플 MonkeyChat 앱 찾기 연락처, 새 연락처를 추가 하는 등의 측면에서 메시지 보내고 메시지 기록을 검색 합니다.

공유 프로젝트, PCL 또는 네이티브 라이브러리에이 공통 코드를 이동 하 여 한곳에서 해당 코드를 유지 하기 위해 쉽게 및를 확장 하 고 부모 앱 균일 한 환경 및 기능을 제공 사용자에 대 한 보장 합니다.

MonkeyChat 예제 앱의 경우 데이터 모델 및 처리 코드 네트워크 및 데이터베이스 액세스와 같은 네이티브 라이브러리로 이동 될 예정입니다.

다음을 수행합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac 용 Visual Studio를 시작 하 고 MonkeyChat 앱을 엽니다.
2. 솔루션 이름을 마우스 오른쪽 단추로 클릭는 **솔루션 패드** 선택 **추가** > **새 프로젝트...** : 

    [![](implementing-sirikit-images/prep01.png "새 프로젝트 추가")](implementing-sirikit-images/prep01.png#lightbox)
3. 선택 **iOS** > **라이브러리** > **클래스 라이브러리** 클릭는 **다음** 단추: 

    [![](implementing-sirikit-images/prep02.png "클래스 라이브러리를 선택 합니다.")](implementing-sirikit-images/prep02.png#lightbox)
4. 입력 `MonkeyChatCommon` 에 대 한는 **이름** 클릭는 **만들기** 단추: 

    [![](implementing-sirikit-images/prep03.png "이름에 대 한 MonkeyChatCommon 입력")](implementing-sirikit-images/prep03.png#lightbox)
5. 마우스 오른쪽 단추로 클릭는 **참조** 에서 기본 응용 프로그램의 폴더는 **솔루션 탐색기** 선택 **참조 편집...** . 확인 된 **MonkeyChatCommon** 프로젝트는 **확인** 단추: 

    [![](implementing-sirikit-images/prep05.png "MonkeyChatCommon 프로젝트를 확인 합니다.")](implementing-sirikit-images/prep05.png#lightbox)
6. 에 **솔루션 탐색기**, 기본 응용 프로그램에서 네이티브 라이브러리를 공용 공유 코드를 끕니다.
7. 끌어 MonkeyChat의 경우는 **DataModels** 및 **프로세서** 네이티브 라이브러리에 대 한 기본 응용 프로그램에서 폴더: 

    [![](implementing-sirikit-images/prep06.png "솔루션 탐색기에서 DataModels 및 프로세서 폴더")](implementing-sirikit-images/prep06.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio를 시작 하 고 MonkeyChat 앱을 엽니다.
2. 솔루션 이름을 마우스 오른쪽 단추로 클릭는 **솔루션 탐색기** 선택 **추가** > **새 프로젝트...** .
3. 선택 **Visual C#** > **공유 프로젝트** 클릭는 **다음** 단추: 

    [![](implementing-sirikit-images/prep02w.png "클래스 라이브러리를 선택 합니다.")](implementing-sirikit-images/prep02w.png#lightbox)
4. 입력 `MonkeyChatCommon` 에 대 한는 **이름** 클릭는 **만들기** 단추입니다.
5. 마우스 오른쪽 단추로 클릭는 **참조** 에서 기본 응용 프로그램의 폴더는 **솔루션 탐색기** 선택 **참조 편집...** . 확인 된 **MonkeyChatCommon** 프로젝트는 **확인** 단추: 

    [![](implementing-sirikit-images/prep05w.png "MonkeyChatCommon 프로젝트를 확인 합니다.")](implementing-sirikit-images/prep05w.png#lightbox)
6. 에 **솔루션 탐색기**, 공유 프로젝트에 기본 응용 프로그램에서 공용 공유 코드를 끕니다.
7. 끌어 MonkeyChat의 경우는 **DataModels** 및 **프로세서** 네이티브 라이브러리에 대 한 기본 응용 프로그램에서 폴더입니다.

-----

네이티브 라이브러리를 이동 하는 파일 중 하나를 편집 하 고 라이브러리의 이름과 일치 하도록 네임 스페이스를 변경 합니다. 예를 들어 변경 `MonkeyChat` 를 `MonkeyChatCommon`:

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

다음으로 기본 응용 프로그램으로 다시 이동 하 고 추가 `using` 문은 네이티브 라이브러리의 네임 스페이스에 대해 아무 곳 이나 응용 프로그램 사용 하 여 이동 된 클래스 중 하나:

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

### <a name="architecting-the-app-for-extensions"></a>확장에 대 한 응용 프로그램 설계

일반적으로 응용 프로그램은 여러 의도에 등록 하 고 개발자가 의도 확장명의 적절 한 수에 대 한 응용 프로그램의 구성 확인 합니다.

응용 프로그램 둘 이상의 의도 필요로 하는 경우 개발자가 각 의도 대 한 별도 의도 확장을 만들거나 의도 확장은 한 번에 배치 하면 의도 처리의 모든 있습니다.

에서 각 의도 대 한 별도 의도 확장을 생성 하도록 선택한 경우 개발자는 많은 양의 각 확장에 상용구 코드 복제를 종료 하 고 많은 양의 프로세서 및 메모리 오버 헤드를 만들 수 없습니다.

두 옵션 중에서 선택 하려면를 선택한 경우는 의도 자연스럽 게 속해 함께 참조 하십시오. 예를 들어, 오디오 및 비디오 호출을 수행 하는 응용 프로그램 유사한 작업을 처리 하는 것 처럼 단일 의도 확장에서 모두 이러한 의도 포함 하려는 및 대부분 코드 재사용을 제공할 수 있습니다.

의도 또는 그룹을 기존 그룹에 적용할 수 없는 의도 담는 데 응용 프로그램의 솔루션에 새 의도 확장을 만듭니다.


### <a name="setting-the-required-entitlements"></a>필요한 자격 설정

SiriKit 통합을 포함 하는 모든 Xamarin.iOS 앱, 올바른 권한이 설정 해야 합니다. 개발자 필수 이러한 자격이 올바르게 설정 하지 않는 경우 됩니다을 설치 하거나 iOS 10 이후 요구 사항 이기도 실제 iOS 하드웨어 10 (또는 그 이상)에서 앱을 테스트 수 시뮬레이터 SiriKit를 지원 하지 않습니다.

다음을 수행합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 두 번 클릭는 `Entitlements.plist` 파일에 **솔루션 탐색기** 를 편집 하기 위해 엽니다.
2. 전환 하는 **소스** 탭 합니다.
3. 추가 `com.apple.developer.siri` **속성**로 설정 된 **형식** 를 `Boolean` 및 **값** 를 `Yes`: 

    [![](implementing-sirikit-images/setup01.png "Com.apple.developer.siri 속성 추가")](implementing-sirikit-images/setup01.png#lightbox)
4. 파일의 변경 내용을 저장합니다.
5. 두 번 클릭는 **프로젝트 파일** 에 **솔루션 탐색기** 를 편집 하기 위해 엽니다.
6. 선택 **iOS 번들 서명** 되어 있는지 확인 하 고는 `Entitlements.plist` 에서 파일을 선택는 **사용자 지정 자격** 필드: 

    [![](implementing-sirikit-images/setup02.png "사용자 지정 자격 필드에서 Entitlements.plist 파일을 선택 합니다.")](implementing-sirikit-images/setup02.png#lightbox)
7. **확인** 단추를 클릭하여 변경 내용을 저장합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 두 번 클릭는 `Entitlements.plist` 파일에 **솔루션 탐색기** 를 편집 하기 위해 엽니다.
3. 추가 `com.apple.developer.siri` **속성**로 설정 된 **형식** 를 `Boolean` 및 **값** 를 `Yes`: 

    [![](implementing-sirikit-images/setup01w.png "Com.apple.developer.siri 속성 추가")](implementing-sirikit-images/setup01w.png#lightbox)
4. 파일의 변경 내용을 저장합니다.
5. 두 번 클릭는 **프로젝트 파일** 에 **솔루션 탐색기** 를 편집 하기 위해 엽니다.
6. 선택 **iOS 번들 서명** 되어 있는지 확인 하 고는 `Entitlements.plist` 에서 파일을 선택는 **사용자 지정 자격** 필드입니다.

-----

완료 되 면 응용 프로그램의 `Entitlements.plist` 파일은 다음과 같습니다 (에서 편집기에 열려 외부):

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

### <a name="correctly-provisioning-the-app"></a>응용 프로그램을 올바르게 프로 비전

Apple SiriKit 프레임 워크 SiriKit를 구현 하는 모든 Xamarin.iOS 앱 주위에 엄격한 보안으로 인해 _해야_ 올바른 응용 프로그램 ID와 자격 (위의 섹션 참조) 및 적절 한으로 서명 되어야 합니다 프로 비전 프로필입니다.

Mac에서 다음을 수행 합니다.

1. 웹 브라우저에서로 이동 [ http://developer.apple.com ](http://developer.apple.com) 및 사용자 계정에 로그인 합니다.
2. 클릭 **인증서**, **식별자** 및 **프로필**합니다.
3. 선택 **프로 비전 프로필** 선택 **의 앱 Id**, 클릭는  **+**  단추입니다.
4. 입력 한 **이름** 새 프로필에 대 한 합니다.
5. 입력 한 **번들 ID** Apple 다음의 권장 명명 합니다.
6. 으로 아래로 스크롤하여는 **응용 프로그램 서비스** 섹션에서 **SiriKit** 클릭는 **계속** 단추: 

    [![](implementing-sirikit-images/setup03.png "SiriKit 선택")](implementing-sirikit-images/setup03.png#lightbox)
7. 다음 설정의 모든 확인 **전송** 응용 프로그램 id입니다.
8. 선택 **프로 비전 프로필** > **개발**, 클릭는  **+**  단추를 선택는 **Apple ID**, 클릭 **계속**합니다.
9. 선택을 클릭 **모든**, 클릭 **계속**합니다.
10. 클릭 **모두 선택** 클릭 한 다음 다시 **계속**합니다.
11. 입력 한 **프로필 이름** 클릭, 사과 사용 하 여 제안의 명명 **계속**합니다.
12. Xcode를 시작합니다.
13. Xcode 메뉴에서 선택 **기본 설정 중...**
14. 선택 **계정**, 클릭는 **세부 정보 보기...** button: 

    [![](implementing-sirikit-images/setup04.png "계정을 선택 하십시오.")](implementing-sirikit-images/setup04.png#lightbox)
15. 클릭는 **모든 프로필 다운로드** 왼쪽 아래 모퉁이의 단추: 

    [![](implementing-sirikit-images/setup05.png "모든 프로필 다운로드")](implementing-sirikit-images/setup05.png#lightbox)
16. 확인는 **프로 비전 프로필** 만든 위에 설치가 완료 된 후 Xcode에서 합니다.
17. Mac.에 대 한 Visual Studio에서 SiriKit 지원을 추가할 프로젝트를 엽니다.
18. 두 번 클릭은 `Info.plist` 파일에 **솔루션 탐색기**합니다.
18. 확인 하는 **번들 식별자** 위의 Apple 개발자 포털에서 만든 것과 일치 합니다. 

    [![](implementing-sirikit-images/setup06.png "번들 식별자")](implementing-sirikit-images/setup06.png#lightbox)
18. 에 **솔루션 탐색기**, 선택는 **프로젝트**합니다.
19. 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **옵션**합니다.
21. 선택 **iOS 번들 서명**, 선택는 **서명 Identity** 및 **프로 비전 프로필** 위에서 만든: 

    [![](implementing-sirikit-images/setup07.png "서명 Id 및 프로비저닝 프로필을 선택 합니다.")](implementing-sirikit-images/setup07.png#lightbox)
22. **확인** 단추를 클릭하여 변경 내용을 저장합니다.

> [!IMPORTANT]
> 테스트 SiriKit에 대해서만 작동 실제 iOS 10 하드웨어 장치에서가 아니라 10 iOS 시뮬레이터. 실제 하드웨어에서 Xamarin.iOS 앱 활성화는 SiriKit 설치에 문제가 있을 경우 필요한 자격, 앱 ID, 서명 식별자 및 프로 비전 프로필이 구성 되었는지 확인 올바르게 모두 Apple 개발자 포털 및 Visual Studio에서 Mac.에 대 한

### <a name="requesting-siri-authorization"></a>Siri 권한 부여를 요청합니다.

모든 사용자 특정 어휘를 추가 하는 응용 프로그램 의도 확장 Siri 연결하기 전에 Siri 액세스 하는 사용자 로부터 권한 부여를 요청 해야 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

응용 프로그램의 편집 `Info.plist` 파일, 전환할는 **소스** 추가 `NSSiriUsageDescription` Siri와 앱은 사용 하는 방법을 설명 하는 문자열 값을 가진 키 형식의 데이터를 전송 됩니다. 예를 들어 MonkeyChat 앱 수 "MonkeyChat 연락처로 보내지며 Siri" 한다고 가정 합니다.

[![](implementing-sirikit-images/request01.png "Info.plist 편집기에서 NSSiriUsageDescription")](implementing-sirikit-images/request01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

응용 프로그램의 편집 `Info.plist` 파일을 추가 `NSSiriUsageDescription` Siri 내용과 앱은 사용 하는 방법을 설명 하는 문자열 값을 가진 키 형식의 데이터를 전송 됩니다. 예를 들어 MonkeyChat 앱 수 "MonkeyChat 연락처로 보내지며 Siri" 한다고 가정 합니다.

[![](implementing-sirikit-images/request01w.png "Info.plist 편집기에서 NSSiriUsageDescription")](implementing-sirikit-images/request01w.png#lightbox)

-----

호출 된 `RequestSiriAuthorization` 의 메서드는 `INPreferences` 앱을 처음 시작할 때 클래스. 편집 된 `AppDelegate.cs` 클래스 하 고는 `FinishedLaunching` 다음과 같은 메서드 모양을:


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

이 메서드는 처음으로 경고는 Siri 액세스를 허용 하 라는 메시지가 표시 됩니다. 개발자에 추가 하는 메시지는 `NSSiriUsageDescription` 위에이 경고에 표시 되도록 합니다. 처음 사용자 액세스를 거부 하는 경우 사용할 수 있습니다는 **설정을** 응용 프로그램에 대 한 액세스 권한을 부여 하는 앱입니다.

언제 든 지 응용 프로그램에 Siri 호출 하 여 액세스 하는 앱의 기능을 확인할 수는 `SiriAuthorizationStatus` 의 메서드는 `INPreferences` 클래스입니다.

### <a name="localization-and-siri"></a>지역화 및 Siri

IOS 장치에서 사용자가 다른 Siri 그때 시스템 기본 언어를 선택할 수 있습니다. 응용 프로그램 사용 해야 합니다 지역화 된 데이터를 사용할 때의 `SiriLanguageCode` 의 메서드는 `INPreferences` 클래스 Siri에서 언어 코드를 가져옵니다. 예를 들어:

```csharp
var language = INPreferences.SiriLanguageCode();

// Take action based on language
if (language == "en-US") {
    // Do something...
}
```

### <a name="adding-user-specific-vocabulary"></a>사용자 특정 어휘 추가

사용자 특정 어휘는 단어 또는 구를 고유한 응용 프로그램의 개별 사용자에 게 제공할 것입니다. 이러한 목록의 시작 부분에 가장 중요 한 용어가 있는 사용자에 대 한 가장 중요 한 사용 현황 우선 순위 정렬 용어를 정렬 된 집합으로 기본 응용 프로그램 (앱 확장명 제외)에서 런타임 시 제공 될 예정입니다.

사용자 특정 어휘는 다음 범주 중 하나에 속해야 합니다.

- 연락처 이름이 (연락처 프레임 워크에 의해 관리 되지 않는).
- 사진 태그입니다.
- 사진 앨범 이름입니다.
- 워크 아웃 이름입니다.

사용자 지정 어휘도 등록 하는 용어를 선택할 때만 용어를 선택할 사용자 응용 프로그램에 익숙하지 않는가 오해 될 수 있습니다. 하지 레지스터 일반적인 용어 "My 워크 아웃" 또는 "My 앨범" 등입니다. 예를 들어 MonkeyChat 응용 프로그램 사용자의 주소록에 각 연락처와 관련 된 애칭 등록 됩니다.

호출 하 여 사용자 특정 어휘도 제공는 `SetVocabularyStrings` 의 메서드는 `INVocabulary` 클래스에 전달 하는 `NSOrderedSet` 기본 응용 프로그램에서 컬렉션입니다. 응용 프로그램 항상 호출 해야는 `RemoveAllVocabularyStrings` 메서드 첫 번째 새 작업을 추가 하기 전에 모든 기존 용어를 제거 하려면. 예를 들어:

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

이 코드 위치에서 사용 될 호출 되 고 있습니다 다음과 같습니다.

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
> Siri 힌트로 사용자 지정 어휘를 취급 하 고 최대한 용어가 많이 포함 됩니다. 그러나 사용자 지정 어휘는 등록 하는 것이 중요 하므로 제한 된 공간 _만_ 따라서 등록 된 용어의 총 수를 최소로 유지 혼동 될 수 있는 용어입니다.

자세한 내용은 참조 하십시오 우리의 [사용자 특정 어휘 참조](~/ios/platform/sirikit/understanding-sirikit.md) 과 Apple [사용자 지정 어휘 참조를 지정 하](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1)합니다.

### <a name="adding-app-specific-vocabulary"></a>응용 프로그램 특정 어휘 추가

응용 프로그램 특정 어휘 모든 차량 형식 또는 워크 아웃 이름과 같은 응용 프로그램의 사용자를 특정 단어와 구를 임을 알 수를 정의 합니다. 에 정의 된 이러한 응용 프로그램의 일부 이므로 `AppIntentVocabulary.plist` 기본 응용 프로그램 번들의 일부로 파일입니다. 또한 이러한 단어와 구 지역화 해야 합니다.

응용 프로그램 특정 용어는 다음 범주 중 하나에 속해야 합니다.

- 옵션을 재정의 합니다.
- 워크 아웃 이름입니다.

응용 프로그램 특정 어휘 파일 두 개의 루트 수준 키를 포함합니다.

- `ParameterVocabularies` **필요한** -응용 프로그램의 사용자 지정 약관에 적용 되는 의도 매개 변수를 정의 합니다.
- `IntentPhrases` **선택적** -에 정의 된 사용자 지정 용어를 사용 하 여 예제 구문이 포함 되어는 `ParameterVocabularies`합니다.

각 항목에는 `ParameterVocabularies` ID 문자열, 용어에 적용 되는 용어는 의도 지정 해야 합니다. 또한 단일 용어 여러 의도를 적용할 수 있습니다.

필요한 파일 구조 및 허용 되는 값의 전체 목록에 대 한 Apple의를 참조 하십시오 [앱 어휘 파일 형식 참조](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1)합니다.

추가 하는 `AppIntentVocabulary.plist` 응용 프로그램 프로젝트에 파일에서 다음을 수행 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 프로젝트 이름을 마우스 오른쪽 단추로 클릭는 **솔루션 탐색기** 선택 **추가** > **새 파일...**   >  **iOS**:

    [![](implementing-sirikit-images/plist01.png "속성 목록 추가")](implementing-sirikit-images/plist01.png#lightbox) 
2. 두 번 클릭는 `AppIntentVocabulary.plist` 파일에 **솔루션 탐색기** 를 편집 하기 위해 엽니다.
3. 클릭는  **+**  설정 키를 추가 하는 **이름** 를 `ParameterVocabularies` 및 **형식** 를 `Array`:

    [![](implementing-sirikit-images/plist02.png "이름을 ParameterVocabularies 및 배열 형식으로 설정")](implementing-sirikit-images/plist02.png#lightbox)
4. 확장 `ParameterVocabularies` 클릭는  **+**  단추 및 설정의 **형식** 를 `Dictionary`:

    [![](implementing-sirikit-images/plist03.png "사전에 형식을 설정합니다")](implementing-sirikit-images/plist03.png#lightbox)
5. 클릭는  **+**  설정 하 여 새 키를 추가 하려면는 **이름** 를 `ParameterNames` 및 **형식** 를 `Array`:

    [![](implementing-sirikit-images/plist04.png "이름을 ParameterNames 및 배열 형식으로 설정")](implementing-sirikit-images/plist04.png#lightbox)
6. 클릭는  **+**  된 새 키를 추가 하는 **형식** 의 `String` 및 사용 가능한 매개 변수 이름 중 하나로 값입니다. 예를 들어 `INStartWorkoutIntent.workoutName`:

    [![](implementing-sirikit-images/plist05.png "INStartWorkoutIntent.workoutName 키")](implementing-sirikit-images/plist05.png#lightbox)
7. 추가 `ParameterVocabulary` 키를 `ParameterVocabularies` 와 키의 **형식** 의 `Array`:

    [![](implementing-sirikit-images/plist06.png "형식 배열 ParameterVocabularies 키에 ParameterVocabulary 키 추가")](implementing-sirikit-images/plist06.png#lightbox)
8. 사용 하 여 새 키를 추가 **형식** 의 `Dictionary`:

    [![](implementing-sirikit-images/plist07.png "사전 종류를 사용 하 여 새 키를 추가 합니다.")](implementing-sirikit-images/plist07.png#lightbox)
9. 추가 `VocabularyItemIdentifier` 인 키는 **형식** 의 `String` 용어에 대 한 고유 ID를 지정 합니다.

    [![](implementing-sirikit-images/plist08.png "문자열 형식을 사용 하 여 VocabularyItemIdentifier 키를 추가 하 고 고유 ID를 지정 합니다.")](implementing-sirikit-images/plist08.png#lightbox)
10. 추가 `VocabularyItemSynonyms` 와 키의 **형식** 의 `Array`:

    [![](implementing-sirikit-images/plist09.png "형식 배열 VocabularyItemSynonyms 키 추가")](implementing-sirikit-images/plist09.png#lightbox)
11. 사용 하 여 새 키를 추가 **형식** 의 `Dictionary`:

    [![](implementing-sirikit-images/plist10.png "사전 종류를 사용 하 여 새 키를 추가 합니다.")](implementing-sirikit-images/plist10.png#lightbox)
12. 추가 `VocabularyItemPhrase` 와 키의 **형식** 의 `String` 응용 프로그램에서 정의 하는 용어 및:

    [![](implementing-sirikit-images/plist11.png "문자열 형식 및 응용 프로그램을 정의 하는 용어와 VocabularyItemPhrase 키 추가")](implementing-sirikit-images/plist11.png#lightbox)
13. 추가 `VocabularyItemPronunciation` 와 키의 **형식** 의 `String` 및 용어의 음성 발음:

    [![](implementing-sirikit-images/plist12.png "문자열 형식 및 해당 기간의 음성 발음 VocabularyItemPronunciation 키 추가")](implementing-sirikit-images/plist12.png#lightbox)
14. 추가 `VocabularyItemExamples` 와 키의 **형식** 의 `Array`:

    [![](implementing-sirikit-images/plist13.png "형식 배열 VocabularyItemExamples 키 추가")](implementing-sirikit-images/plist13.png#lightbox)
15. 몇 가지 추가 `String` 용어의 사용 하 여 예제를 사용 하 여 키:

    [![](implementing-sirikit-images/plist14.png "용어에 대 한 예에서는 사용 하는 몇 가지 문자열 키 추가")](implementing-sirikit-images/plist14.png#lightbox)
16. 응용 프로그램을 정의 해야 합니다. 다른 사용자 지정 조건에 대 한 위의 단계를 반복 합니다.
17. 축소 된 `ParameterVocabularies` 키입니다.
18. 추가 `IntentPhrases` 와 키의 **형식** 의 `Array`:

    [![](implementing-sirikit-images/plist15.png "형식 배열 IntentPhrases 키 추가")](implementing-sirikit-images/plist15.png#lightbox)
19. 사용 하 여 새 키를 추가 **형식** 의 `Dictionary`:

    [![](implementing-sirikit-images/plist16.png "사전 종류를 사용 하 여 새 키를 추가 합니다.")](implementing-sirikit-images/plist16.png#lightbox)
20. 추가 `IntentName` 와 키의 **형식** 의 `String` 예제에 대 한 의도 및:

    [![](implementing-sirikit-images/plist17.png "이 예제에서는 문자열 형식 및 의도 IntentName 키 추가")](implementing-sirikit-images/plist17.png#lightbox)
21. 추가 `IntentExamples` 와 키의 **형식** 의 `Array`:

    [![](implementing-sirikit-images/plist18.png "형식 배열 IntentExamples 키 추가")](implementing-sirikit-images/plist18.png#lightbox)
22. 몇 가지 추가 `String` 용어의 사용 하 여 예제를 사용 하 여 키:

    [![](implementing-sirikit-images/plist19.png "용어에 대 한 예에서는 사용 하는 몇 가지 문자열 키 추가")](implementing-sirikit-images/plist19.png#lightbox)
23. 응용 프로그램의 사용 예를 제공 해야 할 모든 의도 대 한 위의 단계를 반복 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 프로젝트 이름을 마우스 오른쪽 단추로 클릭는 **솔루션 탐색기** 선택 **추가** > **새 파일...**   >  **iOS**:

    [![](implementing-sirikit-images/plist01w.png "새 Info.plist 추가")](implementing-sirikit-images/plist01w.png#lightbox) 
2. 두 번 클릭는 `AppIntentVocabulary.plist` 파일에 **솔루션 탐색기** 를 편집 하기 위해 엽니다.
3. 클릭는  **+**  설정 키를 추가 하는 **이름** 를 `ParameterVocabularies` 및 **형식** 를 `Array`:

    [![](implementing-sirikit-images/plist02w.png "이름을 ParameterVocabularies 및 배열 형식으로 설정")](implementing-sirikit-images/plist02w.png#lightbox)
4. 확장 `ParameterVocabularies` 클릭는  **+**  단추 및 설정의 **형식** 를 `Dictionary`:

    [![](implementing-sirikit-images/plist03w.png "사전에 형식을 설정합니다")](implementing-sirikit-images/plist03w.png#lightbox)
5. 클릭는  **+**  설정 하 여 새 키를 추가 하려면는 **이름** 를 `ParameterNames` 및 **형식** 를 `Array`:

    [![](implementing-sirikit-images/plist04w.png "이름을 ParameterNames 및 배열 형식으로 설정")](implementing-sirikit-images/plist04w.png#lightbox)
6. 클릭는  **+**  된 새 키를 추가 하는 **형식** 의 `String` 및 사용 가능한 매개 변수 이름 중 하나로 값입니다. 예를 들어 `INStartWorkoutIntent.workoutName`:

    [![](implementing-sirikit-images/plist05w.png "INStartWorkoutIntent.workoutName 키")](implementing-sirikit-images/plist05w.png#lightbox)
7. 추가 `ParameterVocabulary` 키를 `ParameterVocabularies` 와 키의 **형식** 의 `Array`:

    [![](implementing-sirikit-images/plist06w.png "형식 배열 ParameterVocabularies 키에 ParameterVocabulary 키 추가")](implementing-sirikit-images/plist06w.png#lightbox)
8. 사용 하 여 새 키를 추가 **형식** 의 `Dictionary`:

    [![](implementing-sirikit-images/plist07w.png "사전 종류를 사용 하 여 새 키를 추가 합니다.")](implementing-sirikit-images/plist07w.png#lightbox)
9. 추가 `VocabularyItemIdentifier` 인 키는 **형식** 의 `String` 용어에 대 한 고유 ID를 지정 합니다.

    [![](implementing-sirikit-images/plist08w.png "문자열 형식을 사용 하 여 VocabularyItemIdentifier 키를 추가 하 고 용어에 대 한 고유 ID를 지정 합니다.")](implementing-sirikit-images/plist08w.png#lightbox)
10. 추가 `VocabularyItemSynonyms` 와 키의 **형식** 의 `Array`:

    [![](implementing-sirikit-images/plist09w.png "형식 배열 VocabularyItemSynonyms 키 추가")](implementing-sirikit-images/plist09w.png#lightbox)
11. 사용 하 여 새 키를 추가 **형식** 의 `Dictionary`:

    [![](implementing-sirikit-images/plist10w.png "사전 종류를 사용 하 여 새 키를 추가 합니다.")](implementing-sirikit-images/plist10w.png#lightbox)
12. 추가 `VocabularyItemPhrase` 와 키의 **형식** 의 `String` 응용 프로그램에서 정의 하는 용어 및:

    [![](implementing-sirikit-images/plist11w.png "문자열 형식 및 응용 프로그램을 정의 하는 용어와 VocabularyItemPhrase 키 추가")](implementing-sirikit-images/plist11w.png#lightbox)
13. 추가 `VocabularyItemPronunciation` 와 키의 **형식** 의 `String` 및 용어의 음성 발음:

    [![](implementing-sirikit-images/plist12w.png "문자열 형식 및 해당 기간의 음성 발음 VocabularyItemPronunciation 키 추가")](implementing-sirikit-images/plist12w.png#lightbox)
14. 추가 `VocabularyItemExamples` 와 키의 **형식** 의 `Array`:

    [![](implementing-sirikit-images/plist13w.png "형식 배열 VocabularyItemExamples 키 추가")](implementing-sirikit-images/plist13w.png#lightbox)
15. 몇 가지 추가 `String` 용어의 사용 하 여 예제를 사용 하 여 키:

    [![](implementing-sirikit-images/plist14w.png "용어에 대 한 예에서는 사용 하는 몇 가지 문자열 키 추가")](implementing-sirikit-images/plist14w.png#lightbox)
16. 응용 프로그램을 정의 해야 합니다. 다른 사용자 지정 조건에 대 한 위의 단계를 반복 합니다.
17. 축소 된 `ParameterVocabularies` 키입니다.
18. 추가 `IntentPhrases` 와 키의 **형식** 의 `Array`:

    [![](implementing-sirikit-images/plist15w.png "형식 배열 IntentPhrases 키 추가")](implementing-sirikit-images/plist15w.png#lightbox)
19. 사용 하 여 새 키를 추가 **형식** 의 `Dictionary`:

    [![](implementing-sirikit-images/plist16w.png "사전 종류를 사용 하 여 새 키를 추가 합니다.")](implementing-sirikit-images/plist16w.png#lightbox)
20. 추가 `IntentName` 와 키의 **형식** 의 `String` 예제에 대 한 의도 및:

    [![](implementing-sirikit-images/plist17w.png "이 예제에서는 문자열 형식 및 의도 IntentName 키 추가")](implementing-sirikit-images/plist17w.png#lightbox)
21. 추가 `IntentExamples` 와 키의 **형식** 의 `Array`:

    [![](implementing-sirikit-images/plist18w.png "형식 배열 IntentExamples 키 추가")](implementing-sirikit-images/plist18w.png#lightbox)
22. 몇 가지 추가 `String` 용어의 사용 하 여 예제를 사용 하 여 키:

    [![](implementing-sirikit-images/plist19w.png "용어에 대 한 예에서는 사용 하는 몇 가지 문자열 키 추가")](implementing-sirikit-images/plist19w.png#lightbox)
23. 응용 프로그램의 사용 예를 제공 해야 할 모든 의도 대 한 위의 단계를 반복 합니다.

-----

> [!IMPORTANT]
> `AppIntentVocabulary.plist` 등록 될 Siri로 테스트 된 개발 도중 장치에 대 한 사용자 지정 어휘를 통합 하는 Siri 다소 시간이 걸릴 수도 있습니다. 결과적으로,은 테스터에 업데이트 된 응용 프로그램 특정 어휘를 테스트 하기 전에 몇 분 정도 기다려야 할 수 있습니다.

자세한 내용은 참조 하십시오 우리의 [응용 프로그램 특정 어휘 참조](~/ios/platform/sirikit/understanding-sirikit.md) 과 Apple [사용자 지정 어휘 참조를 지정 하](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1)합니다.

## <a name="adding-an-intents-extension"></a>의도 확장 추가

응용 프로그램 준비 SiriKit 채택 해야 했으므로 개발자 Siri의 통합에 필요한 의도 처리 하기 위해 솔루션에 하나 이상의 의도 확장을 추가 해야 합니다.

필요한 각 의도 확장명에 대해 다음을 수행 합니다.

- Xamarin.iOS 앱 솔루션에 의도 확장 프로젝트를 추가 합니다.
- 의도 확장 구성 `Info.plist` 파일입니다.
- 의도 확장 기본 클래스를 수정 합니다.

자세한 내용은 참조 하십시오 우리의 [의도 확장 Reference](~/ios/platform/sirikit/understanding-sirikit.md) 과 Apple [의도 확장 참조를 만드는](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CreatingtheIntentsExtension.html#//apple_ref/doc/uid/TP40016875-CH4-SW1)합니다.

### <a name="creating-the-extension"></a>확장명 만들기

솔루션에는 의도 확장을 추가 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 마우스 오른쪽 단추로 클릭는 **솔루션 이름** 에 **솔루션 패드** 선택 **추가** > **새 프로젝트 추가...** .
2. 대화 상자에서 선택 **iOS** > **확장** > **의도 확장** 클릭는 **다음** button: 

    [![](implementing-sirikit-images/intents05.png "의도 확장 선택")](implementing-sirikit-images/intents05.png#lightbox)
3. 그 다음 입력 한 **이름** 의도 확장과 클릭에 대 한는 **다음** 단추: 

    [![](implementing-sirikit-images/intents06.png "의도 확장에 대 한 이름을 입력 합니다.")](implementing-sirikit-images/intents06.png#lightbox)
4. 마지막으로, 클릭는 **만들기** 의도 확장 응용 프로그램 솔루션을 추가 하는 단추: 

    [![](implementing-sirikit-images/intents07.png "앱 솔루션에 의도 확장 추가")](implementing-sirikit-images/intents07.png#lightbox)
5. 에 **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭는 **참조** 새로 만든된 의도 확장 폴더입니다. (즉 응용 프로그램은 위에서 만든) 공용 공유 코드 라이브러리 프로젝트의 이름을 확인 하 고 클릭 하 고 **확인** 단추: 

    [![](implementing-sirikit-images/intents08.png "공용 공유 코드 라이브러리 프로젝트의 이름을 선택 합니다.")](implementing-sirikit-images/intents08.png#lightbox)
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 마우스 오른쪽 단추로 클릭는 **솔루션 이름** 에 **솔루션 탐색기** 선택 **추가** > **새 프로젝트 추가...** .
2. 대화 상자에서 선택 **iOS** > **확장** > **의도 확장** 클릭는 **다음** button: 

    [![](implementing-sirikit-images/intents05w.png "의도 확장 선택")](implementing-sirikit-images/intents05w.png#lightbox)
3. 그 다음 입력 한 **이름** 의도 확장과 클릭에 대 한는 **확인** 단추입니다.
5. 에 **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭는 **참조** 새로 만든된 의도 확장 폴더입니다. (즉 응용 프로그램은 위에서 만든) 공용 공유 코드 라이브러리 프로젝트의 이름을 확인 하 고 클릭 하 고 **확인** 단추: 

    [![](implementing-sirikit-images/intents08w.png "공용 공유 코드 라이브러리 프로젝트의 이름을 선택 합니다.")](implementing-sirikit-images/intents08w.png#lightbox)
    
-----

의도 확장명의 수에 대 한 이러한 단계를 반복 (기반 [확장에 대 한 응용 프로그램을 설계](#Architecting-the-App-for-Extensions) 위의 섹션) 응용 프로그램 해야 하는 합니다.

### <a name="configuring-the-infoplist"></a>Info.plist 구성

각 응용 프로그램의 솔루션에 추가 하는 의도 확장에 대해 구성 해야는 `Info.plist` 응용 프로그램을 사용 하는 파일입니다.

모든 일반적인 응용 프로그램 확장와 마찬가지로 응용 프로그램의 기존 키 갖습니다 `NSExtension` 및 `NSExtensionAttributes`합니다. 의도 확장 구성 해야 하는 두 개의 새로운 특성을 가지 있습니다.

[![](implementing-sirikit-images/intents01.png "구성 해야 하는 두 개의 새로운 특성")](implementing-sirikit-images/intents01.png#lightbox)

- **IntentsSupported** -필요 하 고 응용 프로그램에서 의도 확장 프로그램에서 지원 하려는 의도 클래스 이름의 배열로 구성 됩니다.
- **IntentsRestrictedWhileLocked** -응용 프로그램 확장의 잠금 화면 동작을 지정 하는 데는 선택적 키입니다. 사용자가 의도 확장에서 사용 하려면 로그인 해야 하는 응용 프로그램 의도 클래스 이름의 배열을 구성 됩니다.

의도 확장을 구성 하려면 `Info.plist` 파일,이 창에서 원하는 **솔루션 탐색기** 를 편집 하기 위해 엽니다. 다음으로 전환 하는 **소스** 확인 한 후 확장의 `NSExtension` 및 `NSExtensionAttributes` 편집기에 있는 키:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents02.png "편집기에서 NSExtension 및 NSExtensionAttributes 키")](implementing-sirikit-images/intents02.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents02w.png "편집기에서 NSExtension 및 NSExtensionAttributes 키")](implementing-sirikit-images/intents02w.png#lightbox)

-----

확장 된 `IntentsSupported` 키와이 확장 지원할 의도 클래스의 이름을 추가 합니다. 예제 MonkeyChat 앱에 대 한 지원의 `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents09.png "INSendMessageIntent 키")](implementing-sirikit-images/intents09.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents09w.png "INSendMessageIntent 키")](implementing-sirikit-images/intents09w.png#lightbox)

-----

사용자 지정된 의도 사용 하 여를 확장 하 고 장치에 로그온 하는 응용 프로그램 필요에 따라 요구 하는 경우는 `IntentRestrictedWhileLocked` 키를 액세스가 제한 된 의도의 클래스 이름을 추가 합니다. 예제 MonkeyChat 앱에 대 한 사용자 로그인 해야 하므로 추가 했습니다 채팅 메시지를 보내려고 `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents10.png "추가 된 INSendMessageIntent 키")](implementing-sirikit-images/intents10.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents10w.png "추가 된 INSendMessageIntent 키")](implementing-sirikit-images/intents10w.png#lightbox)

-----


사용 가능한 의도 도메인의 전체 목록은, Apple의를 참조 하십시오 [의도 도메인 참조](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)합니다.

### <a name="configuring-the-main-class"></a>기본 클래스를 구성합니다.

다음으로, 개발자 Siri에 의도 확장에 대 한 주 진입점 역할을 하는 기본 클래스를 구성 해야 합니다. 서브 클래스 여야 `INExtension` 를 준수 하는 `IINIntentHandler` 위임 합니다. 예를 들어:

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

응용 프로그램 의도 확장 주 클래스에 구현 해야 하는 하나의 벡 메서드가 없는 `GetHandler` 메서드. 이 메서드는 의도 SiriKit 전달 되 고 응용 프로그램 반환 해야 합니다는 **의도 처리기** 주어진된 의도의 형식과 일치 하는 합니다.

이후 예제 MonkeyChat 응용 프로그램 의도 한 처리를 반환 하는 자체는 `GetHandler` 메서드. 개발자 의도 각 형식에 대 한 클래스를 추가 하 고 여기에 따라 인스턴스를 반환할 확장 둘 이상의 의도 처리 하는 경우는 `Intent` 메서드에 전달 합니다.

### <a name="handling-the-resolve-stage"></a>해결 단계를 처리합니다.

해결 단계 의도 확장은 분명히 설명 하 고 매개 변수 유효성 검사에 전달 Siri에서 하 고 사용자의 대화를 통해 설정 되었습니다.

Siri에서 전송 된 각 매개 변수에 대해는 `Resolve` 메서드. 응용 프로그램은 응용 프로그램 사용자에서 올바른 답을 얻을 수 Siri의 도움말을 할 수 있는 모든 매개 변수에 대해이 메서드를 구현 해야 합니다.

예제 MonkeyChat 앱의 경우 의도 확장에는 하나 이상의 받는 사람 메시지를 보내는 데 필요 합니다. 목록에서 각 받는 사람에 대 한 확장 다음 결과 가질 수 있는 연락처를 검색 해야 합니다.

- 정확히 하나의 일치 하는 연락처를 찾을 수 있습니다.
- 두 개 이상의 일치 하는 연락처가 검색 됩니다.
- 일치 하는 연락처가 검색 됩니다.

또한 MonkeyChat 메시지의 본문에 대 한 콘텐츠가 필요합니다. 사용자가을 지정 하지 않으면이 Siri 콘텐츠에 대 한 사용자 메시지를 표시 해야 합니다.

의도 확장은 각각의이 경우를 정상적으로 처리 해야 합니다.


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

확인 단계를 사용 하는 의도 확장 모두 사용자의 요청을 충족 하는 정보를 확인 하는 위치는입니다. 응용 프로그램의 모든 지원 정보는에 대 한 발생할 Siri를 확인할 수는 사용자와 작업은이 따라 확인 됩니다를 보내려고 합니다.

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

자세한 내용은 참조 하십시오 우리의 [확인 단계 참조](~/ios/platform/sirikit/understanding-sirikit.md)합니다.

### <a name="processing-the-intent"></a>의도 처리합니다.

이 실제로 수행 작업을 사용자의 요청을 충족 하 고 사용자를 알릴 수 있도록 Siri에 다시 결과 전달 하는 의도 확장 지점입니다.


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

## <a name="adding-an-intents-ui-extension"></a>의도 UI 확장 추가

선택적 의도 UI 확장 응용 프로그램의 UI 및 Siri 환경에 브랜딩 영업 기회 수를 표시 하 고 생각 될 앱에 연결 된 사용자가 확인 합니다. 이 확장으로 응용 프로그램에는 브랜드 정보 뿐 아니라 시각 및 기타 대 본에 가져올 수 있습니다.

[![](implementing-sirikit-images/intentsui01.png "의도 UI 확장 출력 예")](implementing-sirikit-images/intentsui01.png#lightbox)

의도 확장와 마찬가지로 개발자가 의도 UI 확장에 대 한 다음 단계를 수행 합니다.

- Xamarin.iOS 앱 솔루션에 의도 UI 확장 프로젝트를 추가 합니다.
- 의도 UI 확장 구성 `Info.plist` 파일입니다.
- 의도 UI 확장 기본 클래스를 수정 합니다.

자세한 내용은 참조 하십시오 우리의 [의도 UI 확장 Reference](~/ios/platform/sirikit/understanding-sirikit.md) 과 Apple [사용자 지정 인터페이스 참조를 제공 하](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ProvidingaCustomInterface.html#//apple_ref/doc/uid/TP40016875-CH7-SW1)합니다.

### <a name="creating-the-extension"></a>확장명 만들기

의도 UI 확장 솔루션을 추가 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 마우스 오른쪽 단추로 클릭는 **솔루션 이름** 에 **솔루션 패드** 선택 **추가** > **새 프로젝트 추가...** .
2. 대화 상자에서 선택 **iOS** > **확장** > **의도 UI 확장** 클릭는 **다음** button: 

    [![](implementing-sirikit-images/intents11.png "의도 UI 확장 선택")](implementing-sirikit-images/intents11.png#lightbox)
3. 그 다음 입력 한 **이름** 의도 확장과 클릭에 대 한는 **다음** 단추: 

    [![](implementing-sirikit-images/intents12.png "의도 확장에 대 한 이름을 입력 합니다.")](implementing-sirikit-images/intents12.png#lightbox)
4. 마지막으로, 클릭는 **만들기** 의도 확장 응용 프로그램 솔루션을 추가 하는 단추: 

    [![](implementing-sirikit-images/intents13.png "앱 솔루션에 의도 확장 추가")](implementing-sirikit-images/intents13.png#lightbox)
5. 에 **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭는 **참조** 새로 만든된 의도 확장 폴더입니다. (즉 응용 프로그램은 위에서 만든) 공용 공유 코드 라이브러리 프로젝트의 이름을 확인 하 고 클릭 하 고 **확인** 단추: 

    [![](implementing-sirikit-images/intents14.png "공용 공유 코드 라이브러리 프로젝트의 이름을 선택 합니다.")](implementing-sirikit-images/intents14.png#lightbox)
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 마우스 오른쪽 단추로 클릭는 **솔루션 이름** 에 **솔루션 탐색기** 선택 **추가** > **새 프로젝트 추가...**
2. 대화 상자에서 선택 **iOS** > **확장** > **의도 UI 확장** 클릭는 **다음** 단추입니다.
3. 그 다음 입력 한 **이름** 의도 확장과 클릭에 대 한는 **확인** 단추입니다.
5. 에 **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭는 **참조** 새로 만든된 의도 확장 폴더입니다. (즉 응용 프로그램은 위에서 만든) 공용 공유 코드 라이브러리 프로젝트의 이름을 확인 하 고 클릭는 **확인** 단추입니다.
    
-----

### <a name="configuring-the-infoplist"></a>Info.plist 구성

의도 UI 확장 구성 `Info.plist` 응용 프로그램을 사용 하는 파일입니다.

모든 일반적인 응용 프로그램 확장와 마찬가지로 응용 프로그램의 기존 키 갖습니다 `NSExtension` 및 `NSExtensionAttributes`합니다. 의도 확장에 대 한 새 구성 해야 하는 특성이 하나 있습니다.

[![](implementing-sirikit-images/intents03.png "구성 해야 하는 하나의 새 특성")](implementing-sirikit-images/intents03.png#lightbox)

**IntentsSupported** 필수 항목이 며 의도 확장 프로그램에서 지원 하 고 응용 프로그램 의도 클래스 이름의 배열로 구성 됩니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

의도 UI 확장을 구성 하려면 `Info.plist` 파일,이 창에서 원하는 **솔루션 탐색기** 를 편집 하기 위해 엽니다. 다음으로 전환 하는 **소스** 확인 한 후 확장의 `NSExtension` 및 `NSExtensionAttributes` 편집기에 있는 키:

[![](implementing-sirikit-images/intents04.png "편집기에서 NSExtension 및 NSExtensionAttributes 키")](implementing-sirikit-images/intents04.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

의도 UI 확장을 구성 하려면 `Info.plist` 파일,이 창에서 원하는 **솔루션 탐색기** 를 편집 하기 위해 엽니다. 확장 된 `NSExtension` 및 `NSExtensionAttributes` 편집기에 있는 키:

[![](implementing-sirikit-images/intents04w.png "편집기에서 구성 제품 NSExtension 및 NSExtensionAttributes 키")](implementing-sirikit-images/intents04w.png#lightbox)

-----

확장 된 `IntentsSupported` 키와이 확장 지원할 의도 클래스의 이름을 추가 합니다. 예제 MonkeyChat 앱에 대 한 지원의 `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents15.png "INSendMessageIntent 키")](implementing-sirikit-images/intents15.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents15w.png "INSendMessageIntent 키")](implementing-sirikit-images/intents15w.png#lightbox)

-----

사용 가능한 의도 도메인의 전체 목록은, Apple의를 참조 하십시오 [의도 도메인 참조](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)합니다.

### <a name="configuring-the-main-class"></a>기본 클래스를 구성합니다.

Siri에 의도 UI 확장에 대 한 주 진입점 역할을 하는 기본 클래스를 구성 합니다. 서브 클래스 여야 `UIViewController` 를 준수 하는 `IINUIHostedViewController` 인터페이스입니다. 예를 들어:

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

Siri 전달 하는 `INInteraction` 클래스 인스턴스를는 `Configure` 의 메서드는 `UIViewController` 의도 UI 확장 내에서 인스턴스.

`INInteraction` 개체는 세 가지 주요 정보가 확장 프로그램에 대 한 정보를 제공 합니다.

1. 처리 중인 의도 하는 개체입니다.
2. 의도 Response 개체는 `Confirm` 및 `Handle` 의도 확장의 메서드.
3. 앱과 Siri 간의 상호 작용의 상태를 정의 하는 상호 작용 상태입니다.

`UIViewController` 인스턴스가 Siri와의 상호 작용에 대 한 원칙 클래스에서 상속 하 고 `UIViewController`, 모든 UIKit 기능에 액세스할 수 있습니다.

Siri 호출 하는 경우는 `Configure` 의 메서드는 `UIViewController` 뷰 컨트롤러는 하거나 Siri Snippit 또는 지도 카드에서 호스팅할 수 없다는 뷰 컨텍스트에 전달 합니다.

Siri 구성 되는 응용 프로그램 보기의 원하는 크기를 반환 하는 응용 프로그램 완료 처리기도 전달 합니다.

### <a name="design-the-ui-in-ios-designer"></a>IOS 디자이너에서에서 UI 디자인

레이아웃 iOS 디자이너에서에서 의도 UI 확장의 사용자 인터페이스입니다. 확장의 두 번 클릭 `MainInterface.storyboard` 파일에 **솔루션 탐색기** 를 편집 하기 위해 엽니다. 모든 사용자 인터페이스를 빌드하고 변경 내용을 저장 하는 필수 UI 요소를 끕니다.

> [!IMPORTANT]
> 대화형 요소를 추가할 수 있지만 `UIButtons` 또는 `UITextFields` 의도 UI 확장 프로그램에 `UIViewController`, 이러한 엄격 하 게로 사용할 수 없습니다 의도 UI에 비 대화형 및 사용자 상호 작용할 수 없습니다.

### <a name="wire-up-the-user-interface"></a>유선 전화 접속 사용자 인터페이스

IOS 디자이너에서에서 만든 의도 UI 확장의 사용자 인터페이스와 편집은 `UIViewController` 하위 클래스 및 재정의 `Configure` 메서드를 다음과 같이 합니다.

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

의도 UI 확장은 항상 수는 UI의 맨 위쪽에 이름 및 응용 프로그램 아이콘 같은 다른 Siri 내용과 함께 표시 또는 의도에 따라, (예: 송신 또는 취소) 단추 아래에 표시 될 수 있습니다.

Siri 메시징 같이 기본적으로 사용자에 게 표시 되는 정보 또는 응용 프로그램 응용 프로그램에 맞게 하나가 지정 된 기본 환경을 바꿀 수 지도 앱 바꿀 수 있는 몇 가지 인스턴스가 있습니다.

의도 UI 확장 기본 Siri UI의 요소를 재정의 하는 경우는 `UIViewController` 하위 클래스를 구현 해야 합니다는 `IINUIHostedViewSiriProviding` 인터페이스와에 옵트인 특정 인터페이스 요소를 표시 합니다.

다음 코드를 추가 하는 `UIViewController` 하기가 Siri 의도 UI 확장 메시지 내용의 표시 하 고 이미 서브 클래스:

```csharp
public bool DisplaysMessage {
    get {return true;}
}
```

### <a name="considerations"></a>고려 사항

Apple는 개발자는 다음 사항을 디자인 하 고 의도 UI 확장을 구현 하는 경우 고려해를 취하는 것을 제안:

- **메모리 사용량의 인식 될** -때문에 의도 UI 확장은 임시 또는 시스템 전체 앱과 함께 사용 하는 것 보다 더 엄격한 메모리 제약 조건 적용만 짧은 시간에 대 한 표시 합니다.
- **최소 및 최대 보기 크기 고려** -의도 UI 확장 프로그램에서 모든 iOS 장치 유형, 크기 및 방향을 제대로 표시 되는지 확인 합니다. 또한 앱 Siri로 다시 보내는 원하는 크기로 부여할 못할 수 있습니다.
- **유동, 적응 레이아웃 패턴을 사용 하 여** -다시 UI는 모든 장치에서 훌륭한 필드가 표시 되도록 합니다.

## <a name="summary"></a>요약

이 문서는 SiriKit 대상 않았으며 iOS 장치에서 Siri와 지도 응용 프로그램을 사용 하는 사용자에 액세스할 수 있는 서비스를 제공 하도록 Xamarin.iOS 앱에 추가할 수 있습니다 어떻게 표시 됩니다.




## <a name="related-links"></a>관련 링크

- [ElizaChat 샘플](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [SiriKit 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [의도 프레임 워크 참조](https://developer.apple.com/reference/intents)
- [의도 UI 프레임 워크 참조](https://developer.apple.com/reference/intentsui)
- [의도 도메인 참조](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)
