---
title: Xamarin.Forms 자료 Visual
description: Xamarin.Forms 자료 Visual iOS 및 Android에서 동일 하거나 거의 동일 보이는 Xamarin.Forms 응용 프로그램을 만드는 데 사용할 수 있습니다.
ms.prod: xamarin
ms.assetid: B774F68C-EF9E-49E1-B738-CDC64879ADA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
ms.openlocfilehash: a626532ac507185b6c01abb5327efa7015c787f5
ms.sourcegitcommit: 0fd04ea3af7d6a6d6086525306523a5296eec0df
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67512903"
---
# <a name="xamarinforms-material-visual"></a>Xamarin.Forms 자료 Visual

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VisualDemos/)

[재질 디자인](https://material.io) 는 크기, 색, 간격 및 보기 및 레이아웃 해야 모양과 동작의 다른 측면은 규정 하는 Google에서 자기 주장이 있는 디자인 시스템입니다.

Xamarin.Forms 자료 Visual 사용할 수 있습니다 Xamarin.Forms 응용 프로그램에 재질 디자인 규칙을 적용할 iOS 및 Android에서 동일 하거나 거의 동일 보이는 응용 프로그램을 만들기. 자료 Visual을 사용 하면 지원 되는 뷰는 동일한 디자인 플랫폼 간 통합된 된 모양과 느낌 만들기를 채택 합니다. 재질 디자인 규칙을 적용 하는 자재 렌더러를 사용 하 여 수행 됩니다.

응용 프로그램에서 Xamarin.Forms 자료 Visual을 사용 하기 위한 프로세스 다음과 같습니다.

1. 추가 된 [Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) iOS 및 Android 플랫폼 프로젝트에 NuGet 패키지. 이 NuGet 패키지는 iOS 및 Android에서 최적화 된 재료 디자인 렌더러를 제공합니다. Ios의 경우 패키지는 전이적 종속성을 제공 [Xamarin.iOS.MaterialComponents](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents), 되는 C# Google의 바인딩할 [iOS에 대 한 자료 구성 요소](https://material.io/develop/ios/)합니다. Android 패키지에 TargetFramework 올바르게 설정 되었는지 확인 하려면 빌드 대상을 제공 합니다.
1. 각 플랫폼 프로젝트에서 material 렌더러를 초기화 합니다. 자세한 내용은 [재질 렌더러 초기화](#initialize-material-renderers)합니다.
1. 설정 하 여 재질 렌더러를 사용 합니다 [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) 속성을 `Material` 재료 디자인 규칙을 채택 해야 하는 모든 페이지에서. 자세한 내용은 [재질 렌더러 사용](#consume-material-renderers)합니다.
1. [선택 사항] 재질 렌더러를 사용자 지정 합니다. 자세한 내용은 [재질 렌더러를 사용자 지정](#customize-material-renderers)합니다.

> [!IMPORTANT]
> Android 재료 렌더러 (API 21) 5.0 이상이 필요 하거나, 이상과 TargetFramework 버전 9.0의 (API 28). 또한 플랫폼 프로젝트에 필요한 Android 지원 라이브러리 28.0.0 이상 해당 테마 자료 구성 요소 테마에서 상속 하거나 계속 AppCompat 테마를에서 상속 해야 합니다. 자세한 내용은 [자료 구성 요소를 사용 하 여 Android에 대 한 시작](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md)합니다.

재료 렌더러는 현재에 포함 된 [Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) 다음 뷰에 대 한 NuGet 패키지:

- [`Button`](xref:Xamarin.Forms.Button)
- `CheckBox`
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`Frame`](xref:Xamarin.Forms.Frame)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)
- [`DatePicker`](xref:Xamarin.Forms.DatePicker)
- [`TimePicker`](xref:Xamarin.Forms.TimePicker)
- [`Picker`](xref:Xamarin.Forms.Picker)
- [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)
- [`Editor`](xref:Xamarin.Forms.Editor)
- [`Slider`](xref:Xamarin.Forms.Slider)
- [`Stepper`](xref:Xamarin.Forms.Stepper)

기능적으로 재료 렌더러는 기본 렌더러에 차이가 없습니다.

## <a name="initialize-material-renderers"></a>재질 렌더러 초기화

설치한 후 합니다 [Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet 패키지를 자료 렌더러는 각 플랫폼 프로젝트에서 초기화 되어야 합니다.

Ios의 경우에 발생 해야 합니다 **AppDelegate.cs** 를 호출 하 여 합니다 `FormsMaterial.Init` 메서드 *후* 는 `Xamarin.Forms.Forms.Init` 메서드:

```csharp
global::Xamarin.Forms.Forms.Init();
FormsMaterial.Init();
```

Android에서에서 발생 해야 합니다 **MainActivity.cs** 를 호출 하 여 합니다 `FormsMaterial.Init` 메서드 *후* 는 `Xamarin.Forms.Forms.Init` 메서드:

```csharp
global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
FormsMaterial.Init(this, savedInstanceState);
```

## <a name="consume-material-renderers"></a>재질 렌더러를 사용 합니다.

응용 프로그램을 설정 하 여 재질 렌더러를 사용 하 여 옵트인 할 수는 [ `VisualElement.Visual` ](xref:Xamarin.Forms.VisualElement.Visual) 속성 페이지, 레이아웃 또는 보기를 `Material`:

```xaml
<ContentPage Visual="Material"
             ...>
    ...
</ContentPage>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = VisualMarker.Material;
```

합니다 [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) 구현 하는 형식에 속성을 설정할 수 있습니다 `IVisual`를 사용 하 여 합니다 [ `VisualMarker` ](xref:Xamarin.Forms.VisualMarker) 다음을 제공 하는 클래스 `IVisual` 속성:

- `Default` – 기본 렌더러를 사용 하 여 뷰를 렌더링 해야 함을 나타냅니다.
- `MatchParent` – 뷰를 직접 부모와 같은 렌더러를 사용 해야 함을 나타냅니다.
- `Material` – 재료 렌더러를 사용 하 여 뷰를 렌더링 해야 함을 나타냅니다.

> [!IMPORTANT]
> [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) 속성에 정의 된 합니다 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) 클래스를 상속 하는 뷰를 사용 하 여는 `Visual` 부모에서 속성 값입니다. 따라서 설정 합니다 `Visual` 속성을 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 페이지의 지원 되는 모든 보기에서 해당 시각적 개체를 사용 하면 합니다. 또한는 `Visual` 뷰에 속성을 재정의할 수 있습니다.

다음 스크린샷은 기본 렌더러를 사용 하 여 렌더링 하는 사용자 인터페이스를 보여 줍니다.

[![IOS 및 Android에서 기본 렌더러의 스크린 샷](material-visual-images/default-renderers.png "기본 렌더러를 사용 하 여 뷰")](material-visual-images/default-renderers-large.png#lightbox)

다음 스크린샷은 재료 렌더러를 사용 하 여 렌더링 동일한 사용자 인터페이스를 보여 줍니다.

[![IOS 및 Android에서 material 렌더러의 스크린 샷](material-visual-images/material-renderers.png "재료 렌더러를 사용 하 여 뷰")](material-visual-images/material-renderers-large.png#lightbox)

기본 렌더러와 자재 렌더러의 경우 여기에 표시 된 주요 표시 차이점은 대문자로 표시 된 material 렌더러 [ `Button` ](xref:Xamarin.Forms.Button) 텍스트의 모퉁이 둥글게 하 고 [ `Frame` ](xref:Xamarin.Forms.Frame)테두리입니다. 그러나 재료 렌더러에 네이티브 컨트롤을 사용 하 고 따라서 여전히 있을 글꼴, 그림자, 색 및 권한 상승 같은 영역에 대 한 플랫폼의 사용자 인터페이스 차이점.

> [!NOTE]
> 재질 디자인 구성 요소는 Google의 지침을 밀접 하 게 준수합니다. 결과적으로, 재질 디자인 렌더러는 크기 조정 및 동작에 대해 맞추어져 있습니다. 스타일 또는 동작의 제어를 요구 하면 계속 만들면 고유한 [효과](~/xamarin-forms/app-fundamentals/effects/index.md)를 [동작](~/xamarin-forms/app-fundamentals/behaviors/index.md), 또는 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) 필요한 세부 정보를 달성 하기 위해.

## <a name="customize-material-renderers"></a>재질 렌더러를 사용자 지정

재질 렌더러 필요에 따라 사용자 지정할 수 있습니다, 기본 렌더러의 경우 다음 기본 클래스를 통해 마찬가지로:

- `MaterialButtonRenderer`
- `MaterialCheckBoxRenderer`
- `MaterialEntryRenderer`
- `MaterialFrameRenderer`
- `MaterialProgressBarRenderer`
- `MaterialDatePickerRenderer`
- `MaterialTimePickerRenderer`
- `MaterialPickerRenderer`
- `MaterialActivityIndicatorRenderer`
- `MaterialEditorRenderer`
- `MaterialSliderRenderer`
- `MaterialStepperRenderer`

다음 코드를 사용자 지정의 예를 보여 줍니다.는 `MaterialProgressBarRenderer` 클래스:

```csharp
using Xamarin.Forms.Material.Android;

[assembly: ExportRenderer(typeof(ProgressBar), typeof(CustomMaterialProgressBarRenderer), new[] { typeof(VisualMarker.MaterialVisual) })]
namespace MyApp.Android
{
    public class CustomMaterialProgressBarRenderer : MaterialProgressBarRenderer
    {
        ...
    }
}
```

이 예제에서는 `ExportRendererAttribute` 지정를 `CustomMaterialProgressBarRenderer` 클래스에서 렌더링 하는 데 사용 됩니다는 [ `ProgressBar` ](xref:Xamarin.Forms.ProgressBar) 뷰를 사용 하 여는 `IVisual` 세 번째 인수로 형식이 등록 합니다.

> [!NOTE]
> 렌더러를 지정 하는 `IVisual` 의 일부로 형식 해당 `ExportRendererAttribute`, 하는 데 사용할 렌더링 기본 렌더러 대신 보기에 옵트인 합니다. 렌더러 선택 시의 `Visual` 보기의 속성을 검사 하 고 렌더러 선택 프로세스에 포함 합니다.

사용자 지정 렌더러에 대 한 자세한 내용은 참조 하세요. [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)합니다.

## <a name="related-links"></a>관련 링크

- [재질 Visual (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VisualDemos/)
- [Xamarin.Forms 시각적 렌더러 만들기](create.md)
- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
