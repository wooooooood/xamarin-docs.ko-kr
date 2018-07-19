---
title: Xamarin.Forms DataTemplate 만들기
description: 데이터 템플릿 또는 사용자 지정 형식 또는 적절 한 Xamarin.Forms 셀 형식에서 ResourceDictionary를 인라인으로 만들 수 있습니다. 이 문서에서는 각 기법을 살펴봅니다.
ms.prod: xamarin
ms.assetid: CFF4AB5E-9069-461C-84D8-F9F6C38510AB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 63f9bf82bc8e637aced1afa5d5699ac1e8dc3f8c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994617"
---
# <a name="creating-a-xamarinforms-datatemplate"></a>Xamarin.Forms DataTemplate 만들기

_데이터 템플릿 또는 사용자 지정 형식 또는 적절 한 Xamarin.Forms 셀 형식에서 ResourceDictionary를 인라인으로 만들 수 있습니다. 이 문서에서는 각 기법을 살펴봅니다._

에 대 한 일반적인 사용 시나리오를 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 표시 되는 개체의 컬렉션에서 데이터를 [ `ListView` ](xref:Xamarin.Forms.ListView)합니다. 각 셀에 대 한 데이터의 모양을 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 설정 하 여 관리할 수 있습니다 합니다 [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) 속성을를 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)합니다. 이 위해 사용할 수 있는 기술의 사항이 있습니다.

- [만들기는 인라인 DataTemplate](#inline)합니다.
- [DataTemplate을 형식으로 만들기](#type)합니다.
- [DataTemplate을 리소스로 만들기](#resource)합니다.

사용 되는 기법을에 관계 없이 결과 각 셀의 모양을 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 정의한를 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)다음 스크린샷에 표시 된 것 처럼:

![](creating-images/data-template-appearance.png "DataTemplate 사용 하 여 ListView")

<a name="inline" />

## <a name="creating-an-inline-datatemplate"></a>데이터는 인라인 템플릿 만들기

합니다 [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) 인라인으로 속성을 설정할 수 있습니다 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)합니다. 다른 곳에서 데이터 템플릿을 다시 사용할 필요가 없는 경우 적절 한 컨트롤 속성의 직계 자식으로 배치 되는는 인라인 템플릿을 사용 해야 합니다. 에 지정 된 요소는 `DataTemplate` 다음 XAML 코드 예제에 표시 된 대로 각 셀의 모양을 정의 합니다.

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

인라인 자식의 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 의 수 또는에서 파생을 입력 해야 합니다 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell)합니다. 내의 레이아웃을 `ViewCell` 여기에서 관리 되는 [ `Grid` ](xref:Xamarin.Forms.Grid)합니다. 합니다 `Grid` 포함 된 세 가지 [ `Label` ](xref:Xamarin.Forms.Label) 바인딩하는 인스턴스 해당 [ `Text` ](xref:Xamarin.Forms.Label.Text) 각각의 적절 한 속성을 속성 `Person` 컬렉션의 개체입니다.

해당하는 C# 코드가 다음 코드 예제에 표시됩니다.

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

C#에서 인라인 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 지정 하는 생성자 오버 로드를 사용 하 여 만들어집니다는 `Func` 인수입니다.

<a name="type" />

## <a name="creating-a-datatemplate-with-a-type"></a>DataTemplate을 형식으로 만들기

합니다 [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) 속성 설정할 수도 있습니다는 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 셀 형식에서 만들어집니다. 이 방법의 장점은 응용 프로그램 전체에서 여러 데이터 템플릿에서 셀 유형을 정의한 모양을 재사용이 가능 한다는 점입니다. 다음 XAML 코드를이 방법의 예를 보여 줍니다.

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

여기에 [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) 속성을 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 셀 모양을 정의 하는 사용자 지정 형식에서 만들어집니다. 사용자 지정 형식을 형식에서 파생 되어야 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell)다음 코드 예제 에서처럼:

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

내 합니다 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), 레이아웃 여기에서 관리 되는 [ `Grid` ](xref:Xamarin.Forms.Grid)합니다. 합니다 `Grid` 포함 된 세 가지 [ `Label` ](xref:Xamarin.Forms.Label) 바인딩하는 인스턴스 해당 [ `Text` ](xref:Xamarin.Forms.Label.Text) 각각의 적절 한 속성을 속성 `Person` 컬렉션의 개체입니다.

해당 하는 C# 코드는 다음 예제에 표시 됩니다.

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

C#의 경우에 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 셀 형식 인수로 지정 하는 생성자 오버 로드를 사용 하 여 만들어집니다. 셀 유형을 형식에서 파생 되어야 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell)다음 코드 예제 에서처럼:

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
> Xamarin.Forms에도 간단한 데이터를 표시 하려면 사용할 수 있는 셀 형식 포함 [ `ListView` ](xref:Xamarin.Forms.ListView) 셀입니다. 자세한 내용은 [셀 모양](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)합니다.

<a name="resource" />

## <a name="creating-a-datatemplate-as-a-resource"></a>DataTemplate을 리소스로 만들기

데이터 템플릿은 다시 사용할 수 있는 개체로 만들 수 있습니다는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)합니다. 고유한 각 선언을 제공 하 여 이렇게 `x:Key` 에 설명이 포함 된 키를 사용 하 여 제공 하는 특성을 `ResourceDictionary`다음 XAML 코드 예제 에서처럼:

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

[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 에 할당 되는 [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) 사용 하 여 속성은 `StaticResource` 태그 확장 합니다. 있는 동안 합니다 `DataTemplate` 페이지에 정의 된 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), 제어 수준 또는 응용 프로그램 수준에서 정의할 수도 있습니다.

다음 코드 예제에서는 C#의 해당 페이지를 보여 줍니다.

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

[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 에 추가 됩니다는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 사용 하 여는 [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) 메서드를 지정 하는 `Key` 하는 데 사용 되는 문자열 참조 된 `DataTemplate` 검색 하는 경우.

## <a name="summary"></a>요약

이 문서에서는 사용자 지정 형식에서 또는 데이터 템플릿, 인라인을 만드는 방법 설명에 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)합니다. 인라인 템플릿을 사용할 경우 데이터 템플릿을 다른 곳에서 다시 사용할 필요가 없습니다. 또는 데이터 템플릿 사용자 지정 형식으로 또는 제어 수준, 페이지 수준 또는 응용 프로그램 수준 리소스로 정의 하 여 재사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [셀 모양](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [데이터 템플릿 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [데이터 템플릿](xref:Xamarin.Forms.DataTemplate)
