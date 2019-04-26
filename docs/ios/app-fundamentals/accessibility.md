---
title: IOS에서 내게 필요한 옵션
description: 이 문서는 다양 한 속성 및 최대한 많은 사용자가 응용 프로그램을 사용할 수 있도록 하려면 사용할 수 있는 기능을 설명 하는 iOS의 내게 필요한 옵션을 설명 합니다.
ms.prod: xamarin
ms.assetid: 88D59B36-05A3-4356-AE29-EC2B69CE7162
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/18/2016
ms.openlocfilehash: aa3e15797ae1dac621ea8a78345044be1387ebaa
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61179609"
---
# <a name="accessibility-on-ios"></a>IOS에서 내게 필요한 옵션

이 페이지에 따라 앱을 빌드할 iOS 내게 필요한 옵션 Api를 사용 하는 방법에 설명 합니다 [접근성 검사 목록](~/cross-platform/app-fundamentals/accessibility.md)합니다.
참조를 [Android 접근성](~/android/app-fundamentals/accessibility.md) 및 [OS X 내게 필요한 옵션](~/mac/app-fundamentals/accessibility.md) 다른 플랫폼 Api에 대 한 페이지입니다.

## <a name="describing-ui-elements"></a>UI 요소를 설명 하는

iOS를 제공 합니다 `AccessibilityLabel` 및 `AccessibilityHint` 속성의 VoiceOver에서 사용할 수 있는 설명 텍스트를 추가 하는 개발자를 위한 화면 판독기 컨트롤을 보다 쉽게 액세스할 수 있도록 합니다. 컨트롤에 액세스할 수 있는 모드 추가 컨텍스트를 제공 하는 하나 이상의 특성을 사용 하 여 태그 될 수 있습니다.

일부 컨트롤 (예는 텍스트 입력 또는 장식용 으로만 사용 되는 이미지의 레이블을)-액세스할 수 있도록 필요 하지 않을 수 있습니다는 `IsAccessibilityElement` 사례만의 내게 필요한 옵션을 사용 하지 않도록 설정 하기 위해 제공 됩니다.

**UI 디자이너**

합니다 **Properties Pad** 이러한 설정은 iOS UI 디자이너에서에서 컨트롤을 선택 하면 편집할 수 있도록 하는 내게 필요한 옵션 섹션을 포함 합니다.

![](accessibility-images/ios-designer-sml.png "내게 필요한 옵션 설정")

**C#**

또한 코드에서 직접 이러한 속성을 설정할 수 있습니다.

```csharp
usernameInput.AccessibilityLabel = "Search";
usernameInput.Hint = "Press Enter after typing to search employee list";
someLabel.IsAccessibilityElement = false;
displayOnlyText.AccessibilityTraits = UIAccessibilityTrait.Header | UIAccessibilityTrait.Selected;
```

### <a name="what-is-accessibilityidentifier"></a>AccessibilityIdentifier 란?

`AccessibilityIdentifier` UIAutomation API를 통해 사용자 인터페이스 요소를 참조할 수 있는 고유 키를 설정 하는 데 사용 됩니다.

변수의 `AccessibilityIdentifier` 음성으로 변환 되거나 사용자에 게 표시 되지 않습니다.

<a name="postnotification" />

## <a name="postnotification"></a>PostNotification

`UIAccessibility.PostNotification` 메서드를 사용 하면 직접 상호 작용 (예: 특정 컨트롤과 상호 작용할 때) 외부에서 사용자에 게 발생 하는 이벤트입니다.

### <a name="announcement"></a>알림

공지 사항 (예: 백그라운드 작업을 완료) 일부 상태가 변경 되었음을 사용자에 게 알려 코드에서 보낼 수 있습니다. 사용자 인터페이스의 시각적 표시 수반 될 수 없습니다.

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.Announcement,
    new NSString(@"Item was saved"));
```

### <a name="layoutchanged"></a>LayoutChanged

`LayoutChanged` 발표 되 면 화면 레이아웃:

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.LayoutChanged,
    someControl);  // someControl gets focus
```


## <a name="accessibility-and-localization"></a>접근성 및 지역화

접근성 속성 레이블 및 힌트는 방금 지역화 될 수와 같은 사용자 인터페이스의 다른 텍스트를 선호 합니다.

**MainStoryboard.strings**

사용자 인터페이스를 스토리 보드에 배치 되 면 동일한 방식으로 다른 속성의 접근성 속성에 대 한 번역을 제공할 수 있습니다. 다음 예제에는 `UITextField` 에 **지역화 ID** 의 `Pqa-aa-ury` 및 스페인어로 설정 하는 두 가지 내게 필요한 옵션 속성:

```csharp
/* Accessibility */
"Pqa-aa-ury.accessibilityLabel" = "Notas input";
"Pqa-aa-ury.accessibilityHint" = "escriba más información";
```

이 파일에 배치 됩니다 합니다 **es.lproj** 스페인어 콘텐츠에 대 한 디렉터리입니다.

**Localizable.strings**

번역에 추가할 수 있습니다 또는 합니다 **Localizable.strings** 디렉터리의 파일에 지역화 된 콘텐츠 (예: **es.lproj** 스페인어):

```csharp
/* Accessibility */
"Notes" = "Notas input";
"Provide more information" = "escriba más información";
```

이러한 번역을 사용할 수 있습니다 C# 를 통해 합니다 `LocalizedString` 메서드.

```csharp
notesText.AccessibilityLabel = NSBundle.MainBundle.LocalizedString ("Notes", "");
notesText.AccessibilityHint = NSBundle.MainBundle.LocalizedString ("Provide more information", "");
```

참조 된 [iOS 지역화 가이드](~/ios/app-fundamentals/localization/index.md) 콘텐츠 지역화에 대 한 자세한 내용은 합니다.

<a name="testing" />

## <a name="testing-accessibility"></a>접근성 테스트

VoiceOver에서 사용 되는 **설정을** 로 이동 하 여 앱 **일반 > 내게 필요한 옵션 > VoiceOver**:

![](accessibility-images/settings-sml.png "말하는 속도 설정합니다.")

합니다 **내게 필요한 옵션** 화면 확대/축소, 텍스트 크기, 색 및 대비 옵션, 음성 설정 및 기타 구성 옵션에 대 한 설정을 제공 합니다.

따르세요 [VoiceOver 지침](https://developer.apple.com/library/ios/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) iOS 장치에서 액세스 가능성을 테스트 합니다.


## <a name="simulator-testing"></a>시뮬레이터 테스트

시뮬레이터에서 테스트 하는 경우는 **액세스 가능성 검사기** 접근성 속성 및 이벤트 올바르게 구성 되었는지 확인 하는 데 사용할 수 있습니다. 검사기를 설정 합니다 **설정을** 로 이동 하 여 앱 **일반 > 내게 필요한 옵션 > 액세스 가능성 검사기**:

![](accessibility-images/settings-inspector-sml.png "액세스 가능성 검사기를 사용 하도록 설정")

사용 하도록 설정 하면 항상 검사기 창 iOS 화면 위로 가져갈 합니다.
다음은 출력의 예제 테이블 보기 행을 선택 하는 경우 – 확인 합니다 **레이블** 의 행과도 하는 것은 "done" 콘텐츠를 제공 하는 문장을 포함 (ie. 눈금 표시 됩니다):

![](accessibility-images/tableview-a11y-sml.png "액세스 가능성 검사기 사용")

관리자가 표시 하는 동안 일시적으로 표시 및 숨기기 오버레이 및 내게 필요한 옵션 설정/해제를 왼쪽 위에 있는 "X" 아이콘을 사용 합니다.



## <a name="related-links"></a>관련 링크

- [플랫폼 간 접근성](~/cross-platform/app-fundamentals/accessibility.md)
- [iOS (Apple) 내게 필요한 옵션](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/iPhoneAccessibility/Accessibility_on_iPhone/Accessibility_on_iPhone.html)
- [iOS VoiceOver](http://www.apple.com/accessibility/ios/voiceover/)
