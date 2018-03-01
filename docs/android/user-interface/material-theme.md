---
title: "자재 테마"
description: "테마 하는 방법 자료 테마와 Xamarin.Android 앱"
ms.topic: article
ms.prod: xamarin
ms.assetid: DC4CDBD0-3DF9-4B7E-B876-29128985E2A7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: ba73e03d6bdeea64918e0232b2188bf8e3b65084
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="material-theme"></a>자재 테마

<a name="overview" />

## <a name="overview"></a>개요

*자재 테마* 뷰와 Android 5.0 (롤리팝)로 시작 하는 활동의 디자인을 결정 하는 사용자 인터페이스 스타일입니다. 자재 테마는 물론 응용 프로그램에서 시스템 UI에서 사용 하도록 Android 5.0에 포함 됩니다. 사용자 설정 메뉴에서 동적으로 선택할 수 있는 시스템 수준의 모양 옵션의 의미에서 "테마" 자재 테마가 아닙니다. 대신, 자료 테마 앱의 모양과 느낌을 사용자 지정 하는 데 사용할 수 있는 관련된 기본 제공 기본 스타일의 집합으로 생각할 수 있습니다.

Android에서는 세 가지 자료 테마 특성을 제공합니다.

-  `Theme.Material` &ndash; 자료 테마;의 어두운 버전 이 Android 5.0의 기본 버전입니다.

-  `Theme.Material.Light` &ndash; 자료 테마의 라이브 버전입니다.

-  `Theme.Material.Light.DarkActionBar` &ndash; 어두운 작업 모음 하면서도 자료 테마의 라이브 버전입니다.

이러한 자료 테마 특성의 예는 여기에 표시 됩니다.

[![어두운 테마, 밝은 테마 및 어두운 작업 모음 테마 예제 스크린 샷](material-theme-images/three-flavors-sml.png)](material-theme-images/three-flavors.png)

재정의 일부 또는 모든 색 특성 직접 테마를 만들 자료 테마에서 파생할 수 있습니다. 파생 되는 테마를 만들 수는 예를 들어 `Theme.Material.Light`, 브랜드 색과 일치 하도록 응용 프로그램 표시줄 색을 재정의 하지만 합니다. 개별 뷰만 스타일도 수 있습니다. 에 대 한 스타일을 만들 수는 예를 들어 [CardView](~/android/user-interface/controls/card-view.md) 모서리가 둥근 더 하 고 어두운 배경색을 사용 합니다.

단일 테마를 사용 하 여 전체 응용 프로그램 또는 응용 프로그램의 다른 화면 (활동)에 대 한 다양 한 테마를 사용할 수 있습니다. 위의 스크린샷에서 예를 들어 단일 응용 프로그램을 사용 하 여 각 활동에 대 한 다양 한 테마 기본 제공 색상표를 보여 줍니다. 라디오 단추 각기 다른 활동으로 앱을 전환 하 고 결과적으로, 다양 한 테마를 표시 합니다.

자료 테마 이상 Android 5.0 에서만 지원 되므로 사용할 수 없습니다 (또는 자료 테마에서 파생 된 사용자 지정 테마) 테마를 앱 Android의 이전 버전에서 실행 합니다. 그러나 Android 5.0 장치에서 자료 테마를 사용 하 고 적절 하 게 대체 이전 테마를 이전 버전의 Android에서 실행 될 경우 앱을 구성할 수 있습니다 (참조는 [호환성](#compatibility) 대 한 자세한 내용은이 문서의 섹션).

<a name="requirements" />

## <a name="requirements"></a>요구 사항

다음은 Xamarin 기반 앱에서 새 Android 5.0 자료 테마 기능을 사용 하려면 필요 합니다.

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 이상을 설치 하 고 Mac.에 대 한 Visual Studio 또는 Visual Studio를 사용 하 여 구성 해야 

-  **Android SDK** &ndash; Android 5.0 (API 21) 나중에 설치 되어 있어야 Android SDK Manager를 통해 합니다.

-  **Java JDK 1.8** &ndash; JDK 1.7 구체적으로 대상으로 하는 API 수준 23 및 이전 버전의 경우 사용할 수 있습니다. 사용할 수 있는 JDK 1.8은 [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)합니다.

Android 5.0 응용 프로그램 프로젝트를 구성 하는 방법을 알아보려면 참조 [설정을 Android 5.0 프로젝트](~/android/platform/lollipop.md)합니다.

<a name="builtinthemes" />

## <a name="using-the-built-in-themes"></a>기본 테마를 사용 하 여

자료 테마를 사용 하는 가장 쉬운 방법은 사용자 지정 하지 않고 기본 테마를 사용 하도록 앱을 구성 하는 것입니다. 테마를 명시적으로 구성 하지 않으려면 응용 프로그램에서 기본적으로를 `Theme.Material` (어두운 테마). 앱의 활동이 하나만 있으면 응용 프로그램 수준에서 테마를 구성할 수 있습니다. 응용 프로그램에 여러 활동을 동일한 테마를 사용 하 여 모든 활동 간에 또는 다른 활동에 다양 한 테마를 할당할 수 있도록 응용 프로그램 수준에서 테마를 구성할 수 있습니다. 다음 섹션에서는 응용 프로그램 수준 및 작업 수준에서 테마를 구성 하는 방법에 설명 합니다.

<a name="themeanapp" />

### <a name="theming-an-application"></a>응용 프로그램 테마 설정

자료 테마 버전을 사용 하는 전체 응용 프로그램을 구성 하려면 설정는 `android:theme` 에서 응용 프로그램 노드의 특성 **AndroidManifest.xml** 를 다음 중 하나:

-  `@android:style/Theme.Material` &ndash; 어두운 테마입니다.

-  `@android:style/Theme.Material.Light` &ndash; 밝은 테마입니다.

-  `@android:style/Theme.Material.Light.DarkActionBar` &ndash; 밝은 테마와 어두운 작업 모음입니다.

다음 예제에서는 응용 프로그램을 구성 *MyApp* 밝은 테마를 사용 하려면:

```xml
<application android:label="MyApp" 
             android:theme="@android:style/Theme.Material.Light">
</application>
```

또는 응용 프로그램을 설정할 수 `Theme` 특성 **AssemblyInfo.cs** (또는 **Properties.cs**). 예:

```C#
[assembly: Application(Theme="@android:style/Theme.Material.Light")]
```

응용 프로그램 테마로 설정 되 면 `@android:style/Theme.Material.Light`, 모든 활동에 *MyApp* 사용 하 여 표시 `Theme.Material.Light`합니다.

<a name="activitytheme" />

### <a name="theming-an-activity"></a>테마 설정 활동

활동 테마를 하려면 추가 `Theme` 설정을 `[Activity]` 활동 선언 위에 특성을 할당 `Theme` 사용 하려는 자료 테마 버전을 합니다. 다음 예제에서는 테마와 활동 `Theme.Material.Light`:

```C#
[Activity(Theme = "@android:style/Theme.Material.Light",
          Label = "MyApp", MainLauncher = true, Icon = "@drawable/icon")]  
```

이 응용 프로그램에서 다른 활동에는 기본값이 사용 됩니다 `Theme.Material` 어두운 색 구성표 (또는 응용 프로그램 테마 설정을 구성한 경우).

<a name="customtheme" />

## <a name="using-custom-themes"></a>사용자 지정 테마를 사용 하 여

브랜드를 사용 하 여 앱 스타일 사용자 지정 테마를 만들어서 여 브랜드를 향상 시킬 수 있습니다&rsquo;의색입니다. 사용자 지정 테마를 만들려면 정의 해야 기본 제공 자료 테마 버전에서 파생 된 새 스타일을 변경 하려면 색 특성을 재정의 합니다. 파생 되는 사용자 지정 테마를 정의할 수는 예를 들어 `Theme.Material.Light.DarkActionBar` 흰색이 아닌 베이를 화면 배경 색상을 변경 합니다.

자재 테마 사용자 지정에 대 한 다음과 같은 레이아웃 특성을 노출합니다.

-  `colorPrimary` &ndash; 앱 바의 색입니다.

-  `colorPrimaryDark` &ndash; 상태 표시줄 및 상황에 맞는 앱 표시줄의 색 이 일반적으로 어두운 버전의 `colorPrimary`합니다.

-  `colorAccent` &ndash; 확인란, 라디오 단추 및 편집 텍스트 상자와 같은 UI 컨트롤의 색입니다.

-  `windowBackground` &ndash; 색 화면 배경입니다.

-  `textColorPrimary` &ndash; 앱 바에서 UI 텍스트의 색입니다.

-  `statusBarColor` &ndash; 상태 표시줄의 색입니다.

-  `navigationBarColor` &ndash; 탐색 모음의 색입니다.

이러한 화면 영역 다음 다이어그램에 레이블이 지정 됩니다.

[ ![특성 및 해당 연관 된 화면 영역 다이어그램](material-theme-images/screen-attributes-sml.png)](material-theme-images/screen-attributes.png)

기본적으로 `statusBarColor` 의 값으로 설정 되어 `colorPrimaryDark`합니다. 설정할 수 있습니다 `statusBarColor` 의 단색를 설정할 수 있습니다 또는 `@android:color/transparent` 투명 하 게 하려면 상태 표시줄입니다. 탐색 모음 만들 수도 있습니다 투명 하 게 설정 하 여 `navigationBarColor` 를 `@android:color/transparent`합니다.

<a name="customapptheme" />

### <a name="creating-a-custom-app-theme"></a>사용자 지정 응용 프로그램 테마 만들기

만들고에 파일을 수정 하 여 사용자 지정 앱 테마를 만들 수는 **리소스** 응용 프로그램 프로젝트의 폴더입니다. 사용자 지정 테마를 사용 하 여 앱을 스타일을 지정 하려면 다음 단계를 사용 합니다.

-   만들기는 **colors.xml** 파일 **리소스/값** &mdash; 이 파일을 사용 하 여 사용자 지정 테마 색을 정의 합니다. 다음 코드를 붙여 넣을 수는 예를 들어 **colors.xml** 시작할 수 있도록 하려면:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <color name="my_blue">#3498DB</color>
    <color name="my_green">#77D065</color>
    <color name="my_purple">#B455B6</color>
    <color name="my_gray">#738182</color>
</resources>
```

-   이 예제에서는 파일 이름 및 사용자 지정 테마에 사용할 수 있는 색 리소스에 대 한 색상 코드를 정의할 수를 수정 합니다.

-   만들기는 **리소스/값-v21** 폴더입니다. 이 폴더에 만듭니다는 **styles.xml** 파일:

    [ ![Styles.xml 리소스/값-21.xml 폴더에서의 위치](material-theme-images/values-v21-sml.png)](material-theme-images/values-v21.png)

    **리소스/값-v21** Android 5.0에 관련 된 &ndash; 이전 버전의 Android이이 폴더의 파일을 읽지 것입니다.

-   추가 `resources` 노드를 **styles.xml** 정의 `style` 사용자 지정 테마의 이름으로 노드. 예를 들어, 다음은 한 **styles.xml** 정의 하는 파일 *MyCustomTheme* (기본 제공에서 파생 된 `Theme.Material.Light` 테마 스타일):

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Customizations go here -->
    </style>
</resources>
```

-   이 시점에서 응용 프로그램을 사용 하 여 *MyCustomTheme* 재고 ½ ֳ µ `Theme.Material.Light` 테마 사용자 지정 하지 않고:

    [ ![사용자 지정 하기 전에 사용자 지정 테마 모양](material-theme-images/custom-theme-before-sml.png)](material-theme-images/custom-theme-before.png)

-   색 사용자 지정을 추가 **styles.xml** 레이아웃 특성을 변경 하려면 색 정의 합니다. 예를 들어, 응용 프로그램 표시줄 색을 변경 하려면 `my_blue` 에 UI 컨트롤의 색을 변경 `my_purple`를 추가 하려면 재정의 색 **styles.xml** 에 구성 된 색 리소스를 참조 하는 **colors.xml**:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Override the app bar color -->
        <item name="android:colorPrimary">@color/my_blue</item>
        <!-- Override the color of UI controls -->
        <item name="android:colorAccent">@color/my_purple</item>
    </style>
</resources>
```

위치에 이러한 변경 내용으로는 응용 프로그램을 사용 하 *MyCustomTheme* 에 응용 프로그램 표시줄 색 ½ ֳ µ `my_blue` 및 UI 컨트롤에 `my_purple`, 사용 하지만 `Theme.Material.Light` 다른 색 구성표:

[ ![사용자 지정 테마 모양을 사용자 지정 후](material-theme-images/custom-theme-after-sml.png)](material-theme-images/custom-theme-after.png)

이 예제에서는 *MyCustomTheme* 에서 색을 빌리 기 `Theme.Material.Light` 배경에 대 한 색, 상태 표시줄 및 텍스트 색, 하지만의 색을 변경 하려면 앱 바 `my_blue` 라디오단추의색을설정하고`my_purple`.

<a name="customview" />

### <a name="creating-a-custom-view-style"></a>사용자 지정 보기 스타일 만들기

Android 5.0를 하면 개별 뷰 스타일 지정 수 있습니다. 만든 후 **colors.xml** 및 **styles.xml** (에 설명 된 대로 이전 섹션), 뷰 스타일을 추가할 수 있습니다 **styles.xml**합니다.
개별 보기를 스타일을 지정 하려면 다음 단계를 사용 합니다.

-   편집 **Resources/values-v21/styles.xml** 추가 `style` 사용자 지정 보기 스타일의 이름으로 노드. 이 보기에 대 한 사용자 지정 색 특성 설정 `style` 노드. 예를 들어, 사용자 지정 만들어 [CardView](~/android/user-interface/controls/card-view.md) 스타일을 사용 하 여 모퉁이가 둥근 더 `my_blue` 카드 배경색을 추가 하는 `style` 노드를 **styles.xml** (의 내`resources`노드) 및 배경 색 및 모퉁이 radius 구성:

```xml
<!-- Theme an individual view: -->
<style name="CardView.MyBlue">

    <!-- Change the background color to Xamarin blue: -->
    <item name="cardBackgroundColor">@color/my_blue</item>

    <!-- Make the corners very round: -->
    <item name="cardCornerRadius">18dp</item>
</style>
```

-   레이아웃에 설정 된 `style` 이전 단계에서 선택한 스타일 사용자 이름과 일치 하도록 해당 보기에 대 한 특성입니다. 예:

```xml
<android.support.v7.widget.CardView
    style="@style/CardView.MyBlue"
    android:layout_width="200dp"
    android:layout_height="100dp"
    android:layout_gravity="center_horizontal">
```

기본 예제를 제공 하는 다음 스크린 샷에서 `CardView` (표시 왼쪽)에 비해 대로 `CardView` 를 사용자 지정 스타일 `CardView.MyBlue` 테마 (오른쪽에 표시):

[ ![CardView 기본 및 사용자 지정 CardView 예제](material-theme-images/custom-cardview-sml.png)](material-theme-images/custom-cardview.png)

이 예제에서는 사용자 지정 `CardView` 배경색으로 표시 `my_blue` 및 18dp 모서리 반지름입니다.

<a name="compatibility" />

## <a name="compatibility"></a>호환성

Android 5.0에서 자료 테마를 사용 하지만 하향 호환 스타일 이전 Android 버전에는 자동으로 전환 되도록 응용 프로그램 스타일 지정, 다음 단계를 사용 합니다.

-   사용자 지정 테마를 정의 **Resources/values-v21/styles.xml** 자료 테마 스타일에서 파생 된 합니다. 예:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   사용자 지정 테마를 정의 **Resources/values/styles.xml** 하는 테마에는 이전 버전에서 파생 되지만 위와 동일한 테마 이름을 사용 합니다. 예:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Holo.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   **AndroidManifest.xml**, 사용자 지정 테마 이름으로 앱을 구성 합니다. 
    예:

```xml
<application android:label="MyApp" 
             android:theme="@style/MyCustomTheme">
</application>
```

-   또는 사용자 지정 테마를 사용 하 여 특정 작업을 스타일을 지정할 수 있습니다.

```C#
[Activity(Label = "MyActivity", Theme = "@style/MyCustomTheme")]
```

테마에 정의 된 색을 사용 하는 경우는 **colors.xml** 파일에서이 파일에 배치 해야 **리소스/값** (대신 **리소스/값-v21**) 되도록 두 버전의 사용자 지정 테마 색 정의 액세스할 수 있습니다.

에 지정 된 테마 정의 사용 합니다 Android 5.0 장치에서 앱 실행 되 면 **Resources/values-v21/styles.xml**합니다. 이 앱이 오래 된 Android 장치에서 실행 하는 경우 자동으로 대체 됩니다에 지정 된 테마 정의를 **Resources/values/styles.xml**합니다.

테마 호환성 이전 Android 버전에 대 한 자세한 내용은 참조 [대체 리소스](~/android/app-fundamentals/resources-in-android/alternate-resources.md)합니다.

## <a name="summary"></a>요약

이 문서에서는 Android 5.0 (롤리팝)에 포함 된 새 자료 테마 사용자 인터페이스 스타일을 도입 되었습니다. 응용 프로그램 스타일을 지정 하는 데 사용할 수 있는 세 가지 기본 제공 자료 테마 특성을 설명 하 고 응용 프로그램 브랜딩에 대해 사용자 지정 테마를 만드는 방법을 설명 하기 개별 보기 테마 방법의 예로 제공 합니다. 마지막으로,이 문서의 이전 버전의 Android와 아래쪽의 호환성을 유지 하면서 응용 프로그램에서 자료 테마를 사용 하는 방법을 설명 합니다.



## <a name="related-links"></a>관련 링크

- [ThemeSwitcher (샘플)](https://developer.xamarin.com/samples/monodroid/android5.0/ThemeSwitcher)
- [롤리팝 소개](~/android/platform/lollipop.md)
- [CardView](~/android/user-interface/controls/card-view.md)
- [대체 리소스](~/android/app-fundamentals/resources-in-android/alternate-resources.md)
- [Android L 개발자 미리 보기](http://developer.android.com/preview/index.html)
- [자재 디자인](http://developer.android.com/preview/material/index.html)
- [자재 디자인 원칙](http://static.googleusercontent.com/media/www.google.com/en/us/design/material-design.pdf)
- [호환성을 유지 관리](http://developer.android.com/preview/material/compatibility.html)
