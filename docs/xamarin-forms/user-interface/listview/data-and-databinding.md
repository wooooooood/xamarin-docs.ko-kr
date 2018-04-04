---
title: ListView 데이터 원본
description: 하 여 ListView 데이터로 채우는 방법을 알아봅니다.
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 6f0553f2887d4acb1015da71395be3f316e9acee
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="listview-data-sources"></a>ListView 데이터 원본

ListView는 데이터의 목록을 표시 하기 위해 사용 됩니다. 데이터 및 선택한 항목에 어떻게 바인딩할 수 있는 ListView 채우기 알아봅니다.

- **[ItemsSource를 설정](#ItemsSource)**  &ndash; 단순 목록 또는 배열의 사용 합니다.
- **[데이터 바인딩](#Data_Binding)**  &ndash; 모델과 ListView 사이의 관계를 설정 합니다. 바인딩은 MVVM 패턴에 이상적입니다.

## <a name="itemssource"></a>ItemsSource
ListView를 사용 하 여 데이터 채워집니다는 `ItemsSource` 속성을 구현 하는 모든 컬렉션을 받을 수 있는 `IEnumerable`합니다. 채우는 한 가장 간단한 방법은 `ListView` 에서는 문자열의 배열을 사용 하 여:

```csharp
var listView = new ListView();
listView.ItemsSource = new string[]{
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

위의 방법은 채워집니다는 `ListView` 문자열의 목록입니다. 기본적으로 `ListView` 호출 `ToString` 에 결과 표시 한 `TextCell` 각 행에 대 한 합니다. 참조 데이터 표시 방법을 사용자 지정할 [셀 모양](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)합니다.

때문에 `ItemsSource` ध를 배열에 기본 목록 또는 배열의 변경으로 콘텐츠가 업데이트 되지 것입니다. ListView에 항목 추가, 제거 및 내부 목록에서 변경할 때 자동으로 업데이트 하려는 경우 사용 해야 합니다는 `ObservableCollection`합니다. [`ObservableCollection`](https://developer.xamarin.com/api/type/System.Collections.ObjectModel.ObservableCollection%3CT%3E/) 에 정의 된 `System.Collections.ObjectModel` 와 동일 하 게 되며 `List`한다는 점을 제외 하 게 알릴 수 있는 `ListView` 변경:

```csharp
ObservableCollection<Employees> employeeList = new ObservableCollection<Employess>();
listView.ItemsSource = employeeList;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employeeList.Add(new Employee(){ DisplayName="Mr. Mono"});
```

<a name="Data_Binding" />

## <a name="data-binding"></a>데이터 바인딩
데이터 바인딩은 프로그램 ViewModel에서 클래스와 같은 일부 CLR 개체의 속성에 사용자 인터페이스 개체의 속성을 바인딩하는 "연결"입니다. 데이터 바인딩 많은 지루한 작업 상용구 코드를 대체 하 여 사용자 인터페이스 개발을 간소화 하기 때문에 유용 합니다.

데이터 바인딩 바인딩된 값이 변경으로 개체를 동기화 상태로 유지 하 여 작동 합니다. 컨트롤의 값이 변경 될 때마다에 대 한 이벤트 처리기를 작성 하는 대신 바인딩을 설정 하 고 프로그램 ViewModel에서 바인딩을 사용 하도록 설정 합니다.

데이터 바인딩에 대 한 자세한 내용은 참조 하십시오. [데이터 바인딩 기본](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) 중 4 개 포함 되어 있는 [Xamarin.Forms XAML 기본 사항 문서 시리즈](~/xamarin-forms/xaml/xaml-basics/index.md)합니다.

### <a name="binding-cells"></a>셀 바인딩
셀 (및 셀의 자식) 속성에 있는 개체의 속성에 바인딩할 수 있습니다는 `ItemsSource`합니다. 예를 들어 이미지와 직원의 목록을 표시할 ListView는 사용할 수 있습니다.

직원의 클래스.

```csharp
public class Employee{
    public string DisplayName {get; set;}
}
```

`ObservableCollection<Employee>` 생성 되어으로 설정 된 `ListView`의 `ItemsSource`:

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

다음 코드 조각에서는 `ListView` 직원의 목록에 연결:

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

XAML에서 연결 된 있기 수 있지만 바인딩 않았거나 간단히 하기 위해 코드에서 설치를 확인 합니다.

이전 약간의 XAML 정의 `ContentPage` 를 포함 하는 `ListView`합니다. 데이터 소스는 `ListView` 를 통해 설정 됩니다는 `ItemsSource` 특성입니다. 각 행의 레이아웃은 `ItemsSource`내에 정의 된는 `ListView.ItemTemplate` 요소입니다.

다음은 결과입니다.

![](data-and-databinding-images/bound-data.png "데이터 바인딩 사용 하 여 ListView")

### <a name="binding-selecteditem"></a>SelectedItem 바인딩

선택된 된 항목을 바인딩할 해야 하는 종종는 `ListView`대신, 이벤트 처리기를 사용 하 여 변경 내용에 응답 하는 보다 합니다. XAML에서이 작업을 수행 하려면 바인딩는 `SelectedItem` 속성:

```xaml
<ListView x:Name="listView"
 SelectedItem="{Binding Source={x:Reference SomeLabel},
 Path=Text}">
 …
</ListView>
```

가정 `listView`의 `ItemsSource` 은 문자열의 목록 `SomeLabel` 에 바인딩된 텍스트 속성은 해당는 `SelectedItem`합니다.



## <a name="related-links"></a>관련 링크

- [양방향 바인딩을 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [1.4의 릴리스 정보](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1.3 릴리스 정보](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
