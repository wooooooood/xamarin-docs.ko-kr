---
title: "Haptic 사용자 의견 제공"
description: "이 문서에서는 새로운 유형의 haptic 피드백 Xamarin.iOS에 구현 하는 방법과 iOS 10에서 사용할 수 있는 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 888106D1-58F4-453F-BACC-91D51FA39C80
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 9f4eb989fe0c91471c9473c512c4befd36e4ace2
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="providing-haptic-feedback"></a>Haptic 사용자 의견 제공

_이 문서에서는 새로운 유형의 haptic 피드백 Xamarin.iOS에 구현 하는 방법과 iOS 10에서 사용할 수 있는 설명 합니다._

<a name="Overview" />

## <a name="overview"></a>개요

IPhone의 7 iPhone에 7 및, 사과 실제로 사용자 의견을 교환 하는 다른 방법을 제공 하는 새 haptic 응답 포함 합니다. Force, 진동 또는 동작) (통해 터치의 의미를 사용 하 여 사용자 인터페이스 디자인에 haptic 피드백 (라고도 단순히 Haptics). 이러한 새 tactile 사용자 의견 옵션을 사용 하 여 사용자의 주의 가져오고 그 작업을 강화 하 합니다.

다음 항목에 대해 자세히 설명합니다.

- [Haptic 피드백에 대 한](#About-Haptic-Feedback)
- [UIImpactFeedbackGenerator](#UIImpactFeedbackGenerator)
- [UINotificationFeedbackGenerator](#UINotificationFeedbackGenerator)
- [UISelectionFeedbackGenerator](#UISelectionFeedbackGenerator)

<a name="About-Haptic-Feedback" />

## <a name="about-haptic-feedback"></a>Haptic 피드백에 대 한

여러 기본 제공 UI 요소는 이미 선택, 스위치 및 슬라이더와 같은 haptic 피드백을 제공 합니다. iOS 10 haptics의 구체적인 서브 클래스를 사용 하 여 프로그래밍 방식으로 실행 하는 기능을 이제 추가 `UIFeedbackGenerator` 클래스입니다.

개발자는 다음 중 하나를 사용할 수 `UIFeedbackGenerator` 트리거 haptic 피드백과 프로그래밍 방식으로 서브 클래스:

- `UIImpactFeedbackGenerator` -작업 또는 보기 슬라이드 제자리에 또는 두 개의 화면에 나타나는 개체 충돌 하는 경우 "울려"를 제공 하는 등의 작업을 보완 하기 위해이 피드백 생성기를 사용 합니다.
- `UINotificationFeedbackGenerator` -경고의 작업 완료, 실패 또는 다른 모든 형식과 같은 알림에 대 한이 피드백 생성기를 사용 합니다.
- `UISelectionFeedbackGenerator` -목록에서 항목을 선택 하는 등 변경 적극적으로 선택에 대 한이 피드백 생성기를 사용 합니다.

<a name="UIImpactFeedbackGenerator" />

### <a name="uiimpactfeedbackgenerator"></a>UIImpactFeedbackGenerator

작업 또는 보기 슬라이드 제자리에 또는 두 개의 화면에 나타나는 개체 충돌 하는 경우 "울려"를 제공 하는 등의 작업을 보완 하기 위해이 피드백 생성기를 사용 합니다.

트리거 영향 피드백에 다음 코드를 사용 합니다.

```csharp
using UIKit;
...

// Initialize feedback
var impact = new UIImpactFeedbackGenerator (UIImpactFeedbackStyle.Heavy);
impact.Prepare ();

// Trigger feedback
impact.ImpactOccurred ();
```

개발자의 새 인스턴스를 생성할 때는 `UIImpactFeedbackGenerator` 제공 하는 클래스는 `UIImpactFeedbackStyle` 피드백을의 강도 지정:

- `Heavy`
- `Medium`
- `Light`

`Prepare` 의 메서드는 `UIImpactFeedbackGenerator` haptic 피드백 대기 시간을 최소화할 수 있도록 이루어진다는 것을 시스템에 알리기 위해 호출 됩니다.

`ImpactOccurred` 메서드 다음 haptic 피드백을 트리거합니다.

<a name="UINotificationFeedbackGenerator" />

### <a name="uinotificationfeedbackgenerator"></a>UINotificationFeedbackGenerator

이 피드백 생성기를 사용 하 여 경고의 작업 완료, 실패 또는 다른 모든 형식과 같은 알림.

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

새 인스턴스는 `UINotificationFeedbackGenerator` 클래스가 만들어집니다 및 해당 `Prepare` 메서드 haptic 피드백 대기 시간을 최소화할 수 있도록 이루어진다는 것을 시스템에 알리기 위해 호출 됩니다.

`NotificationOccurred` 트리거할 haptic 완료 하기 위해 호출 됩니다는 주어진 `UINotificationFeedbackType` 의:

- `Success`
- `Warning`
- `Error`

<a name="UISelectionFeedbackGenerator" />

### <a name="uiselectionfeedbackgenerator"></a>UISelectionFeedbackGenerator

이 피드백 생성기를 사용 하 여 선택 하는 목록에서 항목을 선택 하는 등 적극적으로 변경 합니다.

다음 코드를 트리거 선택 피드백을 사용 합니다.

```csharp
using UIKit;
...

// Initialize feedback
var selection = new UISelectionFeedbackGenerator ();
selection.Prepare ();

// Trigger feedback
selection.SelectionChanged ();
```

새 인스턴스는 `UISelectionFeedbackGenerator` 클래스가 만들어집니다 및 해당 `Prepare` 메서드 haptic 피드백 대기 시간을 최소화할 수 있도록 이루어진다는 것을 시스템에 알리기 위해 호출 됩니다.

`SelectionChanged` 메서드 다음 haptic 피드백을 트리거합니다.

## <a name="summary"></a>요약

이 문서는 새로운 유형의 haptic 피드백 Xamarin.iOS에 구현 하는 방법과 iOS 10에서 사용할 수 있는 검사가 수행 됩니다.

## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
