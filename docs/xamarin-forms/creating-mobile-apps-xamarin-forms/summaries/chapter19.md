---
title: 19 장 요약입니다. 컬렉션 뷰
description: 'Xamarin.ios를 사용 하 여 Mobile Apps 만들기: 19 장 요약 컬렉션 뷰'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 0AEC3A5C-586E-4D0F-9895-67E99A053A79
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: bffbd2dec4a8494723597ba6e0f0af69e57f3718
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032860"
---
# <a name="summary-of-chapter-19-collection-views"></a>19 장 요약입니다. 컬렉션 뷰

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)

> [!NOTE] 
> 이 페이지의 정보는 Xamarin.ios가 책에 제공 된 자료에서 달라져서 있는 영역을 표시 합니다.

Xamarin.ios는 컬렉션을 유지 관리 하 고 해당 요소를 표시 하는 세 가지 뷰를 정의 합니다.

- [`Picker`](xref:Xamarin.Forms.Picker) 은 사용자가 하나를 선택할 수 있는 문자열 항목의 비교적 짧은 목록입니다.
- 일반적으로 [`ListView`](xref:Xamarin.Forms.ListView) 는 일반적으로 동일한 형식 및 서식의 긴 항목 목록으로, 사용자가 하나를 선택할 수 있습니다.
- [`TableView`](xref:Xamarin.Forms.TableView) 은 데이터를 표시 하거나 사용자 입력을 관리 하기 위한 *셀* 모음 (일반적으로 다양 한 형식 및 모양)입니다.

MVVM 응용 프로그램은 `ListView`를 사용 하 여 선택 가능한 개체 컬렉션을 표시 하는 것이 일반적입니다.

## <a name="program-options-with-picker"></a>선택이 포함 된 프로그램 옵션

[`Picker`](xref:Xamarin.Forms.Picker) 은 사용자가 비교적 짧은 `string` 항목 목록 중에서 옵션을 선택할 수 있도록 해야 하는 경우에 적합 합니다.

### <a name="the-picker-and-event-handling"></a>선택 및 이벤트 처리

이 [**샘플에서는**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo) XAML을 사용 하 여 `Picker` [`Title`](xref:Xamarin.Forms.Picker.Title) 속성을 설정 하 고 [`Items`](xref:Xamarin.Forms.Picker.Items) 컬렉션에 `string` 항목을 추가 하는 방법을 보여 줍니다. 사용자가 `Picker`를 선택 하면 플랫폼에 종속 된 방식으로 `Items` 컬렉션의 항목이 표시 됩니다.

[`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) 이벤트는 사용자가 항목을 선택 했음을 나타냅니다. 0부터 시작 하는 [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) 속성은 선택한 항목을 나타냅니다. 항목을 선택 하지 않으면 `SelectedIndex` &ndash;1과 같습니다.

`SelectedIndex`를 사용 하 여 선택한 항목을 초기화할 수도 있지만 `Items` 컬렉션을 채운 후에는 설정 해야 합니다. XAML에서이는 속성 요소를 사용 하 여 `SelectedIndex`를 설정 하는 것을 의미 합니다.

### <a name="data-binding-the-picker"></a>선택 데이터 바인딩

`SelectedIndex` 속성은 바인딩 가능한 속성에 의해 지원 되지만 `Items`는 지원 되지 않으므로 `Picker`에서 데이터 바인딩을 사용 하는 것은 어렵습니다. 한 가지 해결 방법은 Xamarin.ios를 사용 하 여 [**도구 키트**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리와 같은 [`ObjectToIndexConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs) 와 함께 `Picker`를 사용 하는 것입니다. 이 [**는이**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding) 기능이 작동 하는 방식을 보여 줍니다.

> [!NOTE] 
> 이제 Xamarin.ios `Picker`는 데이터 바인딩을 지 원하는 `ItemsSource` 및 `SelectedItem` 속성을 포함 합니다. [선택](~/xamarin-forms/user-interface/picker/index.md)항목을 참조 하세요.

## <a name="rendering-data-with-listview"></a>ListView를 사용 하 여 데이터 렌더링

[`ListView`](xref:Xamarin.Forms.ListView) 은 [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) 및 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) 속성을 상속 하는 [`ItemsView<TVisual>`](xref:Xamarin.Forms.ItemsView`1) 에서 파생 되는 유일한 클래스입니다.

`ItemsSource` `IEnumerable` 형식 이지만 기본적으로 `null` 되며, 데이터 바인딩을 통해 컬렉션으로 명시적으로 초기화 하거나 (보다 일반적으로) 설정 해야 합니다. 이 컬렉션의 항목은 모든 형식일 수 있습니다.

`ListView`는 `ItemsSource` 컬렉션의 항목 중 하나로 설정 되는 [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) 속성을 정의 하거나 항목을 선택 하지 않은 경우 `null`를 정의 합니다. 새 항목을 선택 하면 `ListView` [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트를 발생 시킵니다.

### <a name="collections-and-selections"></a>컬렉션 및 선택

[**ListViewList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList) 샘플은 `List<Color>` 컬렉션에서 17 개의 `Color` 값으로 `ListView`를 채웁니다. 항목은 선택할 수 있지만 기본적으로 끌지 `ToString` 표시와 함께 표시 됩니다. 이 장에서 설명 하는 몇 가지 예는 해당 디스플레이를 수정 하 고 원하는 대로 설정 하는 방법을 보여 줍니다.

### <a name="the-row-separator"></a>행 구분 기호

IOS 및 Android에서 표시 되는 줄은 행을 구분 합니다. [`SeparatorVisibility`](xref:Xamarin.Forms.ListView.SeparatorVisibility) 및 [`SeparatorColor`](xref:Xamarin.Forms.ListView.SeparatorColor) 속성을 사용 하 여이를 제어할 수 있습니다. `SeparatorVisibility` 속성은 [`SeparatorVisibility`](xref:Xamarin.Forms.SeparatorVisibility)형식이 며, 열거형은 두 개의 멤버가 있습니다.

- [`Default`](xref:Xamarin.Forms.SeparatorVisibility.Default)기본 설정
- [`None`](xref:Xamarin.Forms.SeparatorVisibility.None)

### <a name="data-binding-the-selected-item"></a>선택한 항목에 대 한 데이터 바인딩

`SelectedItem` 속성은 바인딩 가능한 속성에 의해 지원 되므로 데이터 바인딩의 원본 또는 대상이 될 수 있습니다. 기본 `BindingMode` `OneWayToSource`되지만 일반적으로 MVVM 시나리오에서 양방향 데이터 바인딩의 대상이 됩니다. [**ListViewArray**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray) 샘플에서는 이러한 형식의 바인딩을 보여 줍니다.

### <a name="the-observablecollection-difference"></a>System.collections.objectmodel.observablecollection 차이

[**ListViewLogger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger) 샘플에서는 `ListView`의 `ItemsSource` 속성을 `List<DateTime>` 컬렉션으로 설정 하 고, 타이머를 사용 하 여 초당 새 `DateTime` 개체를 컬렉션에 추가 합니다.

그러나 컬렉션에서 항목을 추가 하거나 제거 하는 경우를 나타내는 알림 메커니즘이 `List<T>` 컬렉션에 없기 때문에 `ListView` 자동으로 업데이트 되지 않습니다.

이러한 시나리오에서 사용할 수 있는 보다 효율적인 클래스는 `System.Collections.ObjectModel` 네임 스페이스에 정의 [`ObservableCollection<T>`](xref:System.Collections.ObjectModel.ObservableCollection`1) . 이 클래스는 [`INotifyCollectionChanged`](xref:System.Collections.Specialized.INotifyCollectionChanged) 인터페이스를 구현 하며, 컬렉션에서 항목을 추가 하거나 제거할 때 또는 컬렉션 내에서 항목이 바뀌거나 이동 될 때 [`CollectionChanged`](xref:System.Collections.ObjectModel.ObservableCollection`1.CollectionChanged) 이벤트를 발생 시킵니다. `ListView`에서 `INotifyCollectionChanged`를 구현 하는 클래스가 `ItemsSource` 속성으로 설정 된 것을 내부적으로 검색 하는 경우 처리기를 `CollectionChanged` 이벤트에 연결 하 고 컬렉션이 변경 될 때 표시를 업데이트 합니다.

[**ObservableLogger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger) 샘플에서는 `ObservableCollection`를 사용 하는 방법을 보여 줍니다.

### <a name="templates-and-cells"></a>템플릿 및 셀

기본적으로 `ListView`는 각 항목의 `ToString` 메서드를 사용 하 여 컬렉션에 항목을 표시 합니다. 항목을 표시 하는 템플릿을 정의 하는 것이 더 나은 방법입니다.

이 기능을 사용 하 여 실험 하려면 [**Xamarin.ios 도구 키트**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 [`NamedColor`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) 클래스를 사용 하면 됩니다. 이 클래스는 `Color` 구조의 public 필드에 해당 하는 141 `NamedColor` 개체를 포함 하는 `IList<NamedColor>` 형식의 정적 `All` 속성을 정의 합니다.

[**NaiveNamedColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList) 샘플은 `ListView` `ItemsSource`을이 `NamedColor.All` 속성으로 설정 하지만 `NamedColor` 개체의 정규화 된 클래스 이름만 표시 됩니다.

이러한 항목을 표시 하려면 템플릿이 필요 `ListView`. 코드에서 [`Cell`](xref:Xamarin.Forms.Cell) 클래스의 파생 클래스를 참조 하는 [`DataTemplate` 생성자](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type)) 를 사용 하 여 `ItemsView<TVisual>`으로 정의 된 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) 속성을 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 개체로 설정할 수 있습니다. `Cell`에는 다음과 같은 5 개의 파생 요소가 있습니다.

- [`TextCell`](xref:Xamarin.Forms.TextCell) &mdash;에는 두 개의 `Label` 뷰 (개념적으로 말하기)가 포함 됩니다.
- [`ImageCell`](xref:Xamarin.Forms.ImageCell) &mdash;에 `Image` 뷰를 추가 `TextCell`
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) &mdash;에 `Entry` 보기가 포함 된 `Label`
- [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) &mdash;에 `Switch` 포함 된 `Label`
- [`ViewCell`](xref:Xamarin.Forms.ViewCell) &mdash;는 `View` 수 있습니다 (자식이 있을 수 있음).

그런 다음 [`SetValue`](xref:Xamarin.Forms.DataTemplate.SetValue(Xamarin.Forms.BindableProperty,System.Object)) 를 호출 하 고 `DataTemplate` 개체를 [`SetBinding`](xref:Xamarin.Forms.DataTemplate.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) 하 여 값을 `Cell` 속성과 연결 하거나 `Cell` 컬렉션의 항목 속성을 참조 하는 `ItemsSource` 속성에 대해 데이터 바인딩을 설정 합니다. 이는 [**Text3| Listcode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode) 샘플에서 설명 합니다.

`ListView`에서 각 항목을 표시 하는 경우 작은 시각적 트리가 템플릿에서 생성 되 고,이 시각적 트리에 있는 요소의 속성과 항목 간에 데이터 바인딩이 설정 됩니다. `ListView`의 [`ItemAppearing`](xref:Xamarin.Forms.ListView.ItemAppearing) 및 [`ItemDisappearing`](xref:Xamarin.Forms.ListView.ItemDisappearing) 이벤트에 대 한 처리기를 설치 하거나, 항목의 시각적 트리가 될 때마다 호출 되는 함수를 사용 하는 대체 [`DataTemplate` 생성자](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Func{System.Object})) 를 사용 하 여이 프로세스를 파악할 수 있습니다. 만들 수 있습니다.

[**Text3| listxaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml) 은 완전히 xaml로 기능 하는 동일한 프로그램을 보여 줍니다. `DataTemplate` 태그는 `ListView`의 `ItemTemplate` 속성으로 설정 되 고 `TextCell`는 `DataTemplate`로 설정 됩니다. 컬렉션에 있는 항목의 속성에 대 한 바인딩은 `TextCell`의 [`Text`](xref:Xamarin.Forms.TextCell.Text) 및 [`Detail`](xref:Xamarin.Forms.TextCell.Detail) 속성에 직접 설정 됩니다.

### <a name="custom-cells"></a>사용자 지정 셀

XAML에서 `DataTemplate` [`ViewCell`](xref:Xamarin.Forms.ViewCell) 를 설정한 다음 사용자 지정 시각적 트리를 `ViewCell`의 [`View`](xref:Xamarin.Forms.ViewCell.View) 속성으로 정의할 수 있습니다. (`View`은 `ViewCell`의 콘텐츠 속성 이므로 `ViewCell.View` 태그가 필요 하지 않습니다.) [**Customnamedcolorlist**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList) 샘플은이 기법을 보여 줍니다.

[![사용자 지정 명명 된 색 목록의 삼중 스크린샷](images/ch19fg11-small.png "사용자 지정 명명 된 색 목록")](images/ch19fg11-large.png#lightbox "사용자 지정 명명 된 색 목록")

모든 플랫폼의 크기를 조정 하는 것은 복잡할 수 있습니다. [`RowHeight`](xref:Xamarin.Forms.ListView.RowHeight) 속성이 유용 하지만 경우에 따라 효율성이 낮지만 `ListView`에서 행의 크기를 조정 하는 [`HasUnevenRows`](xref:Xamarin.Forms.ListView.HasUnevenRows) 속성을 사용할 수 있습니다. IOS 및 Android의 경우 이러한 두 속성 중 하나를 사용 하 여 적절 한 행 크기 조정을 가져와야 합니다.

### <a name="grouping-the-listview-items"></a>ListView 항목 그룹화

`ListView`는 항목 그룹을 지원 하 고 이러한 그룹 간에 이동할 수 있습니다. `ItemsSource` 속성은 컬렉션의 컬렉션으로 설정 해야 합니다. `ItemsSource`가 설정 된 개체는 `IEnumerable`를 구현 해야 하며, 컬렉션의 각 항목은 `IEnumerable`도 구현 해야 합니다. 각 그룹에는 그룹에 대 한 텍스트 설명과 3 자 약어 라는 두 개의 속성이 포함 되어야 합니다.

[**Xamarin.ios 도구 키트**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 [`NamedColorGroup`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs) 클래스는 7 개의 `NamedColor` 개체 그룹을 만듭니다. [**ColorGroupList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList) 샘플에서는 `true``ListView` 설정 된 [`IsGroupingEnabled`](xref:Xamarin.Forms.ListView.IsGroupingEnabled) 속성을 사용 하 여 이러한 그룹을 사용 하는 방법과 각 그룹의 속성에 바인딩된 [`GroupDisplayBinding`](xref:Xamarin.Forms.ListView.GroupDisplayBinding) 및 [`GroupShortNameBinding`](xref:Xamarin.Forms.ListView.GroupShortNameBinding) 속성을 사용 하는 방법을 보여 줍니다.

### <a name="custom-group-headers"></a>사용자 지정 그룹 헤더

`GroupDisplayBinding` 속성을 헤더에 대 한 템플릿을 정의 하는 [`GroupHeaderTemplate`](xref:Xamarin.Forms.ListView.GroupHeaderTemplate) 로 바꿔서 `ListView` 그룹에 대 한 사용자 지정 헤더를 만들 수 있습니다.

### <a name="listview-and-interactivity"></a>ListView 및 대화형 작업

일반적으로 응용 프로그램은 `ItemSelected` 또는 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트에 처리기를 연결 하거나 `SelectedItem` 속성에 데이터 바인딩을 설정 하 여 `ListView`와의 사용자 상호 작용을 얻습니다. 그러나 일부 셀 형식 (`EntryCell` 및 `SwitchCell`)은 사용자 상호 작용을 허용 하 고, 사용자가 직접 사용자와 상호 작용 하는 사용자 지정 셀을 만들 수 있습니다. [**InteractiveListView**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView) 는 [`ColorViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) 100 인스턴스를 만들고 사용자가 3 개 숫자의 `Slider` 요소를 사용 하 여 각 색을 변경할 수 있도록 합니다. 또한이 프로그램은 [**Xamarin.ios 도구 키트**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)의 [`ColorToContrastColorConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs) 을 사용 합니다.

## <a name="listview-and-mvvm"></a>ListView 및 MVVM

`ListView` MVVM 시나리오에서 큰 역할을 수행 합니다. ViewModel에 `IEnumerable` 컬렉션이 있을 때마다 `ListView`에 바인딩됩니다. 또한 컬렉션의 항목은 종종 템플릿에서 속성을 사용 하 여 바인딩할 `INotifyPropertyChanged`을 구현 합니다.

### <a name="a-collection-of-viewmodels"></a>ViewModels 컬렉션

이를 살펴보기 위해 [**SchoolOfFineArts**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) library는이 가상 학교에서 [XML 데이터 파일](https://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml) 및 가상 학생의 이미지를 기반으로 여러 클래스를 만듭니다.

[`Student`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs) 클래스는 [`ViewModelBase`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs)에서 파생 됩니다. [`StudentBody`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs) 클래스는 `Student` 개체의 컬렉션이 며 `ViewModelBase`에서도 파생 됩니다. [`SchoolViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) XML 파일을 다운로드 하 고 모든 개체를 어셈블합니다.

[**StudentList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList) 프로그램은 `ImageCell`를 사용 하 여 학생 및 해당 이미지를 `ListView`에 표시 합니다.

[![학생 목록의 삼중 스크린샷](images/ch19fg18-small.png "학생 목록")](images/ch19fg18-large.png#lightbox "학생 목록")

[**ListViewHeader**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader) 샘플은 [`Header`](xref:Xamarin.Forms.ListView.Header) 속성을 추가 하지만 Android에만 표시 됩니다.

### <a name="selection-and-the-binding-context"></a>선택 및 바인딩 컨텍스트

[**SelectedStudentDetail**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail) 프로그램은 `StackLayout` `BindingContext`을 `ListView`의 `SelectedItem` 속성에 바인딩합니다. 그러면 프로그램에서 선택한 학생에 대 한 자세한 정보를 표시할 수 있습니다.

### <a name="context-menus"></a>상황에 맞는 메뉴

셀은 플랫폼별 방식으로 구현 되는 상황에 맞는 메뉴를 정의할 수 있습니다. 이 메뉴를 만들려면 `Cell`의 [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) 속성에 [`MenuItem`](xref:Xamarin.Forms.MenuItem) 개체를 추가 합니다.

`MenuItem`는 5 가지 속성을 정의 합니다.

- [`Text`](xref:Xamarin.Forms.MenuItem.Text)(`string` 형식)
- [`Icon`](xref:Xamarin.Forms.MenuItem.Icon)(`FileImageSource` 형식)
- [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive)(`bool` 형식)
- [`Command`](xref:Xamarin.Forms.MenuItem.Command)(`ICommand` 형식)
- [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter)(`object` 형식)

`Command` 및 `CommandParameter` 속성은 각 항목의 ViewModel에 원하는 메뉴 명령을 수행 하는 메서드가 포함 되어 있음을 의미 합니다. MVVM 않는 시나리오에서 `MenuItem` [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked) 이벤트도 정의 합니다.

이 기법은이 기법 [**을 보여 줍니다**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu) . 각 `MenuItem`의 `Command` 속성은 `Student` 클래스에서 `ICommand` 형식의 속성에 바인딩됩니다. 선택한 개체를 제거 하거나 삭제 하는 `MenuItem`에 대해 `IsDestructive` 속성을 `true`로 설정 합니다.

### <a name="varying-the-visuals"></a>시각적 개체 변경

경우에 따라 속성에 따라 `ListView` 항목의 시각적 개체에 약간의 변형이 필요 합니다. 예를 들어 학생의 점수 포인트 평균이 2.0 미만이 면 [**Colorcodedstudents**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents) 샘플은 해당 학생의 이름을 빨간색으로 표시 합니다.
이는 [**xamarin.ios의 도구 키트**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에서 [`ThresholdToObjectConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs)바인딩 값 변환기를 사용 하 여 수행 됩니다.

### <a name="refreshing-the-content"></a>콘텐츠 새로 고침

`ListView`는 해당 데이터를 새로 고치기 위한 풀 다운 제스처를 지원 합니다. 프로그램은이를 사용 하도록 설정 하려면 [`IsPullToRefresh`](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) 속성을 `true`으로 설정 해야 합니다. `ListView`는 [`IsRefreshing`](xref:Xamarin.Forms.ListView.IsRefreshing) 속성을 `true`로 설정 하 고 [`Execute`](xref:Xamarin.Forms.ListView.RefreshCommand) 속성의`RefreshCommand`메서드를 호출 하 여 [`Refreshing`](xref:Xamarin.Forms.ListView.Refreshing) 이벤트와 (MVVM 시나리오의 경우)를 발생 시켜 풀 다운 제스처에 응답 합니다.

`Refresh` 이벤트 또는 `RefreshCommand`를 처리 하는 코드는 `ListView`에 의해 표시 되는 데이터를 업데이트 하 고 `IsRefreshing`를 다시 `false`로 설정 합니다.

[**Rssfeed**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed) 샘플은 데이터 바인딩에 대 한 `RefreshCommand` 및 `IsRefreshing` 속성을 구현 하는 [`RssFeedViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs) 을 사용 하는 방법을 보여 줍니다.

## <a name="the-tableview-and-its-intents"></a>TableView 및 해당 의도

`ListView`은 일반적으로 동일한 유형의 여러 인스턴스를 표시 하지만 [`TableView`](xref:Xamarin.Forms.TableView) 는 일반적으로 다양 한 유형의 여러 속성에 대 한 사용자 인터페이스를 제공 하는 데 중점을 두었습니다. 각 항목은 속성을 표시 하거나이에 대 한 사용자 인터페이스를 제공 하기 위해 파생 된 자체 [`Cell`](xref:Xamarin.Forms.Cell) 와 연결 됩니다.

### <a name="properties-and-hierarchies"></a>속성 및 계층 구조

`TableView`는 다음 네 가지 속성만 정의 합니다.

- 열거형 [`TableIntent`](xref:Xamarin.Forms.TableIntent)형식의 [`Intent`](xref:Xamarin.Forms.TableView.Intent)
- [`TableRoot`](xref:Xamarin.Forms.TableRoot)형식의 [`Root`](xref:Xamarin.Forms.TableView.Root) `TableView`의 content 속성
- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight)(`int` 형식)
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows)(`bool` 형식)

`TableIntent` 열거형은 `TableView`를 사용 하는 방법을 나타냅니다.

- [`Data`](xref:Xamarin.Forms.TableIntent.Data)
- [`Form`](xref:Xamarin.Forms.TableIntent.Form)
- [`Settings`](xref:Xamarin.Forms.TableIntent.Settings)
- [`Menu`](xref:Xamarin.Forms.TableIntent.Menu)

또한 이러한 멤버는 `TableView`에 대 한 일부 사용을 제안 합니다.

테이블을 정의 하는 데는 몇 가지 다른 클래스가 있습니다.

- [`TableSectionBase`](xref:Xamarin.Forms.TableSectionBase) 은 `BindableObject`에서 파생 되 고 [`Title`](xref:Xamarin.Forms.TableSectionBase.Title) 속성을 정의 하는 추상 클래스입니다.

- [`TableSectionBase<T>`](xref:Xamarin.Forms.TableSectionBase`1) 은 `TableSectionBase`에서 파생 되 고 `IList<T>` 및를 구현 하는 추상 클래스 `INotifyCollectionChanged`

- [`TableSection`](xref:Xamarin.Forms.TableSection) 은 `TableSectionBase<Cell>`에서 파생 됩니다.

- [`TableRoot`](xref:Xamarin.Forms.TableRoot) 은 `TableSectionBase<TableSection>`에서 파생 됩니다.

간단히 말해서 `TableView`에는 `TableRoot` 개체로 설정 하는 `Root` 속성이 있습니다 .이 개체는 `TableSection` 개체의 컬렉션으로, 각 개체는 `Cell` 개체의 컬렉션입니다. 테이블에는 여러 섹션이 있으며 각 섹션에는 여러 개의 셀이 있습니다. 테이블 자체에는 제목이 있을 수 있으며, 각 섹션에는 제목이 있을 수 있습니다. `TableView`은 `Cell` 파생물을 사용 하지만 `DataTemplate`를 사용 하지 않습니다.

### <a name="a-prosaic-form"></a>Prosaic 폼

[**Entryform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm) 샘플은 `TableView`의 `BindingContext` 되는 인스턴스인 [`PersonalInformation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) 뷰 모델을 정의 합니다. `TableSection`에서 파생 된 각 `Cell`에는 `PersonalInformation` 클래스의 속성에 대 한 바인딩이 있을 수 있습니다.

### <a name="custom-cells"></a>사용자 지정 셀

[**ConditionalCells**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells) 샘플은 **entryform**을 확장 합니다. [`ProgrammerInformation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) 클래스에는 두 개의 추가 속성에 대 한 적용 가능성을 제어 하는 부울 속성이 포함 되어 있습니다. 이러한 두 가지 추가 속성의 경우 프로그램은 [**xamarin.ios**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) PickerCell.xaml.cs 라이브러리의 [및](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs) [를 기반](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml) 으로 하는 사용자 지정 `PickerCell`를 사용 합니다.

두 `PickerCell` 요소의 `IsEnabled` 속성이 `ProgrammerInformation`의 부울 속성에 바인딩되므로이 기술은 다음 샘플을 표시 하는 작동 하지 않는 것 같습니다.

### <a name="conditional-sections"></a>조건부 섹션

[**ConditionalSection**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection) 샘플은 부울 항목의 선택에 대 한 조건에 해당 하는 두 항목을 별도의 `TableSection`에 배치 합니다. 코드 해제 파일은 `TableView`에서이 섹션을 제거 하거나 부울 속성을 기반으로 다시 추가 합니다.

### <a name="a-tableview-menu"></a>TableView 메뉴

`TableView`의 또 다른 용도는 메뉴입니다. [**Menucommands**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands) 샘플은 화면 주위에 `BoxView`를 조금 이동 하는 데 사용할 수 있는 메뉴를 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [19 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [19 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [선택기](~/xamarin-forms/user-interface/picker/index.md)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
