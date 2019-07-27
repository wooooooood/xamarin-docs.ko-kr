---
title: ToggleButton
ms.prod: xamarin
ms.assetid: 9ADA8FCF-63ED-897A-DD56-D7D86535A92C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 91003f9a23c667b38028a9852b28dba656ba13db
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510330"
---
# <a name="togglebutton"></a>ToggleButton

이 섹션에서는 [`ToggleButton`](xref:Android.Widget.ToggleButton) 위젯을 사용 하 여 두 상태를 전환 하는 데 특별히 사용 되는 단추를 만듭니다. 이 위젯은 함께 사용할 수 없는 두 개의 간단한 상태 (예: "on" 및 "off")가 있는 경우 라디오 단추 대신 사용할 수 있습니다. Android 4.0 (API 수준 14)에는 [`Switch`](xref:Android.Widget.Switch)라고 하는 설정/해제 단추에 대 한 대안이 도입 되었습니다.

**ToggleButton** 의 예제는 이미지의 왼쪽 쌍에 표시 되는 반면 이미지의 오른쪽 쌍은 **스위치**의 예를 보여 줍니다.

![On 및 off 상태의 스위치 및 ToggleButtons 예](toggle-button-images/togglebutton-switch.png)  

응용 프로그램에서 사용 하는 컨트롤은 스타일의 문제입니다. 두 위젯은 기능적으로 동일 합니다.

**Resources/layout/Main. axml** 파일을 열고 요소를 [`ToggleButton`](xref:Android.Widget.ToggleButton) 에 [`LinearLayout`](xref:Android.Widget.LinearLayout)추가 합니다.

상태가 변경 될 때 작업을 수행 하려면 다음 코드를의 끝에 추가 합니다.[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
방법이

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

이는 레이아웃 [`ToggleButton`](xref:Android.Widget.ToggleButton) 에서 요소를 캡처하고 단추를 클릭할 때 수행할 동작을 정의 하는 Click 이벤트를 처리 합니다. 이 예제에서 메서드는 단추의 새 상태를 확인 한 다음 현재 상태를 나타내는 [`Toast`](xref:Android.Widget.Toast) 메시지를 표시 합니다.

는 [`ToggleButton`](xref:Android.Widget.ToggleButton) checked와 unchecked 사이에 자신의 상태 변경을 처리 하므로 사용자에 게 요청 하는 것을 확인 합니다.

애플리케이션을 실행합니다.


> [!TIP]
> 저장 [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)된를 로드 하는 경우와 같이 사용자가 직접 상태를 변경 해야 하는 경우 다음을 사용 합니다.[`Checked`](xref:Android.Widget.CompoundButton.Checked)
> 속성 setter 또는[`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)
> 메서드를 재정의합니다.


## <a name="related-links"></a>관련 링크

- [ToggleButton](https://developer.android.com/reference/android/widget/ToggleButton.html)
- [스위치](https://developer.android.com/reference/android/widget/Switch.html)
