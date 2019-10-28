---
title: Xamarin.Forms 상대 바인딩
description: 이 문서에서는 RelativeSource 태그 확장을 사용하여 바인딩 대상의 위치에 상대적으로 바인딩 소스를 설정함으로써 상대 바인딩을 만드는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: CC64BB1D-8303-46B1-94B6-4EF2F20317A8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/30/2019
ms.openlocfilehash: 24879b1ffcac97acdba27c32a22e43bfb6e80459
ms.sourcegitcommit: e71474f91639bb43159b22f5d534325c3270ba93
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/22/2019
ms.locfileid: "72749782"
---
# <a name="xamarinforms-relative-bindings"></a>Xamarin.Forms 상대 바인딩

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

상대 바인딩을 사용하면 바인딩 대상의 위치에 상대적으로 바인딩 소스를 설정할 수 있습니다. 상대 바인딩은 `RelativeSource` 태그 확장을 사용하여 생성되며, 바인딩 식의 `Source` 속성으로 설정됩니다.

`RelativeSource` 태그 확장은 다음 속성을 정의하는 `RelativeSourceExtension` 클래스에서 지원됩니다.

- `RelativeBindingSourceMode` 형식의 `Mode`: 바인딩 대상의 위치에 상대적으로 바인딩 소스의 위치를 설명합니다.
- `int` 형식의 `AncestorLevel`: `Mode` 속성이 `FindAncestor`인 경우 살펴보아야 할 선택적 상위 항목 수준입니다.
- `Type` 형식의 `AncestorType`: `Mode` 속성이 `FindAncestor`인 경우 살펴보아야 할 상위 항목 유형입니다.

> [!NOTE]
> XAML 파서를 사용하면 `RelativeSourceExtension` 클래스를 `RelativeSource`로 축약할 수 있습니다.

`Mode` 속성은 `RelativeBindingSourceMode` 열거형 멤버 중 하나로 설정되어야 합니다.

- `TemplatedParent`: 바운드 요소가 존재하는 템플릿이 적용되는 요소를 나타냅니다. 자세한 내용은 [템플릿화된 부모에 바인딩](#bind-to-a-templated-parent)을 참조하세요.
- `Self`: 바인딩이 설정되는 요소를 나타냅니다. 해당 요소의 속성 하나를 동일한 요소의 다른 속성에 바인딩할 수 있습니다. 자세한 내용은 [자기 자신에게 바인딩](#bind-to-self)을 참조하세요.
- `FindAncestor`: 바운드 요소의 시각적 트리에 있는 상위 항목을 나타냅니다. 이 모드는 `AncestorType` 속성으로 표현되는 상위 항목 컨트롤에 바인딩하는 데 사용해야 합니다. 자세한 내용은 [상위 항목에 바인딩](#bind-to-an-ancestor)을 참조하세요.
- `FindAncestorBindingContext`: 바운드 요소의 시각적 트리에 있는 상위 항목의 `BindingContext`를 나타냅니다. 이 모드는 `AncestorType` 속성으로 표현되는 상위 항목의 `BindingContext`에 바인딩하는 데 사용해야 합니다. 자세한 내용은 [상위 항목에 바인딩](#bind-to-an-ancestor)을 참조하세요.

`Mode` 속성은 `RelativeSourceExtension` 클래스의 콘텐츠 속성입니다. 따라서 중괄호가 사용된 XAML 태그 식의 경우 식의 `Mode=` 부분을 제거할 수 있습니다.

Xamarin.Forms 태그 확장에 대한 자세한 내용은 [XAML 태그 확장](~/xamarin-forms/xaml/markup-extensions/index.md)을 참조하세요.

## <a name="bind-to-self"></a>자기 자신에게 바인딩

`Self` 상대 바인딩 모드는 요소의 속성을 동일한 요소의 다른 속성에 바인딩하는 데 사용됩니다.

```xaml
<BoxView Color="Red"
         WidthRequest="200"
         HeightRequest="{Binding Source={RelativeSource Self}, Path=WidthRequest}"
         HorizontalOptions="Center" />
```

이 예제에서 [`BoxView`](xref:Xamarin.Forms.BoxView)는 `WidthRequest` 속성을 고정된 크기로 설정하고 `HeightRequest` 속성은 `WidthRequest` 속성에 바인딩됩니다. 따라서 두 속성은 동일하며 다음과 같이 정사각형이 그려집니다.

[![iOS 및 Android의 셀프 모드 상대 바인딩 스크린샷](relative-bindings-images/self-relative-binding.png "자체 상대 바인딩 모드")](relative-bindings-images/self-relative-binding-large.png#lightbox "자체 상대 바인딩 모드")

> [!IMPORTANT]
> 요소의 속성을 동일한 요소의 다른 속성에 바인딩할 때는 두 속성의 유형이 같아야 합니다. 또는 바인딩에 값을 변환하는 변환기를 지정할 수도 있습니다.

이 바인딩 모드는 개체의 `BindingContext`를 자기 자신의 속성에 설정하는 용도로 자주 사용됩니다. 다음 코드에서 그 예를 볼 수 있습니다.

```xaml
<ContentPage ...
             BindingContext="{Binding Source={RelativeSource Self}, Path=DefaultViewModel}">
    <StackLayout>
        <ListView ItemsSource="{Binding Employees}">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

이 예제에서, 페이지의 `BindingContext`가 자기 자신의 `DefaultViewModel` 속성으로 설정되었습니다. 이 속성은 페이지의 코드 숨김 파일에 정의되어 있으며, viewmodel 인스턴스를 제공합니다. [`ListView`](xref:Xamarin.Forms.ListView)는 viewmodel의 `Employees` 속성에 바인딩됩니다.

## <a name="bind-to-an-ancestor"></a>상위 항목에 바인딩

`FindAncestor` 및 `FindAncestorBindingContext` 상대 바인딩 모드는 시각적 트리에서 특정 유형의 부모 요소에 바인딩하는 데 사용됩니다. `FindAncestor` 모드는 [`Element`](xref:Xamarin.Forms.Element) 형식에서 파생되는 부모 요소에 바인딩하는 데 사용됩니다. `FindAncestorBindingContext` 모드는 부모 요소의 `BindingContext`에 바인딩하는 데 사용됩니다.

> [!WARNING]
> `FindAncestor` 및 `FindAncestorBindingContext` 상대 바인딩 모드를 사용할 때는 `AncestorType` 속성을 `Type`으로 설정해야 합니다. 그러지 않으면 `XamlParseException`이 throw됩니다.

`Mode` 속성이 명시적으로 설정되지 않은 경우, `AncestorType` 속성을 [`Element`](xref:Xamarin.Forms.Element)에서 파생된 형식으로 설정하면 `Mode` 속성이 암시적으로 `FindAncestor`로 설정됩니다. 마찬가지로, `AncestorType` 속성을 `Element`에서 파생되지 않은 형식으로 설정하면 `Mode` 속성이 암시적으로 `FindAncestorBindingContext`로 설정됩니다.

다음 XAML은 `Mode` 속성이 암시적으로 `FindAncestorBindingContext`로 설정되는 예제를 보여 줍니다.

```xaml
<ContentPage ...
             BindingContext="{Binding Source={RelativeSource Self}, Path=DefaultViewModel}">
    <StackLayout>
        <ListView ItemsSource="{Binding Employees}">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <Label Text="{Binding Fullname}"
                                   VerticalOptions="Center" />
                            <Button Text="Delete"
                                    Command="{Binding Source={RelativeSource AncestorType={x:Type local:PeopleViewModel}}, Path=DeleteEmployeeCommand}"
                                    CommandParameter="{Binding}"
                                    HorizontalOptions="EndAndExpand" />
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

이 예제에서, 페이지의 `BindingContext`가 자기 자신의 `DefaultViewModel` 속성으로 설정되었습니다. 이 속성은 페이지의 코드 숨김 파일에 정의되어 있으며, viewmodel 인스턴스를 제공합니다. [`ListView`](xref:Xamarin.Forms.ListView)는 viewmodel의 `Employees` 속성에 바인딩됩니다. `ListView`의 각 항목의 모양을 정의하는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)은 [`Button`](xref:Xamarin.Forms.Button)을 포함합니다. 단추의 `Command` 속성은 부모의 viewmodel에서 `DeleteEmployeeCommand`에 바인딩되어 있습니다. `Button`을 탭하면 직원 한 명이 삭제됩니다.

[![iOS 및 Android의 FindAncestor 모드 상대 바인딩 스크린샷](relative-bindings-images/findancestor-relative-binding.png "FindAncestor 상대 바인딩 모드")](relative-bindings-images/findancestor-relative-binding-large.png#lightbox "FindAncestor 상대 바인딩 모드")

이에 더해, 선택적 `AncestorLevel` 속성은 시각적 트리에 특정 유형의 상위 항목이 둘 이상 있는 경우 상위 항목을 쉽게 구분하는 데 도움이 됩니다.

```xaml
<Label Text="{Binding Source={RelativeSource AncestorType={x:Type Entry}, AncestorLevel=2}, Path=Text}" />
```

이 예제에서 `Label.Text` 속성은 바인딩의 대상 요소에서 시작하는 상향 경로에서 발견된 두 번째 [`Entry`](xref:Xamarin.Forms.Entry)의 `Text` 속성에 바인딩됩니다.

> [!NOTE]
> `AncestorLevel` 속성은 바인딩 대상 요소에 가장 가까운 상위 항목을 찾도록 1로 설정되어야 합니다.

## <a name="bind-to-a-templated-parent"></a>템플릿화된 부모에 바인딩

`TemplatedParent` 상대 바인딩 모드는 컨트롤 템플릿 내부에서 해당 템플릿이 적용되는 런타임 개체 인스턴스(템플릿화된 부모)에 바인딩하는 데 사용됩니다. 이 모드는 상대 바인딩이 컨트롤 템플릿 내부에 있는 경우에만 적용되며, `TemplateBinding`을 설정하는 것과 비슷합니다.

다음 XAML은 `TemplatedParent` 상대 바인딩 모드의 예를 보여 줍니다.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ControlTemplate x:Key="CardViewControlTemplate">
            <Frame BindingContext="{Binding Source={RelativeSource TemplatedParent}}"
                   BackgroundColor="{Binding CardColor}"
                   BorderColor="{Binding BorderColor}"
                   ...>
                <Grid>
                    ...
                    <Label Text="{Binding CardTitle}"
                           ... />
                    <BoxView BackgroundColor="{Binding BorderColor}"
                             ... />
                    <Label Text="{Binding CardDescription}"
                           ... />
                </Grid>
            </Frame>
        </ControlTemplate>
    </ContentPage.Resources>
    <StackLayout>        
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="John Doe"
                           CardDescription="Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla elit dolor, convallis non interdum."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"
                           ControlTemplate="{StaticResource CardViewControlTemplate}" />
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="Jane Doe"
                           CardDescription="Phasellus eu convallis mi. In tempus augue eu dignissim fermentum. Morbi ut lacus vitae eros lacinia."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"
                           ControlTemplate="{StaticResource CardViewControlTemplate}" />
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="Xamarin Monkey"
                           CardDescription="Aliquam sagittis, odio lacinia fermentum dictum, mi erat scelerisque erat, quis aliquet arcu."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"
                           ControlTemplate="{StaticResource CardViewControlTemplate}" />
    </StackLayout>
</ContentPage>
```

이 예제에서, `ControlTemplate`의 루트 요소인 [`Frame`](xref:Xamarin.Forms.Frame)의 `BindingContext`는 해당 템플릿이 적용되는 런타임 개체 인스턴스로 설정됩니다. 따라서 `Frame`과 그 자식은 각 `CardView` 개체의 속성에 대해 바인딩 식이 확인됩니다.

[![iOS 및 Android의 TemplatedParent 모드 상대 바인딩 스크린샷](relative-bindings-images/templatedparent-relative-binding.png "TemplatedParent 상대 바인딩 모드")](relative-bindings-images/templatedparent-relative-binding-large.png#lightbox "TemplatedParent 상대 바인딩 모드")

컨트롤 템플릿에 대한 자세한 내용은 [Xamarin.Forms 컨트롤 템플릿](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)을 참조하세요.

## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [XAML 태그 확장](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Xamarin.Forms 컨트롤 템플릿](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)
