---
title: Xamarin.Essentials 플랫폼 및 기능 지원
description: Xamarin.Essentials는 사용자 인터페이스가 생성된 방식과 관계없이 공유 코드에서 액세스할 수 있는 모든 iOS, Android 또는 UWP 애플리케이션에서 작동하는 단일 플랫폼 간 API를 제공합니다.
ms.assetid: 63FA28A5-6F52-4CB7-AF39-8DF7B436B5A4
author: jamesmontemagno
ms.author: jamont
ms.date: 08/20/2019
ms.openlocfilehash: 6d0e4df22ed363951759f24cc81f8147df482aef
ms.sourcegitcommit: 099b06e311a40c00eeea85465ff9b97867a5c5de
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78295432"
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
>
> * Tizen은 Samsung 개발 팀에서 공식적으로 지원합니다.
> * tvOS 및 watchOS의 API 범위는 제한되어 있습니다. 자세한 내용은 기능 가이드를 참조하세요.

## <a name="feature-support"></a>기능 지원

Xamarin.Essentials는 항상 모든 플랫폼에 기능을 가져오려고 하지만, 디바이스에 따라 제한되는 경우도 있습니다. 다음은 각 플랫폼에서 지원되는 기능에 대한 지침입니다.

아이콘 지침:

* ![전체 지원](~/media/shared/yes.png "전체 지원") - 전체 지원
* ![제한적 지원](~/media/shared/warn.png "제한적 지원") - 제한적 지원
* ![지원 안 함](~/media/shared/no.png "지원 안 함") - 지원 안 함

| 기능 | Android | iOS | UWP | watchOS | tvOS | Tizen |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| [가속도계](accelerometer.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [앱 정보](app-information.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원](~/media/shared/yes.png "tvOS 지원") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [앱 테마](app-theme.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [지표](barometer.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [배터리](battery.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 제한적 지원](~/media/shared/warn.png "watchOS 제한적 지원") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 제한적 지원](~/media/shared/warn.png "Tizen 제한적 지원") | 
| [클립보드](clipboard.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원 안 함](~/media/shared/no.png "Tizen 지원 안 함") | 
| [색 변환기](color-converters.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원](~/media/shared/yes.png "tvOS 지원") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [나침반](compass.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [연결](connectivity.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원](~/media/shared/yes.png "tvOS 지원") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [흔들림 탐지](detect-shake.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원](~/media/shared/yes.png "tvOS 지원") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [디바이스 디스플레이 정보](device-display.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원 안 함](~/media/shared/no.png "Tizen 지원 안 함") | 
| [디바이스 정보](device-information.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원](~/media/shared/yes.png "tvOS 지원") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [전자메일](email.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [파일 시스템 도우미](file-system-helpers.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원](~/media/shared/yes.png "tvOS 지원") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [손전등](flashlight.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [지오코딩](geocoding.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원](~/media/shared/yes.png "tvOS 지원") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [지리적 위치](geolocation.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [자이로스코프](gyroscope.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [Launcher](launcher.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [지자기 센터](magnetometer.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [MainThread](main-thread.md?content=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원](~/media/shared/yes.png "tvOS 지원") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [지도](maps.md?content=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [브라우저 열기](open-browser.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [방향 센서](orientation-sensor.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [권한](permissions.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원](~/media/shared/yes.png "tvOS 지원") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [전화 걸기](phone-dialer.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [플랫폼 확장](platform-extensions.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원](~/media/shared/yes.png "tvOS 지원") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [기본 설정](preferences.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원](~/media/shared/yes.png "tvOS 지원") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [보안 스토리지](secure-storage.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원](~/media/shared/yes.png "tvOS 지원") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [공유](share.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [SMS](sms.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [텍스트 음성 변환](text-to-speech.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원](~/media/shared/yes.png "tvOS 지원") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [단위 변환기](unit-converters.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원](~/media/shared/yes.png "tvOS 지원") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [버전 추적](version-tracking.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원](~/media/shared/yes.png "tvOS 지원") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") | 
| [진동](vibrate.md?context=xamarin/xamarin-forms) | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") |
