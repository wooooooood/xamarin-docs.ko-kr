---
title: CheckBox
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 06908ad8993eb6d47476006b23865fa1c7fe694f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029341"
---
# <a name="checkbox"></a>CheckBox

이 섹션에서는 [`CheckBox`](xref:Android.Widget.CheckBox) 위젯을 사용 하 여 항목을 선택 하는 확인란을 만듭니다. 확인란을 누르면 알림 메시지에 확인란의 현재 상태가 표시 됩니다.

**Resources/layout/Main. axml** 파일을 열고 [`CheckBox`](xref:Android.Widget.CheckBox) 요소 ( [`LinearLayout`](xref:Android.Widget.LinearLayout)내)를 추가 합니다.

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

상태가 변경 될 때 작업을 수행 하려면 [`OnCreate()`](xref:Android.App.Activity.OnCreate*) 메서드의 끝에 다음 코드를 추가 합니다.

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

그러면 레이아웃에서 [`CheckBox`](xref:Android.Widget.CheckBox) 요소가 캡처되고 클릭 된 후 확인란을 클릭할 때 수행할 동작을 정의 하는 클릭 이벤트가 처리 됩니다. 이 단추를 클릭 하면 [`Checked`](xref:Android.Widget.CompoundButton.Checked) 속성이 호출 되어 확인란의 새 상태를 확인 합니다. 확인란이 선택 된 [`Toast`](xref:Android.Widget.Toast) 경우 "선택" 메시지가 표시 되 고 그렇지 않으면 "선택 되지 않음"이 표시 됩니다. [`CheckBox`](xref:Android.Widget.CheckBox) 는 자신의 상태 변경을 처리 하므로 현재 상태만 쿼리해야 합니다.

실행 합니다.

> [!TIP]
> 저장 된 [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)를 로드 하는 경우와 같이 사용자가 직접 상태를 변경 해야 하는 경우 [`Checked`](xref:Android.Widget.CompoundButton.Checked) 속성 setter 또는 [`Toggle()`](xref:Android.Widget.CompoundButton.Toggle) 메서드를 사용 합니다.

*이 페이지의 일부는 Android 오픈 소스 프로젝트에서 만들고 공유하는 작업을 기반으로 한 수정사항이며*  [*Creative Commons 2.5 Attribution License*](https://creativecommons.org/licenses/by/2.5/)에 설명된 용어에 따라 사용됩니다.
