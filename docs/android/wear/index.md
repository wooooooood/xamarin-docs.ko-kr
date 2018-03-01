---
title: "Android 마모"
description: "Android 착용 식 장치에 대 한 응용 프로그램을 구축 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1dad5e859efdf69e7003b45724f718b16faffd62
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="android-wear"></a>Android 마모

## <a name="android-wear"></a>Android 마모

Android 마모는 android 착용 식 장치 스마트 감시 등을 위해 디자인 버전입니다. 이 섹션에서는 설치 하 고 직접 만들기 위한 앱 착용를 참조할 수 있는 샘플의 목록과 첫 번째 마모 장치를 만들기 위한 단계별 연습, 마모 개발을 위해 필요한 도구를 구성 하는 방법에 대 한 지침입니다.

##  <a name="getting-startedandroidwearget-startedindexmd"></a>[시작](~/android/wear/get-started/index.md)

Android 착용 소개, 설치 하 고, 마모 개발을 위해 컴퓨터를 구성 하는 방법을 설명 하 고 만들고 에뮬레이터 또는 마모 장치에서 첫 번째 쓰는 유형 Android 앱을 실행 하는 단계를 제공 합니다.

##  <a name="user-interfaceandroidwearuser-interfaceindexmd"></a>[사용자 인터페이스](~/android/wear/user-interface/index.md)

제어 하 고 이러한 컨트롤을 사용 하는 방법을 보여 주는 샘플에 대 한 링크를 제공 하는 Android 마모 관련에 설명 합니다.

##  <a name="platform-featuresandroidwearplatformindexmd"></a>[플랫폼 기능](~/android/wear/platform/index.md)

이 섹션의 문서를 쓰는 유형 Android에 대 한 기능을 다룹니다. 여기는 WatchFace를 만드는 방법을 설명 하는 항목을 찾을 수 있습니다.

##  <a name="screen-sizesandroidwearscreen-sizesmd"></a>[한 화면 크기](~/android/wear/screen-sizes.md)

미리 보기 및 사용 가능한 화면 크기에 대 한 사용자 인터페이스를 최적화 합니다.

##  <a name="deployment--testingandroidweardeploy-testindexmd"></a>[배포 및 테스트](~/android/wear/deploy-test/index.md)

마모에 대해 구성 된 Android 에뮬레이터 또는 Android 착용 장치에 쓰는 유형 Android 앱을 배포 하는 방법에 설명 합니다. 디버깅 팁 및 개발 컴퓨터 및 Android 장치 간에 Bluetooth 연결을 설정 하는 방법에 대 한 정보가 포함 됩니다.


<a name="Samples" />

## <a name="samples"></a>샘플

다양 한를 찾을 수 [샘플](https://developer.xamarin.com/samples/android/Android%20Wear/) Android 착용를 사용 하 여 (또는 직접 이동할 [github](https://github.com/xamarin/monodroid-samples/tree/master/wear)). 

<table align="center" border="1" cellpadding="1" cellspacing="1">
  <thead>
      <th>
          <strong>샘플</strong>
      </th>
      <th>
          <strong>설명</strong>
      </th>
      <th>
          <strong>스크린 샷</strong>
      </th>
  </thead>
  <tbody>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/SkeletonWear/">SkeletonWear</a>
      </td>
      <td valign="top">
간단한 예제 GridViewPager 및 대화형 알림 포함 착용 식 프로젝트의 기본 사항입니다.
      </td>
      <td>
          <img src="Images/skeleton.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/WatchViewStub/">WatchViewStub</a>
      </td>
      <td valign="top">
화면 모양을 감지 하 고 올바른 레이아웃을 자동으로 로드 하는 WatchViewStub 컨트롤의 간단한 데모입니다.
WatchViewStub에서 작동 하는 방법을 참조는 <b>Resources/layout/main_actvity.xml</b> 레이아웃 합니다.
      </td>
      <td>
          <img src="Images/watchview.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/RecipeAssistant/">RecipeAssistant</a>
      </td>
      <td valign="top">
레시피 단계의 형태로 마모 알림 페이지의 데모를 제공 합니다. 알림이 생성 된 <b>RecipeService.cs</b>합니다.
      </td>
      <td>
          <img src="Images/recipeassist.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/ElizaChat/">ElizaChat</a>
      </td>
      <td valign="top">
"개인 비서"와 상호 작용 재미 샘플 마모 대화형 알림을 사용 하 여 미리 준비 된 구체적인된 응답을 사용 하는 대화를 만드는 Eliza 호출 됩니다.
      </td>
      <td>
          <img src="Images/eliza.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/GridViewPager/">GridViewPager</a>
      </td>
      <td valign="top">
여기서 사용자 천공 기와 세로로 2D 탐색 패턴을 구현 하는 GridViewPager 다음 가로로 옵션 및 콘텐츠를 탐색 하 합니다.
      </td>
      <td>
          <img src="Images/gridviewpager.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/monodroid/wear/WatchFace">WatchFace</a>
      </td>
      <td valign="top">
          <b>WatchFace</b> 아날로그 스타일 시간, 분 및 두 번째 포인터를 사용 하 여 사용자 지정 조사식 얼굴 됩니다. 이 샘플에서는 현재 시간 가져오는 조사식 얼굴 서비스를 만드는 방법과 핸들 앰비언트 모드 및 표시 유형 변경 이벤트를 보여 줍니다. 표준 시간대 변경 내용을 수신 대기 하 고 자동으로 시간을 적절 하 게 업데이트 하는 브로드캐스트 수신기를 포함 합니다.
      </td>
      <td>
          <img src="Images/watchface.png" class="tableimg">
      </td>
  </tr>
  </tbody>
</table>

##  <a name="videos"></a>비디오

체크 아웃 비디오 이러한 Xamarin.Android 마모도 설명 하는 링크를 지원 합니다.

<table align="center" border="0" cellpadding="1" cellspacing="1">
    <tr>
        <td>
        <a href="http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/"><img src="Images/video-android-l.png" border="0"/ /></td>
        <td><a href="http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/">Android L 및 더 많은</a>
        <br />
Android L 개발자 미리 보기 자료 디자인, 알림 및 새 애니메이션 등을 포함 하는 새로운 Api 활용 하기 위해 개발자를 위한 다양 한을 도입 되었습니다.</td>
    </tr>
    <tr>
        <td>
        <a href="https://www.youtube.com/watch?v=80H8tXByZQc"><img src="Images/video-eyes-ears.png" border="0" / /></td>
        <td><a href="https://www.youtube.com/watch?v=80H8tXByZQc">C#이 내 귀 및 눈에: Google 유리 및 Android 쓰는 유형</a>
        <br />
미래 (또는 검사기 가젯 에피소드)에서 것 처럼 보이기는 착용 식 계산 하지만 많은 사람들이 미래 오늘 디렉터리가 이미! C# 개발자 이러한 내용을 알아야 하 고 있는 도구와 기술을 활용 하 여 (Evolve 2014)에서 착용 식 장치.</td>
    </tr>
    <tr>
        <td>
        <a href="https://www.youtube.com/watch?v=Gpqc2XZIQfU"><img src="Images/video-whats-new.png" border="0" / /></td>
        <td><a href="https://www.youtube.com/watch?v=Gpqc2XZIQfU">Xamarin.Android의 새로운 기능</a>
        <br />
        <i>Android L, Android 마모, Android TV, Android 자동, 자재 디자인 및 아트; 그에 Xamarin 개발자? </i> 에서 2014 발전 합니다.</td>
    </tr>
</table>


<!--

March 18
http://blog.xamarin.com/android-wear/

August 14
http://blog.xamarin.com/android-l-developer-preview-android-wear-support/

August 27
http://blog.xamarin.com/tips-for-your-first-android-wear-app/

Watch Face
https://github.com/Redth/Xamarin.Wear.WatchFace
-->
