---
title: 다양한 화면에 대한 리소스 만들기
ms.prod: xamarin
ms.assetid: 3D17DE45-115C-7192-5685-44F8EEE07DCC
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/28/2018
ms.openlocfilehash: 63cbe556783ffe22512ff5312817d522120bd15e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61013650"
---
# <a name="creating-resources-for-varying-screens"></a>다양 한 화면에 대 한 리소스 만들기

자체 android 다양 한 해상도, 화면 크기 및 화면 밀도 각각 다양 한 장치에서 실행 됩니다. Android 확장 및 크기 조정 응용 프로그램에서 이러한 장치에서 작동 하도록 수행 됩니다 있지만이 최적 사용자 환경에서 발생할 수 있습니다. 예를 들어, 이미지, 흐리게 표시 될 수 있습니다 또는 보기에서 예상한 대로 배치 될 수 있습니다.


## <a name="concepts"></a>개념

몇 가지 용어 및 개념은 여러 화면을 지원 하기 위해 이해 해야 합니다.

- **화면 크기** &ndash; 응용 프로그램을 표시 하기 위한 물리적 공간의 양

- **화면 밀도** &ndash; 화면의 지정 된 영역에는 픽셀 수입니다. 일반적인 측정 단위가 dpi (인치당)입니다.

- **해상도** &ndash; 화면에서 픽셀의 총 수입니다. 응용 프로그램을 개발할 때 해결 화면 크기와 밀도 중요 하지 않습니다.

- **밀도 독립적 픽셀 (dp)** &ndash; 밀도 독립적 레이아웃 디자인을 수 있도록 가상 측정 단위입니다. 이 수식은 dp 화면 픽셀 변환에 사용 됩니다.

    px &equals; dp &times; dpi &divide; 160

- **방향을** &ndash; 화면의 방향 너비가 높이 보다 큰 경우 가로 방향으로 간주 됩니다. 반면, 세로 방향에는 화면 보다 크도록 높이가 때입니다. 사용자가 장치를 회전할 때 응용 프로그램의 수명 동안 방향을 변경할 수 있습니다.

이러한 개념 중 처음 3 간 관련 되어 있음을 알 수 있습니다. &ndash; 화면 크기가 늘어납니다 밀도 증가 하지 않고 해상도 높이면 합니다. 그러나 밀도 해상도 증가 하는 경우 다음 화면 크기 수 변경 되지 않습니다. 화면 크기, 밀도 및 확인이이 관계는 신속 하 게 화면 지원을 복잡해 집니다.

이 복잡성을 처리를 위해 Android 프레임 워크 사용을 선호 *밀도 독립적 픽셀 (dp)* 화면 레이아웃에 대 한 합니다. (밀도 독립적 픽셀)를 사용 하 여 UI 요소는 동일한 물리적 크기가 서로 다른 밀도 사용 하 여 화면에 사용자에 게 표시 됩니다.


## <a name="supporting-various-screen-sizes-and-densities"></a>다양 한 화면 크기와 밀도 지원합니다.

Android는 레이아웃을 각 화면 구성에 대해 올바르게 렌더링 하는 작업의 대부분을 처리 합니다. 그러나 시스템을 수행할 수 있는 몇 가지 작업이 있습니다.

실제 픽셀 레이아웃 하는 대신 밀도 독립적 픽셀의 사용은 밀도 독립성을 보장 하려면 대부분의 경우 충분 합니다.
Android는 적절 한 크기를 런타임에 드로어 블을 확장 됩니다.
그러나 있기 확장 비트맵을 흐리게 표시 하면 됩니다. 이 문제를 해결 하려면 다른 밀도 대 한 대체 리소스를 제공 합니다. 다중 해상도 및 화면 밀도 대 한 디자인 장치 고 증명 되는 보다 쉽게 시작 밀도 더 높은 해상도 이미지 시간과 축소 합니다.


### <a name="declare-the-supported-screen-size"></a>지원 되는 화면 크기를 선언 합니다.

화면 크기를 선언 하면 지원 되는 장치에만 응용 프로그램을 다운로드할 수 있습니다. 설정 하 여 이렇게 합니다 [지원 화면](https://developer.android.com/guide/topics/manifest/supports-screens-element.html) 요소에는 **AndroidManifest.xml** 파일입니다. 이 요소를 사용 하 여 어떤 화면 크기는 응용 프로그램에서 지원 되도록 합니다. 지정된 된 화면 응용 프로그램 화면에 맞게 해당 레이아웃을 올바르게 배치할 수 있는 경우 지원 되는 데 간주 됩니다. 이 매니페스트 요소를 사용 하 여 응용 프로그램은에 표시 되지 [ *Google Play* ](https://play.google.com/) 화면 사양을 충족 하지 않는 장치에 대 한 합니다. 그러나 응용 프로그램이 지원 되지 않는 화면을 사용 하 여 장치에서 실행 되기는 하지만 레이아웃을 희미하게 보일 수 있습니다 및 잡아 합니다.

지원 되는 화면 sixes에서 선언 되는 **Properites/AndroidManifest.xml** 솔루션의 파일:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Android Manifest](resources-for-varying-screens-images/01-android-manifest-sml.w1581.png)](resources-for-varying-screens-images/01-android-manifest.w1581.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Android Manifest](resources-for-varying-screens-images/01-android-manifest-sml.m761.png)](resources-for-varying-screens-images/01-android-manifest.m761.png#lightbox)

-----

편집할 **AndroidManifest.xml** 포함할 [지원 화면](https://developer.android.com/guide/topics/manifest/supports-screens-element.html):

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

### <a name="provide-alternate-layouts-for-different-screen-sizes"></a>다양 한 화면 크기에 대 한 대체 레이아웃을 제공 합니다.


대체 레이아웃을 사용 하면 특정 화면 크기, 위치 변경 또는 구성 요소 UI 요소의 크기에 대 한 보기를 사용자 지정할 수 있습니다.

API 수준 13 (Android 3.2)부터 화면 크기는 사용 되지 않습니다는 sw 사용 하기 위해*N*dp 한정자입니다. 이 새 한정자 선언 공간의 레이아웃을 지정된 해야 합니다. Android 3.2 이상 실행 하는 응용 프로그램 최신 이러한 한정자를 사용 해야 하는 것이 좋습니다.

예를 들어 레이아웃을 최소 700 필요한 경우 화면 너비의 dp, 대체 레이아웃 폴더에서 할까요 **레이아웃 sw700dp**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![700 dp 화면 너비에 대 한 레이아웃 폴더](resources-for-varying-screens-images/03-layout-sw700dp-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![700 dp 화면 너비에 대 한 레이아웃 폴더](resources-for-varying-screens-images/03-layout-sw700dp-xs.png)

-----


참고로 다양 한 장치에 대 한 일부 숫자 다음과 같습니다.

- **일반적인 전화** &ndash; 320 dp: 일반적인 휴대폰

- **5"태블릿 /"tweener"장치** &ndash; 480 dp: Samsung 메모 등

- **7"태블릿** &ndash; 600 dp:는 Barnes 같은 &amp; Noble Nook

- **10"태블릿** &ndash; 720 dp: Motorola Xoom 등

응용 프로그램에 대 한 대상 API 수준 12 (Android 3.1)이 최대는 레이아웃에서에서 진행 해야 한정자를 사용 하는 디렉터리가 **소규모**/**정상**/**큰**  / **초대형** 대부분의 장치에서 사용할 수 있는 다양 한 화면 크기의 일반화로 합니다. 예를 들어 아래 이미지에는 네 개의 서로 다른 화면 크기에 대 한 대체 리소스

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![4 개의 화면 크기에 대 한 대체 리소스](resources-for-varying-screens-images/04-layout-large-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![4 개의 화면 크기에 대 한 대체 리소스](resources-for-varying-screens-images/04-layout-large-xs.png)

-----

다음은 이전 pre-API 수준 13 화면 크기 한정자 밀도 독립적 픽셀을 비교 하는 방법의 비교입니다.

- 426 dp dp를 x 320 **작은**

- 470 dp dp를 x 320 **정상**

- 640 dp dp를 x 480 **큰**

- 960 dp dp를 x 720 **초대형**

최신 화면 API 수준 13 한정자 크기 있고 등록 API 수준 12 및 낮은의 이전 화면 한정자 보다 우선 순위가 높습니다.
이전 및 새 API 수준에 걸쳐 있는 응용 프로그램의 경우 다음 스크린샷에 표시 된 대로 한정자 집합을 모두 사용 하 여 대체 리소스를 만들어야 할 수 있습니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![두 한정자를 사용 하 여 대체 리소스](resources-for-varying-screens-images/05-both-qualifiers-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![두 한정자를 사용 하 여 대체 리소스](resources-for-varying-screens-images/05-both-qualifiers-xs.png)

-----



### <a name="provide-different-bitmaps-for-different-screen-densities"></a>다양 한 화면 밀도 대 한 다양 한 비트맵을 제공 합니다.

Android 장치에 대 한 필요에 따라 비트맵 조정 하지만 자체 비트맵 수 하지 원활 하 게 확장 또는 축소: 유사 항목 일치 또는 흐리게 표시 될 수 있습니다. 화면 밀도 대 한 적절 한 비트맵을 제공 하는이 문제를 완화 됩니다.

예를 들어 아래 이미지는 발생할 수 있는 레이아웃 및 모양을 문제의 예로 밀도-지정 경우 리소스는 제공 되지 않습니다.

![밀도 리소스 없이도 스크린 샷](resources-for-varying-screens-images/06-density-not-provided.png)

밀도 별 리소스를 사용 하 여 설계 된 레이아웃에이 비교 합니다.

![밀도 별 리소스를 사용 하 여 스크린 샷](resources-for-varying-screens-images/07-density-specific-resources.png)


### <a name="create-varying-density-resources-with-android-asset-studio"></a>Android Asset Studio를 사용 하 여 다양 한 밀도 리소스 만들기

다양 한 밀도의 이러한 비트맵 생성은 지루한 작업이 될 수 있습니다. 따라서 Google 이라는 이러한 비트맵 만들기와 관련 된 번거로움을 중 일부를 줄일 수 있는 온라인 유틸리티를 만들었습니다 합니다 [ **Android Asset Studio**](https://romannurik.github.io/AndroidAssetStudio/)합니다.

[![Android Asset Studio](resources-for-varying-screens-images/08-android-asset-studio-sml.png)](resources-for-varying-screens-images/08-android-asset-studio.png#lightbox)

이 웹 사이트 생성 한 이미지를 제공 하 여 네 가지 일반 화면 밀도 대상으로 하는 비트맵의 도움이 됩니다. Android Asset Studio를 다음 일부 사용자 지정을 사용 하 여 비트맵을 만들고 zip 파일로 다운로드할 수 있도록 합니다.


## <a name="tips-for-multiple-screens"></a>여러 화면에 대 한 팁

Android 장치의 놀랄만한 숫자로 실행 되며 화면 크기 및 화면 밀도의 조합을 울 수 있습니다. 다음 팁을 다양 한 장치를 지 원하는 데 필요한 노력을 최소화할 수 있습니다.

- **만 디자인 및 개발 해야 하는 작업에 대 한** &ndash; 다양 한 장치, 있지만 일부 디자인 및 개발 하기 위한 많은 노력이 소요 되는 드문 폼 팩터에 존재 합니다. 합니다 [ **화면 크기와 밀도** ](https://developer.android.com/resources/dashboard/screens.html) 대시보드는 화면 크기/화면 밀도 매트릭스의 분석에 데이터를 제공 하는 Google에서 제공 하는 페이지입니다. 이 분석 화면을 지원 하기 위해 개발 노력 하는 방법에 대 한 정보를 제공 합니다.

- **픽셀 보다는 DPs 사용** -픽셀 화면 밀도 변경 될 때 문제가 됩니다. 하드 코딩 하지 마십시오. 픽셀 값. Dp (밀도 독립적 픽셀)를 위해 픽셀을 방지 합니다.

- **방지** [AbsoluteLayout](https://developer.xamarin.com/api/type/Android.Widget.AbsoluteLayout/)
  **아무 곳에 나 가능** &ndash; API 수준 3 (Android 1.5)에서 사용 되지 않습니다 하 고 불안정 레이아웃 됩니다. 사용 해야 합니다. 대신,와 같은 유연한 레이아웃 위젯 사용 하려고 [ **LinearLayout**](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)하십시오 [ **RelativeLayout**](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/), 또는 새 [ **GridLayout**](https://developer.xamarin.com/api/type/Android.Widget.GridLayout/)합니다.

- **기본값으로 하나의 레이아웃 방향을 선택** &ndash; 대체 리소스를 제공 하는 대신에 예를 들어 **레이아웃 land** 하 고 **레이아웃 포트**에 대 한 리소스 가로 **레이아웃**, 및 세로 모드에 대 한 리소스 **레이아웃 포트**합니다.

- **LayoutParams를 사용 하 여 높이 및 너비에 대 한** -XML 레이아웃 파일의 UI 요소를 정의 하는 경우 사용 하 여 Android 응용 프로그램을 **wrap_content** 및 **fill_parent** 값에 더 많은 성공 해야 합니다. 픽셀 또는 밀도 독립적 단위를 사용 하 여 보다 다양 한 장치의 적절 한 참조를 확인 합니다. Android 리소스 크기를 조정할 비트맵을 적절 하 게 발생 하는 이러한 차원 값입니다. 이와 동일한 이유로 밀도 독립적 단위는 가장 예약 된 시기에 대 한 UI 요소의 안쪽 여백 및 여백을 지정 합니다.


## <a name="testing-multiple-screens"></a>여러 화면 테스트

지원 되는 모든 구성에 대 한 Android 응용 프로그램을 테스트 해야 합니다. 이상적으로 실제 장치 자체에서 장치를 테스트 해야 하지만 대부분의 경우에서 이것이 불가능 합니다.
이 경우 각 장치 구성에 대해 에뮬레이터 및 Android 가상 장치 설정 사용 데 도움이 됩니다.

Android SDK 크기, 밀도 및 다양 한 장치 해상도 일부 에뮬레이터 스킨 Avd를 만드는 데 사용할 수는 복제를 제공 합니다.
많은 하드웨어 공급 업체는 마찬가지로 장치 스킨을 제공합니다.

서비스를 테스트 하는 타사 서비스를 사용 하는 방법도 있습니다.
이러한 서비스는 APK를 수행 하 고 다양 한 장치에서 실행 후 응용 프로그램의 작동 방식을 피드백을 제공 됩니다.
