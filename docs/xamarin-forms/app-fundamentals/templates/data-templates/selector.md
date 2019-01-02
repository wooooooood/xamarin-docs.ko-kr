---
title: Xamarin.Forms DataTemplateSelector 만들기
description: 이 문서에서는 데이터 바인딩된 속성 값을 기반으로, 런타임 시 DataTemplate을 선택하는 데 사용할 수 있는 DataTemplateSelector를 만들고 사용하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: A4629E8F-2BAF-45CE-A76E-DF225FE8D26C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 75ea30dd510021d7755814e19728bfe60f765645
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53057290"
---
# <a name="creating-a-xamarinforms-datatemplateselector"></a>Xamarin.Forms DataTemplateSelector 만들기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplateselector/)

_DataTemplateSelector는 데이터 바인딩된 속성의 값에 기반하여 런타임 시 DataTemplate을 선택하는 데 사용할 수 있습니다. 이렇게 하면 여러 DataTemplates 인스턴스를 같은 유형의 개체에 적용하여 특정 개체의 모양을 사용자 지정할 수 있습니다. 이 문서에서는 DataTemplateSelector을 만들고 사용하는 방법을 보여 줍니다._

데이터 템플릿 선택기를 통해 개체 컬렉션에 바인딩되는 [`ListView`](xref:Xamarin.Forms.ListView)와 같은 시나리오가 가능합니다. 여기서 `ListView`의 각 개체 모양은 특정 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)를 반환하는 데이터 템플릿 선택기로 런타임 시 선택할 수 있습니다.

## <a name="creating-a-datatemplateselector"></a>DataTemplateSelector 만들기

데이터 템플릿 선택기는 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)에서 상속되는 클래스를 만들어 구현합니다. 그러면 `OnSelectTemplate` 메서드는 다음 코드 예제처럼 특정 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)을 반환하도록 재정의됩니다.

```csharp
public class PersonDataTemplateSelector : DataTemplateSelector
{
  public DataTemplate ValidTemplate { get; set; }
  public DataTemplate InvalidTemplate { get; set; }

  protected override DataTemplate OnSelectTemplate (object item, BindableObject container)
  {
    return ((Person)item).DateOfBirth.Year >= 1980 ? ValidTemplate : InvalidTemplate;
  }
}
```

`OnSelectTemplate` 메서드는 `DateOfBirth` 속성 값에 따라 적절한 템플릿을 반환합니다. 반환할 템플릿은 `PersonDataTemplateSelector`를 사용할 때 설정되는 `ValidTemplate` 속성 또는 `InvalidTemplate` 속성 값입니다.

그런 다음, 데이터 템플릿 선택기 클래스의 인스턴스는 Xamarin.Forms 컨트롤 속성(예: [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1))에 할당할 수 있습니다. 유효한 속성 목록은 [DataTemplate 만들기](~/xamarin-forms/app-fundamentals/templates/data-templates/creating.md)를 참조하세요.

### <a name="limitations"></a>제한 사항

[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) 인스턴스에는 다음과 같은 제한 사항이 있습니다.

- `DataTemplateSelector` 서브클래스는 여러 번 쿼리해도 동일한 데이터에 대해서는 항상 동일한 템플릿을 반환해야 합니다.
- `DataTemplateSelector` 서브클래스는 다른 `DataTemplateSelector` 서브클래스를 반환해서는 안 됩니다.
- `DataTemplateSelector` 서브클래스는 각 호출에서 `DataTemplate`의 새 인스턴스를 반환해서는 안 됩니다. 대신, 동일한 인스턴스를 반환해야 합니다. 그렇게 하지 않으면 메모리 누수가 발생하고 가상화가 비활성화됩니다.
- Android의 경우 `ListView`당 서로 다른 데이터 템플릿이 20개를 넘을 수 없습니다.

## <a name="consuming-a-datatemplateselector-in-xaml"></a>XAML에서 DataTemplateSelector 사용

XAML에서 `PersonDataTemplateSelector`는 다음 코드 예제와 같이 리소스로 선언하여 인스턴스화할 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Selector;assembly=Selector" x:Class="Selector.HomePage">
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="validPersonTemplate">
                <ViewCell>
                   ...
                </ViewCell>
            </DataTemplate>
            <DataTemplate x:Key="invalidPersonTemplate">
                <ViewCell>
                   ...
                </ViewCell>
            </DataTemplate>
            <local:PersonDataTemplateSelector x:Key="personDataTemplateSelector"
                ValidTemplate="{StaticResource validPersonTemplate}"
                InvalidTemplate="{StaticResource invalidPersonTemplate}" />
        </ResourceDictionary>
    </ContentPage.Resources>
  ...
</ContentPage>
```

이 페이지 수준 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)는 두 개의 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 인스턴스와 한 개의 `PersonDataTemplateSelector` 인스턴스를 정의합니다. `PersonDataTemplateSelector` 인스턴스는 `StaticResource` 태그 확장을 사용하여 해당 `ValidTemplate` 및 `InvalidTemplate` 속성을 적절한 `DataTemplate` 인스턴스로 설정합니다. 리소스는 페이지의 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에 정의되지만 컨트롤 수준이나 애플리케이션 수준에서도 정의될 수 있습니다.

`PersonDataTemplateSelector` 인스턴스는 다음 코드 예제와 같이 [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) 속성에 할당하여 사용합니다.

```xaml
<ListView x:Name="listView" ItemTemplate="{StaticResource personDataTemplateSelector}" />
```

런타임 시 [`ListView`](xref:Xamarin.Forms.ListView)는 기본 컬렉션의 각 항목에 대해 `PersonDataTemplateSelector.OnSelectTemplate` 메서드를 호출하며, 호출 시 데이터 개체를 `item` 매개 변수로 전달합니다. 그런 다음, 메서드에서 반환된 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)이 해당 개체에 적용됩니다.

다음 스크린샷은 기본 컬렉션의 각 개체에 `PersonDataTemplateSelector`를 적용한 [`ListView`](xref:Xamarin.Forms.ListView)의 결과를 보여 줍니다.

![](selector-images/data-template-selector.png "데이터 템플릿 선택기를 사용하는 ListView")

`DateOfBirth` 속성 값이 1980보다 크거나 같은 `Person` 개체는 녹색으로 표시되고 나머지 개체는 빨간색으로 표시됩니다.

## <a name="consuming-a-datatemplateselector-in-cnum"></a>C&num;에서 DataTemplateSelector 사용

C#에서는 다음 코드 예제와 같이 `PersonDataTemplateSelector`를 인스턴스화하고 [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1) 속성에 할당할 수 있습니다.

```csharp
public class HomePageCS : ContentPage
{
  DataTemplate validTemplate;
  DataTemplate invalidTemplate;

  public HomePageCS ()
  {
    ...
    SetupDataTemplates ();
    var listView = new ListView {
      ItemsSource = people,
      ItemTemplate = new PersonDataTemplateSelector {
        ValidTemplate = validTemplate,
        InvalidTemplate = invalidTemplate }
    };

    Content = new StackLayout {
      Margin = new Thickness (20),
      Children = {
        ...
        listView
      }
    };
  }
  ...  
}
```

`PersonDataTemplateSelector` 인스턴스는 해당 `ValidTemplate` 및 `InvalidTemplate` 속성을 `SetupDataTemplates` 메서드로 만든 적절한 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 인스턴스로 설정합니다. 런타임 시 [`ListView`](xref:Xamarin.Forms.ListView)는 기본 컬렉션의 각 항목에 대해 `PersonDataTemplateSelector.OnSelectTemplate` 메서드를 호출하며, 호출 시 데이터 개체를 `item` 매개 변수로 전달합니다. 그런 다음, 메서드에서 반환된 `DataTemplate`이 해당 개체에 적용됩니다.

## <a name="summary"></a>요약

이 문서에서는 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)를 만들고 사용하는 방법을 보여줍니다. `DataTemplateSelector`는 데이터 바인딩된 속성의 값에 기반하여 런타임 시 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)을 선택하는 데 사용됩니다. 이렇게 하면 여러 `DataTemplate` 인스턴스를 같은 형식의 개체에 적용하여 특정 개체의 모양을 사용자 지정할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [데이터 템플릿 선택기(샘플)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplateselector/)
- [DataTemplateSelector](xref:Xamarin.Forms.DataTemplateSelector)
