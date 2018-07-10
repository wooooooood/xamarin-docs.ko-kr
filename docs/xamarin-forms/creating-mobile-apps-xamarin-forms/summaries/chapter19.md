---
title: 요약 19 장입니다. 컬렉션 뷰
description: 'Xamarin.Forms를 사용 하 여 모바일 앱 만들기: 19 장 요약 합니다. 컬렉션 뷰'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 0AEC3A5C-586E-4D0F-9895-67E99A053A79
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 7ae6ff5bb08977ab83f95242770794b4c4363145
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935175"
---
# <a name="summary-of-chapter-19-collection-views"></a>요약 19 장입니다. 컬렉션 뷰

Xamarin.Forms는 컬렉션을 유지 관리 하 고 해당 요소를 표시 하는 세 가지 뷰를 정의 합니다.

- [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 문자열 항목 하나를 선택할 수 있는 비교적 짧은 목록
- [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 일반적으로 동일한 유형의 항목의 긴 목록을 경우가, 서식도 하나를 선택할 수 있도록 허용
- [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) 컬렉션인 *셀* (일반적으로 다양 한 형식 및 모양을)의 데이터를 표시 하거나 사용자 입력을 관리

일반적으로 MVVM 응용 프로그램이 사용 하는 `ListView` 선택할 수 있는 개체의 컬렉션을 표시 하 합니다.

## <a name="program-options-with-picker"></a>선택기를 사용 하 여 프로그램 옵션

합니다 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 사용자는 비교적 짧은 목록 중에서 원하는 옵션을 선택 하도록 허용 해야 할 때 적합 한 선택은 `string` 항목입니다.

### <a name="the-picker-and-event-handling"></a>선택 및 이벤트 처리

[ **PickerDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo) 샘플 XAML을 사용 하 여 설정 하는 방법에 설명 합니다 `Picker` [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) 속성 추가 `string` 합니다 항목[ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) 컬렉션입니다. 사용자가 선택 하는 경우는 `Picker`에 있는 항목 표시를 `Items` 플랫폼에 종속 된 방식으로 컬렉션.

합니다 [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) 이벤트는 사용자가 항목을 선택 하는 경우를 나타냅니다. 0부터 시작 [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) 속성 다음 선택한 항목을 나타냅니다. 선택 된 항목이 면 `SelectedIndex` equals &ndash;1입니다.

사용할 수도 있습니다 `SelectedIndex` 있지만 선택한 항목을 초기화 하려면 후 설정 되어야 합니다는 `Items` 컬렉션 채워집니다. 사용할 것 속성 요소를 설정 하려면 즉, XAML에서 `SelectedIndex`합니다.

### <a name="data-binding-the-picker"></a>데이터 바인딩 선택기

`SelectedIndex` 바인딩 가능한 속성으로 속성을 지원 하지만 `Items` 사용 하 여 데이터 바인딩을 사용 하 여 되지 않습니다는 `Picker` 어렵습니다. 하나의 솔루션은 사용 하는 `Picker` 와 함께에서 [ `ObjectToIndexConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs) 같은 합니다 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리. 합니다 [ **PickerBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding) 작동 방식을 보여 줍니다.

## <a name="rendering-data-with-listview"></a>ListView 사용 하 여 데이터를 렌더링합니다.

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 에서 파생 되는 유일한 클래스 [ `ItemsView<TVisual>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) 상속 된 합니다 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) 고 [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) 속성입니다.

`ItemsSource` 유형의 `IEnumerable` 이지만 `null` 기본적으로 명시적으로 초기화 하거나 컬렉션 데이터 바인딩을 통해 (자주)로 수 해야 합니다. 이 컬렉션의 항목은 모든 형식일 수 있습니다.

`ListView` 정의 [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) 중 하나는 속성의 항목 중 하나로 설정 합니다 `ItemsSource` 컬렉션 또는 `null` 선택 된 항목이 있는 경우. `ListView` 발생 합니다 [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) 이벤트는 새 항목을 선택 합니다.

### <a name="collections-and-selections"></a>컬렉션 및 선택

[ **ListViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList) 채우기 샘플을 `ListView` 17을 사용 하 여 `Color` 값을 `List<Color>` 컬렉션입니다. 항목을 선택할 수 있지만 기본적으로 표시 됩니다는 이상한 `ToString` 표현 합니다. 이 장에서 몇 가지 예제를 표시 하는 수정으로 유용한 필요에 따라 하는 방법을 보여 줍니다.

### <a name="the-row-separator"></a>행 구분 기호

IOS 및 Android 표시 얇은 선 행을 구분합니다. 이를 제어할 수 있습니다.는 [ `SeparatorVisibiliy` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorVisibility/) 하 고 [ `SeparatorColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorColor/) 속성입니다. `SeparatorVisibility` 형식 속성은 [ `SeparatorVisbility` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SeparatorVisibility/), 두 멤버로 구성 된 열거형:

- [`Default`](xref:Xamarin.Forms.SeparatorVisibility.Default)를 기본 설정
- [`None`](xref:Xamarin.Forms.SeparatorVisibility.None)

### <a name="data-binding-the-selected-item"></a>데이터 바인딩 선택된 된 항목

`SelectedItem` 원본 또는 대상 데이터 바인딩을 사용할 수 있도록 바인딩 가능한 속성으로 속성을 지원 합니다. 기본값으로 `BindingMode` 는 `OneWayToSource`, 이지만 일반적으로 MVVM 시나리오에서 특히 양방향 데이터 바인딩의 대상입니다. 합니다 [ **ListViewArray** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray) 샘플에서는이 유형의 바인딩 보여 줍니다.

### <a name="the-observablecollection-difference"></a>ObservableCollection 차이

[ **ListViewLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger) 집합 샘플를 `ItemsSource` 의 속성을 `ListView` 에 `List<DateTime>` 컬렉션 점진적으로 추가한 다음 새 `DateTime` 개체를 컬렉션에 모든 타이머를 사용 하 여 두 번째입니다.

그러나 합니다 `ListView` 하지 자동 업데이트 때문에 `List<T>` 컬렉션에 항목이 추가 되거나 컬렉션에서 제거할 경우를 나타내기 위해 알림 메커니즘을 포함 하지 않습니다.

이러한 시나리오에서 사용 하는 훨씬 더 나은 클래스 [ `ObservableCollection<T>` ](https://developer.xamarin.com/api/type/System.Collections.ObjectModel.ObservableCollection%3CT%3E/) 에 정의 된 `System.Collections.ObjectModel` 네임 스페이스입니다. 이 클래스를 구현 합니다 [ `INotifyCollectionChanged` ](https://developer.xamarin.com/api/type/System.Collections.Specialized.INotifyCollectionChanged/) 인터페이스 및 결과적으로 발생을 [ `CollectionChanged` ](https://developer.xamarin.com/api/event/System.Collections.ObjectModel.ObservableCollection%3CT%3E.CollectionChanged/) 이벤트 항목을 추가 하거나 바꾸거나 내에서 이동 하는 경우 또는 컬렉션에서를 제거할 때 컬렉션입니다. 경우는 `ListView` 내부적으로 감지 하 구현 하는 클래스 `INotifyCollectionChanged` 로 설정 된 해당 `ItemsSource` 처리기를 연결 속성을는 `CollectionChanged` 이벤트는 컬렉션이 변경 될 때 해당 디스플레이 업데이트 하 고 합니다.

합니다 [ **ObservableLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger) 샘플의 사용법을 보여 줍니다. `ObservableCollection`합니다.

### <a name="templates-and-cells"></a>템플릿 및 셀

기본적으로 `ListView` 각 항목을 사용 하 여 해당 컬렉션에 항목을 표시 `ToString` 메서드. 더 나은 방법은 항목을 표시 하는 템플릿 정의 포함 합니다.

이 기능을 사용 하 여 실험에 사용할 수 있습니다 합니다 [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) 클래스를 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리입니다. 이 클래스 정의 정적 `All` 형식의 속성 `IList<NamedColor>` 141를 포함 하는 `NamedColor` 개체의 public 필드에 해당 하는 `Color` 구조입니다.

[ **NaiveNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList) 샘플 집합을 `ItemsSource` 의 `ListView` 이 `NamedColor.All` 속성이 아니라의 정규화 된 클래스 이름만 `NamedColor` 개체는 표시 됩니다.

`ListView` 이러한 항목을 표시 하는 템플릿은 필요 합니다. 코드에서 설정할 수 있습니다는 [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) 정의한 속성 `ItemsView<TVisual>` 에 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 사용 하 여 개체를 [ `DataTemplate` 생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/) 는 파생 항목을 참조 합니다 [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) 클래스입니다. `Cell` 5 파생형에 있습니다.

- [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) &mdash; 에 두 개의 `Label` (개념적) 보기
- [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/) &mdash; 추가 `Image` 보고 `TextCell`
- [`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) &mdash; 포함을 `Entry` 와 보기를 `Label`
- [`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/) &mdash; 포함 된 `Switch` 으로 `Label`
- [`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) &mdash; 일 수 있습니다 `View` (자식이 가능성이 거의)

다음 호출 [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) 및 [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) 에 `DataTemplate` 사용 하 여 값을 연결할 개체를 `Cell` 속성을 또는 데이터 바인딩을 설정 하는 `Cell` 속성에 있는 항목의 속성 참조는 `ItemsSource` 컬렉션입니다. 에 설명 되어이 [ **TextCellListCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode) 샘플입니다.

각 항목은 표시 되는 `ListView`작은 시각적 트리는 템플릿에서 생성 되 고 데이터 바인딩을이 시각적 트리 요소의 속성과 항목 간에 설정 됩니다. 에 대 한 처리기를 설치 하 여이 프로세스는 아이디어를 얻을 수 있습니다는 [ `ItemAppearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemAppearing/) 하 고 [ `ItemDisappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemDisappearing/) 의 이벤트를 `ListView`, 또는 대안을 사용 하 여 [ `DataTemplate`생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Func%7BSystem.Object%7D/) 항목의 시각적 트리를 만들어야 할 때마다 호출 되는 함수를 사용 하는 합니다.

합니다 [ **TextCellListXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml) XAML에서 완전히 기능적으로 동일한 프로그램을 보여 줍니다. `DataTemplate` 로 설정 된 태그를 `ItemTemplate` 의 속성을 `ListView`, 차례로 `TextCell` 로 설정 되어를 `DataTemplate`. 바인딩 컬렉션에 있는 항목의 속성에 직접 설정 됩니다는 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Text/) 및 [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Detail/) 의 속성을 `TextCell`입니다.

### <a name="custom-cells"></a>사용자 지정 셀

XAML에서 설정할 수는 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) 에 `DataTemplate` 다음으로 사용자 지정 시각적 트리를 정의 합니다 [ `View` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ViewCell.View/) 의 속성 `ViewCell`. (`View` 의 콘텐츠 속성인 `ViewCell` 하므로 `ViewCell.View` 태그는 필요 하지 않습니다.) 합니다 [ **CustomNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList) 샘플에는이 기술을 보여 줍니다.

[![사용자 지정 명명 된 색 목록에 삼중 스크린샷](images/ch19fg11-small.png "명명 된 색 목록 사용자 지정")](images/ch19fg11-large.png#lightbox "명명 된 색 목록 사용자 지정")

모든 플랫폼에 대 한 오른쪽 크기 조정 하기는 까다로울 수 있습니다. [ `RowHeight` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RowHeight/) 속성은 유용 하지만 일부 경우에서에 사용 하려는 [ `HasUnevenRows` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.HasUnevenRows/) 속성 효율성이 떨어지기는 하지만 강제로 `ListView` 행 크기입니다. IOS 및 Android에 대 한 적절 한 행 크기를 가져오려면 이러한 두 속성 중 하나 사용 해야 합니다.

### <a name="grouping-the-listview-items"></a>ListView 항목 그룹화

`ListView` 항목을 그룹화 하 고 해당 그룹 탐색을 지원 합니다. `ItemsSource` 컬렉션의 컬렉션에 속성을 설정 해야: 개체는 `ItemsSource` 해야로 설정 되어 구현 `IEnumerable`, 컬렉션의 각 항목 속성도 구현 해야 하 고 `IEnumerable`입니다. 각 그룹에는 두 가지 속성이 포함 되어야 합니다. 3 자 약어 및 그룹에 대 한 텍스트 설명을입니다.

합니다 [ `NamedColorGroup` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs) 클래스를 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리에는 7 개의 그룹을 만들어 `NamedColor` 개체입니다. [ **ColorGroupList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList) 샘플에 사용 하 여 이러한 그룹을 사용 하는 방법을 보여 줍니다는 [ `IsGroupingEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsGroupingEnabled/) 속성 `ListView` 로 `true`, 및를 [ `GroupDisplayBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupDisplayBinding/) 하 고 [ `GroupShortNameBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupShortNameBinding/) 속성 각 그룹의 속성에 바인딩됩니다.

### <a name="custom-group-headers"></a>사용자 지정 그룹 헤더

에 대 한 사용자 지정 헤더를 만들 수 있기를 `ListView` 대체 하 여 그룹을 `GroupDisplayBinding` 속성과 [ `GroupHeaderTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupHeaderTemplate/) 헤더에 대 한 템플릿을 정의 합니다.

### <a name="listview-and-interactivity"></a>ListView 및 상호 작용

일반적으로 응용 프로그램 사용자 상호 작용을 가져옵니다를 `ListView` 처리기를 연결 하 여 합니다 `ItemSelected` 또는 [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) 이벤트 또는에서 데이터 바인딩을 설정 하 여는 `SelectedItem` 속성입니다. 하지만 일부 셀 형식 (`EntryCell` 및 `SwitchCell`) 사용자 상호 작용을 허용 하며 이기도 사용자와 상호 작용 하는 사용자 지정 셀을 만들 수 있습니다. 합니다 [ **InteractiveListView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView) 인스턴스가 100 개인 만듭니다 [ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) 의 도움을 받을 사용 하 여 각 색을 변경 하 고 사용자 `Slider` 요소입니다. 프로그램 활용 합니다 [ `ColorToContrastColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs) 에 [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)합니다.

## <a name="listview-and-mvvm"></a>ListView 및 MVVM

`ListView` MVVM 시나리오에서 큰 역할을 담당 합니다. 때마다를 `IEnumerable` ViewModel에 있는 컬렉션, 종종에 바인딩되어는 `ListView`합니다. 또한 종종 컬렉션의 항목 구현 `INotifyPropertyChanged` 템플릿에서 속성을 사용 하 여 바인딩할 합니다.

### <a name="a-collection-of-viewmodels"></a>Viewmodel의 컬렉션

이 탐색 하는 [ **SchoolOfFineArts** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) 라이브러리를 기반으로 하는 여러 클래스를 만듭니다를 [XML 데이터 파일](http://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml) 및 가상의 학생 들이 가상의 학교에서의 이미지입니다.

합니다 [ `Student` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs) 클래스에서 파생 됩니다 [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs)합니다. 합니다 [ `StudentBody` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs) 클래스의 컬렉션인 `Student` 개체에서 파생 되 고 `ViewModelBase`입니다. 합니다 [ `SchoolViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) XML 파일을 다운로드 하 고 모든 개체를 어셈블합니다.

합니다 [ **StudentList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList) 사용 하 여 프로그램을 `ImageCell` 학생 및 해당 이미지를 표시 하는 `ListView`:

[![학생 목록에 삼중 스크린샷](images/ch19fg18-small.png "학생 목록")](images/ch19fg18-large.png#lightbox "학생 목록")

[ **ListViewHeader** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader) 추가 하는 샘플을 [ `Header` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.Header/) 속성 하지만 나타나는데 Android에서.

### <a name="selection-and-the-binding-context"></a>선택 및 바인딩 컨텍스트

[ **SelectedStudentDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail) 바인딩합니다 프로그램를 `BindingContext` 의 `StackLayout` 에 `SelectedItem` 속성은 `ListView`합니다. 따라서 프로그램을 선택한 학생에 대 한 자세한 정보를 표시할 수 있습니다.

### <a name="context-menus"></a>상황에 맞는 메뉴

셀은 플랫폼 특정 방식으로 구현 되는 상황에 맞는 메뉴를 정의할 수 있습니다. 이 메뉴를 만들려면 추가 [ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) 개체를 합니다 [ `ContextActions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Cell.ContextActions/) 속성을 `Cell`입니다.

`MenuItem` 5 가지 속성을 정의합니다.

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) 형식의 `string`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) 형식의 `FileImageSource`
- [`IsDestructive`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.IsDestructive/) 형식의 `bool`
- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Command/) 형식의 `ICommand`
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.CommandParameter/) 형식의 `object`

합니다 `Command` 및 `CommandParameter` 속성 의미 각 항목에 대 한 ViewModel 원하는 메뉴 명령을 수행 하는 메서드를 포함 합니다. MVVM이 아닌 시나리오 `MenuItem` 정의 [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/) 이벤트입니다.

합니다 [ **CellContextMenu** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu) 이 기술을 보여 줍니다. `Command` 의 각 속성 `MenuItem` 형식의 속성에 바인딩된 `ICommand` 에 `Student` 클래스입니다. 설정 된 `IsDestructive` 속성을 `true` 에 대 한는 `MenuItem` 제거 하거나 선택한 개체를 삭제 하는 합니다.

### <a name="varying-the-visuals"></a>시각적 개체를 변경합니다.

하려는 경우가 있습니다 항목의 시각적 개체에 약간 변형 된 `ListView` 속성을 기준으로 합니다. 예를 들어 경우 학생의 성적 점수 평균 미만 2.0 합니다 [ **ColorCodedStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents) 샘플 빨간색에서 해당 학생의 이름을 표시 합니다.
바인딩 값 변환기를 사용 하 여 이렇게 [ `ThresholdToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs)는 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리입니다.

### <a name="refreshing-the-content"></a>콘텐츠 새로 고침

`ListView` 해당 데이터 새로 고침에 대 한 풀 다운 제스처를 지원 합니다. 프로그램을 설정 해야 합니다는 [ `IsPullToRefresh` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsPullToRefreshEnabled/) 속성을 `true` 이 사용 하도록 설정 합니다. `ListView` 풀다운을 제스처를 설정 하 여 응답 해당 [ `IsRefreshing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsRefreshing/) 속성을 `true`, 및 시켜 합니다 [ `Refreshing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.Refreshing/) 이벤트 및 (MVVM 시나리오) 호출 합니다 `Execute` 메서드의 해당 [ `RefreshCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RefreshCommand/) 속성입니다.

처리 코드를 `Refresh` 이벤트 또는 `RefreshCommand` 다음에 표시 된 데이터를 가능한 업데이트를 `ListView` 설정 `IsRefreshing` 돌아가기 `false`합니다.

합니다 [ **RssFeed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed) 샘플을 사용 하 여 보여 줍니다는 [ `RssFeedViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs) 구현 하는 `RefreshCommand` 및 `IsRefreshing` 데이터 바인딩에 대 한 속성입니다.

## <a name="the-tableview-and-its-intents"></a>TableView 하 고 해당 의도

하는 동안 합니다 `ListView` 일반적으로 동일한 형식의 여러 인스턴스를 표시 합니다 [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) 다양 한 종류의 여러 속성에 대 한 사용자 인터페이스를 제공 하는 일반적으로 포커스가 합니다. 각 항목은 연결 된 고유한 [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) 파생 속성을 표시 또는 사용자 인터페이스를 제공 합니다.

### <a name="properties-and-hierarchies"></a>속성 및 계층 구조

`TableView` 4 개의 속성을 정의합니다.

- [`Intent`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Intent/) 형식의 [ `TableIntent` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableIntent/), 열거형
- [`Root`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Root/) 형식의 [ `TableRoot` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/)의 content 속성 `TableView`
- [`RowHeight`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.RowHeight/) 형식의 `int`
- [`HasUnevenRows`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.HasUnevenRows/) 형식의 `bool`

합니다 `TableIntent` 열거형을 사용 하는 방식 나타냅니다는 `TableView`:

- [`Data`](xref:Xamarin.Forms.TableIntent.Data)
- [`Form`](xref:Xamarin.Forms.TableIntent.Form)
- [`Settings`](xref:Xamarin.Forms.TableIntent.Settings)
- [`Menu`](xref:Xamarin.Forms.TableIntent.Menu)

이러한 멤버에 대 한 몇 가지 사용을 제안할 수도 `TableView`합니다.

다른 몇 가지 클래스는 테이블 정의에 관련 됩니다.

- [`TableSectionBase`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase/) 파생 되는 추상 클래스 `BindableObject` 정의 된 [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TableSectionBase.Title/) 속성

- [`TableSectionBase<T>`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase%3CT%3E/) 파생 되는 추상 클래스 `TableSectionBase` 구현 및 `IList<T>` 및 `INotifyCollectionChanged`

- [`TableSection`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSection/) 파생 `TableSectionBase<Cell>`

- [`TableRoot`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/) 파생 `TableSectionBase<TableSection>`

즉, `TableView` 에 `Root` 를 설정 하는 속성을 `TableRoot` 개체 컬렉션인의 `TableSection` 개체의 컬렉션은 각각 `Cell` 개체. 테이블 섹션이 여러 개 있고 각 섹션에 여러 셀입니다. 테이블 자체를 지정할 수 제목을 있으며 각 섹션에는 제목이 있을 수 있습니다. 하지만 `TableView` 이용 `Cell` 파생형을 수행 하지 사용 `DataTemplate`합니다.

### <a name="a-prosaic-form"></a>사용할 수 있는 폼

[ **EntryForm** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm) 샘플 정의 [ `PersonalInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) 보기 모델에는 인스턴스는 `BindingContext` 의 `TableView`합니다. 각 `Cell` 에서 파생 해당 `TableSection` 의 속성에 대 한 바인딩을 가질 수는 `PersonalInformation` 클래스입니다.

### <a name="custom-cells"></a>사용자 지정 셀

합니다 [ **ConditionalCells** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells) 샘플을 확장 **EntryForm**합니다. 합니다 [ `ProgrammerInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) 클래스 두 추가 속성에 적용할 수 있는지를 제어 하는 부울 속성을 포함 합니다. 이러한 두 추가 속성에 대 한 프로그램에서는 사용자 지정 `PickerCell` 기반을 [PickerCell.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml) 및 [PickerCell.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs) 에 [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리입니다.

있지만 `IsEnabled` 두 속성 `PickerCell` 요소에서 부울 속성에 바인딩된 `ProgrammerInformation`,이 기술은 하지 않는 것을 작업 하려면 라는 다음 샘플 메시지를 표시 합니다.

### <a name="conditional-sections"></a>조건부 섹션

합니다 [ **ConditionalSection** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection) 샘플은 별도의 부울 항목의 선택 영역에 대해 조건부는 두 항목을 배치 `TableSection`합니다. 코드 숨김 파일에서이 섹션을 제거 합니다 `TableView` 또는 부울 속성에 따라 다시 추가 합니다.

### <a name="a-tableview-menu"></a>TableView 메뉴

또 다른 용도 `TableView` 메뉴입니다. 합니다 [ **Menucommand** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands) 샘플 조금 이동할 수 있는 메뉴를 보여 줍니다. `BoxView` 화면.



## <a name="related-links"></a>관련 링크

- [19 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [19 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
