---
title: 여백 및 안쪽 여백
description: 여백 및 안쪽 여백 속성 사용자 인터페이스에서 요소를 렌더링할 때 레이아웃 동작을 제어 합니다. 이 문서에서는 두 개의 속성 및 속성을 설정 하는 방법의 차이 보여줍니다.
ms.prod: xamarin
ms.assetid: BEB096BB-51DF-410F-B0F1-D235287B0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 545468d3b02f9651c45fcaebe159351aafea6432
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30790602"
---
# <a name="margin-and-padding"></a>여백 및 안쪽 여백

_여백 및 안쪽 여백 속성 사용자 인터페이스에서 요소를 렌더링할 때 레이아웃 동작을 제어 합니다. 이 문서에서는 두 개의 속성 및 속성을 설정 하는 방법의 차이 보여줍니다._

## <a name="overview"></a>개요

여백 및 안쪽 여백은 레이아웃 관련된 개념:

- [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) 요소와 해당 인접 한 요소 간의 거리를 나타내는 속성과 요소의 렌더링 위치 및 해당 인접 한 항목의 렌더링 위치를 제어 하는 데 사용 됩니다. `Margin` 에 값을 지정할 수 있습니다 [레이아웃](~/xamarin-forms/user-interface/controls/layouts.md) 및 [보기](~/xamarin-forms/user-interface/controls/views.md) 클래스입니다.
- [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) 요소와 해당 자식 요소 간의 거리를 나타내는 속성과에서 자체 콘텐츠 컨트롤을 분리 하는 데 사용 됩니다. `Padding` 에 값을 지정할 수 있습니다 [레이아웃](~/xamarin-forms/user-interface/controls/layouts.md) 클래스입니다.

다음 다이어그램에서는 두 가지 개념을 보여 줍니다.

[![](margin-and-padding-images/margins-and-padding-sml.png "여백 및 안쪽 여백 개념")](margin-and-padding-images/margins-and-padding.png#lightbox "여백 및 안쪽 여백 개념")

[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) 값은 가산적입니다. 따라서 20 픽셀의 여백을 지정 하는 인접 요소 두 개 요소 간의 거리 40 픽셀 됩니다. 여백 및 안쪽 여백도 모두 적용 되 면 요소 및 콘텐츠 사이의 거리는 여백 및 안쪽 여백 됩니다 한다는 점에서 가산 성입니다.

## <a name="specifying-a-thickness"></a>두께 지정합니다.

[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) 및 [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) 속성은 모두 형식의 [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/)합니다. 세 가지 기능이 있습니다를 만들 때 한 `Thickness` 구조:

- 만들기는 [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) 균일 한 단일 값으로 정의 된 구조체입니다. 단일 값은 왼쪽, 위쪽, 오른쪽 및 아래쪽 요소에 적용 됩니다.
- 만들기는 [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) 가로 및 세로 값에 의해 정의 된 구조체입니다. 가로 값 대칭적으로 대칭적 요소의 위쪽과 아래쪽에 적용 되 고 세로 값과 해당 요소의 왼쪽과 오른쪽에 적용 됩니다.
- 만들기는 [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) 왼쪽, 위쪽, 오른쪽 및 아래쪽 요소에 적용 되는 네 가지 값으로 정의 된 구조체입니다.

다음 XAML 코드 예제에서는 모든 세 가지를 보여 줍니다.

```xaml
<StackLayout Padding="0,20,0,0">
  <Label Text="Xamarin.Forms" Margin="20" />
  <Label Text="Xamarin.iOS" Margin="10, 15" />
  <Label Text="Xamarin.Android" Margin="0, 20, 15, 5" />
</StackLayout>
```

해당하는 C# 코드가 다음 코드 예제에 표시됩니다.

```csharp
var stackLayout = new StackLayout {
  Padding = new Thickness(0,20,0,0),
  Children = {
    new Label { Text = "Xamarin.Forms", Margin = new Thickness (20) },
    new Label { Text = "Xamarin.iOS", Margin = new Thickness (10, 25) },
    new Label { Text = "Xamarin.Android", Margin = new Thickness (0, 20, 15, 5) }
  }
};
```

> [!NOTE]
> `Thickness` 일반적으로 자르거나 콘텐츠 overdraws 값 음수가 될 수 있습니다.

## <a name="summary"></a>요약

이 문서에 명시 간의 차이 [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) 및 [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) 속성 및 속성을 설정 하는 방법입니다. 속성은 요소는 사용자 인터페이스에서 렌더링 될 때 레이아웃 동작을 제어 합니다.


## <a name="related-links"></a>관련 링크

- [여백](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/)
- [안쪽 여백](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)
- [두께](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/)
