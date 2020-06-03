---
title: Xamarin.Essentials
description: 이 문서에는 모바일 애플리케이션의 플랫폼 간 API를 개발자에게 제공하는 Xamarin.Essentials를 설명하는 다양한 가이드에 대한 링크가 들어 있습니다.
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 27421ecc8b089321cd2331829d87365f3cf37a65
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139466"
---
# Xamarin.Essentials

Xamarin.Essentials는 모바일 애플리케이션의 플랫폼 간 API를 개발자에게 제공합니다.

Android, iOS 및 UWP는 개발자가 Xamarin을 활용하여 C#에서 모두 액세스할 수 있는 고유한 운영 체제 및 플랫폼 API를 제공합니다. Xamarin.Essentials는 사용자 인터페이스가 생성된 방식과 관계없이 공유 코드에서 액세스할 수 있는 모든 Xamarin.Forms, Android, iOS 또는 UWP 애플리케이션에서 작동하는 단일 플랫폼 간 API를 제공합니다.

## <a name="get-started-with-xamarinessentialsget-startedmdcontextxamarinxamarin-forms"></a>[Xamarin.Essentials 시작](get-started.md?context=xamarin/xamarin-forms)

기존 또는 새로운 Xamarin.Forms, Android, iOS 또는 UWP 프로젝트에 **Xamarin.Essentials** NuGet 패키지를 설치하려면 [시작 가이드](get-started.md)를 따르세요.

## <a name="feature-guides"></a>기능 가이드

다음과 같은 Xamarin.Essentials 기능을 애플리케이션에 통합하려면 가이드를 따르세요.

* [가속도계](accelerometer.md?context=xamarin/xamarin-forms) - 3차원 공간에서 디바이스의 가속 데이터를 검색합니다.
* [앱 정보](app-information.md?context=xamarin/xamarin-forms) - 애플리케이션에 대한 정보를 확인합니다.
* [앱 테마](app-theme.md?context=xamarin/xamarin-forms) – 애플리케이션에 대해 요청된 현재 테마를 검색합니다.
* [기압계](barometer.md?context=xamarin/xamarin-forms) - 기압계에서 압력 변경 내용을 모니터링합니다.
* [배터리](battery.md?context=xamarin/xamarin-forms) - 배터리 잔량, 소스 및 상태를 쉽게 검색합니다.
* [클립보드](clipboard.md?context=xamarin/xamarin-forms) - 클립보드에서 쉽고 빠르게 텍스트를 설정하거나 읽습니다.
* [색 변환기](color-converters.md?context=xamarin/xamarin-forms) – System.Drawing.Color에 대한 도우미 메서드.
* [나침반](compass.md?context=xamarin/xamarin-forms) - 나침반에서 변경 내용을 모니터링합니다.
* [연결](connectivity.md?context=xamarin/xamarin-forms) - 연결 상태를 확인하고 변경 내용을 검색합니다.
* [흔들림 탐지](detect-shake.md?context=xamarin/xamarin-forms) – 디바이스의 움직임을 탐지합니다.
* [디바이스 디스플레이 정보](device-display.md?context=xamarin/xamarin-forms) - 디바이스의 화면 메트릭 및 방향을 가져옵니다.
* [디바이스 정보](device-information.md?context=xamarin/xamarin-forms) - 편리하게 디바이스에 대해 알아봅니다.
* [전자 메일](email.md?context=xamarin/xamarin-forms) - 전자 메일 메시지를 쉽게 전송합니다.
* [파일 시스템 도우미](file-system-helpers.md?context=xamarin/xamarin-forms) - 앱 데이터에 파일을 쉽게 저장합니다.
* [손전등](flashlight.md?context=xamarin/xamarin-forms) - 손전등을 켜고 끄는 간단한 방법입니다.
* [지오코딩](geocoding.md?context=xamarin/xamarin-forms) - 지오코드 및 역방향 지오코드 주소 및 좌표입니다.
* [지리적 위치](geolocation.md?context=xamarin/xamarin-forms) - 디바이스의 GPS 위치를 검색합니다.
* [자이로스코프](gyroscope.md?context=xamarin/xamarin-forms) - 디바이스의 주 축 3개를 중심으로 하는 회전을 추적합니다.
* [시작 관리자](launcher.md?context=xamarin/xamarin-forms) - 애플리케이션이 시스템을 통해 URI를 열 수 있습니다.
* [자력계](magnetometer.md?context=xamarin/xamarin-forms) - 지구의 자기장을 기준으로 디바이스의 방향을 검색합니다.
* [MainThread](main-thread.md?content=xamarin/xamarin-forms) - 애플리케이션의 주 스레드에서 코드를 실행합니다.
* [지도](maps.md?content=xamarin/xamarin-forms) - 지도 애플리케이션에서 특정 위치를 엽니다.
* [브라우저 열기](open-browser.md?context=xamarin/xamarin-forms) - 브라우저에서 특정 웹 사이트를 쉽고 빠르게 엽니다.
* [방향 센서](orientation-sensor.md?context=xamarin/xamarin-forms) - 3차원 공간에서 디바이스의 방향을 검색합니다.
* [권한](permissions.md?context=xamarin/xamarin-forms) – 사용자로부터 권한을 확인하고 요청합니다.
* [전화 걸기](phone-dialer.md?context=xamarin/xamarin-forms) - 전화 걸기를 엽니다.
* [플랫폼 확장](platform-extensions.md?context=xamarin/xamarin-forms) – Rect, 크기 및 포인트 변환을 위한 도우미 메서드.
* [기본 설정](preferences.md?context=xamarin/xamarin-forms) - 영구적 기본 설정을 쉽고 빠르게 추가합니다.
* [보안 스토리지](secure-storage.md?context=xamarin/xamarin-forms) - 데이터를 안전하게 저장합니다.
* [공유](share.md?context=xamarin/xamarin-forms) - 텍스트 및 웹 사이트 URI를 다른 앱에 전송합니다.
* [SMS](sms.md?context=xamarin/xamarin-forms) - 보낼 SMS 메시지를 만듭니다.
* [텍스트 음성 변환](text-to-speech.md?context=xamarin/xamarin-forms) - 디바이스의 텍스트를 말합니다.
* [단위 변환기](unit-converters.md?context=xamarin/xamarin-forms) – 단위를 변환하는 도우미 메서드.
* [버전 추적](version-tracking.md?context=xamarin/xamarin-forms) - 애플리케이션 버전 및 빌드 번호를 추적합니다.
* [진동](vibrate.md?context=xamarin/xamarin-forms) - 디바이스를 진동합니다.
* [웹 인증자](web-authenticator.md?context=xamarin/xamarin-forms) - 웹 인증 흐름을 시작하고 콜백을 수신 대기합니다.

## <a name="troubleshooting"></a>[문제 해결](troubleshooting.md?context=xamarin/xamarin-forms)

문제가 발생하는 경우 도움말을 찾으세요.

## <a name="release-notes"></a>[릴리스 정보](https://docs.microsoft.com/xamarin/essentials/release-notes/)

Xamarin.Essentials의 각 릴리스에 대한 전체 릴리스 정보를 찾으세요.

## <a name="api-documentation"></a>[API 문서](xref:Xamarin.Essentials)

API 문서에서 Xamarin.Essentials의 모든 기능을 살펴보세요.
