---
title: 사용자 지정 단추
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: ecb745f2f50b5aa0e22e331a4def0be9d8f86aa5
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510393"
---
# <a name="custom-button"></a>사용자 지정 단추

이 섹션에서는 [`Button`](xref:Android.Widget.Button) 위젯 및 여러 단추 상태에 사용할 3 개의 다른 이미지를 정의 하는 XML 파일을 사용 하 여 텍스트 대신 사용자 지정 이미지를 사용 하 여 단추를 만듭니다. 단추를 누르면 짧은 메시지가 표시 됩니다.

아래의 세 이미지를 마우스 오른쪽 단추로 클릭 하 고 다운로드 한 다음 프로젝트의 **리소스/그릴** 수 있는 디렉터리에 복사 합니다. 이러한 단추는 여러 단추 상태에 사용 됩니다.

 표준 상태 [ 주황색android![](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png#lightbox) 아이콘 포커스가 있는 상태에 대 한 녹색 android 아이콘 누름 상태의 노란색 android 아이콘 [ ![](custom-button-images/android-normal.png)](custom-button-images/android-normal.png#lightbox) [ ![](custom-button-images/android-focused.png)](custom-button-images/android-focused.png#lightbox)

**Android_button**라는 **리소스/그릴** 때 디렉터리에 새 파일을 만듭니다. 다음 XML을 삽입 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/android_pressed"
          android:state_pressed="true" />
    <item android:drawable="@drawable/android_focused"
          android:state_focused="true" />
    <item android:drawable="@drawable/android_normal" />
</selector>
```

이는 단추의 현재 상태에 따라 해당 이미지를 변경 하는 그릴 수 있는 단일 리소스를 정의 합니다. 첫 `<item>` 번째는 단추를 누를 때 (활성화 된) **android_pressed** 를 이미지로 정의 하 고, 두 번째 `<item>` 는 단추가 포커스를 가질 때 **android_focused** 를 이미지로 정의 합니다. 트랙볼 또는 방향 패드를 사용 하 여 강조 표시 됨). 그리고 세 번째 `<item>` 는 **android_normal** 를 표준 상태에 대 한 이미지로 정의 합니다 (누르거나 포커스가 지정 되지 않은 경우). 이 XML 파일은 이제 그릴 수 있는 단일 리소스를 나타내며, 해당 [`Button`](xref:Android.Widget.Button) 배경에 대해에서 참조 될 때 표시 되는 이미지는 이러한 세 가지 상태에 따라 변경 됩니다.


> [!NOTE]
> `<item>` 요소의 순서는 중요 합니다. 이 그릴 수 있는이를 참조 `<item>`하는 경우 현재 단추 상태에 적합 한 항목을 확인 하기 위해가 차례로 트래버스 됩니다.
> "Normal" 이미지는 마지막 이미지 이므로 조건이 `android:state_pressed` `android:state_focused` 모두 false 인 경우에만 적용 됩니다.

**Resources/layout/Main. axml** 파일을 열고 요소를 [`Button`](xref:Android.Widget.Button) 추가 합니다.

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

특성 `android:background` 은 단추 배경에 사용할 그릴 수 있는 리소스를 지정 합니다 .이 리소스는 **리소스/그릴 수 있는/android .xml**에 저장 하는 경우 `@drawable/android`로 참조 됩니다. 이는 시스템 전체에서 단추에 사용 되는 일반 배경 이미지를 대체 합니다. 그릴 수 있는 단추가 단추 상태를 기준으로 이미지를 변경 하려면 이미지를 배경에 적용 해야 합니다.

단추를 누를 때 다른 작업을 수행 하려면의 끝에 다음 코드를 추가 합니다.[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
방법이

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

그러면 레이아웃 [`Button`](xref:Android.Widget.Button) 에서가 캡처되고 [`Button`](xref:Android.Widget.Button) 가 클릭 될 때 표시 [`Toast`](xref:Android.Widget.Toast) 될 메시지를 추가 합니다.

이제 응용 프로그램을 실행 합니다.


*이 페이지의 일부는 Android 오픈 소스 프로젝트에서 만들고 공유 하 고*
[*Creative Commons 2.5 특성 라이선스*](http://creativecommons.org/licenses/by/2.5/)에 설명 된 용어에 따라 사용 되는 작업을 기반으로 수정 됩니다.
