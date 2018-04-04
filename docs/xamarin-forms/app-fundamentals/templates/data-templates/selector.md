---
title: DataTemplateSelector 만들기
description: 데이터 바인딩된 속성의 값에 따라 런타임에 DataTemplate을 선택 하는 DataTemplateSelector는 사용할 수 있습니다. 이렇게 하면 여러 DataTemplates 동일한 유형의 특정 개체의 모양을 사용자 지정할 개체에 적용 될 수 있습니다. 이 문서를 만들고는 DataTemplateSelector를 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: A4629E8F-2BAF-45CE-A76E-DF225FE8D26C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: d87b9f3cf47e3b3efa42ad53cfba91bac1d07d4f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-datatemplateselector"></a>DataTemplateSelector 만들기

_데이터 바인딩된 속성의 값에 따라 런타임에 DataTemplate을 선택 하는 DataTemplateSelector는 사용할 수 있습니다. 이렇게 하면 여러 DataTemplates 동일한 유형의 특정 개체의 모양을 사용자 지정할 개체에 적용 될 수 있습니다. 이 문서를 만들고는 DataTemplateSelector를 사용 하는 방법을 보여 줍니다._

데이터 템플릿 선택기와 같은 시나리오를 지원 하는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 개체의 컬렉션에 바인딩할 위치에 있는 각 개체의 모양을 `ListView` 데이터 템플릿 선택기를 반환 하 여 런타임 시 선택할 수는 특정 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)합니다.

## <a name="creating-a-datatemplateselector"></a>DataTemplateSelector 만들기

데이터 템플릿 선택기에서 상속 되는 클래스를 만들어 구현 [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/)합니다. `OnSelectTemplate` 다음 메서드는 특정 반환할 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)다음 코드 예제에 나온 것 처럼:

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

`OnSelectTemplate` 메서드 반환 값에 따라 적절 한 템플릿을 `DateOfBirth` 속성입니다. 반환할 서식 파일의 값입니다는 `ValidTemplate` 속성 또는 `InvalidTemplate` 사용 될 때 설정 되는 속성은 `PersonDataTemplateSelector`합니다.

데이터 템플릿 선택기 클래스의 인스턴스 그런 다음 할당할 수 있습니다 Xamarin.Forms 컨트롤 속성을 같은 [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/)합니다. 유효한 속성 목록에 대 한 참조 [DataTemplate 만드는](~/xamarin-forms/app-fundamentals/templates/data-templates/creating.md)합니다.

### <a name="limitations"></a>제한 사항

[`DataTemplateSelector`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) 인스턴스는 다음과 같은 제한 사항이 있습니다.

- `DataTemplateSelector` 여러 번 쿼리할 경우 하위 클래스 동일한 데이터에 대해 같은 서식 파일을 항상 반환 해야 합니다.
- `DataTemplateSelector` 하위 클래스 다른을 반환 하지 않아야 `DataTemplateSelector` 하위 클래스입니다.
- `DataTemplateSelector` 하위 클래스의 새 인스턴스를 반환 하지 않아야는 `DataTemplate` 각 호출 합니다. 대신, 동일한 인스턴스를 반환 합니다. 이렇게 하지 않으면 메모리 누수 만들고 가상화 사용할 수 없게 됩니다.
- Android에서는 있을 수 당 20 개 이하의 서로 다른 데이터 템플릿을 `ListView`합니다.

## <a name="consuming-a-datatemplateselector-in-xaml"></a>XAML의 DataTemplateSelector 사용

Xaml에서는 `PersonDataTemplateSelector` 다음 코드 예제에 나와 있는 것 처럼 리소스로 바운딩되어야 선언 하 여 인스턴스화할 수 있습니다.

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

이 페이지 수준 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 두 정의 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 인스턴스 및 `PersonDataTemplateSelector` 인스턴스. `PersonDataTemplateSelector` 집합 인스턴스는 `ValidTemplate` 및 `InvalidTemplate` 속성을 적절 한 `DataTemplate` 를 사용 하 여 인스턴스는 `StaticResource` 태그 확장 합니다. 페이지에 정의 된 리소스 하는 동안는 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), 제어 수준 또는 응용 프로그램 수준에서 정의할 수도 있습니다.

`PersonDataTemplateSelector` 인스턴스를 할당 하 여 사용 되는 [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) 속성을 다음 코드 예제에 나와 있는 것 처럼:

```xaml
<ListView x:Name="listView" ItemTemplate="{StaticResource personDataTemplateSelector}" />
```

런타임에 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 호출은 `PersonDataTemplateSelector.OnSelectTemplate` 는 데이터 개체를 전달 하는 호출을 사용 하 여 내부 컬렉션에 있는 항목의 각 메서드는 `item` 매개 변수입니다. [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 에서 반환 하는 메서드를 해당 개체에 적용 합니다.

결과 표시 하는 다음 스크린샷에서 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 적용 된 `PersonDataTemplateSelector` 기본 컬렉션의 각 개체에:

![](selector-images/data-template-selector.png "데이터 템플릿 선택기 인 ListView")

모든 `Person` 있는 개체는 `DateOfBirth` 1980 보다 크거나 속성 값이 빨간색으로 표시 되 고 나머지 개체와 녹색으로 표시 됩니다.

## <a name="consuming-a-datatemplateselector-in-cnum"></a>C에서 DataTemplateSelector 사용&num;

C#에서 `PersonDataTemplateSelector` 인스턴스화할 수 있고에 할당 된는 [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) 속성을 다음 코드 예제에 나와 있는 것 처럼:

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

`PersonDataTemplateSelector` 집합 인스턴스는 `ValidTemplate` 및 `InvalidTemplate` 속성을 적절 한 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 에서 만든 인스턴스는 `SetupDataTemplates` 메서드. 런타임에 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 호출은 `PersonDataTemplateSelector.OnSelectTemplate` 는 데이터 개체를 전달 하는 호출을 사용 하 여 내부 컬렉션에 있는 항목의 각 메서드는 `item` 매개 변수입니다. `DataTemplate` 에서 반환 하는 메서드를 해당 개체에 적용 합니다.

## <a name="summary"></a>요약

이 문서를 만들고 사용 하는 방법을 제시 합니다.는 [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/)합니다. A `DataTemplateSelector` 선택에 사용할 수는 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 데이터 바인딩된 속성의 값에 따라 런타임에 합니다. 이렇게 하면 여러 `DataTemplate` 인스턴스는 동일한 유형의 특정 개체의 모양을 사용자 지정할 개체에 적용 될 수 있습니다.


## <a name="related-links"></a>관련 링크

- [데이터 서식 파일 선택 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplateselector/)
- [DataTemplateSelector](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/)
