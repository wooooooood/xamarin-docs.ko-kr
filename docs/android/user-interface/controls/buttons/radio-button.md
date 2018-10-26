---
title: RadioButton
ms.prod: xamarin
ms.assetid: 3C32EA3F-D917-C988-72C5-A17354DA791E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: be473580b24dba6b4f08384771e2097d368f8dc8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123598"
---
# <a name="radiobutton"></a>RadioButton

이 섹션에서는 만들게 됩니다 (사용 하도록 설정 하면 한 다른 비활성화) 두 개의 상호 배타적인 라디오 단추를 사용 하 여 [`RadioGroup`](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/)
및 [`RadioButton`](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)
위젯입니다. 라디오 단추 중 하나를 누를 때 알림 메시지가 표시 됩니다.


열기는 **Resources/layout/Main.axml** 파일을 두 개의 추가 [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s, 중첩을 [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) (내부를 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

```xml
<RadioGroup
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"
  android:orientation="vertical">
  <RadioButton android:id="@+id/radio_red"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Red" />
  <RadioButton android:id="@+id/radio_blue"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Blue" />
</RadioGroup>
```

것이 중요 하는 [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s에서 함께 그룹화 됩니다는 [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) 요소 개 이상를 한 번에 선택할 수 있도록 합니다. 이 논리는 Android 시스템에서 자동으로 처리 됩니다. 한 경우 [`RadioButton`](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)
내에서 그룹을 선택 하면 나머지는 자동으로 선택 취소 합니다.

작업을 수행 하는 경우 각 [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) 는 이벤트 처리기를 작성 해야 선택:

```csharp
private void RadioButtonClick (object sender, EventArgs e)
{
    RadioButton rb = (RadioButton)sender;
    Toast.MakeText (this, rb.Text, ToastLength.Short).Show ();
}
```

첫째, 보낸 사람에 게 전달 되는 라디오 단추에 캐스팅 됩니다.
는 [`Toast`](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
메시지는 선택된 된 라디오 단추의 텍스트를 표시합니다.

이제 맨 아래에 [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle)
메서드를 다음을 추가 합니다.

```csharp
RadioButton radio_red = FindViewById<RadioButton>(Resource.Id.radio_red);
RadioButton radio_blue = FindViewById<RadioButton>(Resource.Id.radio_blue);

radio_red.Click += RadioButtonClick;
radio_blue.Click += RadioButtonClick;
```

각 캡처합니다 합니다 [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)레이아웃에서 s는 새로 만든 이벤트 handlerto 각 추가 합니다.

응용 프로그램을 실행합니다.

**팁:** 상태를 변경 해야 할 경우 (때와 같이 저장 된 로드 [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/))를 사용 합니다 [`Checked`](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/)
속성 setter 또는 [`Toggle()`](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/)
메서드를 재정의합니다.

*이 페이지의 일부는 생성 하 고 Android Open Source Project에서 공유 된 조건에 따라 사용 되는 작업에 따라 수정 합니다*
[*Creative Commons 2.5 Attribution License* ](http://creativecommons.org/licenses/by/2.5/). 
