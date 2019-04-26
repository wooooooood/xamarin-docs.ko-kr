---
title: ListView 데이터 원본
description: 이 문서에는 데이터를 사용 하 여 Xamarin.Forms ListView를 채우는 방법 및는 ListView를 사용 하 여 데이터 바인딩을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/30/2018
ms.openlocfilehash: e53f6dce47dd7db60267d21c8d816ece554dc46c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61320014"
---
# <a name="listview-data-sources"></a>ListView 데이터 원본

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)

A [ `ListView` ](xref:Xamarin.Forms.ListView) 데이터의 목록을 표시 하기 위해 사용 됩니다. 데이터 및 선택한 항목에 바인딩할 수 있습니다 하는 방법을 사용 하 여 ListView를 채우는 방법에 대 한 알아보겠습니다.

## <a name="itemssource"></a>ItemsSource

A [ `ListView` ](xref:Xamarin.Forms.ListView) 데이터를 사용 하 여 채워집니다 합니다 [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) 속성을 구현 하는 모든 컬렉션을 허용할 수 있는 `IEnumerable`합니다. 가장 간단한 방법은 채우기는 `ListView` 문자열 배열을 사용 하 여:

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

해당 하는 C# 코드가입니다.

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

//monochrome will not appear in the list because it was added
//after the list was populated.
listView.ItemsSource.Add("monochrome");
```

![](data-and-databinding-images/itemssource-simple.png "ListView 문자열의 목록 표시")

위의 접근 방식을 채워집니다는 `ListView` 문자열 목록으로 합니다. 기본적으로 `ListView` 를 호출 합니다 `ToString` 결과를 표시 하 고는 `TextCell` 각 행에 대 한 합니다. 참조 데이터 표시 방법을 사용자 지정할 [셀 모양](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)합니다.

때문에 `ItemsSource` 보냈습니다를 배열에 기본 목록 또는 배열로 변경 되 면 콘텐츠 업데이트 되지 것입니다. 사용 해야 항목 추가, 제거 및 내부 목록에서 변경할 때 자동으로 업데이트 하는 ListView를 하려는 경우는 `ObservableCollection`합니다. [`ObservableCollection`](xref:System.Collections.ObjectModel.ObservableCollection`1) 에 정의 된 `System.Collections.ObjectModel` 마찬가지로 이며 `List`단 알릴 수 있습니다, `ListView` 변경:

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
listView.ItemsSource = employees;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employees.Add(new Employee(){ DisplayName="Mr. Mono"});
```

<a name="Data_Binding" />

## <a name="data-binding"></a>데이터 바인딩
데이터 바인딩은 프로그램 ViewModel 클래스와 같은 일부 CLR 개체의 속성에는 사용자 인터페이스 개체의 속성 "고리로"입니다. 데이터 바인딩 많은 지루한 상용구 코드를 대체 하 여 사용자 인터페이스 개발을 단순화할 수 있으므로 유용 합니다.

데이터 바인딩은 해당 바인딩된 값이 변경 될 개체를 동기화 상태로 유지 하 여 작동 합니다. 컨트롤의 값이 변경 될 때마다에 대 한 이벤트 처리기를 작성 하는 대신 바인딩을 설정 하 고 ViewModel에 바인딩을 사용 하도록 설정 합니다.

데이터 바인딩에 대 한 자세한 내용은 참조 하세요. [데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) 일부인 중 4 개는 [Xamarin.Forms XAML 기본 사항 연재 기사](~/xamarin-forms/xaml/xaml-basics/index.md)합니다.

### <a name="binding-cells"></a>셀 바인딩
셀 (및 셀의 자식) 속성에서 개체의 속성에 바인딩할 수 있습니다는 `ItemsSource`합니다. 예를 들어 직원의 목록을 표시 하는 ListView는 사용할 수 있습니다.

Employee 클래스:

```csharp
public class Employee
{
    public string DisplayName {get; set;}
}
```

`ObservableCollection<Employee>` 생성 되어로 설정 합니다 `ListView`의 `ItemsSource`:

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
public EmployeeListPage()
{
  //defined in XAML to follow
  EmployeeView.ItemsSource = employees;
  ...
}
```

이 목록은 데이터로 채워집니다.

```csharp
public EmployeeListPage()
{
  ...
  employees.Add(new Employee{ DisplayName="Rob Finnerty"});
  employees.Add(new Employee{ DisplayName="Bill Wrestler"});
  employees.Add(new Employee{ DisplayName="Dr. Geri-Beth Hooper"});
  employees.Add(new Employee{ DisplayName="Dr. Keith Joyce-Purdy"});
  employees.Add(new Employee{ DisplayName="Sheri Spruce"});
  employees.Add(new Employee{ DisplayName="Burt Indybrick"});
}
```

다음 조각은 `ListView` 직원의 목록에 바인딩된:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
Title="Employee List">
  <ListView x:Name="EmployeeView">
    <ListView.ItemTemplate>
      <DataTemplate>
        <TextCell Text="{Binding DisplayName}" />
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

XAML에서 연결 된 있기 수 있지만 바인딩을 않았거나 간단히 하기 위해 코드에서 설치를 참고 합니다.

XAML의 이전 비트 정의 `ContentPage` 를 포함 하는 `ListView`합니다. 데이터 소스를 `ListView` 를 통해 설정 됩니다는 `ItemsSource` 특성입니다. 각 행의 레이아웃을 `ItemsSource`내에 정의 되어는 `ListView.ItemTemplate` 요소입니다.

다음은 결과입니다.

![](data-and-databinding-images/bound-data.png "데이터 바인딩을 사용 하 여 ListView")

### <a name="binding-selecteditem"></a>SelectedItem 바인딩

선택한 항목에 바인딩할 수 경우가 `ListView`아닌 이벤트 처리기를 사용 하 여 변경 내용에 응답할 것입니다. XAML에서 이렇게 하려면 바인딩하는 `SelectedItem` 속성:

```xaml
<ListView x:Name="listView"
 SelectedItem="{Binding Source={x:Reference SomeLabel},
 Path=Text}">
 …
</ListView>
```

가정 `listView`의 `ItemsSource` 문자열 목록은 `SomeLabel` 해당 텍스트 속성은 바인딩되는 `SelectedItem`합니다.

## <a name="related-links"></a>관련 링크

- [양방향 바인딩을 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
