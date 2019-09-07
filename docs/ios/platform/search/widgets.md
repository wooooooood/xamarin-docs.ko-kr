---
title: IOS 10에서 향상 된 검색 및 홈 화면 위젯
description: 이 문서에서는 검색 및 홈 화면 위젯의 업데이트를 포함 하 여 iOS 10의 위젯에 대 한 Apple의 향상 된 기능에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: D66FD9E1-9E23-4BB6-825C-ED19B8F72A81
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: 969d7fc78af9dd10f7ad57f58a6f4f619d0a201a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769450"
---
# <a name="search-and-home-screen-widget-enhancements-in-ios-10"></a>IOS 10에서 향상 된 검색 및 홈 화면 위젯

_이 문서에서는 iOS 10의 위젯 시스템에 대 한 Apple의 향상 된 기능에 대해 설명 합니다._

Apple은 위젯 시스템에 몇 가지 향상 된 기능을 도입 하 여 새 iOS 10 잠금 화면에 있는 모든 배경에서 위젯을 잘 보이도록 합니다. 또한 위젯을 사용 하 여 사용 가능한 콘텐츠의 양을 설명 하 고 사용자가 콘텐츠를 확장 및 축소할 수 있도록 하는 [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) 속성이 포함 됩니다.

위젯 (오늘 확장이 라고도 함)은 몇 가지 유용한 정보를 표시 하거나 앱 특정 기능을 적시에 노출 하는 특수 한 유형의 iOS 확장입니다. 예를 들어 뉴스 앱에는 최상위 헤드라인을 표시 하는 위젯이 있고 달력 앱은 오늘 이벤트를 표시 하 고 예정 된 이벤트를 표시 하는 두 가지 위젯을 제공 합니다.

위젯은 항상 사용자 지정이 가능 하며 텍스트, 이미지, 단추 등의 UI 요소를 포함할 수 있습니다. 또한 개발자는 위젯의 레이아웃을 추가로 사용자 지정할 수 있습니다.

[![](widgets-images/widgets01.png "예제 위젯")](widgets-images/widgets01.png#lightbox)

사용자가 앱의 위젯을 보고 상호 작용할 수 있는 두 가지 주요 위치가 있습니다.

- **검색 화면** -사용자는 검색 화면에 가장 유용한 것으로 확인 된 위젯을 추가할 수 있습니다. 검색 화면에는 홈 및 잠금 화면에서 오른쪽으로 살짝 밀기를 통해 액세스 합니다.
- 홈 **화면** -홈 화면에서 사용자는 3d 터치를 사용 하 여 앱의 아이콘에 압력을 적용 하 여 빠른 작업 목록을 열 수 있습니다. 앱의 위젯은 빠른 작업 목록 위에 표시 됩니다. 자세한 내용은 [3D 터치 설명서 소개를](~/ios/platform/3d-touch.md) 참조 하세요.

## <a name="widgets-developer-suggestions"></a>위젯 개발자 제안

사용자가 검색 화면에 추가 하려는 위젯을 항상 개발자가 시도 하 고 디자인 해야 하는 것이 가장 좋습니다. 이를 위해 Apple의 제안 사항은 다음과 같습니다.

- 상태 업데이트에 대 한 간략 하 고 Glanceable 정보를 제공 하거나 간단한 작업을 신속 하 게 수행할 수 있도록 해 주는 **유용한 Glanceable Experience** 사용자를 만듭니다. 그러면 적절 한 양의 정보를 제공 하 고 상호 작용을 매우 중요 하 게 합니다. 가능 하면 사용자가 한 번 탭 하 여 지정 된 작업을 수행할 수 있도록 허용 합니다. 또한 위젯은 패닝 또는 스크롤을 지원 하지 않으므로 위젯의 디자인에서 고려해 야 합니다.
- **콘텐츠 위젯을 빠르게 표시** 하려면 위젯을 표시 한 후 사용자가 콘텐츠가 로드 될 때까지 기다릴 필요가 없도록 glanceable 디자인 되었습니다. 위젯은 새 콘텐츠를 백그라운드에서 로드 하는 동안 최신 콘텐츠를 표시할 수 있도록 로컬에서 콘텐츠를 캐시 해야 합니다.
- **적절 한 안쪽 여백 및 여백 지정** -위젯은 복잡 하 게 표시 되지 않으므로 위젯 보기의 가장자리에 대 한 콘텐츠를 확장 하지 마십시오. 가장자리와 내용 사이에는 항상 몇 픽셀 넓은 여백이 있어야 합니다. 또한 Apple은 위젯 맨 위에 표시 되는 앱 아이콘을 맞춤 안내선으로 사용 하는 것을 제안 합니다. 위젯이 모눈 레이아웃을 표시 하는 경우 표의 항목 사이에 적절 한 안쪽 여백이 있는지 확인 하 고 항목 수를 최대 4 개로 제한 합니다.
- 조정 가능 **레이아웃 사용** -위젯의 너비는 실행 중인 장치와 장치의 방향에 따라 달라 집니다. 위젯의 높이는 축소 된 (기본값) 또는 확장 (모든 위젯에서 지원 되지 않음) 상태에 표시 되는지 여부에 따라 달라질 수도 있습니다. 축소 된 위젯의 높이는 약 2와 절반 표준 iOS 테이블 행입니다. 개발자는 확장 된 위젯의 크기를 요청할 수 있지만이는 화면 높이 보다 작아야 합니다. 축소 된 상태에서는 위젯에 필수적인 독립 실행형 정보만 표시 됩니다. 확장 하면 위젯에 축소 된 상태에 표시 되는 기본 콘텐츠를 개선 하는 추가 정보가 표시 됩니다. 빠른 작업 목록에 표시 되는 위젯은 축소 된 상태에 있습니다.
- **위젯의 배경을 사용자 지정 하지 마세요** . 위젯은 시스템에서 제공 하는 밝은 흐리게 표시 된 배경에 표시 됩니다. 이 작업은 위젯 간의 일관성을 승격 하 고 콘텐츠 가독성을 높이기 위해 수행 됩니다. 이미지는 사용자의 잠금 및 홈 화면 wallpapers와 충돌할 수 있으므로 위젯 배경으로 사용 하지 마십시오.
- **검은색 또는 진한 회색으로 시스템 글꼴 사용** -위젯에 텍스트를 표시할 때 시스템 글꼴이 가장 잘 작동 합니다. 밝은 파란색 위젯을 강조 표시 하기 위해 글꼴이 검정 또는 어두운 회색으로 표시 되어야 합니다.
- 적절 한 위젯이 항상 앱과 별도로 작동 해야 하는 **경우 앱 액세스를 제공** 하지만, 더 심층적인 기능이 필요한 경우 위젯은 앱을 시작 하 여 특정 정보를 보거나 편집할 수 있어야 합니다. "앱 열기" 단추는 포함 하지 마세요. 사용자가 콘텐츠 자체를 탭 하 고 타사 앱을 열지는 않습니다.
- **명확 하 고 간결한 위젯 이름을 선택** 합니다. 즉, 앱의 아이콘과 위젯의 이름이 항상 위젯의 내용 위에 표시 됩니다. Apple은 기본 위젯에 앱 이름을 사용 하는 것을 제안 하 고, 제공 되는 다른 모든 사용자에 게 명확 하 고 간결한 이름을 제공 합니다. 사용자 지정 위젯 제목을 제공 하는 경우 앱의 이름 (예: 근처 지도, 지도 식당 등)을 접두사로 사용 해야 합니다.
- **인증에 값이 추가 되 면** 알림-사용자를 인증 하 고 로그온 한 경우에만 추가 기능 또는 정보를 사용할 수 있는 경우 사용자에 게 제공 합니다. 예를 들어, 타는 공유 앱에는 "책에 로그인" 이라고 표시 될 수 있습니다.
- **빠른 작업 목록 위젯 선택** -앱에서 두 개 이상의 위젯을 제공 하는 경우 개발자는 3d 터치를 사용 하 여 앱의 아이콘에 압력을 적용 하 여 빠른 작업 목록을 표시할 때 사용할 하나를 선택 해야 합니다.

위젯 사용에 대 한 자세한 내용은 [확장 소개](~/ios/platform/extensions.md), [3d 터치 설명서 소개](~/ios/platform/3d-touch.md) 및 Apple의 [앱 확장 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html)를 참조 하세요.

## <a name="working-with-vibrancy"></a>Vibrancy 사용

Vibrancy는 위젯의 밝은 배경 (시스템에서 제공 됨)에 표시 될 때 위젯의 텍스트를 이해 하기 쉽게 유지 합니다. IOS 10 이전에는 개발자가 위젯의 vibrancy에 [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) 를 사용 합니다. 예를 들어:

```csharp
// DEPRECATED: Get Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreateForNotificationCenter ();
```

이는 iOS 10에서 더 이상 사용 되지 않으며, [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) 또는 [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect)로 바꾸어야 합니다. 예:

```csharp
// Get Primary Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreatePrimaryVibrancyEffectForNotificationCenter ();

// Get Secondary Widget Vibrancy Effect
var vibrancy2 = UIVibrancyEffect.CreateSecondaryVibrancyEffectForNotificationCenter ();
```

## <a name="working-with-collapsed-and-expanded-widgets"></a>축소 및 확장 된 위젯 작업

IOS 10의 새로운 기능에는 이제 개발자가 사용할 수 있는 콘텐츠의 양을 설명 하 고 사용자가 콘텐츠를 확장 및 축소할 수 있도록 하는 [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) 속성이 포함 되어 있습니다.

처음에는 위젯이 표시 될 때 축소 된 상태입니다. 축소 된 위젯의 높이는 약 2와 절반 표준 iOS 테이블 행입니다. 개발자는 확장 된 위젯의 크기를 요청할 수 있지만이는 화면 높이 보다 작아야 합니다.

축소 된 상태에서는 위젯에 필수적인 독립 실행형 정보만 표시 됩니다. 확장 하면 위젯에 축소 된 상태에 표시 되는 기본 콘텐츠를 개선 하는 추가 정보가 표시 됩니다. 예를 들어 날씨 앱은 축소 될 때 현재 날씨 조건을 표시 하 고 확장 될 때 시간별 예측을 추가 합니다.

빠른 작업 목록에 표시 되는 위젯은 축소 된 상태에 있습니다. 앱에서 두 개 이상의 위젯을 제공 하는 경우 개발자는 3D 터치를 사용 하 여 앱의 아이콘에 압력을 적용 하 여 빠른 작업 목록을 표시할 때 사용할 하나를 선택 해야 합니다.

다음 예는 축소 및 확장 된 상태를 처리 하는 간단한 오늘 확장 (위젯)의 예입니다.

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

위젯 표시 모드 특정 코드를 자세히 살펴봅니다. 이 위젯이 확장 된 상태를 지원함을 시스템에 알리려면 다음을 사용 합니다.

```csharp
// Tell widget it can be expanded
ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);
```

위젯의 현재 표시 모드를 가져오기 위해 다음을 사용 합니다.

```csharp
ExtensionContext.GetWidgetActiveDisplayMode()
```

축소 또는 확장 된 상태에 대 한 최대 크기를 가져오려면 다음을 사용 합니다.

```csharp
// Get the maximum size
var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
```

그리고 상태 (표시 모드)를 변경 하는 경우 다음을 사용 합니다.

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

각 상태 (축소 또는 확장)에 대해 요청 된 크기를 설정 하는 것 외에도 새 크기와 일치 하도록 표시 되는 콘텐츠를 업데이트 합니다.

## <a name="summary"></a>요약

이 문서에서는 iOS 10의 위젯 시스템에 대 한 Apple의 향상 된 기능에 대해 설명 하 고 Xamarin.ios에서 이러한 기능을 구현 하는 방법을 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [확장 소개](~/ios/platform/extensions.md)
- [3D 터치 소개](~/ios/platform/3d-touch.md)
- [앱 확장 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html)
