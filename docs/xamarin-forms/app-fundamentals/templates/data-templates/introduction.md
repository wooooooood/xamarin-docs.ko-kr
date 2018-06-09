---
title: Xamarin.Forms 데이터 서식 파일 소개
description: Xamarin.Forms 데이터 템플릿은 지원 되는 컨트롤에 데이터의 표현을 정의 하는 기능을 제공 합니다. 이 문서는 필요한 이유를 검사 하는 데이터 서식 파일에 대 한 소개를 제공 합니다.
ms.prod: xamarin
ms.assetid: 4ED4ACF4-BE4A-44ED-8EAF-C03947B8663B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: e54c7d3ea01c59a20561b69c6e790747567d92f0
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240179"
---
# <a name="introduction-to-xamarinforms-data-templates"></a>Xamarin.Forms 데이터 서식 파일 소개

_Xamarin.Forms 데이터 템플릿은 지원 되는 컨트롤에 데이터의 표현을 정의 하는 기능을 제공 합니다. 이 문서는 필요한 이유를 검사 하는 데이터 서식 파일에 대 한 소개를 제공 합니다._

고려는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 의 컬렉션을 표시 하는 `Person` 개체입니다. 정의 표시 하는 다음 코드 예제는 `Person` 클래스:

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Location { get; set; }
}
```

`Person` 클래스를 정의 `Name`, `Age`, 및 `Location` 설정 될 수 있는 속성 한 `Person` 개체가 만들어집니다. [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 의 컬렉션을 표시 하는 데 사용 되 `Person` 다음 XAML 코드 예제와 같이 개체:

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

에 항목이 추가 되는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 초기화 하 여 XAML에서는 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) 속성의 배열에서 `Person` 인스턴스.

> [!NOTE]
> `x:Array` 요소를 사용 하려면 한 `Type` 배열에 있는 항목의 유형을 나타내는 특성입니다.

C#의 해당 페이지를 초기화 하는 다음 코드 예제에 표시 됩니다는 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) 속성을 한 `List` 의 `Person` 인스턴스:

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

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 호출 `ToString` 때 컬렉션의 개체를 표시 합니다. 있기 때문에 없는 `Person.ToString` 재정의 `ToString` 다음 스크린샷에서 같이 각 개체의 형식 이름을 반환 합니다.

![](introduction-images/no-data-template.png "데이터 서식 파일 없이 ListView")

`Person` 개체 재정의할 수는 `ToString` 메서드에 다음 코드 예제와 같이 의미 있는 데이터를 표시 하려면:

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

이 인해는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 표시는 `Person.Name` 다음 스크린샷에 표시 된 것 처럼 컬렉션에 있는 각 개체에 대 한 속성 값:

![](introduction-images/override-tostring.png "데이터 템플릿 사용 하 여 ListView")

`Person.ToString` 재정의 구성 된 서식이 지정 된 문자열을 반환할 수는 `Name`, `Age`, 및 `Location` 속성입니다. 그러나이 방법을 사용은 제한 된 제어할 데이터의 각 항목의 모양 제공합니다. 더 큰 유연성에 대 한는 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 만들 수는 데이터의 모양을 정의 하는 합니다.

## <a name="creating-a-datatemplate"></a>DataTemplate 만들기

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 데이터의 모양을 지정 하는 데 사용 되 고 일반적으로 데이터 바인딩을 사용 하 여 데이터를 표시 합니다. 개체의 컬렉션에서 데이터를 표시할 때의 일반적인 사용 시나리오는 한 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)합니다. 예를 들어,는 `ListView` 의 컬렉션에 바인딩된 `Person` 개체는 `ListView.ItemTemplate` 속성으로 설정 됩니다는 `DataTemplate` 각각의 모양을 정의 하 `Person` 개체는 `ListView`합니다. `DataTemplate` 각 속성 값에 바인딩 요소가 포함 됩니다 `Person` 개체입니다. 데이터 바인딩에 대한 자세한 내용은 [데이터 바인딩 기본](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)을 참조하세요.

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 다음과 같은 속성에 대 한 값으로 사용할 수 있습니다.

- [`ListView.HeaderTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.HeaderTemplate/)
- [`ListView.FooterTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.FooterTemplate/)
- [`ListView.GroupHeaderTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupHeaderTemplate/)
- [`ItemsView.ItemTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/)에 상속 됩니다 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)합니다.
- [`MultiPage.ItemTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/)에 상속 됩니다 [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), 및 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)합니다.

> [!NOTE]
> 하지만 [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) 사용 [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) 개체를 사용 하지 않는 한 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)합니다. 직접 데이터 바인딩은 항상 설정 때문에 이것이 `Cell` 개체입니다.

A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 위에 나열 된 속성의 직계 자식이 라고 하는 대로 놓입니다는 *인라인 템플릿*합니다. 또는 한 `DataTemplate` 제어 수준, 페이지 수준 또는 응용 프로그램 수준 리소스도 정의할 수 있습니다. 정의할 위치를 선택는 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 사용할 수 있는 위치에 영향을 미칩니다.

- A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 정의 컨트롤에서 수준 수만 컨트롤에 적용할 수는 있습니다.
- A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 정의 페이지에서 수준 페이지에서 여러 유효한 컨트롤에 적용 수 있습니다.
- A [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 응용 프로그램 수준에서 정의 된 응용 프로그램에 걸쳐 유효한 적용된 한 제어 될 수 있습니다.

데이터 템플릿 보기 계층 구조의 하위 우선 공유 하는 때를 높을수록 정의 `x:Key` 특성입니다. 예를 들어, 페이지 수준 데이터 템플릿을 응용 프로그램 수준 데이터 템플릿을 덮어쓸 수 있습니다 및 페이지 수준 데이터 템플릿이 제어 수준 데이터 템플릿이나 인라인 데이터 템플릿을 덮어쓸 수 있습니다.


## <a name="related-links"></a>관련 링크

- [셀 모양](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [데이터 템플릿 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [데이터 템플릿](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)
