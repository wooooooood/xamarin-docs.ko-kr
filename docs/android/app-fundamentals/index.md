---
title: Xamarin.Android 응용 프로그램 기본 사항
description: 응용 프로그램의 핵심 개념
ms.prod: xamarin
ms.assetid: 935B8BFE-23B7-4239-5C87-F4A503B889CB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 1d3bd071eeffb77f94a1b5f35f1df59f2d8c7a8a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105560"
---
# <a name="xamarinandroid-application-fundamentals"></a>Xamarin.Android 응용 프로그램 기본 사항

이 섹션에서는 일반적인 작업 작업 또는 개발자가 Android 응용 프로그램을 개발할 때 주의 해야 하는 개념 중 일부에 제공 합니다.

## <a name="accessibilityandroidapp-fundamentalsaccessibilitymd"></a>[액세스 가능성](~/android/app-fundamentals/accessibility.md)

이 페이지에는 Api를 사용 하 여 Android 내게 필요한 옵션에 따라 앱을 빌드하는 방법을 설명 합니다 [접근성 검사 목록](~/cross-platform/app-fundamentals/accessibility.md)합니다.

##  <a name="understanding-android-api-levelsandroidapp-fundamentalsandroid-api-levelsmd"></a>[Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)

이 가이드에서는 서로 다른 버전의 Android에서 응용 프로그램 호환성을 관리 하기 위해 Android API 수준을 사용 하는 방법을 설명 하 고 앱에서 이러한 API 수준 배포를 Xamarin.Android 프로젝트 설정을 구성 하는 방법을 설명 합니다. 또한이 가이드에서는 다른 API 수준에서 처리 하는 런타임 코드를 작성 하는 방법을 설명 하 고 모든 Android API 수준, 버전 번호 (예: Android 8.0), 코드 이름 (예: Oreo), Android 및 빌드 버전 코드의 참조 목록을 제공 합니다.



##  <a name="resources-in-androidandroidapp-fundamentalsresources-in-androidindexmd"></a>[Android 리소스](~/android/app-fundamentals/resources-in-android/index.md)

이 문서에서는 사용 하는 방법 Xamarin.Android 및 문서에 Android 리소스의 개념을 소개 합니다. 응용 프로그램 지역화 및 다양 한 화면 크기 및 밀도 비롯 한 여러 장치를 지원 하도록 Android 응용 프로그램에서 리소스를 사용 하는 방법을 설명 합니다.




##  <a name="activity-lifecycleandroidapp-fundamentalsactivity-lifecycleindexmd"></a>[작업 수명 주기](~/android/app-fundamentals/activity-lifecycle/index.md)

활동은 Android 응용 프로그램의 기본 빌딩 블록 및 여러 다른 상태에에서 존재할 수 있습니다. 작업 수명 주기 인스턴스화를 사용 하 여 시작 및 소멸을 사용 하 여 종료 사이 많은 상태가 포함 되어 있습니다. 활동 상태 변경 되 면 임박한 상태 변경의 활동에 알리고 해당 변경에 맞게 코드를 실행할 수 있도록 적절 한 수명 주기 이벤트 메서드가 호출 됩니다. 활동의 수명 주기를 검사 하 고 책임에 설명 하는이 문서는 올바르게 동작 하 고 안정적인 응용 프로그램의 일부로 이러한 상태 변화 하는 동안 활동에 합니다.

##  <a name="localizationandroidapp-fundamentalslocalizationmd"></a>[지역화](~/android/app-fundamentals/localization.md)

이 문서에서는 Xamarin.Android를 문자열을 번역 하 고 대체 이미지를 제공 하 여 다른 언어로 지역화 하는 방법에 설명 합니다.

## <a name="servicesandroidapp-fundamentalsservicesindexmd"></a>[서비스](~/android/app-fundamentals/services/index.md)

이 문서에서는 Android 서비스는 백그라운드에서 수행 될 작업을 허용 하는 Android 구성 요소를 설명 합니다. 서비스에 대 한 적합 한 다양 한 시나리오에 설명 하 고 원격 프로시저 호출에 대 한 인터페이스를 제공 하는 것도 장기 실행 백그라운드 작업을 수행 하기 위한 둘 다 구현 하는 방법을 보여 줍니다.

## <a name="broadcast-receiversandroidapp-fundamentalsbroadcast-receiversmd"></a>[브로드캐스트 수신기](~/android/app-fundamentals/broadcast-receivers.md)

이 가이드에서는 만들고 브로드캐스트 수신기를 사용 하는 방법, Xamarin.Android에서 시스템 차원의 브로드캐스트에 응답 하는 Android 구성 요소를 다룹니다.



##  <a name="permissionsandroidapp-fundamentalspermissionsmd"></a>[권한](~/android/app-fundamentals/permissions.md)

Mac 용 Visual Studio 또는 Visual Studio에 기본 제공 도구 지원을 만들고 Android 매니페스트에 권한을 추가를 사용할 수 있습니다. 이 문서에는 Visual Studio 및 Xamarin Studio에서 사용 권한을 추가 하는 방법을 설명 합니다.



##  <a name="graphics-and-animationandroidapp-fundamentalsgraphics-and-animationmd"></a>[그래픽 및 애니메이션](~/android/app-fundamentals/graphics-and-animation.md)

Android 2D 그래픽 및 애니메이션을 지원 하기 위한 매우 다양 하 고 다양 한 프레임 워크를 제공 합니다. 이 문서는 이러한 프레임 워크를 소개 하 고 사용자 지정 그래픽 및 애니메이션을 만들고 Xamarin.Android 응용 프로그램에서 사용 하는 방법을 설명 합니다.


##  <a name="cpu-architecturesandroidapp-fundamentalscpu-architecturesmd"></a>[CPU 아키텍처](~/android/app-fundamentals/cpu-architectures.md)

Xamarin.Android는 32 비트 및 64 비트 장치를 비롯 한 여러 CPU 아키텍처를 지원 합니다. 이 문서에서는 앱이 하나 이상의 Android-지원 되는 CPU 아키텍처를 대상 하는 방법에 설명 합니다.




##  <a name="handling-rotationandroidapp-fundamentalshandling-rotationmd"></a>[회전 처리](~/android/app-fundamentals/handling-rotation.md)

이 문서에서는 Xamarin.Android에서 장치 방향 변경 내용을 처리 하는 방법을 설명 합니다. 이 변경 방향을 프로그래밍 방식으로 처리 하는 방법으로 특정 장치 방향에 대 한 리소스를 자동으로 로드 하려면 Android 리소스 시스템을 사용 하는 방법에 설명 합니다. 그런 다음 장치를 회전할 때 상태를 유지 하는 기술을 설명 합니다.



##  <a name="android-audioandroidapp-fundamentalsandroid-audiomd"></a>[Android 오디오](~/android/app-fundamentals/android-audio.md)

Android OS는 광범위 한 지원 멀티미디어, 오디오와 비디오 둘 다 포함 합니다. 이 가이드는 Android에서 오디오에 중점을 둡니다 하 고 재생 하 고 낮은 수준의 오디오 API 뿐만 아니라 기본 제공 오디오 플레이어 레코더 클래스를 사용 하 여 오디오를 녹음/녹화를 다룹니다. 또한 커지긴 응용 프로그램을 개발할 수 있도록 다른 응용 프로그램에 의해 브로드캐스팅 되는 오디오 이벤트를 사용 하 여 작업을 다룹니다.




##  <a name="notificationsandroidapp-fundamentalsnotificationsindexmd"></a>[알림](~/android/app-fundamentals/notifications/index.md)

이 섹션에서는 Xamarin.Android에서 로컬 및 원격 알림이 구현 하는 방법에 설명 합니다. 여러 UI 요소에는 Android 알림을 설명 하 고 API에 설명의 만들기 및 알림을 표시를 사용 하 여 관련 됩니다. 원격 알림을 Google 클라우드 메시징 및 Firebase Cloud Messaging을 모두 설명 합니다. 단계별 연습 및 코드 샘플이 포함 되어 있습니다.



##  <a name="touchandroidapp-fundamentalstouchindexmd"></a>[터치](~/android/app-fundamentals/touch/index.md)

이 섹션의 개념 및 구현 세부 정보에 대 한 터치 제스처 Android에서 설명 합니다. 터치 Api 도입 하 고 뒤에 제스처 인식기에 설명 합니다.



##  <a name="httpclient-stack-and-ssltlsandroidapp-fundamentalshttp-stackmd"></a>[HttpClient 스택 및 SSL/TLS](~/android/app-fundamentals/http-stack.md)

이 섹션에서는 Android에 대 한 HttpClient 스택 및 SSL/TLS 구현 선택기를 설명 합니다. 이러한 설정은 Xamarin.Android 앱에 사용할 SSL/TLS 및 HttpClient 구현을 결정 합니다.


##  <a name="writing-responsive-applicationswriting-responsive-appsmd"></a>[응답성이 뛰어난 응용 프로그램 작성](writing-responsive-apps.md)

이 문서에 백그라운드 스레드에서 장기 실행 작업을 이동 하 여 Xamarin.Android 응용 프로그램 응답성을 유지 하려면 스레드를 사용 하는 방법을 설명 합니다.