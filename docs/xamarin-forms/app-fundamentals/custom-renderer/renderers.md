---
title: "렌더러의 기본 클래스와 기본 컨트롤"
description: "Xamarin.Forms는 모든 컨트롤에 네이티브 컨트롤의 인스턴스를 생성 하는 각 플랫폼에 대 한 함께 제공 되는 렌더러 있습니다. 이 문서에서는 렌더러와 각 Xamarin.Forms 페이지, 레이아웃, 뷰 및 셀을 구현 하는 네이티브 컨트롤 클래스를 나열 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/15/2016
ms.openlocfilehash: 06887e6c1a39dd695fdaddb2fade8a463d9d4580
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
---
# <a name="renderer-base-classes-and-native-controls"></a>렌더러의 기본 클래스와 기본 컨트롤

_Xamarin.Forms는 모든 컨트롤에 네이티브 컨트롤의 인스턴스를 생성 하는 각 플랫폼에 대 한 함께 제공 되는 렌더러 있습니다. 이 문서에서는 렌더러와 각 Xamarin.Forms 페이지, 레이아웃, 뷰 및 셀을 구현 하는 네이티브 컨트롤 클래스를 나열 합니다._

제외 된 `MapRenderer` 클래스 플랫폼별 렌더러는 다음 네임 스페이스에서 찾을 수 있습니다.

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Android (AppCompat)** – Xamarin.Forms.Platform.Android.AppCompat
- **유니버설 Windows 플랫폼 (UWP)** – Xamarin.Forms.Platform.UWP

`MapRenderer` 다음 네임 스페이스에서 클래스를 찾을 수 있습니다.

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** – Xamarin.Forms.Maps.Android
- **유니버설 Windows 플랫폼 (UWP)** – Xamarin.Forms.Maps.UWP

## <a name="pages"></a>인쇄할 페이지

다음 표에서 렌더러와 각 Xamarin.Forms를 구현 하는 네이티브 컨트롤 클래스 [페이지](~/xamarin-forms/user-interface/controls/pages.md) 유형:

|페이지|렌더러|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)|[PageRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md)|UIViewController|ViewGroup||FrameworkElement|
|[`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)|PhoneMasterDetailRenderer (iOS – 전화) (iOS – 태블릿), TabletMasterDetailPageRenderer MasterDetailRenderer (Android), MasterDetailPageRenderer (Android AppCompat), MasterDetailPageRenderer (UWP)|UIViewController (전화) UISplitViewController (태블릿)|DrawerLayout (v4)|DrawerLayout (v4)|FrameworkElement (사용자 정의 컨트롤)|
|[`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)|(IOS 및 Android) NavigationRenderer, NavigationPageRenderer (Android AppCompat), NavigationPageRenderer (UWP)|UIToolbar|ViewGroup|ViewGroup|FrameworkElement (사용자 정의 컨트롤)|
|[`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)|(IOS 및 Android) TabbedRenderer, TabbedPageRenderer (Android AppCompat), TabbedPageRenderer (UWP)|UIView|ViewPager|ViewPager|FrameworkElement (피벗)|
|[`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/)|PageRenderer|UIViewController|ViewGroup||FrameworkElement|
|[`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)|CarouselPageRenderer|UIScrollView|ViewPager|ViewPager|FrameworkElement (FlipView)|

## <a name="layouts"></a>레이아웃

다음 표에서 렌더러와 각 Xamarin.Forms를 구현 하는 네이티브 컨트롤 클래스 [레이아웃](~/xamarin-forms/user-interface/controls/layouts.md) 유형:

|레이아웃|렌더러|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`ContentPresenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)|ViewRenderer|UIView|보기|FrameworkElement|
|[`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)|ViewRenderer|UIView|보기|FrameworkElement|
|[`Frame`](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/)|FrameRenderer|UIView|ViewGroup|테두리|
|[`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/)|ScrollViewRenderer|UIScrollView|ScrollView|ScrollViewer|
|[`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/)|ViewRenderer|UIView|보기|FrameworkElement|
|[`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/)|ViewRenderer|UIView|보기|FrameworkElement|
|[`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)|ViewRenderer|UIView|보기|FrameworkElement|
|[`RelativeLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/)|ViewRenderer|UIView|보기|FrameworkElement|
|[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)|ViewRenderer|UIView|보기|FrameworkElement|

## <a name="views"></a>보기

다음 표에서 렌더러와 각 Xamarin.Forms를 구현 하는 네이티브 컨트롤 클래스 [보기](~/xamarin-forms/user-interface/controls/views.md) 유형:

|보기|렌더러|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ActivityIndicator`](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/)|ActivityIndicatorRenderer|UIActivityIndicator|ProgressBar||ProgressBar|
|[`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)|(IOS 및 Android) BoxRenderer, BoxViewRenderer (Windows Phone 및 WinRT)|UIView|ViewGroup||사각형|
|[`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)|ButtonRenderer|UIButton|단추|AppCompatButton|단추|
|[`CarouselView`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/)|CarouselViewRenderer|UIScrollView|RecyclerView||FlipView|
|[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/)|DatePickerRenderer|UITextField|EditText||DatePicker|
|[`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/)|EditorRenderer|UITextView|EditText||TextBox|
|[`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)|[EntryRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/entry.md)|UITextField|EditText||TextBox|
|[`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)|ImageRenderer|UIImageView|ImageView||이미지|
|[`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)|LabelRenderer|UILabel|TextView||TextBlock|
|[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)|[ListViewRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)|UITableView|ListView||ListView|
|[`Map`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/)|[MapRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)|MKMapView|MapView||MapControl|
|[`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)|PickerRenderer|UITextField|EditText|EditText|ComboBox|
|[`ProgressBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/)|ProgressBarRenderer|UIProgressView|ProgressBar||ProgressBar|
|[`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/)|SearchBarRenderer|UISearchBar|SearchView||AutoSuggestBox|
|[`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)|SliderRenderer|UISlider|SeekBar||슬라이더|
|[`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/)|StepperRenderer|UIStepper|LinearLayout||Control|
|[`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/)|SwitchRenderer|UISwitch|전환|SwitchCompat|ToggleSwitch|
|[`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/)|TableViewRenderer|UITableView|ListView||ListView|
|[`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/)|TimePickerRenderer|UITextField|EditText||TimePicker|
|[`WebView`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/)|WebViewRenderer|UIWebView|웹 보기||웹 보기|

## <a name="cells"></a>셀

다음 표에서 렌더러와 각 Xamarin.Forms를 구현 하는 네이티브 컨트롤 클래스 [셀](~/xamarin-forms/user-interface/controls/cells.md) 유형:

|셀|렌더러|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/)|EntryCellRenderer|UITextField와 UITableViewCell|TextView와 EditText LinearLayout|TextBox와 DataTemplate|
|[`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/)|SwitchCellRenderer|UISwitch와 UITableViewCell|전환|TextBlock과 ToggleSwitch를 포함 하는 표를 DataTemplate|
|[`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/)|TextCellRenderer|UITableViewCell|두 개의 TextViews와 LinearLayout|두 개의 Textblock 포함 된 StackPanel와 DataTemplate|
|[`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/)|ImageCellRenderer|UIImage와 UITableViewCell|두 개의 TextViews 및는 ImageView LinearLayout|이미지와 두 개의 Textblock를 포함 하는 표를 DataTemplate|
|[`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)|[ViewCellRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)|UITableViewCell|보기|DataTemplate을 ContentPresenter와|

## <a name="summary"></a>요약

이 문서는 렌더러와 각 Xamarin.Forms 페이지, 레이아웃, 뷰 및 셀을 구현 하는 네이티브 컨트롤 클래스 나열 합니다. Xamarin.Forms는 모든 컨트롤에 네이티브 컨트롤의 인스턴스를 생성 하는 각 플랫폼에 대 한 함께 제공 되는 렌더러 있습니다.

## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러 (Xamarin 대학 비디오)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
