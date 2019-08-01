---
title: Xamarin.Forms DataTemplate 만들기
description: 데이터 템플릿은 ResourceDictionary에서 인라인으로 만들거나 사용자 지정 형식 또는 적절한 Xamarin.Forms 셀 형식으로 만들 수 있습니다. 이 문서에서는 각 기법을 살펴봅니다.
ms.prod: xamarin
ms.assetid: CFF4AB5E-9069-461C-84D8-F9F6C38510AB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 1163f264fc54a461d8d95854524439589cdc81f5
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68646979"
---
# <a name="creating-a-xamarinforms-datatemplate"></a>Xamarin.Forms DataTemplate 만들기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)

_데이터 템플릿은 ResourceDictionary에서 인라인으로 만들거나 사용자 지정 형식 또는 적절한 Xamarin.Forms 셀 형식으로 만들 수 있습니다. 이 문서에서는 각 기법을 살펴봅니다._

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)에 대한 일반적인 시나리오는 [`ListView`](xref:Xamarin.Forms.ListView)에서 개체 컬렉션의 데이터를 표시하는 것입니다. [`ListView`](xref:Xamarin.Forms.ListView)에서 각 셀에 대한 데이터 모양은 [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) 속성을 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)으로 설정하여 관리할 수 있습니다. 이 작업을 수행하는 데 사용할 수 있는 여러 기법이 있습니다.

- [인라인 DataTemplate 만들기](#inline)
- [형식과 함께 DataTemplate 만들기](#type)
- [DataTemplate을 리소스로 만들기](#resource)

다음 스크린샷에 표시된 것처럼 사용되는 기법에 관계 없이, [`ListView`](xref:Xamarin.Forms.ListView)에서 각 셀의 모양은 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)으로 정의됩니다.

![](creating-images/data-template-appearance.png "DataTemplate을 사용한 ListView")

<a name="inline" />

## <a name="creating-an-inline-datatemplate"></a>인라인 DataTemplate 만들기

[`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) 속성은 인라인 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)으로 설정할 수 있습니다. 해당 컨트롤 속성의 직계 자식으로 배치되는 인라인 템플릿은 다른 곳에서 데이터 템플릿을 재사용할 필요가 없는 경우 사용해야 합니다. `DataTemplate`에 지정된 요소는 다음 XAML 코드 예제와 같이 각 셀의 모양을 정의합니다.

```xaml
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
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <Grid>
                    ...
                    <Label Text="{Binding Name}" FontAttributes="Bold" />
                    <Label Grid.Column="1" Text="{Binding Age}" />
                    <Label Grid.Column="2" Text="{Binding Location}" HorizontalTextAlignment="End" />
                </Grid>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

인라인 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)의 자식은 [`Cell`](xref:Xamarin.Forms.Cell) 형식이거나 파생된 것이어야 합니다. 이 예제에서는 `Cell`에서 파생되는 [`ViewCell`](xref:Xamarin.Forms.ViewCell)을 사용합니다. `ViewCell` 내의 레이아웃은 여기서 [`Grid`](xref:Xamarin.Forms.Grid)로 관리됩니다. `Grid`는 [`Text`](xref:Xamarin.Forms.Label.Text) 속성을 컬렉션에 있는 각 `Person` 개체의 적절한 속성으로 바인딩하는 세 개의 [`Label`](xref:Xamarin.Forms.Label) 인스턴스를 포함합니다.

동등한 C# 코드는 다음 코드 예제와 같습니다.

```csharp
public class WithDataTemplatePageCS : ContentPage
{
    public WithDataTemplatePageCS()
    {
        ...
        var people = new List<Person>
        {
            new Person { Name = "Steve", Age = 21, Location = "USA" },
            ...
        };

        var personDataTemplate = new DataTemplate(() =>
        {
            var grid = new Grid();
            ...
            var nameLabel = new Label { FontAttributes = FontAttributes.Bold };
            var ageLabel = new Label();
            var locationLabel = new Label { HorizontalTextAlignment = TextAlignment.End };

            nameLabel.SetBinding(Label.TextProperty, "Name");
            ageLabel.SetBinding(Label.TextProperty, "Age");
            locationLabel.SetBinding(Label.TextProperty, "Location");

            grid.Children.Add(nameLabel);
            grid.Children.Add(ageLabel, 1, 0);
            grid.Children.Add(locationLabel, 2, 0);

            return new ViewCell { View = grid };
        });

        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = {
                ...
                new ListView { ItemsSource = people, ItemTemplate = personDataTemplate, Margin = new Thickness(0, 20, 0, 0) }
            }
        };
    }
}
```

C#에서는 인라인 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)이 `Func` 인수를 지정하는 생성자 오버로드를 사용하여 생성됩니다.

<a name="type" />

## <a name="creating-a-datatemplate-with-a-type"></a>형식과 함께 DataTemplate 만들기

[`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) 속성을 셀 형식에서 만든 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)으로 설정할 수도 있습니다. 이 방법의 장점은 셀 형식으로 정의된 모양을 애플리케이션 전체에서 여러 데이터 템플릿으로 재사용할 수 있다는 것입니다. 다음 XAML 코드에서는 이 방법에 대한 예제를 보여 줍니다.

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
                    ...
                </x:Array>
            </ListView.ItemsSource>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <local:PersonCell />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

여기에 [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) 속성은 셀 모양을 정의하는 사용자 지정 형식에서 생성된 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)으로 설정됩니다. 사용자 지정 형식은 다음 코드처럼 [`ViewCell`](xref:Xamarin.Forms.ViewCell)에서 파생되어야 합니다.

```xaml
<ViewCell xmlns="http://xamarin.com/schemas/2014/forms"
          xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
          x:Class="DataTemplates.PersonCell">
     <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.2*" />
            <ColumnDefinition Width="0.3*" />
        </Grid.ColumnDefinitions>
        <Label Text="{Binding Name}" FontAttributes="Bold" />
        <Label Grid.Column="1" Text="{Binding Age}" />
        <Label Grid.Column="2" Text="{Binding Location}" HorizontalTextAlignment="End" />
    </Grid>
</ViewCell>
```

[`ViewCell`](xref:Xamarin.Forms.ViewCell) 내에서 레이아웃은 여기서 [`Grid`](xref:Xamarin.Forms.Grid)로 관리됩니다. `Grid`는 [`Text`](xref:Xamarin.Forms.Label.Text) 속성을 컬렉션에 있는 각 `Person` 개체의 적절한 속성으로 바인딩하는 세 개의 [`Label`](xref:Xamarin.Forms.Label) 인스턴스를 포함합니다.

해당하는 C# 코드가 다음 예제에 표시됩니다.

```csharp
public class WithDataTemplatePageFromTypeCS : ContentPage
{
    public WithDataTemplatePageFromTypeCS()
    {
        ...
        var people = new List<Person>
        {
            new Person { Name = "Steve", Age = 21, Location = "USA" },
            ...
        };

        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = {
                ...
                new ListView { ItemTemplate = new DataTemplate(typeof(PersonCellCS)), ItemsSource = people, Margin = new Thickness(0, 20, 0, 0) }
            }
        };
    }
}
```

C#에서는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)이 인수로 셀 형식을 지정하는 생성자 오버로드를 사용하여 생성됩니다. 셀 형식은 다음 코드 예제처럼 [`ViewCell`](xref:Xamarin.Forms.ViewCell)에서 파생되어야 합니다.

```csharp
public class PersonCellCS : ViewCell
{
    public PersonCellCS()
    {
        var grid = new Grid();
        ...
        var nameLabel = new Label { FontAttributes = FontAttributes.Bold };
        var ageLabel = new Label();
        var locationLabel = new Label { HorizontalTextAlignment = TextAlignment.End };

        nameLabel.SetBinding(Label.TextProperty, "Name");
        ageLabel.SetBinding(Label.TextProperty, "Age");
        locationLabel.SetBinding(Label.TextProperty, "Location");

        grid.Children.Add(nameLabel);
        grid.Children.Add(ageLabel, 1, 0);
        grid.Children.Add(locationLabel, 2, 0);

        View = grid;
    }
}
```

> [!NOTE]
> Xamarin.Forms에도 [`ListView`](xref:Xamarin.Forms.ListView) 셀에 간단한 데이터를 표시하는 데 사용할 수 있는 셀 형식이 포함됩니다. 자세한 내용은 [셀 모양](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)을 참조하세요.

<a name="resource" />

## <a name="creating-a-datatemplate-as-a-resource"></a>DataTemplate을 리소스로 만들기

데이터 템플릿은 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에서 재사용 가능한 개체로 만들 수도 있습니다. 이 작업은 다음 XAML 코드 예제와 같이, `ResourceDictionary`에 설명적 키를 제공하는 고유한 `x:Key` 특성을 각 선언에 제공하여 수행합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="personTemplate">
                 <ViewCell>
                    <Grid>
                        ...
                    </Grid>
                </ViewCell>
            </DataTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    <StackLayout Margin="20">
        ...
        <ListView ItemTemplate="{StaticResource personTemplate}" Margin="0,20,0,0">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:Person}">
                    <local:Person Name="Steve" Age="21" Location="USA" />
                    ...
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </StackLayout>
</ContentPage>
```

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)은 `StaticResource` 태그 확장을 사용하여 [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) 속성에 할당됩니다. `DataTemplate`은 페이지의 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에 정의되지만 컨트롤 수준이나 애플리케이션 수준에서도 정의될 수 있습니다.

다음 코드 예제에서는 C#에 해당하는 페이지를 보여 줍니다.

```csharp
public class WithDataTemplatePageCS : ContentPage
{
  public WithDataTemplatePageCS ()
  {
    ...
    var personDataTemplate = new DataTemplate (() => {
      var grid = new Grid ();
      ...
      return new ViewCell { View = grid };
    });

    Resources = new ResourceDictionary ();
    Resources.Add ("personTemplate", personDataTemplate);

    Content = new StackLayout {
      Margin = new Thickness(20),
      Children = {
        ...
        new ListView { ItemTemplate = (DataTemplate)Resources ["personTemplate"], ItemsSource = people };
      }
    };
  }
}
```

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)은 [`Add`](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) 메서드를 사용하여, 검색 시 `DataTemplate`을 참조하는 데 사용되는 `Key` 문자열을 지정하는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에 추가됩니다.

## <a name="summary"></a>요약

이 문서에서는 사용자 지정 형식 또는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에서 데이터 템플릿, 인라인을 만드는 방법에 대해 설명했습니다. 다른 곳에서 데이터 템플릿을 다시 사용할 필요가 없으면 인라인 템플릿을 사용해야 합니다. 또는 데이터 템플릿을 사용자 지정 유형으로 정의하거나 컨트롤 수준, 페이지 수준 또는 애플리케이션 수준 리소스로 정의하여 다시 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [셀 모양](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [데이터 템플릿(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)
- [DataTemplate](xref:Xamarin.Forms.DataTemplate)
