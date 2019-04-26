---
title: Xamarin.Android에는 ListView를 사용 하 여
description: ListView는 Android 응용 프로그램의 중요 한 UI 구성 요소 데 사용 됩니다 어디에서 나 메뉴 옵션의 짧은 목록에서 연락처 또는 인터넷 즐겨찾기 긴 목록입니다. 수는 기본 제공 스타일으로 서식이 지정 하거나 광범위 하 게 사용자 지정 하는 행의 스크롤 목록을 표시 하는 간단한 방법을 제공 합니다.
ms.prod: xamarin
ms.assetid: C2BA2705-9B20-01C2-468D-860BDFEDC157
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: a30256722647bbea482970d0c4a751954810d99e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61170802"
---
# <a name="listview"></a>ListView

_ListView는 Android 응용 프로그램의 중요 한 UI 구성 요소 데 사용 됩니다 어디에서 나 메뉴 옵션의 짧은 목록에서 연락처 또는 인터넷 즐겨찾기 긴 목록입니다. 수는 기본 제공 스타일으로 서식이 지정 하거나 광범위 하 게 사용자 지정 하는 행의 스크롤 목록을 표시 하는 간단한 방법을 제공 합니다._


## <a name="overview"></a>개요

목록 뷰 및 어댑터는 Android 응용 프로그램의 가장 기본적인 구성 요소에 포함 됩니다. `ListView` 클래스 인지 짧은 메뉴 또는 스크롤 긴 목록 데이터를 제공 하는 유연한 방법을 제공 합니다. 빠른 스크롤, 인덱스 및 응용 프로그램에 대 한 모바일 친화적 사용자 인터페이스를 빌드할 수 있도록 단일 또는 다중 선택 등의 유용성 기능을 제공 합니다. `ListView` 인스턴스에는 행 보기에 포함된 데이터를 사용하여 피드하는 *어댑터*가 필요합니다.

이 가이드를 구현 하는 방법에 설명 `ListView` 및 다양 한 `Adapter` Xamarin.Android에는 클래스입니다. 모양을 사용자 지정 하는 방법을 보여 줍니다는 `ListView`, 메모리 소비를 줄이기 위해 다시 사용 하는 행의 중요성에 설명 합니다. 작업 수명 주기 미치는 일부 설명은 이기도 `ListView` 고 `Adapter` 사용 합니다. Xamarin.iOS 사용 하 여 플랫폼 간 응용 프로그램에서 작업 하는 경우는 `ListView` 컨트롤은 iOS에 구조적으로 비슷한 `UITableView` (및 Android `Adapter` 비슷합니다는 `UITableViewSource`).

첫째, 간략 한 자습서 소개는 `ListView` 기본 코드 예제를 사용 하 여 합니다. 고급 항목의 링크를 사용할 수 있도록 제공 되는 다음으로, `ListView` 실제 앱에서.


> [!NOTE]
> 합니다 `RecyclerView` 위젯 버전이 더 고급 하 고 유연한 `ListView`합니다. 때문에 `RecyclerView` 에 대 한 후속 되도록 설계 되었습니다 `ListView` (및 `GridView`)를 사용 하는 것이 좋습니다 `RecyclerView` 대신 `ListView` 새 앱 개발을 위한 합니다. 자세한 내용은 [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)합니다.



## <a name="listview-tutorial"></a>ListView 자습서

[`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/) 가 [`ViewGroup`](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)
스크롤 가능한 항목의 목록을 만듭니다. 목록 항목을 사용 하 여 목록에 자동으로 삽입 되는 [ `IListAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.IListAdapter/)합니다.

이 자습서에서는 스크롤할 수 있는 문자열 배열에서 읽는 국가 이름 목록을 만듭니다. 목록 항목을 선택 하면 알림 메시지는 항목의 위치 목록에 표시 됩니다.


명명 된 새 프로젝트를 시작 **HelloListView**합니다.

라는 XML 파일을 만듭니다 **list_item.xml** 내에서 저장 된 **리소스/레이아웃/** 폴더. 다음을 삽입 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp"
    android:textSize="16sp">
</TextView>
```

이 파일에 배치 될 각 항목에 대 한 레이아웃을 정의 합니다 [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/)합니다.

오픈 `MainActivity.cs` 확장할 클래스를 수정 하 고 [ `ListActivity` ](https://developer.xamarin.com/api/type/Android.App.ListActivity/) (대신 [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/)):

```csharp
public class MainActivity : ListActivity
{
```

다음 코드를 삽입 합니다 [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle))
방법:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);

    ListView.TextFilterEnabled = true;

    ListView.ItemClick += delegate (object sender, AdapterView.ItemClickEventArgs args)
    {
        Toast.MakeText(Application, ((TextView)args.View).Text, ToastLength.Short).Show();
    };
}
```

이 작업에 대 한 레이아웃 파일을를 로드 하지는 않습니다 통지 (일반적으로 사용 하 여 수행할 [ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32))).
대신, 설정 된 [`ListAdapter`](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/)
속성 자동으로 추가 합니다.는 [`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
전체 화면에 맞게 합니다 [ `ListActivity` ](https://developer.xamarin.com/api/type/Android.App.ListActivity/)합니다.
이 메서드는 [ `ArrayAdapter<T>` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/)에 배치 될 목록 항목의 배열을 관리 하는 합니다 [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/).
는 [`ArrayAdapter<T>`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/)
생성자는 응용 프로그램 [ `Context` ](https://developer.xamarin.com/api/type/Android.Content.Context/), (이전 단계에서 만든) 각 목록 항목에 대 한 레이아웃 설명 및 `T[]` 또는 [`Java.Util.IList<T>`](https://developer.xamarin.com/api/type/Java.Util.IList/)
배열에 삽입할 개체는 [`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
(다음 정의 됨).

는 [`TextFilterEnabled`](https://developer.xamarin.com/api/property/Android.Widget.AbsListView.TextFilterEnabled/)
속성 설정에 대 한 필터링 하는 텍스트를 [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/)는 사용자 입력을 시작, 목록 필터링 됩니다.

는 [`ItemClick`](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/)
클릭에 대 한 처리기를 구독할 이벤트를 사용할 수 있습니다. 항목을 [`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
클릭 하면 처리기가 호출 되 고 [`Toast`](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
클릭 한 항목의에서 텍스트를 사용 하 여 메시지가 표시 됩니다.

에 대 한 사용자 고유의 레이아웃 파일을 정의 하는 대신 플랫폼에서 제공 하는 목록 항목 디자인을 사용할 수는 [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/)합니다.
예를 들어 사용해 보세요 `Android.Resource.Layout.SimpleListItem1` 대신 `Resource.Layout.list_item`합니다.

다음 추가 `using` 문:

```csharp
using System;
```
다음으로, 다음 문자열 배열의 멤버로 추가 `MainActivity`:

```csharp
static readonly string[] countries = new String[] {
    "Afghanistan","Albania","Algeria","American Samoa","Andorra",
    "Angola","Anguilla","Antarctica","Antigua and Barbuda","Argentina",
    "Armenia","Aruba","Australia","Austria","Azerbaijan",
    "Bahrain","Bangladesh","Barbados","Belarus","Belgium",
    "Belize","Benin","Bermuda","Bhutan","Bolivia",
    "Bosnia and Herzegovina","Botswana","Bouvet Island","Brazil","British Indian Ocean Territory",
    "British Virgin Islands","Brunei","Bulgaria","Burkina Faso","Burundi",
    "Cote d'Ivoire","Cambodia","Cameroon","Canada","Cape Verde",
    "Cayman Islands","Central African Republic","Chad","Chile","China",
    "Christmas Island","Cocos (Keeling) Islands","Colombia","Comoros","Congo",
    "Cook Islands","Costa Rica","Croatia","Cuba","Cyprus","Czech Republic",
    "Democratic Republic of the Congo","Denmark","Djibouti","Dominica","Dominican Republic",
    "East Timor","Ecuador","Egypt","El Salvador","Equatorial Guinea","Eritrea",
    "Estonia","Ethiopia","Faeroe Islands","Falkland Islands","Fiji","Finland",
    "Former Yugoslav Republic of Macedonia","France","French Guiana","French Polynesia",
    "French Southern Territories","Gabon","Georgia","Germany","Ghana","Gibraltar",
    "Greece","Greenland","Grenada","Guadeloupe","Guam","Guatemala","Guinea","Guinea-Bissau",
    "Guyana","Haiti","Heard Island and McDonald Islands","Honduras","Hong Kong","Hungary",
    "Iceland","India","Indonesia","Iran","Iraq","Ireland","Israel","Italy","Jamaica",
    "Japan","Jordan","Kazakhstan","Kenya","Kiribati","Kuwait","Kyrgyzstan","Laos",
    "Latvia","Lebanon","Lesotho","Liberia","Libya","Liechtenstein","Lithuania","Luxembourg",
    "Macau","Madagascar","Malawi","Malaysia","Maldives","Mali","Malta","Marshall Islands",
    "Martinique","Mauritania","Mauritius","Mayotte","Mexico","Micronesia","Moldova",
    "Monaco","Mongolia","Montserrat","Morocco","Mozambique","Myanmar","Namibia",
    "Nauru","Nepal","Netherlands","Netherlands Antilles","New Caledonia","New Zealand",
    "Nicaragua","Niger","Nigeria","Niue","Norfolk Island","North Korea","Northern Marianas",
    "Norway","Oman","Pakistan","Palau","Panama","Papua New Guinea","Paraguay","Peru",
    "Philippines","Pitcairn Islands","Poland","Portugal","Puerto Rico","Qatar",
    "Reunion","Romania","Russia","Rwanda","Sqo Tome and Principe","Saint Helena",
    "Saint Kitts and Nevis","Saint Lucia","Saint Pierre and Miquelon",
    "Saint Vincent and the Grenadines","Samoa","San Marino","Saudi Arabia","Senegal",
    "Seychelles","Sierra Leone","Singapore","Slovakia","Slovenia","Solomon Islands",
    "Somalia","South Africa","South Georgia and the South Sandwich Islands","South Korea",
    "Spain","Sri Lanka","Sudan","Suriname","Svalbard and Jan Mayen","Swaziland","Sweden",
    "Switzerland","Syria","Taiwan","Tajikistan","Tanzania","Thailand","The Bahamas",
    "The Gambia","Togo","Tokelau","Tonga","Trinidad and Tobago","Tunisia","Turkey",
    "Turkmenistan","Turks and Caicos Islands","Tuvalu","Virgin Islands","Uganda",
    "Ukraine","United Arab Emirates","United Kingdom",
    "United States","United States Minor Outlying Islands","Uruguay","Uzbekistan",
    "Vanuatu","Vatican City","Venezuela","Vietnam","Wallis and Futuna","Western Sahara",
    "Yemen","Yugoslavia","Zambia","Zimbabwe"
  };
```

이 배치할 수 있는 문자열의 배열 합니다 [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/).

애플리케이션을 실행합니다. 목록 스크롤 또는 다음 항목을 클릭 하는 메시지가 표시를 필터링 하려면 입력 수 있습니다. 다음과 같이 표시되어야 합니다.

[![국가 이름 사용 하 여 ListView의 예제 스크린샷](images/01-listview-example-sml.png)](images/01-listview-example.png#lightbox)

하드 코드 된 문자열 배열을 사용 하 여 최상의 디자인 되지 않은 참고 합니다. 설명 하기 위해 편의상,이 자습서에 사용 된는 [`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
widget. 와 같은 외부 리소스를 정의한 문자열 배열을 참조 하는 것이 좋습니다는 `string-array` 프로젝트에서 리소스 **Resources/Values/Strings.xml** 파일입니다. 예를 들어:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <string name="app_name">HelloListView</string>
  <string-array name="countries_array">
    <item>Bahrain</item>
    <item>Bangladesh</item>
    <item>Barbados</item>
    <item>Belarus</item>
    <item>Belgium</item>
    <item>Belize</item>
    <item>Benin</item>
  </string-array>
</resources>
```

에 대 한 이러한 리소스 문자열을 사용 하는 [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/)를 바꾸려면 [`ListAdapter`](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/)
다음 줄:

```csharp
string[] countries = Resources.GetStringArray (Resource.Array.countries_array);
ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);
```
애플리케이션을 실행합니다. 다음과 같이 표시되어야 합니다.

[![더 작은 이름 목록 사용 하 여 ListView의 예제 스크린샷](images/02-smaller-example-sml.png)](images/02-smaller-example.png#lightbox)


## <a name="going-further-with-listview"></a>ListView를 사용 하 여 계속 진행

나머지 항목에서는 (아래 링크) 작업에 대해 포괄적으로 수행 합니다 `ListView` 클래스 및 다양 한 유형의 어댑터 유형 함께 사용할 수 있습니다. 구조체는 다음과 같습니다.

-   **시각적 모양을** &ndash; 부분을 `ListView` 컨트롤 및 작동 방식입니다.

-   **클래스** &ndash; 표시 하는 데 사용 하는 클래스의 개요는 `ListView`합니다.

-   **ListView에 데이터 표시** &ndash; 데이터의 단순 목록을 표시 하는 방법, 구현 방법 `ListView's` 유용성 기능, 다른 기본 제공 행 레이아웃;를 사용 하는 방법 및 어댑터 다시 행 뷰를 사용 하 여 메모리에 저장 하는 방법입니다.

-   **사용자 지정 모양을** &ndash; 스타일을 변경 합니다 `ListView` 사용자 지정 레이아웃, 글꼴 및 색을 사용 하 여 합니다.

-   **SQLite를 사용 하 여** &ndash; 사용 하 여 SQLite 데이터베이스에서 데이터를 표시 하는 방법을 `CursorAdapter`입니다.

-   **작업 수명 주기** &ndash; 구현 하는 경우에 디자인 고려 사항 `ListView` 여기서 수명 주기에서 채워야 데이터 및 리소스를 해제 하는 경우를 비롯 한 작업을 합니다.

(6 개 부분으로 나눌) 토론의 개요를 시작 합니다 `ListView` 점진적으로 더 복잡 한 예가 사용 하는 방법 소개 하기 전에 클래스 자체입니다.

-   [ListView 파트 및 기능](~/android/user-interface/layouts/list-view/parts-and-functionality.md)
-   [데이터로 ListView 채우기](~/android/user-interface/layouts/list-view/populating.md)
-   [ListView 모양 사용자 지정](~/android/user-interface/layouts/list-view/customizing-appearance.md)
-   [CursorAdapters 사용](~/android/user-interface/layouts/list-view/cursor-adapters.md)
-   [ContentProvider 사용](~/android/user-interface/layouts/list-view/content-provider.md)
-   [ListView 및 작업 수명 주기](~/android/user-interface/layouts/list-view/activity-lifecycle.md)


## <a name="summary"></a>요약

이 항목 집합에 도입 `ListView` 의 기본 제공 기능을 사용 하는 방법의 몇 가지 예제를 제공 하 고는 `ListActivity`합니다. 이 설명의 사용자 지정 구현을 `ListView` 하며 간단 하 게 다루었습니다 작업 수명 주기의 관련성에 다채로운 레이아웃에 대 한 허용 및 SQLite 데이터베이스를 사용 하는 프로그램 `ListView` 구현 합니다.


## <a name="related-links"></a>관련 링크

- [AccessoryViews (샘플)](https://developer.xamarin.com/samples/AccessoryViews/)
- [BasicTableAndroid (샘플)](https://developer.xamarin.com/samples/BasicTableAndroid/)
- [BasicTableAdapter (샘플)](https://developer.xamarin.com/samples/BasicTableAdapter/)
- [BuiltInViews (샘플)](https://developer.xamarin.com/samples/BuiltInViews/)
- [CustomRowView (샘플)](https://developer.xamarin.com/samples/CustomRowView/)
- [FastScroll (샘플)](https://developer.xamarin.com/samples/FastScroll/)
- [SectionIndex (샘플)](https://developer.xamarin.com/samples/SectionIndex/)
- [SimpleCursorTableAdapter (샘플)](https://developer.xamarin.com/samples/SimpleCursorTableAdapter/)
- [CursorTableAdapter (샘플)](https://developer.xamarin.com/samples/CursorTableAdapter/)
- [활동 수명 주기 자습서](~/android/app-fundamentals/activity-lifecycle/index.md)
- [테이블 및 셀 (Xamarin.iOS)에서 작업](~/ios/user-interface/controls/tables/index.md)
- [ListView 클래스 참조](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
- [ListActivity 클래스 참조](https://developer.xamarin.com/api/type/Android.App.ListActivity/)
- [BaseAdapter 클래스 참조](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)
- [ArrayAdapter 클래스 참조](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)
- [CursorAdapter 클래스 참조](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/)
