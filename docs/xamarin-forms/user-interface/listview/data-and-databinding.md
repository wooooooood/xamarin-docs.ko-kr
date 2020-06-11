---
제목: "ListView 데이터 원본" 설명: "이 문서에서는 listview를 데이터로 채우는 방법 Xamarin.Forms 및 listview에서 데이터 바인딩을 사용 하는 방법을 설명 합니다."
assetid: B5571660-1E82-4379-95C3-0725288CF5D9: xamarin-forms author: davidbritch: dabritch:: 03/23/2020-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="listview-data-sources"></a>ListView 데이터 원본

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)

는 Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) 데이터 목록을 표시 하는 데 사용 됩니다. 이 문서에서는 데이터를 데이터로 채우는 방법 `ListView` 및 선택한 항목에 데이터를 바인딩하는 방법을 설명 합니다.

## <a name="itemssource"></a>ItemsSource

는를 [`ListView`](xref:Xamarin.Forms.ListView) 구현 하는 [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) 모든 컬렉션을 허용할 수 있는 속성을 사용 하 여 데이터로 채워집니다 `IEnumerable` . 을 채우는 가장 간단한 방법은 `ListView` 문자열 배열을 사용 하는 것입니다.

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

이 방법을 사용 하면가 `ListView` 문자열 목록으로 채워집니다. 기본적으로는를 `ListView` 호출 `ToString` 하 고 `TextCell` 각 행에 대해 결과를 표시 합니다. 데이터가 표시 되는 방식을 사용자 지정 하려면 [셀 모양](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)을 참조 하세요.

`ItemsSource`가 배열로 전송 되었기 때문에 기본 목록 또는 배열이 변경 될 때 콘텐츠가 업데이트 되지 않습니다. 기본 목록에서 항목이 추가, 제거 및 변경 될 때 ListView가 자동으로 업데이트 되 게 하려면를 사용 해야 `ObservableCollection` 합니다. [`ObservableCollection`](xref:System.Collections.ObjectModel.ObservableCollection`1)는에 정의 되어 `System.Collections.ObjectModel` 있으며 `List` 변경 내용을 알릴 수 있다는 점을 제외 하 고는와 동일 합니다 `ListView` .

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
listView.ItemsSource = employees;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employees.Add(new Employee(){ DisplayName="Mr. Mono"});
```

## <a name="data-binding"></a>데이터 바인딩

데이터 바인딩은 사용자 인터페이스 개체의 속성을 viewmodel의 클래스와 같은 일부 CLR 개체의 속성에 바인딩하는 "glue"입니다. 데이터 바인딩은 많은 지루한 상용구 코드를 대체 하 여 사용자 인터페이스 개발을 간소화 하므로 유용 합니다.

데이터 바인딩은 바인딩된 값이 변경 될 때 개체를 동기화 된 상태로 유지 하는 방식으로 작동 합니다. 컨트롤의 값이 변경 될 때마다에 대 한 이벤트 처리기를 작성 하는 대신, viewmodel에서 바인딩을 설정 하 고 바인딩을 사용 하도록 설정 합니다.

데이터 바인딩에 대 한 자세한 내용은 [ Xamarin.Forms XAML 기본 사항 문서 시리즈](~/xamarin-forms/xaml/xaml-basics/index.md)의 4 부 인 [데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) 을 참조 하세요.

### <a name="binding-cells"></a>셀 바인딩

셀의 속성 (및 셀의 자식)은의 개체 속성에 바인딩할 수 있습니다 `ItemsSource` . 예를 들어는 `ListView` 직원 목록을 표시 하는 데 사용할 수 있습니다.

Employee 클래스:

```csharp
public class Employee
{
    public string DisplayName {get; set;}
}
```

`ObservableCollection<Employee>`가 만들어지고로 설정 `ListView` `ItemsSource` 되며 목록이 데이터로 채워집니다.

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
> 는 `ListView` 내부 변경 내용에 대 한 응답으로 업데이트 되지만 `ObservableCollection` `ListView` 다른 `ObservableCollection` 인스턴스가 원래 `ObservableCollection` 참조 (예:)에 할당 된 경우는 업데이트 되지 않습니다 `employees = otherObservableCollection;` .

다음 코드 조각은 `ListView` 직원 목록에 바인딩된을 보여 줍니다.

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

이 XAML 예제는 `ContentPage` 를 포함 하는를 정의 `ListView` 합니다. `ListView`의 데이터 원본은 `ItemsSource` 특성를 통해 설정됩니다. `ItemsSource`에서 각 행의 레이아웃은 `ListView.ItemTemplate` 요소 내에 정의됩니다. 그러면 다음과 같은 스크린샷을 생성 합니다.

![](data-and-databinding-images/bound-data.png "ListView using Data Binding")

> [!WARNING]
> `ObservableCollection`는 스레드로부터 안전 하지 않습니다. 을 수정 하면 `ObservableCollection` UI 업데이트가 수정 작업을 수행한 동일한 스레드에서 수행 됩니다. 스레드가 기본 UI 스레드가 아닌 경우 예외를 발생 시킵니다.

### <a name="binding-selecteditem"></a>바인딩 SelectedItem

`ListView`이벤트 처리기를 사용 하 여 변경 내용에 응답 하는 대신의 선택 된 항목에 바인딩하려는 경우가 많습니다. XAML에서이 작업을 수행 하려면 속성을 바인딩합니다 `SelectedItem` .

```xaml
<ListView x:Name="listView"
          SelectedItem="{Binding Source={x:Reference SomeLabel},
          Path=Text}">
 …
</ListView>
```

가 문자열 목록 이라고 가정 하면의 `listView` `ItemsSource` 속성이에 `SomeLabel` `Text` 바인딩됩니다 `SelectedItem` .

## <a name="related-links"></a>관련 링크

- [양방향 바인딩 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
