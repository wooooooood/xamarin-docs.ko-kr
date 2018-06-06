---
title: IOS에서 내게 필요한 옵션
description: 이 문서에서는 다양 한 속성 및 사용할 수 있는 응용 프로그램을 사용할 수 있도록 많은 사용자가 가능한 기능 iOS의 내게 필요한 옵션을 설명 합니다.
ms.prod: xamarin
ms.assetid: 88D59B36-05A3-4356-AE29-EC2B69CE7162
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/18/2016
ms.openlocfilehash: fa85459870211ff26c3bfdd3cc25f722a635952c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783573"
---
# <a name="accessibility-on-ios"></a>IOS에서 내게 필요한 옵션

이 페이지에 따라 앱을 빌드할 iOS 내게 필요한 옵션 Api를 사용 하는 방법에 설명 된 [내게 필요한 옵션 확인 목록](~/cross-platform/app-fundamentals/accessibility.md)합니다.
참조는 [Android 내게 필요한 옵션](~/android/app-fundamentals/accessibility.md) 및 [OS X 내게 필요한 옵션](~/mac/app-fundamentals/accessibility.md) 다른 플랫폼 Api에 대 한 페이지입니다.

## <a name="describing-ui-elements"></a>UI 요소를 설명 하는

iOS 제공는 `AccessibilityLabel` 및 `AccessibilityHint` 속성은 음성 전달 하 여 사용할 수 있는 설명을 추가 하는 개발자를 위한 화면 판독기 컨트롤을 보다 쉽게 액세스할 수 있도록 합니다. 컨트롤은 액세스 가능 모드에서 추가 컨텍스트를 제공 하는 하나 이상의 특성으로 태그 수도 수 있습니다.

일부 컨트롤 필요 하지 않을 (에 대 한 예제, 입력 텍스트 또는 장식용 으로만 사용 되는 이미지의 레이블을)-액세스할 수는 `IsAccessibilityElement` 이러한 경우의 내게 필요한 옵션을 사용 하지 않도록 설정 하기 위해 제공 됩니다.

**UI 디자이너**

**속성 패드** 이러한 설정은 iOS UI 디자이너에서에서 컨트롤을 선택할 때 편집할 수 있도록 하는 내게 필요한 옵션 섹션이 포함:

![](accessibility-images/ios-designer-sml.png "내게 필요한 옵션 설정")

**C#**

코드에서 직접 이러한 속성을 설정할 수도 있습니다.

```csharp
usernameInput.AccessibilityLabel = "Search";
usernameInput.Hint = "Press Enter after typing to search employee list";
someLabel.IsAccessibilityElement = false;
displayOnlyText.AccessibilityTraits = UIAccessibilityTrait.Header | UIAccessibilityTrait.Selected;
```

### <a name="what-is-accessibilityidentifier"></a>AccessibilityIdentifier 란?

`AccessibilityIdentifier` UIAutomation API를 통해 사용자 인터페이스 요소를 가리키는 데 사용할 수 있는 고유 키를 설정 하는 데 사용 됩니다.

값 `AccessibilityIdentifier` 발음 되거나 사용자에 게 표시 되지 않습니다.

<a name="postnotification" />

## <a name="postnotification"></a>PostNotification

`UIAccessibility.PostNotification` 메서드 (예: 특정 컨트롤 상호 작용 하는 경우)의 직접적인 상호 작용 외부 사용자에 게 발생 하는 이벤트를 허용 합니다.

### <a name="announcement"></a>알림

공지 (예: 백그라운드 작업이 완료 된) 일부 상태가 변경 되었음을 사용자에 게에 코드에서 보낼 수 있습니다. 사용자 인터페이스에 시각적 표시 동반 할 수 없습니다.

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.Announcement,
    new NSString(@"Item was saved"));
```

### <a name="layoutchanged"></a>LayoutChanged

`LayoutChanged` 알림 사용 되는 경우 화면 레이아웃:

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.LayoutChanged,
    someControl);  // someControl gets focus
```


## <a name="accessibility-and-localization"></a>내게 필요한 옵션 및 지역화

레이블 및 설명만 지역화할 수와 같은 내게 필요한 옵션 속성 같은 사용자 인터페이스에서 다른 텍스트입니다.

**MainStoryboard.strings**

사용자 인터페이스는 스토리 보드의 배치를 하는 경우에 동일한 방식으로 다른 속성에서 접근성 속성에 대 한 번역을 제공할 수 있습니다. 다음 예제에는 `UITextField` 에 **지역화 ID** 의 `Pqa-aa-ury` 와 스페인어로 설정 되는 두 가지 내게 필요한 옵션 속성:

```csharp
/* Accessibility */
"Pqa-aa-ury.accessibilityLabel" = "Notas input";
"Pqa-aa-ury.accessibilityHint" = "escriba más información";
```

이 파일에 배치할는 **es.lproj** 스페인어 콘텐츠에 대 한 디렉터리입니다.

**Localizable.strings**

번역에 추가할 수 있습니다 또는 **Localizable.strings** 않음 (예: 지역화 된 콘텐츠 디렉터리의 파일 **es.lproj** 스페인어):

```csharp
/* Accessibility */
"Notes" = "Notas input";
"Provide more information" = "escriba más información";
```

이러한 번역을 통해 C#에서 사용할 수는 `LocalizedString` 메서드:

```csharp
notesText.AccessibilityLabel = NSBundle.MainBundle.LocalizedString ("Notes", "");
notesText.AccessibilityHint = NSBundle.MainBundle.LocalizedString ("Provide more information", "");
```

참조는 [iOS 지역화 가이드](~/ios/app-fundamentals/localization/index.md) 에 대 한 자세한 내용은 콘텐츠 지역화 합니다.

<a name="testing" />

## <a name="testing-accessibility"></a>내게 필요한 옵션 테스트

음성 전달을 사용 하는 **설정** 앱으로 이동 하 여 **일반 > 내게 필요한 옵션 > 음성**:

![](accessibility-images/settings-sml.png "읽기 속도 설정합니다.")

**내게 필요한 옵션** 화면 확대/축소, 텍스트 크기, 색 및 대비 옵션, 음성 설정 및 기타 구성 옵션에 대 한 설정을 제공 합니다.

이 따라 [음성 지침](https://developer.apple.com/library/ios/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) iOS 장치에서 내게 필요한 옵션 테스트 합니다.


## <a name="simulator-testing"></a>시뮬레이터 테스트

시뮬레이터에서 테스트할 때는 **내게 필요한 옵션 검사기** 내게 필요한 옵션 속성 및 이벤트 올바르게 구성 되었는지 확인 하는 데 도움이 됩니다. 설정에 관리자는 **설정** 로 이동 하 여 응용 프로그램 **일반 > 내게 필요한 옵션 > 내게 필요한 옵션 검사기**:

![](accessibility-images/settings-inspector-sml.png "내게 필요한 옵션 검사기를 사용 하도록 설정")

활성화 되 면 검사기 창에 항상 iOS 화면 위로 이동할 합니다.
다음은 출력의 예 표 뷰의 행을 선택 하는 워드는 **레이블** 행과도 하는 것은 "done" 콘텐츠를 제공 하는 문장을 포함 (ie. 눈금 표시 되는지):

![](accessibility-images/tableview-a11y-sml.png "내게 필요한 옵션 검사기를 사용 하 여")

관리자가 표시 하는 동안 일시적으로 표시 하 고 오버레이 숨기고 내게 필요한 옵션 설정 사용/사용 안 함 "X" 아이콘 왼쪽 위에서 사용 합니다.



## <a name="related-links"></a>관련 링크

- [플랫폼 간 내게 필요한 옵션](~/cross-platform/app-fundamentals/accessibility.md)
- [iOS (Apple) 내게 필요한 옵션](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/iPhoneAccessibility/Accessibility_on_iPhone/Accessibility_on_iPhone.html)
- [iOS VoiceOver](http://www.apple.com/accessibility/ios/voiceover/)
