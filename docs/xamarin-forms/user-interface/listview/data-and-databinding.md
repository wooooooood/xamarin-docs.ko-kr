---
title: ListView 데이터 원본
description: 이 문서에는 데이터를 사용 하 여 Xamarin.Forms ListView를 채우는 방법 및는 ListView를 사용 하 여 데이터 바인딩을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2020
ms.openlocfilehash: e51f0bd011750b030c0a11b9b89a2c2473f2a9ed
ms.sourcegitcommit: d83c6af42ed26947aa7c0ecfce00b9ef60f33319
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80247589"
---
# <a name="listview-data-sources"></a>ListView 데이터 원본

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)

Xamarin.ios [`ListView`](xref:Xamarin.Forms.ListView) 데이터 목록을 표시 하는 데 사용 됩니다. 이 문서에서는 데이터를 사용 하 여 `ListView`를 채우는 방법과 데이터를 선택한 항목에 바인딩하는 방법에 대해 설명 합니다.

## <a name="itemssource"></a>ItemsSource

[`ListView`](xref:Xamarin.Forms.ListView) 는 `IEnumerable`를 구현 하는 모든 컬렉션을 받아들일 수 있는 [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) 속성을 사용 하 여 데이터로 채워집니다. `ListView`를 채우는 가장 간단한 방법은 문자열의 배열을 사용 하는 것입니다.

```xaml
<ListView>
      <ListView.ItemsSource>
          <x:Array Type="{x:Type x:String}">
            <x:String>mono</x:String>
            <x:String>monodroid</x:String>
            <x:String>monotouch</x:String>
            <x:String>monorail</x:String>
            <x:String>monodevelop</x:String>
            <x:String>monotone</x:String>
            <x:String>monopoly</x:String>
            <x:String>monomodal</x:String>
            <x:String>mononucleosis</x:String>
          </x:Array>
      </ListView.ItemsSource>
</ListView>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
var listView = new ListView();
listView.ItemsSource = new string[]
{
  "mono",
  "monodroid",
  "monotouch",
  "monorail",
  "monodevelop",
  "monotone",
  "monopoly",
  "monomodal",
  "mononucleosis"
};
```

![](data-and-databinding-images/itemssource-simple.png "ListView Displaying List of Strings")

이 방법을 사용 하면 `ListView` 문자열 목록으로 채워집니다. 기본적으로 `ListView`는 `ToString`를 호출 하 고 각 행에 대 한 `TextCell` 결과를 표시 합니다. 데이터가 표시 되는 방식을 사용자 지정 하려면 [셀 모양](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)을 참조 하세요.

`ItemsSource` 배열로 전송 되었기 때문에 기본 목록 또는 배열이 변경 될 때 콘텐츠가 업데이트 되지 않습니다. 기본 목록에서 항목이 추가, 제거 및 변경 될 때 ListView가 자동으로 업데이트 되도록 하려면 `ObservableCollection`를 사용 해야 합니다. [`ObservableCollection`](xref:System.Collections.ObjectModel.ObservableCollection`1) 은 `System.Collections.ObjectModel`에 정의 되어 있으며 변경 내용을 `ListView` 알릴 수 있다는 점을 제외 하 고 `List`와 동일 합니다.

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
listView.ItemsSource = employees;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employees.Add(new Employee(){ DisplayName="Mr. Mono"});
```

## <a name="data-binding"></a>데이터 바인딩

데이터 바인딩은 사용자 인터페이스 개체의 속성을 viewmodel의 클래스와 같은 일부 CLR 개체의 속성에 바인딩하는 "glue"입니다. 데이터 바인딩 많은 지루한 상용구 코드를 대체 하 여 사용자 인터페이스 개발을 단순화할 수 있으므로 유용 합니다.

데이터 바인딩은 해당 바인딩된 값이 변경 될 개체를 동기화 상태로 유지 하 여 작동 합니다. 컨트롤의 값이 변경 될 때마다에 대 한 이벤트 처리기를 작성 하는 대신, viewmodel에서 바인딩을 설정 하 고 바인딩을 사용 하도록 설정 합니다.

데이터 바인딩에 대 한 자세한 내용은 [XAMARIN.IOS XAML 기본 사항 문서 시리즈](~/xamarin-forms/xaml/xaml-basics/index.md)의 4 부로 구성 된 [데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) 을 참조 하세요.

### <a name="binding-cells"></a>셀 바인딩

셀의 속성 (및 셀의 자식)은 `ItemsSource`개체의 속성에 바인딩할 수 있습니다. 예를 들어 `ListView`는 직원 목록을 표시 하는 데 사용할 수 있습니다.

Employee 클래스:

```csharp
public class Employee
{
    public string DisplayName {get; set;}
}
```

`ObservableCollection<Employee>` 생성 되 고 `ListView` `ItemsSource`설정 되며 목록이 데이터로 채워집니다.

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
public ObservableCollection<Employee> Employees { get { return employees; }}

public EmployeeListPage()
{
    EmployeeView.ItemsSource = employees;

    // ObservableCollection allows items to be added after ItemsSource
    // is set and the UI will react to changes
    employees.Add(new Employee{ DisplayName="Rob Finnerty"});
    employees.Add(new Employee{ DisplayName="Bill Wrestler"});
    employees.Add(new Employee{ DisplayName="Dr. Geri-Beth Hooper"});
    employees.Add(new Employee{ DisplayName="Dr. Keith Joyce-Purdy"});
    employees.Add(new Employee{ DisplayName="Sheri Spruce"});
    employees.Add(new Employee{ DisplayName="Burt Indybrick"});
}
```

> [!WARNING]
> `ListView` 기본 `ObservableCollection`의 변경 내용에 대 한 응답으로 업데이트 되지만 다른 `ObservableCollection` 인스턴스가 원래 `ObservableCollection` 참조 (예: `employees = otherObservableCollection;`)에 할당 된 경우에는 `ListView` 업데이트 되지 않습니다.

다음 코드 조각은 직원 목록에 바인딩된 `ListView`를 보여 줍니다.

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
             x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
             Title="Employee List">
  <ListView x:Name="EmployeeView"
            ItemsSource="{Binding Employees}">
    <ListView.ItemTemplate>
      <DataTemplate>
        <TextCell Text="{Binding DisplayName}" />
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

이 XAML 예제는 `ListView`를 포함 하는 `ContentPage`를 정의 합니다. `ListView`의 데이터 원본은 `ItemsSource` 특성을 통해 설정 됩니다. `ItemsSource`에 있는 각 행의 레이아웃은 `ListView.ItemTemplate` 요소 내에 정의 됩니다. 그러면 다음과 같은 스크린샷을 생성 합니다.

![](data-and-databinding-images/bound-data.png "ListView using Data Binding")

> [!WARNING]
> `ObservableCollection`은 스레드로부터 안전 하지 않습니다. `ObservableCollection` 수정 하면 UI 업데이트가 수정 작업을 수행한 동일한 스레드에서 수행 됩니다. 스레드가 기본 UI 스레드가 아닌 경우 예외를 발생 시킵니다.

### <a name="binding-selecteditem"></a>SelectedItem 바인딩

이벤트 처리기를 사용 하 여 변경 내용에 응답 하지 않고 `ListView`의 선택 된 항목에 바인딩하려는 경우가 많습니다. XAML에서이 작업을 수행 하려면 `SelectedItem` 속성을 바인딩합니다.

```xaml
<ListView x:Name="listView"
          SelectedItem="{Binding Source={x:Reference SomeLabel},
          Path=Text}">
 …
</ListView>
```

`listView`의 `ItemsSource` 문자열 목록 이라고 가정 하면 `SomeLabel` `Text` 속성을 `SelectedItem`에 바인딩합니다.

## <a name="related-links"></a>관련 링크

- [양방향 바인딩 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
