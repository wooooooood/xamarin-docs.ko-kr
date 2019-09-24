---
title: Xamarin.ios 재질 시각적 개체
description: Xamarin.ios 재질 시각적 개체는 iOS 및 Android에서 동일 하거나 거의 동일한 Xamarin Forms 응용 프로그램을 만드는 데 사용할 수 있습니다.
ms.prod: xamarin
ms.assetid: B774F68C-EF9E-49E1-B738-CDC64879ADA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
ms.openlocfilehash: b735541d51321231775b025745e68c54552697d3
ms.sourcegitcommit: 76f930ce63b193ca3f7f85f768b031e59cb342ec
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198488"
---
# <a name="xamarinforms-material-visual"></a>Xamarin.ios 재질 시각적 개체

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)

[재질 설계](https://material.io) 는 Google에서 만든 독단적인 디자인 시스템이 며,이 시스템은 보기와 레이아웃의 모양과 동작에 대 한 크기, 색, 간격 및 기타 측면을 규정 합니다.

Xamarin.ios 재질 시각적 개체를 사용 하 여 iOS 및 Android에서 동일 하거나 거의 동일한 응용 프로그램을 만들거나 Xamarin. Forms 응용 프로그램에 재질 디자인 규칙을 적용할 수 있습니다. 재질 시각적 개체를 사용 하는 경우 지원 되는 보기는 동일한 디자인 플랫폼을 채택 하 여 통일 된 모양과 느낌을 만듭니다. 이는 재질 디자인 규칙을 적용 하는 재질 렌더러를 사용 하 여 달성 됩니다.

응용 프로그램에서 Xamarin.ios 재질 시각적 개체를 사용 하도록 설정 하는 프로세스는 다음과 같습니다.

1. IOS 및 Android 플랫폼 프로젝트에 [Xamarin. 재질](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet 패키지를 추가 합니다. 이 NuGet 패키지는 iOS 및 Android에서 최적화 된 재질 디자인 렌더러를 제공 합니다. IOS에서 패키지는 [ios 용 Google의 재질 구성 요소에 대](https://material.io/develop/ios/)한 [MaterialComponents](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents) C# 에 대 한 전이적 종속성을 제공 합니다. Android에서 패키지는 TargetFramework가 올바르게 설정 되도록 빌드 대상을 제공 합니다.
1. 각 플랫폼 프로젝트에서 재질 렌더러를 초기화 합니다. 자세한 내용은 [재질 렌더러 초기화](#initialize-material-renderers)를 참조 하세요.
1. 재질 디자인 규칙을 채택 해야 하 [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) 는 모든 `Material` 페이지에서 속성을로 설정 하 여 재질 렌더러를 사용 합니다. 자세한 내용은 [재질 렌더러 사용](#consume-material-renderers)을 참조 하세요.
1. 필드 재질 렌더러를 사용자 지정 합니다. 자세한 내용은 [재질 렌더러 사용자 지정](#customize-material-renderers)을 참조 하세요.

> [!IMPORTANT]
> Android에서 재질 렌더러에는 최소 버전 5.0 (API 21) 이상이 필요 하며, 버전 9.0 (API 28)의 TargetFramework가 필요 합니다. 또한 플랫폼 프로젝트에는 Android 지원 라이브러리 28.0.0 이상이 필요 하며, 해당 테마는 재질 구성 요소 테마에서 상속 하거나 AppCompat 테마에서 계속 상속 해야 합니다. 자세한 내용은 [자료 구성 요소를 사용 하 여 Android에 대 한 시작](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md)합니다.

재질 렌더러는 현재 다음 뷰에 대 한 [Xamarin. material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet 패키지에 포함 되어 있습니다.

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

[Xamarin.ios](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet 패키지를 설치한 후에는 각 플랫폼 프로젝트에서 재질 렌더러를 초기화 해야 합니다.

IOS에서 메서드를 호출한 *후* `Xamarin.Forms.Forms.Init` 메서드 를 `Xamarin.Forms.FormsMaterial.Init` 호출 하 여 AppDelegate.cs에서 발생 해야 합니다.

```csharp
global::Xamarin.Forms.Forms.Init();
global::Xamarin.Forms.FormsMaterial.Init();
```

Android에서 메서드를 호출한 *후* `Xamarin.Forms.Forms.Init` 메서드 를 `Xamarin.Forms.FormsMaterial.Init` 호출 하 여 MainActivity.cs에서 발생 해야 합니다.

```csharp
global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
global::Xamarin.Forms.FormsMaterial.Init(this, savedInstanceState);
```

## <a name="consume-material-renderers"></a>재질 렌더러 사용

응용 프로그램은 페이지, 레이아웃 또는 뷰의 [`VisualElement.Visual`](xref:Xamarin.Forms.VisualElement.Visual) 속성을 다음과 같이 설정 하 여 재질 렌더러를 사용 하도록 `Material`선택할 수 있습니다.

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

속성 [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) 은 [`VisualMarker`](xref:Xamarin.Forms.VisualMarker) 다음 `IVisual` 속성`IVisual` 을 제공 하는 클래스를 사용 하 여을 구현 하는 모든 형식으로 설정할 수 있습니다.

- `Default`– 뷰가 기본 렌더러를 사용 하 여 렌더링 되어야 함을 나타냅니다.
- `MatchParent`– 뷰에서 직접 부모와 동일한 렌더러를 사용 해야 함을 나타냅니다.
- `Material`– 뷰가 재질 렌더러를 사용 하 여 렌더링 되어야 함을 나타냅니다.

> [!IMPORTANT]
> 속성은 부모에서 속성 값 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 을 `Visual` 상속 하는 뷰를 사용 하 여 클래스에서 정의 됩니다. [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) `Visual` [따라서`ContentPage`](xref:Xamarin.Forms.ContentPage) 의 속성을 설정 하면 페이지에서 지원 되는 모든 뷰에서 해당 시각적 개체를 사용할 수 있습니다. 또한는 `Visual` 뷰에 속성을 재정의할 수 있습니다.

다음 스크린샷에는 기본 렌더러를 사용 하 여 렌더링 된 사용자 인터페이스가 나와 있습니다.

[![IOS 및 Android의 기본 렌더러 스크린샷](material-visual-images/default-renderers.png "기본 렌더러를 사용 하는 뷰")](material-visual-images/default-renderers-large.png#lightbox)

다음 스크린샷은 재료 렌더러를 사용 하 여 렌더링 동일한 사용자 인터페이스를 보여 줍니다.

[![IOS 및 Android의 재질 렌더러 스크린샷](material-visual-images/material-renderers.png "재질 렌더러를 사용 하는 뷰")](material-visual-images/material-renderers-large.png#lightbox)

여기에 표시 된 기본 렌더러와 재질 렌더러의 주요 차이점은 재질 렌더러에서 텍스트를 대문자로 [`Button`](xref:Xamarin.Forms.Button) 표시 하 고 테두리의 [`Frame`](xref:Xamarin.Forms.Frame) 모퉁이를 둥글게 한다는 것입니다. 그러나 재질 렌더러에서는 네이티브 컨트롤을 사용 하므로 글꼴, 그림자, 색, 권한 상승 등의 영역에 대 한 플랫폼 간에 사용자 인터페이스 차이가 있을 수 있습니다.

> [!NOTE]
> 재질 디자인 구성 요소는 Google의 지침을 준수 합니다. 따라서 재질 디자인 렌더러는 크기 조정 및 동작에 따라 결정 됩니다. 스타일이 나 동작을 더 세밀 하 게 제어 해야 하는 경우에도 사용자가 원하는 [효과](~/xamarin-forms/app-fundamentals/effects/index.md), [동작](~/xamarin-forms/app-fundamentals/behaviors/index.md)또는 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) 를 만들어 필요한 세부 정보를 달성할 수 있습니다.

## <a name="customize-material-renderers"></a>재질 렌더러 사용자 지정

다음 기본 클래스를 통해 기본 렌더러와 마찬가지로 재질 렌더러를 선택적으로 사용자 지정할 수 있습니다.

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

다음 코드에서는 클래스를 `MaterialProgressBarRenderer` 사용자 지정 하는 예제를 보여 줍니다.

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

`ExportRendererAttribute` 이 예제에서는 `CustomMaterialProgressBarRenderer` 클래스를 사용 하 여 [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) 뷰를 렌더링 하 고, `IVisual` 세 번째 인수로 등록 된 형식을 사용 하도록 지정 합니다.

> [!NOTE]
> 의 일부로 형식을`IVisual` 지정 하는 렌더러는 기본 렌더러 대신 옵트인 (opt in) 뷰를 렌더링 하는 데 사용 됩니다. `ExportRendererAttribute` 렌더러 선택 시의 `Visual` 보기의 속성을 검사 하 고 렌더러 선택 프로세스에 포함 합니다.

사용자 지정 렌더러에 대 한 자세한 내용은 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [재질 시각적 개체 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)
- [Xamarin Forms 시각적 렌더러 만들기](create.md)
- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
