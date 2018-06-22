---
title: CheckBox
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 85c505d03e7a763b24fb176b6a94c0fe43009b79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30765755"
---
# <a name="checkbox"></a>CheckBox

이 섹션에서는 만듭니다 항목 선택을 위한 확인란을 사용 하 여는 [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox) 위젯입니다. checkbox를 누른 경우 알림 메시지는 확인란의 현재 상태를 나타냅니다.

열기는 **Resources/layout/Main.axml** 파일을 추가 [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) 요소 (내는 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout)):

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

작업을 수행할 상태가 변경 되 면, 다음 코드의 끝에 추가 된 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle) 메서드:

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

이 캡처의 [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) 레이아웃에서 요소에는 다음 확인란을 클릭할 때 수행 되어야 하는 동작을 정의 하는 Click 이벤트를 처리 합니다. 를 클릭 하면는 [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/) 속성 확인란의 새 상태를 확인 하기 위해 호출 됩니다. 검사 된 경우는 [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) "해제" 표시 "선택 됨", 그렇지 않으면 메시지가 표시 됩니다. [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) 되므로 현재 상태를 쿼리할 필요가 자체 상태 변경 사항을 처리 합니다.

실행 합니다.

**팁:** 상태를 변경 해야 할 경우 (예를 들어 로드 하는 저장 된 [ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference)를 사용 하 여는 [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked) 속성 setter 또는 [ `Toggle()` ](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle) 메서드.

*이 페이지의 일부는 Android 열려 있는 소스 프로젝트에서 공유 하 고 만들고에 설명 된 조건에 따라 사용 작업에 따라 수정 된*
[*Creative Commons 2.5 Attribution 라이선스* ](http://creativecommons.org/licenses/by/2.5/).
