---
title: 두 번째 도구 모음 추가
ms.prod: xamarin
ms.assetid: FCE0AD27-8B6B-47C6-AD19-2B1C12E1BBBF
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: 43030d4a221ceadf68c30df2e37449e7f996fa6d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029116"
---
# <a name="adding-a-second-toolbar"></a>두 번째 도구 모음 추가

## <a name="overview"></a>개요 

작업에서 여러 번 사용할 수 &ndash; 작업 표시줄을 대체 하는 것 보다 더 많은 작업을 수행할 수 `Toolbar`, 화면에서 배치 하도록 사용자 지정 하 고 화면의 일부 너비로만 구성 될 수 있습니다. 아래 예제에서는 두 번째 `Toolbar`를 만들어 화면 아래쪽에 넣는 방법을 보여 줍니다. 이 `Toolbar`는 **복사**, **잘라내기**및 **붙여넣기** 메뉴 항목을 구현 합니다. 

## <a name="define-the-second-toolbar"></a>두 번째 도구 모음 정의 

레이아웃 파일 **Main. axml** 을 편집 하 고 내용을 다음 XML로 바꿉니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <include
        android:id="@+id/toolbar"
        layout="@layout/toolbar" />
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:id="@+id/main_content"
        android:layout_below="@id/toolbar">
      <ImageView
          android:layout_width="fill_parent"
          android:layout_height="0dp"
          android:layout_weight="1" />
      <Toolbar
          android:id="@+id/edit_toolbar"
          android:minHeight="?android:attr/actionBarSize"
          android:background="?android:attr/colorAccent"
          android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
          android:layout_width="match_parent"
          android:layout_height="wrap_content" />
    </LinearLayout>
</RelativeLayout>
```

이 XML은 화면 아래쪽에 빈 `ImageView`를 입력 하 여 화면 아래쪽에 두 번째 `Toolbar`를 추가 합니다. 이 `Toolbar`의 높이는 작업 모음의 높이로 설정 됩니다. 

```xml
android:minHeight="?android:attr/actionBarSize"
```

이 `Toolbar`의 배경색은 다음에 정의 되는 악센트 색으로 설정 됩니다.

```xml
android:background="?android:attr/colorAccent
```

이 `Toolbar`는 &ndash; [작업 모음](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md) `Toolbar`에서 사용 하는 것과 다른 테마 (**ThemeOverlay. ActionBar**)를 기반으로 합니다 .이는 활동의 창 décor 또는에서 사용 되는 테마에 바인딩되지 않습니다. 첫 번째 `Toolbar`입니다.

**리소스/값/스타일 .xml** 을 편집 하 고 스타일 정의에 다음 강조 색을 추가 합니다. 

```xml
<item name="android:colorAccent">#C7A935</item>
```

그러면 아래쪽 도구 모음에 짙은 주황색 색이 제공 됩니다. 앱을 빌드하고 실행 하면 화면 아래쪽에 빈 두 번째 도구 모음이 표시 됩니다. 

[화면 맨 아래에 노란색 두 번째 도구 모음이 있는 앱의![스크린샷](adding-a-second-toolbar-images/01-second-toolbar-sml.png)](adding-a-second-toolbar-images/01-second-toolbar.png#lightbox)

## <a name="add-edit-menu-items"></a>편집 메뉴 항목 추가 

이 섹션에서는 아래쪽 `Toolbar`에 편집 메뉴 항목을 추가 하는 방법에 대해 설명 합니다. 

보조 `Toolbar`에 메뉴 항목을 추가 하려면 다음을 수행 합니다. 

1. 응용 프로그램 프로젝트의 `mipmap-` 폴더 (필요한 경우)에 메뉴 아이콘을 추가 합니다.

2. **리소스/메뉴**에 추가 메뉴 리소스 파일을 추가 하 여 메뉴 항목의 내용을 정의 합니다. 

3. 활동의 `OnCreate` 메서드에서 `FindViewById`를 호출 하 여 `Toolbar`를 찾고 `Toolbar`의 메뉴를 확장 합니다.

4. `OnCreate`에서 새 메뉴 항목에 대 한 클릭 처리기를 구현 합니다. 

다음 섹션에서는이 프로세스에 대해 자세히 설명 합니다. **잘라내기**, **복사**및 **붙여넣기** 메뉴 항목이 아래쪽 `Toolbar`에 추가 됩니다. 

### <a name="define-the-edit-menu-resource"></a>편집 메뉴 리소스 정의

**리소스/메뉴** 하위 디렉터리에서 **EDIT_MENUS** 라는 새 xml 파일을 만들고 내용을 다음 xml로 바꿉니다.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item
       android:id="@+id/menu_cut"
       android:icon="@mipmap/ic_menu_cut_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Cut" />
  <item
       android:id="@+id/menu_copy"
       android:icon="@mipmap/ic_menu_copy_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Copy" />
  <item
       android:id="@+id/menu_paste"
       android:icon="@mipmap/ic_menu_paste_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Paste" />
</menu>
```

이 XML은 **잘라내기**, **복사**및 **붙여넣기** 메뉴 항목을 만듭니다 ( [작업 모음를 바꿀](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md)때 `mipmap-` 폴더에 추가 된 아이콘 사용).

### <a name="inflate-the-menus"></a>메뉴를 확장 합니다.

**MainActivity.cs**의 `OnCreate` 메서드 끝에 다음 코드 줄을 추가 합니다. 

```csharp
var editToolbar = FindViewById<Toolbar>(Resource.Id.edit_toolbar);
editToolbar.Title = "Editing";
editToolbar.InflateMenu (Resource.Menu.edit_menus);
editToolbar.MenuItemClick += (sender, e) => {
    Toast.MakeText(this, "Bottom toolbar tapped: " + e.Item.TitleFormatted, ToastLength.Short).Show();
};
```

이 코드는 늘어납니다에 정의 된 `edit_toolbar` 뷰를 찾고 해당 제목을 **편집**으로 설정 하 고 **edit_menus**에 정의 된 메뉴 **항목을 표시**합니다. 누른 편집 아이콘을 표시 하는 알림 메시지를 표시 하는 메뉴 클릭 처리기를 정의 합니다. 

앱을 빌드하고 실행합니다. 앱이 실행 되 면 위에 추가 된 텍스트와 아이콘이 다음과 같이 표시 됩니다. 

[잘라내기, 복사 및 붙여넣기 아이콘이 있는 아래쪽 도구 모음의![다이어그램](adding-a-second-toolbar-images/02-bottom-toolbar-sml.png)](adding-a-second-toolbar-images/02-bottom-toolbar.png#lightbox)

**잘라내기** 메뉴 아이콘을 탭 하면 다음과 같은 알림이 표시 됩니다. 

[잘라내기 메뉴 아이콘이 탭 되었음을 나타내는 알림 스크린샷![](adding-a-second-toolbar-images/03-bottom-tapped-sml.png)](adding-a-second-toolbar-images/03-bottom-tapped.png#lightbox)

두 도구 모음에서 메뉴 항목을 누르면 결과 알림을 표시 됩니다. 

[탭 하 여 저장, 복사 및 붙여넣기 메뉴 항목에 대 한 알림을 스크린샷![](adding-a-second-toolbar-images/04-menu-action-sml.png)](adding-a-second-toolbar-images/04-menu-action.png#lightbox)

## <a name="the-up-button"></a>위쪽 단추 

대부분의 Android 앱은 앱 탐색에 대해 **뒤로** 단추를 사용 합니다. **뒤로** 단추를 누르면 사용자가 이전 화면으로 이동 합니다.
그러나 사용자가 앱의 주 화면으로 "위로" 이동할 수 있도록 하 **는 단추를 제공 하는 것** 도 좋습니다. 사용자가 **위로** 단추를 선택 하면 사용자가 앱 계층 &ndash; 구조에서 상위 수준으로 이동 합니다. 즉, 앱이 이전에 방문한 작업을 다시 팝 하지 않고 뒤로 스택에 있는 사용자의 여러 작업을 다시 팝 합니다. 

`Toolbar`를 작업 모음으로 사용 하는 두 번째 활동에서 **위로** 단추를 사용 하도록 설정 하려면 `SetDisplayHomeAsUpEnabled`를 호출 하 고 두 번째 활동의 `OnCreate` 메서드에서 `SetHomeButtonEnabled` 메서드를 호출 합니다.

```csharp
SetActionBar (toolbar);
...
ActionBar.SetDisplayHomeAsUpEnabled (true);
ActionBar.SetHomeButtonEnabled (true);
```

[Support V7 Toolbar](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar) 코드 샘플은 **Up** 단추를 실행 하는 방법을 보여 줍니다. 이 샘플 (다음에 설명 된 AppCompat 라이브러리 사용)은 이전 작업에 대 한 계층적 탐색을 위한 도구 모음 **위쪽** 단추를 사용 하는 두 번째 활동을 구현 합니다. 이 예제에서 `DetailActivity` 홈 단추를 사용 하면 다음과 같은 `SupportActionBar` 메서드를 호출 하 여 **Up** 단추를 사용할 수 있습니다. 

```csharp
SetSupportActionBar (toolbar);
...
SupportActionBar.SetDisplayHomeAsUpEnabled (true);
SupportActionBar.SetHomeButtonEnabled (true);
```

사용자가 `MainActivity`에서 `DetailActivity`로 이동 하는 경우 스크린샷에 표시 된 것 처럼 `DetailActivity` **위쪽** 단추 (왼쪽 화살표)가 표시 됩니다.

[![도구 모음에서 위쪽 단추 왼쪽 화살표의 스크린샷 예제](adding-a-second-toolbar-images/05-up-button-sml.png)](adding-a-second-toolbar-images/05-up-button.png#lightbox)

이 **위로** 단추를 누르면 앱이 `MainActivity`으로 돌아옵니다. 계층의 여러 수준을 포함 하는 더 복잡 한 앱에서이 단추를 누르면 사용자가 이전 화면이 아닌 앱에서 가장 높은 수준으로 돌아갑니다. 

## <a name="related-links"></a>관련 링크

- [롤리팝 도구 모음 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat 도구 모음 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
