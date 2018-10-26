---
title: 두 번째 도구 모음 추가
ms.prod: xamarin
ms.assetid: FCE0AD27-8B6B-47C6-AD19-2B1C12E1BBBF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: b8da13afb7fd8d7198e8bfe7476b40a5cd09769a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112554"
---
# <a name="adding-a-second-toolbar"></a>두 번째 도구 모음 추가


## <a name="overview"></a>개요 

`Toolbar` 대체 보다 더 많은 작업 모음 작업 수 &ndash; 사용할 수 있습니다 여러 번 활동 내에서 사용할 수 있습니다, 화면에서 아무 곳 이나 배치에 대 한 사용자 지정 하 고 화면의 너비를 부분에 걸쳐 구성할 수 있습니다. 아래 예제에서는 두 번째를 만드는 방법을 보여 줍니다 `Toolbar` 화면 맨 아래에 놓습니다. 이 `Toolbar` 구현 **복사**합니다 **잘라내기**, 및 **붙여넣기** 메뉴 항목입니다. 


## <a name="define-the-second-toolbar"></a>두 번째 도구 모음을 정의 합니다. 

레이아웃 파일을 편집 **Main.axml** 및 해당 콘텐츠를 다음 XML로 바꿉니다.

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

두 번째를 추가 하는이 XML `Toolbar` 빈 화면 맨 아래에 `ImageView` 화면의 가운데를 입력 합니다. 이 항목의 높이 `Toolbar` 작업 모음의 높이로 설정 됩니다. 

```xml
android:minHeight="?android:attr/actionBarSize"
```

이 항목의 배경색 `Toolbar` 다음에 정의할 수 있는 강조색으로 설정 됩니다.

```xml
android:background="?android:attr/colorAccent
```

알림이 `Toolbar` 다른 테마를 기반으로 (**ThemeOverlay.Material.Dark.ActionBar**)에서 사용 하는 것은 `Toolbar` 에서 만든 [작업 모음 바꾸기](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md) &ndash;활동 창 장식 또는 첫 번째 범위에서 사용 되는 테마에 바인딩되어 있지 않은 `Toolbar`합니다.

편집할 **Resources/values/styles.xml** 스타일 정의에 다음의 강조 색을 추가 합니다. 

```xml
<item name="android:colorAccent">#C7A935</item>
```

아래쪽 도구 모음의 짙은 주황색 색을 제공합니다. 빌드하고 앱을 실행 화면 맨 아래에 빈 두 번째 도구 모음을 표시 합니다. 

[![화면 맨 아래에 노란색 두 번째 도구 모음을 사용 하 여 앱의 스크린 샷](adding-a-second-toolbar-images/01-second-toolbar-sml.png)](adding-a-second-toolbar-images/01-second-toolbar.png#lightbox)


 
## <a name="add-edit-menu-items"></a>편집 메뉴 항목 추가 

이 섹션 아래쪽에 편집 메뉴 항목을 추가 하는 방법에 설명 `Toolbar`합니다. 

메뉴 항목을 추가 하 여 보조 데이터베이스 `Toolbar`: 

1.  메뉴 아이콘을 추가 합니다 `mipmap-` (필요한 경우) 앱 프로젝트의 폴더입니다.

2.  추가 메뉴 리소스 파일을 추가 하 여 메뉴 항목의 내용을 정의 **리소스/메뉴**합니다. 

3.  활동에서 `OnCreate` 메서드를 찾을 합니다 `Toolbar` (호출 하 여 `FindViewById`) 확장 하 고는 `Toolbar`의 메뉴.

4.  클릭 처리기를 구현 `OnCreate` 새 메뉴 항목에 대 한 합니다. 

다음 섹션에서는이 프로세스를 자세히 보여 줍니다. **잘라내기**, **복사**, 및 **붙여넣기** 메뉴 항목이 맨 아래에 추가 됩니다 `Toolbar`합니다. 



### <a name="define-the-edit-menu-resource"></a>편집 메뉴 리소스를 정의 합니다.

에 **리소스/메뉴** 하위 디렉터리 라는 새 XML 파일을 만듭니다 **edit_menus.xml** 그 내용을 다음 XML로 바꿉니다.

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

이 XML은 만듭니다는 **잘라내기**, **복사**, 및 **붙여넣기** 메뉴 항목 (에 추가 된 아이콘을 사용 하는 `mipmap-` 폴더에 [작업 모음 바꾸기 ](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md)).



### <a name="inflate-the-menus"></a>메뉴를 확장 합니다.

끝에 `OnCreate` 에서 메서드 **MainActivity.cs**, 코드의 다음 줄을 추가 합니다. 

```csharp
var editToolbar = FindViewById<Toolbar>(Resource.Id.edit_toolbar);
editToolbar.Title = "Editing";
editToolbar.InflateMenu (Resource.Menu.edit_menus);
editToolbar.MenuItemClick += (sender, e) => {
    Toast.MakeText(this, "Bottom toolbar tapped: " + e.Item.TitleFormatted, ToastLength.Short).Show();
};
```

이 코드를 찾습니다는 `edit_toolbar` 에 정의 된 뷰 **Main.axml**, 해당 제목 설정 **편집용**, 해당 메뉴 항목을 확장 하 고 (에 정의 된 **edit_menus.xml**). 메뉴 정의를 편집 아이콘을 누른 나타내는 알림을 표시 하는 처리기를 클릭 합니다. 

앱을 빌드하고 실행합니다. 앱을 실행할 때 위에서 추가한 아이콘과 텍스트 다음과 같이 표시 됩니다. 

[![아래쪽 도구 모음 아이콘을 잘라내기, 복사 및 붙여넣기를 사용 하 여 다이어그램](adding-a-second-toolbar-images/02-bottom-toolbar-sml.png)](adding-a-second-toolbar-images/02-bottom-toolbar.png#lightbox)

탭의 **잘라내기** 메뉴 아이콘 표시 될 다음 알림이 발생 합니다. 

[![스크린 샷의 알림은 나타내는 잘라내기 메뉴 아이콘을 누른](adding-a-second-toolbar-images/03-bottom-tapped-sml.png)](adding-a-second-toolbar-images/03-bottom-tapped.png#lightbox)

결과 팝업을 표시 하거나 도구 모음에서 메뉴 항목 탭: 

[![에 대 한 팝업의 스크린 샷을 저장, 복사 및 탭 되는 메뉴 항목 붙여넣기](adding-a-second-toolbar-images/04-menu-action-sml.png)](adding-a-second-toolbar-images/04-menu-action.png#lightbox)



## <a name="the-up-button"></a>위쪽 단추 

대부분의 Android 앱을 사용 합니다 **다시** 앱 탐색 단추; 키를 눌러 합니다 **다시** 단추는 이전 화면으로 사용자를 사용 합니다.
그러나 있도록 수도 있습니다는 **등록** 단추를 사용 하면 사용자가 앱의 주 화면에 "최대" 탐색 하기 쉽게 합니다. 사용자가 선택 하는 경우는 **등록** 단추를 사용자가 앱 계층에서 더 높은 수준으로 위로 &ndash; 즉, 앱을 팝 사용자를 다시 이전에 방문 하는 팝업 돌아갑니다 보다는 백 스택의 여러 활동 작업입니다. 

사용 하도록 설정 하려면를 **등록** 단추를 사용 하는 두 번째 작업을 `Toolbar` 의 작업 표시줄을 호출 하는 `SetDisplayHomeAsUpEnabled` 및 `SetHomeButtonEnabled` 두 번째 작업의 메서드 `OnCreate` 메서드:

```csharp
SetActionBar (toolbar);
...
ActionBar.SetDisplayHomeAsUpEnabled (true);
ActionBar.SetHomeButtonEnabled (true);
```

[v7 도구 모음을 지원](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/) 보여 주는 코드 예제는 **등록** 작업 단추입니다. 이 샘플 (다음에 설명 된 AppCompat 라이브러리 사용) 도구 모음을 사용 하는 두 번째 작업을 구현 **등록** 이전 작업을 다시 계층적 탐색 단추입니다. 이 예제에서는 합니다 `DetailActivity` 홈 단추를 사용 하면 합니다 **위로** 다음 여는 단추 `SupportActionBar` 메서드 호출: 

```csharp
SetSupportActionBar (toolbar);
...
SupportActionBar.SetDisplayHomeAsUpEnabled (true);
SupportActionBar.SetHomeButtonEnabled (true);
```

사용자가 탐색 하는 경우 `MainActivity` 하 `DetailActivity`의 `DetailActivity` 표시를 **위로** 스크린샷에 표시 된 것 처럼 (왼쪽된 화살표) 단추:

[![도구 모음에서 위쪽 단추 왼쪽된 화살표의 스크린샷 예제](adding-a-second-toolbar-images/05-up-button-sml.png)](adding-a-second-toolbar-images/05-up-button.png#lightbox)

이 탭 **위로** 단추를 클릭 하면 앱에 반환할 `MainActivity`합니다. 여러 수준의 계층 구조를 사용 하 여 더 복잡 한 앱에서이 단추를 누르면 반환 사용자 앱은 다음 최상위 아닌 이전 화면으로 합니다. 



## <a name="related-links"></a>관련 링크

- [롤리팝 도구 모음 (샘플)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat 도구 모음 (샘플)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
