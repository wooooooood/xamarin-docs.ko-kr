---
title: 재질 테마
description: 재질 테마를 사용 하 여 Xamarin Android 앱에 테마를 만드는 방법
ms.prod: xamarin
ms.assetid: DC4CDBD0-3DF9-4B7E-B876-29128985E2A7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 809f6241b3a17f63fe3077f896095c303e1dfd2e
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940833"
---
# <a name="material-theme"></a>재질 테마

*재질 테마* 는 Android 5.0 (롤리팝)로 시작 하는 보기 및 작업의 모양과 느낌을 결정 하는 사용자 인터페이스 스타일입니다. 재질 테마는 Android 5.0에 기본 제공 되므로 응용 프로그램 뿐만 아니라 시스템 UI에서 사용 됩니다. 재질 테마는 사용자가 설정 메뉴에서 동적으로 선택할 수 있는 시스템 수준 모양 옵션의 의미에서 "테마"가 아닙니다. 대신 재질 테마는 앱의 모양과 느낌을 사용자 지정 하는 데 사용할 수 있는 일련의 관련 기본 제공 기본 스타일로 간주할 수 있습니다.

Android는 다음과 같은 세 가지 재질 테마를 제공 합니다.

- `Theme.Material` &ndash; 짙은 재질 테마 버전 이 버전은 Android 5.0의 기본 버전입니다.

- 재질 테마의 &ndash; 라이트 버전을 `Theme.Material.Light` 합니다.

- 재질 테마의 라이트 버전을 &ndash; 하 고 어두운 작업 모음을 사용 하 여 `Theme.Material.Light.DarkActionBar` 합니다.

이러한 재질 테마의 예가 여기에 표시 됩니다.

[어두운 테마, 밝은 테마 및 어두운 작업 모음 테마의 ![예제 스크린샷](material-theme-images/three-flavors-sml.png)](material-theme-images/three-flavors.png#lightbox)

재질 테마에서 파생 하 여 사용자 고유의 테마를 만들고 일부 또는 모든 색 특성을 재정의할 수 있습니다. 예를 들어 `Theme.Material.Light`에서 파생 되는 테마를 만들 수 있지만, 앱 바 색을 재정의 하 여 브랜드 색과 일치 시킵니다. 또한 개별 보기의 스타일을 지정할 수 있습니다. 예를 들어 모퉁이가 더 둥글고 더 진한 배경색을 사용 하는 [CardView](~/android/user-interface/controls/card-view.md) 에 대 한 스타일을 만들 수 있습니다.

전체 앱에 단일 테마를 사용 하거나 앱의 다양 한 화면 (작업)에 다른 테마를 사용할 수 있습니다. 예를 들어 위의 스크린샷에서 단일 앱은 각 활동에 대해 다른 테마를 사용 하 여 기본 제공 색 구성표를 보여 줍니다. 라디오 단추는 앱을 다른 활동으로 전환 하 고, 결과적으로 다른 테마를 표시 합니다.

재질 테마는 Android 5.0 이상 에서만 지원 되므로이 테마를 사용 하 여 이전 버전의 Android에서 실행할 수 있도록 앱에 테마를 적용할 수 없습니다. 그러나 Android 5.0 장치에서 재질 테마를 사용 하도록 앱을 구성 하 고 이전 버전의 Android에서 실행 하는 경우 이전 테마를 정상적으로 대체할 수 있습니다. 자세한 내용은이 문서의 [호환성](#compatibility) 섹션을 참조 하세요.

## <a name="requirements"></a>요구 사항

Xamarin 기반 앱에서 새로운 Android 5.0 재질 테마 기능을 사용 하려면 다음이 필요 합니다.

- Visual Studio 또는 Mac용 Visual Studio를 사용 하 여 **xamarin.ios &ndash; xamarin. android 4.20** 이상 버전을 설치 하 고 구성 해야 합니다. 

- Android SDK Manager를 통해 **Android SDK** &ndash; Android 5.0 (API 21) 이상을 설치 해야 합니다.

- 구체적으로 API 레벨 23 및 이전 버전을 대상으로 하는 경우 **JAVA jdk 1.8** &ndash; jdk 1.7를 사용할 수 있습니다. JDK 1.8은 [Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)에서 사용할 수 있습니다.

Android 5.0 앱 프로젝트를 구성 하는 방법을 알아보려면 [android 5.0 프로젝트 설정](~/android/platform/lollipop.md)을 참조 하세요.

## <a name="using-the-built-in-themes"></a>기본 제공 테마 사용

재질 테마를 사용 하는 가장 쉬운 방법은 사용자 지정 없이 기본 제공 테마를 사용 하도록 앱을 구성 하는 것입니다. 테마를 명시적으로 구성 하지 않으려는 경우 앱은 기본적으로 `Theme.Material` (짙은 테마)로 설정 됩니다. 앱에 활동이 하나만 있는 경우 활동 수준에서 테마를 구성할 수 있습니다. 앱에 여러 활동이 있는 경우 모든 활동에서 동일한 테마를 사용 하도록 응용 프로그램 수준에서 테마를 구성 하거나 다른 활동에 다른 테마를 할당할 수 있습니다. 다음 섹션에서는 앱 수준 및 활동 수준에서 테마를 구성 하는 방법을 설명 합니다.

### <a name="theming-an-application"></a>응용 프로그램 테마

재질 테마 버전을 사용 하도록 전체 응용 프로그램을 구성 하려면 **Androidmanifest .xml** 에서 응용 프로그램 노드의 `android:theme` 특성을 다음 중 하나로 설정 합니다.

- 진한 테마를 &ndash; `@android:style/Theme.Material`.

- &ndash; 밝은 테마를 `@android:style/Theme.Material.Light` 합니다.

- 어두운 동작 표시줄이 있는 밝은 테마를 &ndash; `@android:style/Theme.Material.Light.DarkActionBar`.

다음 예제에서는 light 테마를 사용 하도록 응용 프로그램 *MyApp* 를 구성 합니다.

```xml
<application android:label="MyApp" 
             android:theme="@android:style/Theme.Material.Light">
</application>
```

또는 **AssemblyInfo.cs** (또는 **Properties.cs**)에서 응용 프로그램 `Theme` 특성을 설정할 수 있습니다. 예를 들면 다음과 같습니다.:

```C#
[assembly: Application(Theme="@android:style/Theme.Material.Light")]
```

응용 프로그램 테마가 `@android:style/Theme.Material.Light`로 설정 된 경우 *MyApp* 의 모든 작업이 `Theme.Material.Light`를 사용 하 여 표시 됩니다.

### <a name="theming-an-activity"></a>활동 테마

활동에 테마를 지정 하려면 `Theme` 설정을 활동 선언 위의 `[Activity]` 특성에 추가 하 고 사용 하려는 재질 테마 버전에 `Theme`을 할당 합니다. 다음 예제에서는 `Theme.Material.Light`를 사용 하 여 작업을 테마에 사용 합니다.

```C#
[Activity(Theme = "@android:style/Theme.Material.Light",
          Label = "MyApp", MainLauncher = true, Icon = "@drawable/icon")]  
```

이 앱의 다른 작업은 기본 `Theme.Material` 짙은 색 구성표 (또는 구성 된 경우 응용 프로그램 테마 설정)를 사용 합니다.

<a name="customtheme" />

## <a name="using-custom-themes"></a>사용자 지정 테마 사용

브랜드&rsquo;s 색으로 앱에 스타일을 지정 하는 사용자 지정 테마를 만들어 브랜드를 향상할 수 있습니다. 사용자 지정 테마를 만들려면 기본 제공 재질 테마 버전에서 파생 되는 새 스타일을 정의 하 여 변경 하려는 색 특성을 재정의 합니다. 예를 들어 `Theme.Material.Light.DarkActionBar`에서 파생 되는 사용자 지정 테마를 정의 하 고 화면 배경색을 흰색이 아닌 베이 지로 변경할 수 있습니다.

재질 테마는 사용자 지정을 위해 다음과 같은 레이아웃 특성을 제공 합니다.

- `colorPrimary` 앱 바의 색 &ndash; 합니다.

- `colorPrimaryDark` 상태 표시줄 및 상황별 앱 바의 색 &ndash; 합니다. 이는 일반적으로 `colorPrimary`의 어두운 버전입니다.

- `colorAccent` 확인란, 라디오 단추 및 편집 텍스트 상자와 같은 UI 컨트롤의 색 &ndash; 합니다.

- `windowBackground` 화면 배경의 색 &ndash; 합니다.

- `textColorPrimary` &ndash; 앱 바의 UI 텍스트 색입니다.

- `statusBarColor` 상태 표시줄의 색 &ndash; 합니다.

- `navigationBarColor` 탐색 모음의 색 &ndash; 합니다.

이러한 화면 영역에는 다음 다이어그램이 표시 됩니다.

[특성 및 관련 화면 영역의 ![다이어그램](material-theme-images/screen-attributes-sml.png)](material-theme-images/screen-attributes.png#lightbox)

기본적으로 `statusBarColor`은 `colorPrimaryDark`값으로 설정 됩니다. `statusBarColor`를 단색으로 설정 하거나, `@android:color/transparent`으로 설정 하 여 상태 표시줄을 투명 하 게 만들 수 있습니다. `navigationBarColor`을 `@android:color/transparent`설정 하 여 탐색 모음을 투명 하 게 만들 수도 있습니다.

### <a name="creating-a-custom-app-theme"></a>사용자 지정 앱 테마 만들기

앱 프로젝트의 **리소스** 폴더에서 파일을 만들고 수정 하 여 사용자 지정 앱 테마를 만들 수 있습니다. 사용자 지정 테마를 사용 하 여 앱 스타일을 지정 하려면 다음 단계를 사용 합니다.

- 이 파일을 사용 하 여 사용자 지정 테마 색을 정의 하 &mdash; **리소스/값** 에서 **색 .xml** 파일을 만듭니다. 예를 들어 다음 코드를 **색 .xml** 에 붙여넣어 시작 하는 데 도움이 될 수 있습니다.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <color name="my_blue">#3498DB</color>
    <color name="my_green">#77D065</color>
    <color name="my_purple">#B455B6</color>
    <color name="my_gray">#738182</color>
</resources>
```

- 사용자 지정 테마에서 사용할 색 리소스의 이름 및 색 코드를 정의 하려면이 예제 파일을 수정 합니다.

- **리소스/값-v21** 폴더를 만듭니다. 이 폴더에서 **스타일 .xml** 파일을 만듭니다.

    [Resources/values-21 폴더에서 스타일 .xml의 위치를 ![합니다.](material-theme-images/values-v21-sml.png)](material-theme-images/values-v21.png#lightbox)

    **리소스/값-v21** 은 android 5.0에만 해당 &ndash; 이전 버전의 android는이 폴더의 파일을 읽지 않습니다.

- `resources` 노드를 **스타일 .xml** 에 추가 하 고 사용자 지정 테마 이름으로 `style` 노드를 정의 합니다. 예를 들어 다음은 *Mycustomtheme* (기본 제공 `Theme.Material.Light` 테마 스타일에서 파생 됨)을 정의 하는 **스타일 .xml** 파일입니다.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Customizations go here -->
    </style>
</resources>
```

- 이 시점에서 *Mycustomtheme* 을 사용 하는 앱은 사용자 지정 없이 stock `Theme.Material.Light` 테마를 표시 합니다.

    [사용자 지정 전에 사용자 지정 테마 모양 ![](material-theme-images/custom-theme-before-sml.png)](material-theme-images/custom-theme-before.png#lightbox)

- 변경 하려는 레이아웃 특성의 색을 정의 하 여 **스타일 .xml** 에 색 사용자 지정을 추가 합니다. 예를 들어 앱 바 색을 `my_blue`로 변경 하 고 UI 컨트롤의 색을 `my_purple`으로 변경 하려면 색 **xml**에 구성 된 색 리소스를 참조 하는 **스타일 .xml** 에 색 재정의를 추가 합니다.

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

이러한 변경 내용을 적용 하면 *Mycustomtheme* 을 사용 하는 앱이 `my_purple`의 `my_blue` 및 UI 컨트롤에 앱 바 색을 표시 하지만 다른 모든 위치에서 `Theme.Material.Light` 색 구성표를 사용 합니다.

[사용자 지정 후 사용자 지정 테마 모양 ![](material-theme-images/custom-theme-after-sml.png)](material-theme-images/custom-theme-after.png#lightbox)

이 예제에서 *Mycustomtheme* 은 배경색, 상태 표시줄 및 텍스트 색에 대 한 `Theme.Material.Light` 색을 활용, 앱 바의 색을 `my_blue`으로 변경 하 고 라디오 단추 색을 `my_purple`로 설정 합니다.

<a name="customview" />

### <a name="creating-a-custom-view-style"></a>사용자 지정 보기 스타일 만들기

또한 Android 5.0을 사용 하면 개별 보기의 스타일을 지정할 수 있습니다. 이전 섹션에서 설명한 대로 **색 .xml** 및 **스타일 .xml** 을 만든 후에는 **스타일 .xml**에 보기 스타일을 추가할 수 있습니다.
개별 보기의 스타일을 만들려면 다음 단계를 사용 합니다.

- **Resources/values-v21/styles** 를 편집 하 고 사용자 지정 보기 스타일의 이름과 함께 `style` 노드를 추가 합니다. 이 `style` 노드 내에서 보기에 대 한 사용자 지정 색 특성을 설정 합니다. 예를 들어 모퉁이가 더 둥글고 `my_blue`를 카드 배경색으로 사용 하는 사용자 지정 [CardView](~/android/user-interface/controls/card-view.md) 스타일을 만들려면 `resources`노드 내 에서 **스타일 .xml**에 `style`노드를 추가 하 고 배경색을 구성 합니다. 모퉁이 반경:

```xml
<!-- Theme an individual view: -->
<style name="CardView.MyBlue">

    <!-- Change the background color to Xamarin blue: -->
    <item name="cardBackgroundColor">@color/my_blue</item>

    <!-- Make the corners very round: -->
    <item name="cardCornerRadius">18dp</item>
</style>
```

- 레이아웃에서 해당 보기에 대 한 `style` 특성을 이전 단계에서 선택한 사용자 지정 스타일 이름과 일치 하도록 설정 합니다. 예를 들면 다음과 같습니다.:

```xml
<android.support.v7.widget.CardView
    style="@style/CardView.MyBlue"
    android:layout_width="200dp"
    android:layout_height="100dp"
    android:layout_gravity="center_horizontal">
```

다음 스크린샷은 사용자 지정 `CardView.MyBlue` 테마를 사용 하 여 스타일이 지정 된 `CardView`에 비해 (왼쪽에 표시 된) 기본 `CardView` (오른쪽에 표시 됨)의 예를 제공 합니다.

[![기본 CardView 및 사용자 지정 CardView 예](material-theme-images/custom-cardview-sml.png)](material-theme-images/custom-cardview.png#lightbox)

이 예제에서 사용자 지정 `CardView`는 배경색 `my_blue` 및 18dp 모퉁이 반지름이 표시 됩니다.

## <a name="compatibility"></a>호환성

Android 5.0에서 재질 테마를 사용 하지만 이전 Android 버전에서 하향 호환 스타일로 자동으로 전환 하도록 앱의 스타일을 설정 하려면 다음 단계를 사용 합니다.

- 재질 테마 스타일에서 파생 되는 **Resources/values-v21/styles xml** 에서 사용자 지정 테마를 정의 합니다. 예를 들면 다음과 같습니다.:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

- 이전 테마에서 파생 되지만 위와 동일한 테마 이름을 사용 하는 **리소스/값/스타일 .xml** 의 사용자 지정 테마를 정의 합니다. 예를 들면 다음과 같습니다.:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Holo.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

- **Androidmanifest**에서 사용자 지정 테마 이름으로 앱을 구성 합니다. 
    예를 들면 다음과 같습니다.:

```xml
<application android:label="MyApp" 
             android:theme="@style/MyCustomTheme">
</application>
```

- 또는 사용자 지정 테마를 사용 하 여 특정 작업의 스타일을 지정할 수 있습니다.

```C#
[Activity(Label = "MyActivity", Theme = "@style/MyCustomTheme")]
```

테마에서 **색 .xml** 파일에 정의 된 색을 사용 하는 경우 사용자 지정 테마의 두 버전이 색 정의에 액세스할 수 있도록이 파일을 리소스/값 **-V21**이 아닌 **리소스/값** 에 저장 해야 합니다.

앱이 Android 5.0 장치에서 실행 되는 경우 **Resources/values-v21/styles**에 지정 된 테마 정의를 사용 합니다. 이전 Android 장치에서이 앱을 실행 하면 **리소스/값/스타일 .xml**에 지정 된 테마 정의로 자동으로 대체 됩니다.

이전 Android 버전과의 테마 호환성에 대 한 자세한 내용은 [대체 리소스](~/android/app-fundamentals/resources-in-android/alternate-resources.md)를 참조 하세요.

## <a name="summary"></a>요약

이 문서에서는 Android 5.0 (롤리팝)에 포함 된 새로운 재질 테마 사용자 인터페이스 스타일을 소개 했습니다. 응용 프로그램의 스타일을 지정 하는 데 사용할 수 있는 세 가지 기본 제공 재질 테마의 기능을 설명 하 고, 앱에 브랜딩 하기 위한 사용자 지정 테마를 만드는 방법에 대해 설명 하 고, 개별 뷰에 테마를 지정 하는 방법에 대 한 예제를 제공 했습니다. 마지막으로,이 문서에서는 이전 버전의 Android와의 하향 호환성을 유지 하면서 앱에서 재질 테마를 사용 하는 방법을 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [ThemeSwitcher (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-themeswitcher)
- [롤리팝 소개](../platform/lollipop.md)
- [CardView](controls/card-view.md)
- [대체 리소스](../app-fundamentals/resources-in-android/alternate-resources.md)
- [Android 롤리팝](https://developer.android.com/about/versions/lollipop)
- [Android 원형 개발자](https://developer.android.com/about/versions/pie/)
- [재질 디자인](https://developer.android.com/guide/topics/ui/look-and-feel/)
- [재질 디자인 원칙](https://material.io/design/)
- [호환성 유지](https://developer.android.com/training/backward-compatible-ui/)
