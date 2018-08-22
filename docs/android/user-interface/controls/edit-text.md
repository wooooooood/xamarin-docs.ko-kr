---
title: 텍스트 편집
description: EditText 위젯을 사용 하 여 사용자 입력을 사용 하는 방법.
ms.prod: xamarin
ms.assetid: E513BCBC-438E-15E8-B83A-4B768A8E8B32
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/09/2018
ms.openlocfilehash: bb2cb13472e7e17eb1b0438ed67033f2b04defe2
ms.sourcegitcommit: b6f3e55d4f3dcdc505abc8dc9241cff0bb5bd154
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/10/2018
ms.locfileid: "40251071"
---
# <a name="edit-text"></a>텍스트 편집

이 섹션에서는 사용할지 합니다 [EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) 사용자 입력에 대 한 텍스트 필드를 만들려면 위젯입니다. 필드에 입력 된 후의 **Enter** 키 알림 메시지의 텍스트가 표시 됩니다.

열기 **Resources/layout/activity_main.axml** 추가한 합니다 [EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) 요소를 포함 하는 레이아웃을 합니다. 다음 예제에서는 **activity_main.axml** 에 `EditText` 에 추가 된를 `LinearLayout`:

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

이 코드 예제는 `EditText` 특성 `android:imeOptions` 로 설정 된 `actionGo`합니다. 이 설정이 기본값을 변경 [수행](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_DONE) 작업을 합니다 [이동](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_GO) 작업 탭 있도록를 **Enter** 트리거 키를 `KeyPress` 입력된 처리기입니다.
(일반적으로 `actionGo` 는 되도록 합니다 **Enter** 키를 입력 하는 URL의 대상 사용자를 안내 합니다.)

사용자 텍스트 입력을 처리 하려면 다음 코드의 끝에 추가 합니다 [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/) 메서드에서 **MainActivity.cs**:

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

또한 다음을 추가 `using` 의 맨 위에 문을 **MainActivity.cs** 아직 제공 되지 경우:

```csharp
using Android.Views;
```

이 코드 예제를 확장 합니다 [EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) 요소를 레이아웃에서 추가 [KeyPress](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) 위젯을 포커스가 있는 동안 키를 누를 때 수행 되어야 하는 작업을 정의 하는 처리기. 수신 대기할 메서드가 정의 된 경우에 **Enter** (탭) 하는 경우 키를 다음 팝업을 [알림](https://developer.xamarin.com/api/type/Android.Widget.Toast/) 입력 된 텍스트를 사용 하 여 메시지. 합니다 [처리](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) 속성은 항상 `true` 이벤트가 처리 되었습니다. 이 버블링에서 이벤트를 방지 하는 데 필요한 최대 (결과적으로 텍스트 필드에는 캐리지 리턴에).

응용 프로그램을 실행 하 고 텍스트 필드에 일부 텍스트를 입력 합니다. 누를 때 합니다 **Enter** 키, 알림을 표시할 오른쪽에 표시 된 것과 같이:

[![EditText에 텍스트를 입력 하는 예제](edit-text-images/edit-text-sml.png)](edit-text-images/edit-text.png#lightbox)

*이 페이지의 일부는 만든 작업에 따라 수정 하 고* [ *Android Open Source Project에서 공유* ](http://code.google.com/policies.html) *는에설명된조건에따라사용및* [ *2.5 creative Commons Attribution 라이선스* ](http://creativecommons.org/licenses/by/2.5/) *합니다. 이 자습서는 기반으로 합니다* [ *Android 양식 항목 자습서* ](http://developer.android.com/resources/tutorials/views/hello-formstuff.html) *합니다.*


## <a name="related-links"></a>관련 링크

- [EditTextSample](https://developer.xamarin.com/samples/monodroid/UserInterface/EditTextSample/)
