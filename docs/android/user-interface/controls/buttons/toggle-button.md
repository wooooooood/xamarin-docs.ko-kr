---
title: ToggleButton
ms.prod: xamarin
ms.assetid: 9ADA8FCF-63ED-897A-DD56-D7D86535A92C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 22eb8f999450ed8fb46b1f7809c92540be13aa65
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105924"
---
# <a name="togglebutton"></a>ToggleButton

이 섹션에서는 특히 사용 하 여 두 상태 간의 전환에 사용 되는 단추 만듭니다는 [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) 위젯입니다. 이 위젯에 라디오 단추에 뛰어난 대안 상호 배타적인 두 개의 간단한 상태에 있는 경우 ("on" 및 "off" 예를 들어). Android 4.0 (API 수준 14) 토글 단추 라고도 하는 대신 도입 된 [ `Switch` ](https://developer.xamarin.com/api/type/Android.Widget.Switch/)합니다.

예는 **ToggleButton** 이미지의 오른쪽 위 쌍의 예를 제공 하는 동안 이미지의 왼쪽 위 쌍에서 볼 수 있습니다는 **스위치**:

![켜고 상태에서 전환 및 ToggleButtons의 예](toggle-button-images/togglebutton-switch.png)  

응용 프로그램에서 사용 하는 컨트롤은 스타일의 문제입니다. 두 위젯 기능적으로 동일합니다.

열기를 **Resources/layout/Main.axml** 파일을 추가 합니다 [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) 요소 (내 합니다 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

항목의 상태가 변경 된 경우를 위해 다음 코드의 끝에 추가 합니다 [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle)
방법:

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

캡처합니다 합니다 [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) 요소를 레이아웃에서 단추를 클릭할 때 수행할 동작을 정의 하는 Click 이벤트를 처리 합니다. 이 예제에서는 메서드 단추의 새 상태를 확인 후 표시를 [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) 현재 상태를 나타내는 메시지입니다.

있음을 합니다 [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) 만 요청 된 있도록 checked 및 unchecked, 간에 변경 되는 자체 상태 처리 합니다.

응용 프로그램을 실행합니다.


**팁:** 상태를 변경 해야 할 경우 (때와 같이 저장 된 로드 [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/))를 사용 합니다 [`Checked`](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/)
속성 setter 또는 [`Toggle()`](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/)
메서드를 재정의합니다.


## <a name="related-links"></a>관련 링크

- [토글 단추](http://developer.android.com/reference/android/widget/ToggleButton.html)
- [스위치](http://developer.android.com/reference/android/widget/Switch.html)
