---
title: RadioButton
ms.prod: xamarin
ms.assetid: 3C32EA3F-D917-C988-72C5-A17354DA791E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 9f51adcbd1accb4f780318cc0853e612ed8e5bed
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029311"
---
# <a name="radiobutton"></a>RadioButton

이 섹션에서는 둘 중 하나를 사용 하지 않도록 설정할 수 있는 두 개의 상호 배타적인 라디오 단추를 만든 다음 [`RadioGroup`](xref:Android.Widget.RadioGroup)
및 [`RadioButton`](xref:Android.Widget.RadioButton)
위젯. 라디오 단추 중 하나를 누르면 알림 메시지가 표시 됩니다.

**Resources/layout/Main. axml** 파일을 열고 [`RadioGroup`](xref:Android.Widget.RadioGroup) 에 중첩 된 두 개의 [`RadioButton`](xref:Android.Widget.RadioButton)s를 추가 합니다 ( [`LinearLayout`](xref:Android.Widget.LinearLayout)내).

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

한 번에 둘 이상의 항목을 선택할 수 있도록 [`RadioButton`](xref:Android.Widget.RadioButton)s를 [`RadioGroup`](xref:Android.Widget.RadioGroup) 요소에 함께 그룹화 하는 것이 중요 합니다. 이 논리는 Android 시스템에서 자동으로 처리 됩니다. 한 [`RadioButton`](xref:Android.Widget.RadioButton)
그룹 내에서 다른 모든 사용자가 자동으로 선택 취소 됩니다.

각 [`RadioButton`](xref:Android.Widget.RadioButton) 를 선택 하는 경우 작업을 수행 하려면 이벤트 처리기를 작성 해야 합니다.

```csharp
private void RadioButtonClick (object sender, EventArgs e)
{
    RadioButton rb = (RadioButton)sender;
    Toast.MakeText (this, rb.Text, ToastLength.Short).Show ();
}
```

먼저 전달 된 발신자가 RadioButton으로 캐스팅 됩니다.
그런 다음 [`Toast`](xref:Android.Widget.Toast)
메시지 선택한 라디오 단추의 텍스트를 표시 합니다.

이제 [`OnCreate()`](xref:Android.App.Activity.OnCreate*) 의 맨 아래에
메서드를 추가 하 고 다음을 추가 합니다.

```csharp
RadioButton radio_red = FindViewById<RadioButton>(Resource.Id.radio_red);
RadioButton radio_blue = FindViewById<RadioButton>(Resource.Id.radio_blue);

radio_red.Click += RadioButtonClick;
radio_blue.Click += RadioButtonClick;
```

그러면 레이아웃에서 각 [`RadioButton`](xref:Android.Widget.RadioButton)s가 캡처되고 새로 만든 이벤트 handlerto 각각에 추가 됩니다.

애플리케이션을 실행합니다.

> [!TIP]
> 저장 된 [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)를 로드 하는 경우와 같이 사용자가 직접 상태를 변경 해야 하는 경우 [`Checked`](xref:Android.Widget.CompoundButton.Checked) 를 사용 합니다.
> 속성 setter 또는 [`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)
> 메서드를 재정의합니다.

*이 페이지의 일부는 Android 오픈 소스 프로젝트에서 만들고 공유한 작업을 기반으로 하 고*
[*Creative Commons 2.5 특성 라이선스*](https://creativecommons.org/licenses/by/2.5/)에 설명 된 용어에 따라 사용 되는 수정 내용입니다. 
