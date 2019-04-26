---
title: 전환
description: Xamarin.Android 응용 프로그램에서 스위치 위젯을 사용 하는 방법
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/29/2018
ms.openlocfilehash: ef400aaa31992b577762ad695418b865882e2e2d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075787"
---
# <a name="switch"></a>전환

`Switch` 위젯 (아래 참조)에서 이러한 두 상태 간의 전환 또는 해제 수 있습니다. `Switch` 기본값은 OFF입니다. 위젯에 ON 및 OFF 상태에 아래 표시 됩니다.

[![켜고 상태의 스위치 위젯의 스크린 샷](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)


## <a name="creating-a-switch"></a>스위치를 만들기

스위치를 만들려면 간단히 선언 된 `Switch` 다음과 같은 xml에서 요소:

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

이 아래와 같이 기본 스위치를 만듭니다.

[![스위치를 OFF 상태를 표시 하는 데모 앱의 스크린 샷](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)


## <a name="changing-default-values"></a>기본값 변경

ON 및 OFF 상태에 대 한 컨트롤이 표시 하는 텍스트와 기본값을 구성할 수 있습니다. 예를 들어, 기본적으로 ON 및 OFF/ON 대신 NO/예 읽기 스위치 되도록 설정할 수 있습니다 합니다 `checked`, `textOn`, 및 `textOff` 다음 XML의 특성입니다.

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```



## <a name="providing-a-title"></a>제목을 제공합니다.

합니다 `Switch` 위젯 설정 하 여 텍스트 레이블을 포함도 지원 합니다 `text` 특성을 다음과 같이:

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

이 태그는 런타임 시 다음 스크린 샷에서 생성합니다.

[![스위치 위젯 앞에 가로로 텍스트를 사용 하 여 데모 앱의 스크린 샷](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

경우는 `Switch`의 값이 변경 발생를 `CheckedChange` 이벤트.
예를 들어, 다음 코드에서는이 이벤트를 캡처하고 하 고 있는 `Toast` 메시지를 사용 하는 위젯이 기반으로 합니다 `isChecked` 값 `Switch`의 일부로 이벤트 처리기에 전달 되는 `CompoundButton.CheckedChangeEventArg` 인수.

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```


## <a name="related-links"></a>관련 링크

- [SwitchDemo (샘플)](https://developer.xamarin.com/samples/monodroid/SwitchDemo/)
- [탭 레이아웃 자습서](~/android/user-interface/layouts/tab-layout/index.md)
