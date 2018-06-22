---
title: 텍스트 편집
ms.prod: xamarin
ms.assetid: E513BCBC-438E-15E8-B83A-4B768A8E8B32
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: d6be8ae1587742a8c2a37b22b2da3701187dde2a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30774605"
---
# <a name="edit-text"></a>텍스트 편집

이 섹션에서는 사용자 입력을 사용 하 여에 대 한 텍스트 필드를 만듭니다는 [EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) 위젯입니다. 필드에 입력 된 텍스트가 있는지 "Enter" 키 알림 메시지에 텍스트를 표시 됩니다.

열기는 <code>Resources\layout\main.xml</code> 파일을 추가 [EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) 요소 (내는 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

```xml
<pre><code class=" syntax brush-C#">&lt;EditText
    android:id="@+id/edittext"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"/&gt;</code></pre>
```

사용자 입력의 끝에 다음 코드를 추가 하는 텍스트를 통해 가능한 작업을 수행 하는 [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/) 메서드:

```csharp
EditText edittext = FindViewById<EditText>(Resource.Id.edittext);
edittext.KeyPress += (object sender, View.KeyEventArgs e) => {
 e.Handled = false;
 if (e.Event.Action == KeyEventActions.Down && e.KeyCode == Keycode.Enter) {
  Toast.MakeText (this, edittext.Text, ToastLength.Short).Show ();
  e.Handled = true;
         }
};
```

캡처합니다는 [ `EditText` ](https://developer.xamarin.com/api/type/Android.Widget.EditText/) 레이아웃 및 집합에서 요소는 [ `KeyPress` ](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) 위젯의 포커스가 있을 때 키를 누를 때 수행 되어야 하는 동작을 정의 하는 처리기. (누르면 아래로) 하 고 Enter 키를 수신 대기 하 고 팝업 메서드가 정의 된 경우에 [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) 입력 된 텍스트가 있는 메시지입니다. [ `Handled` ](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) 속성은 항상 `true` 경우 이벤트가 처리 된 이벤트 하지 않습니다 버블 업 (결과적 텍스트 필드에는 캐리지 리턴) 되도록 합니다.

응용 프로그램을 실행합니다.

*이 페이지의 일부는 생성 작업에 따라 수정 하 고* [ *Android 열려 있는 소스 프로젝트에서 공유* ](http://code.google.com/policies.html) *는에설명된조건에따라사용하고* [ *2.5 creative Commons Attribution 라이선스* ](http://creativecommons.org/licenses/by/2.5/) *합니다. 이 자습서에 따라는* [ *Android 양식 Stuff 자습서* ](http://developer.android.com/resources/tutorials/views/hello-formstuff.html) *합니다.*



## <a name="related-links"></a>관련 링크

- [EditTextSample](https://developer.xamarin.com/samples/monodroid/UserInterface/EditTextSample/)
