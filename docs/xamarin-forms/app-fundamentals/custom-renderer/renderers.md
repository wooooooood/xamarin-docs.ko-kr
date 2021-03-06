---
title: 'title: “렌더러 기본 클래스 및 네이티브 컨트롤” description: “모든 Xamarin.Forms 컨트롤의 모양과 동작을 효과적으로 사용자 지정할 수 있습니다.'
description: '이 문서에는 각 Xamarin.Forms 페이지, 레이아웃, 뷰 및 셀을 구현하는 네이티브 컨트롤 클래스와 렌더러가 나열됩니다.” ms.prod: xamarin ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 04/17/2020 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/17/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9f6eff831f3eb653e281dd9ee750bae7f7dfda06
ms.sourcegitcommit: 2105091f2eeb7844b19ae94708a6ab07e3e79bce
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84198382"
---
# <a name="renderer-base-classes-and-native-controls"></a>렌더러 기본 클래스 및 네이티브 컨트롤

모든 Xamarin.Forms 컨트롤에는 네이티브 컨트롤의 인스턴스를 만드는 각 플랫폼에 함께 제공되는 렌더러가 있습니다. 이 문서에는 각 Xamarin.Forms 페이지, 레이아웃, 뷰 및 셀을 구현하는 네이티브 컨트롤 클래스와 렌더러가 나열됩니다.

`MapRenderer` 클래스를 제외하고 플랫폼별 렌더러는 다음 네임스페이스에서 찾을 수 있습니다.

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Android(AppCompat)** – Xamarin.Forms.Platform.Android.AppCompat
- **Android(FastRenderers)**  - Xamarin.Forms.Platform.Android.FastRenderers
- **UWP(유니버설 Windows 플랫폼)** – Xamarin.Forms.Platform.UWP

빠른 렌더러에 관한 자세한 내용은 [Xamarin.Forms 빠른 렌더러](~/xamarin-forms/internals/fast-renderers.md)를 참조하세요.

`MapRenderer` 클래스는 다음 네임스페이스에서 찾을 수 있습니다.

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** – Xamarin.Forms.Maps.Android
- **UWP(유니버설 Windows 플랫폼)** – Xamarin.Forms.Maps.UWP

> [!NOTE]
> Shell 애플리케이션의 사용자 지정 렌더러를 만드는 방법에 대한 자세한 내용은 [Xamarin.Forms Shell 사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/shell/customrenderers.md)를 참조하세요.

## <a name="pages"></a>Pages

다음 표에서는 각 Xamarin.Forms [페이지](~/xamarin-forms/user-interface/controls/pages.md) 형식을 구현하는 네이티브 컨트롤 클래스와 렌더러가 나열됩니다.

|페이지|렌더러|iOS|Android|Android(AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ContentPage`](xref:Xamarin.Forms.ContentPage)|[PageRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md)|UIViewController|ViewGroup||FrameworkElement|
|[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)|PhoneMasterDetailRenderer(iOS – 휴대폰), TabletMasterDetailPageRenderer(iOS – 태블릿), MasterDetailRenderer(Android), MasterDetailPageRenderer(Android AppCompat), MasterDetailPageRenderer(UWP)|UIViewController(휴대폰), UISplitViewController(태블릿)|DrawerLayout(v4)|DrawerLayout(v4)|FrameworkElement(사용자 지정 컨트롤)|
|[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)|NavigationRenderer(iOS 및 Android), NavigationPageRenderer(Android AppCompat), NavigationPageRenderer(UWP)|UIToolbar|ViewGroup|ViewGroup|FrameworkElement(사용자 지정 컨트롤)|
|[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)|TabbedRenderer(iOS 및 Android), TabbedPageRenderer(Android AppCompat), TabbedPageRenderer(UWP)|UIView|ViewPager|ViewPager|FrameworkElement(피벗)|
|[`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)|PageRenderer|UIViewController|ViewGroup||FrameworkElement|
|[`CarouselPage`](xref:Xamarin.Forms.CarouselPage)|CarouselPageRenderer|UIScrollView|ViewPager|ViewPager|FrameworkElement(FlipView)|

## <a name="layouts"></a>레이아웃

다음 표에서는 각 Xamarin.Forms [레이아웃](~/xamarin-forms/user-interface/controls/layouts.md) 형식을 구현하는 네이티브 컨트롤 클래스와 렌더러가 나열됩니다.

|레이아웃|렌더러|iOS|Android|Android(AppCompat)|UWP|
|--- |--- |--- |--- |--- |
|[`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`ContentView`](xref:Xamarin.Forms.ContentView)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`FlexLayout`](xref:Xamarin.Forms.FlexLayout)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`Frame`](xref:Xamarin.Forms.Frame)|FrameRenderer|UIView|ViewGroup|CardView|테두리|
|[`ScrollView`](xref:Xamarin.Forms.ScrollView)|ScrollViewRenderer|UIScrollView|ScrollView|ScrollView|ScrollViewer|
|[`TemplatedView`](xref:Xamarin.Forms.TemplatedView)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`Grid`](xref:Xamarin.Forms.Grid)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`StackLayout`](xref:Xamarin.Forms.StackLayout)|ViewRenderer|UIView|View|View|FrameworkElement|

## <a name="views"></a>뷰

다음 표에서는 각 Xamarin.Forms [뷰](~/xamarin-forms/user-interface/controls/views.md) 형식을 구현하는 네이티브 컨트롤 클래스와 렌더러가 나열됩니다.

|뷰|렌더러|iOS|Android|Android(AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)|ActivityIndicatorRenderer|UIActivityIndicator|ProgressBar||ProgressBar|
|[`BoxView`](xref:Xamarin.Forms.BoxView)|BoxRenderer(iOS 및 Android), BoxViewRenderer(UWP)|UIView|ViewGroup||사각형|
|[`Button`](xref:Xamarin.Forms.Button)|ButtonRenderer|UIButton|단추|AppCompatButton|단추|
|[`CarouselView`](xref:Xamarin.Forms.CarouselView)|CarouselViewRenderer|UICollectionView||RecyclerView|ListViewBase|
|`CheckBox`|CheckBoxRenderer|UIButton||AppCompatCheckBox|CheckBox|
|[`CollectionView`](xref:Xamarin.Forms.CollectionView)|CollectionViewRenderer|UICollectionView||RecyclerView|ListViewBase|
|[`DatePicker`](xref:Xamarin.Forms.DatePicker)|DatePickerRenderer|UITextField|EditText||DatePicker|
|[`Editor`](xref:Xamarin.Forms.Editor)|EditorRenderer|UITextView|EditText||TextBox|
|[`Entry`](xref:Xamarin.Forms.Entry)|[EntryRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/entry.md)|UITextField|EditText||TextBox|
|[`Image`](xref:Xamarin.Forms.Image)|ImageRenderer|UIImageView|ImageView||이미지|
|[`ImageButton`](xref:Xamarin.Forms.ImageButton)|ImageButtonRenderer|UIButton||AppCompatImageButton|단추|
|`IndicatorView`|IndicatorViewRenderer|UIPageControl||LinearLayout||
|[`Label`](xref:Xamarin.Forms.Label)|LabelRenderer|UILabel|TextView||TextBlock|
|[`ListView`](xref:Xamarin.Forms.ListView)|[ListViewRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)|UITableView|ListView||ListView|
|[`Map`](xref:Xamarin.Forms.Maps.Map)|[MapRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)|MKMapView|MapView||MapControl|
|[`MediaElement`](xref:Xamarin.Forms.MediaElement)|MediaElementRenderer|UIView||VideoView|MediaElement|
|[`Picker`](xref:Xamarin.Forms.Picker)|PickerRenderer|UITextField|EditText|EditText|ComboBox|
|`RadioButton`|RadioButtonRenderer|UIButton||AppCompatRadioButton|RadioButton|
|[`ProgressBar`](xref:Xamarin.Forms.ProgressBar)|ProgressBarRenderer|UIProgressView|ProgressBar||ProgressBar|
|[`RefreshView`](xref:Xamarin.Forms.RefreshView)|RefreshViewRenderer|UIView||SwipeRefreshLayout|RefreshContainer|
|[`SearchBar`](xref:Xamarin.Forms.SearchBar)|SearchBarRenderer|UISearchBar|SearchView||AutoSuggestBox|
|[`Slider`](xref:Xamarin.Forms.Slider)|SliderRenderer|UISlider|SeekBar||슬라이더|
|[`Stepper`](xref:Xamarin.Forms.Stepper)|StepperRenderer|UIStepper|LinearLayout||컨트롤|
|`SwipeView`|SwipeViewRenderer|UIView||View|SwipeControl|
|[`Switch`](xref:Xamarin.Forms.Switch)|SwitchRenderer|UISwitch|스위치|SwitchCompat|ToggleSwitch|
|[`TableView`](xref:Xamarin.Forms.TableView)|TableViewRenderer|UITableView|ListView||ListView|
|[`TimePicker`](xref:Xamarin.Forms.TimePicker)|TimePickerRenderer|UITextField|EditText||TimePicker|
|[`WebView`](xref:Xamarin.Forms.WebView)|WkWebViewRenderer(iOS), WebViewRenderer(Android and UWP)|WkWebView|WebView||WebView|

> [!NOTE]
> `Expander` 컨트롤은 애니메이션이 포함된 [`StackLayout`](xref:Xamarin.Forms.StackLayout)을 사용하여 구현됩니다. 따라서 플랫폼 렌더러가 없습니다.

## <a name="cells"></a>셀

다음 표에서는 각 Xamarin.Forms [셀](~/xamarin-forms/user-interface/controls/cells.md) 형식을 구현하는 네이티브 컨트롤 클래스와 렌더러가 나열됩니다.

|셀|렌더러|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`EntryCell`](xref:Xamarin.Forms.EntryCell)|EntryCellRenderer|UITextField를 포함한 UITableViewCell|TextView와 EditText를 포함한 LinearLayout|TextBox를 포함한 DataTemplate|
|[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)|SwitchCellRenderer|UISwitch를 포함한 UITableViewCell|스위치|TextBlock과 ToggleSwitch가 있는 그리드를 포함한 DataTemplate|
|[`TextCell`](xref:Xamarin.Forms.TextCell)|TextCellRenderer|UITableViewCell|두 개의 TextViews를 포함한 LinearLayout|두 개의 TextBlock이 있는 StackPanel을 포함한 DataTemplate|
|[`ImageCell`](xref:Xamarin.Forms.ImageCell)|ImageCellRenderer|UIImage를 포함한 UITableViewCell|두 개의 TextViews와 ImageView를 포함한 LinearLayout|이미지와 두 개의 TextBlock이 있는 그리드를 포함한 DataTemplate|
|[`ViewCell`](xref:Xamarin.Forms.ViewCell)|[ViewCellRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)|UITableViewCell|View|ContentPresenter를 포함한 DataTemplate|

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms 빠른 렌더러](~/xamarin-forms/internals/fast-renderers.md)
- [Xamarin.Forms 셸 사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/shell/customrenderers.md)
