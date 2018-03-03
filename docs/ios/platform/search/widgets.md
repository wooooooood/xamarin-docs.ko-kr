---
title: "검색 및 홈 화면 위젯 향상 된 기능"
description: "이 문서에서는 Apple iOS 10에서에서 위젯 시스템에 대가 향상 된 기능을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: D66FD9E1-9E23-4BB6-825C-ED19B8F72A81
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: a6749ca9d8a793372ec088433780d622f2f05b41
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="search-and-home-screen-widget-enhancements"></a>검색 및 홈 화면 위젯 향상 된 기능

_이 문서에서는 Apple iOS 10에서에서 위젯 시스템에 대가 향상 된 기능을 설명 합니다._


Apple에 위젯 10 잠금 화면 새 iOS에 있는 모든 백그라운드에서 잘 한지 확인 하기 위하여 위젯 시스템에 여러 개선 된 기능이 도입 되었습니다. 또한 위젯을 포함 이제는 [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) 얼마나 많은 콘텐츠를 사용할 수를 설명 하는 개발자 및 확장 하 고 콘텐츠를 축소할 수 있도록 하는 속성입니다.

위젯을 (라고도 오늘 Extensions)는 적은 양의 유용한 정보를 표시 하거나 적절 한 시간 내에 응용 프로그램 특정 기능을 노출 하는 iOS 확장의 특별 한 종류입니다. 예를 들어 뉴스 응용 프로그램에 상위 헤드라인을 보여 주는 위젯 및 일정 앱에서는 다른 위젯 두 개가: 오늘날의 이벤트 및 예정 된 이벤트를 표시 하려면 하나를 표시 하려면 하나 있습니다.

위젯 폭넓게 사용자 지정할 수 및 텍스트, 이미지, 단추 등의 UI 요소를 포함할 수 있습니다. 또한 개발자 해당 위젯 레이아웃 추가로 사용자 지정할 수 있습니다.

[ ![](widgets-images/widgets01.png "예제 위젯")](widgets-images/widgets01.png)

사용자 보고는 응용 프로그램의 위젯을 상호 작용할 수 있는 주요 자릿수가 두 가지가 있습니다.

- **검색 화면** -사용자가 검색 화면에 가장 유용한을 찾게 위젯을 추가할 수 있습니다. 검색 화면 홈와 잠금 화면에서 오른쪽을 살짝 밀어 액세스할 수 있습니다.
- **홈 화면** -에서 홈 화면에서 사용자 응용 프로그램의 아이콘에 압력을 적용 하 여 빠른 작업 목록을 열려면 3D 터치를 사용할 수 있습니다. 응용 프로그램의 위젯은 빠른 작업 목록 위에 표시 됩니다. 참조 하십시오 우리의 [3D 터치 소개](~/ios/platform/3d-touch.md) 자세한 정보에 대 한 설명서입니다.

## <a name="widgets-developer-suggestions"></a>위젯 개발자 제안

이상적으로 개발자는 항상 시도 및 사용자는 해당 검색 화면에 추가 하려면 도구를 디자인할 합니다. 이 위해서는 Apple 다음 제안에 있습니다.

- **만들기는 좋은 동적인 경험** -사용자의 원하는 상태 업데이트의 간략 하 고 동적인 정보를 제공 하거나 간단한 작업을 빠르게 수행할 수 있도록 하는 위젯입니다. 따라서 적절 한 양의 정보 및 필수 상호 작용을 제공 합니다. 가능 하면 항상 단일 탭이 있는 지정된 된 태스크를 수행 하려면 사용자를 허용 합니다. 또한 위젯 패닝 또는 스크롤을 지원 하지 않는 이후 위젯의 디자인에 고려해 야 해야 할 됩니다이 합니다.
- **콘텐츠를 신속 하 게 표시** -위젯을 사용자 콘텐츠 위젯을 표시 되 면 로드를 기다릴 필요는 하므로 동적인, 되도록 설계 되었습니다. 위젯에 콘텐츠를 캐시 해야 새 콘텐츠 백그라운드에서 로드 되는 동안 최근의 콘텐츠를 표시할 수 있습니다 하므로 로컬로 합니다.
- **적절 한 안쪽 여백 및 여백을 제공** -위젯 해야 사용 안 함 확인 복잡 합니다, 따라서 콘텐츠는 위젯 보기의 가장자리를 확장 되지 않도록 합니다. 가장자리와 내용 사이의 몇 가지 픽셀 너비 여백 항상 있을 것입니다. 또한이 Apple 맞춤 안내선으로 위젯의 위쪽에 표시 되는 응용 프로그램의 아이콘을 사용 하 여 제안 합니다. 모눈에 있는 항목 사이의 안쪽 여백 적절 한지 확인 위젯을 사용 하면 모눈 레이아웃을 제시 하는 경우 4 개의 최대 항목 수를 제한 하려고 합니다.
- **조정 가능한 레이아웃을 사용 하 여** -A 위젯 너비에서 실행 되는 장치 및 장치의 방향에 따라 달라 집니다. Collapsed (기본값) 또는 확장 (모든 장치에서 지원 되지 않음) 상태에 표시 되는 경우에 위젯의 높이에 따라 달라질 수 있습니다. 축소 된 위젯 약 2.5 표준 iOS 테이블 행의 높이 있습니다. 개발자는 확장 위젯에 대 한 크기를 요청할 수 있지만 가능 해야 합니다. 화면의 높이 보다 작으면. Collapsed 상태의 위젯의 필수, 독립 실행형 정보를 표시 해야 합니다. 때 확장, 위젯의 Collapsed 상태에 표시 되는 기본 콘텐츠를 개선 하는 추가 정보가 표시 됩니다. Collapsed 상태에서 위젯 빠른 작업 목록에 표시 됩니다.
- **위젯의 배경 사용자 지정 하지** -위젯 시스템에서 제공 밝은, 흐리게 표시 된 배경에 표시 됩니다. 이 옵션은 위젯 간 일관성의 수준을 올립니다 및 해당 콘텐츠의 가독성을 개선 하기 위해 수행 됩니다. 사용자의 잠금 및 홈 화면 배경 무늬 충돌할 수 있으므로 위젯 배경으로 이미지를 사용 하지 마십시오.
- **검은색 이나 진한 회색 시스템 글꼴을 사용 하 여** -위젯, 시스템 글꼴 효과적에에서 텍스트를 표시 하는 경우. 글꼴 밝은, 흐리게 표시 된 위젯 배경에 대해 두드러지게 검정 또는 진한 회색 색에 있어야 합니다.
- **그러나 응용 프로그램 액세스 때 적절 한 제공** -응용 프로그램에서 위젯의 별도로 항상 작동 되 면 깊은 기능이 필요한 경우 위젯 수를 보거나 특정 부분의 정보를 편집 하려면 앱을 시작 하려면. 하지 열기 타사 응용 프로그램 및 콘텐츠 자체를 탭 하 고 사용자 허용 단순히 "앱을 열고" 단추를 포함 하지 않습니다.
- **Clear, 간결한 위젯 이름 선택** -응용 프로그램의 아이콘 및 위젯의 이름을 위젯의 콘텐츠에 대해 항상 표시 됩니다. Apple의 기본 장치에 대 한 응용 프로그램의 이름과 다른 제공 명확 하 고 간결한 이름을 사용 하 여 제안 합니다. 사용자 지정 위젯 제목이 제공 하는 경우 응용 프로그램의 이름 (예: 근처 지도, 지도 식당 등)과 접두사로 야 합니다.
- **인증 값을 추가 하는 경우 알릴** -추가 기능 또는 정보는 사용자가 인증 하는 경우에 사용할 수 있으며 사용자에 게이 표시, 서명 하는 경우. 예를 들어 한 안에서 공유 앱 내 말하기 "에 로그인 책 안에서"입니다.
- **빠른 작업 목록 위젯을 선택** -개발자 사용자 압력 3D 터치를 사용 하 여 응용 프로그램의 아이콘을 적용 하 여 빠른 작업 목록을 표시 될 때 표시 하도록 하나를 선택 해야 응용 프로그램을 여러 개 위젯을 제공 합니다.

작업 위젯에 대 한 자세한 내용은 참조 하십시오 우리의 [확장명 소개](~/ios/platform/extensions.md), [3D 터치 소개](~/ios/platform/3d-touch.md) 설명서 및 Apple의 [App 확장 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html).

## <a name="working-with-vibrancy"></a>작업을 표현

표현 하는 위젯 텍스트 위젯의 light, 흐리게 표시 된 배경 (시스템에서 제공)에 나타날 때 읽을 수 유지 되도록 합니다. 개발자 10 iOS 하기 전에 사용 한 [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) 위젯의 표현에 대 한 합니다. 예:

```csharp
// DEPRECATED: Get Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreateForNotificationCenter ();
```

IOS 10에서에서 사용 되지 않습니다는이 사용 하 여 교체 해야 합니다는 [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) 또는 [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect)합니다. 예:

```csharp
// Get Primary Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreatePrimaryVibrancyEffectForNotificationCenter ();

// Get Secondary Widget Vibrancy Effect
var vibrancy2 = UIVibrancyEffect.CreateSecondaryVibrancyEffectForNotificationCenter ();
```

## <a name="working-with-collapsed-and-expanded-widgets"></a>축소 또는 확장 위젯 사용

새로운 iOS 10에 위젯 이제 포함 한 [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) 얼마나 많은 콘텐츠를 사용할 수를 설명 하는 개발자 및 확장 하 고 콘텐츠를 축소할 수 있도록 하는 속성입니다.

위젯은 처음 표시 되 면 Collapsed 상태입니다. 축소 된 위젯 약 2.5 표준 iOS 테이블 행의 높이 있습니다. 개발자는 확장 위젯에 대 한 크기를 요청할 수 있지만 가능 해야 합니다. 화면의 높이 보다 작으면. 

Collapsed 상태의 위젯의 필수, 독립 실행형 정보를 표시 해야 합니다. 때 확장, 위젯의 Collapsed 상태에 표시 되는 기본 콘텐츠를 개선 하는 추가 정보가 표시 됩니다. 예를 들어 Weather app 축소 하면 현재 기상 상태를 표시 하 고 확장할 때 예측 시간별을 추가 합니다.

Collapsed 상태에서 위젯 빠른 작업 목록에 표시 됩니다. 둘 이상의 위젯을 제공 하는 응용 프로그램 개발자 사용자 압력 3D 터치를 사용 하 여 응용 프로그램의 아이콘을 적용 하 여 빠른 작업 목록을 표시 하는 경우 나타나는 하나를 선택 해야 합니다.

다음 예제에서는 단순한 오늘 확장 (위젯)는 확장 및 축소 상태를 처리 하는입니다.

```csharp
using System;
using NotificationCenter;
using Foundation;
using UIKit;
using CoreGraphics;

namespace MonkeyAbout
{
    public partial class TodayViewController : UIViewController, INCWidgetProviding
    {
        protected TodayViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Tell widget it can be expanded
            ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);

            // Get the maximum size
            var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
        }

        [Export ("widgetPerformUpdateWithCompletionHandler:")]
        public void WidgetPerformUpdate (Action<NCUpdateResult> completionHandler)
        {
            // Take action based on the display mode
            switch (ExtensionContext.GetWidgetActiveDisplayMode()) {
            case NCWidgetDisplayMode.Compact:
                Content.Text = "Let's Monkey About!";
                break;
            case NCWidgetDisplayMode.Expanded:
                Content.Text = "Gorilla!!!!";
                break;
            }

            // Report results
            // If an error is encoutered, use NCUpdateResultFailed
            // If there's no update required, use NCUpdateResultNoData
            // If there's an update, use NCUpdateResultNewData
            completionHandler (NCUpdateResult.NewData);
        }

        [Export ("widgetActiveDisplayModeDidChange:withMaximumSize:")]
        public void WidgetActiveDisplayModeDidChange (NCWidgetDisplayMode activeDisplayMode, CGSize maxSize)
        {
            // Take action based on the display mode
            switch (activeDisplayMode) {
            case NCWidgetDisplayMode.Compact:
                PreferredContentSize = maxSize;
                Content.Text = "Let's Monkey About!";
                break;
            case NCWidgetDisplayMode.Expanded:
                PreferredContentSize = new CGSize (0, 200);
                Content.Text = "Gorilla!!!!";
                break;
            }
        }

    }
}
```

자세히 위젯 디스플레이 모드 특정 코드를 살펴보세요. 이 위젯의 확장 상태를 지원 시스템에 알리기을 위해 사용 됩니다.

```csharp
// Tell widget it can be expanded
ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);
```

위젯의 현재 디스플레이 모드를 얻기 위해 사용 됩니다.

```csharp
ExtensionContext.GetWidgetActiveDisplayMode()
```

축소 또는 확장 된 상태에 대 한 최대 크기를 가져오려는 사용 됩니다.

```csharp
// Get the maximum size
var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
```

컴파일하고 (디스플레이 모드) 변경 상태를 처리 하려면 사용 합니다.

```csharp
[Export ("widgetActiveDisplayModeDidChange:withMaximumSize:")]
public void WidgetActiveDisplayModeDidChange (NCWidgetDisplayMode activeDisplayMode, CGSize maxSize)
{
    // Take action based on the display mode
    switch (activeDisplayMode) {
    case NCWidgetDisplayMode.Compact:
        PreferredContentSize = maxSize;
        Content.Text = "Let's Monkey About!";
        break;
    case NCWidgetDisplayMode.Expanded:
        PreferredContentSize = new CGSize (0, 200);
        Content.Text = "Gorilla!!!!";
        break;
    }
}
```

각 상태 (축소 또는 확장)에 대 한 요청된 된 크기를 설정 하는, 새 크기와 일치 하도록 표시 되는 콘텐츠도 업데이트 합니다.

## <a name="summary"></a>요약

이 문서는 Apple iOS 10에서에서 위젯 시스템에 대가 향상 된 기능을 설명 하 고 Xamarin.iOS에 구현 하는 방법이 표시 했습니다.



## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
- [확장 프로그램 소개](~/ios/platform/extensions.md)
- [3D 터치 소개](~/ios/platform/3d-touch.md)
- [응용 프로그램 확장 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html)
