---
title: AppCompat 및 재질 디자인 추가
description: 이 문서는 AppCompat 및 재질 디자인을 사용 하도록 기존 Xamarin.Forms Android 앱을 변환 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 045FBCDF-4D45-48BB-9911-BD3938C87D58
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2017
ms.openlocfilehash: cade72aaad60c30993f6b11e98704addd218ffae
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667492"
---
# <a name="adding-appcompat-and-material-design"></a>AppCompat 및 재질 디자인 추가

_AppCompat 및 재질 디자인을 사용 하도록 기존 Xamarin.Forms Android 앱을 변환 하려면 다음이 단계를 수행 합니다._

<!-- source https://gist.github.com/jassmith/a3b2a543f99126782936
https://blog.xamarin.com/material-design-for-your-xamarin-forms-android-apps/ -->

## <a name="overview"></a>개요

이러한 지침은 AppCompat 라이브러리를 사용 하 여 Xamarin.Forms 앱의 Android 버전에서 재질 디자인을 사용 하도록 설정 하 여 기존 Xamarin.Forms Android 응용 프로그램을 업데이트 하는 방법에 설명 합니다.

### <a name="1-update-xamarinforms"></a>1. Xamarin.Forms를 업데이트 합니다.

솔루션은 2.0 이상 Xamarin.Forms를 사용 하 여 확인 합니다. 필요한 경우에 2.0 Xamarin.Forms Nuget 패키지를 업데이트 합니다.

### <a name="2-check-android-version"></a>2. Android 버전 확인

Android 프로젝트의 대상 프레임 워크가 Android 6.0(marshmallow)을 확인 합니다. 확인 합니다 **Android 프로젝트 > 옵션 > 빌드 > 일반** 정정 프레임 워크 설정을 선택 합니다.

 ![](appcompat-images/target-android-6-sml.png "Android 일반 빌드 구성")

### <a name="3-add-new-themes-to-support-material-design"></a>3. 재질 디자인을 지원 하기 위해 새 테마 추가

Android 프로젝트에 다음 세 개의 파일이 만들고 아래 내용에 붙여 넣습니다. Google에서 제공 된 [스타일 가이드](http://www.google.com/design/spec/style/color.html#color-color-palette) 및 [색 색상표 생성기](http://www.materialpalette.com/) 지정 된 대체 색 구성표를 선택할 수 있도록 합니다.

**Resources/values/colors.xml**

```xml
<resources>
  <color name="primary">#2196F3</color>
  <color name="primaryDark">#1976D2</color>
  <color name="accent">#FFC107</color>
  <color name="window_background">#F5F5F5</color>
</resources>
```

**Resources/values/style.xml**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
  </style>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.NoActionBar">
    <item name="colorPrimary">@color/primary</item>
    <item name="colorPrimaryDark">@color/primaryDark</item>
    <item name="colorAccent">@color/accent</item>
    <item name="android:windowBackground">@color/window_background</item>
    <item name="windowActionModeOverlay">true</item>
  </style>
</resources>
```

에 포함 해야 하는 스타일을 추가 합니다 **값 v21** Android Lollipop에서를 실행 하는 경우 특정 속성을 적용 하는 폴더입니다.

**Resources/values-v21/style.xml**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
    <!--If you are using MasterDetailPage you will want to set these, else you can leave them out-->
    <!--<item name="android:windowDrawsSystemBarBackgrounds">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>-->
  </style>
</resources>
```

### <a name="4-update-androidmanifestxml"></a>4. Update AndroidManifest.xml

이 새 테마 되도록 정보를 사용, 테마에는 **AndroidManifest** 추가 하 여 파일 `android:theme="@style/MyTheme"` (동일 하다는 XML의 나머지 부분을 둠).

**Properties/AndroidManifest.xml**

```xml
...
<application android:label="AppName" android:icon="@drawable/icon"
  android:theme="@style/MyTheme">
...
```

### <a name="5-provide-toolbar-and-tab-layouts"></a>5. 도구 모음 및 탭 레이아웃을 제공 합니다.

만들기 **Tabbar.axml** 및 **Toolbar.axml** 파일을 **리소스/레이아웃** 디렉터리와 내용도에서 아래에 붙여 넣습니다.

**Resources/layout/Tabbar.axml**

```xml
<android.support.design.widget.TabLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/sliding_tabs"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:tabIndicatorColor="@android:color/white"
    app:tabGravity="fill"
    app:tabMode="fixed" />
```

탭의 무게를 비롯 한 탭에 대 한 몇 가지 속성을 설정한 `fill` 및 모드를 `fixed`입니다.
이 전환 하려는 여러 탭이 있는 경우-스크롤 가능 하려면 Android를 자세히 읽습니다 [TabLayout 설명서](https://developer.android.com/reference/android/support/design/widget/TabLayout.html) 에 대해 자세히 알아보세요.

**Resources/layout/Toolbar.axml**

```xml
<android.support.v7.widget.Toolbar
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
    app:layout_scrollFlags="scroll|enterAlways" />
```

이러한 파일의 응용 프로그램에 대 한 달라질 수 있는 도구 모음에 대 한 특정 테마를 만들겠습니다.
참조를 [Hello 도구 모음](https://blog.xamarin.com/android-tips-hello-toolbar-goodbye-action-bar/) 블로그 게시물에 자세히 알아보세요.


### <a name="6-update-the-mainactivity"></a>6. 업데이트는 `MainActivity`

기존 Xamarin.Forms 앱에는 **MainActivity.cs** 클래스에서 상속 됩니다 `FormsApplicationActivity`합니다. 이 사용 하 여 바꾸어야 `FormsAppCompatActivity` 새로운 기능을 사용 하도록 설정 합니다.

**MainActivity.cs**

```csharp
public class MainActivity : FormsAppCompatActivity  // was FormsApplicationActivity
```

마지막으로, "연결"의 5 단계에서 새 레이아웃을 `OnCreate` 메서드를 다음과 같이 합니다.

```csharp
protected override void OnCreate(Bundle bundle)
{
  // set the layout resources first
  FormsAppCompatActivity.ToolbarResource = Resource.Layout.Toolbar;
  FormsAppCompatActivity.TabLayoutResource = Resource.Layout.Tabbar;

  // then call base.OnCreate and the Xamarin.Forms methods
  base.OnCreate(bundle);
  Forms.Init(this, bundle);
  LoadApplication(new App());
}
```
