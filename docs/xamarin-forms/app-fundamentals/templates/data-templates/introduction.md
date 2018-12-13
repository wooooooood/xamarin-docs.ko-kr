---
title: Xamarin.Forms 데이터 템플릿 소개
description: Xamarin.Forms 데이터 템플릿은 지원되는 컨트롤의 데이터 표현을 정의하는 기능을 제공합니다. 이 문서에서는 데이터 템플릿이 필요한 이유를 검토하면서 데이터 템플릿을 소개합니다.
ms.prod: xamarin
ms.assetid: 4ED4ACF4-BE4A-44ED-8EAF-C03947B8663B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 129ce7a04b93bfb3cb1b9a1639aee61cd56d09d5
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998921"
---
# <a name="introduction-to-xamarinforms-data-templates"></a>Xamarin.Forms 데이터 템플릿 소개

_Xamarin.Forms 데이터 템플릿은 지원되는 컨트롤의 데이터 표현을 정의하는 기능을 제공합니다. 이 문서에서는 데이터 템플릿이 필요한 이유를 검토하면서 데이터 템플릿을 소개합니다._

`Person` 개체 컬렉션을 표시하는 [`ListView`](xref:Xamarin.Forms.ListView)를 생각해봅니다. 다음 코드 예제는 `Person` 클래스의 정의를 보여줍니다.

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Location { get; set; }
}
```

`Person` 클래스는 `Person` 개체를 만들 때 설정할 수 있는 `Name`, `Age` 및 `Location` 속성을 정의합니다. [`ListView`](xref:Xamarin.Forms.ListView)는 다음 XAML 코드 예제처럼 `Person` 개체 컬렉션을 표시하는 데 사용됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataTemplates"
             ...>
    <StackLayout Margin="20">
        ...
        <ListView Margin="0,20,0,0">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:Person}">
                    <local:Person Name="Steve" Age="21" Location="USA" />
                    <local:Person Name="John" Age="37" Location="USA" />
                    <local:Person Name="Tom" Age="42" Location="UK" />
                    <local:Person Name="Lucas" Age="29" Location="Germany" />
                    <local:Person Name="Tariq" Age="39" Location="UK" />
                    <local:Person Name="Jane" Age="30" Location="USA" />
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </StackLayout>
</ContentPage>
```

항목은 `Person` 인스턴스 배열에서 [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) 속성을 인스턴스화하여 [`ListView`](xref:Xamarin.Forms.ListView)에 XAML로 추가됩니다.

> [!NOTE]
> `x:Array` 요소는 배열의 항목 유형을 나타내는 `Type` 특성이 필요합니다.

해당하는 C# 페이지는 [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) 속성을 `Person` 인스턴스의 `List`에 초기화하는 다음 코드 예제에 표시되어 있습니다.

```csharp
public WithoutDataTemplatePageCS()
{
    ...
    var people = new List<Person>
    {
        new Person { Name = "Steve", Age = 21, Location = "USA" },
        new Person { Name = "John", Age = 37, Location = "USA" },
        new Person { Name = "Tom", Age = 42, Location = "UK" },
        new Person { Name = "Lucas", Age = 29, Location = "Germany" },
        new Person { Name = "Tariq", Age = 39, Location = "UK" },
        new Person { Name = "Jane", Age = 30, Location = "USA" }
    };

    Content = new StackLayout
    {
        Margin = new Thickness(20),
        Children = {
            ...
            new ListView { ItemsSource = people, Margin = new Thickness(0, 20, 0, 0) }
        }
    };
}
```

[`ListView`](xref:Xamarin.Forms.ListView)는 컬렉션의 개체를 표시할 때 `ToString`을 호출합니다. `Person.ToString` 재정의가 없기 때문에 `ToString`은 다음 스크린샷에 표시된 것처럼 각 개체의 형식 이름을 반환합니다.

![](introduction-images/no-data-template.png "데이터 템플릿 없는 ListView")

`Person` 개체는 다음 코드 예제처럼 의미 있는 데이터를 표시하도록 `ToString` 메서드를 재정의할 수 있습니다.

```csharp
public class Person
{
  ...
  public override string ToString ()
  {
    return Name;
  }
}
```

이렇게 하면 다음 스크린샷처럼 [`ListView`](xref:Xamarin.Forms.ListView)는 컬렉션의 각 개체에 대한 `Person.Name` 속성 값을 표시합니다.

![](introduction-images/override-tostring.png "데이터 템플릿을 사용하는 ListView")

`Person.ToString` 재정의는 `Name`, `Age` 및 `Location` 속성으로 구성되는 형식이 지정된 문자열을 반환할 수 있습니다. 그러나 이 방법은 데이터의 각 항목이 표시되는 모양을 제한적으로 제어할 수 있습니다. 유연성을 높이려면 데이터의 모양을 정의하는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)을 만들어야 합니다.

## <a name="creating-a-datatemplate"></a>DataTemplate 만들기

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)은 데이터 모양을 지정하는 데 사용되며, 일반적으로 데이터 바인딩을 사용하여 데이터를 표시합니다. 일반적인 사용 시나리오는 [`ListView`](xref:Xamarin.Forms.ListView)의 개체 컬렉션에 있는 데이터를 표시하는 것입니다. 예를 들어 `ListView`가 `Person` 개체의 컬렉션에 바인딩되면 `ListView.ItemTemplate` 속성은 `ListView`에 있는 각 `Person` 개체의 모양을 정의하는 `DataTemplate`으로 설정됩니다. `DataTemplate`은 각 `Person` 개체의 속성 값에 바인딩되는 요소를 포함할 것입니다. 데이터 바인딩에 대한 자세한 내용은 [데이터 바인딩 기본](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)을 참조하세요.

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)은 다음 속성의 값으로 사용할 수 있습니다.

- [`ListView.HeaderTemplate`](xref:Xamarin.Forms.ListView.HeaderTemplate)
- [`ListView.FooterTemplate`](xref:Xamarin.Forms.ListView.FooterTemplate)
- [`ListView.GroupHeaderTemplate`](xref:Xamarin.Forms.ListView.GroupHeaderTemplate)
- [`ItemsView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) - [`ListView`](xref:Xamarin.Forms.ListView)에 의해 상속됩니다.
- [`MultiPage.ItemTemplate`](xref:Xamarin.Forms.MultiPage`1) - [`CarouselPage`](xref:Xamarin.Forms.CarouselPage), [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 및 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)에 의해 상속됩니다.

> [!NOTE]
> [`TableView`](xref:Xamarin.Forms.TableView)는 [`Cell`](xref:Xamarin.Forms.Cell) 개체를 사용하지만 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)을 사용하지 않습니다. 데이터 바인딩이 항상 `Cell` 개체에서 직접 설정되기 때문입니다.

위에 나열된 속성의 직계 자식으로 배치되는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)울 *인라인 템플릿*이라고 합니다. 또는 `DataTemplate`을 컨트롤 수준, 페이지 수준 또는 애플리케이션 수준 리소스로 정의할 수 있습니다. [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)을 정의할 위치를 선택하면 사용할 수 있는 위치가 결정됩니다.

- 컨트롤 수준에서 정의된 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)은 컨트롤에만 적용할 수 있습니다.
- 페이지 수준에서 정의된 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)은 페이지의 여러 유효한 컨트롤에 적용할 수 있습니다.
- 애플리케이션 수준에서 정의된 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)은 애플리케이션 전체의 유효한 컨트롤에 적용할 수 있습니다.

보기 계층 구조의 하단에 위치한 데이터 템플릿은 `x:Key` 특성을 공유할 때 상단에 정의된 데이터 템플릿보다 우선권을 갖습니다. 예를 들어 애플리케이션 수준 데이터 템플릿은 페이지 수준 데이터 템플릿에 의해 재정의되고, 페이지 수준 데이터 템플릿은 컨트롤 수준 데이터 템플릿 또는 인라인 데이터 템플릿에 의해 재정의됩니다.


## <a name="related-links"></a>관련 링크

- [셀 모양](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [데이터 템플릿(샘플)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](xref:Xamarin.Forms.DataTemplate)
