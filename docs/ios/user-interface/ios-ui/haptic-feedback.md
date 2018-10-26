---
title: Xamarin.iOS에서 햅 틱 피드백 제공
description: 이 문서에서는 Xamarin.iOS 앱에 햅 틱 피드백을 제공 하는 방법을 설명 합니다. UIImpactFeedbackGenerator, UINotificationFeedbackGenerator, 및 UISelectionFeedbackGenerator에 설명 합니다.
ms.prod: xamarin
ms.assetid: 888106D1-58F4-453F-BACC-91D51FA39C80
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: b2c381c59ba1574e80babc2c7e68535a3deffe35
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123130"
---
# <a name="providing-haptic-feedback-in-xamarinios"></a>Xamarin.iOS에서 햅 틱 피드백 제공

<a name="Overview" />

## <a name="overview"></a>개요

IPhone 7 및 iPhone의 7 및 Apple가 실제로 사용자를 참여 하는 다른 방법을 제공 하는 새 햅 틱 응답에 포함 합니다. 햅 틱 피드백 (라고도 단순히 Haptics) 사용자 인터페이스 디자인 (force, 진동 또는 동작)를 통해 터치의 의미를 사용 합니다. 이러한 새 tactile 피드백 옵션을 사용 하 여 사용자의 주의 가져오고 해당 작업을 보강 합니다.

다음 항목에 대해 자세히 설명합니다.

- [햅 틱 피드백에 대 한](#About-Haptic-Feedback)
- [UIImpactFeedbackGenerator](#UIImpactFeedbackGenerator)
- [UINotificationFeedbackGenerator](#UINotificationFeedbackGenerator)
- [UISelectionFeedbackGenerator](#UISelectionFeedbackGenerator)

<a name="About-Haptic-Feedback" />

## <a name="about-haptic-feedback"></a>햅 틱 피드백에 대 한

여러 기본 제공 UI 요소는 이미 선택, 스위치 및 슬라이더와 같은 햅 틱 피드백을 제공합니다. iOS 10 이제 haptics의 구체적 하위 클래스를 사용 하 여 프로그래밍 방식으로 실행 하는 기능을 추가 합니다 `UIFeedbackGenerator` 클래스입니다.

개발자는 다음 중 하나를 사용할 수 `UIFeedbackGenerator` 프로그래밍 방식으로 트리거 햅 틱 피드백을 서브 클래스:

- `UIImpactFeedbackGenerator` -작업 또는 보기를 슬라이드에 또는 두 화면 개체 충돌 하는 경우 "울려"를 표시 하는 등의 태스크를 보완 하기 위해이 피드백 생성기를 사용 합니다.
- `UINotificationFeedbackGenerator` -경고의 작업 완료, 실패 또는 다른 형식과 같은 알림에이 피드백 생성기를 사용 합니다.
- `UISelectionFeedbackGenerator` -목록에서 항목을 선택 하는 등 변경 적극적으로 선택이 피드백 생성기를 사용 합니다.

<a name="UIImpactFeedbackGenerator" />

### <a name="uiimpactfeedbackgenerator"></a>UIImpactFeedbackGenerator

작업 또는 보기를 슬라이드에 또는 두 화면 개체 충돌 하는 경우 "울려"를 표시 하는 등의 태스크를 보완 하기 위해이 피드백 생성기를 사용 합니다.

트리거 영향을 줄 피드백에 다음 코드를 사용 합니다.

```csharp
using UIKit;
...

// Initialize feedback
var impact = new UIImpactFeedbackGenerator (UIImpactFeedbackStyle.Heavy);
impact.Prepare ();

// Trigger feedback
impact.ImpactOccurred ();
```

개발자의 새 인스턴스를 만들 때 합니다 `UIImpactFeedbackGenerator` 제공 하는 클래스를 `UIImpactFeedbackStyle` 으로 피드백의 강도 지정:

- `Heavy`
- `Medium`
- `Light`

합니다 `Prepare` 메서드는 `UIImpactFeedbackGenerator` 햅 틱 피드백에 대 한 대기 시간을 최소화할 수 있도록 하는 시스템에 알리기 위해 호출 됩니다.

`ImpactOccurred` 메서드는 다음 햅 틱 피드백을 트리거합니다.

<a name="UINotificationFeedbackGenerator" />

### <a name="uinotificationfeedbackgenerator"></a>UINotificationFeedbackGenerator

경고의 작업 완료, 실패 또는 다른 형식과 같은 알림에이 피드백 생성기를 사용 합니다.

트리거 알림 피드백에 다음 코드를 사용 합니다.

```csharp
using UIKit;
...

// Initialize feedback
var notification = new UINotificationFeedbackGenerator ();
notification.Prepare ();

// Trigger feedback
notification.NotificationOccurred (UINotificationFeedbackType.Error);
```

새 인스턴스를 `UINotificationFeedbackGenerator` 클래스를 만들 고 `Prepare` 햅 틱 피드백에 대 한 대기 시간을 최소화할 수 있도록 하는 시스템에 알리기 위해 호출 됩니다.

합니다 `NotificationOccurred` 를 사용 하 여 햅 틱 피드백 트리거할 라고는 지정 된 `UINotificationFeedbackType` 의:

- `Success`
- `Warning`
- `Error`

<a name="UISelectionFeedbackGenerator" />

### <a name="uiselectionfeedbackgenerator"></a>UISelectionFeedbackGenerator

목록에서 항목을 선택 하는 등 변경 적극적으로 선택이 피드백 생성기를 사용 합니다.

트리거 선택 피드백에 다음 코드를 사용 합니다.

```csharp
using UIKit;
...

// Initialize feedback
var selection = new UISelectionFeedbackGenerator ();
selection.Prepare ();

// Trigger feedback
selection.SelectionChanged ();
```

새 인스턴스를 `UISelectionFeedbackGenerator` 클래스를 만들 고 `Prepare` 햅 틱 피드백에 대 한 대기 시간을 최소화할 수 있도록 하는 시스템에 알리기 위해 호출 됩니다.

`SelectionChanged` 메서드는 다음 햅 틱 피드백을 트리거합니다.

## <a name="summary"></a>요약

이 문서에서는 iOS 10과 Xamarin.iOS에서 구현 하는 방법에서 사용할 수 있는 햅 틱 피드백의 새 형식 검사가 수행 합니다.

## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
