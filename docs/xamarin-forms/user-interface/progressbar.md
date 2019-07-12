---
title: Xamarin.Forms ProgressBar
description: Xamarin.Forms ProgressBar는 가로 막대 채워진 float 속성을 기준으로 진행률을 시각적으로 나타내는 컨트롤입니다.
ms.prod: xamarin
ms.assetId: C2F85FED-797C-466B-A0FD-E73CFB79B267
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/09/2019
ms.openlocfilehash: 2e2917077575ebc78dbdda8ae55aa230a39a7ba1
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67837013"
---
# <a name="xamarinforms-progressbar"></a>Xamarin.Forms ProgressBar
[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ProgressBarDemos)

Xamarin.Forms [ `ProgressBar` ](xref:Xamarin.Forms.ProgressBar) 는 가로 막대를 나타내는 백분율로 채워진으로 진행률을 시각적으로 나타내는 컨트롤을 `float` 값입니다. 합니다 `ProgressBar` 클래스에서 상속 [ `View` ](xref:Xamarin.Forms.View)합니다.

다음 스크린 샷에 표시 된 `ProgressBar` iOS 및 Android에서:

![IOS 및 Android에서 ProgressBar의 스크린 샷](progressbar-images/progressbars-default.png "iOS 및 Android에서 ProgressBar")

`ProgressBar` 컨트롤 두 속성을 정의 합니다.

* [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress) 가 `float` 값으로 1로 0에서 현재 진행 상태를 나타내는 값입니다. `Progress` 0 0, 1 1 범위로 고정 됩니다 됩니다 보다 큰 값으로 고정 됩니다 보다 작은 값입니다.
* [`ProgressColor`](xref:Xamarin.Forms.ProgressBar.ProgressColor) `Color` 내부에 영향을 미치는 막대 색 현재 진행 상태를 나타냅니다.

이러한 속성에 의해 지원 됩니다 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 의미 하는 개체는 `ProgressBar` 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

`ProgressBar` 컨트롤 정의 `ProgressTo` 막대 현재 값에서 지정된 된 값에 애니메이션을 적용 하는 메서드. 자세한 내용은 [ProgressBar에 애니메이션 효과 주기](#animate-a-progressbar)합니다.

> [!NOTE]
> `ProgressBar` 컨트롤을 선택 하려면 Tab 키를 사용 하는 경우 건너뛰도록 하므로 사용자 조작을 허용 하지 않습니다.

## <a name="create-a-progressbar"></a>ProgressBar를 만들려면

`ProgressBar` XAML에서 인스턴스화할 수 있습니다. 해당 `Progress` 내부, 색이 지정 된 막대의 채우기 비율을 확인 하려면 속성을 설정할 수 있습니다. 경우는 `Progress` 속성이 설정 되어 있지 않으므로 기본값인 0. 다음 예제에서는 인스턴스화하는 방법을 보여 줍니다는 `ProgressBar` 선택적 XAML에서 `Progress` 속성 집합:

```xaml
<ProgressBar Progress="0.5" />
```

`ProgressBar` 코드에서 만들 수도 있습니다.

```csharp
ProgressBar progressBar = new ProgressBar { Progress = 0.5f };
```

> [!WARNING]
> 제한 되지 않은 가로 레이아웃 옵션을 같은 사용 하지 마십시오 `Center`, `Start`, 또는 `End` 사용 하 여 `ProgressBar`입니다. UWP에서는 `ProgressBar` 막대의 너비가 0으로 축소 합니다. 기본값을 유지 `HorizontalOptions` 의 값 `Fill` 고의 너비를 사용 하지 않는 `Auto` 전환할 때를 `ProgressBar` 에 `Grid` 레이아웃 합니다.

## <a name="progressbar-appearance-properties"></a>ProgressBar 모양 속성

합니다 `ProgressColor` 내부 모음을 정의 하려면 속성을 설정할 수 있습니다 때 색을 `Progress` 속성이 0 보다 큰 합니다. 다음 예제에서는 인스턴스화하는 방법을 보여 줍니다는 `ProgressBar` 사용 하 여 XAML에서는 `ProgressColor` 속성 집합:

```xaml
<ProgressBar OnColor="Orange" />
```

합니다 `ProgressColor` 속성을 만들 때 설정할 수도 있습니다는 `ProgressBar` 코드에서:

```csharp
ProgressBar progressBar = new ProgressBar { ProgressColor = Color.Orange };
```

다음 스크린 샷에 표시 된 `ProgressBar` 사용 하 여는 `ProgressColor` 속성으로 설정 `Color.Orange` iOS 및 Android에서:

![스타일이 적용 된 ProgressBar ios 및 Android의 스크린 샷](progressbar-images/progressbars-styled.png "iOS 및 Android에서 ProgressBar 스타일")

## <a name="animate-a-progressbar"></a>ProgressBar에 애니메이션 효과 주기

`ProgressTo` 메서드 애니메이션을 적용 합니다 `ProgressBar` 현재에서 `Progress` 시간이 지남에 따라 제공 된 값으로 값. 메서드는 허용를 `float` 진행 되는 값을 `uint` 시간 밀리초에서는 `Easing` 열거형 값 반환를 `Task<bool>`. 다음 코드는 애니메이션을 적용 하는 방법에 설명 된 `ProgressBar`:

```csharp
// animate to 75% progress over 500 milliseconds with linear easing
await progressBar.ProgressTo(0.75, 500, Easing.Linear);
```

에 대 한 자세한 내용은 `Easing` 열거형을 참조 하세요 [xamarin.forms에서 감속/가속 함수](~/xamarin-forms/user-interface/animation/easing.md)합니다.

## <a name="related-links"></a>관련 링크

* [ProgressBar 데모](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ProgressBarDemos)
