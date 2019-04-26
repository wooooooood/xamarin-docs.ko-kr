---
title: Xamarin.Forms 시각적 렌더러 만들기
description: 서브 클래스 Xamarin.Forms 뷰 않고도 VisualElement 개체에 선택적으로 적용할 Xamarin.Forms 시각적 개체를 만듭니다.
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
ms.openlocfilehash: a11c2045fa6119d0689834c35794bc8913c80bd6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61023781"
---
# <a name="create-a-xamarinforms-visual-renderer"></a>Xamarin.Forms 시각적 렌더러 만들기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VisualDemos/)

Xamarin.Forms Visual 렌더러를 만들고 선택적으로 적용할 수 있습니다 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) 서브 클래스 Xamarin.Forms 뷰 하지 않고도 개체입니다. 렌더러를 지정 하는 `IVisual` 의 일부로 형식 해당 `ExportRendererAttribute`, 하는 데 사용할 렌더링 기본 렌더러 대신 보기에 옵트인 합니다. 렌더러 선택 시의 `Visual` 보기의 속성을 검사 하 고 렌더러 선택 프로세스에 포함 합니다.

> [!IMPORTANT]
> 현재는 [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) 뷰를 렌더링할 하지만이 이후 릴리스에서 변경 후 속성을 변경할 수 없습니다.

만들고 Xamarin.Forms 시각적 렌더러를 사용 하기 위한 프로세스 다음과 같습니다.

1. 필요한 보기에 대 한 플랫폼 렌더러를 만듭니다. 자세한 내용은 [렌더러를 만들](#create-platform-renderers)합니다.
1. 파생 되는 형식을 만드는 `IVisual`합니다. 자세한 내용은 [IVisual 형식을 만들려면](#create-an-ivisual-type)합니다.
1. 등록 된 `IVisual` 의 일부로 형식은 `ExportRendererAttribute` 된 렌더러를 데코 레이트 하는 합니다. 자세한 내용은 [IVisual 형식을 등록](#register-the-ivisual-type)합니다.
1. 설정 하 여 시각적 렌더러를 사용 합니다 [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) 뷰의 속성을 `IVisual` 이름. 자세한 내용은 [시각적 렌더러 사용](#consume-the-visual-renderer)합니다.
1. [선택 사항] 이름에 대 한 등록을 `IVisual` 형식입니다. 자세한 내용은 [IVisual 형식의 이름을 등록](#register-a-name-for-the-ivisual-type)합니다.

## <a name="create-platform-renderers"></a>플랫폼 렌더러 만들기

렌더러 클래스를 만드는 방법에 대 한 자세한 내용은 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)합니다. 그러나 뷰를 하위 클래스로 필요 없이 Xamarin.Forms 시각적 렌더러를 보기에 적용 되는 note 합니다.

여기에 설명 된 렌더러 클래스 사용자 지정 구현 [ `Button` ](xref:Xamarin.Forms.Button) 그림자가 적용 된 텍스트를 표시 하는 합니다.

### <a name="ios"></a>iOS

다음 코드 예제에서는 iOS에 대 한 단추 렌더러를 보여 줍니다.

```csharp
public class CustomButtonRenderer : ButtonRenderer
{
    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        base.OnElementChanged(e);

        if (e.OldElement != null)
        {
            // Cleanup
        }

        if (e.NewElement != null)
        {
            Control.TitleShadowOffset = new CoreGraphics.CGSize(1, 1);
            Control.SetTitleShadowColor(Color.Black.ToUIColor(), UIKit.UIControlState.Normal);
        }
    }
}
```

### <a name="android"></a>Android

다음 코드 예제에서는 Android에 대 한 단추 렌더러를 보여 줍니다.

```csharp
public class CustomButtonRenderer : Xamarin.Forms.Platform.Android.AppCompat.ButtonRenderer
{
    public CustomButtonRenderer(Context context) : base(context)
    {
    }

    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        base.OnElementChanged(e);

        if (e.OldElement != null)
        {
            // Cleanup
        }

        if (e.NewElement != null)
        {
            Control.SetShadowLayer(5, 3, 3, Color.Black.ToAndroid());
        }
    }
}
```

## <a name="create-an-ivisual-type"></a>IVisual 형식 만들기

플랫폼 간 라이브러리에서 파생 되는 형식을 만듭니다 `IVisual`:

```csharp
public class CustomVisual : IVisual
{
}
```

합니다 `CustomVisual` 형식을 다음 등록할 수 있습니다 렌더러 클래스에 대해 허용 [ `Button` ](xref:Xamarin.Forms.Button) 렌더러를 사용 하는 옵트인 하는 개체입니다.

## <a name="register-the-ivisual-type"></a>IVisual 형식 등록

플랫폼 프로젝트를 사용 하 여 렌더러 클래스를 데코 레이트 된 `ExportRendererAttribute`:

```csharp
[assembly: ExportRenderer(typeof(Xamarin.Forms.Button), typeof(CustomButtonRenderer), new[] { typeof(CustomVisual) })]
public class CustomButtonRenderer : ButtonRenderer
{
    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        ...
    }
}
```

이 예제에서는 `ExportRendererAttribute` 지정 합니다 `CustomButtonRenderer` 클래스를 사용 하 여 소비를 렌더링 하는 [ `Button` ](xref:Xamarin.Forms.Button) 개체를 사용 하 여는 `IVisual` 세 번째 인수로 형식이 등록. 렌더러를 지정 하는 `IVisual` 의 일부로 형식 해당 `ExportRendererAttribute`, 하는 데 사용할 렌더링 기본 렌더러 대신 보기에 옵트인 합니다.

## <a name="consume-the-visual-renderer"></a>시각적 렌더러를 사용 합니다.

A [ `Button` ](xref:Xamarin.Forms.Button) 개체를 설정 하 여 렌더러 클래스를 사용 하 여 옵트인 할 수 해당 [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) 속성을 `Custom`:

```xaml
<Button Visual="Custom"
        Text="CUSTOM BUTTON"
        BackgroundColor="{StaticResource PrimaryColor}"
        TextColor="{StaticResource SecondaryTextColor}"
        HorizontalOptions="FillAndExpand" />
```

> [!NOTE]
> XAML에서 형식 변환기를 제거에서 "Visual" 접미사를 포함 해야 합니다 [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) 속성 값입니다. 그러나 전체 형식 이름을 지정할 수도 있습니다.

해당 하는 C# 코드가입니다.

```csharp
Button button = new Button { Text = "CUSTOM BUTTON", ... };
button.Visual = new CustomVisual();
```

렌더러 선택 시 합니다 [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) 의 속성을 [ `Button` ](xref:Xamarin.Forms.Button) 를 검사 하 고 렌더러 선택 프로세스에 포함 합니다. 렌더러를 없으면 Xamarin.Forms 기본 렌더러 사용 됩니다.

다음 스크린샷에서 표시 된 렌더링 [ `Button` ](xref:Xamarin.Forms.Button)에 그림자가 적용 된 텍스트를 표시 하는:

[![IOS 및 Android에서 섀도 텍스트를 사용 하 여 사용자 지정 단추의 스크린샷](material-visual-images/custom-button.png "섀도 텍스트가 있는 단추")](material-visual-images/custom-button-large.png#lightbox)

## <a name="register-a-name-for-the-ivisual-type"></a>IVisual 형식에 대 한 이름 등록

합니다 [ `VisualAttribute` ](xref:Xamarin.Forms.VisualAttribute) 필요에 따라 다른 이름으로 등록에 사용할 수는 `IVisual` 형식입니다. 이 방법을 사용 하 여 해당 형식 이름이 아닌 다른 이름을 여기서 하려는 시각적 개체를 참조 하는 경우 또는 다른 시각적 라이브러리 간에 이름 충돌을 해결 사용할 수 있습니다.

합니다 [ `VisualAttribute` ](xref:Xamarin.Forms.VisualAttribute) 플랫폼 프로젝트 또는 플랫폼 간 라이브러리에서 어셈블리 수준에서 정의 되어야 합니다.

```csharp
[assembly: Visual("MyVisual", typeof(CustomVisual))]
```

`IVisual` 형식 등록된 된 이름을 통해 사용할 수 있습니다.

```xaml
<Button Visual="MyVisual"
        ... />
```

> [!NOTE]
> 등록된 된 이름을 통해 시각적 개체를 사용 하는 경우 "Visual" 접미사가 포함 되어야 합니다.

## <a name="related-links"></a>관련 링크

- [재질 Visual (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VisualDemos/)
- [Xamarin.Forms 자료 Visual](material-visual.md)
- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
