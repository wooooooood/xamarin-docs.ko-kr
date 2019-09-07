---
title: 텍스트 편집
description: 편집 텍스트 위젯을 사용 하 여 사용자 입력을 수락 하는 방법입니다.
ms.prod: xamarin
ms.assetid: E513BCBC-438E-15E8-B83A-4B768A8E8B32
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/09/2018
ms.openlocfilehash: e8ffe337e1f5c74bc348b9600a466f1232f40b0b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758837"
---
# <a name="xamarinandroid-edit-text"></a>Xamarin Android 편집 텍스트

이 섹션에서는 [편집 텍스트](xref:Android.Widget.EditText) 위젯을 사용 하 여 사용자 입력에 대 한 텍스트 필드를 만듭니다. 필드에 텍스트를 입력 한 후에는 **Enter** 키를 통해 알림 메시지에 텍스트가 표시 됩니다.

**Resources/layout/activity_main** 를 열고 [편집 텍스트](xref:Android.Widget.EditText) 요소를 포함 하는 레이아웃에 추가 합니다. 다음 예 activity_main에는에 추가 `LinearLayout`된가 `EditText` 있습니다 **.**

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <EditText
        android:id="@+id/edittext"
        android:layout_width="match_parent"
        android:imeOptions="actionGo"
        android:inputType="text"
        android:layout_height="wrap_content" />
</LinearLayout>
```

이 코드 예제 `EditText` 에서 특성 `android:imeOptions` 은로 `actionGo`설정 됩니다. 이 설정은 **Enter 키** 를 눌러 `KeyPress`입력처리기를 트리거하기 위해 기본[완료](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_DONE)동작을 [Go](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_GO)동작으로 변경 합니다.
일반적으로 `actionGo` **Enter** 키를 사용 하 여에 입력 된 URL의 대상으로 사용자를 가져옵니다.

사용자 텍스트 입력을 처리 하려면 **MainActivity.cs**에서 [OnCreate](xref:Android.App.Activity.OnCreate*) 메서드의 끝에 다음 코드를 추가 합니다.

```csharp
EditText edittext = FindViewById<EditText>(Resource.Id.edittext);
edittext.KeyPress += (object sender, View.KeyEventArgs e) => {
    e.Handled = false;
    if (e.Event.Action == KeyEventActions.Down && e.KeyCode == Keycode.Enter) 
    {
        Toast.MakeText(this, edittext.Text, ToastLength.Short).Show();
        e.Handled = true;
    }
};
```

또한 MainActivity.cs의 맨 위에 다음 `using` 문을 추가 합니다 (아직 없는 경우).

```csharp
using Android.Views;
```

이 코드 예제는 레이아웃에서 [Edittext](xref:Android.Widget.EditText) 요소를 늘어납니다 하 고 위젯에 포커스가 있을 때 키를 누를 때 수행할 동작을 정의 하는 [KeyPress](xref:Android.Views.View.KeyPress) 처리기를 추가 합니다. 이 경우 메서드는 **Enter** 키를 수신 대기 하도록 정의 된 다음 입력 된 텍스트와 함께 [알림](xref:Android.Widget.Toast) 메시지를 표시 합니다. 이벤트가 처리 된 경우 [처리](xref:Android.Views.View.KeyEventArgs.Handled) 된 속성은 `true` 항상 이어야 합니다. 이는 이벤트를 버블링 하지 않도록 하기 위해 필요 합니다 (텍스트 필드에서 캐리지 리턴을 반환 함).

응용 프로그램을 실행 하 고 텍스트 필드에 일부 텍스트를 입력 합니다. **Enter** 키를 누르면 알림이 오른쪽에 표시 된 대로 표시 됩니다.

[![텍스트를 편집 텍스트로 입력 하는 예](edit-text-images/edit-text-sml.png)](edit-text-images/edit-text.png#lightbox)

*이 페이지의 일부는 생성 된 작업을 기반으로 하 여 수정 됩니다* . [*Android 오픈 소스 프로젝트에서 공유*](http://code.google.com/policies.html) 에 *설명 된 용어에 따라 사용 됩니다* . [*Creative Commons 2.5 특성 라이선스*](http://creativecommons.org/licenses/by/2.5/) *. 이 자습서는* [*Android 양식 제품 자습서*](https://developer.android.com/resources/tutorials/views/hello-formstuff.html) 를 기반으로 *합니다.*

## <a name="related-links"></a>관련 링크

- [EditTextSample](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-edittextsample)
