---
title: "수동으로 APK 업로드"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1309C251-ABF0-4412-B1F5-200DC8321A9D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 37e38ddd84b50709bec147c54cdfa9f79404a39f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="manually-uploading-the-apk"></a>수동으로 APK 업로드


APK가 Google Play에 처음으로 제출될 경우(또는 초기 버전의 Xamarin.Android를 사용할 경우) [Google Play 개발자 콘솔](https://play.google.com/apps/publish)을 통해 수동으로 APK를 업로드해야 합니다. 이 가이드에서는 이 프로세스에 필요한 단계를 설명합니다. 


## <a name="google-play-developer-console"></a>Google Play 개발자 콘솔

APK가 컴파일되고프로모션 자산이 준비된 후에는 응용 프로그램을 Google Play에 업로드해야 합니다. 이 작업은 다음 그림의 [Google Play 개발자 콘솔](https://play.google.com/apps/publish)에 로그인하여 이루어집니다. **Google Play에 Android 앱 게시** 단추를 클릭하여 응용 프로그램을 배포 프로세스를 초기화합니다.

[![Google Play 개발자 콘솔](manually-uploading-the-apk-images/00-google-play-developer-console-sml.png)](manually-uploading-the-apk-images/00-google-play-developer-console.png#lightbox)

Google Play에 등록한 기존 앱이 있는 경우 **새 응용 프로그램 추가** 단추를 클릭합니다.

[![새 응용 프로그램 추가 단추](manually-uploading-the-apk-images/01-existing-app-sml.png)](manually-uploading-the-apk-images/01-existing-app.png#lightbox)

**새 응용 프로그램 추가** 대화 상자가 표시되면 앱 이름을 입력하고 **APK 업로드**를 클릭합니다.

[![APK 업로드 단추](manually-uploading-the-apk-images/02-add-new-application-sml.png)](manually-uploading-the-apk-images/02-add-new-application.png#lightbox)

다음 화면에서 알파 테스트, 베타 테스트 또는 프로덕션용으로 앱을 게시할 수 있습니다. 다음 예제에서는 **알파 테스트** 탭을 선택합니다. **MyApp**이 라이선스 서비스를 사용하지 않으므로 이 예제에서는 **라이선스 키 가져오기** 단추를 클릭하지 않아도 됩니다. 여기서는 **알파에 첫 번째 APK 업로드** 단추를 클릭하여 알파 채널에 게시합니다.

[![알파에 첫 번째 APK 업로드](manually-uploading-the-apk-images/03-upload-to-alpha-sml.png)](manually-uploading-the-apk-images/03-upload-to-alpha.png#lightbox)

**ALPHA에 새 APK 업로드** 대화 상자가 표시됩니다. APK는 **파일 찾아보기** 단추를 클릭하거나 APK를 끌어서 놓아 업로드할 수 있습니다. 

[![ALPHA에 새 APK 업로드 대화 상자](manually-uploading-the-apk-images/04-upload-dialog-sml.png)](manually-uploading-the-apk-images/04-upload-dialog.png#lightbox)

릴리스 준비가 된 APK를 배포 대상으로 업로드해야 합니다.
다음 대화 상자에는 APK 업로드의 진행률이 표시됩니다.

[![업로드 진행률 표시](manually-uploading-the-apk-images/05-upload-progress-sml.png)](manually-uploading-the-apk-images/05-upload-progress.png#lightbox)

APK를 업로드한 후에 테스트 메서드를 선택할 수 있습니다.

[![테스트 메서드 선택 대화 상자](manually-uploading-the-apk-images/06-select-testing-method-sml.png)](manually-uploading-the-apk-images/06-select-testing-method.png#lightbox)

앱 테스트에 대한 자세한 내용은 참조 Google의 [알파/베타 테스트 설정](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en) 가이드를 참조하세요.

APK를 업로드한 후에는 초안으로 저장됩니다. 다음에서 설명한 것처럼 추가 세부 정보가 Google Play에 제공되기 전까지는 게시할 수 없습니다.


## <a name="store-listing"></a>스토어 목록

**Google Play 개발자 콘솔**에서 **스토어 목록**을 클릭하여 Google Play가 해당 응용 프로그램의 미래 사용자에게 표시할 정보를 입력합니다. 

[![스토어 목록 대화 상자](manually-uploading-the-apk-images/07-store-listing-sml.png)](manually-uploading-the-apk-images/07-store-listing.png#lightbox)


### <a name="graphics-assets"></a>그래픽 자산

**스토어 목록** 페이지의 **그래픽 자산** 섹션까지 아래로 스크롤합니다.

[![그래픽 자산 섹션](manually-uploading-the-apk-images/08-graphic-assets-sml.png)](manually-uploading-the-apk-images/08-graphic-assets.png#lightbox)

앞에서 준비된 모든 프로모션 자산이 이 섹션에서 업로드됩니다. 어떤 프로모션 자산을 어떤 형식으로 제공해야 하는지에 관한 지침이 제공됩니다.


### <a name="categorization"></a>분류

**그래픽 자산** 섹션이 **분류** 섹션이 되면 응용 프로그램 종류와 범주를 선택합니다.

[![분류 섹션](manually-uploading-the-apk-images/09-categorization-sml.png)](manually-uploading-the-apk-images/09-categorization.png#lightbox)

콘텐츠 등급은 다음 섹션 뒤에서 다룹니다.


### <a name="contact-details"></a>연락처 세부 정보

이 페이지의 마지막 섹션은 **연락처 세부 정보** 섹션입니다. 이 섹션을 사용하여 응용 프로그램 개발자에 관한 연락처 정보를 수집합니다.

[![연락처 세부 정보 섹션](manually-uploading-the-apk-images/10-contact-details-sml.png)](manually-uploading-the-apk-images/10-contact-details.png#lightbox)

앞에서 표시한 것처럼 **개인 정보 취급 방침** 섹션에서 앱의 개인 정보 취급 방침 정책에 대한 URL을 제공할 수 있습니다.


## <a name="content-rating"></a>콘텐츠 등급

**Google Play 개발자 콘솔**에서 **콘텐츠 등급**을 클릭합니다. 이 페이지에서는 앱의 콘텐츠 등급을 지정합니다. Google Play에서는 모든 응용 프로그램의 콘텐츠 등급 지정을 요구합니다. **계속** 단추를 클릭하여 콘텐츠 등급 질문을 완료합니다.

[![콘텐츠 등급 섹션](manually-uploading-the-apk-images/11-content-rating-sml.png)](manually-uploading-the-apk-images/11-content-rating.png#lightbox)

Google Play의 모든 응용 프로그램은 Google Play 등급 시스템에 따라 등급을 지정해야 합니다. 콘텐츠 등급 외에도 모든 응용 프로그램은 Google의 [개발자 콘텐츠 정책](http://www.android.com/us/developer-content-policy.html)을 준수해야 합니다.

다음은 Google Play 등급 시스템의 4수준을 나열하고 등급 수준을 요구 또는 강제 적용하는 기능이나 콘텐츠에 관한 몇 가지 지침을 제공합니다. 

-   **모든 사용자** &ndash; 위치 데이터를 액세스, 게시 또는 공유하지 못합니다. 사용자가 생성한 콘텐츠를 호스트할 수 없습니다. 사용자 간의 커뮤니케이션을 사용하도록 설정할 수 없습니다. 

-   **하** &ndash; 위치 데이터를 액세스하지만 공유하지 않는 응용 프로그램입니다. 가볍거나 만화 수준의 폭력 묘사 

-   **중** &ndash; 마약, 주류 또는 담배에 대 한 언급 도박 테마 또는 시뮬레이션 적의가 있는 콘텐츠 불경스럽거나 외설적인 유머  도발적 또는 성적 언급 
    강한 수준의 판타지 폭력  현실적인 폭력  사용자가 서로 찾을 수 있음 사용자가 서로 커뮤니케이션할 수 있음 
    사용자 위치 데이터 공유 

-   **상** &ndash; 주류, 담배 또는 약물 사용 도는 판매에 초점 도발적 또는 성적 언급에 초점 그래픽 폭력  

중 목록의 항목은 주관적인 판단이 필요하므로, 중 등급에 해당하는 것처럼 보이는 지침이 상 등급 만큼 강도가 있을 수 있습니다. 


## <a name="pricing-amp-distribution"></a>&amp; 배포 가격 책정

**Google Play 개발자 콘솔**에서 **가격 책정 및 배포**를 클릭합니다. 앱이 유료 앱이면 이 페이지에서 가격을 설정합니다.
또는 응용 프로그램을 모든 사용자에게 무료로 배포할 수 있습니다. 응용 프로그램을 무료로 지정하면 계속 무료여야 합니다.
Google Play에서는 무료 응용 프로그램이 유료 앱으로 변경되는 것을 허용하지 않습니다(그러나 무료 앱에서 앱 내 청구로 콘텐츠 판매는 가능). Google Play에서 유료 앱은 언제든 무료로 변경할 수 있습니다.

유료 앱을 게시하려면 먼저 판매자 계정이 필요합니다. 이를 위해 **판매자 계정 설정**을 클릭하고 지시에 따릅니다.

[![가격 및 배포 대화 상자](manually-uploading-the-apk-images/12-pricing-sml.png)](manually-uploading-the-apk-images/12-pricing.png#lightbox)


### <a name="manage-countries"></a>국가 관리 

다음 섹션 **국가 관리**에서는 앱을 배포할 수 있는 국가를 제어합니다.

[![국가 관리 대화 상자](manually-uploading-the-apk-images/13-manage-countries-sml.png)](manually-uploading-the-apk-images/13-manage-countries.png#lightbox)


### <a name="other-information"></a>기타 정보

더 아래로 스크롤하여 앱의 광고 포함 여부를 지정합니다. 또한 **장치 범주** 섹션에서는 선택적으로 앱을 Android Wear, Android TV 또는 Android Auto에 배포하는 옵션을 제공합니다.

[![광고 포함 섹션](manually-uploading-the-apk-images/14-contains-ads-sml.png)](manually-uploading-the-apk-images/14-contains-ads.png#lightbox)

이 섹션 다음에는 **가족용으로 지정**, 교육용 Google Play를 통해 배포 등을 선택할 수 있는 추가 옵션이 있습니다.


### <a name="consent"></a>동의

**가격 책정 &amp; 배포** 페이지의 하단은 **동의** 섹션입니다.
필수 섹션이며, 응용 프로그램이 [Android 콘텐츠 지침](http://www.android.com/market/terms/developer-content-policy.html#hl=us)에 부합하고 미국 수출 법률의 적용 대상임을 인정하는 데 사용됩니다.

[![동의 섹션](manually-uploading-the-apk-images/15-consent-sml.png)](manually-uploading-the-apk-images/15-consent.png#lightbox)

Xamarin.Android 앱 게시에는 이 가이드에서 다루는 내용보다 훨씬 더 많은 사항을 고려해야 합니다.
Google Play 앱 게시에 대한 자세한 내용은 [Google Play 개발자 콘솔 도움말 센터 시작](https://support.google.com/googleplay/android-developer#topic=3450769)을 참조하세요.



## <a name="google-play-filters"></a>Google Play 필터

사용자가 Google Play 웹 사이트에서 응용 프로그램을 탐색할 때는 모든 게시된 응용 프로그램을 검색할 수 있습니다. 사용자가 Google Play에서 Android 장치를 찾아보면 결과가 다소 차이가 있습니다. 결과는 사용 중인 장치와의 호환성에 따라 필터링됩니다. 예를 들어 응용 프로그램에서 SMS 메시지를 보내야 할 경우 Google Play는 SMS 메시지를 보낼 수 없는 장치에는 응용 프로그램을 표시하지 않습니다. 검색에 적용된 필터는 다음을 통해 만들어집니다.

1.  장치의 하드웨어 구성
2.  응용 프로그램 매니페스트 파일의 선언
3.  사용되는 이동 통신 사업자(있는 경우)
4.  장치 위치

Google Play 스토어에서 앱이 필터링되는 방식을 제어하기 위해 앱의 매니페스트에 요소를 추가할 수 있습니다. 다음은 응용 프로그램 필터링에 사용할 수 있는 매니페스트 요소 및 특성을 나열합니다.

-   [supports-screen](http://developer.android.com/guide/topics/manifest/supports-screens-element.html) &ndash; Google Play는 이 특성을 사용하여 화면 크기를 기준으로 응용 프로그램을 장치에 배포할 수 있는지 판단합니다. 
    Google Play에서는 Android가 더 작은 레이아웃에서 더 큰 레이아웃으로는 조절할 수 있지만 그 반대로는 조절할 수 없다고 가정합니다. 따라서 일반 화면을 지원한다고 밝힌 응용 프로그램은 대형 화면 검색에는 표시되나 소형 화면에는 표시되지 않습니다. Xamarin.Android 응용 프로그램이 매니페스트 파일에서 `<supports-screen>` 요소를 제공하지 않을 경우 Google Play는 모든 특성의 값이 참이며 응용 프로그램이 모든 화면 크기를 지원하다고 가정합니다. 이 요소는 **AndroidManifest.xml**에 수동으로 추가해야 합니다. 

-   [uses-configuration](http://developer.android.com/guide/topics/manifest/uses-configuration-element.html) &ndash; 이 매니페스트 요소는 키보드 형식, 탐색 장치, 터치 스크린 등, 특정 하드웨어 기능을 요청하는 데 사용됩니다. 이 요소는 **AndroidManifest.xml**에 수동으로 추가해야 합니다. 

-   [uses-feature](http://developer.android.com/guide/topics/manifest/uses-feature-element.html) &ndash; 이 매니페스트 요소는 응용 프로그램이 작동하기 위해 장치에 필요한 하드웨어 또는 소프트웨어 기능을 선언합니다. 이 특성은 정보 제공에만 해당합니다. Google Play에서는 이 필터에 부합하지 않는 응용 프로그램을 장치에 표시하지 않습니다. 다른 방법(수동 또는 다운로드)을 통해 응용 프로그램을 설치할 수는 있습니다. 이 요소는 **AndroidManifest.xml**에 수동으로 추가해야 합니다. 

-   [uses-library](http://developer.android.com/guide/topics/manifest/uses-library-element.html) &ndash; 이 요소는 Google Maps 등, 장치에 표시해야 하는 특정 공유 라이브러리를 지정합니다. 이 요소는 `Android.App.UsesLibaryAttribute`를 통해서도 지정할 수 있습니다. 예: 

    ```csharp
    [assembly: UsesLibrary("com.google.android.maps", true)]
    ```

-   [uses-permission](http://developer.android.com/guide/topics/manifest/uses-permission-element.html) &ndash; 이 요소는 `<uses-feature>` 요소에서 제대로 선언되지 않았을 수 있으며 응용 프로그램의 실행에 필요한 특정 하드웨어 기능을 유추하는 데 사용됩니다. 예를 들어, 응용 프로그램이 카메라 사용 권한을 요청할 경우 Google Play는 카메라를 선언하는 `<uses-feature>` 요소가 없더라도 해당 장치에 카메라가 있다고 가정합니다. 이 요소는 `Android.App.UsesPermissionsAttribute`를 통해서 설정할 수 있습니다. 예: 

    ```csharp
    [assembly: UsesPermission(Manifest.Permission.Camera)]
    ```

-   [uses-sdk](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html) &ndash; 이 요소는 응용 프로그램에 필요한 최소한의 Android API 수준을 선언하는 데 사용됩니다. 이 요소는 Xamarin.Android 프로젝트의 Xamarin.Android 옵션에서 설정할 수 있습니다. 

-   [compatible-screens](http://developer.android.com/guide/topics/manifest/compatible-screens-element.html) &ndash; 이 요소는 이 요소에서 지정한 화면 크기와 밀도에 부합하지 않는 응용 프로그램을 필터링하는 데 사용됩니다. 대부분의 응용 프로그램에서는 이 필터를 사용하면 안 됩니다. 응용 프로그램 배포에 엄격한 제어가 필요한 특정 고성능 게임이나 응용 프로그램에 적용됩니다. 앞에서 언급한 `<support-screen>` 특성을 사용하는 것이 좋습니다. 

-   [supports-gl-texture](http://developer.android.com/guide/topics/manifest/supports-gl-texture-element.html) &ndash; 이 요소는 응용 프로그램이 요구하는 GL 질감 압축 형식을 선언하는 데 사용됩니다. 대부분의 응용 프로그램에서는 이 필터를 사용하면 안 됩니다. 응용 프로그램 배포에 엄격한 제어가 필요한 특정 고성능 게임이나 응용 프로그램에 적용됩니다. 

앱 매니페스트 구성에 대한 자세한 내용은 Android [앱 매니페스트](https://developer.android.com/guide/topics/manifest/manifest-intro.html) 항목을 참조하세요.
