---
title: "다양 한 화면에 대 한 리소스 만들기"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3D17DE45-115C-7192-5685-44F8EEE07DCC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: fcd77d97d492baee441cfd428e58ea83525f927e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="creating-resources-for-varying-screens"></a>다양 한 화면에 대 한 리소스 만들기

Android 자체 각각 다양 한 해상도, 한 화면 크기 및 화면 밀도 가지 다양 한 장치에서 실행 됩니다. Android를 크기 조정 및 이러한 장치에서 작동 하는 응용 프로그램을 쉽게 크기 조정을 수행 합니다 되지만이 하위 최적의 사용자 경험을 제공할 수 있습니다. 예를 들어 이미지가 흐리게 표시, 이미지 될 수 있습니다 유발 하는 레이아웃에서 UI 요소의 위치 겹치거나 멀리 떨어져 너무 수는 과도 한 (또는 부족) 화면 공간을 차지 하는 것입니다.


## <a name="concepts"></a>개념

몇 가지 용어 및 개념은 여러 화면을 지원 하기 위해 이해 해야 합니다.

- **화면 크기** &ndash; 응용 프로그램을 표시 하기 위한 물리적 공간의 양

- **밀도 화면** &ndash; 화면에 지정 된 영역에는 픽셀 수입니다. 일반적인 측정 단위가 (dpi)입니다.

- **해상도** &ndash; 화면에서 픽셀의 총 수입니다. 응용 프로그램을 개발할 때 해결 화면 크기와 밀도으로 중요 하지 않습니다.

- **밀도 독립적 픽셀 (dp)** &ndash; 밀도 무관을 설계 해야 레이아웃을 허용 하도록 가상 측정 단위입니다. Dp 화면 픽셀으로 변환 하려면 다음 수식을 사용 됩니다.

    px &equals; dp &times; dpi &divide; 160

- **방향** &ndash; 화면의 방향은 높이 보다 긴 경우 가로 방향으로 간주 됩니다. 반면, 세로 방향 화면 너비 보다 높이가 긴 경우입니다. 사용자가 장치를 회전할 때 응용 프로그램의 수명 동안 방향을 변경할 수 있습니다.

이러한 개념 중 처음 3 개의 상호 연결 되며 &ndash; 밀도가 화면 크기가 늘어납니다. 없이 해상도 증가 합니다. 그러나 밀도 해상도 모두 만큼 증가 하는 경우 다음의 화면 크기 수 변경 되지 않습니다. 화면 크기, 밀도 및 해결이이 관계에 화면 지원을 매우 신속 하 게 복잡해 집니다.

이 복잡성이를 해결 하려면 Android 프레임 워크를 사용 하 여 선호 *밀도 독립적 픽셀 (dp)* 화면 레이아웃에 대 한 합니다. UI 요소는 독립적 픽셀 밀도 사용 하 여 다른 밀도 화면에는 동일한 실제 크기를 사용자에 게 표시 됩니다.


## <a name="supporting-various-screen-sizes-and-densities"></a>다양 한 화면 크기와 밀도 지원합니다.

Android에는 각 화면 구성에 대해 제대로 레이아웃을 렌더링 하는 작업의 대부분을 처리 합니다. 그러나 시스템을 수행할 수 있는 몇 가지 작업이 있습니다.

밀도 독립성을 위해 대부분의 경우에서 충분 한가 레이아웃에서 실제 픽셀 대신 픽셀 밀도 관계 없이 사용 되었습니다.
Android에는 적절 한 크기를 런타임에 drawables를 확장 됩니다.
그러나 있기이 크기 조정은 흐리게 표시 하는 비트맵 발생 합니다. 이 방지 하려면 다른 밀도 대 한 대체 리소스를 제공 해야 할 수도 있습니다. 것이 더 쉽게 하는 다중 해상도 형식 및 화면 밀도 대 한 장치를 디자인할 때 시작 하 여 더 높은 해상도 또는 밀도 이미지, 다음 축소할 수도 있습니다. 이렇게 하면 모든 뜨 리고 또는 크기 조정이 발생할 수 있는 왜곡 되지 것입니다.


### <a name="declare-the-screen-size-the-application-supports"></a>화면 크기에서 응용 프로그램에서 지 원하는 선언

화면 크기를 선언 하면만 지원 되는 장치는 응용 프로그램을 다운로드할 수 있습니다. 설정 하 여 이렇게는 [지원 화면](http://developer.android.com/guide/topics/manifest/supports-screens-element.html) 요소에는 **AndroidManifest.xml** 파일입니다. 이 요소를 사용 하 여 어떤 화면 크기는 응용 프로그램에서 지원 되는 지정할 수 있습니다. 응용 프로그램이 제대로 수 있는 경우 지원 해야 하는 특정된 화면 간주 됩니다 채우도록 레이아웃의 합니다. 이 매니페스트 요소를 사용 하 여 응용 프로그램에 표시 되지 [ *Google Play* ](https://play.google.com/) 화면 사양에 맞지 않는 장치에 대 한 합니다. 그러나 지원 되지 않는 화면을 사용 하 여 장치에 응용 프로그램 실행 되기는 하지만 사용할 수 있는 레이아웃 흐리게 표시 될 수 있습니다 및 픽셀입니다.

Xamarin.Android에서이 작업을 수행 하려면 먼저 추가 하는 데 필요한는 **AndroidManifest.xml** 아직 없는 경우에 프로젝트에 파일:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Android 매니페스트에서](resources-for-varying-screens-images/01-android-manifest-vs-sml.png)](resources-for-varying-screens-images/01-android-manifest-vs.png#lightbox)

**AndroidManifest.xml** 에 추가 되 고 **속성** 디렉터리입니다. 파일을 포함 하도록 한 다음 편집할 [지원 화면](http://developer.android.com/guide/topics/manifest/supports-screens-element.html):

[![화면 지원 추가](resources-for-varying-screens-images/02-adding-supports-screens-vs-sml.png)](resources-for-varying-screens-images/02-adding-supports-screens-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Android 매니페스트에서](resources-for-varying-screens-images/01-android-manifest-xs-sml.png)](resources-for-varying-screens-images/01-android-manifest-xs.png#lightbox)

**AndroidManifest.xml** 에 추가 되 고 **속성** 디렉터리입니다. 파일을 포함 하도록 한 다음 편집할 [지원 화면](http://developer.android.com/guide/topics/manifest/supports-screens-element.html):

[![화면 지원 추가](resources-for-varying-screens-images/02-adding-supports-screens-xs-sml.png)](resources-for-varying-screens-images/02-adding-supports-screens-xs.png#lightbox)

-----

### <a name="provide-alternate-layouts-for-different-screen-sizes"></a>다양 한 화면 크기에 대 한 대체 레이아웃을 제공 합니다.

Android 화면 크기에 따라 크기를 조정할지, 하지만 일부 경우에는 충분 한 아닐 수 있습니다. 큰 화면에 있는 일부 UI 요소의 크기를 늘리려면 또는 작은 화면에 대 한 UI 요소의 위치 설정을 변경 하려면 바람직하지 수도 있습니다.

API 수준 (Android 3.2) 13 부터는 한 화면 크기는 이제는 소프트웨어를 사용 하 여*N*dp 한정자입니다. 이 새 한정자 필요한 특정된 레이아웃 공간의 크기를 선언 합니다. Android 3.2 이상이 실행 되어야 하는 응용 프로그램 최신 이러한 한정자 사용 될 것이 좋습니다.

예를 들어 레이아웃 화면 너비의 최소 700dp, 필요한 경우 대체 레이아웃 거쳐야 폴더에 **레이아웃 sw700dp**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![700dp 화면 너비에 대 한 레이아웃 폴더](resources-for-varying-screens-images/03-layout-sw700dp-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![700dp 화면 너비에 대 한 레이아웃 폴더](resources-for-varying-screens-images/03-layout-sw700dp-xs.png)

-----


참고로, 다양 한 장치에 대 한 몇 가지 번호 다음과 같습니다.

- **일반적인 전화** &ndash; 320dp: 일반적인 전화

- **태블릿 5"/"tweener"장치** &ndash; 480dp: 삼성 메모 등

- **7"태블릿** &ndash; 600dp: 등의 Barnes &amp; Noble Nook

- **10"태블릿** &ndash; 720dp: Motorola Xoom 등

응용 프로그램에 대 한 대상 API 레벨 최대 12 (Android 3.1)을 사용할 수 있는 레이아웃에 삽입 되어야 할지 한정자를 사용 하는 디렉터리 **작은**/**일반**/**큰**  / **xlarge** 으로 대부분의 장치에서 사용할 수 있는 다양 한 화면 크기의 일반화 합니다. 예를 들어 아래 이미지에는 4 개의 다양 한 화면 크기에 대 한 대체 리소스

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![4 개의 화면 크기에 대 한 대체 리소스](resources-for-varying-screens-images/04-layout-large-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![4 개의 화면 크기에 대 한 대체 리소스](resources-for-varying-screens-images/04-layout-large-xs.png)

-----

다음은 이전 이전 API 수준 13 화면 크기 한정자 어떻게 다른 밀도 독립적 픽셀의 비교입니다.

- 426dp x 320dp은 **작은**

- 470dp x 320dp는 **정상**

- 640dp x 480dp은 **큰**

- 960dp x 720dp는 **xlarge**

새 화면 13 API 수준에서 한정자 크기 있고를 12 미만과 API 수준의 이전 화면 한정자 보다 우선 순위가 높습니다.
이전 클라이언트 암호 및 새로운 API 수준에 걸쳐 있는 응용 프로그램의 경우 다음 스크린 샷에서 같이 두 가지 한정자를 사용 하 여 대체 리소스를 만들어야 할 수 있습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![두 한정자를 사용 하 여 대체 리소스](resources-for-varying-screens-images/05-both-qualifiers-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![두 한정자를 사용 하 여 대체 리소스](resources-for-varying-screens-images/05-both-qualifiers-xs.png)

-----



### <a name="provide-different-bitmaps-for-different-screen-densities"></a>다른 화면 밀도 대 한 다른 비트맵을 제공 합니다.

Android 장치에 대해 필요에 따라 비트맵을 크기 조정 하지만 자체 비트맵 수 하지 원활 하 게 확장 하거나 축소할 수: 유사 항목 일치 또는 흐리게 될 수 있습니다. 화면 밀도 대 한 적절 한 비트맵을 제공 하는이 문제를 완화 합니다.

예를 들어 아래 이미지는 발생할 수 있는 레이아웃 및 모양을 문제의 예시 리소스 제공 하지 않으면 밀도 지정 하는 경우.

![밀도 리소스 없이 스크린 샷](resources-for-varying-screens-images/06-density-not-provided.png)

밀도 특정 리소스에 두고 설계 된 레이아웃에이 비교해 보십시오.

![밀도 별 리소스와 스크린샷](resources-for-varying-screens-images/07-density-specific-resources.png)


### <a name="create-varying-density-resources-with-android-asset-studio"></a>Android 자산 Studio와 함께 다양 한 밀도 리소스 만들기

다양 한 밀도 이러한 비트맵 만들기 지루한 작업이 될 수 있습니다. 따라서 Google 이라는 이러한 비트맵 만들기와 관련 된 건 일부 줄일 수 있습니다는 온라인 유틸리티 만들었습니다는 [ **자산 o**](https://romannurik.github.io/AndroidAssetStudio/)합니다.

[![Android Asset Studio](resources-for-varying-screens-images/08-android-asset-studio-sml.png)](resources-for-varying-screens-images/08-android-asset-studio.png#lightbox)

이 웹 사이트 생성의 비트맵 이미지를 제공 하 여 4 개의 일반적인 화면 밀도 대상으로 하는 데 도움이 됩니다. 자산 o 다음 비트맵 일부 사용자 지정을 만들고 zip 파일로 다운로드할 수 있도록 합니다.


## <a name="tips-for-multiple-screens"></a>여러 화면에 대 한 팁

이러한 다양 한 장치에서 android를 실행 하 고 화면 크기 및 화면 밀도의 조합이 진행 하기가 매우 보일 수 있습니다. 다음 팁 다양 한 장치를 지 원하는 데 필요한 노력을 최소화할 수 있습니다.

- **디자인 및 개발 하는 요구에 대 한만** &ndash; 다양 한 장치, 많이 있지만 일부 드문 폼 팩터를 디자인 하 고 개발에 대 한 본격적으로 걸리는에 존재 합니다. [ **화면 크기와 밀도** ](http://developer.android.com/resources/dashboard/screens.html) 대시보드는 화면 크기/화면 밀도 매트릭스의 분석 결과에 데이터를 제공 하는 Google에서 제공 하는 페이지입니다. 이 분석 결과 화면을 지 원하는 개발 과정이 필요 하는 방법에 대 한 정보를 제공 합니다.

- **픽셀 보다는 Dp 사용** -픽셀 화면 밀도 변경 될 때 문제가 됩니다. 하드 코딩 하지 마십시오. 픽셀 값입니다. Dp (밀도 독립적 픽셀)를 위해 픽셀을 하지 마십시오.

- **방지** [AbsoluteLayout](https://developer.xamarin.com/api/type/Android.Widget.AbsoluteLayout/)
  **아무 곳에 나 가능한** &ndash; API 수준 (Android 1.5) 3에서에서 사용 되지 않는 하 고 불안정 레이아웃에서 발생 합니다. 사용 해야 합니다. 대신,와 같은 보다 유연한 레이아웃 위젯을 사용 하려고 [ **LinearLayout**](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/), [ **RelativeLayout**](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/), 또는 새로운 [ **GridLayout**](https://developer.xamarin.com/api/type/Android.Widget.GridLayout/)합니다.

- **기본 레이아웃 방향을 선택** &ndash; 대체 리소스를 제공 하는 대신 예를 들어 **레이아웃 토지** 및 **레이아웃 포트**, 배치에 대 한 리소스 가로로 **레이아웃**, 및 리소스에 세로로 **레이아웃 포트**합니다.

- **LayoutParams를 사용 하 여 높이 및 너비에 대 한** -XML 레이아웃 파일로에서 UI 요소를 정의 하는 경우 사용 하 여 Android 응용 프로그램의 **wrap_content** 및 **fill_parent** 값 자세한 성공 기준이 있는 됩니다 픽셀 또는 밀도 독립적인 단위를 사용 하 여 보다 다양 한 장치의 적절 한 참조를 확인 합니다. 이러한 차원 값 Android 배율 비트맵 리소스를 적절 하 게 중단. 이와 동일한 이유로 밀도 독립적 단위 예약어 가장 시기에 대 한 여백을 지정 하 고 UI 요소의 안쪽 여백입니다.


## <a name="testing-multiple-screens"></a>여러 화면 테스트

Android 응용 프로그램에서 지원 되는 모든 구성에 대해 테스트 해야 합니다. 이상적으로 자체 실제 장치에서 장치를 테스트 해야 하지만 대부분의 경우에서 이것이 가능 하거나 적합할 합니다.
이 경우 에뮬레이터 및 Android 가상 장치 설정의 각 장치 구성에 대해 사용 하 여 도움이 됩니다.

Android SDK는, 밀도의 크기와 해상도 많은 장치 스킨 AVD의를 만드는 데 사용할 수는 몇 가지 에뮬레이터는 복제를 제공 합니다.
다양 한 하드웨어 공급 업체는 마찬가지로 자신의 장치에 대 한 스킨을 제공합니다.

두 번째 방법은 서비스를 테스트 하는 타사 서비스를 사용 하는 것입니다.
이러한 서비스는 APK 수행 됩니다, 그리고 다양 한 장치에서 실행 되 고 응용 프로그램이 작동 하는 방법을 피드백을 입력 하십시오.
