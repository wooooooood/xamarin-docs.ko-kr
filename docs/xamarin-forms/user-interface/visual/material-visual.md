---
title: Xamarin.Forms 자료 Visual
description: Xamarin.Forms 자료 Visual iOS 및 Android에서 동일 하거나 거의 동일 보이는 Xamarin.Forms 응용 프로그램을 만드는 데 사용할 수 있습니다.
ms.prod: xamarin
ms.assetid: B774F68C-EF9E-49E1-B738-CDC64879ADA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2019
ms.openlocfilehash: 46ffe29f898e2f7bd8fcbe759f5a2f54e3dd60e1
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557505"
---
# <a name="xamarinforms-material-visual"></a>Xamarin.Forms 자료 Visual

Xamarin.Forms 자료 Visual iOS 및 Android에서 동일 하거나 거의 동일 보이는 Xamarin.Forms 응용 프로그램을 만드는 데 사용할 수 있습니다. 응용 프로그램을 통해 모양을 옵트인 재료는 형태를 구현 하는 추가 렌더러를 사용 하 여 이렇게는 `Visual` 속성:

```xaml
<ContentPage ...
             Visual="Material">
    ...
</ContentPage>
```

해당 하는 C# 코드가입니다.

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = VisualMarker.Material;
```

> [!IMPORTANT]
> `Visual` 속성에 정의 된 합니다 `VisualElement` 클래스를 상속 하는 뷰를 사용 하 여는 `Visual` 부모에서 속성 값입니다. 따라서 설정 합니다 `Visual` 속성을는 `ContentPage` 페이지의 지원 되는 모든 보기에서 해당 모양을 사용 되도록 보장 합니다. 또한는 `Visual` 뷰에 속성을 재정의할 수 있습니다.

재질 렌더러 iOS 및 Android에서 다음과 같은 뷰에 대 한 현재 포함 되어 있습니다.

- [`Button`](xref:Xamarin.Forms.Button)
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

IOS 및 Android 플랫폼 프로젝트가 있어야 합니다 [Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/ https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet 패키지를 설치 합니다. NuGet 패키지를 설치한 후 초기화 코드는 각 플랫폼 프로젝트에서 필요한 *한 후* 는 `Xamarin.Forms.Forms.Init` 메서드 호출 합니다. Ios의 경우 다음 코드를 사용 합니다.

```csharp
global::Xamarin.Forms.Forms.Init();
FormsMaterial.Init();
```

Android에서 전달 되는 동일한 매개 변수를 전달 해야 `Forms.Init`:

```csharp
global::Xamarin.Forms.Forms.Init(this, bundle);
FormsMaterial.Init(this, bundle);
```

> [!IMPORTANT]
> Android에서 자료만 TargetFramework 9.0 (API 28)를 사용 하 여 작동합니다. 또한 플랫폼 프로젝트 v28 지원 라이브러리를 사용 해야 하며 해당 테마 자료 구성 요소 테마에서 상속 하거나 계속 AppCompat 테마를에서 상속 합니다. 자세한 내용은 [자료 구성 요소를 사용 하 여 Android에 대 한 시작](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md)합니다.

다음 스크린샷에서 자료에 대 한 렌더러 존재 하지만 기본 렌더러를 사용 하 여 렌더링 네 가지 보기를 포함 하는 사용자 인터페이스를 보여 줍니다.

[![IOS 및 Android에서 기본 렌더러의 스크린 샷](material-visual-images/default-renderers.png "기본 렌더러를 사용 하 여 뷰")](material-visual-images/default-renderers-large.png#lightbox)

다음 스크린샷은 재료 렌더러를 사용 하 여 렌더링 동일한 사용자 인터페이스를 보여 줍니다.

[![IOS 및 Android에서 material 렌더러의 스크린 샷](material-visual-images/material-renderers.png "재료 렌더러를 사용 하 여 뷰")](material-visual-images/material-renderers-large.png#lightbox)

> [!NOTE]
> 재료 렌더러를 사용 하 여 렌더링 된 컨트롤은 네이티브 컨트롤을 유지 하 고 따라서 수 있으며 사용자 인터페이스 차이점 글꼴, 그림자, 색 및 권한 상승 같은 영역에 대 한 플랫폼.

## <a name="material-renderers"></a>재질 렌더러

재질 렌더러 사용자 지정할 수 있습니다, 기본 렌더러의 경우와 마찬가지로 다음 기본 클래스를 사용 하 여:

- `MaterialButtonRenderer`
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

## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
