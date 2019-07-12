---
title: CheckBox
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 9f2fd10e5cfe28206d323b2769517c2584919232
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67829718"
---
# <a name="checkbox"></a>CheckBox

이 섹션에서는 만들려는 항목을 선택 하는 것에 대 한 확인란을 사용 하 여 [`CheckBox`](https://developer.xamarin.com/api/type/Android.Widget.CheckBox)
widget. 확인란을 누르면 알림 메시지는 확인란의 현재 상태를 나타냅니다.

열기를 **Resources/layout/Main.axml** 파일을 추가 합니다 [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) 요소 (내 합니다 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout)):

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

항목의 상태가 변경 된 경우를 위해 다음 코드의 끝에 추가 합니다 [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle)
방법:

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

이 캡처는 [`CheckBox`](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/)
요소를 레이아웃에서 확인란을 클릭할 때 수행 되어야 하는 작업을 정의 하는 Click 이벤트를 처리 합니다. 클릭 하면는 [`Checked`](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/)
확인란의 새 상태를 확인 하려면 속성 이라고 합니다. 선택 된 경우는 [`Toast`](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
"취소" 표시 "선택 됨", 그렇지 않으면 메시지를 표시 합니다. 는 [`CheckBox`](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/)
현재 상태를 쿼리할 해야 하므로 자체 상태 변경 사항을 처리 합니다.

실행 합니다.

> [!TIP]
> 상태를 변경 해야 할 경우 (때와 같이 저장 된 로드 [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference)를 사용 합니다 [`Checked`](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked)
> 속성 setter 또는 [`Toggle()`](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle)
> 메서드를 재정의합니다.

*이 페이지의 일부는 생성 하 고 Android Open Source Project에서 공유 된 조건에 따라 사용 되는 작업에 따라 수정 합니다*
[*Creative Commons 2.5 Attribution License* ](http://creativecommons.org/licenses/by/2.5/).
