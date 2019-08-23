---
title: Xamarin.ios의 iOS 확장
description: 이 문서에서는 알림 센터 내에서와 같은 표준 컨텍스트에서 iOS에 표시 되는 위젯의 확장에 대해 설명 합니다. 확장을 만들고 부모 앱에서 통신 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 3DEB3D43-3E4A-4099-8331-93C1E7A77095
ms.technology: xamarin-ios
ms.custom: xamu-video
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 4137ce7542a213a0a4c27b6a66b38828e4646520
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68653475"
---
# <a name="ios-extensions-in-xamarinios"></a>Xamarin.ios의 iOS 확장

> [!VIDEO https://youtube.com/embed/Sd0-ch9Udmk]

**IOS 비디오에서 확장 만들기**

Ios 8에 도입 된 확장은 사용자가 특수 `UIViewControllers` 입력 또는 기타 컨텍스트를 수행 하기 위해 요청 하는 사용자 지정 키보드 유형으로 **알림 센터**내에서와 같은 표준 컨텍스트 내에서 ios에 의해 제공 되는 특수화입니다. 확장에서 특수 효과 필터를 제공할 수 있는 사진을 편집 하는 것과 같습니다.

모든 확장 프로그램은 컨테이너 앱과 함께 설치 되며 (두 요소가 모두 64 비트 통합 Api를 사용 하 여 작성 됨) 호스트 앱의 특정 확장 지점에서 활성화 됩니다. 또한 기존 시스템 함수에 대 한 보완으로 사용 되므로 고성능, 간결한 및 강력 해야 합니다. 

## <a name="extension-points"></a>확장 위치

|형식|설명|확장 지점|호스트 앱|
|--- |--- |--- |--- |
|작업|특정 미디어 유형에 대 한 특수 편집기나 뷰어|`com.apple.ui-services`|임의의 값|
|문서 공급자|앱에서 원격 문서 저장소를 사용할 수 있습니다.|`com.apple.fileprovider-ui`|[Uidocumentpickerviewcontroller](xref:UIKit.UIDocumentPickerViewController) 를 사용 하는 앱|
|키보드|대체 키보드|`com.apple.keyboard-service`|임의의 값|
|사진 편집|사진 조작 및 편집|`com.apple.photo-editing`|사진. 앱 편집기|
|공유|는 소셜 네트워크, 메시징 서비스 등을 사용 하 여 데이터를 공유 합니다.|`com.apple.share-services`|임의의 값|
|오늘|오늘 화면 또는 알림 센터에 표시 되는 "위젯"|`com.apple.widget-extensions`|오늘 및 알림 센터|

[추가 확장 지점은](~/ios/platform/introduction-to-ios10/index.md#app-extensions) iOS 10에 추가 되었습니다.

## <a name="limitations"></a>제한 사항

확장에는 몇 가지 제한 사항이 있습니다. 일부는 모든 형식 (예를 들어, 어떤 유형의 확장이 카메라 또는 마이크에 액세스할 수 없음)이 고 다른 유형의 확장에는 사용에 대 한 특정 제한 사항이 있을 수 있습니다 (예를 들어 사용자 지정 키보드). 는 암호 등의 보안 데이터 입력 필드에 사용할 수 없습니다. 

일반적인 제한 사항은 다음과 같습니다.

- [상태 키트](~/ios/platform/healthkit.md) 및 [이벤트 키트 UI](~/ios/platform/eventkit.md) 프레임 워크를 사용할 수 없습니다.
- 확장은 [확장 된 백그라운드 모드](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md) 를 사용할 수 없습니다.
- 확장은 장치의 카메라 또는 마이크 (기존 미디어 파일에 액세스할 수 있지만)에 액세스할 수 없습니다.
- 확장은 공기 삭제 데이터를 수신할 수 없습니다 (Air을 통해 데이터를 전송할 수 있는 경우에도).
- [Uiactionsheet](xref:UIKit.UIActionSheet) 및 [uiactionsheet](xref:UIKit.UIAlertView) 는 사용할 수 없습니다. 확장은 [Uialertcontroller](xref:UIKit.UIAlertController) 를 사용 해야 합니다.
- [Uiapplication 프로그램](xref:UIKit.UIApplication) 의 여러 멤버를 사용할 수 없습니다. [Uiapplication 프로그램 sharedapplication](xref:UIKit.UIApplication.SharedApplication), [Uiapplication. Openurl](xref:UIKit.UIApplication.OpenUrl(Foundation.NSUrl)), [UIApplication.BeginIgnoringInteractionEvents](xref:UIKit.UIApplication.BeginIgnoringInteractionEvents) 및 [uiapplication. EndIgnoringInteractionEvents](xref:UIKit.UIApplication.EndIgnoringInteractionEvents)
- iOS는 현재 확장에 16gb 메모리 사용 제한을 적용 합니다.
- 기본적으로 키보드 확장에는 네트워크에 대 한 액세스 권한이 없습니다. 이는 Xamarin에서 디버깅을 수행 하기 위해 네트워크 액세스가 필요 하기 때문에 장치에서 디버깅에 영향을 줍니다 (시뮬레이터에서 제한이 적용 되지 않음). 프로젝트 info.plist의 `Requests Open Access` 값을로 `Yes`설정 하 여 네트워크 액세스를 요청할 수 있습니다. 키보드 확장 제한 사항에 대 한 자세한 내용은 Apple의 [사용자 지정 키보드 가이드](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html) 를 참조 하세요.

개별 제한 사항은 Apple의 [앱 확장 프로그래밍 가이드](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/)를 참조 하세요.

## <a name="distributing-installing-and-running-extensions"></a>확장 배포, 설치 및 실행

확장은 컨테이너 앱 내에서 배포 되며,이는 차례로 앱 스토어를 통해 전송 되 고 배포 됩니다. 앱과 함께 배포 된 확장은 해당 시점에 설치 되지만 사용자는 각 확장을 명시적으로 사용 하도록 설정 해야 합니다. 서로 다른 방식으로 다양 한 유형의 확장을 사용할 수 있습니다. 여러 사용자가 **설정** 앱으로 이동 하 여이 앱에서 사용 하도록 설정 해야 하는 경우도 있습니다. 사진을 보낼 때 공유 확장을 사용 하도록 설정 하는 것과 같이 사용 지점에서 사용할 수 있는 경우도 있습니다. 

확장이 사용 되는 앱 (사용자가 확장 지점을 발견 함)은 실행 시 확장을 호스트 하는 앱 이므로 **호스트 앱**이라고 합니다. 확장을 설치 하는 앱은 설치 시 확장을 포함 하는 앱 이므로 **컨테이너 앱**입니다.  

일반적으로 컨테이너 앱은 확장을 설명 하 고 사용자가이를 사용 하도록 설정 하는 과정을 안내 합니다.

## <a name="extension-lifecycle"></a>확장 수명 주기

확장은 단일 [Uiviewcontroller](xref:UIKit.UIViewController) 또는 UI의 여러 화면을 표시 하는 더 복잡 한 확장 처럼 간단할 수 있습니다. 사용자가 _확장 지점_ (예: 이미지를 공유 하는 경우)을 발견 하면 해당 확장 지점에 등록 된 확장에서 선택할 수 있습니다. 

앱 확장 중 하나를 선택 하는 경우 해당 `UIViewController` 확장은 인스턴스화되고 표준 뷰 컨트롤러 수명 주기를 시작 합니다. 그러나 일시 중단 되었지만 일반적으로 종료 되지 않는 일반 앱과 달리, 사용자의 상호 작용을 마치면 확장이 로드 되 고 실행 된 후 반복적으로 종료 됩니다.

확장은 [NSExtensionContext](xref:Foundation.NSExtensionContext) 개체를 통해 해당 호스트 앱과 통신할 수 있습니다. 일부 확장에는 결과가 포함 된 비동기 콜백을 받는 작업이 있습니다. 이러한 콜백은 백그라운드 스레드에서 실행 되며 확장을 고려해 야 합니다. 예를 들어 사용자 인터페이스를 업데이트 하려는 경우 [InvokeOnMainThread](xref:Foundation.NSObject.InvokeOnMainThread*) 를 사용 합니다. 자세한 내용은 아래의 [호스트 앱과의 통신](#communicating-with-the-host-app) 단원을 참조 하세요.

기본적으로 확장 및 해당 컨테이너 앱은 함께 설치 되더라도 통신할 수 없습니다. 경우에 따라 컨테이너 앱은 확장이 설치 된 후 목적이 제공 되는 빈 "배송" 컨테이너입니다. 그러나 상황이 지시한 경우 컨테이너 앱 및 확장은 공통 영역에서 리소스를 공유할 수 있습니다. 또한 **오늘 확장** 은 URL을 열기 위해 컨테이너 앱을 요청할 수 있습니다. 이 동작은 [이벤트 카운트다운 위젯에](https://github.com/xamarin/ios-samples/tree/master/intro-to-extensions)표시 됩니다.

## <a name="creating-an-extension"></a>확장 만들기

확장 (및 해당 컨테이너 앱)은 64 비트 이진 이어야 하며 Xamarin.ios [통합 api](~/cross-platform/macios/unified/index.md)를 사용 하 여 빌드됩니다. 확장을 개발할 때 솔루션에는 두 개 이상의 프로젝트가 포함 됩니다. 컨테이너 앱과 컨테이너에서 제공 하는 각 확장에 대해 하나의 프로젝트가 포함 됩니다.

### <a name="container-app-project-requirements"></a>컨테이너 앱 프로젝트 요구 사항

확장을 설치 하는 데 사용 되는 컨테이너 앱에는 다음과 같은 요구 사항이 있습니다.

- 확장 프로젝트에 대 한 참조를 유지 해야 합니다.   
- 확장을 설치 하는 방법을 제공 하는 것 외에는 아무 작업도 수행 하지 않는 경우에도 완전 한 앱 이어야 합니다 (를 성공적으로 시작 하 고 실행할 수 있어야 함). 
- 확장 프로젝트의 번들 식별자를 기반으로 하는 번들 식별자가 있어야 합니다. 자세한 내용은 아래 섹션을 참조 하세요.

### <a name="extension-project-requirements"></a>확장 프로젝트 요구 사항

또한 확장의 프로젝트에는 다음과 같은 요구 사항이 있습니다.

- 컨테이너 앱의 번들 식별자로 시작 하는 번들 식별자가 있어야 합니다. 예를 들어 컨테이너 앱의 번들 식별자 `com.myCompany.ContainerApp`가 인 경우 확장의 식별자는 다음과 같을 `com.myCompany.ContainerApp.MyExtension`수 있습니다. 

    ![](extensions-images/bundleidentifiers.png) 
- 해당 `Info.plist` 파일에서 적절 한 `NSExtensionPointIdentifier`값 ( `com.apple.widget-extension` 예: **오늘** 알림 센터 위젯)을 사용 하 여 키를 정의 해야 합니다.
- 또한 해당 `Info.plist` 파일의 키 또는 `NSExtensionMainStoryboard` `NSExtensionPrincipalClass` 키를 적절 한 값 *으로 정의 해야* 합니다.
    - 키를 사용 하 여 확장 (빼기 `.storyboard`)의 주 UI를 표시 하는 Storyboard의 이름을 지정 합니다. `NSExtensionMainStoryboard` 예 `Main` 를 들어 `Main.storyboard` 파일의 경우입니다.
    - `NSExtensionPrincipalClass` 키를 사용 하 여 확장이 시작 될 때 초기화 될 클래스를 지정 합니다. 값은의 `UIViewController` **등록** 값과 일치 해야 합니다. 

    ![](extensions-images/registerandprincipalclass.png)

특정 유형의 확장에는 추가 요구 사항이 있을 수 있습니다. 예를 들어 **오늘** 또는 **알림 센터** 확장의 보안 주체 클래스는 [INCWidgetProviding](xref:NotificationCenter.INCWidgetProviding)를 구현 해야 합니다.

> [!IMPORTANT]
> Mac용 Visual Studio에서 제공 하는 확장 템플릿 하나를 사용 하 여 프로젝트를 시작 하는 경우 이러한 요구 사항은 대부분 자동으로 제공 되 고 템플릿에 의해 자동으로 충족 됩니다.

## <a name="walkthrough"></a>연습 

다음 연습에서는 연도에 남은 일 수와 일 수를 계산 하는 예제 **오늘** 위젯을 만듭니다.

[![](extensions-images/carpediemscreenshot-sm.png "연도에서 남은 일 수 및 일 수를 계산 하는 오늘 위젯의 예")](extensions-images/carpediemscreenshot.png#lightbox)

### <a name="creating-the-solution"></a>솔루션 만들기

필요한 솔루션을 만들려면 다음을 수행 합니다.

1. 먼저 새 iOS, **단일 뷰 응용 프로그램** 프로젝트를 만들고 **다음** 단추를 클릭 합니다. 

    [![](extensions-images/today01.png "먼저 새 iOS, 단일 뷰 응용 프로그램 프로젝트를 만들고 다음 단추를 클릭 합니다.")](extensions-images/today01.png#lightbox)
2. 프로젝트 `TodayContainer` 를 호출 하 고 **다음** 단추를 클릭 합니다. 

    [![](extensions-images/today02.png "TodayContainer 프로젝트를 호출 하 고 다음 단추를 클릭 합니다.")](extensions-images/today02.png#lightbox)
3. **프로젝트 이름** 및 **SolutionName** 를 확인 하 고 **만들기** 단추를 클릭 하 여 솔루션을 만듭니다. 

    [![](extensions-images/today03.png "프로젝트 이름 및 SolutionName를 확인 하 고 만들기 단추를 클릭 하 여 솔루션을 만듭니다.")](extensions-images/today03.png#lightbox)
4. 그런 다음 **솔루션 탐색기**에서 솔루션을 마우스 오른쪽 단추로 클릭 하 고 **오늘 확장** 템플릿에서 새 **iOS 확장** 프로젝트를 추가 합니다. 

    [![](extensions-images/today04.png "그런 다음 솔루션 탐색기에서 솔루션을 마우스 오른쪽 단추로 클릭 하 고 오늘 확장 템플릿에서 새 iOS 확장 프로젝트를 추가 합니다.")](extensions-images/today04.png#lightbox)
5. 프로젝트 `DaysRemaining` 를 호출 하 고 **다음** 단추를 클릭 합니다. 

    [![](extensions-images/today05.png "DaysRemaining 프로젝트를 호출 하 고 다음 단추를 클릭 합니다.")](extensions-images/today05.png#lightbox)
6. 프로젝트를 검토 하 고 **만들기** 단추를 클릭 하 여 해당 프로젝트를 만듭니다. 

    [![](extensions-images/today06.png "프로젝트를 검토 하 고 만들기 단추를 클릭 하 여 해당 프로젝트를 만듭니다.")](extensions-images/today06.png#lightbox)

이제 결과 솔루션에는 다음과 같이 두 개의 프로젝트가 있습니다.

[![](extensions-images/today07.png "이제 결과 솔루션에는 다음과 같이 두 개의 프로젝트가 있습니다.")](extensions-images/today07.png#lightbox)

### <a name="creating-the-extension-user-interface"></a>확장 사용자 인터페이스 만들기

다음으로 **오늘** 위젯의 인터페이스를 디자인 해야 합니다. 스토리 보드를 사용 하거나 코드에서 UI를 만들어이 작업을 수행할 수 있습니다. 두 방법 모두 아래에서 자세히 설명 합니다.

#### <a name="using-storyboards"></a>Storyboard 사용

스토리 보드를 사용 하 여 UI를 빌드하려면 다음을 수행 합니다.

1. **솔루션 탐색기**에서 확장 프로젝트의 `Main.storyboard` 파일을 두 번 클릭 하 여 편집용으로 엽니다. 

    [![](extensions-images/today08.png "확장 프로젝트 주 storyboard 파일을 두 번 클릭 하 여 편집용으로 엽니다.")](extensions-images/today08.png#lightbox)
2. 템플릿에 의해 UI에 자동으로 추가 된 레이블을 선택 하 고 **속성 탐색기**의 **위젯** 탭에서 **이름을** `TodayMessage` 지정 합니다. 

    [![](extensions-images/today09.png "템플릿에 의해 UI에 자동으로 추가 된 레이블을 선택 하 고 속성 탐색기의 위젯 탭에서 이름을 TodayMessage로 지정 합니다.")](extensions-images/today09.png#lightbox)
3. 스토리 보드에 대 한 변경 내용을 저장 합니다.

#### <a name="using-code"></a>코드 사용

코드에서 UI를 빌드하려면 다음을 수행 합니다. 

1. **솔루션 탐색기**에서 **DaysRemaining** 프로젝트를 선택 하 `CodeBasedViewController`고 새 클래스를 추가 하 고 호출 합니다. 

    [![](extensions-images/code01.png "DaysRemaining 프로젝트를 선택 하 고 새 클래스를 추가 하 고 CodeBasedViewController를 호출 합니다.")](extensions-images/code01.png#lightbox)
2. 다시 **솔루션 탐색기**에서 확장 `Info.plist` 파일을 두 번 클릭 하 여 편집용으로 엽니다. 

    [![](extensions-images/code02.png "확장명 info.plist 파일을 두 번 클릭 하 여 편집용으로 엽니다.")](extensions-images/code02.png#lightbox)
3. 화면 맨 아래에서 **원본 뷰** 를 선택 하 고 노드를 `NSExtension` 엽니다. 

    [![](extensions-images/code03.png "화면 아래쪽에서 원본 뷰를 선택 하 고 N이상 압력 노드를 엽니다.")](extensions-images/code03.png#lightbox)
4. 키를 `NSExtensionMainStoryboard` 제거 하 고 값 `NSExtensionPrincipalClass` `CodeBasedViewController`이 인을 추가 합니다. 

    [![](extensions-images/code04.png "NSExtensionMainStoryboard 키를 제거 하 고 값이 CodeBasedViewController 인 NSExtensionPrincipalClass를 추가 합니다.")](extensions-images/code04.png#lightbox)
5. 변경 내용을 저장합니다.

그런 다음 `CodeBasedViewController.cs` 파일을 편집 하 여 다음과 같이 만듭니다.

```csharp
using System;
using Foundation;
using UIKit;
using NotificationCenter;
using CoreGraphics;

namespace DaysRemaining
{
    [Register("CodeBasedViewController")]
    public class CodeBasedViewController : UIViewController, INCWidgetProviding
    {
        public CodeBasedViewController ()
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Add label to view
            var TodayMessage = new UILabel (new CGRect (0, 0, View.Frame.Width, View.Frame.Height)) {
                TextAlignment = UITextAlignment.Center
            };

            View.AddSubview (TodayMessage);
            
            // Insert code to power extension here...

        }
    }
}
```

는 `[Register("CodeBasedViewController")]` 위의`NSExtensionPrincipalClass` 에 대해 지정한 값과 일치 합니다.

### <a name="coding-the-extension"></a>확장 코딩

사용자 인터페이스를 만든 후에는 위의 사용자 `TodayViewController.cs` 인터페이스를 `CodeBasedViewController.cs` 만드는 데 사용 되는 방법에 따라 또는 파일을 열고 **ViewDidLoad** 메서드를 변경 하 고 다음과 같이 표시 합니다.

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Calculate the values
    var dayOfYear = DateTime.Now.DayOfYear;
    var leapYearExtra = DateTime.IsLeapYear (DateTime.Now.Year) ? 1 : 0;
    var daysRemaining = 365 + leapYearExtra - dayOfYear;

    // Display the message
    if (daysRemaining == 1) {
        TodayMessage.Text = String.Format ("Today is day {0}. There is one day remaining in the year.", dayOfYear);
    } else {
        TodayMessage.Text = String.Format ("Today is day {0}. There are {1} days remaining in the year.", dayOfYear, daysRemaining);
    }
}
```

코드 기반 사용자 인터페이스 메서드를 사용 하는 경우 `// Insert code to power extension here...` 주석을 위의 새 코드로 바꿉니다. 기본 구현을 호출 하 고 코드 기반 버전에 대 한 레이블을 삽입 한 후에는이 코드가 간단한 계산을 수행 하 여 해당 연도와 남은 일 수를 가져옵니다. 그런 다음 UI 디자인에서 만든 레이블 (`TodayMessage`)에 메시지를 표시 합니다.

응용 프로그램을 작성 하는 일반적인 프로세스와 유사한 프로세스를 확인 합니다. 확장 프로그램의 `UIViewController` 수명 주기는 앱의 보기 컨트롤러와 동일 합니다. 단, 확장에는 백그라운드 모드가 없고 사용자가 해당 확장을 사용 하는 경우 일시 중단 되지 않습니다. 대신 확장이 반복적으로 초기화 되 고 필요에 따라 할당 취소 됩니다.

### <a name="creating-the-container-app-user-interface"></a>컨테이너 앱 사용자 인터페이스 만들기

이 연습에서 컨테이너 앱은 단순히 확장을 제공 하 고 설치 하는 방법으로 사용 되며 자체의 기능을 제공 하지 않습니다. TodayContainer `Main.storyboard` 파일을 편집 하 고 확장 프로그램의 기능을 정의 하는 일부 텍스트를 추가 하 고이를 설치 하는 방법을 추가 합니다.

[![](extensions-images/today10.png "TodayContainers 주 storyboard 파일을 편집 하 고 Extensions 함수를 정의 하는 텍스트와이를 설치 하는 방법을 추가 합니다.")](extensions-images/today10.png#lightbox)

스토리 보드에 대 한 변경 내용을 저장 합니다.

### <a name="testing-the-extension"></a>확장 테스트

IOS 시뮬레이터에서 확장을 테스트 하려면 **TodayContainer** 앱을 실행 합니다. 컨테이너의 주 뷰가 표시 됩니다.

[![](extensions-images/run01.png "컨테이너 주 뷰가 표시 됩니다.")](extensions-images/run01.png#lightbox)

그런 다음 시뮬레이터의 **홈** 단추를 누르고 화면 맨 위에서 아래로 살짝 밀어 **알림 센터**를 열고 **오늘** 탭을 선택 하 고 **편집** 단추를 클릭 합니다.

[![](extensions-images/run02.png "시뮬레이터의 홈 단추를 누르고 화면 맨 위에서 아래로 살짝 밀어 알림 센터를 열고 오늘 탭을 선택 하 고 편집 단추를 클릭 합니다.")](extensions-images/run02.png#lightbox)

**오늘** 보기에 **DaysRemaining** 확장을 추가 하 고 **완료** 단추를 클릭 합니다.

[![](extensions-images/run03.png "오늘 보기에 DaysRemaining 확장을 추가 하 고 완료 단추를 클릭 합니다.")](extensions-images/run03.png#lightbox)

새 위젯이 **오늘** 보기에 추가 되 고 결과가 표시 됩니다.

[![](extensions-images/run04.png "새 위젯이 오늘 보기에 추가 되 고 결과가 표시 됩니다.")](extensions-images/run04.png#lightbox)

## <a name="communicating-with-the-host-app"></a>호스트 앱과 통신

위에서 만든 예제 오늘 확장은 호스트 앱 ( **오늘** 화면)과 통신 하지 않습니다. 이 경우 `TodayViewController` 또는 `CodeBasedViewController` 클래스의 [extensioncontext](xref:Foundation.NSExtensionContext) 속성을 사용 합니다. 

호스트 앱에서 데이터를 수신 하는 확장의 경우 데이터 `UIViewController`는 확장의 [Extensioncontext](xref:Foundation.NSExtensionContext) 의 [Inputitems](xref:Foundation.NSExtensionContext.InputItems) 속성에 저장 된 [NSExtensionItem](xref:Foundation.NSExtensionItem) 개체의 배열 형식입니다.

사진 편집 확장과 같은 다른 확장 프로그램은 사용자를 완료 하거나 사용 취소 하는 것을 구분할 수 있습니다. 이는 [Extensioncontext](xref:Foundation.NSExtensionContext) 속성의 [CompleteRequest](xref:Foundation.NSExtensionContext.CompleteRequest*) 및 [cancelrequest](xref:Foundation.NSExtensionContext.CancelRequest*) 메서드를 통해 호스트 앱으로 다시 신호를 받습니다.

자세한 내용은 Apple의 [앱 확장 프로그래밍 가이드](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW1)를 참조 하세요.

## <a name="communicating-with-the-parent-app"></a>부모 앱과 통신

앱 그룹을 사용하면 서로 다른 애플리케이션(또는 애플리케이션과 해당 확장 프로그램)이 공유 파일 저장소 위치에 액세스할 수 있습니다. 앱 그룹은 다음과 같은 데이터에 사용할 수 있습니다.

- [Apple Watch 설정](~/ios/watchos/app-fundamentals/settings.md).
- [공유 NSUserDefaults](~/ios/app-fundamentals/user-defaults.md).
- [공유 파일](~/ios/watchos/app-fundamentals/parent-app.md#files).

자세한 내용은 **기능 사용** 설명서의 [앱 그룹](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) 섹션을 참조 하세요.

## <a name="mobilecoreservices"></a>MobileCoreServices

확장 작업을 수행할 때 UTI (Uniform Type Identifier)를 사용 하 여 앱, 다른 앱 및/또는 서비스 간에 교환 되는 데이터를 만들고 조작 합니다.

정적 `MobileCoreServices.UTType` 클래스는 Apple의 `kUTType...` 정의와 관련 된 다음과 같은 도우미 속성을 정의 합니다.

- `kUTTypeAlembic` - `Alembic`
- `kUTTypeAliasFile` - `AliasFile`
- `kUTTypeAliasRecord` - `AliasRecord`
- `kUTTypeAppleICNS` - `AppleICNS`
- `kUTTypeAppleProtectedMPEG4Audio` - `AppleProtectedMPEG4Audio`
- `kUTTypeAppleProtectedMPEG4Video` - `AppleProtectedMPEG4Video`
- `kUTTypeAppleScript` - `AppleScript`
- `kUTTypeApplication` - `Application`
- `kUTTypeApplicationBundle` - `ApplicationBundle`
- `kUTTypeApplicationFile` - `ApplicationFile`
- `kUTTypeArchive` - `Archive`
- `kUTTypeAssemblyLanguageSource` - `AssemblyLanguageSource`
- `kUTTypeAudio` - `Audio`
- `kUTTypeAudioInterchangeFileFormat` - `AudioInterchangeFileFormat`
- `kUTTypeAudiovisualContent` - `AudiovisualContent`
- `kUTTypeAVIMovie` - `AVIMovie`
- `kUTTypeBinaryPropertyList` - `BinaryPropertyList`
- `kUTTypeBMP` - `BMP`
- `kUTTypeBookmark` - `Bookmark`
- `kUTTypeBundle` - `Bundle`
- `kUTTypeBzip2Archive` - `Bzip2Archive`
- `kUTTypeCalendarEvent` - `CalendarEvent`
- `kUTTypeCHeader` - `CHeader`
- `kUTTypeCommaSeparatedText` - `CommaSeparatedText`
- `kUTTypeCompositeContent` - `CompositeContent`
- `kUTTypeConformsToKey` - `ConformsToKey`
- `kUTTypeContact` - `Contact`
- `kUTTypeContent` - `Content`
- `kUTTypeCPlusPlusHeader` - `CPlusPlusHeader`
- `kUTTypeCPlusPlusSource` - `CPlusPlusSource`
- `kUTTypeCSource` - `CSource`
- `kUTTypeData` - `Database`
- `kUTTypeDelimitedText` - `DelimitedText`
- `kUTTypeDescriptionKey` - `DescriptionKey`
- `kUTTypeDirectory` - `Directory`
- `kUTTypeDiskImage` - `DiskImage`
- `kUTTypeElectronicPublication` - `ElectronicPublication`
- `kUTTypeEmailMessage` - `EmailMessage`
- `kUTTypeExecutable` - `Executable`
- `kUTExportedTypeDeclarationsKey` - `ExportedTypeDeclarationsKey`
- `kUTTypeFileURL` - `FileURL`
- `kUTTypeFlatRTFD` - `FlatRTFD`
- `kUTTypeFolder` - `Folder`
- `kUTTypeFont` - `Font`
- `kUTTypeFramework` - `Framework`
- `kUTTypeGIF` - `GIF`
- `kUTTypeGNUZipArchive` - `GNUZipArchive` 
- `kUTTypeHTML` - `HTML`
- `kUTTypeICO` - `ICO`
- `kUTTypeIconFileKey` - `IconFileKey`
- `kUTTypeIdentifierKey` - `IdentifierKey`
- `kUTTypeImage` - `Image`
- `kUTImportedTypeDeclarationsKey` - `ImportedTypeDeclarationsKey`
- `kUTTypeInkText` - `InkText`
- `kUTTypeInternetLocation` - `InternetLocation`
- `kUTTypeItem` - `Item`
- `kUTTypeJavaArchive` - `JavaArchive`
- `kUTTypeJavaClass` - `JavaClass`
- `kUTTypeJavaScript` - `JavaScript`
- `kUTTypeJavaSource` - `JavaSource`
- `kUTTypeJPEG` - `JPEG`
- `kUTTypeJPEG2000` - `JPEG2000`
- `kUTTypeJSON` - `JSON`
- `kUTType3dObject` - `k3dObject`
- `kUTTypeLivePhoto` - `LivePhoto`
- `kUTTypeLog` - `Log` 
- `kUTTypeM3UPlaylist` - `M3UPlaylist`
- `kUTTypeMessage` - `Message`
- `kUTTypeMIDIAudio` - `MIDIAudio`
- `kUTTypeMountPoint` - `MountPoint`
- `kUTTypeMovie` - `Movie`
- `kUTTypeMP3` - `MP3`
- `kUTTypeMPEG` - `MPEG`
- `kUTTypeMPEG2TransportStream` - `MPEG2TransportStream`
- `kUTTypeMPEG2Video` - `MPEG2Video`
- `kUTTypeMPEG4` - `MPEG4`
- `kUTTypeMPEG4Audio` - `MPEG4Audio`
- `kUTTypeObjectiveCPlusPlusSource` - `ObjectiveCPlusPlusSource`
- `kUTTypeObjectiveCSource` - `ObjectiveCSource`
- `kUTTypeOSAScript` - `OSAScript`
- `kUTTypeOSAScriptBundle` - `OSAScriptBundle`
- `kUTTypePackage` - `Package`
- `kUTTypePDF` - `PDF`
- `kUTTypePerlScript` - `PerlScript`
- `kUTTypePHPScript` - `PHPScript`
- `kUTTypePICT` - `PICT`
- `kUTTypePKCS12` - `PKCS12`
- `kUTTypePlainText` - `PlainText`
- `kUTTypePlaylist` - `Playlist`
- `kUTTypePluginBundle` - `PluginBundle`
- `kUTTypePNG` - `PNG`
- `kUTTypePolygon` - `Polygon`
- `kUTTypePresentation` - `Presentation`
- `kUTTypePropertyList` - `PropertyList`
- `kUTTypePythonScript` - `PythonScript`
- `kUTTypeQuickLookGenerator` - `QuickLookGenerator`
- `kUTTypeQuickTimeImage` - `QuickTimeImage`
- `kUTTypeQuickTimeMovie` - `QuickTimeMovie` 
- `kUTTypeRawImage` - `RawImage`
- `kUTTypeReferenceURLKey` - `ReferenceURLKey`
- `kUTTypeResolvable` - `Resolvable`
- `kUTTypeRTF` - `RTF`
- `kUTTypeRTFD` - `RTFD`
- `kUTTypeRubyScript` - `RubyScript`
- `kUTTypeScalableVectorGraphics` - `ScalableVectorGraphics`
- `kUTTypeScript` - `Script`
- `kUTTypeShellScript` - `ShellScript`
- `kUTTypeSourceCode` - `SourceCode`
- `kUTTypeSpotlightImporter` - `SpotlightImporter`
- `kUTTypeSpreadsheet` - `Spreadsheet`
- `kUTTypeStereolithography` - `Stereolithography`
- `kUTTypeSwiftSource` - `SwiftSource`
- `kUTTypeSymLink` - `SymLink`
- `kUTTypeSystemPreferencesPane` - `SystemPreferencesPane`
- `kUTTypeTabSeparatedText` - `TabSeparatedText`
- `kUTTagClassFilenameExtension` - `TagClassFilenameExtension`
- `kUTTagClassMIMEType` - `TagClassMIMEType`
- `kUTTypeTagSpecificationKey` - `TagSpecificationKey`
- `kUTTypeText` - `Text`
- `kUTType3DContent` - `ThreeDContent`
- `kUTTypeTIFF` - `TIFF`
- `kUTTypeToDoItem` - `ToDoItem`
- `kUTTypeTXNTextAndMultimediaData` - `TXNTextAndMultimediaData`
- `kUTTypeUniversalSceneDescription` - `UniversalSceneDescription`
- `kUTTypeUnixExecutable` - `UnixExecutable`
- `kUTTypeURL` - `URL` 
- `kUTTypeURLBookmarkData` - `URLBookmarkData`
- `kUTTypeUTF16ExternalPlainText` - `UTF16ExternalPlainText`
- `kUTTypeUTF16PlainText` - `UTF16PlainText`
- `kUTTypeUTF8PlainText` - `UTF8PlainText`
- `kUTTypeUTF8TabSeparatedText` - `UTF8TabSeparatedText`
- `kUTTypeVCard` - `VCard`
- `kUTTypeVersionKey` - `VersionKey` 
- `kUTTypeVideo` - `Video` 
- `kUTTypeVolume` - `Volume` 
- `kUTTypeWaveformAudio` - `WaveformAudio`
- `kUTTypeWebArchive` - `WebArchive`
- `kUTTypeWindowsExecutable` - `WindowsExecutable`
- `kUTTypeX509Certificate` - `X509Certificate`
- `kUTTypeXML` - `XML`
- `kUTTypeXMLPropertyList` - `XMLPropertyList`
- `kUTTypeXPCService` - `XPCService`
- `kUTTypeZipArchive` - `ZipArchive`

다음 예제를 참조하십시오.

```csharp
using MobileCoreServices;
...

NSItemProvider itemProvider = new NSItemProvider ();
itemProvider.LoadItem(UTType.PropertyList ,null, (item, err) => {
    if (err == null) {
        NSDictionary results = (NSDictionary )item;
        NSString baseURI =
results.ObjectForKey("NSExtensionJavaScriptPreprocessingResultsKey");
    }
});
```

자세한 내용은 **기능 사용** 설명서의 [앱 그룹](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) 섹션을 참조 하세요.

## <a name="precautions-and-considerations"></a>예방 조치 및 고려 사항

확장은 앱이 수행 하는 것 보다 훨씬 더 많은 메모리를 사용할 수 있습니다. 사용자와 사용자가 호스트 하는 앱에 대 한 최소한의 침입으로 신속 하 게 수행 되어야 합니다. 그러나 확장은 사용자가 자신이 속한 확장의 개발자 또는 컨테이너 앱을 식별할 수 있도록 하는 브랜드 UI를 사용 하 여 사용 하는 앱에 유용한 특수 기능을 제공 해야 합니다.

이러한 엄격한 요구 사항을 고려 하 여 철저 하 게 테스트 되 고 성능 및 메모리 사용을 위해 최적화 된 확장만 배포 해야 합니다. 

## <a name="summary"></a>요약

이 문서에는 확장, 확장 지점의 유형 및 iOS에서 확장에 적용 되는 알려진 제한 사항이 포함 되어 있습니다. 확장 및 확장 수명 주기 생성, 배포, 설치 및 실행에 대해 설명 했습니다. 스토리 보드 또는 코드를 사용 하 여 위젯의 UI를 만드는 두 가지 방법을 보여 주는 간단한 **오늘** 위젯을 만드는 연습을 제공 했습니다. IOS 시뮬레이터에서 확장을 테스트 하는 방법을 살펴보았습니다. 마지막으로, 호스트 앱과의 통신에 대해 간략하게 설명 하 고 확장을 개발할 때 고려해 야 하는 몇 가지 예방 조치와 고려 사항을 간략하게 설명 합니다. 

## <a name="related-links"></a>관련 링크

- [ContainerApp (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/intro-to-extensions)
- [Xamarin.ios에서 확장 만들기 (비디오)](https://university.xamarin.com/lightninglectures/creating-extensions-in-ios)
