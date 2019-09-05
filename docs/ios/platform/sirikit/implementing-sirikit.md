---
title: Xamarin.ios에서 SiriKit 구현
description: 이 문서에서는 Xamarin.ios 앱에서 SiriKit 지원을 구현 하는 데 필요한 단계에 대해 설명 합니다. 의도 확장 및 의도 UI 확장에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 20FFB981-EB10-48BA-BF79-40F37F0291EB
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/03/2018
ms.openlocfilehash: 5c891943d0d23c24169a6d226a10f83964c9257a
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290646"
---
# <a name="implementing-sirikit-in-xamarinios"></a>Xamarin.ios에서 SiriKit 구현

_이 문서에서는 Xamarin.ios 앱에서 SiriKit 지원을 구현 하는 데 필요한 단계를 설명 합니다._

IOS 10의 새로운 기능으로 SiriKit를 사용 하면 Xamarin.ios 앱에서 Siri를 사용 하는 사용자에 게 액세스 가능한 서비스와 iOS 장치에서 맵 앱을 제공할 수 있습니다. 이 문서에서는 필수 항목 확장, 의도 UI 확장 및 어휘를 추가 하 여 Xamarin.ios 앱에서 SiriKit 지원을 구현 하는 데 필요한 단계를 설명 합니다.

Siri는 **도메인**개념과 관련 태스크에 대 한 알고 있는 작업 그룹을 사용 하 여 작동 합니다. 앱에서 Siri를 사용 하는 각 상호 작용은 다음과 같이 알려진 서비스 도메인 중 하나에 속해야 합니다.

- 오디오나 비디오를 호출 합니다.
- 을 예약 합니다.
- 워크를 관리 합니다.
- 전송.
- 사진을 검색 하 고 있습니다.
- 지불 보내기 또는 받기.

사용자가 앱 확장의 서비스 중 하나를 포함 하는 Siri를 요청 하면 SiriKit에서 해당 확장을 지원 데이터와 함께 사용자의 요청을 설명 하는 **의도** 개체에 보냅니다. 그러면 앱 확장이 지정 된 **의도**에 대 한 적절 한 **응답** 개체를 생성 하 여 확장에서 요청을 처리할 수 있는 방법을 자세히 설명 합니다.

이 가이드에서는 기존 앱에 SiriKit 지원을 포함 하는 간단한 예제를 제공 합니다. 이 예제에서는 다음과 같이 가짜 MonkeyChat 앱을 사용 합니다.

[![](implementing-sirikit-images/monkeychat01.png "MonkeyChat 아이콘")](implementing-sirikit-images/monkeychat01.png#lightbox)

MonkeyChat는 각각 화면 이름 (예: Bobo)과 연결 된 사용자의 친구에 대 한 고유한 연락처 설명서를 유지 하 고 사용자가 화면 이름으로 각 친구에 게 텍스트 채팅을 보낼 수 있도록 합니다.

## <a name="extending-the-app-with-sirikit"></a>SiriKit를 사용 하 여 앱 확장

[Sirikit 개념 이해](~/ios/platform/sirikit/understanding-sirikit.md) 가이드에 표시 된 것 처럼 sirikit를 사용 하 여 앱을 확장 하는 데는 세 가지 주요 부분이 있습니다.

[![](implementing-sirikit-images/elements01.png "SiriKit 다이어그램으로 앱 확장")](implementing-sirikit-images/elements01.png#lightbox)

이러한 개체는 다음과 같습니다.

1. 인 텐트 **확장** -사용자 응답을 확인 하 고, 앱에서 요청을 처리할 수 있는지 확인 하 고 실제로 사용자의 요청을 처리 하는 작업을 수행 합니다.
2. **인 텐트 ui 확장** - *옵션인*siri 환경의 응답에 사용자 지정 ui를 제공 하 고, 앱 UI 및 브랜딩을 siri로 가져와서 사용자 환경을 보강 할 수 있습니다.
3. **앱** -siri를 사용 하 여 작업 하는 데 사용할 수 있는 사용자별 어휘를 앱에 제공 합니다. 

이러한 모든 요소 및 이러한 요소를 앱에 포함 하는 단계는 아래 섹션에서 자세히 설명 합니다.


## <a name="preparing-the-app"></a>앱 준비

SiriKit는 확장을 기반으로 하지만 앱에 확장을 추가 하기 전에 개발자가 SiriKit를 채택 하는 데 도움이 되는 몇 가지 사항이 있습니다.

### <a name="moving-common-shared-code"></a>공통 공유 코드 이동 

먼저 개발자는 앱과 확장 간에 공유 되는 공용 코드 중 일부를 _공유 프로젝트_, _pcls (이식 가능한 클래스 라이브러리)_ 또는 _네이티브 라이브러리로_이동할 수 있습니다.

확장이 앱에서 수행 하는 모든 작업을 수행할 수 있어야 합니다. 샘플 MonkeyChat 앱의 용어로 연락처 찾기, 새 연락처 추가, 메시지 전송 및 메시지 기록 검색 등의 작업을 수행 합니다.

이 공통 코드를 공유 프로젝트인 PCL 또는 네이티브 라이브러리로 이동 하면 해당 코드를 하나의 공통 된 장소에서 쉽게 유지 관리 하 고 확장 및 부모 앱이 사용자에 게 일관 된 경험과 기능을 제공할 수 있습니다.

예제 앱 MonkeyChat의 경우 네트워크 및 데이터베이스 액세스와 같은 데이터 모델 및 처리 코드가 네이티브 라이브러리로 이동 됩니다.

다음을 수행합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac용 Visual Studio를 시작 하 고 MonkeyChat 앱을 엽니다.
2. **Solution Pad** 에서 솔루션 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가** > **새 프로젝트 ...** 를 선택 합니다. 

    [![](implementing-sirikit-images/prep01.png "새 프로젝트 추가")](implementing-sirikit-images/prep01.png#lightbox)
3. **IOS** > **라이브러리** 클래스 라이브러리를 선택 하 고 다음 단추를 클릭 합니다. >  

    [![](implementing-sirikit-images/prep02.png "클래스 라이브러리 선택")](implementing-sirikit-images/prep02.png#lightbox)
4. 이름 `MonkeyChatCommon` 으로를 입력 하 고 **만들기** 단추를 클릭 합니다. 

    [![](implementing-sirikit-images/prep03.png "이름에 MonkeyChatCommon을 입력 합니다.")](implementing-sirikit-images/prep03.png#lightbox)
5. **솔루션 탐색기** 에서 주 앱의 **참조** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **참조 편집**...을 선택 합니다. **Monkeychatcommon** 프로젝트를 선택 하 고 **확인** 단추를 클릭 합니다. 

    [![](implementing-sirikit-images/prep05.png "MonkeyChatCommon 프로젝트를 확인 합니다.")](implementing-sirikit-images/prep05.png#lightbox)
6. **솔루션 탐색기**에서 일반 공유 코드를 주 앱에서 네이티브 라이브러리로 끌어 옵니다.
7. MonkeyChat의 경우 주 앱에서 기본 라이브러리로 **DataModels** 및 **프로세서** 폴더를 끌어 옵니다. 

    [![](implementing-sirikit-images/prep06.png "솔루션 탐색기의 DataModels 및 processor 폴더")](implementing-sirikit-images/prep06.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio를 시작 하 고 MonkeyChat 앱을 엽니다.
2. **솔루션 탐색기** 에서 솔루션 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가** > **새 프로젝트**...를 선택 합니다.
3. **시각적 C#**  공유 프로젝트를 선택 하 고 다음 단추를 클릭 합니다.  >  

    [![](implementing-sirikit-images/prep02.w157-sml.png "클래스 라이브러리 선택")](implementing-sirikit-images/prep02.w157.png#lightbox)
4. 이름 `MonkeyChatCommon` 으로를 입력 하 고 **만들기** 단추를 클릭 합니다.
5. **솔루션 탐색기** 에서 주 앱의 **참조** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **참조 편집**...을 선택 합니다. **Monkeychatcommon** 프로젝트를 선택 하 고 **확인** 단추를 클릭 합니다. 

    [![](implementing-sirikit-images/prep05w.png "MonkeyChatCommon 프로젝트를 확인 합니다.")](implementing-sirikit-images/prep05w.png#lightbox)
6. **솔루션 탐색기**에서 일반 공유 코드를 주 앱에서 공유 프로젝트로 끌어 옵니다.
7. MonkeyChat의 경우 주 앱에서 **DataModels** **및 Processor** 폴더를 네이티브 라이브러리로 끌어 옵니다.

-----

네이티브 라이브러리로 이동한 파일을 편집 하 고 라이브러리의 이름과 일치 하도록 네임 스페이스를 변경 합니다. 예를 들어 `MonkeyChatCommon`다음과 `MonkeyChat` 같이 변경 합니다.

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

다음으로, 기본 앱으로 돌아가서 앱이 이동 된 `using` 클래스 중 하나를 사용 하는 모든 위치에서 네이티브 라이브러리의 네임 스페이스에 대 한 문을 추가 합니다.

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

### <a name="architecting-the-app-for-extensions"></a>확장을 위한 응용 프로그램 설계

일반적으로 앱은 여러 의도를 등록 하 고 개발자는 앱이 적절 한 수의 의도 확장에 맞게 설계 되었는지 확인 해야 합니다.

앱이 둘 이상의 의도를 필요로 하는 상황에서 개발자는 모든 의도 처리를 하나의 의도 확장에 배치 하거나 각 의도에 대 한 별도의 의도 확장을 만들 수 있습니다.

각 의도에 대해 별도의 의도 확장을 만들도록 선택 하는 경우 개발자는 각 확장에서 많은 양의 상용구 코드를 복제 하 고 많은 양의 프로세서 및 메모리 오버 헤드를 만들 수 있습니다.

두 옵션 중 하나를 선택 하는 데 도움이 되도록 모든 의도를 자연스럽 게 함께 포함 하는 경우를 참조 하세요. 예를 들어, 오디오 및 비디오 호출을 수행 하는 앱은 유사한 작업을 처리 하 고 대부분의 코드 재사용을 제공할 수 있으므로 이러한 의도를 단일 의도 확장에 포함 하는 것이 좋습니다.

기존 그룹에 맞지 않는 의도 또는 의도 그룹의 경우 앱의 솔루션에 새 의도 확장을 만들어 해당 항목을 포함 합니다.


### <a name="setting-the-required-entitlements"></a>필요한 자격 설정

SiriKit 통합을 포함 하는 모든 Xamarin.ios 앱에는 올바른 자격 집합이 있어야 합니다. 개발자가 이러한 필수 자격을 올바르게 설정 하지 않으면 실제 iOS 10 이상의 하드웨어에서 앱을 설치 하거나 테스트할 수 없게 됩니다 .이는 iOS 10 시뮬레이터가 SiriKit를 지원 하지 않으므로 요구 사항 이기도 합니다.

다음을 수행합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. `Entitlements.plist` **솔루션 탐색기** 파일을 두 번 클릭 하 여 편집용으로 엽니다.
2. **원본** 탭으로 전환 합니다.
3. `Boolean` `Yes`속성을 추가 하 고, 형식을로 설정 하 고, 값을로 설정 합니다. `com.apple.developer.siri` 

    [![](implementing-sirikit-images/setup01.png ".Com 속성을 추가 합니다.")](implementing-sirikit-images/setup01.png#lightbox)
4. 파일의 변경 내용을 저장합니다.
5. **솔루션 탐색기** 에서 **프로젝트 파일** 을 두 번 클릭 하 여 편집용으로 엽니다.
6. **IOS 번들 서명** 을 선택 하 고 `Entitlements.plist` **사용자 지정 자격** 필드에서 파일이 선택 되었는지 확인 합니다. 

    [![](implementing-sirikit-images/setup02.png "사용자 지정 자격 필드에서 info.plist 파일을 선택 합니다.")](implementing-sirikit-images/setup02.png#lightbox)
7. **확인** 단추를 클릭하여 변경 내용을 저장합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. `Entitlements.plist` **솔루션 탐색기** 파일을 두 번 클릭 하 여 편집용으로 엽니다.
2. `Boolean` `Yes`속성을 추가 하 고, 형식을로 설정 하 고, 값을로 설정 합니다. `com.apple.developer.siri` 

    [![](implementing-sirikit-images/setup01w.png ".Com 속성을 추가 합니다.")](implementing-sirikit-images/setup01w.png#lightbox)
3. 파일의 변경 내용을 저장합니다.
4. **솔루션 탐색기** 에서 **프로젝트 파일** 을 두 번 클릭 하 여 편집용으로 엽니다.
5. **IOS 번들 서명** 을 선택 하 고 `Entitlements.plist` **사용자 지정 자격** 필드에서 파일이 선택 되었는지 확인 합니다.

-----

완료 되 면 앱의 `Entitlements.plist` 파일이 다음과 같이 표시 됩니다 (외부 편집기에서 열림).

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

### <a name="correctly-provisioning-the-app"></a>앱 올바르게 프로 비전

Apple에서 SiriKit 프레임 워크를 사용 하는 엄격한 보안으로 인해 SiriKit를 구현 하는 Xamarin.ios 앱은 올바른 앱 ID 및 자격을 갖고 _있어야_ 합니다 (위 섹션 참조). 적절 한 프로 비전 프로필을 사용 하 여 서명 해야 합니다.

Mac에서 다음을 수행 합니다.

1. 웹 브라우저에서로 [https://developer.apple.com](https://developer.apple.com) 이동 하 여 계정에 로그인 합니다.
2. **인증서**, **식별자** 및 **프로필**을 클릭 합니다.
3. **프로 비전 프로필** 을 선택 하 고 **앱 id**를 선택한 **+** 다음 단추를 클릭 합니다.
4. 새 프로필의 **이름을** 입력 합니다.
5. Apple의 명명 권장 사항에 따라 **번들 ID** 를 입력 합니다.
6. **App Services** 섹션으로 스크롤하고 **sirikit** 를 선택 하 고 **계속** 단추를 클릭 합니다. 

    [![](implementing-sirikit-images/setup03.png "SiriKit 선택")](implementing-sirikit-images/setup03.png#lightbox)
7. 모든 설정을 확인 한 다음 앱 ID를 **제출** 합니다.
8. 프로 **비전 프로필** > **개발**을 선택 하 **+** 고 단추를 클릭 하 고 **Apple ID**를 선택한 다음 **계속**을 클릭 합니다.
9. **모두**선택을 클릭 하 고 **계속**을 클릭 합니다.
10. **모두 선택** 을 다시 클릭 한 다음 **계속**을 클릭 합니다.
11. Apple의 명명 제안을 사용 하 여 **프로필 이름을** 입력 하 고 **계속**을 클릭 합니다.
12. Xcode를 시작합니다.
13. Xcode 메뉴에서 **기본 설정 ...** 을 선택 합니다.
14. **계정**을 선택 하 고 **자세히 보기** ...를 클릭 합니다. 단추만 

    [![](implementing-sirikit-images/setup04.png "계정 선택")](implementing-sirikit-images/setup04.png#lightbox)
15. 왼쪽 아래 모서리에 있는 **모든 프로필 다운로드** 단추를 클릭 합니다. 

    [![](implementing-sirikit-images/setup05.png "모든 프로필 다운로드")](implementing-sirikit-images/setup05.png#lightbox)
16. 위에서 만든 **프로 비전 프로필이** Xcode에 설치 되어 있는지 확인 합니다.
17. Mac용 Visual Studio에서 SiriKit 지원을 추가할 프로젝트를 엽니다.
18. `Info.plist` **솔루션 탐색기**파일을 두 번 클릭 합니다.
19. 위의 Apple 개발자 포털에서 만든 **번들 식별자** 와 일치 하는지 확인 합니다. 

    [![](implementing-sirikit-images/setup06.png "번들 식별자")](implementing-sirikit-images/setup06.png#lightbox)
20. **솔루션 탐색기**에서 **프로젝트**를 선택 합니다.
21. 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **옵션**을 선택 합니다.
22. **IOS 번들 서명**을 선택 하 고 위에서 만든 **서명 Id** 및 **프로 비전 프로필** 을 선택 합니다. 

    [![](implementing-sirikit-images/setup07.png "서명 Id 및 프로 비전 프로필을 선택 합니다.")](implementing-sirikit-images/setup07.png#lightbox)
23. **확인** 단추를 클릭하여 변경 내용을 저장합니다.

> [!IMPORTANT]
> SiriKit 테스트는 iOS 10 시뮬레이터가 아닌 실제 iOS 10 하드웨어 장치 에서만 작동 합니다. 실제 하드웨어에서 SiriKit를 사용 하도록 설정 된 Xamarin.ios 앱을 설치 하는 데 문제가 있는 경우 Apple의 개발자 포털 및 Mac용 Visual Studio 둘 다에서 필요한 자격, 앱 ID, 서명 식별자 및 프로비저닝 프로필을 올바르게 구성 해야 합니다.

### <a name="requesting-siri-authorization"></a>Siri 권한 부여 요청

앱에서 사용자 관련 어휘를 추가 하거나 의도 확장이 Siri에 연결 하기 전에 Siri에 액세스 하도록 사용자의 권한 부여를 요청 해야 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

앱의 `Info.plist` 파일을 편집 하 고, **원본** `NSSiriUsageDescription` 뷰로 전환 하 고, 앱에서 siri를 사용 하는 방법과 전송 될 데이터 유형을 설명 하는 문자열 값이 포함 된 키를 추가 합니다. 예를 들어 MonkeyChat 앱은 "MonkeyChat 연락처가 Siri로 전송 됨"을 나타낼 수 있습니다.

[![](implementing-sirikit-images/request01.png "Info.plist 편집기의 NSSiriUsageDescription")](implementing-sirikit-images/request01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

앱 `Info.plist` 의 파일을 편집 하 고 앱 `NSSiriUsageDescription` 에서 siri를 사용 하는 방법과 전송 될 데이터 유형을 설명 하는 문자열 값이 포함 된 키를 추가 합니다. 예를 들어 MonkeyChat 앱은 "MonkeyChat 연락처가 Siri로 전송 됨"을 나타낼 수 있습니다.

[![](implementing-sirikit-images/request01w.png "Info.plist 편집기의 NSSiriUsageDescription")](implementing-sirikit-images/request01w.png#lightbox)

-----

앱이 `RequestSiriAuthorization` 처음 시작 될 `INPreferences` 때 클래스의 메서드를 호출 합니다. 클래스를 `AppDelegate.cs` 편집 하 고 메서드 `FinishedLaunching` 를 다음과 같이 만듭니다.


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

이 메서드가 처음 호출 될 때 앱에서 Siri에 액세스할 수 있도록 허용 하 라는 경고가 표시 됩니다. `NSSiriUsageDescription` 위의 개발자가 추가한 메시지는이 경고에 표시 됩니다. 사용자가 처음으로 액세스를 거부 하는 경우 **설정** 앱을 사용 하 여 앱에 대 한 액세스 권한을 부여할 수 있습니다.

언제 든 지 앱은 `SiriAuthorizationStatus` `INPreferences` 클래스의 메서드를 호출 하 여 응용 프로그램에서 siri에 액세스할 수 있는지 확인할 수 있습니다.

### <a name="localization-and-siri"></a>지역화 및 Siri

IOS 장치에서 사용자는 시스템 기본값과 다른 Siri에 대 한 언어를 선택할 수 있습니다. 지역화 된 데이터로 작업 하는 경우 응용 프로그램은 `SiriLanguageCode` `INPreferences` 클래스의 메서드를 사용 하 여 siri에서 언어 코드를 가져와야 합니다. 예:

```csharp
var language = INPreferences.SiriLanguageCode();

// Take action based on language
if (language == "en-US") {
    // Do something...
}
```

### <a name="adding-user-specific-vocabulary"></a>사용자별 어휘 추가

사용자 특정 어휘는 앱의 개별 사용자에 게 고유한 단어나 구를 제공 합니다. 이러한 항목은 목록 시작 부분에서 가장 중요 한 용어를 사용 하 여 사용자에 대 한 가장 중요 한 사용 우선 순위에 따라 정렬 된 주문 집합으로 주 앱 (앱 확장이 아님)에서 런타임에 제공 됩니다.

사용자 관련 어휘는 다음 범주 중 하나에 속해야 합니다.

- 연락처 이름 (연락처 프레임 워크에서 관리 되지 않음)
- 사진 태그.
- 사진 앨범 이름입니다.
- 단련 이름.

사용자 지정 어휘로 등록 하는 용어를 선택 하는 경우 앱에 익숙하지 않은 사람이 잘못 해석 될 수 있는 용어를 선택 합니다. "내 체력" 또는 "내 앨범"과 같은 일반적인 용어를 등록 하지 마십시오. 예를 들어 MonkeyChat 앱은 사용자 주소록의 각 연락처와 연결 된 애칭을 등록 합니다.

앱은 `SetVocabularyStrings` `INVocabulary` 클래스의 메서드를 호출 하 고 주 앱에서 컬렉션을 `NSOrderedSet` 전달 하 여 사용자별 어휘를 제공 합니다. 새 용어를 추가 하기 전에 `RemoveAllVocabularyStrings` 응용 프로그램에서 항상 메서드를 먼저 호출 하 여 기존 용어를 제거 해야 합니다. 예:

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

이 코드가 준비 되 면 다음과 같이 호출 될 수 있습니다.

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
> Siri는 사용자 지정 어휘를 힌트로 처리 하 고 최대한 많은 용어를 통합 합니다. 그러나 사용자 지정 어휘를 위한 공간이 제한 되므로 혼동 될 수 있는 용어를 등록 하는 것이 중요 _하므로 등록 된_ 용어의 총 수가 최소로 유지 됩니다.

자세한 내용은 사용자 지정 어휘 [참조](~/ios/platform/sirikit/understanding-sirikit.md) 및 Apple의 [사용자 지정 어휘 참조 지정](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1)을 참조 하세요.

### <a name="adding-app-specific-vocabulary"></a>앱 특정 어휘 추가

앱 특정 어휘는 차량 유형 또는 체력 단련 이름과 같은 모든 앱 사용자에 게 알려진 특정 단어 및 구를 정의 합니다. 이러한 `AppIntentVocabulary.plist` 파일은 응용 프로그램의 일부 이기 때문에 주 앱 번들의 일부로 파일에 정의 됩니다. 또한 이러한 단어와 구를 지역화 해야 합니다.

앱 특정 어휘 용어는 다음 범주 중 하나에 속해야 합니다.

- 타는 옵션입니다.
- 단련 이름.

앱 특정 어휘 파일에는 두 개의 루트 수준 키가 포함 되어 있습니다.

- `ParameterVocabularies`**필수** -적용 되는 앱의 사용자 지정 용어 및 의도 매개 변수를 정의 합니다.
- `IntentPhrases`**선택 사항** -에 `ParameterVocabularies`정의 된 사용자 지정 용어를 사용 하는 예제 구를 포함 합니다.

의 각 항목은 `ParameterVocabularies` 해당 용어가 적용 되는 ID 문자열, 용어 및 의도를 지정 해야 합니다. 또한 단일 용어는 여러 의도에 적용 될 수 있습니다.

허용 되는 값 및 필요한 파일 구조에 대 한 전체 목록은 Apple의 [앱 어휘 파일 형식 참조](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1)를 참조 하세요.

응용 프로그램 프로젝트 `AppIntentVocabulary.plist` 에 파일을 추가 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **솔루션 탐색기** 에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다.  >  **iOS**:

    [![](implementing-sirikit-images/plist01.png "속성 목록 추가")](implementing-sirikit-images/plist01.png#lightbox)
2. `AppIntentVocabulary.plist` **솔루션 탐색기** 파일을 두 번 클릭 하 여 편집용으로 엽니다.
3. `ParameterVocabularies` `Array` **+** 키를 추가 하려면를 클릭 하 고, 이름을로 설정 하 고, **유형을** 로 설정 합니다.

    [![](implementing-sirikit-images/plist02.png "이름을 ParameterVocabularies로 설정 하 고 형식을 배열로 설정 합니다.")](implementing-sirikit-images/plist02.png#lightbox)
4. 단추를 확장 `ParameterVocabularies` 하 고 `Dictionary` 클릭 한 다음 형식을로 설정 합니다. **+**

    [![](implementing-sirikit-images/plist03.png "형식을 사전으로 설정 합니다.")](implementing-sirikit-images/plist03.png#lightbox)
5. `ParameterNames` `Array`새키를 추가 하려면를 클릭 하 고, 이름을로 설정 하 고, 유형을로 설정 합니다 **+** .

    [![](implementing-sirikit-images/plist04.png "이름을 ParameterNames로 설정 하 고 형식을 배열로 설정 합니다.")](implementing-sirikit-images/plist04.png#lightbox)
6. **+** **형식을** 사용하여새키를추가하고값을사용가능한매개변수이름중하나로추가하려면를클릭합니다.`String` 예를 `INStartWorkoutIntent.workoutName`들면 다음과 같습니다.

    [![](implementing-sirikit-images/plist05.png "INStartWorkoutIntent Outname 키")](implementing-sirikit-images/plist05.png#lightbox)
7. 다음 형식을 사용 하 여 `ParameterVocabularies` 키를 키에 추가 합니다. `ParameterVocabulary` `Array`

    [![](implementing-sirikit-images/plist06.png "배열 형식으로 parametervocabulary 키를 Parametervocabulary 키에 추가 합니다.")](implementing-sirikit-images/plist06.png#lightbox)
8. `Dictionary`다음 **형식을** 사용 하 여 새 키를 추가 합니다.

    [![](implementing-sirikit-images/plist07.png "사전 형식으로 새 키를 추가 합니다.")](implementing-sirikit-images/plist07.png#lightbox)
9. 의 `VocabularyItemIdentifier` 형식`String` 으로 키를 추가 하 고 해당 용어에 대 한 고유 ID를 지정 합니다.

    [![](implementing-sirikit-images/plist08.png "문자열 형식으로 VocabularyItemIdentifier 키를 추가 하 고 고유 ID를 지정 합니다.")](implementing-sirikit-images/plist08.png#lightbox)
10. 다음`Array` **형식** 으로 키를 추가 합니다 `VocabularyItemSynonyms` .

    [![](implementing-sirikit-images/plist09.png "배열의 형식으로 VocabularyItemSynonyms 키를 추가 합니다.")](implementing-sirikit-images/plist09.png#lightbox)
11. `Dictionary`다음 **형식을** 사용 하 여 새 키를 추가 합니다.

    [![](implementing-sirikit-images/plist10.png "사전 형식으로 새 키를 추가 합니다.")](implementing-sirikit-images/plist10.png#lightbox)
12. **형식** `VocabularyItemPhrase` 및앱이정의하는용어를사용하여키를추가합니다.`String`

    [![](implementing-sirikit-images/plist11.png "문자열 형식 및 앱이 정의 하는 용어를 사용 하 여 VocabularyItemPhrase 키를 추가 합니다.")](implementing-sirikit-images/plist11.png#lightbox)
13. 의 `VocabularyItemPronunciation` 형식`String` 으로 키를 추가 하 고 용어의 발음을 추가 합니다.

    [![](implementing-sirikit-images/plist12.png "문자열의 형식 및 단어의 발음을 VocabularyItemPronunciation 키를 추가 합니다.")](implementing-sirikit-images/plist12.png#lightbox)
14. 다음`Array` **형식** 으로 키를 추가 합니다 `VocabularyItemExamples` .

    [![](implementing-sirikit-images/plist13.png "배열의 형식으로 VocabularyItemExamples 키를 추가 합니다.")](implementing-sirikit-images/plist13.png#lightbox)
15. 몇 가지 용어 `String` 를 사용 하 여 몇 가지 키를 추가 합니다.

    [![](implementing-sirikit-images/plist14.png "용어 사용 예제를 사용 하 여 몇 가지 문자열 키를 추가 합니다.")](implementing-sirikit-images/plist14.png#lightbox)
16. 앱에서 정의 해야 하는 다른 사용자 지정 조건에 대해 위의 단계를 반복 합니다.
17. 키를 `ParameterVocabularies` 축소 합니다.
18. 다음`Array` **형식** 으로 키를 추가 합니다 `IntentPhrases` .

    [![](implementing-sirikit-images/plist15.png "배열 형식으로 IntentPhrases 키를 추가 합니다.")](implementing-sirikit-images/plist15.png#lightbox)
19. `Dictionary`다음 **형식을** 사용 하 여 새 키를 추가 합니다.

    [![](implementing-sirikit-images/plist16.png "사전 형식으로 새 키를 추가 합니다.")](implementing-sirikit-images/plist16.png#lightbox)
20. 예제에 `IntentName` 대 한 `String` 및 의도의 **형식** 으로 키를 추가 합니다.

    [![](implementing-sirikit-images/plist17.png "예제에 대 한 문자열 및 의도 형식으로 IntentName 키를 추가 합니다.")](implementing-sirikit-images/plist17.png#lightbox)
21. 다음`Array` **형식** 으로 키를 추가 합니다 `IntentExamples` .

    [![](implementing-sirikit-images/plist18.png "배열 형식으로 IntentExamples 키를 추가 합니다.")](implementing-sirikit-images/plist18.png#lightbox)
22. 몇 가지 용어 `String` 를 사용 하 여 몇 가지 키를 추가 합니다.

    [![](implementing-sirikit-images/plist19.png "용어 사용 예제를 사용 하 여 몇 가지 문자열 키를 추가 합니다.")](implementing-sirikit-images/plist19.png#lightbox)
23. 앱에서 사용 예제를 제공 하는 데 필요한 모든 의도에 대해 위의 단계를 반복 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **솔루션 탐색기** 에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 항목 ...을 선택 합니다. > Apple > 속성 목록 > info.plist**:

    [![](implementing-sirikit-images/plist01.w157-sml.png "새 정보를 추가 합니다. info.plist")](implementing-sirikit-images/plist01.w157.png#lightbox)

2. `AppIntentVocabulary.plist` **솔루션 탐색기** 파일을 두 번 클릭 하 여 편집용으로 엽니다.
3. `ParameterVocabularies` `Array` **+** 키를 추가 하려면를 클릭 하 고, 이름을로 설정 하 고, **유형을** 로 설정 합니다.

    [![](implementing-sirikit-images/plist02w.png "이름을 ParameterVocabularies로 설정 하 고 형식을 배열로 설정 합니다.")](implementing-sirikit-images/plist02w.png#lightbox)
4. 단추를 확장 `ParameterVocabularies` 하 고 `Dictionary` 클릭 한 다음 형식을로 설정 합니다. **+**

    [![](implementing-sirikit-images/plist03w.png "형식을 사전으로 설정 합니다.")](implementing-sirikit-images/plist03w.png#lightbox)
5. `ParameterNames` `Array`새키를 추가 하려면를 클릭 하 고, 이름을로 설정 하 고, 유형을로 설정 합니다 **+** .

    [![](implementing-sirikit-images/plist04w.png "이름을 ParameterNames로 설정 하 고 형식을 배열로 설정 합니다.")](implementing-sirikit-images/plist04w.png#lightbox)
6. **+** **형식을** 사용하여새키를추가하고값을사용가능한매개변수이름중하나로추가하려면를클릭합니다.`String` 예를 `INStartWorkoutIntent.workoutName`들면 다음과 같습니다.

    [![](implementing-sirikit-images/plist05w.png "INStartWorkoutIntent Outname 키")](implementing-sirikit-images/plist05w.png#lightbox)
7. 다음 형식을 사용 하 여 `ParameterVocabularies` 키를 키에 추가 합니다. `ParameterVocabulary` `Array`

    [![](implementing-sirikit-images/plist06w.png "배열 형식으로 parametervocabulary 키를 Parametervocabulary 키에 추가 합니다.")](implementing-sirikit-images/plist06w.png#lightbox)
8. `Dictionary`다음 **형식을** 사용 하 여 새 키를 추가 합니다.

    [![](implementing-sirikit-images/plist07w.png "사전 형식으로 새 키를 추가 합니다.")](implementing-sirikit-images/plist07w.png#lightbox)
9. 의 `VocabularyItemIdentifier` 형식`String` 으로 키를 추가 하 고 해당 용어에 대 한 고유 ID를 지정 합니다.

    [![](implementing-sirikit-images/plist08w.png "문자열 형식으로 VocabularyItemIdentifier 키를 추가 하 고 해당 용어에 대 한 고유 ID를 지정 합니다.")](implementing-sirikit-images/plist08w.png#lightbox)
10. 다음`Array` **형식** 으로 키를 추가 합니다 `VocabularyItemSynonyms` .

    [![](implementing-sirikit-images/plist09w.png "배열의 형식으로 VocabularyItemSynonyms 키를 추가 합니다.")](implementing-sirikit-images/plist09w.png#lightbox)
11. `Dictionary`다음 **형식을** 사용 하 여 새 키를 추가 합니다.

    [![](implementing-sirikit-images/plist10w.png "사전 형식으로 새 키를 추가 합니다.")](implementing-sirikit-images/plist10w.png#lightbox)
12. **형식** `VocabularyItemPhrase` 및앱이정의하는용어를사용하여키를추가합니다.`String`

    [![](implementing-sirikit-images/plist11w.png "문자열 형식 및 앱이 정의 하는 용어를 사용 하 여 VocabularyItemPhrase 키를 추가 합니다.")](implementing-sirikit-images/plist11w.png#lightbox)
13. 의 `VocabularyItemPronunciation` 형식`String` 으로 키를 추가 하 고 용어의 발음을 추가 합니다.

    [![](implementing-sirikit-images/plist12w.png "문자열의 형식 및 단어의 발음을 VocabularyItemPronunciation 키를 추가 합니다.")](implementing-sirikit-images/plist12w.png#lightbox)
14. 다음`Array` **형식** 으로 키를 추가 합니다 `VocabularyItemExamples` .

    [![](implementing-sirikit-images/plist13w.png "배열의 형식으로 VocabularyItemExamples 키를 추가 합니다.")](implementing-sirikit-images/plist13w.png#lightbox)
15. 몇 가지 용어 `String` 를 사용 하 여 몇 가지 키를 추가 합니다.

    [![](implementing-sirikit-images/plist14w.png "용어 사용 예제를 사용 하 여 몇 가지 문자열 키를 추가 합니다.")](implementing-sirikit-images/plist14w.png#lightbox)
16. 앱에서 정의 해야 하는 다른 사용자 지정 조건에 대해 위의 단계를 반복 합니다.
17. 키를 `ParameterVocabularies` 축소 합니다.
18. 다음`Array` **형식** 으로 키를 추가 합니다 `IntentPhrases` .

    [![](implementing-sirikit-images/plist15w.png "배열 형식으로 IntentPhrases 키를 추가 합니다.")](implementing-sirikit-images/plist15w.png#lightbox)
19. `Dictionary`다음 **형식을** 사용 하 여 새 키를 추가 합니다.

    [![](implementing-sirikit-images/plist16w.png "사전 형식으로 새 키를 추가 합니다.")](implementing-sirikit-images/plist16w.png#lightbox)
20. 예제에 `IntentName` 대 한 `String` 및 의도의 **형식** 으로 키를 추가 합니다.

    [![](implementing-sirikit-images/plist17w.png "예제에 대 한 문자열 및 의도 형식으로 IntentName 키를 추가 합니다.")](implementing-sirikit-images/plist17w.png#lightbox)
21. 다음`Array` **형식** 으로 키를 추가 합니다 `IntentExamples` .

    [![](implementing-sirikit-images/plist18w.png "배열 형식으로 IntentExamples 키를 추가 합니다.")](implementing-sirikit-images/plist18w.png#lightbox)
22. 몇 가지 용어 `String` 를 사용 하 여 몇 가지 키를 추가 합니다.

    [![](implementing-sirikit-images/plist19w.png "용어 사용 예제를 사용 하 여 몇 가지 문자열 키를 추가 합니다.")](implementing-sirikit-images/plist19w.png#lightbox)
23. 앱에서 사용 예제를 제공 하는 데 필요한 모든 의도에 대해 위의 단계를 반복 합니다.

-----

> [!IMPORTANT]
> 는 `AppIntentVocabulary.plist` 개발 중에 테스트 장치에 siri에 등록 되며, siri가 사용자 지정 어휘를 통합 하는 데 약간의 시간이 걸릴 수 있습니다. 따라서 테스터는 업데이트 된 앱 별 어휘 테스트를 시도 하기 전에 몇 분 정도 기다려야 합니다.

자세한 내용은 [앱 별 용어 참조](~/ios/platform/sirikit/understanding-sirikit.md) 및 Apple의 [사용자 지정 어휘 참조 지정](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1)을 참조 하세요.

## <a name="adding-an-intents-extension"></a>의도 확장 추가

이제 앱이 SiriKit를 채택할 준비가 되었으므로 개발자는 Siri 통합에 필요한 의도를 처리 하기 위해 하나 이상의 의도 확장을 솔루션에 추가 해야 합니다.

필요한 각 의도 확장에 대해 다음을 수행 합니다.

- Xamarin.ios 앱 솔루션에 의도 확장 프로젝트를 추가 합니다.
- 의도 확장 `Info.plist` 파일을 구성 합니다.
- 의도 확장 주 클래스를 수정 합니다.

자세한 내용은 [의도 확장 참조](~/ios/platform/sirikit/understanding-sirikit.md) 를 참조 하 고, 인 텐트 [확장 참조](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CreatingtheIntentsExtension.html#//apple_ref/doc/uid/TP40016875-CH4-SW1)를 만드세요.

### <a name="creating-the-extension"></a>확장 만들기

솔루션에 의도 확장을 추가 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad** 에서 **솔루션 이름을** 마우스 오른쪽 단추로 클릭 하 고 **추가** > **새 프로젝트 추가**...를 선택 합니다.
2. 대화 상자에서 **iOS** > **확장** > **의도 확장** 을 선택 하 고 **다음** 단추를 클릭 합니다. 

    [![](implementing-sirikit-images/intents05.png "의도 확장 선택")](implementing-sirikit-images/intents05.png#lightbox)
3. 다음으로 의도 확장의 **이름을** 입력 하 고 **다음** 단추를 클릭 합니다. 

    [![](implementing-sirikit-images/intents06.png "의도 확장의 이름을 입력 합니다.")](implementing-sirikit-images/intents06.png#lightbox)
4. 마지막으로, **만들기** 단추를 클릭 하 여 앱 솔루션에 의도 확장을 추가 합니다. 

    [![](implementing-sirikit-images/intents07.png "앱 솔루션에 의도 확장 추가")](implementing-sirikit-images/intents07.png#lightbox)
5. **솔루션 탐색기**에서 새로 만든 의도 확장의 **References** 폴더를 마우스 오른쪽 단추로 클릭 합니다. 공용 공유 코드 라이브러리 프로젝트 (위에서 만든 앱)의 이름을 확인 하 고 **확인** 단추를 클릭 합니다. 

    [![](implementing-sirikit-images/intents08.png "공통 공유 코드 라이브러리 프로젝트의 이름을 선택 합니다.")](implementing-sirikit-images/intents08.png#lightbox)
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **솔루션 탐색기** 에서 **솔루션 이름을** 마우스 오른쪽 단추로 클릭 하 고 **추가** > **새 프로젝트 추가**...를 선택 합니다.
2. 대화 상자에서  **C# Visual > IOS 확장 > 의도 확장** 을 선택 하 고 **다음** 단추를 클릭 합니다.

    [![](implementing-sirikit-images/intents05.w157-sml.png "의도 확장 선택")](implementing-sirikit-images/intents05.w157.png#lightbox)
3. 그런 다음 의도 확장의 **이름을** 입력 하 고 **확인** 단추를 클릭 합니다.
4. **솔루션 탐색기**에서 새로 만든 의도 확장의 **참조** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **> 참조 추가**를 선택 합니다. 공용 공유 코드 라이브러리 프로젝트 (위에서 만든 앱)의 이름을 확인 하 고 **확인** 단추를 클릭 합니다.

    [![](implementing-sirikit-images/intents08w.png "공통 공유 코드 라이브러리 프로젝트의 이름을 선택 합니다.")](implementing-sirikit-images/intents08w.png#lightbox)
    
-----

앱에 필요한 의도 확장 (위의 [확장에 대 한 앱 설계](#architecting-the-app-for-extensions) 에 기반)의 수에 대해 이러한 단계를 반복 합니다.

### <a name="configuring-the-infoplist"></a>Info.plist 구성

앱의 솔루션에 추가 된 각 의도 확장에 대해는 앱에서 사용할 수 있도록 `Info.plist` 파일에 구성 되어 있어야 합니다.

일반적인 앱 확장과 마찬가지로 앱에는 및 `NSExtension` `NSExtensionAttributes`의 기존 키가 있습니다. 의도 확장의 경우 구성 해야 하는 두 가지 새로운 특성이 있습니다.

[![](implementing-sirikit-images/intents01.png "구성 해야 하는 두 개의 새 특성")](implementing-sirikit-images/intents01.png#lightbox)

- **Intentssupported** -필수 항목으로, 앱이 의도 확장에서 지원 하려고 하는 의도 클래스 이름 배열로 구성 됩니다.
- **IntentsRestrictedWhileLocked** -확장의 잠금 화면 동작을 지정 하기 위해 앱에 대 한 선택적 키입니다. 앱이 의도 확장에서 사용 하기 위해 로그인 해야 하는 의도 클래스 이름 배열로 구성 됩니다.

의도 확장의 `Info.plist` 파일을 구성 하려면 **솔루션 탐색기** 에서 해당 파일을 두 번 클릭 하 여 편집을 위해 엽니다. 그런 다음 **소스** 뷰로 전환한 후 편집기에서 `NSExtension` 및 `NSExtensionAttributes` 키를 확장 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](implementing-sirikit-images/intents02.png "편집기의 NNSExtensionAttributes 압력 및 키 키")](implementing-sirikit-images/intents02.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents02w.png "편집기의 NNSExtensionAttributes 압력 및 키 키")](implementing-sirikit-images/intents02w.png#lightbox)

-----

키를 `IntentsSupported` 확장 하 고이 확장에서 지원할 의도 클래스의 이름을 추가 합니다. MonkeyChat 앱 예제에서는 다음을 지원 합니다 `INSendMessageIntent`.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](implementing-sirikit-images/intents09.png "INSendMessageIntent 키")](implementing-sirikit-images/intents09.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents09w.png "INSendMessageIntent 키")](implementing-sirikit-images/intents09w.png#lightbox)

-----

앱에서 지정 된 의도를 사용 하기 위해 사용자가 장치에 로그온 해야 하는 경우 `IntentRestrictedWhileLocked` 키를 확장 하 고 제한 된 액세스 권한이 있는 의도의 클래스 이름을 추가 합니다. 예 MonkeyChat 앱의 경우 사용자는 다음을 추가 `INSendMessageIntent`하기 위해 채팅 메시지를 보내도록 로그인 되어 있어야 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](implementing-sirikit-images/intents10.png "추가 된 INSendMessageIntent 키")](implementing-sirikit-images/intents10.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents10w.png "추가 된 INSendMessageIntent 키")](implementing-sirikit-images/intents10w.png#lightbox)

-----


사용 가능한 의도 도메인의 전체 목록은 Apple의 [의도 된 도메인 참조](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)를 참조 하세요.

### <a name="configuring-the-main-class"></a>주 클래스 구성

다음으로 개발자는 내재 된 확장의 기본 진입점 역할을 하는 주 클래스를 Siri로 구성 해야 합니다. 이는 `IINIntentHandler` 대리자를 준수 하 `INExtension` 는의 서브 클래스 여야 합니다. 예를 들어:

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

앱이 의도 확장 주 클래스 ( `GetHandler` 메서드)에서 구현 해야 하는 하나의 독립 메서드가 있습니다. 이 메서드는 SiriKit의 의도에 따라 전달 되며 앱은 지정 된 의도의 형식과 일치 하는 **의도 처리기** 를 반환 해야 합니다.

예제 monkeychat 앱은 하나의 의도만 처리 하기 때문에 `GetHandler` 메서드에서 자신을 반환 합니다. 확장이 둘 이상의 의도를 처리 한 경우 개발자는 각 의도 형식에 대 한 클래스를 추가 하 고 메서드에 전달 된 `Intent` 에 따라 여기에 인스턴스를 반환 합니다.

### <a name="handling-the-resolve-stage"></a>확인 단계 처리

확인 단계는 의도 확장이 Siri에서 전달 되 고 사용자의 대화를 통해 설정 된 매개 변수를 명확 하 게 확인 하 고 유효성을 검사 하는 위치입니다.

Siri에서 전송 되는 각 매개 변수에는 `Resolve` 메서드가 있습니다. 앱은 사용자 로부터 올바른 답변을 얻기 위해 Siri 도움말이 필요할 수 있는 모든 매개 변수에 대해이 메서드를 구현 해야 합니다.

MonkeyChat 앱 예제에서 의도 확장은 메시지를 보낼 수신자가 하나 이상 필요 합니다. 목록의 각 받는 사람에 대해 확장에서 다음과 같은 결과를 받을 수 있는 연락처 검색을 수행 해야 합니다.

- 정확히 하나의 일치 하는 연락처를 찾았습니다.
- 둘 이상의 일치 하는 연락처를 찾았습니다.
- 일치 하는 연락처를 찾을 수 없습니다.

또한 MonkeyChat는 메시지 본문에 대 한 콘텐츠가 필요 합니다. 사용자가이를 제공 하지 않은 경우 Siri는 사용자에 게 콘텐츠를 입력 하 라는 메시지를 표시 해야 합니다.

의도 확장은 이러한 각 사례를 정상적으로 처리 해야 합니다.


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

자세한 내용은 [확인 단계 참조](~/ios/platform/sirikit/understanding-sirikit.md) 및 Apple의 [해결 및 처리 의도 참조](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ResolvingandHandlingIntents.html#//apple_ref/doc/uid/TP40016875-CH5-SW1)를 참조 하세요.

### <a name="handling-the-confirm-stage"></a>확인 단계 처리

확인 단계는 의도 확장이 사용자의 요청을 충족 하는 모든 정보를 포함 하 고 있는지 확인 하는 것입니다. 앱은 Siri에 발생 하는 것에 대 한 지원 세부 정보를 모두 확인 하 여 사용자가 의도 된 동작 임을 확인할 수 있도록 합니다.

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

자세한 내용은 [확인 단계 참조](~/ios/platform/sirikit/understanding-sirikit.md)를 참조 하세요.

### <a name="processing-the-intent"></a>의도 처리

이는 의도 확장이 실제로 사용자 요청을 처리 하기 위해 작업을 수행 하 고, 사용자에 게 알릴 수 있도록 결과를 Siri에 다시 전달 하는 지점입니다.


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

자세한 내용은 [핸들 단계 참조](~/ios/platform/sirikit/understanding-sirikit.md)를 참조 하세요.

## <a name="adding-an-intents-ui-extension"></a>인 텐트 UI 확장 추가

선택적인 인 UI 확장은 응용 프로그램의 UI 및 브랜딩을 Siri 환경으로 전환 하 고 사용자가 앱에 연결 되도록 할 수 있는 기회를 제공 합니다. 이 확장을 사용 하 여 앱은 브랜드 뿐만 아니라 시각적 개체 및 기타 정보를 성적 증명서에 가져올 수 있습니다.

[![](implementing-sirikit-images/intentsui01.png "예제 인 UI 확장 출력")](implementing-sirikit-images/intentsui01.png#lightbox)

의도 확장과 마찬가지로 개발자는이 UI 확장에 대해 다음 단계를 수행 합니다.

- 인 텐트 UI 확장 프로젝트를 Xamarin.ios 앱 솔루션에 추가 합니다.
- 인 텐트 UI 확장 `Info.plist` 파일을 구성 합니다.
- 인 텐트 UI 확장 기본 클래스를 수정 합니다.

자세한 내용은 [의도 UI 확장 참조](~/ios/platform/sirikit/understanding-sirikit.md) 및 Apple의 [사용자 지정 인터페이스 참조 제공](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ProvidingaCustomInterface.html#//apple_ref/doc/uid/TP40016875-CH7-SW1)을 참조 하세요.

### <a name="creating-the-extension"></a>확장 만들기

솔루션에 의도 UI 확장을 추가 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad** 에서 **솔루션 이름을** 마우스 오른쪽 단추로 클릭 하 고 **추가** > **새 프로젝트 추가**...를 선택 합니다.
2. 대화 상자에서 **iOS** > **확장** > **의도 UI 확장** 을 선택 하 고 **다음** 단추를 클릭 합니다. 

    [![](implementing-sirikit-images/intents11.png "의도 UI 확장 선택")](implementing-sirikit-images/intents11.png#lightbox)
3. 다음으로 의도 확장의 **이름을** 입력 하 고 **다음** 단추를 클릭 합니다. 

    [![](implementing-sirikit-images/intents12.png "의도 확장의 이름을 입력 합니다.")](implementing-sirikit-images/intents12.png#lightbox)
4. 마지막으로, **만들기** 단추를 클릭 하 여 앱 솔루션에 의도 확장을 추가 합니다. 

    [![](implementing-sirikit-images/intents13.png "앱 솔루션에 의도 확장 추가")](implementing-sirikit-images/intents13.png#lightbox)
5. **솔루션 탐색기**에서 새로 만든 의도 확장의 **References** 폴더를 마우스 오른쪽 단추로 클릭 합니다. 공용 공유 코드 라이브러리 프로젝트 (위에서 만든 앱)의 이름을 확인 하 고 **확인** 단추를 클릭 합니다. 

    [![](implementing-sirikit-images/intents14.png "공통 공유 코드 라이브러리 프로젝트의 이름을 선택 합니다.")](implementing-sirikit-images/intents14.png#lightbox)
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **솔루션 탐색기** 에서 **솔루션 이름을** 마우스 오른쪽 단추로 클릭 하 고 **추가** > **새 프로젝트 추가** ...를 선택 합니다.
2. 대화 상자에서 **iOS** > **확장** > **의도 UI 확장** 을 선택 하 고 **다음** 단추를 클릭 합니다.
3. 그런 다음 의도 확장의 **이름을** 입력 하 고 **확인** 단추를 클릭 합니다.
4. **솔루션 탐색기**에서 새로 만든 의도 확장의 **References** 폴더를 마우스 오른쪽 단추로 클릭 합니다. 공용 공유 코드 라이브러리 프로젝트 (위에서 만든 앱)의 이름을 확인 하 고 **확인** 단추를 클릭 합니다.
    
-----

### <a name="configuring-the-infoplist"></a>Info.plist 구성

앱에서 사용할 수 있도록 의도 `Info.plist` UI 확장의 파일을 구성 합니다.

일반적인 앱 확장과 마찬가지로 앱에는 및 `NSExtension` `NSExtensionAttributes`의 기존 키가 있습니다. 의도 확장의 경우 구성 해야 하는 새 특성이 하나 있습니다.

[![](implementing-sirikit-images/intents03.png "구성 해야 하는 새 특성 하나")](implementing-sirikit-images/intents03.png#lightbox)

**Intentssupported** 는 필수 이며 앱이 의도 확장에서 지원 하려고 하는 의도 클래스 이름 배열로 구성 됩니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

의도 UI 확장의 `Info.plist` 파일을 구성 하려면 **솔루션 탐색기** 에서 해당 파일을 두 번 클릭 하 여 편집을 위해 엽니다. 그런 다음 **소스** 뷰로 전환한 후 편집기에서 `NSExtension` 및 `NSExtensionAttributes` 키를 확장 합니다.

[![](implementing-sirikit-images/intents04.png "편집기의 NNSExtensionAttributes 압력 및 키 키")](implementing-sirikit-images/intents04.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

의도 UI 확장의 `Info.plist` 파일을 구성 하려면 **솔루션 탐색기** 에서 해당 파일을 두 번 클릭 하 여 편집을 위해 엽니다. 편집기에서 `NSExtension` 및 `NSExtensionAttributes` 키를 확장 합니다.

[![](implementing-sirikit-images/intents04w.png "편집기에서 N화면이 어떻게 보일지 압력 및 NSExtensionAttributes 키를 검색 합니다.")](implementing-sirikit-images/intents04w.png#lightbox)

-----

키를 `IntentsSupported` 확장 하 고이 확장에서 지원할 의도 클래스의 이름을 추가 합니다. MonkeyChat 앱 예제에서는 다음을 지원 합니다 `INSendMessageIntent`.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](implementing-sirikit-images/intents15.png "INSendMessageIntent 키")](implementing-sirikit-images/intents15.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents15w.png "INSendMessageIntent 키")](implementing-sirikit-images/intents15w.png#lightbox)

-----

사용 가능한 의도 도메인의 전체 목록은 Apple의 [의도 된 도메인 참조](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)를 참조 하세요.

### <a name="configuring-the-main-class"></a>주 클래스 구성

내재 된 UI 확장의 기본 진입점 역할을 하는 주 클래스를 Siri에 구성 합니다. 인터페이스를 준수 하는의 `UIViewController` 서브 클래스 여야 합니다. `IINUIHostedViewController` 예를 들어:

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

Siri는 `INInteraction` 클래스 인스턴스 `Configure` 를 내재 된 UI 확장 내 `UIViewController` 인스턴스의 메서드에 전달 합니다.

개체 `INInteraction` 는 확장에 대 한 세 가지 주요 정보를 제공 합니다.

1. 처리 중인 의도 개체입니다.
2. 의도 확장의 `Confirm` 및 `Handle` 메서드에서 의도 응답 개체입니다.
3. 앱과 Siri 간의 상호 작용 상태를 정의 하는 상호 작용 상태입니다.

인스턴스 `UIViewController` 는 siri와의 상호 작용에 대 한 원칙 클래스 이며에서 `UIViewController`상속 되기 때문에 uikit의 모든 기능에 액세스할 수 있습니다.

Siri가 `UIViewController` 클래스의 `Configure` 메서드를 호출할 때 뷰 컨트롤러는 siri Snippit 또는 Maps 카드에서 호스트 됨을 나타내는 뷰 컨텍스트를 전달 합니다.

Siri는 앱이 구성을 완료 한 후 앱에서 원하는 보기 크기를 반환 해야 하는 완료 처리기를 전달 합니다.

### <a name="design-the-ui-in-ios-designer"></a>IOS 디자이너에서 UI 디자인

IOS 디자이너에서 의도 UI 확장의 사용자 인터페이스를 레이아웃 합니다. `MainInterface.storyboard` **솔루션 탐색기** 에서 확장 파일을 두 번 클릭 하 여 편집용으로 엽니다. 모든 필수 UI 요소를 끌어 사용자 인터페이스를 작성 하 고 변경 내용을 저장 합니다.

> [!IMPORTANT]
> 또는 `UIButtons` `UIViewController`와 같은 대화형 요소를 의도 ui 확장에 추가할 수 있지만 비 대화형으로는 의도 ui로 엄격히 사용할 수 없으며 사용자는 상호 작용할 수 없습니다. `UITextFields`

### <a name="wire-up-the-user-interface"></a>사용자 인터페이스 연결

IOS 디자이너에서 만든 의도 UI 확장의 사용자 인터페이스를 사용 하 여 `UIViewController` 하위 클래스를 편집 하 고 다음과 같이 `Configure` 메서드를 재정의 합니다.

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

### <a name="overriding-the-default-siri-ui"></a>기본 Siri UI 재정의

인 텐트 UI 확장은 항상 UI의 맨 위에 있는 앱 아이콘 및 이름과 같은 다른 Siri 콘텐츠와 함께 표시 되거나 의도에 따라 단추 (예: 보내기 또는 취소)가 아래쪽에 표시 될 수 있습니다.

앱이 기본적으로 Siri가 표시 하는 정보를 사용자에 게 교체할 수 있는 몇 가지 인스턴스가 있습니다. 예를 들어 앱이 기본 환경을 앱에 맞게 조정 된 상태로 바꿀 수 있습니다.

인 텐트 ui 확장에서 기본 siri ui의 요소를 재정의 해야 하는 경우 `UIViewController` 하위 클래스는 `IINUIHostedViewSiriProviding` 인터페이스를 구현 하 고 특정 인터페이스 요소를 표시 하기 위해 옵트인 해야 합니다.

`UIViewController` 서브 클래스에 다음 코드를 추가 하 여 의도 UI 확장에서 메시지 내용을 이미 표시 하 고 있음을 siri에 알립니다.

```csharp
public bool DisplaysMessage {
    get {return true;}
}
```

### <a name="considerations"></a>고려 사항

Apple은 의도 UI 확장을 디자인 하 고 구현할 때 개발자가 다음 사항을 고려 하는 것을 제안 합니다.

- **메모리 사용** 에 대해 설명 합니다. 의도 UI 확장은 일시적 이며 짧은 시간 동안만 표시 되기 때문에 시스템은 전체 앱에서 사용 되는 것 보다 더 엄격한 메모리 제약 조건을 적용 합니다.
- **최소 및 최대 보기 크기를 고려** 합니다. 즉, 모든 iOS 장치 유형, 크기 및 방향에 대해 의도 UI 확장이 제대로 작동 하는지 확인 합니다. 또한 앱에서 Siri로 다시 보내는 원하는 크기를 부여 하지 못할 수 있습니다.
- **유연 하 고 적응 레이아웃 패턴을 사용** 하 여 모든 장치에서 UI가 제대로 표시 되도록 합니다.

## <a name="summary"></a>요약

이 문서에서는 SiriKit에 대해 설명 하 고 iOS 장치에서 Siri 및 Maps 앱을 사용 하 여 사용자에 게 액세스할 수 있는 서비스를 제공 하기 위해 Xamarin.ios 앱에 추가 하는 방법을 보여 줍니다.




## <a name="related-links"></a>관련 링크

- [ElizaChat 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-elizachat)
- [SiriKit 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [의도 프레임 워크 참조](https://developer.apple.com/reference/intents)
- [의도 UI 프레임 워크 참조](https://developer.apple.com/reference/intentsui)
- [의도 도메인 참조](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)
