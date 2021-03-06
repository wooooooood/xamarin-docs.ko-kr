---
title: Xamarin Android 회전자
ms.prod: xamarin
ms.assetid: 004089E9-7C1D-2285-765A-B69143091F2A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: ba4a83eb997b879e8a2398f9857e2fd80221f8ef
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029162"
---
# <a name="xamarinandroid-spinner"></a>Xamarin Android 회전자

[`Spinner`](xref:Android.Widget.Spinner) 는 항목을 선택할 수 있는 드롭다운 목록을 표시 하는 위젯입니다. 이 가이드에서는 회전자에서 선택 항목 목록을 표시 하는 간단한 앱을 만든 다음 선택한 선택 항목과 관련 된 다른 값을 표시 하는 수정 방법을 설명 합니다.

## <a name="basic-spinner"></a>기본 회전자

이 자습서의 첫 번째 부분에서는 행성 목록을 표시 하는 간단한 회전자 위젯을 만듭니다. 행성을 선택 하면 선택 된 항목을 표시 하는 알림 메시지가 표시 됩니다.

[HelloSpinner 앱의 ![예제 스크린샷](spinner-images/01-example-screenshots-sml.png)](spinner-images/01-example-screenshots.png#lightbox)

**HelloSpinner**라는 새 프로젝트를 시작 합니다.

**Resources/Layout/Main. axml** 을 열고 다음 xml을 삽입 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:padding="10dip"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content">
    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="10dip"
        android:text="@string/planet_prompt"
    />
    <Spinner
        android:id="@+id/spinner"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:prompt="@string/planet_prompt"
    />
</LinearLayout>
```

[`TextView`](xref:Android.Widget.TextView)의 `android:text` 특성과 [`Spinner`](xref:Android.Widget.Spinner)의 `android:prompt` 특성은 모두 동일한 문자열 리소스를 참조 합니다. 이 텍스트는 위젯의 제목으로 동작 합니다. [`Spinner`](xref:Android.Widget.Spinner)에 적용 하면 위젯을 선택할 때 표시 되는 선택 대화 상자에 제목 텍스트가 표시 됩니다.

**리소스/값/문자열 .xml** 을 편집 하 고 다음과 같이 파일을 수정 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <string name="app_name">HelloSpinner</string>
  <string name="planet_prompt">Choose a planet</string>
  <string-array name="planets_array">
    <item>Mercury</item>
    <item>Venus</item>
    <item>Earth</item>
    <item>Mars</item>
    <item>Jupiter</item>
    <item>Saturn</item>
    <item>Uranus</item>
    <item>Neptune</item>
  </string-array>
</resources>
```

두 번째 `<string>` 요소는 [`TextView`](xref:Android.Widget.TextView) 에서 참조 하는 제목 문자열과 위의 레이아웃에 [`Spinner`](xref:Android.Widget.Spinner) 을 정의 합니다.
`<string-array>` 요소는 [`Spinner`](xref:Android.Widget.Spinner) 위젯에 목록으로 표시 되는 문자열 목록을 정의 합니다.

이제 **MainActivity.cs** 을 열고 다음 `using` 문을 추가 합니다.

```csharp
using System;
```

다음으로 [`OnCreate()`](xref:Android.App.Activity.OnCreate*)) 메서드에 대해 다음 코드를 삽입 합니다.

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "Main" layout resource
    SetContentView (Resource.Layout.Main);

    Spinner spinner = FindViewById<Spinner> (Resource.Id.spinner);

    spinner.ItemSelected += new EventHandler<AdapterView.ItemSelectedEventArgs> (spinner_ItemSelected);
    var adapter = ArrayAdapter.CreateFromResource (
            this, Resource.Array.planets_array, Android.Resource.Layout.SimpleSpinnerItem);

    adapter.SetDropDownViewResource (Android.Resource.Layout.SimpleSpinnerDropDownItem);
    spinner.Adapter = adapter;
}
```

`Main.axml` 레이아웃이 콘텐츠 뷰로 설정 된 후에는 [`Spinner`](xref:Android.Widget.Spinner) 위젯이 [`FindViewById<>(int)`](xref:Android.App.Activity.FindViewById*)레이아웃에서 캡처됩니다.
[`CreateFromResource()`](xref:Android.Widget.ArrayAdapter.CreateFromResource*)
그런 다음 메서드는 새 [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)를 만듭니다 .이는 문자열 배열의 각 항목을 [`Spinner`](xref:Android.Widget.Spinner) 의 초기 모양 (선택 하면 회전자에 표시 되는 방법)에 바인딩합니다. `Resource.Array.planets_array` ID는 위에 정의 된 `string-array`를 참조 하 고, `Android.Resource.Layout.SimpleSpinnerItem` ID는 플랫폼에서 정의한 표준 회전자 모양의 레이아웃을 참조 합니다.
[`SetDropDownViewResource`](xref:Android.Widget.ArrayAdapter.SetDropDownViewResource*)
위젯을 열 때 각 항목의 모양을 정의 하기 위해가 호출 됩니다. 마지막으로 [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter) 는 [`Adapter`](xref:Android.Widget.ArrayAdapter) 속성을 설정 하 여 모든 항목을 [`Spinner`](xref:Android.Widget.Spinner) 에 연결 하도록 설정 되어 있습니다.

이제 [`Spinner`](xref:Android.Widget.Spinner)에서 항목을 선택 했을 때 응용 프로그램을 notifys 콜백 메서드를 제공 합니다. 이 메서드는 다음과 같습니다.

```csharp
private void spinner_ItemSelected (object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format ("The planet is {0}", spinner.GetItemAtPosition (e.Position));
    Toast.MakeText (this, toast, ToastLength.Long).Show ();
}
```

항목이 선택 되 면 발신자는 항목에 액세스할 수 있도록 [`Spinner`](xref:Android.Widget.Spinner) 캐스팅 됩니다. `ItemEventArgs`에서 `Position` 속성을 사용 하 여 선택한 개체의 텍스트를 확인 하 고이를 사용 하 여 [`Toast`](xref:Android.Widget.Toast)를 표시할 수 있습니다.

응용 프로그램을 실행 합니다. 다음과 같이 표시 됩니다.

[![에서 Mars가 행성으로 선택 된 회전자의 예제 스크린샷](spinner-images/02-basic-example-sml.png)](spinner-images/02-basic-example.png#lightbox)

## <a name="spinner-using-keyvalue-pairs"></a>키/값 쌍을 사용 하 여 회전자

앱에서 사용 하는 특정 종류의 데이터와 연결 된 키 값을 표시 하는 데 `Spinner`를 사용 해야 하는 경우가 종종 있습니다. `Spinner` 키/값 쌍으로 직접 작동 하지 않기 때문에 키/값 쌍을 개별적으로 저장 하 고, `Spinner` 키 값으로 채운 다음, 회전자에서 선택한 키의 위치를 사용 하 여 연결 된 데이터 값을 조회 해야 합니다. 

다음 단계에서 **HelloSpinner** 앱은 선택한 행성의 평균 온도를 표시 하도록 수정 됩니다.

**MainActivity.cs**에 다음 `using` 문을 추가 합니다.

```csharp
using System.Collections.Generic;
```

`MainActivity` 클래스에 다음 인스턴스 변수를 추가 합니다.
이 목록에는 행성 및 평균 온도에 대 한 키/값 쌍이 포함 됩니다.

```csharp
private List<KeyValuePair<string, string>> planets;
```

`OnCreate` 메서드에서 `adapter` 선언 되기 전에 다음 코드를 추가 합니다.

```csharp
planets = new List<KeyValuePair<string, string>>
{
    new KeyValuePair<string, string>("Mercury", "167 degrees C"),
    new KeyValuePair<string, string>("Venus", "464 degrees C"),
    new KeyValuePair<string, string>("Earth", "15 degrees C"),
    new KeyValuePair<string, string>("Mars", "-65 degrees C"),
    new KeyValuePair<string, string>("Jupiter" , "-110 degrees C"),
    new KeyValuePair<string, string>("Saturn", "-140 degrees C"),
    new KeyValuePair<string, string>("Uranus", "-195 degrees C"),
    new KeyValuePair<string, string>("Neptune", "-200 degrees C")
};
```

이 코드는 행성 및 연관 된 평균 온도에 대 한 간단한 저장소를 만듭니다. 실제 응용 프로그램에서는 일반적으로 데이터베이스를 사용 하 여 키와 연결 된 데이터를 저장 합니다.

위의 코드 바로 뒤에 다음 줄을 추가 하 여 키를 추출 하 고 목록에 배치 합니다 (순서 대로).

```csharp
List<string> planetNames = new List<string>();
foreach (var item in planets)
    planetNames.Add (item.Key);
```

이 목록을 `planets_array` 리소스 대신 `ArrayAdapter` 생성자에 전달 합니다.

```csharp
var adapter = new ArrayAdapter<string>(this,
    Android.Resource.Layout.SimpleSpinnerItem, planetNames);
```

선택한 지구와 연결 된 값 (온도)을 조회 하는 데 선택한 위치를 사용 하도록 `spinner_ItemSelected`를 수정 합니다.

```csharp
private void spinner_ItemSelected(object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format("The mean temperature for planet {0} is {1}",
        spinner.GetItemAtPosition(e.Position), planets[e.Position].Value);
    Toast.MakeText(this, toast, ToastLength.Long).Show();
}
```

응용 프로그램을 실행 합니다. 알림 메시지는 다음과 같습니다.

[온도를 표시 하는 행성 선택의 예 ![](spinner-images/03-keyvalue-example-sml.png)](spinner-images/03-keyvalue-example.png#lightbox)

## <a name="resources"></a>리소스

- [`Resource.Layout`](xref:Android.Resource.Layout)
- [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)
- [`Spinner`](xref:Android.Widget.Spinner)

*이 페이지의 일부는 Android 오픈 소스 프로젝트에서 만들고 공유한 작업을 기반으로 하 고*
[*Creative Commons 2.5 특성 라이선스*](https://creativecommons.org/licenses/by/2.5/)에 설명 된 용어에 따라 사용 되는 수정 내용입니다.
