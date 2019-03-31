---
title: iOS Xamarin.iOS에서 확장
description: 이 문서에는 알림 센터 내에서 같은 표준 컨텍스트에서 iOS에서 제공 하는 위젯 확장을 설명 합니다. 확장을 만들고 부모 앱에서 상호 통신 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 3DEB3D43-3E4A-4099-8331-93C1E7A77095
ms.technology: xamarin-ios
ms.custom: xamu-video
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: b21bf4da7cf862bd32e71708f9e3657f577682c2
ms.sourcegitcommit: 946ce514fd6575aa6b93ff24181e02a60b24b106
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58677926"
---
# <a name="ios-extensions-in-xamarinios"></a>Xamarin.iOS의 iOS 확장

> [!VIDEO https://youtube.com/embed/Sd0-ch9Udmk]

**ios에서 확장을 만들어 [Xamarin University](https://university.xamarin.com/)**

확장을 iOS 8에에서 도입 된 특수화 된 `UIViewControllers` 하 여 표시 되는 표준 컨텍스트 내에서 iOS 등으로 내 합니다 **알림 센터**처럼 특수화 하는 데 사용자가 요청 하는 사용자 지정 키보드 형식 입력 또는 다른 컨텍스트 확장 필터 특수 효과 제공할 수 있는 사진 편집을 선호 합니다.

모든 확장 (와 함께 64 비트 통합 Api를 사용 하 여 기록 되는 두 요소) 컨테이너 앱을 함께 설치 되 고 호스트 응용 프로그램에서 특정 확장 지점에서 활성화 됩니다. 하며 하므로 기존 시스템 함수에 대 한 조항은로 사용할 것인지, 고성능, 린 (lean), 및 강력한 여야 합니다. 

## <a name="extension-points"></a>확장 지점

|형식|설명|확장 지점|호스트 응용 프로그램|
|--- |--- |--- |--- |
|작업|특수 편집기 또는 특정 미디어 유형에 대 한 뷰어|`com.apple.ui-services`|임의의 값|
|문서 공급자|앱 원격 문서 저장소를 사용할 수 있도록 허용|`com.apple.fileprovider-ui`|사용 하 여 앱을 [UIDocumentPickerViewController](xref:UIKit.UIDocumentPickerViewController)|
|키보드|대체 키보드|`com.apple.keyboard-service`|임의의 값|
|사진 편집|사진 조작 및 편집|`com.apple.photo-editing`|Photos.app 편집기|
|공유|소셜 네트워크, 메시징 등 서비스를 사용 하 여 데이터를 공유 합니다.|`com.apple.share-services`|임의의 값|
|오늘|알림 센터를 현재 화면에 표시 되는 "widget"|`com.apple.widget-extensions`|오늘 및 알림 센터|

[추가 확장 지점](~/ios/platform/introduction-to-ios10/index.md#app-extensions) iOS 10에에서 추가 되었습니다.

## <a name="limitations"></a>제한 사항

확장 수 제한 사항 중 일부는 모든 형식에 유니버설 (확장의 형식이 없는 경우 카메라 또는 마이크에 액세스할 수 있음)의 다른 형식 확장과 용도 (예를 들어, 사용자 지정 키보드에서 특정 제한 될 수 있지만 사용할 수 없습니다 암호와 같은 보안 데이터 항목 필드에 대 한). 

유니버설 제한은 다음과 같습니다.

- 합니다 [상태 키트](~/ios/platform/healthkit.md) 하 고 [이벤트 키트 UI](~/ios/platform/eventkit.md) 프레임 워크를 사용할 수 없는 경우
- 확장에서 사용할 수 없습니다 [백그라운드 모드를 확장 합니다.](https://developer.xamarin.com/guides/cross-platform/application_fundamentals/backgrounding/part_3_ios_backgrounding_techniques/registering_applications_to_run_in_background/)
- (하지만 기존 미디어 파일에 액세스할 수) 확장 장치 카메라 또는 마이크에 액세스할 수 없습니다.
- 확장 (공기 삭제를 통해 데이터를 전송할 수 있습니다) 하지만 공기 삭제 데이터를 받을 수 없습니다.
- [UIActionSheet](xref:UIKit.UIActionSheet) 하 고 [UIAlertView](xref:UIKit.UIAlertView) 를 사용할 수 없습니다; 확장을 사용 해야 [UIAlertController](xref:UIKit.UIAlertController)
- 몇몇 구성원이 [UIApplication](xref:UIKit.UIApplication) 를 사용할 수 없습니다. [UIApplication.SharedApplication](xref:UIKit.UIApplication.SharedApplication)하십시오 [UIApplication.OpenUrl](xref:UIKit.UIApplication.OpenUrl(Foundation.NSUrl))하십시오 [UIApplication.BeginIgnoringInteractionEvents](xref:UIKit.UIApplication.BeginIgnoringInteractionEvents) 고 [ UIApplication.EndIgnoringInteractionEvents](xref:UIKit.UIApplication.EndIgnoringInteractionEvents)
- iOS는 오늘날의 확장에 16MB 메모리 사용량 한계를 적용합니다.
- 기본적으로 키보드 확장에는 네트워크에 액세스를 권한이 없습니다. 이 영향을 줍니다 (제한을 시뮬레이터에는 적용 되지 않습니다) 장치에서 디버깅 Xamarin.iOS 디버깅을 수행 하려면 네트워크 액세스를 필요 하기 때문입니다. 설정 하 여 네트워크 액세스를 요청 하는 합니다 `Requests Open Access` 값을 프로젝트의 Info.plist에 `Yes`입니다. Apple의를 참조 하세요 [사용자 지정 키보드 가이드](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html) 키보드 확장 제한에 대 한 자세한 내용은 합니다.

개별 제한 사항에 대 한 Apple의를 참조 하세요 [앱 확장의 프로그래밍 가이드](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/)합니다.

## <a name="distributing-installing-and-running-extensions"></a>배포, 설치 및 확장 실행

확장은 다시 전송 하 고 분산 앱 스토어를 통해 컨테이너 앱 내에서 배포 됩니다. 앱을 사용 하 여 분산 확장명 해당 시점에 설치 되지만 사용자는 각 확장을 명시적으로 사용 해야 합니다. 다양 한 유형의 확장은 다양 한 방법으로;에서 사용 하도록 설정 로 이동 해야 할 몇 가지는 **설정을** 앱에서에서 사용 하도록 설정 합니다. 하지만 다른 보내면 사진을 공유 확장을 사용 하도록 설정 하는 등의 사용 시점에 활성화 됩니다. 

앱 확장을 사용 하는 (여기서 발생 하는 확장 지점) 라고 합니다 **호스트 앱**실행 될 때 확장을 호스트 하는 앱 이므로, 합니다. 확장을 설치 하는 앱은는 **컨테이너 앱**설치 된 경우 확장을 포함 하는 앱 이므로, 합니다.  

일반적으로 컨테이너 앱 확장에 설명 하 고 사용 하도록 설정 하는 과정을 안내 합니다.

## <a name="extension-lifecycle"></a>확장 수명 주기

확장 단일 처럼 간단할 수 있습니다 [UIViewController](xref:UIKit.UIViewController) 또는 UI의 여러 화면을 제공 하는 더 복잡 한 확장입니다. 발견할 경우에 사용자를 _확장점_ (때와 같이 이미지를 공유), 해당 확장 지점에 대해 등록 된 확장을 선택할 수 있는 기회를 갖게 됩니다. 

앱 중 하나 선택 하는 경우의 확장을 해당 `UIViewController` 를 인스턴스화할 수 및 일반 뷰 컨트롤러 수명 주기를 시작 합니다. 그러나 일시 중단 하지만 일반적으로 사용자가 상호 작용을 마치면 종료, 일반 앱와 달리 확장은 로드, 실행 및을 반복적으로 종료 합니다.

확장을 통해 앱을 해당 호스트와 통신할 수 있는 [NSExtensionContext](xref:Foundation.NSExtensionContext) 개체입니다. 일부 확장에는 결과 사용 하 여 비동기 콜백을 수신 하는 작업이 있습니다. 이러한 콜백을 백그라운드 스레드에서 실행 되 고 확장이 고려해 야 합니다. 사용 하 여 예를 들어 [NSObject.InvokeOnMainThread](xref:Foundation.NSObject.InvokeOnMainThread*) 사용자 인터페이스를 업데이트 하는 경우. 참조 된 [호스트 앱을 사용 하 여 통신](#communicating-with-the-host-app) 대 한 자세한 내용은 아래 섹션입니다.

기본적으로 확장 및 해당 컨테이너 앱 수 통신 하지 않습니다, 함께 설치 되 고 불구 하 고. 경우에 따라 컨테이너 앱은 기본적으로 빈 "shipping" 컨테이너 확장이 설치 되 면 해당 용도로 제공 됩니다. 그러나 상황에 따라 필요 하는 경우 컨테이너 앱 및 확장 공유할 수 있습니다 일반 영역에서 리소스. 또한 한 **오늘 확장** 는 URL을 열 수는 컨테이너 앱을 요청할 수 있습니다. 이 동작에 표시 됩니다는 [카운트다운 위젯 발전](https://github.com/xamarin/monotouch-samples/tree/master/ExtensionsDemo)합니다.

## <a name="creating-an-extension"></a>확장 만들기

확장 (및 해당 컨테이너 앱) 64 비트 이진 파일 이어야 합니다 하 고는 Xamarin.iOS를 사용 하 여 빌드된 [Unified Api](https://developer.xamarin.com/guides/cross-platform/macios/unified)합니다. 확장을 개발 하는 경우, 솔루션 적어도 두 개의 프로젝트가 포함 됩니다: 각 확장의 컨테이너 제공에 대 한 컨테이너 앱 및 프로젝트입니다. 

### <a name="container-app-project-requirements"></a>컨테이너 앱 프로젝트 요구 사항

컨테이너 앱 확장을 설치 하는 데에 다음 요구 사항을 있습니다.

- 이 확장 프로젝트에 대 한 참조를 유지 해야 합니다.   
- (시작 하 고 성공적으로 실행 하는 일을 할 수 있어야) 앱을 완료 해야이 아무 작업도 수행 하지 보다 확장을 설치 하는 방법을 제공 하는 경우에 합니다. 
- 확장 프로그램의 번들 식별자에 대 한 기반이 되는 번들 식별자 있어야 합니다 (자세한 내용은 아래 섹션 참조) 프로젝트.

### <a name="extension-project-requirements"></a>확장 프로젝트 요구 사항

또한 확장의 프로젝트에 다음 요구 사항:

- 해당 컨테이너 앱의 번들 식별자로 시작 하는 번들 식별자 있어야 합니다. 예를 들어 컨테이너 앱의 번들 식별자가 `com.myCompany.ContainerApp`, 확장의 식별자 `com.myCompany.ContainerApp.MyExtension`: 

    ![](extensions-images/bundleidentifiers.png) 
- 키를 정의 해야 합니다 `NSExtensionPointIdentifier`을 적절 한 값 (같은 `com.apple.widget-extension` 에 대 한를 **지금** 알림 센터 위젯)에서 해당 `Info.plist` 파일입니다.
- 도 정의 해야 합니다 *중 하나* 는 `NSExtensionMainStoryboard` 키 또는 `NSExtensionPrincipalClass` 키에 해당 `Info.plist` 적절 한 값을 사용 하 여 파일:
    - 사용 된 `NSExtensionMainStoryboard` 확장에 대 한 기본 UI를 제공 하는 스토리 보드의 이름을 지정 하는 키 (빼기 `.storyboard`). 예를 들어 `Main` 에 대 한는 `Main.storyboard` 파일입니다.
    - 사용 된 `NSExtensionPrincipalClass` 키 확장 시작 될 때 초기화 될 클래스를 지정 합니다. 값이 일치 해야 합니다 **등록** 의 값에 `UIViewController`: 

    ![](extensions-images/registerandprincipalclass.png)

특정 유형의 확장에는 추가 요구 사항이 있을 수 있습니다. 예를 들어를 **오늘** 또는 **알림 센터** 확장의 보안 주체 클래스를 구현 해야 [INCWidgetProviding](https://developer.xamarin.com/api/type/NotificationCenter.INCWidgetProviding/)합니다.

> [!IMPORTANT]
> Mac 용 Visual Studio에서 제공 되는 확장 템플릿을 하나를 사용 하 여 프로젝트를 시작 하는 경우 (모두는 아님) 경우에 대부분의 이러한 요구 사항은 제공 되며 템플릿에서 자동으로를 충족 합니다.

## <a name="walkthrough"></a>연습 

다음 연습에서는 예로 만들어집니다 **오늘** 일 및 연도에 남은 일 수를 계산 하는 위젯:

[![](extensions-images/carpediemscreenshot-sm.png "일 및 연도에 남은 일 수를 계산 하는 예에서는 오늘 위젯")](extensions-images/carpediemscreenshot.png#lightbox)

### <a name="creating-the-solution"></a>솔루션 만들기

필요한 솔루션을 만들려면 다음을 수행 합니다.

1. 먼저 새 iOS를 만듭니다 **단일 뷰 앱** 프로젝트을 클릭 합니다 **다음** 단추: 

    [![](extensions-images/today01.png "먼저 새 iOS, 단일 뷰 앱 프로젝트 만들기 및 다음 단추를 클릭 합니다.")](extensions-images/today01.png#lightbox)
2. 프로젝트를 호출 합니다 `TodayContainer` 을 클릭 합니다 **다음** 단추: 

    [![](extensions-images/today02.png "TodayContainer 프로젝트를 호출 하 고 단추를 클릭 합니다.")](extensions-images/today02.png#lightbox)
3. 확인 합니다 **프로젝트 이름** 및 **SolutionName** 을 클릭 합니다 **만들기** 솔루션을 만드는 단추: 

    [![](extensions-images/today03.png "프로젝트 이름 및 SolutionName을 확인 하 고 솔루션을 만들려면 만들기 단추를 클릭")](extensions-images/today03.png#lightbox)
4. 다음으로 **솔루션 탐색기**솔루션을 마우스 오른쪽 단추로 클릭 하 고 새 **iOS 확장** 에서 프로젝트를 **오늘 확장** 템플릿: 

    [![](extensions-images/today04.png "다음으로, 솔루션 탐색기에서 솔루션을 마우스 오른쪽 단추로 클릭 하 고 오늘 확장 템플릿에서 새 iOS 확장 프로젝트에 추가")](extensions-images/today04.png#lightbox)
5. 프로젝트를 호출 합니다 `DaysRemaining` 을 클릭 합니다 **다음** 단추: 

    [![](extensions-images/today05.png "DaysRemaining 프로젝트를 호출 하 고 단추를 클릭 합니다.")](extensions-images/today05.png#lightbox)
6. 프로젝트를 검토 하 고 클릭 합니다 **만들기** 단추를 만듭니다. 

    [![](extensions-images/today06.png "프로젝트를 검토 하 고 만들려면 만들기 단추를 클릭")](extensions-images/today06.png#lightbox)

결과 솔루션 아래 그림과 같이 이제 두 개의 프로젝트가 있습니다.

[![](extensions-images/today07.png "다음과 같이 결과 솔루션 프로젝트가 이제 해야")](extensions-images/today07.png#lightbox)

### <a name="creating-the-extension-user-interface"></a>확장 사용자 인터페이스 만들기

에 대 한 인터페이스를 디자인 해야 하는 다음으로 프로그램 **오늘** 위젯입니다. 수행할 수 있습니다 하거나 스토리 보드를 사용 하 여 또는 코드에서 UI를 만들어 합니다. 두 방법 모두 아래 자세히 설명 합니다.

#### <a name="using-storyboards"></a>스토리 보드를 사용 하 여

스토리 보드를 사용 하 여 UI를 빌드하려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**, 확장 프로젝트를 두 번 클릭 `Main.storyboard` 파일을 편집용으로 엽니다. 

    [![](extensions-images/today08.png "확장 프로젝트 Main.storyboard 파일 편집에 대 한 열을 두 번 클릭")](extensions-images/today08.png#lightbox)
2. 템플릿에서 UI에 자동으로 추가 된 레이블을 선택 하 고 지정는 **이름** `TodayMessage` 에 **위젯** 탭을 **속성 탐색기**: 

    [![](extensions-images/today09.png "템플릿에서 UI에 자동으로 추가 된 레이블을 선택 하 고 속성 탐색기의 위젯 탭에서 이름을 TodayMessage 지정")](extensions-images/today09.png#lightbox)
3. 스토리 보드에 변경 내용을 저장 합니다.

#### <a name="using-code"></a>코드를 사용 하 여

코드에서 UI를 빌드하려면 다음을 수행 합니다. 

1. 에 **솔루션 탐색기**를 선택 합니다 **DaysRemaining** 프로젝트에서 새 클래스를 추가 하 고 호출 `CodeBasedViewController`: 

    [![](extensions-images/code01.png "Aelect DaysRemaining 프로젝트에 새 클래스를 추가 하 고 CodeBasedViewController 호출")](extensions-images/code01.png#lightbox)
2. 를 다시 합니다 **솔루션 탐색기**, 확장의 두 번 클릭 `Info.plist` 파일을 편집용으로 엽니다. 

    [![](extensions-images/code02.png "확장 Info.plist 파일 편집에 대 한 열을 두 번 클릭")](extensions-images/code02.png#lightbox)
3. 선택 된 **원본 뷰** (화면 맨 아래)에서 연는 `NSExtension` 노드: 

    [![](extensions-images/code03.png "화면 아래쪽에서 소스 보기를 선택 하 고 NSExtension 노드를 열고")](extensions-images/code03.png#lightbox)
4. 제거 합니다 `NSExtensionMainStoryboard` 키를 추가 된 `NSExtensionPrincipalClass` 값을 사용 하 여 `CodeBasedViewController`: 

    [![](extensions-images/code04.png "NSExtensionMainStoryboard 키를 제거 하 고 값이 CodeBasedViewController NSExtensionPrincipalClass 추가")](extensions-images/code04.png#lightbox)
5. 변경 내용을 저장합니다.

다음에 편집을 `CodeBasedViewController.cs` 파일을 다음과 같이 표시 되도록 합니다.

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

`[Register("CodeBasedViewController")]` 에 대해 지정한 값과 일치 하는 `NSExtensionPrincipalClass` 위에 있습니다.

### <a name="coding-the-extension"></a>확장 코딩

만든 사용자 인터페이스를 사용 하 여 열 중 하나는 `TodayViewController.cs` 또는 `CodeBasedViewController.cs` 파일 (위의 사용자 인터페이스를 만드는 데 메서드 기반)을 변경 합니다 **ViewDidLoad** 메서드 수 있으며 다음과 같은 모양:

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

코드를 사용 하 여 사용자 인터페이스 메서드를 기반 하는 경우 대체는 `// Insert code to power extension here...` 위에서 새 코드를 사용 하 여 주석을 합니다. 후 기본 구현을 호출 (및 코드 기반 버전에 대 한 레이블을 삽입)이이 코드는 연도 및 남은 기간 (일)을 가져오려고 간단한 계산을 수행 합니다. 레이블에 메시지를 표시 한 다음 (`TodayMessage`)는 UI 디자인에서 생성 합니다.

앱을 작성 하는 일반적인 프로세스는이 프로세스는 얼마나 유사한 지 note 합니다. 확장의 `UIViewController` 백그라운드 모드에 있지 않은 확장과 사용자 완료 되 면 일시 중단 되지 않습니다 점을 제외 하 고 뷰 컨트롤러를 동일한 수명 주기, 앱에 사용 합니다. 대신 확장 반복적으로 초기화 되 고 필요에 따라 할당을 취소 합니다.

### <a name="creating-the-container-app-user-interface"></a>컨테이너 앱 사용자 인터페이스 만들기

이 연습에서는 컨테이너 앱 제공 및 확장을 설치 하는 방법으로 단순히 사용 됩니다 하 고 자체의 기능을 제공 합니다. TodayContainer의 편집 `Main.storyboard` 파일을 설치 하는 방법 및 확장의 함수를 정의 하는 일부 텍스트를 추가 합니다.

[![](extensions-images/today10.png "TodayContainers Main.storyboard 파일을 편집 하 고 설치 하는 방법과 확장 함수를 정의 하는 일부 텍스트를 추가 합니다.")](extensions-images/today10.png#lightbox)

스토리 보드에 변경 내용을 저장 합니다.

### <a name="testing-the-extension"></a>테스트 확장

IOS 시뮬레이터에서에서 확장 프로그램을 테스트 하려면 다음을 실행 합니다 **TodayContainer** 앱. 컨테이너의 기본 보기 표시 됩니다.

[![](extensions-images/run01.png "컨테이너 기본 보기 표시 됩니다.")](extensions-images/run01.png#lightbox)

다음으로, 적중를 **홈** 를 열려면 화면 맨 위에서 아래쪽으로 살짝 밀어서 시뮬레이터에서 단추를 **알림 센터**를 선택 합니다 **지금** 탭을 클릭 합니다 **편집** 단추:

[![](extensions-images/run02.png "알림 센터를 열고 현재 탭을 선택 하 고 편집 단추를 클릭 하 고 화면 위쪽에서 아래쪽으로 살짝 밀어서 시뮬레이터에서 홈 단추를 누릅니다.")](extensions-images/run02.png#lightbox)

추가 **DaysRemaining** 확장을는 **지금** 를 클릭 합니다 **수행** 단추:

[![](extensions-images/run03.png "오늘 보기에 DaysRemaining 확장을 추가 하 고 완료 단추를 클릭 합니다.")](extensions-images/run03.png#lightbox)

새 위젯을 추가할 수는 **오늘** 보기 및 결과가 표시 됩니다.

[![](extensions-images/run04.png "새 위젯은 오늘 보기에 추가 되 고 결과가 표시 됩니다.")](extensions-images/run04.png#lightbox)

## <a name="communicating-with-the-host-app"></a>호스트 응용 프로그램을 사용 하 여 통신

이 예제에서는 위에서 만든 확장 현재 해당 호스트 앱와 통신 하지 않습니다 (합니다 **오늘** 화면). 사용 하는 경우는 [ExtensionContext](xref:Foundation.NSExtensionContext) 의 속성을 `TodayViewController` 또는 `CodeBasedViewController` 클래스입니다. 

해당 호스트 응용 프로그램에서 데이터를 받을 확장에 대 한 데이터가의 배열의 형태로 [NSExtensionItem](xref:Foundation.NSExtensionItem) 에 저장 된 개체를 [InputItems](xref:Foundation.NSExtensionContext.InputItems) 속성을 [ExtensionContext ](xref:Foundation.NSExtensionContext) 확장의 `UIViewController`합니다.

사진 편집 확장 등의 다른 확장을 완료 하거나 취소 사용 사용자 구분 될 수 있습니다. 이 신호를 받을 통해 호스트 앱으로 다시 합니다 [CompleteRequest](xref:Foundation.NSExtensionContext.CompleteRequest*) 하 고 [CancelRequest](xref:Foundation.NSExtensionContext.CancelRequest*) 의 메서드 [ExtensionContext](xref:Foundation.NSExtensionContext) 속성입니다.

자세한 내용은 Apple의를 참조 하세요 [앱 확장의 프로그래밍 가이드](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW1)합니다.

## <a name="communicating-with-the-parent-app"></a>부모 앱을 사용 하 여 통신

앱 그룹을 사용하면 서로 다른 애플리케이션(또는 애플리케이션과 해당 확장 프로그램)이 공유 파일 저장소 위치에 액세스할 수 있습니다. 앱 그룹은 다음과 같은 데이터에 사용할 수 있습니다.

- [Apple Watch 설정](~/ios/watchos/app-fundamentals/settings.md)합니다.
- [공유 NSUserDefaults](~/ios/app-fundamentals/user-defaults.md)합니다.
- [파일 공유](~/ios/watchos/app-fundamentals/parent-app.md#files)합니다.

자세한 내용은 참조 하십시오는 [앱 그룹](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) 부분 우리의 **기능을 사용 하 여 작업** 설명서.

## <a name="mobilecoreservices"></a>MobileCoreServices

확장을 사용 하는 경우 일정 한 형식 식별자 (UTI)를 사용 하 여 만들고 앱, 다른 앱 및 서비스 간에 교환 되는 데이터를 조작 합니다.

합니다 `MobileCoreServices.UTType` Apple의 관련 된 다음 도우미 속성을 정의 하는 정적 클래스 `kUTType...` 정의:

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

자세한 내용은 참조 하십시오는 [앱 그룹](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) 부분 우리의 **기능을 사용 하 여 작업** 설명서.

## <a name="precautions-and-considerations"></a>주의 사항 및 고려 사항

확장 앱 보다에 사용 가능한 메모리를 훨씬 적은 경우 이러한 신속 하 고 사용자를 앱에서 호스팅되는 최소한의 침입을 사용 하 여 수행 해야 합니다. 그러나 확장 브랜드 확장의 개발자를 식별할 수 있도록 UI 사용 하 여 사용 중인 앱에 속하는 컨테이너 앱을 눈에 띄는 유용한 함수를 제공 해야 합니다.

긴밀 하 게 이러한 요구 사항이 들어 있습니다 철저 하 게 테스트 및 성능 및 메모리 사용에 대 한 최적화 된 확장을만 배포 해야 합니다. 

## <a name="summary"></a>요약

이 문서에서는 이러한 부분이 무엇 인지, 확장 지점 및 ios 확장에 적용 되는 알려진된 제한 사항이 형식의 확장 합니다. 이 설명 만들고, 배포 하 고, 설치 하 고 확장 및 확장 수명 주기를 실행 합니다. 만드는 간단한 연습을 제공 하지 않습니다 **오늘** 위젯: 스토리 보드 또는 코드를 사용 하 여 위젯의 UI를 만드는 두 가지 방법을 표시 합니다. IOS 시뮬레이터에서에서 확장을 테스트 하는 방법에 알아보았습니다. 마지막으로, 해당 간단히 설명한 것 처럼 호스트 앱 및 확장을 개발 하는 경우 수행 해야 하는 고려 사항 몇 가지 예방 조치 디렉터리와 통신 합니다. 

## <a name="related-links"></a>관련 링크

- [ContainerApp (샘플)](https://developer.xamarin.com/samples/monotouch/intro-to-extensions)
- [Xamarin.iOS에서 확장명 (비디오) 만들기](https://university.xamarin.com/lightninglectures/creating-extensions-in-ios)
