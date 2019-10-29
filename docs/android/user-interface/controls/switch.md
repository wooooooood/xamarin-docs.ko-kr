---
title: Xamarin Android 스위치
description: Xamarin Android 응용 프로그램에서 스위치 위젯을 사용 하는 방법
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/29/2018
ms.openlocfilehash: 73becb5e4424854c9be6cdc3554f6cf93507b9a9
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029133"
---
# <a name="xamarinandroid-switch"></a>Xamarin Android 스위치

`Switch` 위젯 (아래 참조)을 사용 하면 사용자가 설정 또는 해제와 같은 두 가지 상태를 전환할 수 있습니다. `Switch` 기본값은 OFF입니다. 위젯은 다음과 같이 ON 및 OFF 상태 모두에 표시 됩니다.

[OFF 및 ON 상태에서 스위치 위젯의 스크린샷![](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)

## <a name="creating-a-switch"></a>스위치 만들기

스위치를 만들려면 다음과 같이 XML에서 `Switch` 요소를 선언 하면 됩니다.

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

그러면 다음과 같이 기본 스위치가 생성 됩니다.

[OFF 상태의 스위치를 표시 하는 데모 앱의![스크린샷](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)

## <a name="changing-default-values"></a>기본값 변경

컨트롤이 켜기 및 끄기 상태에 대해 표시 하는 텍스트와 기본값을 구성할 수 있습니다. 예를 들어 스위치를 기본적으로 ON으로 설정 하 고 OFF/NO/YES를 읽도록 설정 하면 다음 XML에서 `checked`, `textOn`및 `textOff` 특성을 설정할 수 있습니다.

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

## <a name="providing-a-title"></a>제목 제공

`Switch` 위젯은 다음과 같이 `text` 특성을 설정 하 여 텍스트 레이블 포함도 지원 합니다.

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

이 태그는 런타임에 다음 스크린샷을 생성 합니다.

[스위치 위젯 앞에 텍스트를 가로로 포함 하는 데모 앱의 스크린샷![](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

`Switch`값이 변경 되 면 `CheckedChange` 이벤트가 발생 합니다.
예를 들어 다음 코드에서는이 이벤트를 캡처하고 `CompoundButton.CheckedChangeEventArg` 인수의 일부로 이벤트 처리기에 전달 되는 `Switch`의 `isChecked` 값을 기반으로 하 `Toast` 위젯을 표시 합니다.

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```

## <a name="related-links"></a>관련 링크

- [SwitchDemo (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/switchdemo)
- [탭 레이아웃 자습서](~/android/user-interface/layouts/tab-layout/index.md)
