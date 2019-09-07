---
title: IOS의 내게 필요한 옵션
description: 이 문서에서는 iOS의 접근성에 대해 설명 하 고, 가능한 한 많은 사용자가 응용 프로그램을 사용할 수 있도록 하는 데 사용할 수 있는 다양 한 속성 및 기능을 설명 합니다.
ms.prod: xamarin
ms.assetid: 88D59B36-05A3-4356-AE29-EC2B69CE7162
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/18/2016
ms.openlocfilehash: 943cdfaee07bc4fd4ed3273840036055ad40b89a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70766744"
---
# <a name="accessibility-on-ios"></a>IOS의 내게 필요한 옵션

이 페이지에서는 iOS 접근성 Api를 사용 하 여 [내게 필요한 옵션 검사 목록](~/cross-platform/app-fundamentals/accessibility.md)에 따라 앱을 빌드하는 방법을 설명 합니다.
다른 플랫폼 Api는 [Android 접근성](~/android/app-fundamentals/accessibility.md) 및 [OS X 접근성](~/mac/app-fundamentals/accessibility.md) 페이지를 참조 하세요.

## <a name="describing-ui-elements"></a>UI 요소 설명

iOS는 개발자 `AccessibilityLabel` 가 `AccessibilityHint` 컨트롤에 더 쉽게 액세스할 수 있도록 하는 VoiceOver 화면 판독기에서 사용할 수 있는 설명 텍스트를 추가할 수 있는 및 속성을 제공 합니다. 컨트롤에는 액세스 가능 모드에서 추가 컨텍스트를 제공 하는 하나 이상의 특성으로 태그를 지정할 수도 있습니다.

일부 컨트롤에는 액세스할 필요가 없을 수도 있습니다 (예: 텍스트 입력에 대 한 레이블 또는 장식용 이미지) `IsAccessibilityElement` . 이러한 경우 내게 필요한 옵션을 해제 하기 위해가 제공 됩니다.

**UI 디자이너**

**Properties Pad** 에는 IOS UI 디자이너에서 컨트롤을 선택할 때 이러한 설정을 편집할 수 있도록 하는 내게 필요한 옵션 섹션이 포함 되어 있습니다.

![](accessibility-images/ios-designer-sml.png "내게 필요한 옵션 설정")

**C#**

이러한 속성은 코드에서 직접 설정할 수도 있습니다.

```csharp
usernameInput.AccessibilityLabel = "Search";
usernameInput.Hint = "Press Enter after typing to search employee list";
someLabel.IsAccessibilityElement = false;
displayOnlyText.AccessibilityTraits = UIAccessibilityTrait.Header | UIAccessibilityTrait.Selected;
```

### <a name="what-is-accessibilityidentifier"></a>AccessibilityIdentifier 란?

는 uiautomation API를 통해 사용자 인터페이스 요소를 참조 하는 데 사용할 수 있는 고유 키를 설정 하는 데 사용 됩니다.`AccessibilityIdentifier`

의 `AccessibilityIdentifier` 값은 사용자에 게 음성으로 표시 되거나 표시 되지 않습니다.

<a name="postnotification" />

## <a name="postnotification"></a>PostNotification

`UIAccessibility.PostNotification` 메서드를 사용 하면 직접 상호 작용 (예: 특정 컨트롤과 상호 작용 하는 경우) 외부의 사용자에 게 이벤트를 발생 시킬 수 있습니다.

### <a name="announcement"></a>알림

코드에서 알림을 전송 하 여 일부 상태가 변경 되었음을 사용자에 게 알릴 수 있습니다 (예: 백그라운드 작업이 완료 됨). 이는 사용자 인터페이스의 시각적 표시와 함께 나타날 수 있습니다.

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.Announcement,
    new NSString(@"Item was saved"));
```

### <a name="layoutchanged"></a>LayoutChanged

`LayoutChanged` 알림은 화면 레이아웃을 사용할 때 사용 됩니다.

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.LayoutChanged,
    someControl);  // someControl gets focus
```

## <a name="accessibility-and-localization"></a>접근성 및 지역화

레이블 및 힌트와 같은 내게 필요한 옵션 속성은 사용자 인터페이스의 다른 텍스트와 마찬가지로 지역화할 수 있습니다.

**MainStoryboard.strings**

사용자 인터페이스가 storyboard에서 배치 되는 경우 다른 속성과 같은 방법으로 접근성 속성에 대 한 번역을 제공할 수 있습니다. 아래 예제에서에는 `UITextField` 의 `Pqa-aa-ury` 지역화 ID와 스페인어로 설정 되는 두 개의 액세스 가능성 속성이 있습니다.

```csharp
/* Accessibility */
"Pqa-aa-ury.accessibilityLabel" = "Notas input";
"Pqa-aa-ury.accessibilityHint" = "escriba más información";
```

이 파일은 스페인어 콘텐츠에 대 한 **es. lproj** 디렉터리에 배치 됩니다.

**Localizable.strings**

또는 지역화 된 콘텐츠 디렉터리의 지역화할 수 있는 **문자열** 파일에 번역을 추가할 수 있습니다 (예: .스페인어 용 **lproj** ):

```csharp
/* Accessibility */
"Notes" = "Notas input";
"Provide more information" = "escriba más información";
```

이러한 번역은 메서드를 C# `LocalizedString` 통해에서 사용할 수 있습니다.

```csharp
notesText.AccessibilityLabel = NSBundle.MainBundle.LocalizedString ("Notes", "");
notesText.AccessibilityHint = NSBundle.MainBundle.LocalizedString ("Provide more information", "");
```

콘텐츠 지역화에 대 한 자세한 내용은 [iOS 지역화 가이드](~/ios/app-fundamentals/localization/index.md) 를 참조 하세요.

<a name="testing" />

## <a name="testing-accessibility"></a>접근성 테스트

VoiceOver은 **일반 > 내게 필요한 옵션 > voiceover**으로 이동 하 여 **설정** 앱에서 사용 하도록 설정 됩니다.

![](accessibility-images/settings-sml.png "말하기 요금 설정")

**내게 필요한** 옵션 화면에서는 확대/축소, 텍스트 크기, 색 & 대비 옵션, 음성 설정 및 기타 구성 옵션에 대 한 설정도 제공 합니다.

다음 [VoiceOver 지침](https://developer.apple.com/library/ios/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) 에 따라 iOS 장치에서 액세스 가능성을 테스트 합니다.

## <a name="simulator-testing"></a>시뮬레이터 테스트

시뮬레이터에서 테스트할 때 접근성 **검사기** 를 사용 하 여 내게 필요한 옵션 속성 및 이벤트가 올바르게 구성 되었는지 확인 하는 데 도움이 됩니다. **일반 > 내게 필요한 옵션 > 접근성 검사기**로 이동 하 여 **설정** 앱에서 검사기를 설정 합니다.

![](accessibility-images/settings-inspector-sml.png "내게 필요한 옵션 검사자 사용")

사용 하도록 설정 되 면 검사기 창은 항상 iOS 화면을 가리킵니다.
테이블 뷰 행을 선택 하는 경우의 출력 예는 다음과 같습니다. **레이블에** 행의 내용을 제공 하는 문장이 포함 되 고 "done" (즉, 눈금이 표시 됨) 됩니다.

![](accessibility-images/tableview-a11y-sml.png "접근성 검사자 사용")

검사기가 표시 되는 동안 왼쪽 위에 있는 "X" 아이콘을 사용 하 여 오버레이를 임시로 표시 하 고 숨기고 접근성 설정을 사용/사용 하지 않도록 설정 합니다.

## <a name="related-links"></a>관련 링크

- [플랫폼 간 접근성](~/cross-platform/app-fundamentals/accessibility.md)
- [iOS 접근성 (Apple)](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/iPhoneAccessibility/Accessibility_on_iPhone/Accessibility_on_iPhone.html)
- [iOS VoiceOver](http://www.apple.com/accessibility/ios/voiceover/)
