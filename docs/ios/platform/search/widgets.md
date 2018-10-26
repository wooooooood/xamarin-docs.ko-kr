---
title: 검색 및 iOS 10에서에서 홈 화면 위젯 기능
description: 이 문서에서는 향상 된 Apple에 검색 및 홈 화면 위젯 업데이트를 비롯 하 여 10, iOS의 위젯을 설명 합니다.
ms.prod: xamarin
ms.assetid: D66FD9E1-9E23-4BB6-825C-ED19B8F72A81
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: f693b480fff141c177ed135ced60afd65abd77de
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102793"
---
# <a name="search-and-home-screen-widget-enhancements-in-ios-10"></a>검색 및 iOS 10에서에서 홈 화면 위젯 기능

_이 문서에서는 Apple iOS 10에서에서 위젯 시스템에 대 한에 향상 된 기능을 다룹니다._

Apple 위젯 시스템을 확인 하는 위젯 10 잠금 화면에서 새 iOS 존재 하는 백그라운드에서 훌륭해 여러 개선 된 기능을 도입 되었습니다. 또한 위젯을 포함 이제는 [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) 속성을 사용 하면 얼마나 많은 콘텐츠를 사용할 수를 설명 하기 위해 개발자 및 확장 하 고 콘텐츠를 축소 하면입니다.

(라고도 오늘 확장) 위젯은 특별 한 유형의 소량의 한 유용한 정보를 표시 하거나 앱 별 시기 적절 하 게 기능을 노출 하는 iOS 확장 합니다. 예를 들어 뉴스 앱에 상위 헤드라인을 보여주는 위젯 및 일정 앱의 다른 원하는 위젯 두 개가 제공: 표시할 오늘날의 이벤트 및 예정 된 이벤트를 표시 합니다.

위젯 폭넓게 및 텍스트, 이미지, 단추 등의 UI 요소를 포함할 수 있습니다. 또한 개발자는 위젯의 레이아웃 추가로 사용자 지정할 수 있습니다.

[![](widgets-images/widgets01.png "위젯 예제")](widgets-images/widgets01.png#lightbox)

사용자 보기를 업데이트 하 고 앱의 위젯 상호 작용할 수 있는 두 기본 위치는:

- **검색 화면** -사용자가 각자 가장 유용한 해당 검색 화면을 위젯을 추가할 수 있습니다. 홈 및 잠금 화면에서 오른쪽을 살짝 밀어 검색 화면에 액세스 합니다.
- **홈 화면** -에서 홈 화면에서 사용자 앱의 아이콘에 압력을 적용 하 여 빠른 작업 목록을 열려면 3D 터치를 사용할 수 있습니다. 앱의 위젯 빠른 작업 목록 위에 표시 됩니다. 참조 하십시오 우리의 [3D 터치 소개](~/ios/platform/3d-touch.md) 자세한 정보에 대 한 설명서입니다.

## <a name="widgets-developer-suggestions"></a>위젯 개발자 제안

이상적으로 개발자 항상 시도 하 고 사용자가 검색 화면에 추가 하려고 하는 위젯 디자인 합니다. 이 위해 Apple에는 다음 제안에 있습니다.

- **뛰어난, Glanceable 환경을 만드는** -사용자의 원하는 상태 업데이트의 한 간단 하 고 glanceable 정보를 제공 하거나 간단한 작업을 신속 하 게 수행할 수 있도록 하는 위젯입니다. 이렇게 하면 적절 한 양의 정보 및 필수 상호 작용을 제공 합니다. 가능 하면 한 번의 탭을 사용 하 여 지정된 된 태스크를 수행 하려면 사용자를 허용 합니다. 또한 위젯 패닝 또는 스크롤을 지원 하지 않는 있으므로이 해야 위젯의 디자인에서 고려해 야 합니다.
- **콘텐츠를 신속 하 게 표시** -위젯 사용자는 위젯 표시 되 면 로드할 내용을 기다릴 필요가 없습니다 있도록 glanceable, 되도록 설계 되었습니다. 위젯 최신 콘텐츠를 백그라운드에서 로드 되는 동안 최근 콘텐츠 표시 수 있도록 로컬로 콘텐츠를 캐시 해야 합니다.
- **적절 한 안쪽 여백 및 여백 제공** -위젯 해야 꽉 확인 사용 안 함, 있으므로 확장을 위젯 보기의 가장자리에 콘텐츠를 방지 하지 않습니다. 여러 픽셀 넓은 여백 가장자리와 내용 사이의 항상 있을 것입니다. 또한 Apple는 맞춤 가이드 처럼 위젯의 맨 위에 있는 표시 되는 앱의 아이콘을 사용 하 여 제안 합니다. 표 형태 레이아웃을 제공 하는 위젯, 표의 항목 사이의 안쪽 여백 적절 한는 있는지 확인 하 고 4 개의 최대 항목 수를 제한 하려고 합니다.
- **융통성 있는 레이아웃을 사용 하 여** -는 위젯 너비에서 실행 되 고 장치 및 장치 방향에 따라 달라 집니다. 축소 된 (기본값) 또는 확장 (모든 위젯에서 지원 되지 않음) 상태에 표시 되는 경우에 위젯의 높이에 따라 달라질 수 있습니다. 축소 위젯에 약 2.5 표준 iOS 테이블 행의 높이입니다. 개발자는 확장 위젯의 크기를 요청할 수 있지만 이상적 해야 화면의 높이 보다 작으면 합니다. 축소 된 상태에서 위젯을 유일한 필수, 독립 실행형 정보를 표시 해야 합니다. 때 확장, 위젯을 축소 된 상태로 표시 되는 기본 콘텐츠를 향상 시키는 추가 정보를 표시 됩니다. 축소 된 상태에서 위젯 빠른 작업 목록에 표시 됩니다.
- **사용자 지정 위젯의 백그라운드 하지** -위젯 시스템에서 제공 된 조명, 흐리게 표시 된 배경으로 표시 됩니다. 이렇게 위젯 간의 일관성을 승격 하 여 해당 콘텐츠의 가독성을 향상 합니다. 사용자의 잠금 및 홈 화면 배경 무늬에 충돌할 수 있으므로 위젯 배경으로 이미지를 사용 하지 마십시오.
- **진한 회색 또는 검은색 시스템 글꼴을 사용 하 여** -위젯, 최상의 시스템 글꼴 작동에에서 텍스트를 표시 합니다. 글꼴, 흐리게 표시 된 위젯 배경에 대해 돋보이게 하려면 검정 또는 어두운 회색 색에 있어야 합니다.
- **그러나 앱 액세스 때 적절 한 제공** -위젯의 해당 앱에서 별도로 작동 항상, 하위 수준 기능이 필요한 경우 위젯에 수 있어야를 보거나 편집 내용은 특정 응용 프로그램을 실행 합니다. 되지 열기 타사 앱 및 콘텐츠 자체를 탭 하 여 사용자를 허용 하기만 하면 "앱을 열고" 단추를 포함 하지 않습니다.
- **Clear, 간결한 위젯 이름 선택** -앱의 아이콘 및 위젯의 이름이 항상 위젯의 콘텐츠를 통해 표시 됩니다. Apple의 기본 장치에 대 한 앱의 이름과 다른 제공에 대 한 명확 하 고 간결한 이름을 사용 하 여 제안 합니다. 사용자 지정 위젯 제목을 제공 하는 경우 앱의 이름 (예: 주변 맵, 맵 식당 등)를 사용 하 여 접두사가 해야 합니다.
- **인증 값을 추가 하는 경우 알릴** -추가 기능이 나 정보는 사용자가 인증 하는 경우에 사용할 수 있으며, 로그온 사용자에 게 표시 하는 경우. 예를 들어, 앱을 공유 하는 승객 "책 승객에 로그인" 표시 될 수도 있습니다.
- **빠른 작업 목록 위젯을 선택** -개발자 사용자 압력 3D 터치를 사용 하 여 앱의 아이콘을 적용 하 여 빠른 작업 목록을 표시 하는 때를 표시 하도록 하나를 선택 해야 앱이 둘 이상의 위젯을 제공 합니다.

위젯 사용 하 여 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [확장 프로그램 소개](~/ios/platform/extensions.md)를 [3D 터치 소개](~/ios/platform/3d-touch.md) 설명서와 Apple [앱 확장 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html).

## <a name="working-with-vibrancy"></a>표현 사용

표현 위젯의 텍스트 유지 위젯의 빛을 흐리게 표시 된 배경 (시스템에서 제공)에 표시 될 때 읽을 수 있도록 보장 합니다. 이전 iOS 10, 개발자는 사용 하 여는 [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) 위젯의 표현에 대 한 합니다. 예를 들어:

```csharp
// DEPRECATED: Get Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreateForNotificationCenter ();
```

IOS 10에서에서 사용 되지 않음에 및 바꿔야 하거나이 [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) 또는 [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect)합니다. 예를 들어:

```csharp
// Get Primary Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreatePrimaryVibrancyEffectForNotificationCenter ();

// Get Secondary Widget Vibrancy Effect
var vibrancy2 = UIVibrancyEffect.CreateSecondaryVibrancyEffectForNotificationCenter ();
```

## <a name="working-with-collapsed-and-expanded-widgets"></a>축소 및 확장 된 위젯 사용

새 ios 10, 위젯 이제 포함 된 [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) 얼마나 많은 콘텐츠를 사용할 수를 설명 하기 위해 개발자 및 확장 하 고 콘텐츠를 축소할 수 있도록 하는 속성.

위젯 처음 표시 되 면 축소 된 상태로 됩니다. 축소 위젯에 약 2.5 표준 iOS 테이블 행의 높이입니다. 개발자는 확장 위젯의 크기를 요청할 수 있지만 이상적 해야 화면의 높이 보다 작으면 합니다. 

축소 된 상태에서 위젯을 유일한 필수, 독립 실행형 정보를 표시 해야 합니다. 때 확장, 위젯을 축소 된 상태로 표시 되는 기본 콘텐츠를 향상 시키는 추가 정보를 표시 됩니다. 예를 들어 날씨 앱 축소 하면 현재 기상 조건을 표시 하 고 확장 하면 예측 매시간을 추가 합니다.

축소 된 상태에서 위젯 빠른 작업 목록에 표시 됩니다. 둘 이상의 위젯을 제공 하는 앱 개발자는 사용자 압력 3D 터치를 사용 하 여 앱의 아이콘을 적용 하 여 빠른 작업 목록을 표시 하는 때를 표시 하도록 하나를 선택 해야 합니다.

다음 예제에서는 간단한 오늘 확장 (위젯)는 확장 및 축소 된 상태를 처리 하는입니다.

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

세부 정보에서 위젯을 디스플레이 모드 특정 코드를 살펴보세요. 이 위젯의 확장 된 상태 지 시스템 알리는 사용 합니다.

```csharp
// Tell widget it can be expanded
ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);
```

위젯의 현재 표시 모드를 가져오려면 사용 합니다.

```csharp
ExtensionContext.GetWidgetActiveDisplayMode()
```

확장 또는 축소 된 상태에 대 한 최대 크기를 가져오려면 사용 합니다.

```csharp
// Get the maximum size
var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
```

사용 하 여 변경 상태 (표시 모드)를 처리 하려면:

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

각 상태 (축소 또는 확장)에 대 한 요청된 된 크기를 설정 하는 것 외에도 새 크기에 맞게 표시 되는 콘텐츠를 업데이트 합니다.

## <a name="summary"></a>요약

이 문서에서는 Apple에 iOS 10에서에서 위젯 시스템에 대 한 향상 된 기능을 다루는 있으며 Xamarin.iOS에서이 구현 하는 방법이 표시 됩니다.



## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
- [확장 프로그램 소개](~/ios/platform/extensions.md)
- [3D 터치 소개](~/ios/platform/3d-touch.md)
- [앱 확장 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html)
