---
title: Android Wear
description: Android wearable 장치용 앱을 빌드하는 중입니다.
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/16/2018
ms.openlocfilehash: 5768a8fb402304d399e619ddb111aea24d975daf
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758325"
---
# <a name="android-wear"></a>Android Wear

Android 마모는 스마트 감시와 같은 wearable 장치용으로 설계 된 Android 버전입니다. 이 섹션에는 개발에 필요한 도구를 설치 하 고 구성 하는 방법, 첫 번째 마모 된 장치를 만들기 위한 단계별 연습 및 사용자 고유의 마모 앱을 만들기 위해 참조할 수 있는 샘플 목록이 포함 되어 있습니다.

## <a name="getting-startedandroidwearget-startedindexmd"></a>[시작](~/android/wear/get-started/index.md)

Android 마모를 소개 하 고, 개발을 위해 컴퓨터를 설치 및 구성 하는 방법에 대해 설명 하 고, 에뮬레이터 또는 마모 된 장치에서 첫 번째 Android 앱을 만들고 실행 하는 데 도움이 되는 단계를 제공 합니다.

## <a name="user-interfaceandroidwearuser-interfaceindexmd"></a>[사용자 인터페이스](~/android/wear/user-interface/index.md)

Android의 특정 컨트롤에 대해 설명 하 고 이러한 컨트롤을 사용 하는 방법을 보여 주는 샘플에 대 한 링크를 제공 합니다.

## <a name="platform-featuresandroidwearplatformindexmd"></a>[플랫폼 기능](~/android/wear/platform/index.md)

이 섹션의 문서에서는 Android 마모와 관련 한 기능을 다룹니다. 여기에서 WatchFace를 만드는 방법을 설명 하는 항목을 찾을 수 있습니다.

## <a name="screen-sizesandroidwearscreen-sizesmd"></a>[화면 크기](~/android/wear/screen-sizes.md)

사용 가능한 화면 크기에 맞게 사용자 인터페이스를 미리 보고 최적화 합니다.

## <a name="deployment--testingandroidweardeploy-testindexmd"></a>[배포 및 테스트](~/android/wear/deploy-test/index.md)

Android 앱을 android 장치 또는 android 용으로 구성 된 android 장치에 배포 하는 방법에 대해 설명 합니다. 또한 개발 컴퓨터와 Android 장치 간에 Bluetooth 연결을 설정 하는 방법에 대 한 디버깅 팁과 정보를 제공 합니다.

## <a name="wear-apishttpsdeveloperandroidcomreferenceandroidsupportwearable"></a>[마모 Api](https://developer.android.com/reference/android/support/wearable)

Android 개발자 사이트에서는 [Wearable 활동](https://developer.android.com/reference/android/support/wearable/activity/package-summary.html), [의도](https://developer.android.com/reference/com/google/android/wearable/intent/package-summary.html), [인증](https://developer.android.com/reference/android/support/wearable/authentication/package-summary.html), [복잡](https://developer.android.com/reference/android/support/wearable/complications/package-summary.html)한, [복잡 한 렌더링](https://developer.android.com/reference/android/support/wearable/complications/rendering/package-summary.html), [알림](https://developer.android.com/reference/android/support/wearable/notifications/package-summary.html)등과 같은 키 마모 api에 대 한 자세한 정보를 [제공 합니다. 보기](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)및 [WatchFace](https://developer.android.com/reference/android/support/wearable/watchface/package-summary.html).

## <a name="samples"></a>샘플

Android 마모를 사용 하 여 다양 한 [샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Android+wear) 을 찾을 수 있습니다 (또는 [github](https://github.com/xamarin/monodroid-samples/tree/master/wear)로 직접 이동).

|예제|Description|스크린 샷|
|--- |--- |--- |
|[SkeletonWear](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-skeletonwear)|GridViewPager 및 대화형 알림을 비롯 한 wearable 프로젝트의 기본 사항에 대 한 간단한 예입니다.|![Skeletonwear의 스크린샷](images/skeleton.png)|
|[WatchViewStub](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-watchviewstub)|화면 모양을 검색 하 고 올바른 레이아웃을 자동으로 로드 하는 WatchViewStub 컨트롤의 간단한 데모입니다. **Resources/layout/main_activity** 레이아웃에서 WatchViewStub이 작동 하는 방식을 확인 합니다.|![WatchViewStub의 스크린샷](images/watchview.png)|
|[RecipeAssistant](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-recipeassistant)|레 레 레 레 레 레 레 레 레 레 레 레 레 레 레 레 레 레 레 레 레 피가 RecipeService.cs에서 알림이 생성 됩니다.|![RecipeAssistant의 스크린샷](images/recipeassist.png)|
|[ElizaChat](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-elizachat)|마모 된 대화형 알림을 사용 하 여 Eliza 이라는 "개인 길잡이"와 상호 작용 하는 재미 있는 샘플을 통해 고정 응답을 사용 하 여 대화를 만듭니다.|![ElizaChat의 스크린샷](images/eliza.png)|
|[GridViewPager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-gridviewpager)|GridViewPager는 사용자가 세로 방향으로 swipes 다음 가로 방향으로 탐색 하 여 옵션 및 콘텐츠를 탐색 하는 2D 탐색 패턴을 구현 합니다.|![GridViewPager의 스크린샷](images/gridviewpager.png)|
|[WatchFace](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-watchface)|WatchFace는 아날로그 스타일의 시간, 분 및 초 바늘이 있는 사용자 지정 시청 얼굴입니다. 이 샘플에서는 현재 시간을 그리고 앰비언트 모드와 표시 유형 변경 이벤트를 처리 하는 조사식 얼굴 서비스를 만드는 방법을 보여 줍니다. 표준 시간대 변경을 수신 대기 하 고 그에 따라 시간을 자동으로 업데이트 하는 브로드캐스트 수신기가 포함 됩니다.|![WatchFace의 스크린샷](images/gridviewpager.png)|

## <a name="videos"></a>비디오

Xamarin Android를 지 원하는 다음 비디오 링크를 확인 하세요.

|Description|스크린 샷|
|--- |--- |
|[Android L](https://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/) 등 &ndash; Android L Developer Preview에는 재질 디자인, 알림, 새 애니메이션 등을 활용 하는 개발자를 위한 새로운 api 다양 한 도입 되었습니다.|![프레젠테이션 비디오 스크린샷](images/video-android-l.png)|
|[C#내 귀와 내 눈에 있습니다. Google 유리 및 Android](https://www.youtube.com/watch?v=80H8tXByZQc) &ndash; Wearable 컴퓨팅은 미래 (또는 검사기 가젯 에피소드)와 같은 것 처럼 보일 수 있지만, 많은 사람들이 이미 미래를 수용 하 고 있습니다. C#개발자는이를 알고 있으며, wearable 장치의 강력한 기능을 활용 하는 도구와 기술을 이미 갖추고 있습니다 (2014).|![프레젠테이션 비디오 스크린샷](images/video-eyes-ears.png)|
|[Xamarin Android의 새로운 기능](https://www.youtube.com/watch?v=Gpqc2XZIQfU) &ndash; Android L, android 마모, android TV, android 자동, 자재 디자인 및 아트, Xamarin developer로 무엇을 의미 하나요? 2014.|![프레젠테이션 비디오 스크린샷](Images/video-whats-new.png)|

<!--

March 18
https://blog.xamarin.com/android-wear/

August 14
https://blog.xamarin.com/android-l-developer-preview-android-wear-support/

August 27
https://blog.xamarin.com/tips-for-your-first-android-wear-app/

Watch Face
https://github.com/Redth/Xamarin.Wear.WatchFace
-->
