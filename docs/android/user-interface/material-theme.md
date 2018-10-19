---
title: 재질 테마
description: 테마 방법 자료 테마를 사용 하 여 xamarin android 앱
ms.prod: xamarin
ms.assetid: DC4CDBD0-3DF9-4B7E-B876-29128985E2A7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 4b9c39a0ced9a264f501d78142c3bdfd556593ed
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/18/2018
ms.locfileid: "30771322"
---
# <a name="material-theme"></a>재질 테마

*재질 테마* 뷰와 Android 5.0 (Lollipop)로 시작 하는 활동의 모양과 느낌을 결정 하는 사용자 인터페이스 스타일입니다. 재질 테마를 응용 프로그램 뿐만 아니라 시스템 UI 통해 사용 하도록 Android 5.0으로 빌드됩니다. 재질 테마 설정 메뉴에서 동적으로 선택할 수 있는 시스템 차원의 모양 옵션의 의미에서 "테마" 아닙니다. 대신, 재질 테마 앱의 모양과 느낌을 사용자 지정 하는 데 사용할 수 있는 관련된 기본 제공 기본 스타일의 집합으로 간주 될 수 있습니다.

Android는 세 가지 재질 테마 버전을 제공합니다.

-  `Theme.Material` &ndash; 재질 테마;의 어두운 버전 Android 5.0에서 기본 버전입니다.

-  `Theme.Material.Light` &ndash; 재질 테마의 간단한 버전입니다.

-  `Theme.Material.Light.DarkActionBar` &ndash; 재질 테마 하지만 어두운 작업 모음을 사용 하 여 간단한 버전입니다.

이러한 자료 테마 버전의 예제는 여기에 표시 됩니다.

[![어두운 테마, 밝은 테마 및 어두운 작업 모음 테마 예제 스크린샷](material-theme-images/three-flavors-sml.png)](material-theme-images/three-flavors.png#lightbox)

일부 또는 모든 색 특성을 재정의 고유한 테마를 만들 재질 테마에서 파생할 수 있습니다. 예를 들어에서 파생 되는 테마를 만들 수 있습니다 `Theme.Material.Light`, 하지만 여러분의 브랜드 색과 일치 하도록 앱 표시줄 색을 재정의 합니다. 개별 뷰; 스타일도 지정할 수 있습니다. 예를 들어 스타일을 만들 수 있습니다 [CardView](~/android/user-interface/controls/card-view.md) 모서리가 둥근 더을 음영이 짙을 수록 배경색을 사용 합니다.

단일 테마를 사용 하 여 전체 앱 또는 앱에서 다른 화면 (활동)에 대 한 다양 한 테마를 사용할 수 있습니다. 위의 스크린샷에서 예를 들어, 단일 앱을 사용 하 여 각 활동에 대 한 다양 한 테마를 기본 제공 색 구성표를 보여 줍니다. 라디오 단추 다른 작업에 앱을 전환 하 고 결과적으로, 다양 한 테마를 표시 합니다.

재질 테마 이상 Android 5.0 에서만 지원 되므로 사용할 수 없습니다 (또는 재질 테마에서 파생 된 사용자 지정 테마) 테마에 앱 이전 버전의 Android 실행 합니다. 그러나 Android 5.0 장치에서 재질 테마를 사용 하 고 정상적으로 대체 이전 테마를 이전 버전의 Android에서 실행 될 때 앱을 구성할 수 있습니다 (참조를 [호환성](#compatibility) 대 한 자세한 내용은이 문서의 섹션).


## <a name="requirements"></a>요구 사항

다음은 Xamarin 기반 앱에서 새 Android 5.0 재질 테마 기능을 사용 하려면 필요 합니다.

-  **Xamarin.Android** &ndash; 4.20 이상 Xamarin.Android를 설치 하 고 mac 용 Visual Studio 또는 Visual Studio를 사용 하 여 구성 해야 

-  **Android SDK** &ndash; Android 5.0 (API 21) 나중에 설치 해야 Android SDK Manager를 통해 또는 합니다.

-  **Java JDK 1.8** &ndash; 특별히 대상으로 하는 API 수준 23 및 이전 하려는 경우 JDK 1.7을 사용할 수 있습니다. JDK 1.8에서 제공 됩니다 [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)합니다.

Android 5.0 앱 프로젝트를 구성 하는 방법에 알아보려면 참조 [설정 하는 Android 5.0 프로젝트](~/android/platform/lollipop.md)합니다.


## <a name="using-the-built-in-themes"></a>기본 테마를 사용 하 여

재질 테마를 사용 하는 가장 쉬운 방법은 사용자 지정 하지 않고 기본 테마를 사용 하 여 앱을 구성 하는 것입니다. 앱은 기본적으로 명시적으로 테마를 구성 하지 않으려는 경우 `Theme.Material` (어두운 테마). 앱에 활동이 하나만 있으면 응용 프로그램 수준 테마를 구성할 수 있습니다. 앱이 여러 작업에 동일한 테마를 사용 하 여 모든 활동 간에 또는 다른 작업에 다양 한 테마를 할당할 수 있도록 응용 프로그램 수준 테마를 구성할 수 있습니다. 다음 섹션에서는 앱 수준 및 작업 수준에서 테마를 구성 하는 방법에 설명 합니다.


### <a name="theming-an-application"></a>응용 프로그램 테마

재질 테마 버전을 사용 하는 전체 응용 프로그램을 구성 하려면 설정 합니다 `android:theme` 에서 응용 프로그램 노드의 특성 **AndroidManifest.xml** 다음 중 하나에:

-  `@android:style/Theme.Material` &ndash; 어두운 테마입니다.

-  `@android:style/Theme.Material.Light` &ndash; 밝은 테마입니다.

-  `@android:style/Theme.Material.Light.DarkActionBar` &ndash; 어두운 작업 모음을 사용 하 여 밝은 테마입니다.

다음 예제에서는 응용 프로그램 구성 *MyApp* 밝은 테마를 사용 하려면:

```xml
<application android:label="MyApp" 
             android:theme="@android:style/Theme.Material.Light">
</application>
```

또는 응용 프로그램을 설정할 수 있습니다 `Theme` 특성 **AssemblyInfo.cs** (또는 **Properties.cs**). 예를 들어:

```C#
[assembly: Application(Theme="@android:style/Theme.Material.Light")]
```

응용 프로그램 테마 설정 된 경우 `@android:style/Theme.Material.Light`에서 모든 활동 *MyApp* 사용 하 여 표시할 `Theme.Material.Light`합니다.


### <a name="theming-an-activity"></a>테마 설정 작업

추가한 활동 테마를 `Theme` 로 설정 합니다 `[Activity]` 활동 선언 위에 특성 및 할당 `Theme` 사용 하려는 재질 테마 버전을 합니다. 다음 예제에서는 테마와 활동 `Theme.Material.Light`:

```C#
[Activity(Theme = "@android:style/Theme.Material.Light",
          Label = "MyApp", MainLauncher = true, Icon = "@drawable/icon")]  
```

이 앱의 다른 활동에는 기본값이 사용 됩니다 `Theme.Material` 어두운 색 구성표 (또는 응용 프로그램 테마 설정을 구성한 경우).

<a name="customtheme" />

## <a name="using-custom-themes"></a>사용자 지정 테마를 사용 하 여

브랜드를 사용 하 여 앱 스타일 사용자 지정 테마를 만들어 브랜드를 향상 시킬 수 있습니다&rsquo;의색입니다. 사용자 지정 테마를 만들려면 정의 기본 제공 자료 테마 버전에서 파생 되는 새 스타일을 변경 하려면 색 특성을 재정의 합니다. 예를 들어에서 파생 되는 사용자 지정 테마를 정의할 수 있습니다 `Theme.Material.Light.DarkActionBar` 흰색 대신 베이 지 화면 배경색으로 변경 합니다.

재질 테마는 사용자 지정에 대 한 다음 레이아웃 특성을 노출합니다.

-  `colorPrimary` &ndash; 앱 바의 색입니다.

-  `colorPrimaryDark` &ndash; 상태 표시줄 및 상황에 맞는 앱 표시줄의 색 이 일반적으로 어두운 버전 `colorPrimary`합니다.

-  `colorAccent` &ndash; 확인란, 라디오 단추 및 편집 텍스트 상자와 같은 UI 컨트롤의 색입니다.

-  `windowBackground` &ndash; 색 화면 배경색입니다.

-  `textColorPrimary` &ndash; 앱 바에서 UI 텍스트의 색입니다.

-  `statusBarColor` &ndash; 상태 표시줄의 색입니다.

-  `navigationBarColor` &ndash; 탐색 표시줄의 색입니다.

이러한 화면 영역이 다음 다이어그램에 레이블이 지정 됩니다.

[![특성 및 해당 관련된 화면 영역 다이어그램](material-theme-images/screen-attributes-sml.png)](material-theme-images/screen-attributes.png#lightbox)

기본적으로 `statusBarColor` 의 값으로 설정 되어 `colorPrimaryDark`입니다. 설정할 수 있습니다 `statusBarColor` 단색을 설정할 수 있습니다 또는 `@android:color/transparent` 상태 표시줄을 투명 하 게 하려면. 탐색 모음 만들 수도 있습니다 투명 하 게 설정 하 여 `navigationBarColor` 에 `@android:color/transparent`입니다.


### <a name="creating-a-custom-app-theme"></a>사용자 지정 앱 테마 만들기

만들고에 파일을 수정 하 여 사용자 지정 앱 테마를 만들 수는 **리소스** 앱 프로젝트의 폴더입니다. 사용자 지정 테마를 사용 하 여 앱 스타일 지정을 하려면 다음 단계를 사용 합니다.

-   만들기는 **colors.xml** 파일 **리소스/값** &mdash; 이 파일을 사용 하 여 사용자 지정 테마 색을 정의 합니다. 예를 들어, 다음 코드를 붙여넣을 수 있습니다 **colors.xml** 시작할 수 있도록 합니다.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <color name="my_blue">#3498DB</color>
    <color name="my_green">#77D065</color>
    <color name="my_purple">#B455B6</color>
    <color name="my_gray">#738182</color>
</resources>
```

-   이름 및 사용자 지정 테마에 사용할 색 리소스에 대 한 색 코드를 정의 하도록이 예제에서는 파일을 수정 합니다.

-   만들기는 **리소스/값-v21** 폴더입니다. 이 폴더에 만듭니다는 **styles.xml** 파일:

    [![Styles.xml 리소스/값-21.xml 폴더에서의 위치](material-theme-images/values-v21-sml.png)](material-theme-images/values-v21.png#lightbox)

    사실은 **리소스/값-v21** Android 5.0 관련이 &ndash; 이전 버전의 Android는이 폴더의 파일을 읽지 못합니다.

-   추가 `resources` 노드를 **styles.xml** 정의 `style` 노드에 사용자 지정 테마의 이름입니다. 예를 들어, 다음은 **styles.xml** 정의 하는 파일 *MyCustomTheme* (기본 제공에서 파생 된 `Theme.Material.Light` 테마 스타일):

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Customizations go here -->
    </style>
</resources>
```

-   이 시점에서 사용 하는 앱 *MyCustomTheme* 주식 표시 됩니다 `Theme.Material.Light` 없이 사용자 지정 테마:

    [![사용자 지정 하기 전에 사용자 지정 테마 모양](material-theme-images/custom-theme-before-sml.png)](material-theme-images/custom-theme-before.png#lightbox)

-   색 사용자 지정을 추가 **styles.xml** 레이아웃 특성을 변경 하려는의 색을 정의 하 여 합니다. 예를 들어 앱 표시줄 색을 변경 하려면 `my_blue` UI 컨트롤의 색을 변경 하 고 `my_purple`, 추가 하려면 재정의 색 **styles.xml** 에서 구성 하는 색 리소스를 참조 하는 **colors.xml**:

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

이러한 변경이 사용 하 여 앱을 사용 하는 *MyCustomTheme* 에서 앱 표시줄 색을 표시 됩니다 `my_blue` 및 UI 컨트롤에 `my_purple`를 사용 하지만 `Theme.Material.Light` 색 구성표를 다른 곳:

[![사용자 지정 테마 모양을 사용자 지정 후](material-theme-images/custom-theme-after-sml.png)](material-theme-images/custom-theme-after.png#lightbox)

이 예에서 *MyCustomTheme* 에서 색을 차용 `Theme.Material.Light` 배경에 대 한 색, 상태 표시줄 및 텍스트 색을 하지만 앱 막대의 색 변경 됩니다. `my_blue` 라디오단추의색을설정하고`my_purple`.

<a name="customview" />

### <a name="creating-a-custom-view-style"></a>사용자 지정 보기 스타일 만들기

Android 5.0 하기가 스타일 개별 뷰를 수 있습니다. 만든 후 **colors.xml** 하 고 **styles.xml** (설명 된 대로 이전 섹션에서) 보기 스타일을 추가할 수 있습니다 **styles.xml**합니다.
개별 뷰에 스타일을 하려면 다음 단계를 사용 합니다.

-   편집할 **Resources/values-v21/styles.xml** 추가한는 `style` 사용자 지정 보기 스타일의 이름으로 노드. 이 보기에 대 한 사용자 지정 색 특성을 설정할 `style` 노드. 예를 들어, 사용자 지정을 만들려면 [CardView](~/android/user-interface/controls/card-view.md) 더 둥근 모서리와 사용 하는 스타일 `my_blue` 카드 배경색을 추가 하는 `style` 노드를 **styles.xml** (내 합니다 `resources`노드) 및 배경 색 및 모서리 반지름을 구성 합니다.

```xml
<!-- Theme an individual view: -->
<style name="CardView.MyBlue">

    <!-- Change the background color to Xamarin blue: -->
    <item name="cardBackgroundColor">@color/my_blue</item>

    <!-- Make the corners very round: -->
    <item name="cardCornerRadius">18dp</item>
</style>
```

-   배열에 설정 된 `style` 이전 단계에서 선택한 사용자 지정 스타일 이름에 맞게 해당 보기에 대 한 특성입니다. 예를 들어:

```xml
<android.support.v7.widget.CardView
    style="@style/CardView.MyBlue"
    android:layout_width="200dp"
    android:layout_height="100dp"
    android:layout_gravity="center_horizontal">
```

기본 예제를 제공 하는 다음 스크린 샷에서 `CardView` (표시 된 왼쪽)와 비교해 서는 `CardView` 는 스타일이 지정 된 사용자 지정을 사용 하 여 `CardView.MyBlue` 테마 (오른쪽에 표시 됨):

[![기본 CardView 및 사용자 지정 CardView 예제](material-theme-images/custom-cardview-sml.png)](material-theme-images/custom-cardview.png#lightbox)

이 예제에서는 사용자 지정 `CardView` 배경색으로 표시 됩니다 `my_blue` 및는 18dp 모서리 반지름입니다.


## <a name="compatibility"></a>호환성

Android 5.0에서 재질 테마를 사용 하지만 이전 Android 버전에서 호환 아래쪽 스타일을 자동으로 전환 되도록 응용 프로그램 스타일 지정을 하려면 다음 단계를 사용 합니다.

-   사용자 지정 테마를 정의할 **Resources/values-v21/styles.xml** 재질 테마 스타일에서 파생 되는 합니다. 예를 들어:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   사용자 지정 테마를 정의할 **Resources/values/styles.xml** 이전 테마를에서 파생 되지만 위와 동일한 테마 이름을 사용 하는 합니다. 예를 들어:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Holo.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   **AndroidManifest.xml**, 사용자 지정 테마 이름으로 앱을 구성 합니다. 
    예를 들어:

```xml
<application android:label="MyApp" 
             android:theme="@style/MyCustomTheme">
</application>
```

-   또는 사용자 지정 테마를 사용 하 여 특정 활동을 스타일 지정할 수 있습니다.

```C#
[Activity(Label = "MyActivity", Theme = "@style/MyCustomTheme")]
```

테마에 정의 된 색을 사용 하는 경우는 **colors.xml** 파일에서이 파일에 배치 해야 **리소스/값** (대신 **리소스/값-v21**) 있도록 두 버전의 사용자 지정 테마 색 정의 액세스할 수 있습니다.

에 지정 된 테마 정의 사용 하는 앱을 Android 5.0 장치에서 실행 하는 경우 **Resources/values-v21/styles.xml**합니다. 이 앱이 오래 된 Android 장치에서 실행 되 면 자동으로 대체 됩니다에 지정 된 테마 정의 **Resources/values/styles.xml**합니다.

테마 호환성 이전 Android 버전에 대 한 자세한 내용은 참조 하세요. [대체 리소스](~/android/app-fundamentals/resources-in-android/alternate-resources.md)합니다.

## <a name="summary"></a>요약

이 문서에서는 Android 5.0 (Lollipop)에 포함 된 새로운 재질 테마 사용자 인터페이스 스타일을 도입 되었습니다. 응용 프로그램 스타일 지정 하는 데 사용할 수 있는 세 가지 기본 제공 자료 테마 버전 설명 하 고 앱, 브랜딩에 대 한 사용자 지정 테마를 만드는 방법을 설명 했습니다 테마 하는 방법의 예로 개별 보기를 제공 합니다. 마지막으로,이 문서에서는 앱에서 이전 버전의 Android 사용 하 여 아래쪽 호환성을 유지 하면서 재질 테마를 사용 하는 방법을 설명 합니다.



## <a name="related-links"></a>관련 링크

- [ThemeSwitcher (샘플)](https://developer.xamarin.com/samples/monodroid/android5.0/ThemeSwitcher)
- [롤리팝 소개](../platform/lollipop.md)
- [CardView](controls/card-view.md)
- [대체 리소스](../app-fundamentals/resources-in-android/alternate-resources.md)
- [Android Lollipop](https://developer.android.com/about/versions/lollipop)
- [Android 원형 개발자](https://developer.android.com/about/versions/pie/)
- [재질 디자인](https://developer.android.com/guide/topics/ui/look-and-feel/)
- [재질 디자인 원칙](https://material.io/design/)
- [호환성을 유지 관리](https://developer.android.com/training/backward-compatible-ui/)
