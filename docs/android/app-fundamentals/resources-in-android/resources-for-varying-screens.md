---
title: 다양한 화면에 대한 리소스 만들기
ms.prod: xamarin
ms.assetid: 3D17DE45-115C-7192-5685-44F8EEE07DCC
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/28/2018
ms.openlocfilehash: cbd392dcae173eb3baf0fb8f0c3c4ec7c0da23a1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025125"
---
# <a name="creating-resources-for-varying-screens"></a>다양 한 화면에 대 한 리소스 만들기

Android 자체는 각각 다양 한 해상도, 화면 크기 및 화면 밀도를 포함 하는 다양 한 장치에서 실행 됩니다. Android는 이러한 장치에서 응용 프로그램이 작동 하도록 크기 조정 및 크기 조정을 수행 하지만이로 인해 사용자 환경이 매우 최적이 될 수 있습니다. 예를 들어 이미지가 흐리게 표시 되거나 뷰에서 예상 대로 배치 될 수 있습니다.

## <a name="concepts"></a>개념

여러 화면을 지원 하기 위해 몇 가지 용어와 개념을 이해 하는 것이 중요 합니다.

- **화면 크기** &ndash; 응용 프로그램을 표시 하기 위한 실제 공간의 크기

- **화면 밀도** 는 화면에서 지정 된 영역의 픽셀 수를 &ndash; 합니다. 일반적인 측정 단위는 dpi (인치당 도트 수)입니다.

- **해상도** &ndash; 화면의 전체 픽셀 수를 표시 합니다. 응용 프로그램을 개발할 때 해상도는 화면 크기 및 밀도 만큼 중요 하지 않습니다.

- 밀도 **독립적 픽셀 (dp)** &ndash; 레이아웃을 밀도와 무관 하 게 설계할 수 있도록 하는 가상 측정 단위입니다. 이 수식은 dp를 화면 픽셀로 변환 하는 데 사용 됩니다.

    px &equals; dp &times; dpi &divide; 160

- 화면의 방향이 높이가 긴 경우 화면의 방향이 가로 인 것으로 간주 됩니다 **. &ndash;** 반면 세로 방향은 화면이 너비가 넓은 경우입니다. 사용자가 장치를 회전할 때 응용 프로그램의 수명 동안 방향이 변경 될 수 있습니다.

이러한 개념 중 처음 3 개는 상호 &ndash; 관련 되어 있으며, 밀도를 증가 시 키 지 않고 해상도를 늘리면 화면 크기가 증가 합니다. 그러나 밀도와 해상도를 모두 늘리면 화면 크기가 변경 되지 않은 상태로 유지 될 수 있습니다. 화면 크기, 밀도 및 해상도의 이러한 관계는 화면을 신속 하 게 지원 합니다.

이러한 복잡성을 해결 하기 위해 Android framework는 화면 레이아웃에 대 한 *dp (밀도 독립적 픽셀)* 를 사용 하는 것이 선호 됩니다. 밀도 독립 픽셀을 사용 하면 UI 요소가 사용자에 게 표시 되어 다른 밀도를 사용 하는 화면에서 동일한 실제 크기를 갖게 됩니다.

## <a name="supporting-various-screen-sizes-and-densities"></a>다양 한 화면 크기 및 밀도 지원

Android는 각 화면 구성에 맞게 레이아웃을 올바르게 렌더링 하는 작업의 대부분을 처리 합니다. 그러나 시스템을 확장 하는 데 사용할 수 있는 몇 가지 작업이 있습니다.

대부분의 경우 밀도 독립성을 보장 하기 위해 레이아웃에 실제 픽셀 대신 밀도 독립적 픽셀을 사용 하는 것도 충분 합니다.
Android는 런타임에 적합 한 크기로 확장 가능 합니다.
그러나 크기를 조정 하면 비트맵이 흐리게 표시 될 수 있습니다. 이 문제를 해결 하려면 다양 한 밀도에 대 한 대체 리소스를 제공 합니다. 여러 해상도 및 화면 밀도를 위해 장치를 설계할 때 더 높은 해상도 또는 밀도 이미지를 사용 하 여 시작 하 고 축소 하는 것이 더 쉽습니다.

### <a name="declare-the-supported-screen-size"></a>지원 되는 화면 크기 선언

화면 크기를 선언 하면 지원 되는 장치만 응용 프로그램을 다운로드할 수 있습니다. 이렇게 하려면 **Androidmanifest .xml** 파일의 [지원 화면](https://developer.android.com/guide/topics/manifest/supports-screens-element.html) 요소를 설정 합니다. 이 요소는 응용 프로그램에서 지원 되는 화면 크기를 지정 하는 데 사용 됩니다. 지정 된 화면은 응용 프로그램에서 해당 레이아웃을 화면에 올바르게 삽입할 수 있는 경우 지원 되는 것으로 간주 됩니다. 이 매니페스트 요소를 사용 하면 화면 사양을 충족 하지 않는 장치에 대 한 [*Google Play*](https://play.google.com/) 에 응용 프로그램이 표시 되지 않습니다. 그러나 응용 프로그램은 지원 되지 않는 화면을 사용 하는 장치에서 계속 실행 되지만 레이아웃은 흐린 및 모자이크로 표시 될 수 있습니다.

지원 되는 화면 sixes 솔루션의 **속성/AndroidManifest .xml** 파일에 선언 됩니다.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Android Manifest](resources-for-varying-screens-images/01-android-manifest-sml.w1581.png)](resources-for-varying-screens-images/01-android-manifest.w1581.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Android Manifest](resources-for-varying-screens-images/01-android-manifest-sml.m761.png)](resources-for-varying-screens-images/01-android-manifest.m761.png#lightbox)

-----

[지원 화면](https://developer.android.com/guide/topics/manifest/supports-screens-element.html)을 포함 하도록 **Androidmanifest** 을 편집 합니다.

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          android:versionCode="1"
          android:versionName="1.0"
          package="HelloWorld.HelloWorld">
      <uses-sdk android:minSdkVersion="21" android:targetSdkVersion="27" />
      <supports-screens android:resizable="true"
                        android:smallScreens="true"
                        android:normalScreens="true"
                        android:largeScreens="true" />
      <application android:allowBackup="true"
                   android:icon="@mipmap/ic_launcher"
                   android:label="@string/app_name"
                   android:roundIcon="@mipmap/ic_launcher_round"
                   android:supportsRtl="true" android:theme="@style/AppTheme">
  </application>
</manifest>
```

### <a name="provide-alternate-layouts-for-different-screen-sizes"></a>여러 화면 크기에 대 한 대체 레이아웃 제공

대체 레이아웃을 사용 하면 구성 요소 UI 요소의 위치 또는 크기를 변경 하 여 특정 된 화면 크기에 대 한 뷰를 사용자 지정할 수 있습니다.

API 수준 13 (Android 3.2) 부터는 sw*N*dp 한정자를 사용 하기 위해 화면 크기가 더 이상 사용 되지 않습니다. 이 새 한정자는 지정 된 레이아웃에 필요한 공간의 크기를 선언 합니다. Android 3.2 이상에서 실행 되는 응용 프로그램은 이러한 최신 한정자를 사용 하는 것이 좋습니다.

예를 들어 레이아웃에 화면 너비의 최소 700 dp가 필요한 경우 대체 레이아웃은 폴더 **레이아웃-sw700dp**로 이동 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![700 dp 화면 너비의 레이아웃 폴더](resources-for-varying-screens-images/03-layout-sw700dp-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![700 dp 화면 너비의 레이아웃 폴더](resources-for-varying-screens-images/03-layout-sw700dp-xs.png)

-----

다음은 다양 한 장치에 대 한 몇 가지 지침입니다.

- 일반적인 **전화** &ndash; 320 dp: 일반적인 전화

- **5 "태블릿/" tweener "장치** &ndash; 480 dp (예: Samsung Note)

- **7 "태블릿** &ndash; 600 Dp: Barnes and &amp; Noble nook와 같은

- Motorola Xoom과 같은 **10 개의 "태블릿** &ndash; 720 dp:

API 수준을 최대 12 (Android 3.1)로 지정 하는 응용 프로그램의 경우 대부분의 장치에서 사용할 수 있는 다양 한 화면 크기의 일반화로 한정자 **small**/**normal**/**large**/**초대형** 를 사용 하는 디렉터리에 레이아웃을 배치 해야 합니다. 예를 들어 아래 이미지에는 4 개의 다른 화면 크기에 대 한 대체 리소스가 있습니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![4 개의 화면 크기에 대 한 대체 리소스](resources-for-varying-screens-images/04-layout-large-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![4 개의 화면 크기에 대 한 대체 리소스](resources-for-varying-screens-images/04-layout-large-xs.png)

-----

다음은 이전 프리 API 수준 13 화면 크기 한정자가 밀도 독립적 픽셀과 어떻게 비교 되는지 비교 하는 것입니다.

- 426 dp x 320 dp가 **작음**

- 470 dp x 320 dp가 **정상** 입니다.

- 640 dp x 480 dp가 **큼**

- 960 dp x 720 dp는 **초대형**

API 수준 13 이상에서 최신 화면 크기 한정자는 API 수준 12 이하의 이전 화면 한정자 보다 우선 순위가 높습니다.
이전 및 새 API 수준으로 확장 되는 응용 프로그램의 경우 다음 스크린샷에 표시 된 것 처럼 두 한정자 집합을 사용 하 여 대체 리소스를 만들어야 할 수 있습니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![두 한정자가 있는 대체 리소스](resources-for-varying-screens-images/05-both-qualifiers-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![두 한정자가 있는 대체 리소스](resources-for-varying-screens-images/05-both-qualifiers-xs.png)

-----

### <a name="provide-different-bitmaps-for-different-screen-densities"></a>화면 밀도 마다 다른 비트맵 제공

Android는 장치에 필요한 대로 비트맵 크기를 조정 하지만, 비트맵 자체의 크기를 조정 하거나 원활 하지 않을 수 있습니다 .이는 유사 하거나 흐린 것일 수 있습니다. 화면 밀도에 적절 한 비트맵을 제공 하면이 문제를 완화할 수 있습니다.

예를 들어 아래 이미지는 밀도 지정 리소스를 제공 하지 않을 때 발생할 수 있는 레이아웃 및 모양 문제의 예입니다.

![밀도 리소스가 없는 스크린샷](resources-for-varying-screens-images/06-density-not-provided.png)

이를 밀도 별 리소스로 디자인 된 레이아웃과 비교 합니다.

![밀도 관련 리소스가 있는 스크린샷](resources-for-varying-screens-images/07-density-specific-resources.png)

### <a name="create-varying-density-resources-with-android-asset-studio"></a>Android Asset Studio를 사용 하 여 다양 한 밀도 리소스 만들기

다양 한 밀도의 이러한 비트맵을 만드는 것이 다소 지루한 일 수 있습니다. 따라서 Google은 [**Android Asset Studio**](https://romannurik.github.io/AndroidAssetStudio/)라는 이러한 비트맵 만들기와 관련 된 일부 번거로움을를 줄일 수 있는 온라인 유틸리티를 만들었습니다.

[![Android Asset Studio](resources-for-varying-screens-images/08-android-asset-studio-sml.png)](resources-for-varying-screens-images/08-android-asset-studio.png#lightbox)

이 웹 사이트는 하나의 이미지를 제공 하 여 네 가지 일반적인 화면 밀도를 대상으로 하는 비트맵을 만드는 데 도움이 됩니다. 그러면 Android Asset Studio에서 일부 사용자 지정 항목을 사용 하 여 비트맵을 만든 다음 zip 파일로 다운로드할 수 있습니다.

## <a name="tips-for-multiple-screens"></a>여러 화면에 대 한 팁

Android는 놀랄만한 수의 장치에서 실행 되며 화면 크기와 화면 밀도의 조합을 과도 하 게 볼 수 있습니다. 다음은 다양 한 장치를 지 원하는 데 필요한 노력을 최소화 하는 데 도움이 되는 팁입니다.

- **필요한 &ndash;에 대해서만 디자인 하 고 개발할** 수 있습니다. 그러나 몇 가지 장치를 사용 하는 경우에만 디자인 하 고 개발 하는 데 상당한 노력이 소요 될 수 있는 드문 폼 팩터는 있습니다. [**화면 크기 및 밀도**](https://developer.android.com/resources/dashboard/screens.html) 대시보드는 화면 크기/화면 밀도 행렬의 분석에 대 한 데이터를 제공 하는 Google에서 제공 하는 페이지입니다. 이 분석을 통해 화면 지원에 대 한 개발 노력에 대 한 통찰력을 얻을 수 있습니다.

- **픽셀 대신 DPs를 사용** 하면 화면 밀도가 변경 될 때 문제가 됩니다. 픽셀 값을 하드 코딩 하지 마십시오. Dp (밀도 독립적 픽셀)를 사용 하 여 픽셀을 방지 합니다.

- &ndash; **가능** 하면 [AbsoluteLayout](xref:Android.Widget.AbsoluteLayout)
  사용 **하지** 않는 것이 좋습니다 .이는 API 수준 3 (Android 1.5)에서 더 이상 사용 되지 않으며 불안정 레이아웃을 생성 합니다. 사용 하면 안 됩니다. 대신 [**LinearLayout**](xref:Android.Widget.LinearLayout), [**RelativeLayout**](xref:Android.Widget.RelativeLayout)또는 새 [**GridLayout**](xref:Android.Widget.GridLayout)와 같은 더 유연한 레이아웃 위젯을 사용 합니다.

- **기본 &ndash; 레이아웃 방향 하나를 선택** 합니다. 예를 들어 대체 리소스를 제공 하는 대신 레이아웃- **육지** 와 **레이아웃 포트**를 제공 하 고, **레이아웃**에 가로 및 세로의 리소스를 **레이아웃 포트**에 배치 합니다.

- **Height 및 Width에 대해 LayoutParams 사용** -XML 레이아웃 파일에서 UI 요소를 정의 하는 경우 **wrap_content** 및 **fill_parent** 값을 사용 하는 Android 응용 프로그램은 픽셀 또는 밀도 독립적 단위를 사용 하는 것과는 다른 장치에 대해 적절 한 모양을 확인 합니다. 이러한 차원 값으로 인해 Android에서 비트맵 리소스를 적절 하 게 확장할 수 있습니다. 이와 같은 이유로, 밀도 독립적 단위는 UI 요소의 여백 및 안쪽 여백을 지정 하는 경우에 적합 합니다.

## <a name="testing-multiple-screens"></a>여러 화면 테스트

Android 응용 프로그램은 지원 되는 모든 구성에 대해 테스트 되어야 합니다. 이상적인 장치는 실제 장치 자체에서 테스트 해야 하지만 대부분의 경우에는 불가능 하거나 실용적이 지 않습니다.
이 경우 각 장치 구성에 에뮬레이터 및 Android 가상 장치 설치를 사용 하는 것이 유용 합니다.

Android SDK는 AVDs를 만드는 데 사용할 수 있는 일부 에뮬레이터 스킨을 제공 하 여 많은 장치의 크기, 밀도 및 해상도를 복제 합니다.
하드웨어 공급 업체의 대부분은 장치에 대 한 스킨을 제공 합니다.

또 다른 옵션은 타사 테스트 서비스의 서비스를 사용 하는 것입니다.
이러한 서비스는 APK를 사용 하 여 여러 다른 장치에서 실행 한 다음 응용 프로그램이 작동 하는 방식에 대 한 피드백을 제공 합니다.
