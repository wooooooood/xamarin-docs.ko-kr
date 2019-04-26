---
title: 작업 모음 바꾸기
ms.prod: xamarin
ms.assetid: 5341D28E-B203-478D-8464-6FAFDC3A4110
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/27/2018
ms.openlocfilehash: 9e9fa1e2651661670f89baac7fcd438b3d14bfb3
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61201012"
---
# <a name="replacing-the-action-bar"></a>작업 모음 바꾸기

## <a name="overview"></a>개요

에 대 한 가장 일반적인 이유 중 하나를 사용 합니다 `Toolbar` 사용자 지정을 사용 하 여 기본 작업 모음을 대체 하는 `Toolbar` (새 Android 프로젝트를 만들면 사용 하 여 기본 작업 모음). 때문에 `Toolbar` 브랜드 로고, 제목, 메뉴 항목, 탐색 단추 및 사용자 지정 뷰 작업의 앱 표시줄 부분에 추가할 수 있게 UI 것에 비해 중요 한 업그레이드를 기본 작업 모음입니다.

사용 하 여 앱의 기본 작업 표시줄의 이름을 바꾸려면는 `Toolbar`: 

1.  새 사용자 지정 테마를 만들고이 새 테마를 사용할 수 있도록 앱의 속성을 수정 합니다. 

2.  사용 하지 않도록 설정 합니다 `windowActionBar` 사용자 지정 테마의 특성 및 사용 하도록 설정 합니다 `windowNoTitle` 특성입니다.

3.  레이아웃을 정의 합니다 `Toolbar`합니다.

4.  포함 된 `Toolbar` 활동의 레이아웃 **Main.axml** 레이아웃 파일입니다. 

5.  활동의 코드를 추가 `OnCreate` 찾으려고 메서드를 `Toolbar` 호출 `SetActionBar` 를 설치 하는 `ToolBar` 작업 모음으로 합니다.

다음 섹션에서는이 프로세스에 자세히 설명합니다. 간단한 앱을 만들고 해당 작업 표시줄 대신 사용자 지정을 사용 하 여 `Toolbar`입니다. 



## <a name="start-an-app-project"></a>앱 프로젝트를 시작 합니다.

라는 새 Android 프로젝트를 만듭니다 **ToolbarFun** (참조 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) 새 Android 프로젝트를 만드는 방법에 대 한 자세한 내용은). 이 프로젝트를 만든 후 대상 및 최소 Android API 수준으로 설정 **Android 5.0 (API 수준 21-Lollipop)** 이상. Android 버전 수준 설정 하는 방법에 대 한 자세한 내용은 참조 하세요. [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)합니다. 앱을 빌드하고 실행 하는 경우 기본 작업 모음이 스크린샷에 표시 된 것으로 표시 합니다.

[![기본 작업 표시줄의 스크린샷](replacing-the-action-bar-images/01-before-sml.png)](replacing-the-action-bar-images/01-before.png#lightbox)



## <a name="create-a-custom-theme"></a>사용자 지정 테마를 만들려면

엽니다는 **리소스/값** 디렉터리와 새 파일을 호출 하는 create **styles.xml**합니다. 다음 XML을 사용 하 여 해당 내용을 바꿉니다. 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
  <style name="MyTheme" parent="@android:style/Theme.Material.Light.DarkActionBar">
    <item name="android:windowNoTitle">true</item>
    <item name="android:windowActionBar">false</item>
    <item name="android:colorPrimary">#5A8622</item>
  </style>
</resources>
```

이라는 새 사용자 지정 테마를 정의 하는이 XML **MyTheme** 기반으로 하는 합니다 **Theme.Material.Light.DarkActionBar** Lollipop의 테마입니다. 합니다 `windowNoTitle` 특성이로 설정 된 `true` 를 숨기려면 제목 표시줄: 

```xml
<item name="android:windowNoTitle">true</item>
```

기본 사용자 지정 도구 모음을 표시할 `ActionBar` 비활성화 해야 합니다. 

```xml
<item name="android:windowActionBar">false</item>
```

olive-green `colorPrimary` 설정은 도구 모음의 배경색에 사용 됩니다. 
 
```xml
<item name="android:colorPrimary">#5A8622</item>
```

## <a name="apply-the-custom-theme"></a>사용자 지정 테마를 적용 합니다.

편집 **Properties/AndroidManifest.xml** 다음을 추가 합니다 `android:theme` 특성을 `<application>` 요소에 앱을 사용 하도록를 `MyTheme` 사용자 지정 테마: 

```xml
<application android:label="@string/app_name" android:theme="@style/MyTheme"></application>
```

앱에 사용자 지정 테마를 적용 하는 방법에 대 한 자세한 내용은 참조 하세요. [사용자 지정 테마를 사용 하 여](~/android/user-interface/material-theme.md#customtheme)입니다. 



## <a name="define-a-toolbar-layout"></a>도구 모음 레이아웃을 정의 합니다.

에 **리소스/레이아웃** 디렉터리 라는 새 파일을 만듭니다 **toolbar.xml**합니다. 다음 XML을 사용 하 여 해당 내용을 바꿉니다. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?android:attr/actionBarSize"
    android:background="?android:attr/colorPrimary"
    android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"/>
```

이 XML 정의 사용자 지정 `Toolbar` 기본 작업 모음 대체 합니다. 최소 높이 `Toolbar` 대체 하는 작업 표시줄의 크기로 설정 됩니다. 

```csharp
android:minHeight="?android:attr/actionBarSize"
```

배경색을 `Toolbar` 에서 이전에 정의한 olive-green 색상으로 설정 됩니다 **styles.xml**:

```csharp
android:background="?android:attr/colorPrimary"
```

Lollipop부터는 `android:theme` 개별 뷰에 스타일 특성을 사용할 수 있습니다. 합니다 `ThemeOverlay.Material` Lollipop에 도입 된 테마 수 있도록 기본 오버레이 `Theme.Material` 테마, 밝은 또는 어두운 수 있도록 하기 위한 관련 특성을 덮어씁니다. 이 예제에서는 `Toolbar` 어두운 테마를 사용 하 여 해당 내용을 부분은 연한 색이 되도록 합니다. 

```csharp
android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
```

음영이 짙을 수록 배경색을 사용 하 여 메뉴 항목 대조 되도록이 설정이 사용 됩니다.



## <a name="include-the-toolbar-layout"></a>도구 모음 레이아웃 포함

레이아웃 파일을 편집 **Resources/layout/Main.axml** 다음 XML을 사용 하 여 해당 내용을 바꿉니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <include
        android:id="@+id/toolbar"
        layout="@layout/toolbar" />
</RelativeLayout>
```

이 레이아웃에 포함 됩니다는 `Toolbar` 에 정의 된 **toolbar.xml** 사용 하는 `RelativeLayout` 지정 하는 `Toolbar` (단추) 위 UI의 맨 위쪽에 배치 하는 것입니다. 



## <a name="find-and-activate-the-toolbar"></a>찾기 및 도구 모음 활성화

편집할 **MainActivity.cs** 추가한 다음 문을 사용 하 여:

```csharp
using Android.Views;
```

또한 다음 코드 줄의 끝에 추가 된 `OnCreate` 메서드:

```csharp
var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
SetActionBar(toolbar);
ActionBar.Title = "My Toolbar";
```

이 코드에서는 합니다 `Toolbar` 및 호출 `SetActionBar` 있도록는 `Toolbar` 기본 작업 표시줄 특성에 걸립니다. 도구 모음의 제목으로 변경 됩니다 **도구 모음 내**합니다. 이 코드 예제 에서처럼는 `ToolBar` 작업 모음으로 직접 참조 될 수 있습니다. 컴파일 및이 앱을 실행 &ndash; 사용자 지정 `Toolbar` 기본 작업 모음 대신 표시 됩니다. 

[![녹색 색 구성표를 사용 하 여 사용자 지정 도구 모음의 스크린 샷](replacing-the-action-bar-images/02-after-sml.png)](replacing-the-action-bar-images/02-after.png#lightbox)

있음을 합니다 `Toolbar` 와 독립적으로 스타일 지정을 `Theme.Material.Light.DarkActionBar` 앱의 나머지 부분에 적용 되는 테마입니다. 

앱을 실행 하는 동안 예외가 발생 하는 경우 참조를 [문제 해결](#troubleshooting) 아래의 섹션입니다.

 
## <a name="add-menu-items"></a>메뉴 항목 추가 

이 섹션에서는 메뉴에 추가 되는 `Toolbar`합니다. 오른쪽 위 영역 합니다 `ToolBar` 는 메뉴 항목에 대 한 예약 &ndash; 각 메뉴 항목 (라고도 *작업 항목*) 현재 활동 내의 작업을 수행할 수 또는 전체 앱을 대신 하 여 작업을 수행할 수 있습니다. 

메뉴를 추가 하 여 `Toolbar`: 

1.  추가할 메뉴 아이콘 (필요한 경우)는 `mipmap-` 앱 프로젝트의 폴더입니다. Google에서 사용 가능한 메뉴 아이콘 집합을 제공 합니다 [재질 아이콘](https://design.google.com/icons/) 페이지입니다. 

2.  새 메뉴 리소스 파일을 추가 하 여 메뉴 항목의 내용을 정의 **리소스/메뉴**합니다. 

3.  구현 된 `OnCreateOptionsMenu` 활동의 메서드 &ndash; 이 메서드는 메뉴 항목을 확장 합니다. 

4.  구현 된 `OnOptionsItemSelected` 활동의 메서드 &ndash; 메뉴 항목을를 탭 할 때이 메서드는 작업을 수행 합니다. 

다음 섹션에 추가 하 여이 프로세스에 자세히 보여 줍니다 **편집** 하 고 **저장** 메뉴 항목 사용자 지정을 `Toolbar`입니다. 



### <a name="install-menu-icons"></a>설치 메뉴 아이콘

계속 진행 합니다 `ToolbarFun` 예제 앱 메뉴 아이콘을 앱 프로젝트에 추가 합니다. 다운로드 [도구 모음 아이콘](https://github.com/xamarin/monodroid-samples/blob/master/Supportv7/AppCompat/Toolbar/Resources/toolbar-icons-plus.zip?raw=true)를 압축을 풀고 추출한의 내용을 복사 합니다 *mipmap-* 프로젝트 폴더 *mipmap-* 아래에 폴더 **ToolbarFun / 리소스** 프로젝트의 각 추가 아이콘 파일을 포함 합니다.


### <a name="define-a-menu-resource"></a>메뉴 리소스를 정의 합니다.

새 **메뉴** 아래에 하위 디렉터리 **리소스**합니다. 에 **메뉴** 하위 디렉터리 라는 새 메뉴 리소스 파일을 만듭니다 **top_menus.xml** 다음 XML을 사용 하 여 해당 내용을 바꿉니다. 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item
       android:id="@+id/menu_edit"
       android:icon="@mipmap/ic_action_content_create"
       android:showAsAction="ifRoom"
       android:title="Edit" />
  <item
       android:id="@+id/menu_save"
       android:icon="@mipmap/ic_action_content_save"
       android:showAsAction="ifRoom"
       android:title="Save" />
  <item
       android:id="@+id/menu_preferences"
       android:showAsAction="never"
       android:title="Preferences" />
</menu>
```

이 XML 세 가지 메뉴 항목을 만듭니다.

-   **편집할** 메뉴 항목을 사용 하는 `ic_action_content_create.png` 아이콘 (연필). 

-   A **저장** 사용 하는 메뉴 항목을 `ic_action_content_save.png` 아이콘 (디스켓). 

-   A **기본 설정** 아이콘이 없는 메뉴 항목입니다.

`showAsAction` 특성을 **편집** 및 **저장** 메뉴 항목으로 설정 됩니다 `ifRoom` &ndash; 이렇게이 설정 하면 이러한 메뉴 항목에 표시는 `Toolbar` 없는 경우 충분 한 공간이 표시 됩니다. **기본 설정** 메뉴 항목 집합 `showAsAction` 에 `never` &ndash; 이 인해를 **기본 설정** 메뉴에 표시 합니다 *오버플로* 메뉴 (3 개 세로 점)입니다. 


### <a name="implement-oncreateoptionsmenu"></a>OnCreateOptionsMenu 구현

다음 메서드를 추가 **MainActivity.cs**:

```csharp
public override bool OnCreateOptionsMenu(IMenu menu)
{
    MenuInflater.Inflate(Resource.Menu.top_menus, menu);
    return base.OnCreateOptionsMenu(menu);
}
```

Android 호출을 `OnCreateOptionsMenu` 메서드 앱 활동에 대 한 메뉴 리소스를 지정할 수 있도록 합니다. 이 메서드에서 **top_menus.xml** 리소스 확장에 전달 된 `menu`합니다. 이 코드를 사용 하면 새 **편집**를 **저장**, 및 **기본 설정** 에 표시할 메뉴 항목을 `Toolbar`입니다. 



### <a name="implement-onoptionsitemselected"></a>OnOptionsItemSelected 구현

다음 메서드를 추가 **MainActivity.cs**:

```csharp
public override bool OnOptionsItemSelected(IMenuItem item)
{
    Toast.MakeText(this, "Action selected: " + item.TitleFormatted,
        ToastLength.Short).Show();
    return base.OnOptionsItemSelected(item);
}
```

사용자 메뉴 항목을 누르면 Android 호출을 `OnOptionsItemSelected` 메서드와 선택 된 메뉴 항목에 전달 합니다. 이 예제에서는 구현을 메뉴 항목을 누른 나타내는 알림을 표시 합니다. 

빌드 및 실행 `ToolbarFun` 도구 모음에서 새 메뉴 항목을 표시 합니다. `Toolbar` 이제이 스크린샷에 표시 된 것 처럼 세 가지 메뉴 아이콘을 표시 합니다. 

[![오버플로 메뉴 항목 및 저장, 편집의 위치를 보여 주는 다이어그램](replacing-the-action-bar-images/04-menu-items-sml.png)](replacing-the-action-bar-images/04-menu-items.png#lightbox)

때 사용자 탭의 **편집할** 알림 메뉴 항목을 나타내기 위해 표시 됩니다는 `OnOptionsItemSelected` 메서드를 호출한: 

[![스크린 샷의 알림 항목 편집을 탭 할 때를 표시 합니다.](replacing-the-action-bar-images/05-toast-displayed-sml.png)](replacing-the-action-bar-images/05-toast-displayed.png#lightbox)

사용자가 오버플로 메뉴를 누를 때 합니다 **기본 설정** 메뉴 항목이 표시 됩니다. 덜 일반적인 작업을 오버플로 메뉴에 배치 해야 하는 일반적으로 &ndash; 에 대 한 오버플로 메뉴를 사용 하는이 예제 **기본 설정** 자주 사용 되지 않으므로로 **편집** 고  **저장**: 

[![오버플로 메뉴에 표시 되는 기본 설정의 스크린 샷 메뉴 항목](replacing-the-action-bar-images/06-preferences-sml.png)](replacing-the-action-bar-images/06-preferences.png#lightbox)

Android 메뉴에 대 한 자세한 내용은 Android 개발자 참조 [메뉴](https://developer.android.com/guide/topics/ui/menus.html) 항목입니다. 
 

## <a name="troubleshooting"></a>문제 해결

다음 팁을 도구 모음을 사용 하 여 작업 모음을 대체 하는 동안 발생할 수 있는 문제 디버그에 도움이 됩니다.

### <a name="activity-already-has-an-action-bar"></a>활동에는 이미 작업 모음

앱에 설명 된 대로 사용자 지정 테마를 사용 하도록 올바르게 구성 되지 않은 경우 [사용자 지정 테마를 적용](#apply-the-custom-theme), 앱을 실행 하는 동안 다음 예외가 발생할 수 있습니다.

![사용자 지정 테마를 사용 하지 않으면 발생할 수 있는 오류](replacing-the-action-bar-images/03-theme-not-defined.png)

또한 다음과 같은 오류 메시지가 생성 될 수 있습니다. _Java.Lang.IllegalStateException: 이 작업 창 장식에서 제공 하는 작업 표시줄을 이미 있습니다._ 

이 오류를 해결 하려면 있는지를 확인 합니다 `android:theme` 특성이 사용자 지정 테마를 추가할 `<application>` (에서 **Properties/AndroidManifest.xml**)에서 이전에 설명 된 대로 [사용자지정테마를적용](#apply-the-custom-theme). 또한이 오류가 발생할 수 하는 경우는 `Toolbar` 레이아웃 또는 사용자 지정 테마 제대로 구성 되지 않았습니다.


## <a name="related-links"></a>관련 링크

- [롤리팝 도구 모음 (샘플)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat 도구 모음 (샘플)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
