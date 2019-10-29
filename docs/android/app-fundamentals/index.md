---
title: Xamarin Android 응용 프로그램 기본 사항
description: 핵심 응용 프로그램 개념
ms.prod: xamarin
ms.assetid: 935B8BFE-23B7-4239-5C87-F4A503B889CB
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: eb581d68f3b7e57975b6979fe1005b1fac411ec8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019230"
---
# <a name="xamarinandroid-application-fundamentals"></a>Xamarin Android 응용 프로그램 기본 사항

이 섹션에서는 개발자가 Android 응용 프로그램을 개발할 때 알아야 할 몇 가지 일반적인 작업 또는 개념에 대 한 지침을 제공 합니다.

## <a name="accessibilityandroidapp-fundamentalsaccessibilitymd"></a>[액세스 가능성](~/android/app-fundamentals/accessibility.md)

이 페이지에서는 Android 접근성 Api를 사용 하 여 [내게 필요한 옵션 검사 목록](~/cross-platform/app-fundamentals/accessibility.md)에 따라 앱을 빌드하는 방법을 설명 합니다.

## <a name="understanding-android-api-levelsandroidapp-fundamentalsandroid-api-levelsmd"></a>[Android API 수준 이해](~/android/app-fundamentals/android-api-levels.md)

이 가이드에서는 Android API 수준을 사용 하 여 다양 한 Android 버전에서 앱 호환성을 관리 하는 방법에 대해 설명 하 고, 앱에서 이러한 API 수준을 배포 하도록 Xamarin Android 프로젝트 설정을 구성 하는 방법을 설명 합니다. 또한이 가이드에서는 다양 한 API 수준을 처리 하는 런타임 코드를 작성 하는 방법에 대해 설명 하 고, 모든 Android API 수준, 버전 번호 (예: Android 8.0), Android 코드 이름 (예: Oreo) 및 빌드 버전 코드의 참조 목록을 제공 합니다.

## <a name="resources-in-androidandroidapp-fundamentalsresources-in-androidindexmd"></a>[Android의 리소스](~/android/app-fundamentals/resources-in-android/index.md)

이 문서에서는 Xamarin.ios 및 문서 사용 방법에 대 한 Android 리소스의 개념을 소개 합니다. Android 응용 프로그램에서 리소스를 사용 하 여 응용 프로그램 지역화를 지 원하는 방법 및 다양 한 화면 크기와 밀도를 비롯 한 여러 장치를 사용 하는 방법을 설명 합니다.

## <a name="activity-lifecycleandroidapp-fundamentalsactivity-lifecycleindexmd"></a>[작업 수명 주기](~/android/app-fundamentals/activity-lifecycle/index.md)

활동은 Android 응용 프로그램의 기본 구성 요소 이며 다양 한 상태에 있을 수 있습니다. 활동 수명 주기는 인스턴스화로 시작 하 여 소멸으로 끝나고, 사이에 많은 상태를 포함 합니다. 활동의 상태가 변경 되 면 적절 한 수명 주기 이벤트 메서드가 호출 되어 상태 변경 작업을 알리고 해당 변경에 맞게 코드를 실행할 수 있습니다. 이 문서에서는 활동의 수명 주기를 검토 하 고 이러한 각 상태 변경이 잘 작동 하 고 신뢰할 수 있는 응용 프로그램에 포함 되는 책임을 설명 합니다.

## <a name="localizationandroidapp-fundamentalslocalizationmd"></a>[지역화](~/android/app-fundamentals/localization.md)

이 문서에서는 문자열을 변환 하 고 대체 이미지를 제공 하 여 Xamarin Android를 다른 언어로 지역화 하는 방법을 설명 합니다.

## <a name="servicesandroidapp-fundamentalsservicesindexmd"></a>[Services](~/android/app-fundamentals/services/index.md)

이 문서에서는 백그라운드에서 작업을 수행할 수 있도록 하는 Android 구성 요소인 Android 서비스에 대해 설명 합니다. 서비스에 적합 한 다양 한 시나리오를 설명 하 고 장기 실행 백그라운드 작업을 수행 하 고 원격 프로시저 호출에 대 한 인터페이스를 제공 하는 방법을 보여 줍니다.

## <a name="broadcast-receiversandroidapp-fundamentalsbroadcast-receiversmd"></a>[브로드캐스트 수신기](~/android/app-fundamentals/broadcast-receivers.md)

이 가이드에서는 Xamarin Android에서 시스템 수준 브로드캐스트에 응답 하는 Android 구성 요소인 브로드캐스트 수신기를 만들고 사용 하는 방법에 대해 설명 합니다.

## <a name="permissionsandroidapp-fundamentalspermissionsmd"></a>[권한](~/android/app-fundamentals/permissions.md)

Mac용 Visual Studio 또는 Visual Studio에 기본 제공 되는 도구 지원을 사용 하 여 Android 매니페스트에 사용 권한을 만들고 추가할 수 있습니다. 이 문서에서는 Visual Studio 및 Xamarin Studio에서 사용 권한을 추가 하는 방법에 대해 설명 합니다.

## <a name="graphics-and-animationandroidapp-fundamentalsgraphics-and-animationmd"></a>[그래픽 및 애니메이션](~/android/app-fundamentals/graphics-and-animation.md)

Android는 2D 그래픽 및 애니메이션을 지원 하기 위한 매우 풍부 하 고 다양 한 프레임 워크를 제공 합니다. 이 문서에서는 이러한 프레임 워크를 소개 하 고 사용자 지정 그래픽 및 애니메이션을 만들고 Xamarin.ios 응용 프로그램에서 사용 하는 방법을 설명 합니다.

## <a name="cpu-architecturesandroidapp-fundamentalscpu-architecturesmd"></a>[CPU 아키텍처](~/android/app-fundamentals/cpu-architectures.md)

Xamarin Android는 32 비트 및 64 비트 장치를 비롯 한 몇 가지 CPU 아키텍처를 지원 합니다. 이 문서에서는 하나 이상의 Android 지원 CPU 아키텍처에 앱을 대상으로 하는 방법을 설명 합니다.

## <a name="handling-rotationandroidapp-fundamentalshandling-rotationmd"></a>[회전 처리](~/android/app-fundamentals/handling-rotation.md)

이 문서에서는 Xamarin Android에서 장치 방향 변경을 처리 하는 방법을 설명 합니다. Android 리소스 시스템을 사용 하 여 특정 장치 방향에 대 한 리소스를 자동으로 로드 하는 방법 뿐만 아니라 방향 변경을 프로그래밍 방식으로 처리 하는 방법을 설명 합니다. 그런 다음 장치를 회전할 때 상태를 유지 관리 하는 기술을 설명 합니다.

## <a name="android-audioandroidapp-fundamentalsandroid-audiomd"></a>[Android 오디오](~/android/app-fundamentals/android-audio.md)

Android OS는 오디오 및 비디오를 모두 제공 하는 멀티미디어를 광범위 하 게 지원 합니다. 이 가이드는 Android의 오디오를 중심으로 하 고 기본 제공 오디오 플레이어와 레코더 클래스를 사용 하 여 오디오를 재생 하 고 기록 하는 방법 및 하위 수준 오디오 API에 대해 다룹니다. 또한 개발자가 잘 작동 하는 응용 프로그램을 빌드할 수 있도록 다른 응용 프로그램에서 브로드캐스팅하는 오디오 이벤트를 사용 하는 방법을 설명 합니다.

## <a name="notificationsandroidapp-fundamentalsnotificationsindexmd"></a>[알림](~/android/app-fundamentals/notifications/index.md)

이 섹션에서는 Xamarin.ios에서 로컬 및 원격 알림을 구현 하는 방법에 대해 설명 합니다. Android 알림의 다양 한 UI 요소에 대해 설명 하 고 알림을 만들고 표시 하는 것과 관련 된 API에 대해 설명 합니다. 원격 알림의 경우 Google Cloud Messaging 및 Firebase 클라우드 메시징이 모두 설명 되어 있습니다. 단계별 연습 및 코드 샘플이 포함 되어 있습니다.

## <a name="touchandroidapp-fundamentalstouchindexmd"></a>[터치](~/android/app-fundamentals/touch/index.md)

이 섹션에서는 Android에서 터치 제스처를 구현 하는 방법에 대 한 개념 및 세부 정보를 설명 합니다. 터치 Api가 소개 되 고 설명 된 다음 제스처 인식기가 탐색 됩니다.

## <a name="httpclient-stack-and-ssltlsandroidapp-fundamentalshttp-stackmd"></a>[HttpClient 스택 및 SSL/TLS](~/android/app-fundamentals/http-stack.md)

이 섹션에서는 프록시에 대 한 HttpClient 스택 및 SSL/TLS 구현 선택기에 대해 설명 합니다. 이러한 설정에 따라 Xamarin Android 앱에서 사용할 HttpClient 및 SSL/TLS 구현이 결정 됩니다.

## <a name="writing-responsive-applicationswriting-responsive-appsmd"></a>[응답성이 뛰어난 응용 프로그램 작성](writing-responsive-apps.md)

이 문서에서는 장기 실행 작업을 백그라운드 스레드로 이동 하 여 Xamarin Android 응용 프로그램의 응답성을 유지 하기 위해 스레딩을 사용 하는 방법을 설명 합니다.
