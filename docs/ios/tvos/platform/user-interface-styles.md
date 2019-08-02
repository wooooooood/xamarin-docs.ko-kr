---
title: Xamarin에서 tvOS 사용자 인터페이스 스타일
description: 이 문서에서는 Apple이 tvOS 10에 추가 하 고 tvOS 앱에서이를 구현 하는 방법에 추가 된 밝은 UI 및 어두운 UI 테마를 다룹니다.
ms.prod: xamarin
ms.assetid: 8BC37683-AD9E-45CD-BE40-96965618AD1D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: fc9a8acb2c04d805e7abc52b2996ec5546b0fb69
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68657364"
---
# <a name="tvos-user-interface-styles-in-xamarin"></a>Xamarin에서 tvOS 사용자 인터페이스 스타일

_이 문서에서는 Apple이 tvOS 10에 추가 하 고 tvOS 앱에서이를 구현 하는 방법에 추가 된 밝은 UI 및 어두운 UI 테마를 다룹니다._

이제 tvOS 10은 사용자의 기본 설정에 따라 모든 기본 UIKit 컨트롤이 자동으로 조정 되는 짙은 사용자 인터페이스 테마를 모두 지원 합니다. 또한 개발자는 사용자가 선택한 테마를 기반으로 UI 요소를 수동으로 조정 하 여 지정 된 테마를 재정의할 수 있습니다.

<a name="About-the-New-User-Interface-Styles" />

## <a name="about-the-new-user-interface-styles"></a>새 사용자 인터페이스 스타일 정보

위에서 설명한 것 처럼 tvOS 10은 이제 사용자의 기본 설정에 따라 모든 기본 UIKit 컨트롤이 자동으로 조정 되는 짙은 사용자 인터페이스 테마를 모두 지원 합니다.

사용자는 **설정** > **일반**모양으로 이동 하 고 밝은와 어둡게 간을 전환 하 여이 테마를 전환할 수 있습니다. > 

[![](user-interface-styles-images/theme01.png "설정 앱")](user-interface-styles-images/theme01.png#lightbox)

**어두운** 테마를 선택 하면 모든 사용자 인터페이스 요소가 어두운 배경의 밝은 텍스트로 전환 됩니다.

[![](user-interface-styles-images/theme02.png "어두운 테마")](user-interface-styles-images/theme02.png#lightbox)

사용자는 언제 든 지 테마를 전환할 수 있으며, Apple TV가 있는 현재 작업 또는 시간을 기준으로이 작업을 수행할 수 있습니다.

연한 UI 테마는 기본 테마 이며, tvOS 10에 대 한 수정 된 경우를 제외 하 고는 어두운 테마를 활용 하기 위해 기존 tvOS 앱은 사용자의 기본 설정에 관계 없이 밝은 테마를 계속 사용 합니다. 또한 tvOS 10 앱은 현재 테마를 재정의 하 고 UI의 일부 또는 전체에 대해 항상 밝은 테마 또는 어두운 테마를 사용 합니다.

<a name="Adopting-the-Light-and-Dark-Themes" />

## <a name="adopting-the-light-and-dark-themes"></a>밝은 테마 및 어두운 테마 도입

이 기능을 지원 하기 위해 Apple은 `UITraitCollection` 클래스에 새 API를 추가 했으며 tvOS 앱은 해당 `Info.plist` 파일의 설정을 통해 짙은 모양을 지원 하기 위해 옵트인 해야 합니다.

밝은 테마와 어두운 테마 지원을 옵트인 (opt in) 하려면 다음을 수행 합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 `Info.plist` 파일을 두 번 클릭하여 엽니다.
2. 편집기의 맨 아래에서 **원본** 뷰를 선택 합니다.
3. 새 키를 추가 하 고 호출 `UIUserInterfaceStyle`합니다. 

    [![](user-interface-styles-images/theme03.png "UIUserInterfaceStyle 키")](user-interface-styles-images/theme03.png#lightbox)
4. 형식을로 `String` 설정 된 채로 두고 `Automatic`값을 입력 합니다. 

    [![](user-interface-styles-images/theme04.png "자동으로 입력")](user-interface-styles-images/theme04.png#lightbox)
5. 파일의 변경 내용을 저장합니다.

`UIUserInterfaceStyle` 키에 대해 다음과 같은 세 가지 값을 사용할 수 있습니다.

- **Light** -tvOS 앱의 UI에서 항상 밝은 테마를 사용 하도록 합니다.
- **어두움** -tvOS 앱의 UI에서 항상 짙은 테마를 사용 하도록 합니다.
- **자동** -설정의 사용자 기본 설정에 따라 밝은 테마와 어두운 테마 사이를 전환 합니다. 기본 설정입니다.

<a name="UIKit-Theme-Support" />

### <a name="uikit-theme-support"></a>UIKit 테마 지원

TvOS 앱이 표준 기본 제공 `UIView` 기반 컨트롤을 사용 하는 경우 개발자 개입 없이 UI 테마에 자동으로 응답 합니다.

또한 및 `UILabel` `UITextView` 은 선택 UI 테마에 따라 색을 자동으로 변경 합니다.

- 밝은 테마에서 텍스트가 검정색으로 나타납니다.
- 어두운 테마의 텍스트는 흰색으로 나타납니다.

개발자가 스토리 보드 또는 코드에서 텍스트 색을 수동으로 변경 하면 UI 테마에 따라 색 변경을 처리 하는 일을 담당 하 게 됩니다.

<a name="New-Blur-Effects" />

### <a name="new-blur-effects"></a>새 흐림 효과

TvOS 10 앱에서 밝은 테마와 어두운 테마를 지원 하기 위해 Apple에서는 두 개의 새로운 흐림 효과를 추가 했습니다. 이러한 새 효과는 다음과 같이 사용자가 선택한 UI 테마에 따라 자동으로 흐림 효과를 조정 합니다.

- `UIBlurEffectStyleRegular`-밝은 테마에서 밝은 흐림 효과를 사용 하 고 어두운 테마에서 짙은 흐림 효과를 사용 합니다.
- `UIBlurEffectStyleProminent`-밝은 테마에서 매우 밝은 흐림 효과를 사용 하 고 어두운 테마에서 짙은 짙은 흐림 효과를 사용 합니다.

<a name="Working-with-Trait-Collections" />

## <a name="working-with-trait-collections"></a>성분 컬렉션 작업

클래스의 새 `UserInterfaceStyle` 속성을 사용 하 여 현재 선택 된 UI `UIUserInterfaceStyle` 테마를 가져올 수 있으며, 다음 값 중 하나의 열거형이 됩니다. `UITraitCollection`

- **밝게** -밝은 UI 테마가 선택 됩니다.
- **어두움** -짙은 UI 테마가 선택 됩니다.
- **지정** 되지 않음-뷰가 아직 화면에 표시 되지 않았으므로 현재 UI 테마를 알 수 없습니다.

또한 특성 컬렉션은 tvOS 10에서 다음과 같은 기능을 제공 합니다.
 
- 테마에 따라 이미지나 항목 색과 같은 항목 `UserInterfaceStyle` 을 변경 하기 `UITraitCollection` 위해 지정 된의을 기반으로 모양 프록시를 사용자 지정할 수 있습니다.
- TvOS 앱은 `TraitCollectionDidChange` `UIView` 또는 `UIViewController` 클래스의 메서드를 재정의 하 여 특성 컬렉션 변경 내용을 처리할 수 있습니다.

> [!IMPORTANT]
> TvOS 10에 대 한 tvOS 초기 미리 보기는 `UIUserInterfaceStyle` `UITraitCollection` 아직 완전히 지원 되지 않습니다. 이후 릴리스에서는 전체 지원이 추가 될 예정입니다.




<a name="Customizing-Appearance-Based-on-Theme" />

### <a name="customizing-appearance-based-on-theme"></a>테마를 기반으로 모양 사용자 지정

모양 프록시를 지 원하는 사용자 인터페이스 요소의 경우 해당 특성 컬렉션의 UI 테마에 따라 모양을 조정할 수 있습니다. 따라서 지정 된 UI 요소에 대해 개발자는 밝은 테마에 대해 색을 하나 지정 하 고 짙은 테마에 다른 색을 지정할 수 있습니다.

```csharp
button.SetTitleColor (UIColor.Red, UIControlState.Normal);

// TODO - Pseudocode because this isn't currently supported in the preview bindings.
var light = new UITraitCollection(UIUserInterfaceStyle.Light);
var dark = new UITraitCollection(UIUserInterfaceStyle.Dark);

button.ForTraitCollection(light).SetTitleColor (UIColor.Red, UIControlState.Normal);
button.ForTraitCollection(dark).SetTitleColor (UIColor.White, UIControlState.Normal);
```

> [!IMPORTANT]
> 아쉽게도 tvOS 10에 대 한 tvOS 미리 보기는에 대해 `UIUserInterfaceStyle` `UITraitCollection`완벽 하 게 지원 되지 않으므로이 유형의 사용자 지정은 아직 사용할 수 없습니다. 이후 릴리스에서는 전체 지원이 추가 될 예정입니다.

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="responding-to-theme-changes-directly"></a>테마 변경 내용에 직접 응답

개발자는 선택 된 ui 테마를 기반으로 하는 ui 요소의 모양을 세부적으로 제어 해야 합니다. `TraitCollectionDidChange` `UIView` 또는 `UIViewController` 클래스의 메서드를 재정의할 수 있습니다.

예를 들어:

```csharp
public override void TraitCollectionDidChange (UITraitCollection previousTraitCollection)
{
    base.TraitCollectionDidChange (previousTraitCollection);
    
    // Take action based on the Light or Dark theme
    ...
}
```

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="overriding-a-trait-collection"></a>성분 컬렉션 재정의

TvOS 앱의 디자인에 따라 개발자가 지정 된 사용자 인터페이스 요소의 특성 컬렉션을 재정의 하 고 항상 특정 UI 테마를 사용 해야 하는 경우가 있을 수 있습니다.

`UIViewController` 클래스의 메서드를 `SetOverrideTraitCollection` 사용 하 여이 작업을 수행할 수 있습니다. 예를 들어:

```csharp
// Create new trait and configure it
var trait = new UITraitCollection ();
...

// Apply new trait collection
SetOverrideTraitCollection (trait, this);
```

자세한 내용은 [통합 Storyboard 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 설명서의 [특성](~/ios/user-interface/storyboards/unified-storyboards.md) 및 [재정의 특성](~/ios/user-interface/storyboards/unified-storyboards.md) 섹션을 참조 하세요.

<a name="Trait-Collections-and-Storyboards" />

### <a name="trait-collections-and-storyboards"></a>특성 컬렉션 및 Storyboard

TvOS 10에서는 특성 컬렉션에 응답 하도록 앱의 스토리 보드를 설정 하 고 다양 한 UI 요소를 밝은 테마와 어두운 테마를 인식할 수 있습니다. TvOS 10에 대 한 현재 tvOS 초기 미리 보기는 아직 인터페이스 디자이너에서이 기능을 지원 하지 않으므로 스토리 보드를 Xcode의 Interface Builder에 대 한 해결 방법으로 편집 해야 합니다.

성분 컬렉션 지원을 사용 하도록 설정 하려면 다음을 수행 합니다.

1. **솔루션 탐색기** 에서 스토리 보드 파일을 마우스 오른쪽 단추로 클릭 하 고**Xcode Interface Builder** **를 사용 하 여** > 열기를 선택 합니다. 

    [![](user-interface-styles-images/theme05.png "Xcode를 사용 하 여 열기 Interface Builder")](user-interface-styles-images/theme05.png#lightbox) 
2. 성분 컬렉션 지원을 사용 하려면 **파일 검사자** 로 전환 하 고 **Interface Builder 문서** 섹션에서 **특성 변형 사용** 속성을 선택 합니다. 

    [![](user-interface-styles-images/theme06.png "성분 컬렉션 지원 사용")](user-interface-styles-images/theme06.png#lightbox)
3. 특성 변형을 사용 하기 위해 변경 내용을 확인 합니다. 

    [![](user-interface-styles-images/theme07.png "특성 변형 사용 경고")](user-interface-styles-images/theme07.png#lightbox)
4. 스토리 보드 파일의 변경 내용을 저장 합니다.

Apple은 Interface Builder에서 tvOS Storyboard를 편집할 때 다음과 같은 기능을 추가 했습니다.

* 개발자는 **특성 검사자**에서 UI 테마를 기반으로 사용자 인터페이스 요소의 다른 변형을 지정할 수 있습니다.
    
    * 이제 몇 가지 속성은 **+** 클릭 하 여 UI 테마 특정 버전을 추가할 수 있습니다. 

        [![](user-interface-styles-images/theme08.png "UI 테마 특정 버전 추가")](user-interface-styles-images/theme08.png#lightbox) 
    
    * 개발자는 새 속성을 지정 하거나 **x** 단추를 클릭 하 여 제거할 수 있습니다. 

        [![](user-interface-styles-images/theme09.png "새 속성을 지정 하거나 x 단추를 클릭 하 여 제거 합니다.")](user-interface-styles-images/theme09.png#lightbox)
* 개발자는 Interface Builder 내에서 밝은 테마 또는 어두운 테마 중 하나에서 UI 디자인을 미리 볼 수 있습니다.
    
    * Design Surface의 맨 아래에서 개발자는 현재 UI 테마를 전환할 수 있습니다. 

        [![](user-interface-styles-images/theme10.png "Design Surface의 아래쪽입니다.")](user-interface-styles-images/theme10.png#lightbox)
        
    * 새 테마는 Interface Builder 표시 되 고 특성 컬렉션 특정 조정 내용이 표시 됩니다. 

        [![](user-interface-styles-images/theme11.png "에 표시 되는 테마 Interface Builder")](user-interface-styles-images/theme11.png#lightbox)

또한 tvOS 시뮬레이터에는 tvOS 앱을 디버그할 때 개발자가 밝은 테마와 어두운 테마 간을 빠르게 전환할 수 있는 바로 가기 키가 있습니다. **Command Shift-D** 키보드 시퀀스를 사용 하 여 밝은와 어둡게 사이를 전환 합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Apple이 tvOS 10에 추가 하 고 tvOS 앱에서이를 구현 하는 방법에 추가 된 밝은 UI 및 어두운 UI 테마를 살펴보았습니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [TvOS 10의 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
