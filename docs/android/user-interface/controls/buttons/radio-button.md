---
title: RadioButton
ms.prod: xamarin
ms.assetid: 3C32EA3F-D917-C988-72C5-A17354DA791E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 1267491f2d9b7519f76651df059722420fa8e1eb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763116"
---
# <a name="radiobutton"></a>RadioButton

이 섹션에서는 만듭니다 (사용할 수 있도록 하나 비활성화 다른) 두 개의 상호 배타적인 라디오 단추를 사용 하 여는 [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) 및 [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) 위젯입니다. 어느 라디오 단추를 누를 때 알림 메시지가 표시 됩니다.


열기는 **Resources/layout/Main.axml** 파일을 두 개의 추가 [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)에 중첩 된 s는 [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) (내는 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

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

것이 중요 하는 [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s 별로 함께 그룹화 되어는 [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) 요소를 한 번에 하나만 선택할 수 있습니다. 이 논리는 Android 시스템에 의해 자동으로 처리 됩니다. 한 경우 [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) 내에서 그룹을 선택 하면 다른 모든 형식은 자동으로 선택 취소 합니다.

작업을 수행할 때 각 [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) 은 이벤트 처리기를 작성 해야을 선택 합니다.

```csharp
private void RadioButtonClick (object sender, EventArgs e)
{
    RadioButton rb = (RadioButton)sender;
    Toast.MakeText (this, rb.Text, ToastLength.Short).Show ();
}
```

첫째, 보낸 사람에 게 전달 되는 RadioButton으로 캐스팅 됩니다.
[ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) 메시지 선택 된 라디오 단추의 텍스트를 표시 합니다.

이제 맨 아래에 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle) 메서드를 다음에 추가 합니다.

```csharp
RadioButton radio_red = FindViewById<RadioButton>(Resource.Id.radio_red);
RadioButton radio_blue = FindViewById<RadioButton>(Resource.Id.radio_blue);

radio_red.Click += RadioButtonClick;
radio_blue.Click += RadioButtonClick;
```

각각의 캡처합니다는 [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)레이아웃에서 s는 새로 만든 이벤트 handlerto 각 추가 합니다.

응용 프로그램을 실행합니다.

**팁:** 상태를 변경 해야 할 경우 (예를 들어 로드 하는 저장 된 [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/))를 사용 하 여는 [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/) 속성 setter 또는 [ `Toggle()` ](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/) 메서드.

*이 페이지의 일부는 Android 열려 있는 소스 프로젝트에서 공유 하 고 만들고에 설명 된 조건에 따라 사용 작업에 따라 수정 된*
[*Creative Commons 2.5 Attribution 라이선스* ](http://creativecommons.org/licenses/by/2.5/). 
