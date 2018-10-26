---
title: 사용자 지정 단추
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: b5ccefa1eb7e659584c1c82481bbd4473a3a8abc
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122857"
---
# <a name="custom-button"></a>사용자 지정 단추

이 섹션에서는 텍스트 대신 사용자 지정 이미지를 사용 하 여 단추 만들기를 사용 하는 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) 위젯 및 여러 단추 상태에 사용할 3 개의 서로 다른 이미지를 정의 하는 XML 파일. 단추를 누르면 짧은 메시지가 표시 됩니다.

마우스 오른쪽 단추로 클릭 하 고 아래 세 개의 이미지가 다운로드를 복사 합니다 **리소스/drawable** 프로젝트의 디렉터리입니다. 이러한 여러 단추 상태에 대 한 사용 됩니다.

 [![정상 상태에 대 한 녹색 Android 아이콘](custom-button-images/android-normal.png)](custom-button-images/android-normal.png#lightbox) [ ![포커스가 있는 상태에 대 한 Android 주황색 아이콘](custom-button-images/android-focused.png)](custom-button-images/android-focused.png#lightbox) [ ![누름된 상태에 대 한 노란색 Android 아이콘](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png#lightbox)

새 파일을 만듭니다는 **리소스/drawable** 디렉터리로 **android_button.xml**합니다. 다음 XML을 삽입 합니다.

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

단추의 현재 상태를 기반으로 해당 이미지를 변경 하는 단일 드로어 블 리소스를 정의 합니다. 첫 번째 `<item>` 정의 **android_pressed.png** 단추를 누를 때 이미지로 (활성화 된); 두 번째 `<item>` 정의 **android_focused.png** 이미지로 경우는 단추 (트랙볼 또는 방향 패드를 사용 하는 단추가 강조 표시) 하는 경우 포커스가; 세 번째 `<item>` 정의 **android_normal.png** 정상 상태 (누름 아니고 초점을 맞춘)에 대 한 이미지와 합니다. 이 XML 파일은 이제 단일 드로어 블 리소스를 나타내는 시점과에서 참조 하는 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) 해당 배경에 표시 되는 이미지 따라 변경 됩니다 이러한 세 가지 상태입니다.


> [!NOTE]
> 순서는 `<item>` 요소는 것이 중요 합니다. 이 drawable를 참조할 때를 `<item>`가 현재 단추 상태에 적합 한지 결정 하는 순서 대로 이동 합니다.
> 이기 때문에 "normal" 이미지 마지막에 적용 된 경우에만 조건을 `android:state_pressed` 및 `android:state_focused` false 평가 둘 다 있어야 합니다.

엽니다는 **Resources/layout/Main.axml** 파일을 추가 합니다 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) 요소:

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

`android:background` 단추 배경에 사용할 드로어 블 리소스를 지정 하는 특성 (,에 저장 하는 경우 **Resources/drawable/android.xml**를으로 참조 됩니다 `@drawable/android`). 이 시스템 전체에서 단추에 사용 되는 기본 배경 이미지를 대체 합니다. Drawable 단추 상태에 따라 해당 이미지를 변경 하려면에 대 한 순서로 배경에 이미지를 적용 합니다.

단추를 누르면 작업을 수행 하도록 끝에 다음 코드를 추가 합니다 [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle/)
방법:

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

캡처합니다를 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) 레이아웃에서 추가 [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) 때 표시할 메시지를를 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) 를 클릭 합니다.

이제 응용 프로그램을 실행 합니다.


*이 페이지의 일부는 생성 하 고 Android Open Source Project에서 공유 된 조건에 따라 사용 되는 작업에 따라 수정 합니다*
[*Creative Commons 2.5 Attribution License* ](http://creativecommons.org/licenses/by/2.5/).
