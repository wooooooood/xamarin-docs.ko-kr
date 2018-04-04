---
title: ToggleButton
ms.prod: xamarin
ms.assetid: 9ADA8FCF-63ED-897A-DD56-D7D86535A92C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 8323c456a97a033e19374a4bd3ea7468ecafa608
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="togglebutton"></a>ToggleButton

이 섹션에서는 특별히 사용 하 여 두 상태 간의 전환에 사용 되는 단추 만듭니다는 [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) 위젯입니다. 상호 배타적인 두 개의 간단한 상태에 있는 경우이 위젯은 라디오 단추에는 뛰어난 대신 하는 ("on" 및 "off" 이며, 예를 들어). Android 4.0 (API 수준 14) 토글 단추 라고 하는 대신 도입는 [ `Switch` ](https://developer.xamarin.com/api/type/Android.Widget.Switch/)합니다.

예로 **ToggleButton** 이미지의 오른쪽 쌍의 예를 제공 하는 동안와 이미지의 왼쪽 쌍에서 볼 수는 **스위치**:

![스위치 및 ToggleButtons 모두 켜고 상태 예제](toggle-button-images/togglebutton-switch.png)  

응용 프로그램에서 사용 하는 컨트롤은 스타일의 문제입니다. 두 위젯 기능적으로 동일합니다.

열기는 **Resources/layout/Main.axml** 파일을 추가 [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) 요소 (내는 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

작업을 수행할 상태가 변경 되 면, 다음 코드의 끝에 추가 된 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle) 메서드:

```csharp
ToggleButton togglebutton = FindViewById<ToggleButton>(Resource.Id.togglebutton);

togglebutton.Click += (o, e) => {
    // Perform action on clicks
    if (togglebutton.Checked)
        Toast.MakeText(this, "Checked", ToastLength.Short).Show ();
    else
        Toast.MakeText(this, "Not checked", ToastLength.Short).Show ();
};
```

캡처합니다는 [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) 레이아웃에서 요소는 단추를 클릭할 때 수행할 동작을 정의 하는 Click 이벤트를 처리 합니다. 이 예제에서는 메서드 단추의 새 상태를 확인 한 다음 표시는 [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) 현재 상태를 나타내는 메시지입니다.

에 [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) 말 게 않았습니다 하도록 checked 및 unchecked, 사이 자신의 상태를 변경 하는 핸들입니다.

응용 프로그램을 실행합니다.


**팁:** 상태를 변경 해야 할 경우 (예를 들어 로드 하는 저장 된 [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/))를 사용 하 여는 [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/) 속성 setter 또는 [ `Toggle()` ](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/) 메서드.


## <a name="related-links"></a>관련 링크

- [ToggleButton](http://developer.android.com/reference/android/widget/ToggleButton.html)
- [스위치](http://developer.android.com/reference/android/widget/Switch.html)
