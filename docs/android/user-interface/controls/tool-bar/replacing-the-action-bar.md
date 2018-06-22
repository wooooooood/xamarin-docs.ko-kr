---
title: 작업 모음 교체
ms.prod: xamarin
ms.assetid: 5341D28E-B203-478D-8464-6FAFDC3A4110
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/27/2018
ms.openlocfilehash: d9a5db70999a79d9b968fcd9a27d45e6d73354c0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30772593"
---
# <a name="replacing-the-action-bar"></a>작업 모음 교체

## <a name="overview"></a>개요

가장 일반적인 용도 `Toolbar` 기본 작업 모음 사용자 지정을 `Toolbar` (새 Android 프로젝트를 만든 경우 사용 하 여 기본 작업 모음). 때문에 `Toolbar` 브랜드 로고, 제목, 메뉴 항목, 탐색 단추 및 사용자 지정 보기는 활동의 응용 프로그램 표시줄 섹션을 추가 하는 기능을 제공 합니다. UI를 제공 중요 한 업그레이드를 기본 작업 모음을 통해.

응용 프로그램의 기본 작업 모음으로 대체 하는 `Toolbar`: 

1.  새 사용자 지정 테마를 만들고이 새 테마를 사용 하는 응용 프로그램의 속성을 수정 합니다. 

2.  사용 하지 않도록 설정는 `windowActionBar` 사용자 지정 테마의 특성을 사용 하도록 설정 된 `windowNoTitle` 특성입니다.

3.  에 대 한 레이아웃을 정의 `Toolbar`합니다.

4.  포함 된 `Toolbar` 활동의에서 레이아웃 **Main.axml** 레이아웃 파일. 

5.  활동의 코드를 추가 `OnCreate` 찾을 방법은 `Toolbar` 호출 `SetActionBar` 를 설치 하는 `ToolBar` 작업 모음으로 합니다.

다음 섹션에서는이 프로세스를 자세히 설명합니다. 간단한 앱을 만들고 해당 작업 모음 바뀝니다를 사용자 지정 된 `Toolbar`합니다. 



## <a name="start-an-app-project"></a>응용 프로그램 프로젝트를 시작 합니다.

라는 새 Android 프로젝트 만들기 **ToolbarFun** (참조 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) 새 Android 프로젝트 만들기에 대 한 자세한 내용은). 이 프로젝트를 만든 후 대상 및 최소 Android API 수준으로 설정 **Android 5.0 (API 수준 21-롤리팝)** 이상. Android 버전 수준 설정에 대 한 자세한 내용은 참조 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)합니다. 응용 프로그램을 작성 및 실행이 스크린 샷에 표시 된 대로 기본 작업 모음을 표시 합니다.

[![기본 작업 모음의 스크린샷](replacing-the-action-bar-images/01-before-sml.png)](replacing-the-action-bar-images/01-before.png#lightbox)



## <a name="create-a-custom-theme"></a>사용자 지정 테마를 만들려면

열기는 **리소스/값** 디렉터리와 새 파일을 호출 하는 create **styles.xml**합니다. 해당 내용을 다음 XML로 바꿉니다. 

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

호출 하는 새 사용자 지정 테마를 정의 하는이 XML **MyTheme** 기반으로 하는 **Theme.Material.Light.DarkActionBar** 롤리팝에서 테마입니다. `windowNoTitle` 특성이로 설정 된 `true` 를 숨기려면 제목 표시줄: 

```xml
<item name="android:windowNoTitle">true</item>
```

기본 사용자 지정 도구 모음을 표시 하려면 `ActionBar` 해제 해야 합니다. 

```xml
<item name="android:windowActionBar">false</item>
```

olive-green `colorPrimary` 도구 모음의 배경색에 대 한 설정이 사용 됩니다. 
 
```xml
<item name="android:colorPrimary">#5A8622</item>
```

## <a name="apply-the-custom-theme"></a>사용자 지정 테마를 적용 합니다.

편집 **Properties/AndroidManifest.xml** 다음 추가 `android:theme` 특성을 `<application>` 요소에는 응용 프로그램을 사용 하도록는 `MyTheme` 사용자 지정 테마: 

```xml
<application android:label="@string/app_name" android:theme="@style/MyTheme"></application>
```

응용 프로그램에 사용자 지정 테마를 적용 하는 방법에 대 한 자세한 내용은 참조 [사용자 지정 테마를 사용 하 여](~/android/user-interface/material-theme.md#customtheme)합니다. 



## <a name="define-a-toolbar-layout"></a>도구 모음 레이아웃을 정의 합니다.

에 **리소스/레이아웃** 디렉터리 라는 새 파일을 만들려면 **toolbar.xml**합니다. 해당 내용을 다음 XML로 바꿉니다. 

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

이 XML 정의 사용자 지정 `Toolbar` 하는 기본 작업 모음을 대체 합니다. 최소 높이 `Toolbar` 대체 하는 작업 표시줄의 크기로 설정 합니다. 

```csharp
android:minHeight="?android:attr/actionBarSize"
```

배경색은 `Toolbar` 에서 이전에 정의한 olive-green 색상으로 설정 되어 **styles.xml**:

```csharp
android:background="?android:attr/colorPrimary"
```

롤리팝, 부터는 `android:theme` 개별 보기 스타일 특성을 사용할 수 있습니다. `ThemeOverlay.Material` 롤리팝에 도입 된 테마 수 있도록 기본 오버레이할 `Theme.Material` 테마, 밝은 또는 어두운 수 있도록 하기 위한 관련 특성을 덮어씁니다. 이 예제는 `Toolbar` 부분은 연한 색이 특성의 내용은 있도록 어두운 테마를 사용 하 여: 

```csharp
android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
```

이 설정은 짙을 수록 배경색으로 메뉴 항목 대비 있도록 사용 됩니다.



## <a name="include-the-toolbar-layout"></a>도구 모음 레이아웃 포함

레이아웃 파일을 편집 **Resources/layout/Main.axml** 해당 내용을 다음 XML로 바꿉니다.

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

이 레이아웃에 포함 됩니다는 `Toolbar` 에 정의 된 **toolbar.xml** 사용 하 여는 `RelativeLayout` 되도록 지정 하려면는 `Toolbar` (위 단추) UI의 맨 위쪽에 배치 하는 것입니다. 



## <a name="find-and-activate-the-toolbar"></a>찾기 및 도구 모음을 활성화

편집 **MainActivity.cs** 다음 추가 문을 사용 하 여:

```csharp
using Android.Views;
```

또한 다음 코드 줄의 끝에 추가 `OnCreate` 메서드:

```csharp
var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
SetActionBar(toolbar);
ActionBar.Title = "My Toolbar";
```

이 코드를 찾습니다는 `Toolbar` 및 호출 `SetActionBar` 있도록는 `Toolbar` 기본 작업 모음 특성에 대해 수행 합니다. 도구 모음의 제목으로 변경 된 **도구 모음 내**합니다. 이 코드 예제에 표시 되는 `ToolBar` 작업 모음으로 직접 참조 될 수 있습니다. 컴파일 및 실행이 응용 프로그램 &ndash; 는 사용자 지정 된 `Toolbar` 기본 작업 모음 위치에 표시 됩니다. 

[![녹색 색 구성표에 맞춰 사용자 지정된 도구 모음의 스크린 샷](replacing-the-action-bar-images/02-after-sml.png)](replacing-the-action-bar-images/02-after.png#lightbox)

에 `Toolbar` 와 독립적으로 스타일 지정는 `Theme.Material.Light.DarkActionBar` 응용 프로그램의 나머지 부분에 적용 되는 테마입니다. 

응용 프로그램을 실행 하는 동안 예외가 발생 하는 경우 참조는 [문제 해결](#troubleshooting) 아래 섹션.

 
## <a name="add-menu-items"></a>메뉴 항목 추가 

이 섹션에서는 메뉴에 추가 되는 `Toolbar`합니다. 오른쪽 위 영역은 `ToolBar` 메뉴 항목에 예약 되어 &ndash; 각 메뉴 항목 (라고도 *작업 항목*) 현재 활동 내에서 작업을 수행할 수 또는 전체 응용 프로그램을 대신 하 여 작업을 수행할 수 있습니다. 

메뉴를 추가 하는 `Toolbar`: 

1.  추가할 메뉴 아이콘 (필요한 경우)는 `mipmap-` 응용 프로그램 프로젝트의 폴더입니다. 에 사용 가능한 메뉴 아이콘 집합을 제공 하는 Google에서 [자재 아이콘](https://design.google.com/icons/) 페이지. 

2.  새 메뉴 리소스 파일을 추가 하 여 메뉴 항목의 내용을 정의 **리소스/메뉴**합니다. 

3.  구현 된 `OnCreateOptionsMenu` 활동 메서드 &ndash; 이 메서드는 메뉴 항목을 확장 합니다. 

4.  구현 된 `OnOptionsItemSelected` 활동 메서드 &ndash; 메뉴 항목의 탭이 수행 되는 경우이 메서드는 동작을 수행 합니다. 

다음 섹션에서는 추가 하 여이 프로세스를 자세히 보여 **편집** 및 **저장** 메뉴 항목을 사용자 지정 `Toolbar`합니다. 



### <a name="install-menu-icons"></a>설치 메뉴 아이콘

계속는 `ToolbarFun` 예제 응용 프로그램, 응용 프로그램 프로젝트에 메뉴 아이콘을 추가 합니다. 다운로드 [도구 모음 아이콘](https://github.com/xamarin/monodroid-samples/blob/master/Supportv7/AppCompat/Toolbar/Resources/toolbar-icons-plus.zip?raw=true), 압축, 및의 압축을 푼 내용을 복사 *mip 맵-* 프로젝트에 폴더 *mip 맵-* 아래에 폴더 **ToolbarFun / 리소스** 프로젝트에 각 추가 된 아이콘 파일을 포함 합니다.


### <a name="define-a-menu-resource"></a>메뉴 리소스를 정의 합니다.

새 **메뉴** 아래에 하위 디렉터리 **리소스**합니다. 에 **메뉴** 하위 디렉터리를 라는 새 메뉴 리소스 파일을 만듭니다 **top_menus.xml** 해당 내용을 다음 XML로 바꿉니다. 

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

-   **편집** 메뉴 항목을 사용 하는 `ic_action_content_create.png` 아이콘 (연필). 

-   A **저장** 메뉴 항목을 사용 하는 `ic_action_content_save.png` 아이콘 (디스켓). 

-   A **기본 설정** 아이콘이 없는 메뉴 항목입니다.

`showAsAction` 의 특성은 **편집** 및 **저장** 메뉴 항목으로 설정 된 `ifRoom` &ndash; 이 설정을 사용 하면 이러한 새 메뉴 항목에는 `Toolbar` 없는 경우 충분 한 공간이 대로 표시 됩니다. **기본 설정** 메뉴 항목 집합 `showAsAction` 를 `never` &ndash; 이 인해는 **기본 설정** 메뉴에 나타나는 데는 *오버플로* 메뉴 (3 세로 점선)입니다. 


### <a name="implement-oncreateoptionsmenu"></a>OnCreateOptionsMenu 구현

다음 메서드를 추가 **MainActivity.cs**:

```csharp
public override bool OnCreateOptionsMenu(IMenu menu)
{
    MenuInflater.Inflate(Resource.Menu.top_menus, menu);
    return base.OnCreateOptionsMenu(menu);
}
```

Android 호출은 `OnCreateOptionsMenu` 메서드 응용 프로그램 작업에 대 한 메뉴 리소스를 지정할 수 있도록 합니다. 이 메서드에서 **top_menus.xml** 리소스 확장으로 전달 된 `menu`합니다. 이 코드는 새 **편집**, **저장**, 및 **기본 설정** 메뉴 항목에는 `Toolbar`합니다. 



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

Android를 호출 하는 사용자가 메뉴 항목을 누르면는 `OnOptionsItemSelected` 메서드와 선택 된 메뉴 항목에 전달 합니다. 이 예제에서는 구현이 메뉴 항목을 누른 나타내기 위해 알림 표시 됩니다. 

빌드 및 실행 `ToolbarFun` 도구 모음에서 새 메뉴 항목을 표시 합니다. `Toolbar` 이제이 스크린 샷에 표시 된 대로 세 개의 메뉴 아이콘이 표시 됩니다. 

[![오버플로 메뉴 항목 및 저장, 편집의 위치를 보여 주는 다이어그램](replacing-the-action-bar-images/04-menu-items-sml.png)](replacing-the-action-bar-images/04-menu-items.png#lightbox)

때 사용자 탭의 **편집** 메뉴 항목을 알림 메시지를 나타내는 표시 됩니다는 `OnOptionsItemSelected` 메서드를 호출 했습니다. 

[![스크린샷의 알림을 편집 항목 탭이 수행 되는 경우에 표시](replacing-the-action-bar-images/05-toast-displayed-sml.png)](replacing-the-action-bar-images/05-toast-displayed.png#lightbox)

사용자가 오버플로 메뉴를 누를 때는 **기본 설정** 메뉴 항목이 표시 됩니다. 보다 덜 일반적인 동작을 오버플로 메뉴로에 배치 해야 하는 일반적으로 &ndash; 사용 하 여이 예제에 대 한 오버플로 메뉴 **기본 설정** 자주 사용 되지 않으므로으로 **편집** 및  **저장**: 

[![오버플로 메뉴에 표시 되는 기본 설정의 스크린샷 메뉴 항목](replacing-the-action-bar-images/06-preferences-sml.png)](replacing-the-action-bar-images/06-preferences.png#lightbox)

Android 메뉴에 대 한 자세한 내용은 Android 개발자 참조 [메뉴](https://developer.android.com/guide/topics/ui/menus.html) 항목입니다. 
 

## <a name="troubleshooting"></a>문제 해결

다음 팁 작업 모음 도구 모음으로 대체 하는 동안 발생할 수 있는 문제를 디버그할 수 있습니다.

### <a name="activity-already-has-an-action-bar"></a>활동에는 이미 작업 모음

응용 프로그램에 설명 된 대로 사용자 지정 테마를 사용 하도록 제대로 구성 되지 않은 경우 [사용자 지정 테마를 적용](#apply-the-custom-theme), 응용 프로그램을 실행 하는 동안 다음 예외가 발생할 수 있습니다.

![사용자 지정 테마를 사용 하지 않는 경우 발생할 수 있는 오류](replacing-the-action-bar-images/03-theme-not-defined.png)

또한 오류 메시지 인 다음 만들어질 수 있습니다 같은: _Java.Lang.IllegalStateException:이 작업에 이미 창 장식에서 제공 하는 작업 모음입니다._ 

이 오류를 해결 하려면 확인 하는 `android:theme` 특성이 사용자 지정 테마에 추가 됩니다 `<application>` (에서 **Properties/AndroidManifest.xml**)에서 이전에 설명 된 대로 [사용자지정테마를적용](#apply-the-custom-theme). 또한이 오류가 발생할 수 하는 경우는 `Toolbar` 레이아웃 또는 사용자 지정 테마를 제대로 구성 되지 않았습니다.


## <a name="related-links"></a>관련 링크

- [롤리팝 도구 모음 (샘플)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat 도구 모음 (샘플)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
