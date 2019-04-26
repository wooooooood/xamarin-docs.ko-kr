---
title: 도구 모음 호환성
ms.prod: xamarin
ms.assetid: A0798CA1-2C7D-43B6-9E91-4435CC7B6683
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: 12c19cf1024b78e8be30b7c9f2652019e9854375
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61300359"
---
# <a name="toolbar-compatibility"></a>도구 모음 호환성


## <a name="overview"></a>개요

이 섹션에서는 사용 하는 방법에 설명 `Toolbar` Android 5.0 Lollipop 이전 버전의 Android입니다. 앱이 Android 5.0 이전 버전의 Android 지원 하지 않는 경우에이 섹션을 건너뛸 수 있습니다. 

때문에 `Toolbar` 일부인 Android v7 지원 라이브러리의 Android 2.1 (API 수준 7) 실행 하는 장치에서 사용할 수 있습니다 이상. 그러나 합니다 [Android 지원 라이브러리 v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet을 설치 해야 하며 코드를 사용 하도록 수정 합니다 `Toolbar` 이 라이브러리에서 제공 하는 구현. 이 섹션에서는이 NuGet을 설치 하 고 수정 하는 방법을 설명 합니다 **ToolbarFun** 에서 앱 [두 번째 도구 모음 추가](~/android/user-interface/controls/tool-bar/adding-a-second-toolbar.md) Android 5.0 Lollipop 이전의 버전에서 실행 되도록 합니다.

다음 도구 모음의 AppCompat 버전을 사용 하도록 앱을 수정 하려면 

1.  앱에 대 한 최소 및 대상 Android 버전을 설정 합니다.

2.  AppCompat NuGet 패키지를 설치 합니다.

3.  기본 제공 Android 테마 대신는 AppCompat 테마를 사용 합니다.

4.  수정할 `MainActivity` 있도록 것 하위 클래스 `AppCompatActivity` 대신 `Activity`합니다. 

이러한 각 단계는 다음 섹션에서 자세히 설명 합니다.



## <a name="set-the-minimum-and-target-android-version"></a>최소 및 대상 Android 버전 설정

API 수준 21 크거나 앱의 대상 프레임 워크를 설정 해야 하거나 앱이 올바르게 배포 되지 않습니다. 와 같은 오류가 **리소스 식별자가 없는 'tileModeX' 특성에 대 한 'android' 패키지 있습니다** 표시는이 앱을 배포 하는 동안은 대상 프레임 워크 설정 되어 있지 않으므로 **Android 5.0 (API 수준 21-Lollipop)**  큽니다. 

API 수준 21 수준의 이상 대상 프레임 워크를 설정 하 고 Android API 수준 프로젝트 설정을 지원 하기 위해 앱에는 최소 Android 버전으로 설정 합니다. Android API 수준을 설정 하는 방법에 대 한 자세한 내용은 참조 하십시오 [Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)합니다. 에 `ToolbarFun` 예제에서는 최소 Android 버전 KitKat (API 수준 4.4)로 설정 됩니다. 


## <a name="install-the-appcompat-nuget-package"></a>AppCompat NuGet 패키지를 설치 합니다.

다음을 추가 합니다 [Android 지원 라이브러리 v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) 프로젝트에 패키지 합니다. Visual Studio에서 마우스 오른쪽 단추로 클릭 **참조가** 선택한 **NuGet 패키지 관리...** . 클릭 **찾아보기** 검색 **Android 지원 라이브러리 v7 AppCompat**합니다. 선택 **Xamarin.Android.Support.v7.AppCompat** 누릅니다 **설치**: 

[![NuGet 패키지 관리에서 선택한 V7 Appcompat 스크린 샷 패키지](toolbar-compatibility-images/01-appcompat-nuget-sml.png)](toolbar-compatibility-images/01-appcompat-nuget.png#lightbox)

이 NuGet을 설치 하면 몇 가지 다른 NuGet 패키지도 설치 됩니다 아직 없는 경우 (같은 **Xamarin.Android.Support.Animated.Vector.Drawable**하십시오 **Xamarin.Android.Support.v4**, 및 **Xamarin.Android.Support.Vector.Drawable**). NuGet 패키지를 설치 하는 방법에 대 한 자세한 내용은 참조 하세요. [연습: 프로젝트에서 NuGet을 포함 하 여](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)입니다. 


## <a name="use-an-appcompat-theme-and-toolbar"></a>AppCompat 테마 및 도구 모음 사용

AppCompat 라이브러리는 여러 함께 `Theme.AppCompat` AppCompat 라이브러리에서 지 원하는 Android의 모든 버전에서 사용할 수 있는 테마입니다. `ToolbarFun` 예제에서는 앱 테마에서 파생 된 `Theme.Material.Light.DarkActionBar`, 롤리팝 보다 이전 Android 버전에서 사용할 수 없는 합니다. 따라서 `ToolbarFun` 이 테마는 AppCompat 대응 하는 데 적용 해야 `Theme.AppCompat.Light.DarkActionBar`합니다. 또한 때문 `Toolbar` 는 버전의 Android에서 사용할 수 없는 Lollipop 이전의 사용 해야 합니다 AppCompat 버전 `Toolbar`합니다. 따라서 레이아웃을 사용 해야 합니다 `android.support.v7.widget.Toolbar` 대신 `Toolbar`합니다. 


### <a name="update-layouts"></a>레이아웃 업데이트

편집할 **Resources/layout/Main.axml** 바꾸고는 `Toolbar` 다음 xml 요소: 

```xml
<android.support.v7.widget.Toolbar
    android:id="@+id/edit_toolbar"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorAccent"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

편집할 **Resources/layout/toolbar.xml** 다음 XML을 사용 하 여 해당 내용을 바꿉니다. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"/>
```

유의 합니다 `?attr` 값은 더 이상 사용 하 여 앞 `android:` (이전에 설명한 대로 `?` 표기법은 현재 테마의 리소스를 참조). 경우 `?android:attr` 여전히 사용 된 특성 값을 AppCompat 라이브러리 대신 실행 중인 현재 플랫폼에서 Android는 여기에서 참조 합니다. 이 예에서는 사용 하므로 합니다 `actionBarSize` AppCompat 라이브러리를 통해 정의 `android:` 접두사 삭제 됩니다. 마찬가지로 `@android:style` 으로 변경 됩니다 `@style` 있도록를 `android:theme` 특성이 AppCompat 라이브러리의 테마에 설정 된 &ndash; 는 `ThemeOverlay.AppCompat.Dark.ActionBar` 테마 여기서는 대신 `ThemeOverlay.Material.Dark.ActionBar`합니다. 


### <a name="update-the-style"></a>스타일 업데이트

편집할 **Resources/values/styles.xml** 다음 XML을 사용 하 여 해당 내용을 바꿉니다. 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
  <style name="MyTheme" parent="MyTheme.Base"> </style>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="windowNoTitle">true</item>
    <item name="windowActionBar">false</item>
    <item name="colorPrimary">#5A8622</item>
    <item name="colorAccent">#A88F2D</item>
  </style>
</resources>
```

항목 이름 및이 예제의 부모 테마를 사용 하 여 접두사가 더 이상 `android:` AppCompat 라이브러리를 사용 하기 때문입니다. AppCompat 버전의 부모 테마를 변경 하는 또한 `Light.DarkActionBar`합니다. 



### <a name="update-menus"></a>업데이트 메뉴

AppCompat 라이브러리를 지원 하기 위해 이전 버전의 Android의 특성을 미러링 하는 사용자 지정 특성을 사용 하 여 `android:` 네임 스페이스입니다. 하지만 일부 특성 (같은 `showAsAction` 에서 사용 되는 특성을 `<menu>` 태그) 이전 장치에서 Android 프레임 워크에 존재 하지 않습니다 &ndash; `showAsAction` Android API 11에 도입 된 이지만 Android API 7에서 사용할 수 없습니다. 따라서 사용자 지정 네임 스페이스를 모든 지원 라이브러리에서 정의 된 특성을 접두사로 사용 되어야 합니다. 메뉴 리소스 파일에서 네임 스페이스는 다음과 같이 호출 됩니다. `local` 접두사에 대해 정의 된 `showAsAction` 특성입니다. 

편집할 **Resources/menu/top_menus.xml** 다음 XML을 사용 하 여 해당 내용을 바꿉니다.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:local="http://schemas.android.com/apk/res-auto">
  <item
       android:id="@+id/menu_edit"
       android:icon="@mipmap/ic_action_content_create"
       local:showAsAction="ifRoom"
       android:title="Edit" />
  <item
       android:id="@+id/menu_save"
       android:icon="@mipmap/ic_action_content_save"
       local:showAsAction="ifRoom"
       android:title="Save" />
  <item
       android:id="@+id/menu_preferences"
       local:showAsAction="never"
       android:title="Preferences" />
</menu>
```

`local` 네임 스페이스는 줄이 추가 됩니다.

```xml
xmlns:local="http://schemas.android.com/apk/res-auto">
```

합니다 `showAsAction` 특성은이 붙은 `local:` 네임 스페이스 대신 `android:` 

```csharp
local:showAsAction="ifRoom"
```

마찬가지로, 편집 **Resources/menu/edit_menus.xml** 다음 XML을 사용 하 여 해당 내용을 바꿉니다.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:local="http://schemas.android.com/apk/res-auto">
  <item
       android:id="@+id/menu_cut"
       android:icon="@mipmap/ic_menu_cut_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Cut" />
  <item
       android:id="@+id/menu_copy"
       android:icon="@mipmap/ic_menu_copy_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Copy" />
  <item
       android:id="@+id/menu_paste"
       android:icon="@mipmap/ic_menu_paste_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Paste" />
</menu>
```

이 네임 스페이스 스위치 않습니다에 대 한 지원을 제공 하는 방법의 `showAsAction` Android API 수준 11 이전 버전에서 특성이? 사용자 지정 특성 `showAsAction` AppCompat NuGet 설치 될 때 앱에 포함 된 모든 값을 사용할 수 있습니다. 


## <a name="subclass-appcompatactivity"></a>서브 클래스 AppCompatActivity

변환의 마지막 단계는 수정 하는 것 `MainActivity` 의 서브 클래스 되도록 `AppCompactActivity`합니다. 편집할 **MainActivity.cs** 추가한 다음 `using` 문: 

```csharp
using Android.Support.V7.App;
using Toolbar = Android.Support.V7.Widget.Toolbar;
```

이 선언 `Toolbar` AppCompat 버전의 `Toolbar`합니다. 그런 다음 클래스 정의 변경할 `MainActivity`: 

```csharp
public class MainActivity : AppCompatActivity
```

AppCompat 버전의 작업 모음을 설정 하려면 `Toolbar`에 대 한 호출을 대체할 `SetActionBar` 사용 하 여 `SetSupportActionBar`입니다. 이 예제에서는 제목도 변경 됩니다 나타내는 AppCompat 버전 `Toolbar` 사용량:

```csharp
SetSupportActionBar (toolbar);
SupportActionBar.Title = "My AppCompat Toolbar";
```

마지막으로, 사전 롤리팝 값 (예: API 19) 지원 되는 데는 최소 Android 수준을 변경 합니다. 

앱을 빌드하고 사전 롤리팝 장치 또는 Android 에뮬레이터에서 실행 합니다. 다음 스크린샷은 AppCompat 버전이 **ToolbarFun** KitKat (API 19)를 실행 하는 Nexus 4: 

[![두 도구 모음에 표시 됩니다 KitKat 장치에서 실행 되는 앱의 전체 스크린 샷](toolbar-compatibility-images/02-running-on-kitkat-sml.png)](toolbar-compatibility-images/02-running-on-kitkat.png#lightbox)

AppCompat 라이브러리를 사용 하는 경우 테마 않아도 호스팅해서 Android 버전에 따라 &ndash; AppCompat 라이브러리를 사용 하면 모든 지원 되는 Android 버전에서 일관 된 사용자 환경을 제공할 수 있습니다. 




## <a name="related-links"></a>관련 링크

- [롤리팝 도구 모음 (샘플)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat 도구 모음 (샘플)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
