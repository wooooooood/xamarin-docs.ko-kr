---
title: "새로운 사용자 인터페이스 스타일"
description: "이 문서에서는 밝은 및 어두운 UI 테마는 Apple는 tvOS에 추가 10과 Xamarin.tvOS 응용 프로그램에서 구현 하는 방법."
ms.topic: article
ms.prod: xamarin
ms.assetid: 8BC37683-AD9E-45CD-BE40-96965618AD1D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: e400a72f4c759662e70bfecc372134f8fda05ad6
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="new-user-interface-styles"></a>새로운 사용자 인터페이스 스타일

_이 문서에서는 밝은 및 어두운 UI 테마는 Apple는 tvOS에 추가 10과 Xamarin.tvOS 응용 프로그램에서 구현 하는 방법._

tvOS 10 지원이 자동으로 제어에 빌드 UIKit의 모든 Light 사용자 인터페이스와 어두운 테마 여건에 맞춰 적용, 이제 사용자의 기본 설정을 기반으로 합니다. 또한 개발자는 사용자가 선택한 테마에 따라 UI 요소를 수동으로 조정할 수 하 고 지정 된 테마를 재정의할 수 있습니다.


<a name="About-the-New-User-Interface-Styles" />

## <a name="about-the-new-user-interface-styles"></a>새 사용자 인터페이스 스타일에 대 한

위에서 설명 했 듯이 tvOS 10 지원이 자동으로 제어에 빌드 UIKit의 모든 Light 사용자 인터페이스와 어두운 테마 여건에 맞춰 적용, 이제 사용자의 기본 설정에 따라 합니다.

사용자를 이동 하 여이 테마를 전환할 수 **설정** > **일반** > **모양** 간 전환 **Light**  및 **어두운**:

[![](user-interface-styles-images/theme01.png "설정 앱")](user-interface-styles-images/theme01.png#lightbox)

경우는 **어두운** 테마를 선택, 모든 사용자 인터페이스 요소에 어두운 화면에 밝은 텍스트로 전환 됩니다.

[![](user-interface-styles-images/theme02.png "어두운 테마")](user-interface-styles-images/theme02.png#lightbox)

사용자가 언제 든 지 테마를 전환할 수 있습니다 하 고 Apple TV 위치한 현재 활동 또는 하루의 시간에 기반을 수행할 수 있습니다.

Light UI 테마는 기본 테마 및 어두운 테마를 활용 하는 10 tvOS에 대 한 수정 하지 않는 한 모든 기존 tvOS 앱 사용자의 기본 설정에 관계 없이 밝은 테마를 여전히 사용 합니다. TvOS 10 앱에는 현재 테마를 무시 하 고 항상 일부 또는 전체 UI Light 또는 어두운 테마를 사용 하는 기능을 있습니다.

<a name="Adopting-the-Light-and-Dark-Themes" />

## <a name="adopting-the-light-and-dark-themes"></a>밝은 영역과 어두운 테마를 채택합니다.

이 기능을 지원 하기 위해 Apple에 새로운 API를 추가 했습니다는 `UITraitCollection` 클래스 및 tvOS 앱 해야 옵트인 어두운 모양을 지원 하기 위해 (의 설정을 통해 해당 `Info.plist` 파일).

에 옵트인 밝은 테마와 어두운 테마 지원, 하려면 다음을 수행 합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 `Info.plist` 파일을 두 번 클릭하여 엽니다.
2. 선택 된 **소스** (편집기의 맨 아래)에서 보기.
3. 새 키를 추가 하 고 호출할 `UIUserInterfaceStyle`: 

    [![](user-interface-styles-images/theme03.png "UIUserInterfaceStyle 키")](user-interface-styles-images/theme03.png#lightbox)
4. 유형 설정은 둡니다 `String` 의 값을 입력 하 고 `Automatic`: 

    [![](user-interface-styles-images/theme04.png "자동 입력")](user-interface-styles-images/theme04.png#lightbox)
5. 파일의 변경 내용을 저장합니다.

에 대 한 세 가지 가능한 값은 `UIUserInterfaceStyle` 키:

- **밝은** -항상 밝은 테마를 사용 하도록 tvOS 응용 프로그램의 UI를 강제 합니다.
- **어두운** -항상 어두운 테마를 사용 하도록 tvOS 응용 프로그램의 UI를 강제 합니다.
- **자동** -설정에서 사용자의 기본 설정에 따라 밝은 테마와 어두운 테마 사이 전환 합니다. 이것이 기본 설정입니다.

<a name="UIKit-Theme-Support" />

### <a name="uikit-theme-support"></a>UIKit 테마 지원

표준, 기본 제공 tvOS 앱 사용 하는 경우 `UIView` 기반 컨트롤에는 개발자의 개입 없이 UI 테마를 자동으로 응답 합니다.

또한 `UILabel` 및 `UITextView` 선택 UI 테마에 따라 색을 자동으로 변경 됩니다.

- 텍스트 밝은 테마의 검은색 됩니다.
- 텍스트 어두운 테마의 흰색 됩니다.

변경 되는 경우 개발자 텍스트 색 수동으로 (중 하나는 스토리 보드 또는 코드에서)를 UI 테마에 따라 색 변경 내용을 처리 됩니다.

<a name="New-Blur-Effects" />

### <a name="new-blur-effects"></a>새 흐림 효과

밝은 테마와 어두운 테마를 tvOS 10 응용 프로그램에서 지 원하는 대 한 Apple 두 개의 새 흐림 효과 추가 했습니다. 이러한 새 효과 다음과 같이 사용자가 선택한 UI 테마에 따라 흐림 효과 자동 조정 됩니다.

- `UIBlurEffectStyleRegular` -어두운 테마에서 사용 하 여 광원 흐림 밝은 테마 및 어두운은 흐림 효과.
- `UIBlurEffectStyleProminent` -밝은 테마의 매우는 흐림 효과 어두운 테마의 한 3 차 어두운 흐림 효과 사용합니다.

<a name="Working-with-Trait-Collections" />

## <a name="working-with-trait-collections"></a>특성 컬렉션을 사용한 작업

새 `UserInterfaceStyle` 의 속성은 `UITraitCollection` 클래스는 현재 선택한 UI 테마를 가져오는 데 사용할 수 있습니다 및 됩니다는 `UIUserInterfaceStyle` 열거형의 다음 값 중 하나:

- **밝은** -Light UI 테마를 선택 합니다.
- **어두운** -UI 어두운 테마를 선택 합니다.
- **지정 되지 않은** -하므로 현재 UI 테마를 알 수 없는 The 보기가 아직 화면에 표시 된 되지 않습니다.

또한 특성 컬렉션 tvOS 10에에서는 다음과 같은 기능이 포함 되어:
 
- 에 따라 모양을 프록시 사용자 지정할 수는 `UserInterfaceStyle` 의 주어진 `UITraitCollection` 이미지 등을 변경 하거나 테마에 따라 색 항목입니다.
- TvOS 앱 재정의 하 여 특성 컬렉션 변경 사항을 처리할 수는 `TraitCollectionDidChange` 의 메서드는 `UIView` 또는 `UIViewController` 클래스입니다.

> [!IMPORTANT]
> 10 tvOS에 대 한 Xamarin.tvOS 초기 미리 보기는 지원 하지 않습니다 `UIUserInterfaceStyle` 에 대 한 `UITraitCollection` 아직 합니다. 이후 릴리스에서 완벽 한 지원 추가 됩니다.




<a name="Customizing-Appearance-Based-on-Theme" />

### <a name="customizing-appearance-based-on-theme"></a>테마에 따라 모양 사용자 지정

지 원하는 모양을 프록시 사용자 인터페이스 요소에 대 한 모양을 조정할 수 있습니다는 특성 컬렉션의 UI 테마에 따라. 따라서 지정된 된 UI 요소에 대 한 개발자 밝은 테마에 대 한 한 가지 색 및 어두운 테마에 대 한 다른 색을 지정할 수 있습니다.

```csharp
button.SetTitleColor (UIColor.Red, UIControlState.Normal);

// TODO - Pseudocode because this isn't currently supported in the preview bindings.
var light = new UITraitCollection(UIUserInterfaceStyle.Light);
var dark = new UITraitCollection(UIUserInterfaceStyle.Dark);

button.ForTraitCollection(light).SetTitleColor (UIColor.Red, UIControlState.Normal);
button.ForTraitCollection(dark).SetTitleColor (UIColor.White, UIControlState.Normal);
```

> [!IMPORTANT]
> 그러나 10 tvOS에 대 한 Xamarin.tvOS 미리 보기는 지원 하지 않습니다 `UIUserInterfaceStyle` 에 대 한 `UITraitCollection`되었으므로이 유형의 사용자 지정에서 사용할 수 없습니다. 이후 릴리스에서 완벽 한 지원 추가 됩니다.

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="responding-to-theme-changes-directly"></a>직접 테마 변경 내용에 응답

개발자는 UI 요소의 모양에 대 한 깊은 제어 선택한 UI 테마에 따라, 재정의할 수 필요는 `TraitCollectionDidChange` 의 메서드는 `UIView` 또는 `UIViewController` 클래스입니다.

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

### <a name="overriding-a-trait-collection"></a>특성 컬렉션을 재정의합니다.

TvOS 앱의 디자인에 따라, 경우도 개발자는 특정 사용자 인터페이스 요소의 특성 컬렉션을 무시 하 고 항상 특정 UI 테마를 사용 하 게 필요로 하는 경우.

수행할 수 있습니다를 사용 하는 `SetOverrideTraitCollection` 에서 메서드는 `UIViewController` 클래스. 예를 들어:

```csharp
// Create new trait and configure it
var trait = new UITraitCollection ();
...

// Apply new trait collection
SetOverrideTraitCollection (trait, this);
```

자세한 내용은 참조 하십시오는 [특성](~/ios/user-interface/storyboards/unified-storyboards.md) 및 [재정의 특성](~/ios/user-interface/storyboards/unified-storyboards.md) 의 섹션 우리의 [스토리 보드 통합 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 설명서입니다.

<a name="Trait-Collections-and-Storyboards" />

### <a name="trait-collections-and-storyboards"></a>특성 컬렉션 및 스토리 보드

10 tvOS에 특성 컬렉션에 응답 하는 응용 프로그램의 스토리 보드를 설정할 수 있습니다 및 많은 UI 요소가 수 밝은 테마와 어두운 테마 인식할 수입니다. 현재 Xamarin.tvOS 초기 미리 보기 10 tvOS에 대 한 지원 하지 않습니다이 기능 인터페이스 디자이너에서 아직 때문 스토리 보드 해결 방법으로 Xcode의 인터페이스 작성기에서 편집 해야 합니다.

특성 컬렉션 지원 기능을 사용 하도록 설정 하려면 다음을 수행 합니다.

1. 스토리 보드 파일을 마우스 오른쪽 단추로 클릭 합니다.는 **솔루션 탐색기** 선택 **프로그램** > **Xcode 인터페이스 작성기**: 

    [![](user-interface-styles-images/theme05.png "Xcode 인터페이스 작성기로 열기")](user-interface-styles-images/theme05.png#lightbox) 
2. 지 원하는 특성 컬렉션을 사용 하도록 설정 하려면 전환는 **파일 검사기** 확인 하 고는 **성분 변형을 사용 하 여** 속성에는 **인터페이스 작성기 문서** 섹션: 

    [![](user-interface-styles-images/theme06.png "특성 컬렉션입니다. 지원 사용")](user-interface-styles-images/theme06.png#lightbox)
3. 특성 변경을 사용 하 여 변경 내용을 확인 합니다. 

    [![](user-interface-styles-images/theme07.png "특성 변형 경고 사용")](user-interface-styles-images/theme07.png#lightbox)
4. 스토리 보드 파일에 저장 합니다.

Apple이 인터페이스 작성기의 tvOS 스토리 보드를 편집할 때 다음을 추가.

* 개발자의 사용자 인터페이스 요소에서 UI 테마에 따라 다른 버전을 지정할 수는 **특성 검사기**:
    
    * 여러 속성에는 이제는  **+**  UI 테마 특정 버전을 추가 하려면 클릭할 수 있는 그 옆에: 

        [![](user-interface-styles-images/theme08.png "UI 테마 특정 버전 추가")](user-interface-styles-images/theme08.png#lightbox) 
    
    * 개발자는 새 속성을 지정 하거나 클릭 수는 **x** 단추 제거 하려면: 

        [![](user-interface-styles-images/theme09.png "새 속성을 지정 하거나 제거 하려면 x 단추를 클릭")](user-interface-styles-images/theme09.png#lightbox)
* 개발자는 인터페이스 작성기 내에서 Light 또는 어두운 테마의 UI 디자인 미리 볼 수 있습니다.
    
    * 디자인 화면 아래쪽을 사용 하면 개발자는 현재 UI 테마를 전환 하려면: 

        [![](user-interface-styles-images/theme10.png "디자인 화면 아래쪽")](user-interface-styles-images/theme10.png#lightbox)
        
    * 새 테마 인터페이스 작성기에 표시 되 고 특성 컬렉션의 특정 조정한 표시 됩니다. 

        [![](user-interface-styles-images/theme11.png "인터페이스 작성기에 표시 되는 테마")](user-interface-styles-images/theme11.png#lightbox)

또한 tvOS 시뮬레이터에는 이제 개발자는 tvOS 응용 프로그램을 디버깅할 때 밝은 테마와 어두운 테마 사이 빠르게 전환할 수 있도록 하려면 바로 가기 키를 있습니다. 사용 하 여는 **명령 Shift 차원** 키보드 시퀀스 밝은 테마와 어두운 사이 전환 합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서를 생성할 빛 검사가 및 Xamarin.tvOS 응용 프로그램에 구현 하는 방법과 10 어두운 UI 테마는 Apple는 tvOS에 추가 합니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [TvOS 10의에서 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
