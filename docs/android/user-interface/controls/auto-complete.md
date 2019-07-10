---
title: 자동 완성
ms.prod: xamarin
ms.assetid: D4C8CA49-8369-35B7-798D-B147FDC24185
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/31/2018
ms.openlocfilehash: bcbab6deddf5e1a4782cd382623f22281a0823e6
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67674861"
---
# <a name="auto-complete"></a>자동 완성

`AutoCompleteTextView` 사용자가 입력 하는 동안 자동으로 완성 제안을 표시 하는 편집 가능한 텍스트 뷰 요소가입니다. 추천 단어 목록은 사용자와 입력란의 내용을 바꾸려면 항목을 선택할 수 있는 메뉴 드롭다운에 표시 됩니다.

![자동 완성의 예제](images/auto-complete.png)

## <a name="overview"></a>개요

자동 완성 제안 사항을 제공 하는 텍스트 항목 위젯을 만들려면 사용 합니다 [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
widget. 제안을 통해 위젯과 사용 하 여 연결 문자열의 컬렉션에서 수신 되는 [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)합니다.

이 자습서에서는 만듭니다는 [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
국가 이름에 대 한 제안을 제공 하는 위젯입니다.

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

합니다 [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 소개 하는 레이블이 합니다 [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
widget.


## <a name="tutorial"></a>자습서

명명 된 새 프로젝트를 시작 *HelloAutoComplete*합니다.

라는 XML 파일을 만듭니다 `list_item.xml` 내에서 저장 된 **리소스/레이아웃** 폴더. 이 파일의 빌드 작업 설정 `AndroidResource`합니다. 다음과 같이 파일을 편집 합니다.

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

이 파일은 간단한 정의 [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 제안 목록에 표시 되는 각 항목에 대해 사용할 합니다.

오픈 **Resources/Layout/Main.axml** 하 고 다음을 삽입 합니다.

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

오픈 **MainActivity.cs** 에 다음 코드를 삽입 합니다 [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle))
방법:

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

콘텐츠 뷰 설정한 후의 `main.xml` 레이아웃에는 [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
위젯을 사용 하 여 레이아웃에서 캡처된 [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/)합니다. 새 [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) 바인딩할 초기화 됩니다는 `list_item.xml` 레이아웃의 각 목록 항목에는 `COUNTRIES` (다음 단계에서 정의 됨) 하는 문자열 배열입니다. 마지막으로 `SetAdapter()` 연결할 라고 합니다 [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) 와 [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
위젯 제안 목록을 채울 문자열 배열 되도록 합니다.

내부는 `MainActivity` 클래스, 문자열 배열에 추가 합니다.

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

이 사용자에 입력 하는 경우 드롭다운 목록에서 제공 되는 추천 단어 목록을 합니다 [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
widget.

애플리케이션을 실행합니다. 입력할 때 다음과 같이 표시 됩니다.

[![자동 완성 스크린샷 예제 "ca"를 포함 하는 이름을 나열 합니다.](auto-complete-images/helloautocomplete.png)](auto-complete-images/helloautocomplete.png#lightbox)



## <a name="more-information"></a>추가 정보

이때 하드 코드 된 문자열 배열을 사용 하 여 응용 프로그램 코드 동작을 콘텐츠에 집중 해야 하기 때문에 권장 되는 디자인 방법은 아닙니다. 문자열과 같은 응용 프로그램 콘텐츠는 수정 내용 쉽게 및 콘텐츠 지역화를 용이 하 게 하는 코드에서 구체화 해야 합니다. 하드 코드 된 문자열에만 쉽게 집중할이 자습서에 사용 되는 [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
widget. 대신, 응용 프로그램에 XML 파일에 이러한 문자열 배열을 선언 해야 합니다. 사용 하 여이 작업을 수행할 수 있습니다는 `<string-array>` 프로젝트에서 리소스 `res/values/strings.xml` 파일입니다. 예를 들어:

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

에 대 한 이러한 리소스 문자열을 사용 하는 [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)를 바꾸려면 [`ArrayAdapter`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)
다음을 사용 하 여 생성자 줄:

```csharp
string[] countries = Resources.GetStringArray (Resource.array.countries_array);
var adapter = new ArrayAdapter<String> (this, Resource.layout.list_item, countries);
```


### <a name="references"></a>참조

-   [AutoCompleteTextView 레](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/autocomplete_text_view/add_an_autocomplete_text_input) &ndash; 에 대 한 Xamarin.Android 샘플 프로젝트는 `AutoCompleteTextView`합니다.
-   [`ArrayAdapter`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)
-   [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)

*이 페이지의 일부는 생성 하 고 Android Open Source Project에서 공유 된 조건에 따라 사용 되는 작업에 따라 수정 합니다*
[*Creative Commons 2.5 Attribution License* ](http://creativecommons.org/licenses/by/2.5/) *. 이 자습서는 기반으로 합니다*
[*Android 자동 완성 자습서*](https://developer.android.com/resources/tutorials/views/hello-autocomplete.html)
*합니다.*
