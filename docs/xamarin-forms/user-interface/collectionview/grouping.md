---
title: Xamarin.ios CollectionView 그룹화
description: CollectionView는 IsGrouped 속성을 true로 설정 하 여 올바르게 그룹화 된 데이터를 표시할 수 있습니다.
ms.prod: xamarin
ms.assetid: 7E494245-FDBD-49D6-B7FA-CEF976EB59BB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/17/2019
ms.openlocfilehash: 8360123b01f36bde084b4dc315109e6bdaef2207
ms.sourcegitcommit: 99aa05bd9b5e3f66d134066b860f41b54fa2d850
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82103284"
---
# <a name="xamarinforms-collectionview-grouping"></a>Xamarin.ios CollectionView 그룹화

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

자주 발생 하는 데이터 집합은 계속 해 서 스크롤 목록에 표시 될 때 어려울 수 있습니다. 이 시나리오에서 데이터를 그룹으로 구성 하면 데이터를 쉽게 탐색할 수 있으므로 사용자 환경을 개선할 수 있습니다.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)은 그룹화 된 데이터 표시를 지원 하 고 표시 되는 방법을 제어 하는 다음 속성을 정의 합니다.

- `IsGrouped`형식의 `bool`는 기본 데이터가 그룹에 표시 되어야 하는지 여부를 나타냅니다. 이 속성의 기본값은 `false`입니다.
- `GroupHeaderTemplate`각 그룹의 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)헤더에 사용할 템플릿인 형식의입니다.
- `GroupFooterTemplate`형식의 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), 각 그룹의 바닥글에 사용할 템플릿입니다.

이러한 속성은 개체에 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 의해 지원 됩니다. 즉, 속성은 데이터 바인딩의 대상이 될 수 있습니다.

다음 스크린샷은 그룹화 된 데이터 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 를 표시 합니다.

[![IOS 및 Android에서 CollectionView의 그룹화 된 데이터 스크린샷](grouping-images/grouped-data.png "그룹화 된 데이터로 CollectionView")](grouping-images/grouped-data-large.png#lightbox "그룹화 된 데이터로 CollectionView")

데이터 템플릿에 대한 자세한 내용은 [Xamarin.Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)을 참조하세요.

## <a name="group-data"></a>데이터 그룹화

데이터를 표시 하려면 먼저 그룹화 해야 합니다. 이를 위해서는 각 그룹이 항목 목록 인 그룹 목록을 만들어 수행할 수 있습니다. 그룹 목록은 다음과 같은 두 가지 데이터 `IEnumerable<T>` 를 `T` 정의 하는 컬렉션 이어야 합니다.

- 그룹 이름입니다.
- 그룹 `IEnumerable` 에 속한 항목을 정의 하는 컬렉션입니다.

따라서 데이터를 그룹화 하는 프로세스는 다음과 같습니다.

- 단일 항목을 모델링 하는 형식을 만듭니다.
- 단일 항목 그룹을 모델링 하는 형식을 만듭니다.
- `IEnumerable<T>` 컬렉션을 만듭니다. 여기서 `T` 은 단일 항목 그룹을 모델링 하는 형식입니다. 따라서이 컬렉션은 그룹화 된 데이터를 저장 하는 그룹의 컬렉션입니다.
- `IEnumerable<T>` 컬렉션에 데이터를 추가 합니다.

### <a name="example"></a>예제

데이터를 그룹화 할 때 첫 번째 단계는 단일 항목을 모델링 하는 형식을 만드는 것입니다. 다음 예제에서는 예제 응용 `Animal` 프로그램의 클래스를 보여 줍니다.

```csharp
public class Animal
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

클래스 `Animal` 는 단일 항목을 모델링 합니다. 그러면 항목 그룹을 모델링 하는 형식을 만들 수 있습니다. 다음 예제에서는 예제 응용 `AnimalGroup` 프로그램의 클래스를 보여 줍니다.

```csharp
public class AnimalGroup : List<Animal>
{
    public string Name { get; private set; }

    public AnimalGroup(string name, List<Animal> animals) : base(animals)
    {
        Name = name;
    }
}
```

클래스 `AnimalGroup` 는 `List<T>` 클래스에서 상속 되 고 그룹 이름을 `Name` 나타내는 속성을 추가 합니다.

그런 `IEnumerable<T>` 다음 그룹의 컬렉션을 만들 수 있습니다.

```csharp
public List<AnimalGroup> Animals { get; private set; } = new List<AnimalGroup>();
```

이 코드는 컬렉션의 각 `Animals`항목이 `AnimalGroup` 개체인 이라는 컬렉션을 정의 합니다. 각 `AnimalGroup` 개체는 이름 및 그룹의 개체 `List<Animal>` 를 `Animal` 정의 하는 컬렉션으로 구성 됩니다.

그런 다음 그룹화 된 `Animals` 데이터를 컬렉션에 추가할 수 있습니다.

```csharp
Animals.Add(new AnimalGroup("Bears", new List<Animal>
{
    new Animal
    {
        Name = "American Black Bear",
        Location = "North America",
        Details = "Details about the bear go here.",
        ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/0/08/01_Schwarzbär.jpg"
    },
    new Animal
    {
        Name = "Asian Black Bear",
        Location = "Asia",
        Details = "Details about the bear go here.",
        ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/b/b7/Ursus_thibetanus_3_%28Wroclaw_zoo%29.JPG/180px-Ursus_thibetanus_3_%28Wroclaw_zoo%29.JPG"
    },
    // ...
}));

Animals.Add(new AnimalGroup("Monkeys", new List<Animal>
{
    new Animal
    {
        Name = "Baboon",
        Location = "Africa & Asia",
        Details = "Details about the monkey go here.",
        ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
    },
    new Animal
    {
        Name = "Capuchin Monkey",
        Location = "Central & South America",
        Details = "Details about the monkey go here.",
        ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/4/40/Capuchin_Costa_Rica.jpg/200px-Capuchin_Costa_Rica.jpg"
    },
    new Animal
    {
        Name = "Blue Monkey",
        Location = "Central and East Africa",
        Details = "Details about the monkey go here.",
        ImageUrl = "https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/BlueMonkey.jpg/220px-BlueMonkey.jpg"
    },
    // ...
}));
```

이 코드는 `Animals` 컬렉션에 두 개의 그룹을 만듭니다. 첫 번째 `AnimalGroup` 에는 `Bears`이름이로 지정 되 `List<Animal>` 고는 세부 정보 컬렉션을 포함 합니다. 두 번째 `AnimalGroup` 는 이름이 `Monkeys`로 지정 되 고 `List<Animal>` ,는 원숭이 정보 컬렉션을 포함 합니다.

## <a name="display-grouped-data"></a>그룹화 된 데이터 표시

[`CollectionView`](xref:Xamarin.Forms.CollectionView)에서는 `IsGrouped` 속성을로 `true`설정 하 여 데이터를 올바르게 그룹화 한 경우에 그룹화 된 데이터를 표시 합니다.

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                IsGrouped="true">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                ...
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    IsGrouped = true
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
// ...
```

에서 각 항목의 모양은 [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) 속성을으로 설정 하 여 정의 됩니다. [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 자세한 내용은 [항목 모양 정의](~/xamarin-forms/user-interface/collectionview/populate-data.md#define-item-appearance)를 참조 하세요.

> [!NOTE]
> 기본적으로 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 는 그룹 머리글 및 바닥글에 그룹 이름을 표시 합니다. 그룹 머리글 및 그룹 바닥글을 사용자 지정 하 여이 동작을 변경할 수 있습니다.

## <a name="customize-the-group-header"></a>그룹 헤더 사용자 지정

`CollectionView.GroupHeaderTemplate` 속성을로 설정 하 여 각 그룹 머리글의 모양을 사용자 지정할 수 있습니다 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate).

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                IsGrouped="true">
    ...
    <CollectionView.GroupHeaderTemplate>
        <DataTemplate>
            <Label Text="{Binding Name}"
                   BackgroundColor="LightGray"
                   FontSize="Large"
                   FontAttributes="Bold" />
        </DataTemplate>
    </CollectionView.GroupHeaderTemplate>
</CollectionView>
```

이 예제에서 각 그룹 헤더는 그룹 이름을 표시 하 [`Label`](xref:Xamarin.Forms.Label) 고 다른 모양 속성을 설정 하는로 설정 됩니다. 다음 스크린샷은 사용자 지정 된 그룹 헤더를 보여 줍니다.

[![IOS 및 Android에서 CollectionView의 사용자 지정 된 그룹 헤더 스크린샷](grouping-images/customized-header.png "사용자 지정 된 그룹 헤더가 있는 CollectionView")](grouping-images/customized-header-large.png#lightbox "사용자 지정 된 그룹 헤더가 있는 CollectionView")

## <a name="customize-the-group-footer"></a>그룹 바닥글 사용자 지정

`CollectionView.GroupFooterTemplate` 속성을로 설정 하 여 각 그룹 바닥글의 모양을 사용자 지정할 수 있습니다 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate).

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                IsGrouped="true">
    ...
    <CollectionView.GroupFooterTemplate>
        <DataTemplate>
            <Label Text="{Binding Count, StringFormat='Total animals: {0:D}'}"
                   Margin="0,0,0,10" />
        </DataTemplate>
    </CollectionView.GroupFooterTemplate>
</CollectionView>
```

이 예제에서 각 그룹 바닥글은 그룹의 항목 수 [`Label`](xref:Xamarin.Forms.Label) 를 표시 하는로 설정 됩니다. 다음 스크린샷은 사용자 지정 된 그룹 바닥글을 보여 줍니다.

[![IOS 및 Android에서 CollectionView의 사용자 지정 된 그룹 바닥글 스크린샷](grouping-images/customized-footer.png "사용자 지정 된 그룹 바닥글이 있는 CollectionView")](grouping-images/customized-footer-large.png#lightbox "사용자 지정 된 그룹 바닥글이 있는 CollectionView")

## <a name="empty-groups"></a>빈 그룹

에서 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 그룹화 된 데이터를 표시 하면 비어 있는 모든 그룹이 표시 됩니다. 이러한 그룹은 그룹이 비어 있음을 나타내는 그룹 머리글 및 바닥글을 사용 하 여 표시 됩니다. 다음 스크린샷에는 빈 그룹이 표시 됩니다.

[![IOS 및 Android에서 CollectionView의 빈 그룹에 대 한 스크린샷](grouping-images/empty-group.png "빈 그룹이 있는 CollectionView")](grouping-images/empty-group-large.png#lightbox "빈 그룹이 있는 CollectionView")

> [!NOTE]
> IOS 10 이하의 경우 빈 그룹에 대 한 그룹 머리글 및 바닥글이 모두의 맨 위에 표시 될 수 있습니다 `CollectionView`.

## <a name="group-without-templates"></a>템플릿이 없는 그룹

[`CollectionView`](xref:Xamarin.Forms.CollectionView)[`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) 속성을로 설정 하지 않고 올바르게 그룹화 된 데이터를 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)표시할 수 있습니다.

```xaml
<CollectionView ItemsSource="{Binding Animals}"
                IsGrouped="true" />
```

이 시나리오에서는 단일 항목을 모델링 하는 형식의 `ToString` 메서드를 재정의 하 고 단일 항목 그룹을 모델링 하는 형식을 재정의 하 여 의미 있는 데이터를 표시할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [CollectionView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin.Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
