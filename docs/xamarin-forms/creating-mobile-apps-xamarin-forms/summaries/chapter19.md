---
title: "19 장의 요약입니다. 컬렉션 뷰"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 37afa3a54fd20745a65312fb5a24d958c8ec405f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-19-collection-views"></a>19 장의 요약입니다. 컬렉션 뷰

Xamarin.Forms는 컬렉션을 유지 관리 하 고 해당 요소를 표시 하는 세 가지 뷰를 정의 합니다.

- [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 사용자가 하나를 선택할 수 있는 문자열 항목의 상대적으로 짧은 목록
- [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 일반적으로 동일한 형식의 항목 목록이 긴 수 있으며 서식도 하나를 선택할 수 있도록 허용
- [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) 컬렉션인 *셀* (일반적으로 다양 한 유형과의 모양을) 데이터를 표시 하거나 사용자 입력을 관리 하려면

일반적으로 사용 하도록 MVVM 응용 프로그램의 `ListView` 개체의 선택 가능한 컬렉션을 표시 합니다.

## <a name="program-options-with-picker"></a>선택 기가 된 프로그램 옵션

[ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 사용자 비교적 짧은 목록에서 옵션을 선택 하도록 허용 해야 할 때이 좋은 대안 `string` 항목입니다.

### <a name="the-picker-and-event-handling"></a>선택 및 이벤트 처리

[ **PickerDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo) 샘플에서는 XAML을 사용 하 여 설정 하는 방법을 보여 줍니다.는 `Picker` [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) 속성 추가 `string` 는 항목[ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) 컬렉션입니다. 사용자가 선택할 때는 `Picker`에 있는 항목 표시는 `Items` 플랫폼별 방식으로 컬렉션입니다.

[ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) 이벤트는 사용자가 항목을 선택 하는 경우를 나타냅니다. 0부터 시작 [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) 속성 다음 선택된 항목을 나타냅니다. 선택 된 항목이 경우 `SelectedIndex` equals & #x 2013; 1.

사용할 수도 있습니다 `SelectedIndex` 하지만 선택한 항목을 초기화 하려면 다음 설정 되어야 합니다는 `Items` 컬렉션 채워집니다. 사용할 수 있다고 속성 요소를 설정 하려면 즉 XAML에서 `SelectedIndex`합니다.

### <a name="data-binding-the-picker"></a>데이터는 선택기 바인딩

`SelectedIndex` 속성 바인딩 가능한 속성에 의해 지원 됩니다 하지만 `Items` 는 사용 되지 않는 사용 데이터 바인딩을 사용 하 여 한 `Picker` 어렵습니다. 한 가지 해결 방법은 사용 하는 `Picker` 함께에서 [ `ObjectToIndexConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs) 에 같은 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리입니다. [ **PickerBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding) 이 과정을 보여 줍니다.

## <a name="rendering-data-with-listview"></a>ListView 사용 하 여 데이터를 렌더링합니다.

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 에서 파생 된 클래스만 [ `ItemsView<TVisual>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) 상속 된는 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) 및 [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) 속성입니다.

`ItemsSource` 형식의 `IEnumerable` 이지만 `null` 기본적으로 명시적으로 초기화 하거나 데이터 바인딩을 통해 컬렉션으로 설정 (자주) 수 해야 합니다. 이 컬렉션의 항목은 모든 유형일 수 있습니다.

`ListView` 정의 [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) 는 속성에 있는 항목 중 하나로 설정 된 `ItemsSource` 컬렉션 또는 `null` 선택 된 항목이 없는 경우. `ListView` 발생 된 [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) 이벤트 새 항목을 선택 합니다.

### <a name="collections-and-selections"></a>컬렉션 및 선택

[ **ListViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList) 채우기 예제는 `ListView` 17와 `Color` 값에 `List<Color>` 컬렉션입니다. 항목을 선택할 수 있지만 기본적으로이 표시 됩니다는 이상한 `ToString` 표현 합니다. 이 장에서 몇 가지 예제에 표시 하는 수정 하 고 원하는 대로 매력적인으로 확인 하는 방법을 보여 줍니다.

### <a name="the-row-separator"></a>행 구분 기호

IOS 및 Android 디스플레이에서 얇은 선 행을 구분합니다. 이를 제어할 수 있습니다는 [ `SeparatorVisibiliy` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorVisibility/) 및 [ `SeparatorColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorColor/) 속성입니다. `SeparatorVisibility` 속성은 형식이 [ `SeparatorVisbility` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SeparatorVisibility/), 두 멤버가 포함 된 열거:

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.SeparatorVisibility.Default/)를 기본 설정
- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.SeparatorVisibility.None/)

### <a name="data-binding-the-selected-item"></a>데이터 바인딩 선택된 된 항목

`SelectedItem` 원본 또는 대상 데이터 바인딩을 사용할 수 있도록 속성을 바인딩할 수 있는 속성에 의해 지원 됩니다. 기본 `BindingMode` 은 `OneWayToSource`, 하지만 일반적으로 양방향 데이터 바인딩 MVVM 시나리오에서 특히의 대상이 됩니다. [ **ListViewArray** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray) 샘플은이 유형의 바인딩 보여 줍니다.

### <a name="the-observablecollection-difference"></a>ObservableCollection 차이

[ **ListViewLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger) 집합 예제는 `ItemsSource` 속성의는 `ListView` 에 `List<DateTime>` 컬렉션 다음 점진적으로 추가 새 `DateTime` 컬렉션 개체를 모든 타이머를 사용 하 여 두 번째입니다.

그러나는 `ListView` 하지 않는 자동으로 자동으로 업데이트 하기 때문에 `List<T>` 컬렉션 항목은에 추가 또는 컬렉션에서 제거 하는 경우를 나타내기 위해 알림 메커니즘에 없는 합니다.

이러한 시나리오에서 사용 하는 훨씬 더 나은 클래스는 [ `ObservableCollection<T>` ](https://developer.xamarin.com/api/type/System.Collections.ObjectModel.ObservableCollection%3CT%3E/) 에 정의 된는 `System.Collections.ObjectModel` 네임 스페이스입니다. 이 클래스가 구현 하는 [ `INotifyCollectionChanged` ](https://developer.xamarin.com/api/type/System.Collections.Specialized.INotifyCollectionChanged/) 인터페이스와 비슷하게 발생 한 [ `CollectionChanged` ](https://developer.xamarin.com/api/event/System.Collections.ObjectModel.ObservableCollection%3CT%3E.CollectionChanged/) 항목이 추가 되거나 바꾸거나 내에서 이동 하는 경우 또는 컬렉션에서 제거 하는 경우 이벤트 컬렉션입니다. 때는 `ListView` 내부적으로 검색 하는 구현 하는 클래스 `INotifyCollectionChanged` 가로 설정 된 해당 `ItemsSource` 처리기를 연결 속성을는 `CollectionChanged` 이벤트는 컬렉션이 변경 될 때 해당 디스플레이 업데이트 합니다.

[ **ObservableLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger) 샘플의 사용법을 보여줍니다 `ObservableCollection`합니다.

### <a name="templates-and-cells"></a>서식 파일 및 셀

기본적으로는 `ListView` 각 항목을 사용 하 여 해당 컬렉션에 항목을 표시 `ToString` 메서드. 더 나은 방법은 항목을 표시 하는 서식 파일을 정의가 포함 됩니다.

이 기능을 테스트 하려면 사용할 수 있습니다는 [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) 클래스에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리입니다. 이 클래스를 정의 하는 정적 `All` 형식의 속성 `IList<NamedColor>` 141 포함 된 `NamedColor` 개체의 public 필드에 해당 하는 `Color` 구조입니다.

[ **NaiveNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList) 집합 예제는 `ItemsSource` 의 `ListView` 이 `NamedColor.All` 속성이 아니라의 정규화 된 클래스 이름에만 `NamedColor` 개체는 표시 합니다.

`ListView` 이러한 항목을 표시 하는 템플릿이 필요 합니다. 코드에서 설정할 수는 [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) 에 정의 된 속성 `ItemsView<TVisual>` 에 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 를 사용 하 여 개체는 [ `DataTemplate` 생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/) 입니다 참조의 파생 클래스는 [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) 클래스입니다. `Cell` 5 개의 파생 항목에 있습니다.

- [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) & #x 2014; 에 두 개의 `Label` (개념적) 뷰
- [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/) & #x 2014; 추가 `Image` 를 보려면 `TextCell`
- [`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) & #x 2014; 포함 된 `Entry` 포함 된 뷰는 `Label`
- [`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/) & #x 2014; 포함 된 `Switch` 으로 `Label`
- [`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) & #x 2014; 하나일 수 있습니다 `View` (자식이 가능성이 거의)

그런 다음 호출 [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) 및 [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) 에 `DataTemplate` 사용 하 여 값을 연결할 개체는 `Cell` 속성 또는에서 데이터 바인딩을 설정 하는 `Cell` 속성에 있는 항목의 속성을 참조 하는 `ItemsSource` 컬렉션입니다. 이 확인할는 [ **TextCellListCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode) 샘플.

각 항목은 표시 되는 `ListView`, 작은 시각적 트리는 템플릿에서 생성 되 고 데이터 바인딩을이 시각적 트리에 있는 항목의 속성 및 해당 요소 간에 설정 됩니다. 에 대 한 처리기를 설치 하 여이 프로세스의 아이디어를 얻을 수 있습니다는 [ `ItemAppearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemAppearing/) 및 [ `ItemDisappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemDisappearing/) 의 이벤트는 `ListView`, 대체를 사용 하 여 또는 [ `DataTemplate`생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Func%7BSystem.Object%7D/) 항목의 시각적 트리를 만들어야 할 때마다 호출 되는 함수를 사용 하 합니다.

[ **TextCellListXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml) 기능적으로 동일한 프로그램 XAML을 보여 줍니다. A `DataTemplate` 태그로 설정 되어는 `ItemTemplate` 속성의는 `ListView`, 한 다음은 `TextCell` 로 설정 되는 `DataTemplate`합니다. 컬렉션에 항목의 속성에 대 한 바인딩을 직접에 설정 된는 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Text/) 및 [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Detail/) 의 속성은 `TextCell`합니다.

### <a name="custom-cells"></a>사용자 지정 셀

XAML에서 설정할 수는 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) 에 `DataTemplate` 다음으로 사용자 지정 시각적 트리를 정의 하 고는 [ `View` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ViewCell.View/) 속성 `ViewCell`합니다. (`View` 의 content 속성은 `ViewCell` 하므로 `ViewCell.View` 태그 사용할 필요가 없습니다.) [ **CustomNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList) 샘플에는이 기술을 보여 줍니다.

[![명명 된 색 목록을 사용자 지정의 삼중 스크린 샷](images/ch19fg11-small.png "사용자 지정 명명 된 색 목록")](images/ch19fg11-large.png "명명 된 색 목록을 사용자 지정")

가져오기는 모든 플랫폼 권한에 대 한 크기 조정 하는 것은 까다로울 수 있습니다. [ `RowHeight` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RowHeight/) 속성이 유용 하지만 경우에 따라 적용 해야는 [ `HasUnevenRows` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.HasUnevenRows/) 덜 효율적인 속성이 있지만 강제로 `ListView` 행 크기를 조정 합니다. IOS 및 Android에 대 한 하나를 사용 해야 이러한 두 속성의 적절 한 행 크기 조정 얻으려고 합니다.

### <a name="grouping-the-listview-items"></a>ListView 항목 그룹화

`ListView` 항목을 그룹화 하 고 해당 그룹 사이의 탐색을 지원 합니다. `ItemsSource` 속성 컬렉션의 컬렉션으로 설정 되어 있어야: 개체를 `ItemsSource` 해야로 설정 된 구현 `IEnumerable`, 컬렉션의 각 항목이 구현 해야 하 고 `IEnumerable`합니다. 각 그룹에는 두 개의 속성이 포함 되어야 합니다. 3 자 약어 및 그룹에 대 한 텍스트 설명입니다.

[ `NamedColorGroup` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs) 클래스에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리 7 개의 그룹을 만들어 `NamedColor` 개체입니다. [ **ColorGroupList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList) 샘플에서는 이러한 그룹을 사용 하는 방법을 보여 줍니다.는 [ `IsGroupingEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsGroupingEnabled/) 속성 `ListView` 로 설정 `true`, 및는 [ `GroupDisplayBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupDisplayBinding/) 및 [ `GroupShortNameBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupShortNameBinding/) 각 그룹의 속성에 바인딩된 속성입니다.

### <a name="custom-group-headers"></a>사용자 지정 그룹 머리글의 경우

에 대 한 사용자 지정 헤더를 만들 수는 `ListView` 대체 하 여 그룹의 `GroupDisplayBinding` 속성을는 [ `GroupHeaderTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupHeaderTemplate/) 헤더에 대 한 템플릿을 정의 합니다.

### <a name="listview-and-interactivity"></a>ListView 및 상호 작용

응용 프로그램이 사용자와 상호 작용을 가져오면 일반적으로 `ListView` 처리기를 연결 하 여는 `ItemSelected` 또는 [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) 이벤트를 또는 대해 데이터 바인딩을 설정 하 여는 `SelectedItem` 속성입니다. 하지만 일부 셀 형식 (`EntryCell` 및 `SwitchCell`) 사용자 상호 작용을 허용 하며도 가능한 해당 자체 사용자 지정 셀을 만드는 데 사용자 상호 작용 합니다. [ **InteractiveListView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView) 의 100 인스턴스를 만드는 [ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) 있고 사용자의 도움을 받을 사용 하 여 각 색을 변경할 수 있도록 `Slider` 요소입니다. 프로그램 또한를 사용 하 여 [ `ColorToContrastColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs) 에 [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)합니다.

## <a name="listview-and-mvvm"></a>ListView 및 MVVM

`ListView` MVVM 시나리오에서 큰 역할을 수행합니다. 때마다는 `IEnumerable` 는 ViewModel에 컬렉션이 존재에 바인딩된 종종는 `ListView`합니다. 또한 종종 컬렉션의 항목이 구현 `INotifyPropertyChanged` 서식 파일의 속성을 가진 바인딩할 합니다.

### <a name="a-collection-of-viewmodels"></a>Viewmodel의 컬렉션

이 탐색 하는 [ **SchoolOfFineArts** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) 라이브러리에 따라 몇 가지 클래스를 만듭니다.는 [XML 데이터 파일](http://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml) 및이 가상의 학교 가상의 학생의 이미지입니다.

[ `Student` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs) 클래스에서 파생 [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs)합니다. [ `StudentBody` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs) 클래스는 컬렉션의 `Student` 개체에서 파생 되 고 `ViewModelBase`합니다. [ `SchoolViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) XML 파일을 다운로드 하 고 모든 개체 조합 합니다.

[ **StudentList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList) 사용 하 여 프로그램는 `ImageCell` 학생과에서 해당 이미지를 표시 하는 `ListView`:

[![삼중 학생 목록 스크린샷](images/ch19fg18-small.png "학생 목록")](images/ch19fg18-large.png "학생 목록")

[ **ListViewHeader** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader) 샘플 추가 [ `Header` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.Header/) 속성 표시 Android에서.

### <a name="selection-and-the-binding-context"></a>바인딩 컨텍스트와 선택

[ **SelectedStudentDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail) 바인딩 프로그램는 `BindingContext` 의 `StackLayout` 에 `SelectedItem` 의 속성은 `ListView`합니다. 이렇게 하면 프로그램 선택한 학생에 대 한 자세한 정보를 표시 합니다.

### <a name="context-menus"></a>상황에 맞는 메뉴

셀에서 플랫폼에 따라 다르게 구현 되는 상황에 맞는 메뉴를 정의할 수 있습니다. 이 메뉴를 만들려면 추가 [ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) 개체는 [ `ContextActions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Cell.ContextActions/) 속성은 `Cell`합니다.

`MenuItem` 5 가지 속성을 정의합니다.

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) 형식의 `string`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) 형식의 `FileImageSource`
- [`IsDestructive`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.IsDestructive/) 형식의 `bool`
- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Command/) 형식의 `ICommand`
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.CommandParameter/) 형식의 `object`

`Command` 및 `CommandParameter` 속성 각 항목에 대 한 ViewModel에 원하는 메뉴 명령을 수행 하는 메서드가 포함 되어 있음을 의미 합니다. MVVM 아닌 시나리오에서 `MenuItem` 도 정의 [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/) 이벤트입니다.

[ **CellContextMenu** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu) 이 기술을 보여 줍니다. `Command` 각 속성 `MenuItem` 형식의 속성에 바인딩된 `ICommand` 에 `Student` 클래스입니다. 설정는 `IsDestructive` 속성을 `true` 에 대 한는 `MenuItem` 제거 하거나 삭제 하는 선택된 된 개체입니다.

### <a name="varying-the-visuals"></a>시각적 개체를 변경합니다.

에 있는 항목의 시각적 개체에서 약간 변형 해야 경우에 따라는 `ListView` 속성에 따라 합니다. 예를 들어 때 학생의 등급 포인트 평균 미만으로 떨어질 2.0에는 [ **ColorCodedStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents) 샘플 빨간색으로 해당 학생의 이름을 표시 합니다.
바인딩 값 변환기를 사용 하 여 이렇게 [ `ThresholdToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs)에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리입니다.

### <a name="refreshing-the-content"></a>콘텐츠 새로 고침

`ListView` 데이터를 새로 고치고에 대 한 풀 다운 제스처를 지원 합니다. 프로그램 설정 해야 합니다는 [ `IsPullToRefresh` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsPullToRefreshEnabled/) 속성을 `true` 이 사용할 수 있도록 합니다. `ListView` 풀 다운 제스처에 응답 하 여 설정 하 여 해당 [ `IsRefreshing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsRefreshing/) 속성을 `true`, 및를 발생 시켜는 [ `Refreshing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.Refreshing/) 이벤트 및 (MVVM 시나리오) 호출 `Execute` 방식의 해당 [ `RefreshCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RefreshCommand/) 속성입니다.

처리 코드는 `Refresh` 이벤트 또는 `RefreshCommand` 표시 하는 데이터를 가능한 업데이트는 `ListView` 설정 `IsRefreshing` 다시 `false`합니다.

[ **RssFeed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed) 샘플 사용을 보여 줍니다.는 [ `RssFeedViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs) 를 구현 하는 `RefreshCommand` 및 `IsRefreshing` 데이터 바인딩에 대 한 속성.

## <a name="the-tableview-and-its-intents"></a>TableView 및 해당 의도

반면는 `ListView` 일반적으로 동일한 형식의 여러 인스턴스가 표시 됩니다는 [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) 는 일반적으로 데 다양 한 종류의 여러 속성에 대 한 사용자 인터페이스를 제공 합니다. 각 항목은 자체와 연결 [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) 파생 속성을 표시 하거나 사용자 인터페이스를 제공 합니다.

### <a name="properties-and-hierarchies"></a>속성 및 계층 구조

`TableView` 4 개의 속성을 정의합니다.

- [`Intent`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Intent/) 형식의 [ `TableIntent` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableIntent/), 열거형
- [`Root`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Root/) 형식의 [ `TableRoot` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/)의 content 속성 `TableView`
- [`RowHeight`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.RowHeight/) 형식의 `int`
- [`HasUnevenRows`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.HasUnevenRows/) 형식의 `bool`

`TableIntent` 열거형에 사용 하려는 방법을 나타냅니다는 `TableView`:

- [`Data`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Data/)
- [`Form`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Form/)
- [`Settings`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Settings/)
- [`Menu`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Menu/)

이러한 멤버에 대 한 몇 가지 팁을 제안할 수도 `TableView`합니다.

다른 몇 가지 클래스는 테이블 정의에 관련 된:

- [`TableSectionBase`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase/) 클래스는 추상 클래스에서 파생 된 `BindableObject` 정의 [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TableSectionBase.Title/) 속성

- [`TableSectionBase<T>`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase%3CT%3E/) 클래스는 추상 클래스에서 파생 된 `TableSectionBase` 구현 `IList<T>` 및 `INotifyCollectionChanged`

- [`TableSection`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSection/) 파생 `TableSectionBase<Cell>`

- [`TableRoot`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/) 파생 `TableSectionBase<TableSection>`

즉, `TableView` 에 `Root` 속성으로 설정 하는 `TableRoot` 개체 컬렉션인의 `TableSection` 개체의 컬렉션은 각각 `Cell` 개체 합니다. 테이블에 여러 섹션 및 각 섹션에 여러 셀이 포함 됩니다. 테이블 자체 수 제목을 있으며 각 섹션 제목을 가질 수 있습니다. 하지만 `TableView` 활용 `Cell` 파생 항목을 수의 사용 `DataTemplate`합니다.

### <a name="a-prosaic-form"></a>사용할 수 있는 양식

[ **EntryForm** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm) 샘플 정의 [ `PersonalInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) 보기 모델을는 인스턴스는 `BindingContext` 의 `TableView`합니다. 각 `Cell` 에 파생 해당 `TableSection` 의 속성에 대 한 바인딩을 가질 수는 `PersonalInformation` 클래스입니다.

### <a name="custom-cells"></a>사용자 지정 셀

[ **ConditionalCells** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells) 샘플을 확장 **EntryForm**합니다. [ `ProgrammerInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) 클래스는 두 개의 추가 속성의 적용 여부를 제어 하는 부울 속성을 포함 합니다. 프로그램은 이러한 두 개의 추가 속성에 대 한 사용자 지정을 사용 `PickerCell` 기반으로 한 [PickerCell.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml) 및 [PickerCell.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs) 에 [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리입니다.

하지만 `IsEnabled` 속성 두 `PickerCell` 요소는 부울 속성에 바인딩됩니다. `ProgrammerInformation`,이 기술을 하지 않는 것 작동 하도록 하는 다음 샘플 확인 메시지를 표시 합니다.

### <a name="conditional-sections"></a>조건부 섹션

[ **ConditionalSection** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection) 샘플 별도의 부울 항목의 선택에 조건부가 지정 하는 두 항목을 배치 `TableSection`합니다. 코드 숨김 파일에서이 섹션을 제거는 `TableView` 또는 부울 속성에 따라 다시 추가 합니다.

### <a name="a-tableview-menu"></a>TableView 메뉴

또 다른 용도 `TableView` 메뉴입니다. [ **MenuCommands** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands) 샘플 약간 이동할 수 있는 메뉴를 보여 줍니다. `BoxView` 는 화면입니다.



## <a name="related-links"></a>관련 링크

- [19 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [19 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
