---
title: Xamarin.Forms 시각적 렌더러 만들기
description: Xamarin.Forms Visual 렌더러를 서브 클래스 Xamarin.Forms 컨트롤 않고도 VisualElement 개체에 선택적으로 적용할 수 있습니다.
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2019
ms.openlocfilehash: 3c614bcd0044a0aa4a698c830d2f2af5aba6c65a
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557515"
---
# <a name="xamarinforms-visual"></a>Xamarin.Forms Visual

Xamarin.Forms Visual 렌더러를 선택적으로 적용할 수 있도록 `VisualElement` 서브 클래스 Xamarin.Forms 컨트롤 하지 않고도 개체입니다. 지정 하는 모든 렌더러를 `VisualType` 의 일부로 `ExportRendererAttribute` 기본 렌더러 대신 렌더러 뷰 하는 데 사용할 합니다. 렌더러 선택 시의 `Visual` 보기의 속성을 검사 하 고 렌더러 선택 프로세스에 포함 합니다.

> [!IMPORTANT]
> 현재는 `Visual` 컨트롤이 렌더링 된,이 이후 릴리스에서 변경 후에 변경할 수 없습니다.

Xamarin.Forms는 재질 디자인을 기반으로 시각적 모양을 포함 되어 있습니다. 자세한 내용은 [Xamarin.Forms 자료 Visual](material-visual.md)합니다.

## <a name="create-a-visual"></a>시각적 개체 만들기

플랫폼 간 라이브러리에서 파생 되는 형식을 만듭니다 `IVisual`:

```csharp
public class CustomVisual : IVisual
{
}
```

## <a name="register-the-visual-type"></a>시각적 개체 유형을 등록 합니다.

필요한 보기에 대 한 렌더러를 만들고 시각적 개체 유형을 플랫폼의 일부로 등록 `ExportRenderAttribute`:

```csharp
using Xamarin.Forms.Material.Android;

[assembly: ExportRenderer(typeof(ContentPage), typeof(CustomPageRenderer), new[] { typeof(CustomVisual) })]
namespace MyApp.Android
{
    public class CustomPageRenderer : PageRenderer
    {
        ...
    }
}
```

## <a name="consume-the-visual"></a>시각적 개체를 사용 합니다.

응용 프로그램을 통해 사용자 지정 시각적 개체를 사용 하 여 옵트인 할 수는 `Visual` 속성:

```xaml
<ContentPage ...
             Visual="Custom">
    ...
</ContentPage>
```

해당 하는 C# 코드가입니다.

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = new CustomVisual();
```

## <a name="register-a-name-for-a-visual"></a>시각적 개체에 대 한 이름 등록

아마도 하려는 시각적 개체를 다른 이름으로 참조 또는 다른 시각적 라이브러리 간에 하는 충돌이 있을 수 있습니다. 이 시나리오에서는 제공된 된 시각적 개체에 다른 이름을 등록할 어셈블리 특성을 사용할 수 있습니다.

```csharp
[assembly: Visual("MyMaterialName", typeof(VisualMarker.MaterialVisual))]
```

그러면 사용자 지정 시각적 개체는 등록 된 이름을 통해 사용할 수 있습니다.

```xaml
<ContentPage ...
             Visual="MyMaterialName">
    ...
</ContentPage>
````

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms 자료 Visual](material-visual.md)
- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
