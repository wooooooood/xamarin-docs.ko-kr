---
title: 작업 모음 바꾸기
ms.prod: xamarin
ms.assetid: 5341D28E-B203-478D-8464-6FAFDC3A4110
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/27/2018
ms.openlocfilehash: f20568b5d76fcc1788d19497e372bcd0cecc61ff
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029082"
---
# <a name="replacing-the-action-bar"></a>작업 모음 바꾸기

## <a name="overview"></a>개요

`Toolbar`를 사용 하는 가장 일반적인 방법 중 하나는 기본 작업 모음을 사용자 지정 `Toolbar`으로 바꾸는 것입니다. 새 Android 프로젝트를 만들 때 기본 작업 모음을 사용 합니다. `Toolbar`는 브랜드 로고, 제목, 메뉴 항목, 탐색 단추 및 사용자 지정 보기를 활동 UI의 앱 바 섹션에 추가할 수 있는 기능을 제공 하므로 기본 작업 모음에 대 한 중요 한 업그레이드를 제공 합니다.

앱의 기본 작업 모음을 `Toolbar`로 바꾸려면 다음을 수행 합니다. 

1. 새 사용자 지정 테마를 만들고이 새 테마를 사용 하도록 앱의 속성을 수정 합니다. 

2. 사용자 지정 테마에서 `windowActionBar` 특성을 사용 하지 않도록 설정 하 고 `windowNoTitle` 특성을 사용 하도록 설정 합니다.

3. `Toolbar`의 레이아웃을 정의 합니다.

4. 활동의 **주. axml** 레이아웃 파일에 `Toolbar` 레이아웃을 포함 합니다. 

5. 활동의 `OnCreate` 메서드에 코드를 추가 하 여 `Toolbar`를 찾고 `SetActionBar`를 호출 하 여 `ToolBar`를 작업 모음으로 설치 합니다.

다음 섹션에서는이 프로세스에 대해 자세히 설명 합니다. 간단한 앱이 생성 되 고 해당 작업 모음이 사용자 지정 된 `Toolbar`로 바뀝니다. 

## <a name="start-an-app-project"></a>앱 프로젝트 시작

새 android 프로젝트 만들기 (새 Android 프로젝트 만들기에 대 한 자세한 내용은 [Hello, android](~/android/get-started/hello-android/hello-android-quickstart.md) 참조 **) 라는 새** android 프로젝트를 만듭니다. 이 프로젝트를 만든 후에는 대상 및 최소 Android API 수준을 **android 5.0 (API 레벨 21-롤리팝)** 이상으로 설정 합니다. Android 버전 수준을 설정 하는 방법에 대 한 자세한 내용은 [ANDROID API 수준 이해](~/android/app-fundamentals/android-api-levels.md)를 참조 하세요. 앱을 빌드하고 실행 하면 다음 스크린샷에 표시 된 것 처럼 기본 작업 모음이 표시 됩니다.

[기본 작업 표시줄의![스크린샷](replacing-the-action-bar-images/01-before-sml.png)](replacing-the-action-bar-images/01-before.png#lightbox)

## <a name="create-a-custom-theme"></a>사용자 지정 테마 만들기

**Resources/values** 디렉터리를 열고 **스타일 .xml**이라는 새 파일을 만듭니다. 내용을 다음 XML로 바꿉니다. 

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

이 XML은 롤리팝의 **DarkActionBar** 테마를 기반으로 하는 **mytheme** 이라는 새로운 사용자 지정 테마를 정의 합니다. `windowNoTitle` 특성은 `true`으로 설정 되어 제목 표시줄을 숨깁니다. 

```xml
<item name="android:windowNoTitle">true</item>
```

사용자 지정 도구 모음을 표시 하려면 기본 `ActionBar`를 사용 하지 않도록 설정 해야 합니다. 

```xml
<item name="android:windowActionBar">false</item>
```

짙은 녹색 `colorPrimary` 설정은 도구 모음의 배경색에 사용 됩니다. 

```xml
<item name="android:colorPrimary">#5A8622</item>
```

## <a name="apply-the-custom-theme"></a>사용자 지정 테마 적용

**Properties/AndroidManifest** 을 편집 하 고 앱이 `MyTheme` 사용자 지정 테마를 사용 하도록 `<application>` 요소에 다음 `android:theme` 특성을 추가 합니다. 

```xml
<application android:label="@string/app_name" android:theme="@style/MyTheme"></application>
```

앱에 사용자 지정 테마를 적용 하는 방법에 대 한 자세한 내용은 [사용자 지정 테마 사용](~/android/user-interface/material-theme.md#customtheme)을 참조 하세요. 

## <a name="define-a-toolbar-layout"></a>도구 모음 레이아웃 정의

**리소스/레이아웃** 디렉터리에서 **toolbar .xml**이라는 새 파일을 만듭니다. 내용을 다음 XML로 바꿉니다. 

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

이 XML은 기본 작업 모음을 대체 하는 사용자 지정 `Toolbar`를 정의 합니다. `Toolbar`의 최소 높이는 대체 하는 작업 모음 크기로 설정 됩니다. 

```csharp
android:minHeight="?android:attr/actionBarSize"
```

`Toolbar`의 배경색은 **스타일 .xml**에서 이전에 정의 된 황록색 색으로 설정 됩니다.

```csharp
android:background="?android:attr/colorPrimary"
```

롤리팝부터 `android:theme` 특성을 사용 하 여 개별 뷰의 스타일을 지정할 수 있습니다. 롤리팝으로 도입 된 `ThemeOverlay.Material` 테마를 사용 하 여 기본 `Theme.Material` 테마를 오버레이 하 고, 관련 특성을 덮어써서 가볍고 어둡게 만들 수 있습니다. 이 예제에서 `Toolbar`는 짙은 테마를 사용 하 여 내용이 밝은 색으로 되어 있습니다. 

```csharp
android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
```

이 설정은 메뉴 항목이 진한 배경색과 대조 되도록 사용 됩니다.

## <a name="include-the-toolbar-layout"></a>도구 모음 레이아웃 포함

Layout file **Resources/layout/Main. axml** 을 편집 하 고 내용을 다음 xml로 바꿉니다.

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

이 레이아웃에는 **toolbar .xml** 에 정의 된 `Toolbar` 포함 되며 `RelativeLayout`를 사용 하 여 `Toolbar`를 UI의 맨 위에 배치 하도록 지정 합니다 (단추 위). 

## <a name="find-and-activate-the-toolbar"></a>도구 모음 찾기 및 활성화

**MainActivity.cs** 를 편집 하 고 다음 using 문을 추가 합니다.

```csharp
using Android.Views;
```

또한 `OnCreate` 메서드의 끝에 다음 코드 줄을 추가 합니다.

```csharp
var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
SetActionBar(toolbar);
ActionBar.Title = "My Toolbar";
```

이 코드는 `Toolbar`를 찾고 `SetActionBar`를 호출 하 여 `Toolbar`가 기본 작업 모음 특성에 적용 되도록 합니다. 도구 모음의 제목이 **내 도구 모음**으로 변경 됩니다. 이 코드 예제에서 볼 수 있듯이 `ToolBar`를 작업 모음으로 직접 참조할 수 있습니다. 사용자 지정 된 `Toolbar` 기본 작업 모음 대신 표시 &ndash;이 앱을 컴파일하고 실행 합니다. 

[녹색 색 구성표를 사용 하는 사용자 지정 된 도구 모음의 스크린샷![](replacing-the-action-bar-images/02-after-sml.png)](replacing-the-action-bar-images/02-after.png#lightbox)

`Toolbar`는 응용 프로그램의 나머지 부분에 적용 되는 `Theme.Material.Light.DarkActionBar` 테마와는 독립적으로 스타일이 지정 됩니다. 

앱을 실행 하는 동안 예외가 발생 하는 경우 아래의 [문제 해결](#troubleshooting) 섹션을 참조 하세요.

## <a name="add-menu-items"></a>메뉴 항목 추가 

이 섹션에서는 메뉴를 `Toolbar`에 추가 합니다. `ToolBar`의 오른쪽 위 영역은 메뉴 항목 &ndash; 예약 되어 있습니다. 각 메뉴 항목 ( *작업 항목이*라고도 함)은 현재 활동 내에서 작업을 수행 하거나 전체 앱을 대신 하 여 작업을 수행할 수 있습니다. 

`Toolbar`에 메뉴를 추가 하려면 다음을 수행 합니다. 

1. 앱 프로젝트의 `mipmap-` 폴더에 메뉴 아이콘 (필요한 경우)을 추가 합니다. Google은 [재질 아이콘](https://design.google.com/icons/) 페이지에 일련의 자유 메뉴 아이콘을 제공 합니다. 

2. **리소스/메뉴**아래에 새 메뉴 리소스 파일을 추가 하 여 메뉴 항목의 내용을 정의 합니다. 

3. 이 메서드가 메뉴 항목을 늘어납니다 하 &ndash; 활동의 `OnCreateOptionsMenu` 메서드를 구현 합니다. 

4. 메뉴 항목을 탭 할 때 작업을 수행 하 &ndash; 작업의 `OnOptionsItemSelected` 메서드를 구현 합니다. 

다음 섹션에서는 사용자 지정 된 `Toolbar`에 **편집** 및 **저장** 메뉴 항목을 추가 하 여이 프로세스에 대해 자세히 설명 합니다. 

### <a name="install-menu-icons"></a>메뉴 아이콘 설치

`ToolbarFun` 예제 앱을 계속 진행 하 여 앱 프로젝트에 메뉴 아이콘을 추가 합니다. [도구 모음 아이콘](https://github.com/xamarin/monodroid-samples/blob/master/Supportv7/AppCompat/Toolbar/Resources/toolbar-icons-plus.zip?raw=true)을 다운로드 하 고 압축을 *풀고 압축을 푼 파일* 의 콘텐츠를 프로젝트에 포함 된 프로젝트의 *밉 맵* 폴더에 복사 합니다. 여기에는 프로젝트에 추가 된 각 아이콘 파일이 포함 됩니다.

### <a name="define-a-menu-resource"></a>메뉴 리소스 정의

**리소스**에서 새 **메뉴** 하위 디렉터리를 만듭니다. **메뉴** 하위 디렉터리에서 **top_menus** 라는 새 메뉴 리소스 파일을 만들고 해당 내용을 다음 xml로 바꿉니다. 

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

이 XML은 세 가지 메뉴 항목을 만듭니다.

- `ic_action_content_create.png` 아이콘 (연필)을 사용 하는 **편집** 메뉴 항목입니다. 

- `ic_action_content_save.png` 아이콘 (디스켓)을 사용 하는 **저장** 메뉴 항목입니다. 

- 아이콘이 없는 **기본 설정** 메뉴 항목입니다.

**편집** 및 **저장** 메뉴 항목의 `showAsAction` 특성은 &ndash; `ifRoom`으로 설정 되어 있습니다 .이 설정을 통해 이러한 메뉴 항목을 표시할 충분 한 공간이 있는 경우 `Toolbar`에 표시 됩니다. **기본 설정** 메뉴 항목은 `showAsAction` 설정 `never` &ndash;이로 인해 **기본 설정** 메뉴가 *오버플로* 메뉴 (세로 3 개 점)에 표시 됩니다. 

### <a name="implement-oncreateoptionsmenu"></a>Oncreate메뉴 구현 메뉴

**MainActivity.cs**에 다음 메서드를 추가 합니다.

```csharp
public override bool OnCreateOptionsMenu(IMenu menu)
{
    MenuInflater.Inflate(Resource.Menu.top_menus, menu);
    return base.OnCreateOptionsMenu(menu);
}
```

Android는 앱에서 활동에 대 한 메뉴 리소스를 지정할 수 있도록 `OnCreateOptionsMenu` 메서드를 호출 합니다. 이 메서드에서 **top_menus** 리소스는 전달 된 `menu`으로 팽창 됩니다. 이 코드는 새 **편집**, **저장**및 **기본 설정** 메뉴 항목이 `Toolbar`표시 되도록 합니다. 

### <a name="implement-onoptionsitemselected"></a>OnOptionsItemSelected 구현

**MainActivity.cs**에 다음 메서드를 추가 합니다.

```csharp
public override bool OnOptionsItemSelected(IMenuItem item)
{
    Toast.MakeText(this, "Action selected: " + item.TitleFormatted,
        ToastLength.Short).Show();
    return base.OnOptionsItemSelected(item);
}
```

사용자가 메뉴 항목을 탭 하면 Android는 `OnOptionsItemSelected` 메서드를 호출 하 고 선택한 메뉴 항목을 전달 합니다. 이 예제에서 구현은 메뉴 항목을 표시 하는 알림 메시지를 표시 하기만 합니다. 

`ToolbarFun`를 빌드하고 실행 하 여 도구 모음에 새 메뉴 항목을 표시 합니다. 이제이 스크린샷에 표시 된 것과 같은 세 개의 메뉴 아이콘이 `Toolbar` 표시 됩니다. 

[편집, 저장 및 오버플로 메뉴 항목의 위치를 보여 주는![다이어그램](replacing-the-action-bar-images/04-menu-items-sml.png)](replacing-the-action-bar-images/04-menu-items.png#lightbox)

사용자가 **편집** 메뉴 항목을 누르면 `OnOptionsItemSelected` 메서드가 호출 되었음을 나타내는 알림이 표시 됩니다. 

[항목 편집을 탭 할 때 표시 되는 알림 스크린샷![](replacing-the-action-bar-images/05-toast-displayed-sml.png)](replacing-the-action-bar-images/05-toast-displayed.png#lightbox)

사용자가 오버플로 메뉴를 누르면 **기본 설정** 메뉴 항목이 표시 됩니다. 일반적으로 일반적이 지 않은 작업은 오버플로 메뉴에 배치 해야 합니다 .이 예에서는 **편집** 및 **저장**에 자주 사용 되지 않기 때문에 **기본 설정** 에 대 한 오버플로 메뉴를 사용 &ndash; 합니다. 

[오버플로 메뉴에 표시 되는 기본 설정 메뉴 항목의![스크린샷](replacing-the-action-bar-images/06-preferences-sml.png)](replacing-the-action-bar-images/06-preferences.png#lightbox)

Android 메뉴에 대 한 자세한 내용은 Android Developer [menus](https://developer.android.com/guide/topics/ui/menus.html) 항목을 참조 하세요. 

## <a name="troubleshooting"></a>문제 해결

다음 팁은 작업 모음을 도구 모음으로 대체 하는 동안 발생할 수 있는 문제를 디버깅 하는 데 도움이 됩니다.

### <a name="activity-already-has-an-action-bar"></a>활동에 작업 모음 이미 있습니다.

[사용자 지정 테마 적용](#apply-the-custom-theme)에 설명 된 대로 사용자 지정 테마를 사용 하도록 앱이 적절히 구성 되지 않은 경우 앱을 실행 하는 동안 다음 예외가 발생할 수 있습니다.

![사용자 지정 테마를 사용 하지 않을 때 발생할 수 있는 오류](replacing-the-action-bar-images/03-theme-not-defined.png)

또한 다음과 같은 오류 메시지가 생성 될 수 있습니다. _IllegalStateException:이 작업은 창 décor에서 제공 하는 작업 모음을 이미 포함_ 하 고 있습니다. 

이 오류를 해결 하려면 앞서 [사용자 지정 테마 적용](#apply-the-custom-theme)의 설명에 따라 사용자 지정 테마에 대 한 `android:theme` 특성이 `<application>` ( **Properties/AndroidManifest .xml**)에 추가 되었는지 확인 합니다. 또한 `Toolbar` 레이아웃이 나 사용자 지정 테마가 제대로 구성 되지 않은 경우이 오류가 발생할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [롤리팝 도구 모음 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat 도구 모음 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
