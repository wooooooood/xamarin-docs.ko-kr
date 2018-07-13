---
title: Xamarin.Forms DataTemplateSelector 만들기
description: 이 문서를 만들고 데이터 바인딩된 속성의 값을 기반으로 런타임에 DataTemplate 선택에 사용할 수 있는 DataTemplateSelector를 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: A4629E8F-2BAF-45CE-A76E-DF225FE8D26C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: a72777c7e51e96a8e123ecd85ad0aa24fc60fc6c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994519"
---
# <a name="creating-a-xamarinforms-datatemplateselector"></a>Xamarin.Forms DataTemplateSelector 만들기

_데이터 바인딩된 속성의 값을 기반으로 런타임에 DataTemplate을 선택 하는 DataTemplateSelector는 사용할 수 있습니다. 이 통해 여러 Datatemplate를 동일한 유형의 특정 개체의 모양을 사용자 지정 개체에 적용할 수 있습니다. 이 문서를 만들고를 DataTemplateSelector 사용 방법을 보여 줍니다._

데이터 템플릿 선택기와 같은 시나리오를 지원 하는 [ `ListView` ](xref:Xamarin.Forms.ListView) 개체의 컬렉션에 바인딩 위치에 있는 각 개체의 모양을 `ListView` 데이터 템플릿 선택기를 반환 하 여 런타임 시 선택할 수 있습니다를 특정 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)합니다.

## <a name="creating-a-datatemplateselector"></a>DataTemplateSelector 만들기

상속 되는 클래스를 만들어 데이터 템플릿 선택기를 구현 [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector)합니다. 합니다 `OnSelectTemplate` 특정 반환할 메서드를 재정의 한 다음 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)다음 코드 예제 에서처럼:

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

합니다 `OnSelectTemplate` 반환 값에 따라 적절 한 템플릿을 `DateOfBirth` 속성입니다. 반환할 템플릿의 값은는 `ValidTemplate` 속성 또는 `InvalidTemplate` 속성을 사용 하는 경우 설정 됩니다는 `PersonDataTemplateSelector`합니다.

데이터 템플릿 선택기 클래스의 인스턴스 수 다음 속성에 할당 되 Xamarin.Forms 컨트롤 같은 [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1)합니다. 유효한 속성 목록에 대해서 [DataTemplate 만들기](~/xamarin-forms/app-fundamentals/templates/data-templates/creating.md)합니다.

### <a name="limitations"></a>제한 사항

[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) 인스턴스는 다음 제한 사항이 있습니다.

- `DataTemplateSelector` 여러 번 쿼리할 경우 서브 클래스에서 동일한 데이터에 대해 동일한 템플릿을 항상 반환 해야 합니다.
- 합니다 `DataTemplateSelector` 서브 클래스 다른 반환 하지 않아야 `DataTemplateSelector` 하위 클래스입니다.
- 합니다 `DataTemplateSelector` 서브 클래스의 새 인스턴스를 반환 하지 않아야는 `DataTemplate` 각 호출에서. 대신, 동일한 인스턴스를 반환 합니다. 이렇게 하지 않으면 메모리 누수를 만들고 가상화 사용 하지 않도록 설정 됩니다.
- Android에서 있을 수 있습니다 당 20 개 이하의 서로 다른 데이터 템플릿을 `ListView`합니다.

## <a name="consuming-a-datatemplateselector-in-xaml"></a>XAML에서 DataTemplateSelector 사용

XAML에서 `PersonDataTemplateSelector` 다음 코드 예제에 표시 된 대로 리소스로 선언 하 여 인스턴스화할 수 있습니다.

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

이 페이지 수준 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 두 정의 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 인스턴스 및 `PersonDataTemplateSelector` 인스턴스. `PersonDataTemplateSelector` 집합 인스턴스 해당 `ValidTemplate` 및 `InvalidTemplate` 속성을 적절 한 `DataTemplate` 사용 하 여 인스턴스를 `StaticResource` 태그 확장 합니다. 페이지에 정의 된 리소스 중 상태인 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), 제어 수준 또는 응용 프로그램 수준에서 정의할 수도 있습니다.

합니다 `PersonDataTemplateSelector` 에 할당 되는 인스턴스를 [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) 속성을 다음 코드 예제 에서처럼:

```xaml
<ListView x:Name="listView" ItemTemplate="{StaticResource personDataTemplateSelector}" />
```

런타임에 [ `ListView` ](xref:Xamarin.Forms.ListView) 호출을 `PersonDataTemplateSelector.OnSelectTemplate` 의 각 데이터 개체를 전달 하는 호출을 사용 하 여 기본 컬렉션에서 항목에 대 한 메서드는 `item` 매개 변수. 합니다 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 에서 반환 하는 메서드를 해당 개체에 적용 됩니다.

다음 스크린샷은 결과를 표시 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 적용 된 `PersonDataTemplateSelector` 기본 컬렉션의 각 개체에:

![](selector-images/data-template-selector.png "데이터 템플릿 선택기를 사용 하 여 ListView")

모든 `Person` 가 있는 개체를 `DateOfBirth` 빨간색으로 표시 되 고 남은 개체를 사용 하 여 속성 값 보다 크거나 같음 1980 녹색으로 표시 됩니다.

## <a name="consuming-a-datatemplateselector-in-cnum"></a>C에서 DataTemplateSelector 사용&num;

C#에서는 합니다 `PersonDataTemplateSelector` 인스턴스화되고 할당할 수는 [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1) 속성을 다음 코드 예제에 표시 된 대로:

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

`PersonDataTemplateSelector` 집합 인스턴스 해당 `ValidTemplate` 하 고 `InvalidTemplate` 속성을 적절 한 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 만든 인스턴스에 `SetupDataTemplates` 메서드. 런타임에 [ `ListView` ](xref:Xamarin.Forms.ListView) 호출을 `PersonDataTemplateSelector.OnSelectTemplate` 의 각 데이터 개체를 전달 하는 호출을 사용 하 여 기본 컬렉션에서 항목에 대 한 메서드는 `item` 매개 변수. `DataTemplate` 에서 반환 하는 메서드를 해당 개체에 적용 됩니다.

## <a name="summary"></a>요약

이 문서를 만들고 사용 하는 방법에 명시 되어는 [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector)합니다. A `DataTemplateSelector` 선택에 사용할 수는 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 데이터 바인딩된 속성의 값을 기반으로 하는 런타임 시. 이렇게 하면 여러 `DataTemplate` 인스턴스가 동일한 유형의 개체를 특정 개체의 모양을 사용자 지정을 적용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [데이터 템플릿 선택기 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplateselector/)
- [DataTemplateSelector](xref:Xamarin.Forms.DataTemplateSelector)
