---
title: CheckBox
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: b947217706fc8ef7ce7945bf4c88349f4367ffcd
ms.sourcegitcommit: cf56d2bae34dc0f8e94c2d3d28d5f460d59807bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985672"
---
# <a name="checkbox"></a>CheckBox

이 섹션에서는 [`CheckBox`](xref:Android.Widget.CheckBox) 위젯을 사용 하 여 항목 선택을 위한 확인란을 만듭니다. 확인란을 누르면 알림 메시지에 확인란의 현재 상태가 표시 됩니다.

**Resources/layout/Main. axml** 파일을 열고 요소를 [`CheckBox`](xref:Android.Widget.CheckBox) 에 [`LinearLayout`](xref:Android.Widget.LinearLayout)추가 합니다.

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

상태가 변경 될 때 작업을 수행 하려면 다음 코드를 [`OnCreate()`](xref:Android.App.Activity.OnCreate*) 메서드의 끝에 추가 합니다.

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

그러면 레이아웃에서 [`CheckBox`](xref:Android.Widget.CheckBox) 요소를 캡처하고 클릭 하 여 확인란을 클릭할 때 수행할 동작을 정의 하는 클릭 이벤트를 처리 합니다. [`Checked`](xref:Android.Widget.CompoundButton.Checked) 속성을 클릭 하면 확인란의 새 상태를 확인 하기 위해가 호출 됩니다. 확인란이 선택 된 경우는 [`Toast`](xref:Android.Widget.Toast) "선택 된" 메시지를 표시 하 고, 그렇지 않으면 "선택 되지 않음"으로 표시 합니다. 는 [`CheckBox`](xref:Android.Widget.CheckBox) 자신의 상태 변경을 처리 하므로 현재 상태만 쿼리해야 합니다.

실행 합니다.

> [!TIP]
> 저장 [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)된를 로드 하는 경우와 같이 사용자가 직접 상태를 변경 해야 하는 [`Checked`](xref:Android.Widget.CompoundButton.Checked) 경우 속성 setter [`Toggle()`](xref:Android.Widget.CompoundButton.Toggle) 또는 메서드를 사용 합니다.

*이 페이지의 일부는 Android 오픈 소스 프로젝트에서 만들고 공유한 작업을 기반으로 하 고,에 설명 된 용어에 따라 사용 됩니다* . [*Creative Commons 2.5 특성 라이선스*](http://creativecommons.org/licenses/by/2.5/).
