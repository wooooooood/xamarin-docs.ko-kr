---
title: CheckBox
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: f6f594d86cab8b1173ee9f67402862e1ec2890b2
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510367"
---
# <a name="checkbox"></a>CheckBox

이 섹션에서는 다음을 사용 하 여 항목 선택을 위한 확인란을 만듭니다.[`CheckBox`](xref:Android.Widget.CheckBox)
위젯. 확인란을 누르면 알림 메시지에 확인란의 현재 상태가 표시 됩니다.

**Resources/layout/Main. axml** 파일을 열고 요소를 [`CheckBox`](xref:Android.Widget.CheckBox) 에 [`LinearLayout`](xref:Android.Widget.LinearLayout)추가 합니다.

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

상태가 변경 될 때 작업을 수행 하려면 다음 코드를의 끝에 추가 합니다.[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
방법이

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

그러면[`CheckBox`](xref:Android.Widget.CheckBox)
그런 다음 레이아웃의 요소는 Click 이벤트를 처리 합니다 .이 이벤트는 확인란을 클릭할 때 수행할 동작을 정의 합니다. 클릭 하면[`Checked`](xref:Android.Widget.CompoundButton.Checked)
속성을 호출 하 여 확인란의 새 상태를 확인 합니다. 확인 된 경우[`Toast`](xref:Android.Widget.Toast)
"선택 된" 메시지를 표시 하 고, 그렇지 않으면 "선택 되지 않음"으로 표시 합니다. 여[`CheckBox`](xref:Android.Widget.CheckBox)
는 자신의 상태 변경을 처리 하므로 현재 상태만 쿼리해야 합니다.

실행 합니다.

> [!TIP]
> 저장 [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)된를 로드 하는 경우와 같이 사용자가 직접 상태를 변경 해야 하는 경우[`Checked`](xref:Android.Widget.CompoundButton.Checked)
> 속성 setter 또는[`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)
> 메서드를 재정의합니다.

*이 페이지의 일부는 Android 오픈 소스 프로젝트에서 만들고 공유 하 고*
[*Creative Commons 2.5 특성 라이선스*](http://creativecommons.org/licenses/by/2.5/)에 설명 된 용어에 따라 사용 되는 작업을 기반으로 수정 됩니다.
