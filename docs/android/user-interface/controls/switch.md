---
title: 전환
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 0f4bfc3646f1ccd956ee8151468b3de20f6e1e2b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="switch"></a>전환

`Switch` (아래 참조) 위젯 시작과 같은 두 가지 상태 간에 전환할 수 있습니다. `Switch` 기본값은 OFF입니다. ON 및 OFF 상태 위젯은 다음과 같습니다.

[![스위치 위젯 오프 했다가 다시 로그온 상태 스크린샷](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)


## <a name="creating-a-switch"></a>스위치 만들기

스위치를 만들려면를 간단히 선언는 `Switch` 다음과 같이 xml에서 요소:

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

그러면 아래와 같이 기본 스위치를 생성 됩니다.

[![꺼짐 상태에는 스위치를 표시 하는 데모 응용 프로그램의 스크린 샷](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)


## <a name="changing-default-values"></a>기본값을 변경

ON 및 OFF 상태에 대 한 컨트롤이 표시 하는 텍스트와 기본값은 구성 가능 합니다. 예를 들어 스위치/끄기 대신 NO/YES를 읽고 ON이 기본값으로 사용 하도록 설정할 수 있습니다는 `checked`, `textOn`, 및 `textOff` 다음 XML의 특성입니다.

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```



## <a name="providing-a-title"></a>제목을 제공합니다.

`Switch` 위젯도 설정 하 여 텍스트 레이블을 포함 하 여 지원는 `text` 특성을 다음과 같이 합니다.

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

이 태그는 런타임 시 다음 스크린 샷에서 생성합니다.

[![가로로 스위치 위젯 앞에 오는 텍스트가 있는 데모 응용 프로그램의 스크린 샷](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

경우는 `Switch`의 발생 값이 변경 되는 `CheckedChange` 이벤트입니다.
예를 들어 다음 코드에서에서는이 이벤트를 캡처하고 제공는 `Toast` 메시지와 함께 widget 기반는 `isChecked` 값 `Switch`의 일환으로 이벤트 처리기로 전달 되는 `CompoundButton.CheckedChangeEventArg` 인수입니다.

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```


## <a name="related-links"></a>관련 링크

- [SwitchDemo (샘플)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/SwitchDemo/)
- [탭 레이아웃 자습서](~/android/user-interface/layouts/tab-layout/index.md)
- [아이스크림 샌드위치 소개](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 플랫폼](http://developer.android.com/sdk/android-4.0.html)
