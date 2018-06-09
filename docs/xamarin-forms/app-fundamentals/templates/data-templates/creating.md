---
title: Xamarin.Forms DataTemplate 만들기
description: 데이터 템플릿은 ResourceDictionary, 또는 사용자 정의 형식 또는 적절 한 Xamarin.Forms 셀 형식에서 인라인을 만들 수 있습니다. 이 문서는 각 기법을 탐색합니다.
ms.prod: xamarin
ms.assetid: CFF4AB5E-9069-461C-84D8-F9F6C38510AB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 8aa0ad693fd1a7f086492f93f18c1e33871dee0e
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240514"
---
# <a name="creating-a-xamarinforms-datatemplate"></a>Xamarin.Forms DataTemplate 만들기

_데이터 템플릿은 ResourceDictionary, 또는 사용자 정의 형식 또는 적절 한 Xamarin.Forms 셀 형식에서 인라인을 만들 수 있습니다. 이 문서는 각 기법을 탐색합니다._

에 대 한 일반적인 사용 시나리오는 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 에 있는 개체의 컬렉션에서 데이터가 표시 되는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)합니다. 각 셀에 대 한 데이터의 모양을 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 설정 하 여 관리할 수는 [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) 속성을 한 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)합니다. 이를 위해 사용할 수 있는 방법의 여러 가지:

- [인라인 DataTemplate 만들기](#inline)합니다.
- [형식으로는 DataTemplate 만들기](#type)합니다.
- [만들기를 리소스로 DataTemplate](#resource)합니다.

사용 하는 방법에 관계 없이 결과 하에 있는 각 셀의 모양을 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 가 정의한는 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)다음 스크린샷에 표시 된 것 처럼:

![](creating-images/data-template-appearance.png "DataTemplate 있는 ListView")

<a name="inline" />

## <a name="creating-an-inline-datatemplate"></a>인라인 DataTemplate 만들기

[ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) 인라인으로 속성을 설정할 수 있습니다 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)합니다. 데이터 서식 파일을 다른 곳에서 다시 사용할 필요가 없는 경우 인라인 템플릿은 형식인 적절 한 컨트롤 속성의 자식으로 배치 된 하나를 사용 해야 합니다. 에 지정 된 요소는 `DataTemplate` 다음 XAML 코드 예제에 나와 있는 것 처럼 각 셀의 모양을 정의 합니다.

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

인라인 자식 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 해야의 또는에서 파생, 입력 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)합니다. 레이아웃 내에서 `ViewCell` 여기에서 관리 되는 [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)합니다. `Grid` 세 개 포함 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 해당 바인딩 인스턴스를 해당 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) 각각의 적절 한 속성을 속성 `Person` 컬렉션의 개체입니다.

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

C#에서는 인라인 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 지정 하는 생성자 오버 로드를 사용 하 여 만들어집니다는 `Func` 인수입니다.

<a name="type" />

## <a name="creating-a-datatemplate-with-a-type"></a>DataTemplate 형식으로 만들기

[ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) 속성으로 설정할 수도 있습니다는 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 셀 형식에서 만들어집니다. 이 방식의 장점은 셀 유형을 정의한 모양에서 여러 데이터 템플릿을 응용 프로그램에 걸쳐 다시 사용할 수입니다. 다음 XAML 코드에서는이 접근 방법의 예를 보여 줍니다.

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

여기서는 [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) 속성이로 설정 되는 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 셀 모양을 정의 하는 사용자 지정 형식에서 만들어집니다. 형식에서 사용자 지정 형식이 파생 되어야 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)다음 코드 예제에 나온 것 처럼:

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

내에서 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), 레이아웃 여기에서 관리 되는 [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)합니다. `Grid` 세 개 포함 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 해당 바인딩 인스턴스를 해당 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) 각각의 적절 한 속성을 속성 `Person` 컬렉션의 개체입니다.

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

C#에서 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 인수로 셀 유형을 지정 하는 생성자 오버 로드를 사용 하 여 만들어집니다. 셀 유형을 형식에서 파생 되어야 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)다음 코드 예제에 나온 것 처럼:

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
> Xamarin.Forms는에 간단한 데이터를 표시 하는 데 사용할 수 있는 셀 형식을 포함 하는 또한 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 셀입니다. 자세한 내용은 참조 [셀 모양](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)합니다.

<a name="resource" />

## <a name="creating-a-datatemplate-as-a-resource"></a>DataTemplate을 리소스로 만들기

다시 사용할 수 있는 개체로 데이터 템플릿을 만들 수도 있습니다는 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)합니다. 고유한 각 선언을 제공 하 여 이렇게 `x:Key` 에 설명이 포함 된 키가 있는 제공 하는 특성은 `ResourceDictionary`다음 XAML 코드 예제에 나온 것 처럼:

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

[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 에 할당 되는 [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) 사용 하 여 속성의 `StaticResource` 태그 확장 합니다. 하지만 `DataTemplate` 는 페이지에 정의 되어 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), 제어 수준 또는 응용 프로그램 수준도 정의할 수 있습니다.

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

[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 에 추가 되는 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 를 사용 하 여는 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) 지정 하는 메서드는 `Key` 에 대해 사용 되는 문자열 참조는 `DataTemplate` 가져올 때는 합니다.

## <a name="summary"></a>요약

이 문서 또는 사용자 지정 형식에서 인라인 데이터 서식 파일을 만드는 방법을 설명 했습니다는 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)합니다. 데이터 서식 파일을 다른 곳에서 다시 사용할 필요가 없는 경우 인라인 템플릿은 사용 해야 합니다. 또는 데이터 서식 파일은 컨트롤 수준 페이지 수준 또는 응용 프로그램 수준 리소스 또는 사용자 지정 형식으로 정의 하 여 재사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [셀 모양](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [데이터 템플릿 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [데이터 템플릿](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)
