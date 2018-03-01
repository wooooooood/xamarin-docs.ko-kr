---
title: "사용자 지정 단추"
ms.topic: article
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: f77a9b8d3bb69bb47d973a56aed5ad1d49f9a02d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="custom-button"></a>사용자 지정 단추

이 섹션에서는 만들어집니다 단추 텍스트를 대신 사용자 지정 이미지와 함께 사용 하 여는 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) 위젯 및 여러 단추 상태에 사용할 세 개의 서로 다른 이미지를 정의 하는 XML 파일입니다. 단추를 누를 때 간단한 메시지가 표시 됩니다.

마우스 오른쪽 단추로 클릭 하 아래 세 개의 이미지를 다운로드 한 다음에 복사는 **리소스/그릴** 프로젝트의 디렉터리입니다. 여러 단추 상태에 대 한 사용 됩니다.

 [![정상 상태에 대 한 Android 녹색 아이콘](custom-button-images/android-normal.png)](custom-button-images/android-normal.png) [ ![포커스가 있는 상태에 대 한 Android 주황색 아이콘이](custom-button-images/android-focused.png)](custom-button-images/android-focused.png) [ ![누른된 상태에 대 한 Android 노란색 아이콘](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png)

새 파일을 만듭니다는 **리소스/그릴** 라는 디렉터리 **android_button.xml**합니다. 다음 XML을 삽입 합니다.

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

이 단추의 현재 상태에 따라 해당 이미지를 변경 하는 단일 그릴 수 있는 리소스를 정의 합니다. 첫 번째 `<item>` 정의 **android_pressed.png** 단추가 눌려질 때 이미지로 (활성화할); 두 번째 `<item>` 정의 **android_focused.png** 이미지로 때는 (경우에 단추가 트랙볼 또는 방향 패드를 사용 하 여 강조 표시) 단추는 데 사용 됩니다. 세 번째 `<item>` 정의 **android_normal.png** (누름 아니고 초점을 맞춘) 경우의 정상 상태에 대 한 이미지 형식으로. 이 XML 파일은 이제 단일 그릴 수 있는 리소스를 나타내는에서 참조 하는 경우 및는 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) 해당 배경에 표시 되는 이미지에 따라 변경 됩니다 이러한 세 가지 상태입니다.


> [!NOTE]
> **참고:** 의 순서는 `<item>` 요소는 중요 합니다. 이 그릴 수 있는 참조 될 때는 `<item>`는 어떤 것은 현재 단추 상태에 적합 한지 결정 하는 순서 대로 이동 합니다.
> 이기 때문에 "일반" 이미지 마지막, 적용 된 경우에만 조건 `android:state_pressed` 및 `android:state_focused` false 평가 둘 다 있어야 합니다.

열기는 **Resources/layout/Main.axml** 파일을 추가 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) 요소:

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

`android:background` 단추 배경에 사용할 그릴 수 있는 리소스를 지정 하는 특성 (을에 저장 될 때 **Resources/drawable/android.xml**로 참조 `@drawable/android`). 이 단추는 시스템 전체에 사용 되는 기본 배경 이미지를 대체 합니다. 단추 상태에 따라 해당 이미지를 변경 하려면 그릴를 백그라운드에 이미지를 적용 합니다.

단추를 누를 때 가능한 작업을 하려면 다음 코드의 끝에 추가 된 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle/) 메서드:

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

캡처합니다는 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) 레이아웃에서 추가 된 [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) 때 표시할 메시지를는 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) 를 클릭 합니다.

이제 응용 프로그램을 실행 합니다.


*이 페이지의 일부는 Android 열려 있는 소스 프로젝트에서 공유 하 고 만들고에 설명 된 조건에 따라 사용 작업에 따라 수정 된*
[*Creative Commons 2.5 Attribution 라이선스* ](http://creativecommons.org/licenses/by/2.5/).
