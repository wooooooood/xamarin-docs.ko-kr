---
title: tvOS Xamarin에서 사용자 인터페이스 스타일
description: 이 문서에서는 밝은 어두운 UI 테마는 Apple에에 추가한 tvOS 10 및 Xamarin.tvOS 앱에서 구현 하는 방법입니다.
ms.prod: xamarin
ms.assetid: 8BC37683-AD9E-45CD-BE40-96965618AD1D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 2536ca5d3bff3f5b7962bc4fcf58b31a130fd03c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104767"
---
# <a name="tvos-user-interface-styles-in-xamarin"></a>tvOS Xamarin에서 사용자 인터페이스 스타일

_이 문서에서는 밝은 어두운 UI 테마는 Apple에에 추가한 tvOS 10 및 Xamarin.tvOS 앱에서 구현 하는 방법입니다._

tvOS 10 지원은 자동으로 컨트롤에 UIKit의 모든 어둡게 및 밝게 사용자 인터페이스 테마에 맞게, 이제 사용자의 기본 설정을 기반으로 합니다. 또한 개발자는 사용자가 선택한 테마에 따라 UI 요소를 수동으로 조정할 수 있고 지정된 테마를 재정의할 수 있습니다.

<a name="About-the-New-User-Interface-Styles" />

## <a name="about-the-new-user-interface-styles"></a>새 사용자 인터페이스 스타일에 대 한

위에서 설명한 대로 tvOS 10 지원은 자동으로 컨트롤에 UIKit의 모든 어둡게 및 밝게 사용자 인터페이스 테마에 맞게, 이제 사용자의 기본 설정에 기반 합니다.

사용자로 이동 하 여이 테마를 전환할 수 있습니다 **설정을** > **일반** > **모양을** 전환 하 고 **Light**  하 고 **진한**:

[![](user-interface-styles-images/theme01.png "설정 앱")](user-interface-styles-images/theme01.png#lightbox)

경우는 **어두운** 테마를 선택한 경우 모든 사용자 인터페이스 요소에 어두운 배경 밝은 텍스트로 전환 됩니다.

[![](user-interface-styles-images/theme02.png "어두운 테마")](user-interface-styles-images/theme02.png#lightbox)

사용자가 언제 든 지 테마를 전환할 수 있습니다 하 고 Apple TV 위치한 현재 활동에 하루 중 시간에 따라 수행할 수 있습니다.

Light UI 테마는 기본 테마 및 어두운 테마를 활용 하기 위해 10 tvOS에 대 한 수정 되지 않는 한 기존 tvOS 앱 밝은 테마를 사용자의 기본 설정에 관계 없이 계속 사용 합니다. TvOS 10 앱을 현재 테마를 재정의 하 고 항상 Light 또는 어두운 테마를 사용 하 여 UI의 일부 또는 전부에 대 한 수가 있습니다.

<a name="Adopting-the-Light-and-Dark-Themes" />

## <a name="adopting-the-light-and-dark-themes"></a>밝은 영역과 어두운 테마를 채택합니다.

Apple이이 기능을 지원 하기 하기 위한 새로운 API가 추가 된 `UITraitCollection` 클래스 및 tvOS 앱을 옵트인 해야 합니다 어두운 모양을 지원 하려면 (의 설정을 통해 해당 `Info.plist` 파일).

최적화에 밝은 테마와 어두운 테마 지원 하려면 다음 절차를 수행 합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 `Info.plist` 파일을 두 번 클릭하여 엽니다.
2. 선택 된 **원본** (편집기의 맨 아래)에서 뷰.
3. 새 키를 추가 하 고 호출 `UIUserInterfaceStyle`: 

    [![](user-interface-styles-images/theme03.png "UIUserInterfaceStyle 키")](user-interface-styles-images/theme03.png#lightbox)
4. 형식으로 그대로 둡니다 `String` 의 값을 입력 하 고 `Automatic`: 

    [![](user-interface-styles-images/theme04.png "자동 입력")](user-interface-styles-images/theme04.png#lightbox)
5. 파일의 변경 내용을 저장합니다.

세 가지 가능한 값에 대 한는 `UIUserInterfaceStyle` 키:

- **Light** -항상 밝은 테마를 사용 하 여 tvOS 앱의 UI를 강제로 수행 합니다.
- **어두운** -항상 어두운 테마를 사용 하 여 tvOS 앱의 UI를 강제로 수행 합니다.
- **자동** -설정에서 사용자의 취향에 따라 밝은 테마와 어두운 테마 간에 전환 합니다. 이것이 기본 설정입니다.

<a name="UIKit-Theme-Support" />

### <a name="uikit-theme-support"></a>UIKit 테마 지원

표준, 기본 제공 tvOS 앱을 사용 하는 경우 `UIView` 는 자동으로 모든 개발자의 개입 없이 UI 테마를 응답할 컨트롤을 기반으로 합니다.

또한 `UILabel` 고 `UITextView` 선택 UI 테마에 따라 해당 색을 자동으로 변경 됩니다.

- 텍스트는 밝은 테마의 검은색 됩니다.
- 텍스트 어두운 테마의 흰색 됩니다.

변경 되는 경우 개발자는 텍스트 색 수동으로 (중 하나는 스토리 보드 또는 코드에서)이 될 UI 테마에 따라 색 변경 처리를 담당 합니다.

<a name="New-Blur-Effects" />

### <a name="new-blur-effects"></a>새 흐림 효과

TvOS 10 앱에서 밝은 테마와 어두운 테마를 지원 하기 위해 Apple에는 두 가지 새 흐림 효과가 추가 되었습니다. 이러한 새로운 효과 사용자가 다음과 같이 선택한 UI 테마에 따라 흐리게 효과 자동으로 조정:

- `UIBlurEffectStyleRegular` -어두운 테마에서 사용 하 여 밝은 테마에 진한 광원을 흐리게 표시 흐리게 표시 합니다.
- `UIBlurEffectStyleProminent` -밝은 테마의 extra-light는 흐린 및 어두운 테마에는 매우 어두운 흐림 효과 사용합니다.

<a name="Working-with-Trait-Collections" />

## <a name="working-with-trait-collections"></a>특성 (trait) 컬렉션으로 작업

새 `UserInterfaceStyle` 의 속성을 `UITraitCollection` 클래스는 현재 선택한 UI 테마를 가져오는 데 사용할 수 있습니다 및 됩니다는 `UIUserInterfaceStyle` 열거형의 다음 값 중 하나:

- **Light** -Light UI 테마를 선택 합니다.
- **어두운** -어두운 UI 테마를 선택 합니다.
- **지정 되지 않은** -현재 UI 테마 알려진 이므로 뷰 아직 화면에 표시 되지 않은 있습니다.

또한 특성 (trait) 컬렉션 tvOS 10 다음과 같은 기능이 있습니다.
 
- 에 따라 사용자 모양을 프록시를 지정할 수 있습니다는 `UserInterfaceStyle` 의 지정 `UITraitCollection` 이미지 등을 변경 하거나 항목 색 테마를 기반으로 합니다.
- TvOS 앱을 재정의 하 여 특성 컬렉션 변경 내용을 처리할 수는 `TraitCollectionDidChange` 메서드를 `UIView` 또는 `UIViewController` 클래스입니다.

> [!IMPORTANT]
> TvOS 10에 대 한 Xamarin.tvOS 초기 미리 보기는 지원 하지 않습니다 `UIUserInterfaceStyle` 에 대 한 `UITraitCollection` 아직 있습니다. 완벽 하 게 지원 향후 릴리스에서 추가 됩니다.




<a name="Customizing-Appearance-Based-on-Theme" />

### <a name="customizing-appearance-based-on-theme"></a>테마를 기반으로 하는 모양 사용자 지정

모양을 프록시를 지 원하는 사용자 인터페이스 요소에 대 한 모양을 조정할 수 있습니다 해당 특성 (trait) 컬렉션의 UI 테마에 따라. 따라서 지정된 된 UI 요소에 대 한 개발자 밝은 테마에 대 한 한 가지 색 및 어두운 테마에 대 한 다른 색을 지정할 수 있습니다.

```csharp
button.SetTitleColor (UIColor.Red, UIControlState.Normal);

// TODO - Pseudocode because this isn't currently supported in the preview bindings.
var light = new UITraitCollection(UIUserInterfaceStyle.Light);
var dark = new UITraitCollection(UIUserInterfaceStyle.Dark);

button.ForTraitCollection(light).SetTitleColor (UIColor.Red, UIControlState.Normal);
button.ForTraitCollection(dark).SetTitleColor (UIColor.White, UIControlState.Normal);
```

> [!IMPORTANT]
> 아쉽게도 tvOS 10에 대 한 Xamarin.tvOS 미리 보기는 지원 하지 않습니다 `UIUserInterfaceStyle` 에 대 한 `UITraitCollection`이므로이 사용자 지정 유형은 아직 사용할 수 없습니다. 완벽 하 게 지원 향후 릴리스에서 추가 됩니다.

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="responding-to-theme-changes-directly"></a>직접 테마 변경에 응답

개발자에서 필요 정교 하 게 제어할 UI 요소의 모양 선택 UI 테마에 따라, 재정의할 수는 `TraitCollectionDidChange` 메서드를 `UIView` 또는 `UIViewController` 클래스입니다.

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

### <a name="overriding-a-trait-collection"></a>특성 (trait) 컬렉션을 재정의

TvOS 앱의 디자인에 따라 경우도 개발자 해야 할 경우 지정된 된 사용자 인터페이스 요소 특성 (trait) 컬렉션을 재정의 하 고 항상 특정 UI 테마를 사용 하 게 합니다.

이 수행할 수 있습니다 사용 하 여는 `SetOverrideTraitCollection` 메서드는 `UIViewController` 클래스. 예를 들어:

```csharp
// Create new trait and configure it
var trait = new UITraitCollection ();
...

// Apply new trait collection
SetOverrideTraitCollection (trait, this);
```

자세한 내용은 참조 하십시오는 [특성](~/ios/user-interface/storyboards/unified-storyboards.md) 및 [특성 재정의](~/ios/user-interface/storyboards/unified-storyboards.md) 부분 우리의 [통합 스토리 보드 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 설명서.

<a name="Trait-Collections-and-Storyboards" />

### <a name="trait-collections-and-storyboards"></a>특성 (trait) 컬렉션 및 스토리 보드

TvOS 10에서에서 특성 (trait) 컬렉션에 응답 하는 앱의 스토리 보드를 설정할 수 있습니다 및 여러 UI 요소 수 조명 및 어두운 테마를 인식 합니다. 현재 Xamarin.tvOS 초기 미리 보기 tvOS 10에 대 한 지원 하지 않습니다이 기능은 인터페이스 디자이너에서 아직 때문 스토리 보드 문제를 해결 하려면 Xcode의 Interface Builder에서 편집 해야 합니다.

지 원하는 특성 (trait) 컬렉션을 사용 하려면 다음을 수행 합니다.

1. 스토리 보드 파일을 마우스 오른쪽 단추로 클릭 합니다 **솔루션 탐색기** 선택한 **열기** > **Xcode Interface Builder**: 

    [![](user-interface-styles-images/theme05.png "Xcode Interface Builder에서 열기")](user-interface-styles-images/theme05.png#lightbox) 
2. 전환할에서 지 원하는 특성 (trait) 컬렉션을 사용 하려면를 **파일 검사기** 확인 하 고는 **사용 하 여 특성 (trait) 변형** 속성에는 **인터페이스 Builder 문서** 섹션: 

    [![](user-interface-styles-images/theme06.png "지 원하는 특성 (trait) 컬렉션을 사용 하도록 설정")](user-interface-styles-images/theme06.png#lightbox)
3. 특성 (trait) 변형을 사용 하 여 변경 내용을 확인 합니다. 

    [![](user-interface-styles-images/theme07.png "특성 (trait) 변형 경고 사용")](user-interface-styles-images/theme07.png#lightbox)
4. 스토리 보드 파일에 저장 합니다.

Apple이 Interface Builder에서 tvOS 스토리 보드를 편집 하는 경우에 다음과 같은 기능을 추가:

* 개발자는 다양 한 사용자 인터페이스 요소에서 UI 테마에 따라 지정할 수 있습니다 합니다 **특성 검사기**:
    
    * 이제 몇 가지 속성을 **+** UI 테마 특정 버전을 추가 하려면 클릭할 수 있는 그 옆에: 

        [![](user-interface-styles-images/theme08.png "UI 테마 특정 버전을 추가 합니다.")](user-interface-styles-images/theme08.png#lightbox) 
    
    * 개발자는 새 속성을 지정 하거나 클릭 수 있습니다 합니다 **x** 단추 제거 하려면: 

        [![](user-interface-styles-images/theme09.png "새 속성을 지정 하거나 제거 하려면 x 단추를 클릭 합니다.")](user-interface-styles-images/theme09.png#lightbox)
* 개발자는 Interface Builder 내에서 표시등 또는 어두운 테마에서 UI 디자인을 미리 볼 수 있습니다.
    
    * 디자인 화면 아래쪽에는 현재 UI 테마를 전환 하려면 개발자를 허용 합니다. 

        [![](user-interface-styles-images/theme10.png "디자인 화면의 맨 아래")](user-interface-styles-images/theme10.png#lightbox)
        
    * 새 테마를 Interface Builder에서 표시 되 고 모든 특성 (trait) 컬렉션 특정 조정을 표시 됩니다. 

        [![](user-interface-styles-images/theme11.png "Interface Builder에서 표시 하는 테마")](user-interface-styles-images/theme11.png#lightbox)

또한 tvOS 시뮬레이터는 이제 개발자 tvOS 앱을 디버깅할 때 밝은 테마와 어두운 테마 간에 빠르게 전환할 수 있도록 하려면 바로 가기 키를 있습니다. 사용 합니다 **명령-Shift-D** 키보드 밝은 테마와 어두운 사이 전환 하려면 시퀀스입니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 이러한 부분이 광원 어두운 UI 테마는 Apple에에 추가한 tvOS 10 및 Xamarin.tvOS 앱에서 구현 하는 방법입니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [TvOS 10의에서 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
