---
title: 두 번째 도구 추가
ms.prod: xamarin
ms.assetid: FCE0AD27-8B6B-47C6-AD19-2B1C12E1BBBF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: f2255ab5ac7e153266093877a1c52bfc08927a09
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="adding-a-second-toolbar"></a>두 번째 도구 추가


## <a name="overview"></a>개요 

`Toolbar` 바꾸기 보다 많은 작업 모음 수행할 수 있는 &ndash; 활동 내에서 여러 번 사용할 수 있습니다, 될 수 있습니다는 화면에 있는 모든 배치에 대 한 사용자 지정 하 고 화면의 너비를 부분에 걸쳐 하도록 구성할 수 있습니다. 아래 예에서는 두 번째를 만드는 방법을 설명 `Toolbar` 화면 맨 아래에 배치 합니다. 이 `Toolbar` 구현 **복사**, **잘라내기**, 및 **붙여넣기** 메뉴 항목입니다. 


## <a name="define-the-second-toolbar"></a>두 번째 도구 모음을 정의 합니다. 

레이아웃 파일을 편집 **Main.axml** 그 내용을 다음 XML로 바꿉니다.

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

이 XML에 두 번째 추가 `Toolbar` 비어 있는 화면 맨 아래에 `ImageView` 화면 가운데를 작성 합니다. 이 항목의 높이 `Toolbar` 동작 막대의 높이를 설정 합니다. 

```xml
android:minHeight="?android:attr/actionBarSize"
```

이 항목의 배경색 `Toolbar` 다음에 정의할 수 있는 강조 색으로 설정 됩니다.

```xml
android:background="?android:attr/colorAccent
```

공지가 `Toolbar` 다양 한 테마를 기반으로 하는 (**ThemeOverlay.Material.Dark.ActionBar**)에서 사용 하는 것 보다는 `Toolbar` 에서 만든 [작업 모음 교체](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md) &ndash;첫 번째 범위에서 사용 되는 테마 또는 활동의 창 장식에 바인딩되어 있지 않은 `Toolbar`합니다.

편집 **Resources/values/styles.xml** 스타일 정의에 다음과 같은 강조 색을 추가 합니다. 

```xml
<item name="android:colorAccent">#C7A935</item>
```

이렇게 하면 아래쪽 도구 모음 주황색 어두운 색입니다. 빌드 및 응용 프로그램을 실행 화면 맨 아래에 빈 두 번째 도구 모음을 표시 합니다. 

[![화면 맨 아래에 노란색 두 번째 도구 모음을 사용 하는 앱의 스크린 샷](adding-a-second-toolbar-images/01-second-toolbar-sml.png)](adding-a-second-toolbar-images/01-second-toolbar.png#lightbox)


 
## <a name="add-edit-menu-items"></a>편집 메뉴 항목 추가 

이 섹션에서는 메뉴 항목 편집 아래쪽에 추가 하는 방법을 설명 `Toolbar`합니다. 

보조 복제본으로 메뉴 항목을 추가 하려면 `Toolbar`: 

1.  메뉴 아이콘을 추가 `mipmap-` (필요한 경우) 응용 프로그램 프로젝트의 폴더입니다.

2.  추가 메뉴 리소스 파일을 추가 하 여 메뉴 항목의 내용을 정의 **리소스/메뉴**합니다. 

3.  활동의에서 `OnCreate` 메서드를 찾을 `Toolbar` (호출 하 여 `FindViewById`) 팽창 및는 `Toolbar`의 메뉴.

4.  Click 처리기에서 구현 `OnCreate` 새 메뉴 항목에 대 한 합니다. 

다음 섹션에서는이 프로세스를 자세히 보여: **잘라내기**, **복사**, 및 **붙여넣기** 메뉴 항목이 아래쪽에 추가 된 `Toolbar`합니다. 



### <a name="define-the-edit-menu-resource"></a>편집 메뉴 리소스를 정의 합니다.

**리소스/메뉴** 하위 디렉터리를 새 XML 파일을 만들 **edit_menus.xml** 하 고 내용을 다음 XML로 바꿉니다.

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

이 XML을 만듭니다는 **잘라내기**, **복사**, 및 **붙여넣기** 메뉴 항목 (에 추가 된 아이콘을 사용 하는 `mipmap-` 폴더에 [작업 모음 교체 ](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md)).



### <a name="inflate-the-menus"></a>메뉴 팽창

끝에는 `OnCreate` 에서 메서드 **MainActivity.cs**, 코드의 다음 줄을 추가 합니다. 

```csharp
var editToolbar = FindViewById<Toolbar>(Resource.Id.edit_toolbar);
editToolbar.Title = "Editing";
editToolbar.InflateMenu (Resource.Menu.edit_menus);
editToolbar.MenuItemClick += (sender, e) => {
    Toast.MakeText(this, "Bottom toolbar tapped: " + e.Item.TitleFormatted, ToastLength.Short).Show();
};
```

이 코드를 찾습니다는 `edit_toolbar` 에 정의 된 보기 **Main.axml**, 제목을 설정 **편집**, 해당 메뉴 항목을 확장 하 고 (에 정의 된 **edit_menus.xml**). 메뉴 정의 편집 아이콘을 누른 나타내기 위해 알림 메시지를 표시 하는 처리기를 클릭 합니다. 

앱을 빌드하고 실행합니다. 앱을 실행할 때 텍스트 및 아이콘 위에 추가 다음과 같이 표시 됩니다. 

[![잘라내기, 복사 및 붙여넣기 아이콘이 있는 도구 모음 아래쪽의 다이어그램](adding-a-second-toolbar-images/02-bottom-toolbar-sml.png)](adding-a-second-toolbar-images/02-bottom-toolbar.png#lightbox)

탭의 **잘라내기** 메뉴 아이콘 표시 되 고 다음 알림을 사용 하면: 

[![스크린샷의 알림을 잘라내기 메뉴 아이콘을 누른 나타내는](adding-a-second-toolbar-images/03-bottom-tapped-sml.png)](adding-a-second-toolbar-images/03-bottom-tapped.png#lightbox)

결과 알림을 표시 하거나 도구 모음에서 메뉴 항목 탭: 

[![에 대 한 알림을의 스크린 샷 복사, 저장 하 고 누른 되 고 메뉴 항목을 붙여 넣을](adding-a-second-toolbar-images/04-menu-action-sml.png)](adding-a-second-toolbar-images/04-menu-action.png#lightbox)



## <a name="the-up-button"></a>위쪽 단추 

대부분의 Android 응용 프로그램에 의존는 **다시** 응용 프로그램 탐색을 위한 단추를 누르면;는 **다시** 단추 이전 화면으로 이동 합니다.
그러나 수 또한 제공 하려는 **를** 사용자가 응용 프로그램의 주 화면에 "최대" 탐색을 용이 하 게 하는 단추입니다. 사용자가 선택할 때의 **를** 단추를 사용자가 응용 프로그램 계층에서 더 높은 수준으로 위로 &ndash; 즉, 앱 팝 하는 사용자를 다시 팝 다시 이전에 방문한로 대신 백 스택에에 여러 활동 작업입니다. 

사용할 수 있도록는 **를** 단추를 사용 하는 두 번째 활동에는 `Toolbar` 해당 작업 모음으로 호출 하는 `SetDisplayHomeAsUpEnabled` 및 `SetHomeButtonEnabled` 메서드는 두 번째 작업에서 `OnCreate` 메서드:

```csharp
SetActionBar (toolbar);
...
ActionBar.SetDisplayHomeAsUpEnabled (true);
ActionBar.SetHomeButtonEnabled (true);
```

[v7 도구 모음을 지 원하는](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/) 보여 주는 코드 예제는 **를** 실행에서 단추입니다. (사용 하 여 다음에 설명한 AppCompat 라이브러리)이이 샘플에서는 도구 모음을 사용 하 여 두 번째 활동 구현 **를** 이전 활동으로 계층적 탐색 단추입니다. 이 예제는 `DetailActivity` 홈 단추를 사용 하는 **를** 다음 함으로써 단추 `SupportActionBar` 메서드 호출: 

```csharp
SetSupportActionBar (toolbar);
...
SupportActionBar.SetDisplayHomeAsUpEnabled (true);
SupportActionBar.SetHomeButtonEnabled (true);
```

사용자가 탐색 하는 경우 `MainActivity` 를 `DetailActivity`, `DetailActivity` 표시는 **를** 스크린샷에 표시 된 대로 (왼쪽된 화살표) 단추:

[![도구 모음에서 위로 단추 왼쪽된 화살표의 예에서는 스크린 샷](adding-a-second-toolbar-images/05-up-button-sml.png)](adding-a-second-toolbar-images/05-up-button.png#lightbox)

이 탭 **를** 단추를 클릭 하면 응용 프로그램으로 돌아가려면 `MainActivity`합니다. 여러 수준의 계층으로 더 복잡 한 응용 프로그램에서이 단추를 누르면는 반환 사용자 앱에서 다음으로 가장 높은 수준으로 아닌 이전 화면으로 합니다. 



## <a name="related-links"></a>관련 링크

- [롤리팝 도구 모음 (샘플)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat 도구 모음 (샘플)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
