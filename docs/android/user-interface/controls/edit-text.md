---
title: 텍스트 편집
description: 편집 텍스트 위젯을 사용 하 여 사용자 입력을 수락 하는 방법입니다.
ms.prod: xamarin
ms.assetid: E513BCBC-438E-15E8-B83A-4B768A8E8B32
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/09/2018
ms.openlocfilehash: 6180896002d19c51bce47bf53aaecdc11b0cae6e
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725151"
---
# <a name="xamarinandroid-edit-text"></a>Xamarin Android 편집 텍스트

이 섹션에서는 [편집 텍스트](xref:Android.Widget.EditText) 위젯을 사용 하 여 사용자 입력에 대 한 텍스트 필드를 만듭니다. 필드에 텍스트를 입력 한 후에는 **Enter** 키를 통해 알림 메시지에 텍스트가 표시 됩니다.

**Resources/layout/activity_main** 를 열고 [편집 텍스트](xref:Android.Widget.EditText) 요소를 포함 하는 레이아웃에 추가 합니다. 다음 예에서는 **activity_main. axml** 에는 `LinearLayout`에 추가 된 `EditText` 있습니다.

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

이 코드 예제에서는 `EditText` 특성 `android:imeOptions`를 `actionGo`로 설정 합니다. 이 설정은 **Enter** 키를 누르면 `KeyPress` 입력 처리기를 트리거하기 위해 기본 [완료](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_DONE) 동작을 [Go](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_GO) 동작으로 변경 합니다.
일반적으로 **Enter** 키를 사용 하 여에 입력 된 URL의 대상으로 사용자를 가져오는 `actionGo`를 사용 합니다.

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

또한 **MainActivity.cs** 의 맨 위에 다음 `using` 문을 추가 합니다 (아직 없는 경우).

```csharp
using Android.Views;
```

이 코드 예제는 레이아웃에서 [Edittext](xref:Android.Widget.EditText) 요소를 늘어납니다 하 고 위젯에 포커스가 있을 때 키를 누를 때 수행할 동작을 정의 하는 [KeyPress](xref:Android.Views.View.KeyPress) 처리기를 추가 합니다. 이 경우 메서드는 **Enter** 키를 수신 대기 하도록 정의 된 다음 입력 된 텍스트와 함께 [알림](xref:Android.Widget.Toast) 메시지를 표시 합니다. 请注意, 如果已[处理](xref:Android.Views.View.KeyEventArgs.Handled)事件, 则已处理的属性应始终为`true`。 이는 이벤트를 버블링 하지 않도록 하기 위해 필요 합니다 (텍스트 필드에서 캐리지 리턴을 반환 함).

응용 프로그램을 실행 하 고 텍스트 필드에 일부 텍스트를 입력 합니다. **Enter** 키를 누르면 알림이 오른쪽에 표시 된 대로 표시 됩니다.

[텍스트를 편집 텍스트로 입력 하는 ![예제](edit-text-images/edit-text-sml.png)](edit-text-images/edit-text.png#lightbox)

*이 페이지의 일부는 Android 오픈 소스 프로젝트에서 만들고 공유 하 고 Creative Commons 2.5 특성 라이선스에 설명 된 용어에 따라 사용 되는 작업을 * [기반으로 수정 됩니다](https://creativecommons.org/licenses/by/2.5/) *. 이 자습서는* [*Android 양식 제품 자습서*](https://developer.android.com/resources/tutorials/views/hello-formstuff.html) 를 기반으로 *합니다.*

## <a name="related-links"></a>관련 링크

- [EditTextSample](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-edittextsample)
