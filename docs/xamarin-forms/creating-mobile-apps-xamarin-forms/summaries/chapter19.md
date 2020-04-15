---
title: 19장의 요약 정보입니다. 컬렉션 뷰
description: 'Xamarin.Forms로 모바일 앱 만들기: 19장의 요약 정보입니다. 컬렉션 뷰'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 0AEC3A5C-586E-4D0F-9895-67E99A053A79
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: bffbd2dec4a8494723597ba6e0f0af69e57f3718
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73032860"
---
# <a name="summary-of-chapter-19-collection-views"></a>19장의 요약 정보입니다. 컬렉션 뷰

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)

> [!NOTE] 
> 이 페이지의 정보는 Xamarin.Forms가 책에 제공된 자료와 다른 영역을 표시합니다.

Xamarin.Forms는 컬렉션을 유지 관리하고 해당 요소를 표시하는 다음 세 가지 뷰를 정의합니다.

- [`Picker`](xref:Xamarin.Forms.Picker)는 사용자가 하나를 선택할 수 있는 비교적 짧은 문자열 항목 목록입니다.
- [`ListView`](xref:Xamarin.Forms.ListView)는 일반적으로 형식과 서식이 동일한 긴 항목 목록으로, 마찬가지로 사용자가 하나를 선택할 수 있습니다.
- [`TableView`](xref:Xamarin.Forms.TableView)는 데이터를 표시하거나 사용자 입력을 관리하는 (일반적으로 형식과 모양이 다양한) *셀* 컬렉션입니다.

MVVM 애플리케이션에는 `ListView`를 사용하여 선택 가능한 개체 컬렉션을 표시하는 것이 일반적입니다.

## <a name="program-options-with-picker"></a>선택기가 포함된 프로그램 옵션

사용자가 비교적 짧은 `string` 항목 목록 중에서 옵션을 선택할 수 있도록 해야 하는 경우 [`Picker`](xref:Xamarin.Forms.Picker)를 선택하는 것이 좋습니다.

### <a name="the-picker-and-event-handling"></a>선택기 및 이벤트 처리

[**PickerDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo) 샘플은 XAML을 사용하여 `Picker` [`Title`](xref:Xamarin.Forms.Picker.Title) 속성을 설정하고 [`Items`](xref:Xamarin.Forms.Picker.Items) 컬렉션에 `string` 항목을 추가하는 방법을 보여줍니다. 사용자가 `Picker`를 선택하면 플랫폼에 종속된 방식으로 `Items` 컬렉션의 항목이 표시됩니다.

[`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) 이벤트는 사용자가 항목을 선택한 시간을 나타냅니다. 그러면 0부터 시작하는 [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) 속성은 사용자가 선택한 항목을 나타냅니다. 사용자가 항목을 선택하지 않으면 `SelectedIndex`는 &ndash;1입니다.

`SelectedIndex`를 사용하여 선택된 항목을 초기화할 수도 있지만, `Items` 컬렉션이 채워진 후에 설정해야 합니다. XAML에서는 속성 요소를 사용하여 `SelectedIndex`를 설정하는 것을 의미합니다.

### <a name="data-binding-the-picker"></a>선택기 데이터 바인딩

`SelectedIndex` 속성은 바인딩 가능한 속성에서 지원되지만 `Items`는 지원되지 않으므로 `Picker`에서 데이터 바인딩을 사용하기 어렵습니다. 한 가지 해결 방법은 [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에 있는 것처럼 `Picker`를 [`ObjectToIndexConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs)와 함께 사용하는 것입니다. [**PickerBinding**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding)은 이 방법이 어떻게 작동하는지 보여줍니다.

> [!NOTE] 
> 이제 Xamarin.Forms `Picker`는 데이터 바인딩을 지 원하는 `ItemsSource` 및 `SelectedItem` 속성을 포함하고 있습니다. [선택기](~/xamarin-forms/user-interface/picker/index.md)를 참조하세요.

## <a name="rendering-data-with-listview"></a>ListView를 사용하여 데이터 렌더링

[`ListView`](xref:Xamarin.Forms.ListView)는 [`ItemsView<TVisual>`](xref:Xamarin.Forms.ItemsView`1)에서 파생되는 유일한 클래스이며, [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) 및 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) 속성을 상속합니다.

`ItemsSource`는 `IEnumerable` 형식이지만 기본적으로 `null`이며 데이터 바인딩을 통해 명시적으로 초기화하거나 (보다 일반적으로) 컬렉션으로 설정해야 합니다. 이 컬렉션의 항목은 어떤 형식이어도 상관 없습니다.

`ListView`는 [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) 속성을 정의합니다. 이 속성은 `ItemsSource` 컬렉션의 항목 중 하나로 설정되거나 항목이 선택되지 않은 경우 `null`로 설정됩니다. `ListView`는 새 항목이 선택되면 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트를 실행합니다.

### <a name="collections-and-selections"></a>컬렉션 및 선택

[**ListViewList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList) 샘플은 `List<Color>` 컬렉션의 17개 `Color` 값으로 `ListView`를 채웁니다. 항목은 선택 가능하지만, 기본적으로 보기에 좋지 않은 `ToString` 표현으로 표시됩니다. 이 챕터의 여러 예제는 이러한 디스플레이를 수정하여 원하는 대로 예쁘게 만드는 방법을 보여줍니다.

### <a name="the-row-separator"></a>행 구분 기호

iOS 및 Android 디스플레이에서는 얇은 선이 행을 구분합니다. [`SeparatorVisibility`](xref:Xamarin.Forms.ListView.SeparatorVisibility) 및 [`SeparatorColor`](xref:Xamarin.Forms.ListView.SeparatorColor) 속성을 사용하여 이 선을 제어할 수 있습니다. `SeparatorVisibility` 속성은 [`SeparatorVisibility`](xref:Xamarin.Forms.SeparatorVisibility) 형식이며, 다음 두 멤버가 있는 열거형입니다.

- 기본 설정인 [`Default`](xref:Xamarin.Forms.SeparatorVisibility.Default)
- [`None`](xref:Xamarin.Forms.SeparatorVisibility.None)

### <a name="data-binding-the-selected-item"></a>선택한 항목의 데이터 바인딩

`SelectedItem` 속성은 바인딩 가능한 속성에서 지원되므로 데이터 바인딩의 원본이 될 수도 있고 대상이 될 수도 있습니다. 기본 `BindingMode`는 `OneWayToSource`이지만, 일반적으로 양방향 데이터 바인딩의 대상이며 MVVM 시나리오에서 특히 그렇습니다. [**ListViewArray**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray) 샘플은 이러한 형식의 바인딩을 보여줍니다.

### <a name="the-observablecollection-difference"></a>ObservableCollection의 차이점

[**ListViewLogger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger) 샘플은 `ListView`의 `ItemsSource` 속성을 `List<DateTime>` 컬렉션으로 설정한 다음, 타이머를 사용하여 매초마다 새 `DateTime` 개체를 컬렉션에 추가합니다.

그러나 `List<T>` 컬렉션에는 컬렉션에 항목이 추가 또는 제거될 때 알려주는 알림 메커니즘이 없기 때문에 `ListView`가 자동으로 업데이트되지 않습니다.

이러한 시나리오에 사용할 수 있는 훨씬 효율적인 클래스는 `System.Collections.ObjectModel` 네임스페이스에 정의된 [`ObservableCollection<T>`](xref:System.Collections.ObjectModel.ObservableCollection`1)입니다. 이 클래스는 [`INotifyCollectionChanged`](xref:System.Collections.Specialized.INotifyCollectionChanged) 인터페이스를 구현합니다. 그리고 그 결과로 컬렉션에 항목이 추가 또는 제거되거나 컬렉션 내에서 항목이 대체 또는 이동될 때 [`CollectionChanged`](xref:System.Collections.ObjectModel.ObservableCollection`1.CollectionChanged) 이벤트를 실행합니다. `ListView`는 `INotifyCollectionChanged`를 구현하는 클래스가 `ItemsSource` 속성으로 설정된 것을 내부에서 감지하면 `CollectionChanged` 이벤트에 처리기를 연결하고 컬렉션이 변경될 때 디스플레이를 업데이트합니다.

[**ObservableLogger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger) 샘플은 `ObservableCollection` 바인딩 사용 방법을 보여줍니다.

### <a name="templates-and-cells"></a>템플릿 및 셀

기본적으로 `ListView`는 각 항목의 `ToString` 메서드를 사용하여 컬렉션의 항목을 표시합니다. 보다 효율적인 방법은 항목을 표시하는 템플릿을 정의하는 것입니다.

이 기능을 실험하려면 [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 [`NamedColor`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) 클래스를 사용하면 됩니다. 이 클래스는 `Color` 구조체의 퍼블릭 필드에 해당하는 141개 `NamedColor` 개체를 포함하고 있는 `IList<NamedColor>` 형식의 정적 `All` 속성을 정의합니다.

[**NaiveNamedColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList) 샘플은 `ListView`의 `ItemsSource`를 이 `NamedColor.All` 속성으로 설정하지만, `NamedColor` 개체의 정규화된 클래스 이름만 표시됩니다.

`ListView`는 이러한 항목을 표시하기 위한 템플릿이 필요합니다. 코드에서는 [`Cell`](xref:Xamarin.Forms.Cell) 클래스의 파생 클래스를 참조하는 [`DataTemplate` 생성자](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type))를 사용하여 `ItemsView<TVisual>`에서 정의되는 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) 속성을 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 개체로 설정할 수 있습니다. `Cell`에는 다음과 같은 5개의 파생 항목이 있습니다.

- [`TextCell`](xref:Xamarin.Forms.TextCell) &mdash;에는 두 가지 `Label` 보기가 포함되어 있습니다(개념적으로 보자면).
- [`ImageCell`](xref:Xamarin.Forms.ImageCell) &mdash;은 `TextCell`에 `Image` 보기를 추가합니다.
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) &mdash;에는 `Label`을 사용하는 `Entry` 보기가 포함되어 있습니다.
- [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) &mdash;에는 `Label`을 사용하는 `Switch`가 포함되어 있습니다.
- [`ViewCell`](xref:Xamarin.Forms.ViewCell) &mdash;은 자식 요소와 마찬가지로 어떤 `View`여도 상관 없습니다.

그 후 `DataTemplate` 개체에서 [`SetValue`](xref:Xamarin.Forms.DataTemplate.SetValue(Xamarin.Forms.BindableProperty,System.Object)) 및 [`SetBinding`](xref:Xamarin.Forms.DataTemplate.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase))을 호출하여 값을 `Cell` 속성에 연결하거나, `ItemsSource` 컬렉션의 항목 속성을 참조하는 `Cell` 속성에서 데이터 바인딩을 설정합니다. 이 내용은 [**TextCellListCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode) 샘플에 설명되어 있습니다.

`ListView`가 각 항목을 표시할 때마다 작은 시각적 트리가 템플릿에서 생성되고, 이 시각적 트리에 있는 요소의 속성과 항목 사이에 데이터 바인딩이 설정됩니다. `ListView`의 [`ItemAppearing`](xref:Xamarin.Forms.ListView.ItemAppearing) 및 [`ItemDisappearing`](xref:Xamarin.Forms.ListView.ItemDisappearing) 이벤트용 처리기를 설치하거나, 항목의 시각적 트리를 만들어야 할 때마다 호출되는 함수를 사용하는 대체 [`DataTemplate` 생성자](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Func{System.Object}))를 사용해 보면 이 프로세스를 이해할 수 있습니다.

[**TextCellListXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml)은 기능적으로 완전히 동일한 XAML의 프로그램을 보여줍니다. `DataTemplate` 태그는 `ListView`의 `ItemTemplate` 속성으로 설정되고, `TextCell`은 `DataTemplate`으로 설정됩니다. 컬렉션에 있는 항목의 속성에 대한 바인딩은 `TextCell`의 [`Text`](xref:Xamarin.Forms.TextCell.Text) 및 [`Detail`](xref:Xamarin.Forms.TextCell.Detail) 속성에서 직접 설정됩니다.

### <a name="custom-cells"></a>사용자 지정 셀

XAML에서는 [`ViewCell`](xref:Xamarin.Forms.ViewCell)을 `DataTemplate`으로 설정한 다음, 사용자 지정 시각적 트리를 `ViewCell`의 [`View`](xref:Xamarin.Forms.ViewCell.View) 속성으로 정의할 수 있습니다. (`View`는 `ViewCell`의 콘텐츠 속성이므로 `ViewCell.View` 태그가 필요 없습니다.) 다음과 같이 [**CustomNamedColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList) 샘플은 이 기술을 보여줍니다.

[![사용자 지정 명명된 색 목록의 세 가지 스크린샷](images/ch19fg11-small.png "사용자 지정 명명된 색 목록")](images/ch19fg11-large.png#lightbox "사용자 지정 명명된 색 목록")

모든 플랫폼에 적합한 크기로 조정하는 것은 어려운 일일 수 있습니다. [`RowHeight`](xref:Xamarin.Forms.ListView.RowHeight) 속성은 유용하지만, 경우에 따라 효율성이 떨어지지만 `ListView`에서 행의 크기를 조정하도록 강제하는 [`HasUnevenRows`](xref:Xamarin.Forms.ListView.HasUnevenRows) 속성을 사용할 때도 있습니다. iOS 및 Android의 경우 이러한 두 속성 중 하나를 사용하여 행 크기를 적절하게 조정해야 합니다.

### <a name="grouping-the-listview-items"></a>ListView 항목 그룹화

`ListView`는 항목을 그룹화하고 그룹 간에 이동하는 것을 지원합니다. `ItemsSource` 속성은 다음과 같이 컬렉션의 컬렉션으로 설정해야 합니다. `ItemsSource`가 설정된 개체는 `IEnumerable`을 구현해야 하고, 컬렉션의 각 항목도 `IEnumerable`을 구현해야 합니다. 각 그룹에는 두 가지 속성, 즉, 그룹의 텍스트 설명과 3자 약어가 포함되어야 합니다.

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 [`NamedColorGroup`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs) 클래스는 `NamedColor` 개체 그룹을 7개 만듭니다. [**ColorGroupList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList) 샘플은 이러한 그룹을 `true`로 설정된 `ListView`의 [`IsGroupingEnabled`](xref:Xamarin.Forms.ListView.IsGroupingEnabled) 속성과 각 그룹의 속성에 바인딩된 [`GroupDisplayBinding`](xref:Xamarin.Forms.ListView.GroupDisplayBinding) 및 [`GroupShortNameBinding`](xref:Xamarin.Forms.ListView.GroupShortNameBinding) 속성에 사용하는 방법을 보여줍니다.

### <a name="custom-group-headers"></a>사용자 지정 그룹 헤더

`GroupDisplayBinding` 속성을 헤더의 템플릿을 정의하는 [`GroupHeaderTemplate`](xref:Xamarin.Forms.ListView.GroupHeaderTemplate)으로 바꿔서 `ListView` 그룹의 사용자 지정 헤더를 만들 수 있습니다.

### <a name="listview-and-interactivity"></a>ListView 및 대화형 작업

일반적으로 애플리케이션은 `ItemSelected` 또는 [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) 이벤트에 처리기를 연결하거나 `SelectedItem` 속성에서 데이터 바인딩을 설정하여 `ListView`와의 사용자 상호 작용을 달성합니다. 그러나 일부 셀 형식(`EntryCell` 및 `SwitchCell`)은 사용자 상호 작용을 허용하며, 사용자와 직접 상호 작용하는 사용자 지정 셀을 만들 수 있습니다. [**InteractiveListView**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView)는 [`ColorViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) 인스턴스 100개를 만들고, 사용자가 3개 `Slider` 요소를 사용하여 각 색을 변경할 수 있도록 허용합니다. 또한 이 프로그램은 [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)에서 [`ColorToContrastColorConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs)를 사용합니다.

## <a name="listview-and-mvvm"></a>ListView 및 MVVM

`ListView`는 MVVM 시나리오에서 중요한 역할을 합니다. ViewModel에 `IEnumerable` 컬렉션이 있을 때마다 종종 `ListView`에 바인딩됩니다. 또한 컬렉션의 항목은 종종 템플릿에서 속성을 사용하여 바인딩할 `INotifyPropertyChanged`를 구현합니다.

### <a name="a-collection-of-viewmodels"></a>ViewModels 컬렉션

이 컬렉션을 살펴보기 위해 [**SchoolOfFineArts**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) 라이브러리는 [XML 데이터 파일](https://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml)을 기반으로 여러 클래스를 만들고 이 가상 학교의 가상 학생 이미지를 만듭니다.

[`Student`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs) 클래스는 [`ViewModelBase`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs)에서 파생됩니다. [`StudentBody`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs) 클래스는 `Student` 개체의 컬렉션이며 마찬가지로 `ViewModelBase`에서 파생됩니다. [`SchoolViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs)은 XML 파일을 다운로드하고 모든 개체를 어셈블합니다.

[**StudentList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList) 프로그램은 `ImageCell`을 사용하여 학생과 학생의 이미지를 `ListView`에 표시합니다.

[![학생 목록의 세 가지 스크린샷](images/ch19fg18-small.png "학생 목록")](images/ch19fg18-large.png#lightbox "학생 목록")

[**ListViewHeader**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader) 샘플은 [`Header`](xref:Xamarin.Forms.ListView.Header) 속성을 추가하지만 Android에만 표시됩니다.

### <a name="selection-and-the-binding-context"></a>선택 영역 및 바인딩 텍스트

[**SelectedStudentDetail**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail) 프로그램은 `StackLayout`의 `BindingContext`를 `ListView`의 `SelectedItem` 속성에 바인딩합니다. 이렇게 하면 선택된 학생에 대한 구체적인 정보를 프로그램에서 표시할 수 있습니다.

### <a name="context-menus"></a>상황에 맞는 메뉴

셀은 플랫폼별 방법으로 구현되는 바로 가기 메뉴를 정의할 수 있습니다. 이 메뉴를 만들려면 [`MenuItem`](xref:Xamarin.Forms.MenuItem) 개체를 `Cell`의 [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) 속성에 추가합니다.

`MenuItem`은 다음과 같은 5개 속성을 정의합니다.

- [`Text`](xref:Xamarin.Forms.MenuItem.Text)(`string` 형식)
- [`Icon`](xref:Xamarin.Forms.MenuItem.Icon)(`FileImageSource` 형식)
- [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive)(`bool` 형식)
- [`Command`](xref:Xamarin.Forms.MenuItem.Command)(`ICommand` 형식)
- [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter)(`object` 형식)

`Command` 및 `CommandParameter` 속성은 각 항목의 ViewModel에 원하는 메뉴 명령을 수행하는 메서드가 포함되어 있음을 의미합니다. 비-MVVM 시나리오에서 `MenuItem`은 [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked) 이벤트도 정의합니다.

[**CellContextMenu**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu)는 이 기술을 보여줍니다. 각 `MenuItem`의 `Command` 속성은 `Student` 클래스에 있는 `ICommand` 형식의 속성에 바인딩됩니다. `MenuItem`의 `IsDestructive` 속성을 선택한 개체를 제거 또는 삭제하는 `true`로 설정합니다.

### <a name="varying-the-visuals"></a>시각적 개체 변경

속성에 따라 `ListView`에 있는 항목의 시각적 개체를 약간 변경하고 싶은 경우가 있습니다. 예를 들어 학생의 평균 성적이 2.0 미만으로 떨어지면 [**ColorCodedStudents**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents) 샘플은 해당 학생 이름을 빨간색으로 표시합니다.
이 작업은 [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 바인딩 값 변환기 [`ThresholdToObjectConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs)를 사용하여 수행됩니다.

### <a name="refreshing-the-content"></a>콘텐츠 새로 고침

`ListView`는 데이터를 새로 고치는 풀 다운 제스처를 지원합니다. 이 제스처를 사용하려면 프로그램에서 [`IsPullToRefresh`](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) 속성을 `true`로 설정해야 합니다. `ListView`는 [`IsRefreshing`](xref:Xamarin.Forms.ListView.IsRefreshing) 속성을 `true`로 설정하고, [`Refreshing`](xref:Xamarin.Forms.ListView.Refreshing) 이벤트를 실행하고(MVVM 시나리오의 경우) [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand) 속성의 `Execute` 메서드를 호출하여 풀 다운 제스처에 응답합니다.

`Refresh` 이벤트 또는 `RefreshCommand`를 처리하는 코드는 `ListView`에서 표시하는 데이터를 업데이트하고 `IsRefreshing`을 다시 `false`로 설정합니다.

[**RssFeed**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed) 샘플은 데이터 바인딩을 위한 `RefreshCommand` 및 `IsRefreshing` 속성을 구현하는 [`RssFeedViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs)을 사용하는 방법을 보여줍니다.

## <a name="the-tableview-and-its-intents"></a>TableView 및 해당 의도

`ListView`는 일반적으로 동일한 형식의 여러 인스턴스를 표시하는 반면, [`TableView`](xref:Xamarin.Forms.TableView)는 일반적으로 다양한 형식의 여러 속성에 대한 사용자 인터페이스를 제공하는 데 집중합니다. 각 항목은 속성을 표시하거나 속성에 대한 사용자 인터페이스를 제공하기 위해 파생된 자체 [`Cell`](xref:Xamarin.Forms.Cell)과 연결됩니다.

### <a name="properties-and-hierarchies"></a>속성 및 계층 구조

`TableView`는 다음 네 가지 속성만 정의합니다.

- [`TableIntent`](xref:Xamarin.Forms.TableIntent) 형식의 [`Intent`](xref:Xamarin.Forms.TableView.Intent). 열거형
- [`TableRoot`](xref:Xamarin.Forms.TableRoot) 형식의 [`Root`](xref:Xamarin.Forms.TableView.Root). `TableView`의 콘텐츠 속성
- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight)(`int` 형식)
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows)(`bool` 형식)

`TableIntent` 열거형은 `TableView`를 어떻게 사용할 것인지 의도를 나타냅니다.

- [`Data`](xref:Xamarin.Forms.TableIntent.Data)
- [`Form`](xref:Xamarin.Forms.TableIntent.Form)
- [`Settings`](xref:Xamarin.Forms.TableIntent.Settings)
- [`Menu`](xref:Xamarin.Forms.TableIntent.Menu)

이러한 멤버는 `TableView`의 사용 방법도 제안합니다.

테이블을 정의할 때 다음과 같은 여러 가지 다른 클래스가 개입됩니다.

- [`TableSectionBase`](xref:Xamarin.Forms.TableSectionBase)는 `BindableObject`에서 파생되고 [`Title`](xref:Xamarin.Forms.TableSectionBase.Title) 속성을 정의하는 추상 클래스입니다.

- [`TableSectionBase<T>`](xref:Xamarin.Forms.TableSectionBase`1)는 `TableSectionBase`에서 파생되고 `IList<T>` 및 `INotifyCollectionChanged`를 구현하는 추상 클래스입니다.

- [`TableSection`](xref:Xamarin.Forms.TableSection)은 `TableSectionBase<Cell>`에서 파생됩니다.

- [`TableRoot`](xref:Xamarin.Forms.TableRoot)는 `TableSectionBase<TableSection>`에서 파생됩니다.

간단히 말해서, `TableView`에는 `TableRoot` 개체로 설정하는 `Root` 속성이 있습니다.이 속성은 `TableSection` 개체의 컬렉션이고, 각 개체는 `Cell` 개체의 컬렉션입니다. 테이블에는 여러 섹션이 있으며, 각 섹션에는 여러 셀이 있습니다. 테이블 자체는 제목을 가질 수 있으며, 각 섹션은 제목을 가질 수 있습니다. `TableView`는 `Cell` 파생 항목을 사용하지만 `DataTemplate`을 사용 하지는 않습니다.

### <a name="a-prosaic-form"></a>평범한 양식

[**EntryForm**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm) 샘플은 [`PersonalInformation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) 보기 모델을 정의합니다. 이 인스턴스는 `TableView`의 `BindingContext`가 됩니다. `TableSection`에서 파생된 각 `Cell`에는 `PersonalInformation` 클래스의 속성에 대한 바인딩이 있을 수 있습니다.

### <a name="custom-cells"></a>사용자 지정 셀

[**ConditionalCells**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells) 샘플은 **EntryForm**에서 확장됩니다. [`ProgrammerInformation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) 클래스에는 두 가지 추가 속성의 적용 가능성을 제어하는 부울 속성이 포함되어 있습니다. 이러한 두 가지 추가 속성과 관련하여, 프로그램에서는 [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 [PickerCell.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml) 및 [PickerCell.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs)를 기반으로 하는 사용자 지정 `PickerCell`을 사용합니다.

두 `PickerCell` 요소의 `IsEnabled` 속성이 `ProgrammerInformation`의 부울 속성에 바인딩되는 것으로 보아 이 기술이 작동하지 않는 것 같습니다. 따라서 다음 샘플을 살펴보겠습니다.

### <a name="conditional-sections"></a>조건부 섹션

[**ConditionalSection**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection) 샘플은 부울 항목의 선택에 대한 조건에 해당하는 두 항목을 별도의 `TableSection`에 배치합니다. 코드 숨김 파일은 `TableView`에서 이 섹션을 제거하거나 부울 속성을 기반으로 다시 추가합니다.

### <a name="a-tableview-menu"></a>TableView 메뉴

`TableView`의 또 다른 용도는 메뉴입니다. [**MenuCommands**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands) 샘플은 화면 주위에서 `BoxView`를 약간 이동할 수 있는 메뉴를 보여줍니다.

## <a name="related-links"></a>관련 링크

- [19장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [19장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [선택기](~/xamarin-forms/user-interface/picker/index.md)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
