---
title: Xamarin.Essentials 플랫폼 및 기능 지원
description: Xamarin.Essentials는 사용자 인터페이스가 생성된 방식과 관계없이 공유 코드에서 액세스할 수 있는 모든 iOS, Android 또는 UWP 애플리케이션에서 작동하는 단일 플랫폼 간 API를 제공합니다.
ms.assetid: 63FA28A5-6F52-4CB7-AF39-8DF7B436B5A4
author: jamesmontemagno
ms.author: jamont
ms.date: 08/20/2019
ms.openlocfilehash: b12606317fd6cbc9a6a0f59364270595f1250abf
ms.sourcegitcommit: ab51d32f4ea0e0d4701f0bf2f1465c9323cd070b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70887349"
---
# <a name="platform-support"></a>플랫폼 지원

Xamarin.Essentials는 다음 플랫폼 및 운영 체제를 지원합니다.

| 플랫폼 | 버전 |
| --- | --- |
| Android | 4.4(API 19) 이상 |
| iOS |10.0 이상 |
| Tizen | 4.0 이상 |
| tvOS | 10.0 이상 |
| watchOS | 4.0 이상 |
| UWP | 10.0.16299.0 이상 |

> [!NOTE]
> * Tizen은 Samsung 개발 팀에서 공식적으로 지원합니다.
> * tvOS 및 watchOS의 API 범위는 제한되어 있습니다. 자세한 내용은 기능 가이드를 참조하세요.

## <a name="feature-support"></a>기능 지원

Xamarin.Essentials는 항상 모든 플랫폼에 기능을 가져오려고 하지만, 디바이스에 따라 제한되는 경우도 있습니다. 다음은 각 플랫폼에서 지원되는 기능에 대한 지침입니다.

아이콘 지침:

* ✔ - 완전히 지원됨
* ⚠ - 제한적으로 지원됨
* ❌ - 지원되지 않음

| 기능 | Android | iOS | UWP | watchOS | tvOS | Tizen |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| [가속도계](accelerometer.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
| [앱 정보](app-information.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [지표](barometer.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
| [배터리](battery.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ⚠ | ⚠ |
| [클립보드](clipboard.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ❌ |
| [색 변환기](color-converters.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [나침반](compass.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [연결](connectivity.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [흔들림 탐지](detect-shake.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [디바이스 디스플레이 정보](device-display.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ❌ |
| [디바이스 정보](device-information.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [이메일](email.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [파일 시스템 도우미](file-system-helpers.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [손전등](flashlight.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [지오코딩](geocoding.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [지리적 위치](geolocation.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [자이로스코프](gyroscope.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
| [Launcher](launcher.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [지자기 센터](magnetometer.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
| [MainThread](main-thread.md?content=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [지도](maps.md?content=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
| [브라우저 열기](open-browser.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [방향 센서](orientation-sensor.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
| [전화 걸기](phone-dialer.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [플랫폼 확장](platform-extensions.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [기본 설정](preferences.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [보안 스토리지](secure-storage.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [공유](share.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [SMS](sms.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [텍스트 음성 변환](text-to-speech.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [단위 변환기](unit-converters.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [버전 추적](version-tracking.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [진동](vibrate.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
