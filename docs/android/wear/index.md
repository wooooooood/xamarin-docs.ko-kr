---
title: Android Wear
description: Android 착용 식 장치용 앱을 빌드합니다.
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/16/2018
ms.openlocfilehash: dda00760399572d714300f1487391212c6fa0998
ms.sourcegitcommit: d3f48bfe72bfe03aca247d47bc64bfbfad1d8071
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66740858"
---
# <a name="android-wear"></a>Android Wear

Android Wear 착용 식 장치 스마트 감시 등을 위해 설계 된 Android 버전이 있습니다. 이 섹션에서는 Wear 개발, 첫 번째 Wear 장치 및 사용자 고유의 작성용 Wear 앱을 참조할 수 있는 샘플 목록을 만들기 위한 단계별 연습에 필요한 도구 설치 및 구성 하는 방법에 대 한 지침을 포함 합니다.

##  <a name="getting-startedandroidwearget-startedindexmd"></a>[시작](~/android/wear/get-started/index.md)

Android Wear 소개, 설치 및 Wear 개발을 위해 컴퓨터를 구성 하는 방법을 설명 하 고 만들고 에뮬레이터 또는 Wear 장치에서 첫 번째 Android Wear 앱을 실행할 수 있도록 하는 단계를 제공 합니다.

##  <a name="user-interfaceandroidwearuser-interfaceindexmd"></a>[사용자 인터페이스](~/android/wear/user-interface/index.md)

Android Wear 관련 제어 하 고 이러한 컨트롤을 사용 하는 방법을 보여 주는 샘플에 대 한 링크를 제공에 대해 설명 합니다.

##  <a name="platform-featuresandroidwearplatformindexmd"></a>[플랫폼 기능](~/android/wear/platform/index.md)

이 섹션의 문서 Android Wear 관련 된 기능을 설명합니다. 여기서는 WatchFace를 만드는 방법을 설명 하는 항목을 찾을 수 있습니다.

##  <a name="screen-sizesandroidwearscreen-sizesmd"></a>[화면 크기](~/android/wear/screen-sizes.md)

미리 보기 및 사용 가능한 화면 크기에 대 한 사용자 인터페이스를 최적화 합니다.

##  <a name="deployment--testingandroidweardeploy-testindexmd"></a>[배포 및 테스트](~/android/wear/deploy-test/index.md)

Wear에 대해 구성 하는 Android 에뮬레이터 또는 Android Wear 장치를 Android Wear 앱을 배포 하는 방법에 설명 합니다. 디버깅 팁 및 개발 컴퓨터에서 Android 장치 사이의 Bluetooth 연결을 설정 하는 방법에 대 한 정보가 포함 됩니다.

##  <a name="wear-apishttpsdeveloperandroidcomreferenceandroidsupportwearable"></a>[Wear Api](https://developer.android.com/reference/android/support/wearable)

Android 개발자 사이트와 같은 주요 Wear Api에 대 한 자세한 정보를 제공 합니다 [착용 식 활동](https://developer.android.com/reference/android/support/wearable/activity/package-summary.html), [의도](https://developer.android.com/reference/com/google/android/wearable/intent/package-summary.html)를 [Authentication](https://developer.android.com/reference/android/support/wearable/authentication/package-summary.html), [ 복잡성](https://developer.android.com/reference/android/support/wearable/complications/package-summary.html), [렌더링 복잡성](https://developer.android.com/reference/android/support/wearable/complications/rendering/package-summary.html), [알림을](https://developer.android.com/reference/android/support/wearable/notifications/package-summary.html)를 [뷰](https://developer.android.com/reference/android/support/wearable/view/package-summary.html), 및 [WatchFace](https://developer.android.com/reference/android/support/wearable/watchface/package-summary.html)합니다.



## <a name="samples"></a>샘플

숫자를 찾을 수 있습니다 [샘플](https://developer.xamarin.com/samples/android/Android%20Wear/) Android Wear를 사용 하 여 (하거나 직접 이동할 [github](https://github.com/xamarin/monodroid-samples/tree/master/wear)).

|샘플|설명|스크린 샷|
|--- |--- |--- |
|[SkeletonWear](https://developer.xamarin.com/samples/monodroid/wear/SkeletonWear/)|간단한 예 GridViewPager 및 대화형 알림을 비롯 한 착용 식 프로젝트의 기본 사항입니다.|![Skeletonwear 스크린샷](images/skeleton.png)|
|[WatchViewStub](https://developer.xamarin.com/samples/monodroid/wear/WatchViewStub/)|화면 셰이프를 검색 하 고 올바른 레이아웃을 자동으로 로드 WatchViewStub 컨트롤의 간단한 데모입니다. WatchViewStub 작동 하는 방식을 확인 합니다 **Resources/layout/main_activity.xml** 레이아웃 합니다.|![WatchViewStub 스크린샷](images/watchview.png)|
|[RecipeAssistant](https://developer.xamarin.com/samples/monodroid/wear/RecipeAssistant/)|작성법 단계의 형태로 Wear 알림 페이지의 데모입니다. 알림은 RecipeService.cs에 생성 됩니다.|![RecipeAssistant 스크린샷](images/recipeassist.png)|
|[ElizaChat](https://developer.xamarin.com/samples/monodroid/wear/ElizaChat/)|"개인 비서" 상호 작용의 재미 있게 샘플 Eliza, 미리 준비 된 응답을 사용 하 여 대화를 만드는 Wear 대화형 알림을 사용 하 여 호출 됩니다.|![ElizaChat 스크린샷](images/eliza.png)|
|[GridViewPager](https://developer.xamarin.com/samples/monodroid/wear/GridViewPager/)|GridViewPager 여기서 사용자 천공 기와 세로로 2D 탐색 패턴을 구현 하 고 옵션 및 콘텐츠 탐색을 가로로 합니다.|![GridViewPager 스크린샷](images/gridviewpager.png)|
|[WatchFace](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)|WatchFace는 아날로그 방식의 시간, 분 및 두 번째 실습을 사용 하 여 사용자 지정 시계 모드 이며 이 샘플 현재 그리는 조사식 얼굴 서비스를 만드는 방법과 핸들 앰비언트 모드 및 표시 유형 변경 이벤트를 보여 줍니다. 표준 시간대 변경 내용을 수신 대기 하 고 자동으로 적절 하 게 시간을 업데이트 하는 브로드캐스트 수신기가 포함 됩니다.|![WatchFace 스크린샷](images/gridviewpager.png)|


##  <a name="videos"></a>비디오

체크 아웃 하는 이러한 비디오 Wear 사용 하 여 Xamarin.Android를 설명 하는 링크는 지원:

|설명|스크린 샷|
|--- |--- |
|[Android L 및 훨씬](https://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/) &ndash; Android L 개발자 미리 재료 디자인, 알림 및 새 애니메이션 등을 비롯 한 다양 한 새로운 Api 활용 하는 개발자를 위한 도입 합니다.|![프레젠테이션 비디오 스크린샷](images/video-android-l.png)|
|[C#내 귀에 눈 같습니다. Google 투명 하 고 Android Wear](https://www.youtube.com/watch?v=80H8tXByZQc) &ndash; 미래 (또는 검사기 가젯 에피소드가)에서 것 처럼 보일 수 착용 식 계산 되지만 많은 사람들이 이미 수용 하는 미래 지금! C#개발자가이 사실을 잘 알고 있고 이미 도구 및 기술을 착용 식 장치 (진화 2014)에서 기능을 활용할 수 있습니다.|![프레젠테이션 비디오 스크린샷](images/video-eyes-ears.png)|
|[Xamarin.Android의 새로운 기능](https://www.youtube.com/watch?v=Gpqc2XZIQfU) &ndash; Android L, Android Wear, Android TV, Android Auto, 재질 디자인 및 아트;이 용도 Xamarin 개발자로 서 의미? 진화 2014에서.|![프레젠테이션 비디오 스크린샷](Images/video-whats-new.png)|


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
