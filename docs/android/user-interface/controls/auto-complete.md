---
title: Xamarin Android의 자동 완성
ms.prod: xamarin
ms.assetid: D4C8CA49-8369-35B7-798D-B147FDC24185
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/31/2018
ms.openlocfilehash: 810c6ddead66d191870ce97a50653f29737492b0
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510660"
---
# <a name="auto-complete-for-xamarinandroid"></a>Xamarin Android의 자동 완성

`AutoCompleteTextView`사용자가 입력 하는 동안 완성 제안을 자동으로 표시 하는 편집 가능한 텍스트 뷰 요소입니다. 사용자가 편집 상자의 내용을 바꿀 항목을 선택할 수 있는 드롭다운 메뉴에 제안 목록이 표시 됩니다.

![자동 완성 예제](images/auto-complete.png)

## <a name="overview"></a>개요

자동 완성 제안을 제공 하는 텍스트 항목 위젯을 만들려면[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
위젯. 를 [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)통해 위젯에 연결 된 문자열 컬렉션에서 제안을 받습니다.

이 자습서에서는 다음을 만듭니다.[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
국가 이름에 대 한 제안을 제공 하는 위젯

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="5dp">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Country" />
    <AutoCompleteTextView android:id="@+id/autocomplete_country"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"/>
</LinearLayout>
```

[`TextView`](xref:Android.Widget.TextView) 는 다음을 소개 하는 레이블입니다.[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
위젯.


## <a name="tutorial"></a>자습서

*HelloAutoComplete*라는 새 프로젝트를 시작 합니다.

이라는 `list_item.xml` XML 파일을 만들고 **리소스/레이아웃** 폴더 내에 저장 합니다. 이 파일의 빌드 작업을로 `AndroidResource`설정 합니다. 다음과 같이 파일을 편집 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>

<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp"
    android:textSize="16sp"
    android:textColor="#000">
</TextView> 
```

이 파일은 제안 목록 [`TextView`](xref:Android.Widget.TextView) 에 표시 되는 각 항목에 사용 되는 간단한를 정의 합니다.

**리소스/레이아웃/기본. axml** 을 열고 다음을 삽입 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="5dp">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Country" />
    <AutoCompleteTextView android:id="@+id/autocomplete_country"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"/>
</LinearLayout>
```

**MainActivity.cs** 를 열고 다음 코드를 삽입 합니다.[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
방법이

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "Main" layout resource
    SetContentView (Resource.Layout.Main);

    AutoCompleteTextView textView = FindViewById<AutoCompleteTextView> (Resource.Id.autocomplete_country);
    var adapter = new ArrayAdapter<String> (this, Resource.Layout.list_item, COUNTRIES);

    textView.Adapter = adapter;
}
```

콘텐츠 뷰가 `main.xml` 레이아웃으로 설정 된 후에는[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
위젯은를 사용 하 여 [`FindViewById`](xref:Android.App.Activity.FindViewById*)레이아웃에서 캡처됩니다. 그런 다음 [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter) 새를 초기화 하 여 레이아웃 `list_item.xml` 을 `COUNTRIES` 문자열 배열의 각 목록 항목에 바인딩합니다 (다음 단계에서 정의 됨). 마지막으로 `SetAdapter()` 를 호출 하 여를 [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter) 에 연결 합니다.[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
문자열 배열이 제안 목록을 채우도록 위젯을 표시 합니다.

`MainActivity` 클래스 내에서 문자열 배열을 추가 합니다.

```csharp
static string[] COUNTRIES = new string[] {
  "Afghanistan", "Albania", "Algeria", "American Samoa", "Andorra",
  "Angola", "Anguilla", "Antarctica", "Antigua and Barbuda", "Argentina",
  "Armenia", "Aruba", "Australia", "Austria", "Azerbaijan",
  "Bahrain", "Bangladesh", "Barbados", "Belarus", "Belgium",
  "Belize", "Benin", "Bermuda", "Bhutan", "Bolivia",
  "Bosnia and Herzegovina", "Botswana", "Bouvet Island", "Brazil", "British Indian Ocean Territory",
  "British Virgin Islands", "Brunei", "Bulgaria", "Burkina Faso", "Burundi",
  "Cote d'Ivoire", "Cambodia", "Cameroon", "Canada", "Cape Verde",
  "Cayman Islands", "Central African Republic", "Chad", "Chile", "China",
  "Christmas Island", "Cocos (Keeling) Islands", "Colombia", "Comoros", "Congo",
  "Cook Islands", "Costa Rica", "Croatia", "Cuba", "Cyprus", "Czech Republic",
  "Democratic Republic of the Congo", "Denmark", "Djibouti", "Dominica", "Dominican Republic",
  "East Timor", "Ecuador", "Egypt", "El Salvador", "Equatorial Guinea", "Eritrea",
  "Estonia", "Ethiopia", "Faeroe Islands", "Falkland Islands", "Fiji", "Finland",
  "Former Yugoslav Republic of Macedonia", "France", "French Guiana", "French Polynesia",
  "French Southern Territories", "Gabon", "Georgia", "Germany", "Ghana", "Gibraltar",
  "Greece", "Greenland", "Grenada", "Guadeloupe", "Guam", "Guatemala", "Guinea", "Guinea-Bissau",
  "Guyana", "Haiti", "Heard Island and McDonald Islands", "Honduras", "Hong Kong", "Hungary",
  "Iceland", "India", "Indonesia", "Iran", "Iraq", "Ireland", "Israel", "Italy", "Jamaica",
  "Japan", "Jordan", "Kazakhstan", "Kenya", "Kiribati", "Kuwait", "Kyrgyzstan", "Laos",
  "Latvia", "Lebanon", "Lesotho", "Liberia", "Libya", "Liechtenstein", "Lithuania", "Luxembourg",
  "Macau", "Madagascar", "Malawi", "Malaysia", "Maldives", "Mali", "Malta", "Marshall Islands",
  "Martinique", "Mauritania", "Mauritius", "Mayotte", "Mexico", "Micronesia", "Moldova",
  "Monaco", "Mongolia", "Montserrat", "Morocco", "Mozambique", "Myanmar", "Namibia",
  "Nauru", "Nepal", "Netherlands", "Netherlands Antilles", "New Caledonia", "New Zealand",
  "Nicaragua", "Niger", "Nigeria", "Niue", "Norfolk Island", "North Korea", "Northern Marianas",
  "Norway", "Oman", "Pakistan", "Palau", "Panama", "Papua New Guinea", "Paraguay", "Peru",
  "Philippines", "Pitcairn Islands", "Poland", "Portugal", "Puerto Rico", "Qatar",
  "Reunion", "Romania", "Russia", "Rwanda", "Sqo Tome and Principe", "Saint Helena",
  "Saint Kitts and Nevis", "Saint Lucia", "Saint Pierre and Miquelon",
  "Saint Vincent and the Grenadines", "Samoa", "San Marino", "Saudi Arabia", "Senegal",
  "Seychelles", "Sierra Leone", "Singapore", "Slovakia", "Slovenia", "Solomon Islands",
  "Somalia", "South Africa", "South Georgia and the South Sandwich Islands", "South Korea",
  "Spain", "Sri Lanka", "Sudan", "Suriname", "Svalbard and Jan Mayen", "Swaziland", "Sweden",
  "Switzerland", "Syria", "Taiwan", "Tajikistan", "Tanzania", "Thailand", "The Bahamas",
  "The Gambia", "Togo", "Tokelau", "Tonga", "Trinidad and Tobago", "Tunisia", "Turkey",
  "Turkmenistan", "Turks and Caicos Islands", "Tuvalu", "Virgin Islands", "Uganda",
  "Ukraine", "United Arab Emirates", "United Kingdom",
  "United States", "United States Minor Outlying Islands", "Uruguay", "Uzbekistan",
  "Vanuatu", "Vatican City", "Venezuela", "Vietnam", "Wallis and Futuna", "Western Sahara",
  "Yemen", "Yugoslavia", "Zambia", "Zimbabwe"
};
```

사용자가에 입력할 때 드롭다운 목록에 제공 되는 제안 목록입니다.[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
위젯.

애플리케이션을 실행합니다. 입력 하면 다음과 같은 내용이 표시 됩니다.

[!["Ca"가 포함 된 이름을 나열 하는 자동 완성 스크린샷 예](auto-complete-images/helloautocomplete.png)](auto-complete-images/helloautocomplete.png#lightbox)



## <a name="more-information"></a>추가 정보

하드 코드 된 문자열 배열을 사용 하는 것은 응용 프로그램 코드가 콘텐츠가 아닌 동작에 중점을 두기 때문에 권장 되는 디자인 습관은 아닙니다. 콘텐츠를 더 쉽게 수정 하 고 콘텐츠를 지역화 하는 것을 돕기 위해 문자열과 같은 응용 프로그램 콘텐츠를 코드에서 표면화 된 해야 합니다. 하드 코드 된 문자열은이 자습서에서 간단 하 고 집중 하는 데만 사용 됩니다.[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
위젯. 대신 응용 프로그램에서 이러한 문자열 배열을 XML 파일에 선언 해야 합니다. `<string-array>` 프로젝트`res/values/strings.xml` 파일에서 리소스를 사용 하 여이 작업을 수행할 수 있습니다. 예를 들어:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
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

에 [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)이러한 리소스 문자열을 사용 하려면 원래[`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)
다음을 사용 하는 생성자 줄:

```csharp
string[] countries = Resources.GetStringArray (Resource.array.countries_array);
var adapter = new ArrayAdapter<String> (this, Resource.layout.list_item, countries);
```


### <a name="references"></a>참조

-   [AutoCompleteTextView 조리법](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/autocomplete_text_view/add_an_autocomplete_text_input) 에 대 한 Xamarin Android 샘플 프로젝트 입니다.`AutoCompleteTextView` &ndash;
-   [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)
-   [`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)

*이 페이지의 일부는 Android 오픈 소스 프로젝트에서 만들고 공유 하 고*
[*Creative Commons 2.5 특성 라이선스*](http://creativecommons.org/licenses/by/2.5/) *에 설명 된 용어에 따라 사용 되는 작업을 기반으로 수정 됩니다. 이 자습서는*[*Android 자동 완성 자습서*](https://developer.android.com/resources/tutorials/views/hello-autocomplete.html)
를 기반으로
*합니다.*
