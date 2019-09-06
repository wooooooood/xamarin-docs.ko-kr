---
title: Xamarin.ios에서 햅 피드백 제공
description: 이 문서에서는 Xamarin.ios 앱에서 햅 피드백을 제공 하는 방법을 설명 합니다. UIImpactFeedbackGenerator, UINotificationFeedbackGenerator 및 UISelectionFeedbackGenerator에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 888106D1-58F4-453F-BACC-91D51FA39C80
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: 112ee17eab872f9265687869bec82e72f44e81da
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70287088"
---
# <a name="providing-haptic-feedback-in-xamarinios"></a>Xamarin.ios에서 햅 피드백 제공

<a name="Overview" />

## <a name="overview"></a>개요

IPhone 7 및 iPhone 7 Plus에서 Apple에는 사용자에 게 물리적으로 참여 하는 추가 방법을 제공 하는 새로운 햅 응답이 포함 되었습니다. 햅 피드백 (단순히 Haptics 이라고 함)은 사용자 인터페이스 디자인에서 touch (force, vibrations 또는 움직임을 통해)를 사용 합니다. 이러한 새 tactile 사용자 의견 옵션을 사용 하 여 사용자의 주의가 필요 하 고 해당 작업을 보강 하세요.

다음 항목에 대해 자세히 설명합니다.

- [햅 피드백 정보](#About-Haptic-Feedback)
- [UIImpactFeedbackGenerator](#UIImpactFeedbackGenerator)
- [UINotificationFeedbackGenerator](#UINotificationFeedbackGenerator)
- [UISelectionFeedbackGenerator](#UISelectionFeedbackGenerator)

<a name="About-Haptic-Feedback" />

## <a name="about-haptic-feedback"></a>햅 피드백 정보

여러 기본 제공 UI 요소는 이미 선택기, 스위치 및 슬라이더와 같은 햅 피드백을 제공 합니다. 이제 iOS 10은 `UIFeedbackGenerator` 클래스의 구체적 하위 클래스를 사용 하 여 프로그래밍 방식으로 haptics를 트리거하는 기능을 추가 합니다.

개발자는 다음 `UIFeedbackGenerator` 서브 클래스 중 하나를 사용 하 여 프로그래밍 방식으로 햅 피드백을 트리거할 수 있습니다.

- `UIImpactFeedbackGenerator`-이 피드백 생성기를 사용 하 여 뷰 슬라이드를 발표할 때 또는 두 개의 화면 개체가 충돌 하는 경우 "thud"를 제공 하는 등의 작업 또는 작업을 보완할 수 있습니다.
- `UINotificationFeedbackGenerator`-작업 완료, 실패 또는 다른 유형의 경고와 같은 알림에 대해이 피드백 생성기를 사용 합니다.
- `UISelectionFeedbackGenerator`-목록에서 항목을 선택 하는 등 적극적으로 변경 하는 경우이 피드백 생성기를 사용 합니다.

<a name="UIImpactFeedbackGenerator" />

### <a name="uiimpactfeedbackgenerator"></a>UIImpactFeedbackGenerator

이 피드백 생성기를 사용 하 여 뷰 슬라이드를 게시할 때 또는 두 개의 화면 개체가 충돌 하는 경우 "thud"를 제공 하는 것과 같은 작업 또는 작업을 보완할 수 있습니다.

다음 코드를 사용 하 여 영향 피드백을 트리거합니다.

```csharp
using UIKit;
...

// Initialize feedback
var impact = new UIImpactFeedbackGenerator (UIImpactFeedbackStyle.Heavy);
impact.Prepare ();

// Trigger feedback
impact.ImpactOccurred ();
```

개발자는 `UIImpactFeedbackGenerator` 클래스의 새 인스턴스를 만들 때 다음과 같이 사용자 의견 `UIImpactFeedbackStyle` 수준을 지정 하는를 제공 합니다.

- `Heavy`
- `Medium`
- `Light`

의 메서드는 대기 시간을 최소화할 수 있도록 햅 피드백이 발생 함을 시스템에 알리기 위해 호출 됩니다.`UIImpactFeedbackGenerator` `Prepare`

그런 `ImpactOccurred` 다음이 메서드는 햅 피드백을 트리거합니다.

<a name="UINotificationFeedbackGenerator" />

### <a name="uinotificationfeedbackgenerator"></a>UINotificationFeedbackGenerator

작업 완료, 실패 또는 다른 유형의 경고와 같은 알림에 대해이 피드백 생성기를 사용 합니다.

다음 코드를 사용 하 여 알림 피드백을 트리거합니다.

```csharp
using UIKit;
...

// Initialize feedback
var notification = new UINotificationFeedbackGenerator ();
notification.Prepare ();

// Trigger feedback
notification.NotificationOccurred (UINotificationFeedbackType.Error);
```

`UINotificationFeedbackGenerator` 클래스의 새 인스턴스를 만들고 해당 `Prepare` 메서드를 호출 하 여 대기 시간을 최소화할 수 있도록 햅 피드백이 발생 한다는 사실을 시스템에 알립니다.

`NotificationOccurred` 지정`UINotificationFeedbackType` 된을 사용 하 여 햅 피드백을 트리거하기 위해가 호출 됩니다.

- `Success`
- `Warning`
- `Error`

<a name="UISelectionFeedbackGenerator" />

### <a name="uiselectionfeedbackgenerator"></a>UISelectionFeedbackGenerator

목록에서 항목을 선택 하는 등 적극적으로 변경 하는 경우이 피드백 생성기를 사용 합니다.

다음 코드를 사용 하 여 선택 피드백을 트리거합니다.

```csharp
using UIKit;
...

// Initialize feedback
var selection = new UISelectionFeedbackGenerator ();
selection.Prepare ();

// Trigger feedback
selection.SelectionChanged ();
```

`UISelectionFeedbackGenerator` 클래스의 새 인스턴스를 만들고 해당 `Prepare` 메서드를 호출 하 여 대기 시간을 최소화할 수 있도록 햅 피드백이 발생 한다는 사실을 시스템에 알립니다.

그런 `SelectionChanged` 다음이 메서드는 햅 피드백을 트리거합니다.

## <a name="summary"></a>요약

이 문서에서는 iOS 10에서 제공 되는 새로운 유형의 햅 피드백 및 Xamarin.ios에서 구현 하는 방법에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
