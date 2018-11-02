---
title: Xamarin.Essentials
description: 이 문서에는 모바일 응용 프로그램에 대한 플랫폼 간 API를 개발자에게 제공하는 Xamarin.Essentials를 설명하는 다양한 가이드에 대한 링크가 들어 있습니다.
ms.assetid: 4EDC9897-5FD1-44CA-A26D-2E5AB472C99A
author: jamesmontemagno
ms.author: jamont
ms.date: 07/30/2018
ms.openlocfilehash: b81102c6c0e0d65aaa46b2d32e34db536ab58e03
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "39361004"
---
# <a name="xamarinessentials"></a>Xamarin.Essentials

![시험판 NuGet](~/media/shared/pre-release.png)

Xamarin.Essentials는 모바일 응용 프로그램에 대한 플랫폼 간 API를 개발자에게 제공합니다.

Android, iOS 및 UWP는 개발자가 Xamarin을 활용하여 C#에서 모두 액세스할 수 있는 고유한 운영 체제 및 플랫폼 API를 제공합니다. Xamarin.Essentials는 사용자 인터페이스가 생성된 방식과 관계없이 공유 코드에서 액세스할 수 있는 모든 Xamarin.Forms, Android, iOS 또는 UWP 응용 프로그램에서 작동하는 단일 플랫폼 간 API를 제공합니다.

## <a name="get-started-with-xamarinessentialsget-startedmdcontextxamarinxamarin-forms"></a>[Xamarin.Essentials 시작](get-started.md?context=xamarin/xamarin-forms)

기존 또는 새로운 Xamarin.Forms, Android, iOS 또는 UWP 프로젝트에 **Xamarin.Essentials** NuGet 패키지를 설치하려면 [시작 가이드](get-started.md)를 따르세요.

## <a name="feature-guides"></a>기능 가이드

다음과 같은 Xamarin.Essentials 기능을 응용 프로그램에 통합하려면 가이드를 따르세요.

* [가속도계](accelerometer.md?context=xamarin/xamarin-forms) - 3차원 공간에서 장치의 가속 데이터를 검색합니다.
* [앱 정보](app-information.md?context=xamarin/xamarin-forms) - 응용 프로그램에 대한 정보를 확인합니다.
* [기압계](barometer.md?context=xamarin/xamarin-forms) - 기압계에서 압력 변경 내용을 모니터링합니다.
* [배터리](battery.md?context=xamarin/xamarin-forms) - 배터리 잔량, 소스 및 상태를 쉽게 검색합니다.
* [클립보드](clipboard.md?context=xamarin/xamarin-forms) - 클립보드에서 쉽고 빠르게 텍스트를 설정하거나 읽습니다.
* [나침반](compass.md?context=xamarin/xamarin-forms) - 나침반에서 변경 내용을 모니터링합니다.
* [연결](connectivity.md?context=xamarin/xamarin-forms) - 연결 상태를 확인하고 변경 내용을 검색합니다.
* [데이터 전송](data-transfer.md?context=xamarin/xamarin-forms) - 텍스트 및 웹 사이트 URI를 다른 앱에 전송합니다.
* [장치 디스플레이 정보](device-display.md?context=xamarin/xamarin-forms) - 장치의 화면 메트릭 및 방향을 가져옵니다.
* [장치 정보](device-information.md?context=xamarin/xamarin-forms) - 편리하게 장치에 대해 알아봅니다.
* [전자 메일](email.md?context=xamarin/xamarin-forms) - 전자 메일 메시지를 쉽게 전송합니다.
* [파일 시스템 도우미](file-system-helpers.md?context=xamarin/xamarin-forms) - 앱 데이터에 파일을 쉽게 저장합니다.
* [손전등](flashlight.md?context=xamarin/xamarin-forms) - 손전등을 켜고 끄는 간단한 방법입니다.
* [지오코딩](geocoding.md?context=xamarin/xamarin-forms) - 지오코드 및 역방향 지오코드 주소 및 좌표입니다.
* [지리적 위치](geolocation.md?context=xamarin/xamarin-forms) - 장치의 GPS 위치를 검색합니다.
* [자이로스코프](gyroscope.md?context=xamarin/xamarin-forms) - 장치의 주 축 3개를 중심으로 하는 회전을 추적합니다.
* [시작 관리자](launcher.md?context=xamarin/xamarin-forms) - 응용 프로그램이 시스템을 통해 URI를 열 수 있습니다.
* [자력계](magnetometer.md?context=xamarin/xamarin-forms) - 지구의 자기장을 기준으로 장치의 방향을 검색합니다.
* [MainThread](main-thread.md?content=xamarin/xamarin-forms) - 응용 프로그램의 주 스레드에서 코드를 실행합니다.
* [지도](maps.md?content=xamarin/xamarin-forms) - 지도 응용 프로그램에서 특정 위치를 엽니다.
* [브라우저 열기](open-browser.md?context=xamarin/xamarin-forms) - 브라우저에서 특정 웹 사이트를 쉽고 빠르게 엽니다.
* [방향 센서](orientation-sensor.md?context=xamarin/xamarin-forms) - 3차원 공간에서 장치의 방향을 검색합니다.
* [전화 걸기](phone-dialer.md?context=xamarin/xamarin-forms) - 전화 걸기를 엽니다.
* [전원](power.md?context=xamarin/xamarin-forms) - 장치의 절전 상태를 확인합니다.
* [기본 설정](preferences.md?context=xamarin/xamarin-forms) - 영구적 기본 설정을 쉽고 빠르게 추가합니다.
* [화면 잠금](screen-lock.md?context=xamarin/xamarin-forms) - 장치 화면을 깨어 있는 상태로 유지합니다.
* [보안 저장소](secure-storage.md?context=xamarin/xamarin-forms) - 데이터를 안전하게 저장합니다.
* [SMS](sms.md?context=xamarin/xamarin-forms) - 보낼 SMS 메시지를 만듭니다.
* [텍스트 음성 변환](text-to-speech.md?context=xamarin/xamarin-forms) - 장치의 텍스트를 말합니다.
* [버전 추적](version-tracking.md?context=xamarin/xamarin-forms) - 응용 프로그램 버전 및 빌드 번호를 추적합니다.
* [진동](vibrate.md?context=xamarin/xamarin-forms) - 장치를 진동합니다.

## <a name="troubleshootingtroubleshootingmdcontextxamarinxamarin-forms"></a>[문제 해결](troubleshooting.md?context=xamarin/xamarin-forms)

문제가 발생하는 경우 도움말을 찾으세요.

## <a name="api-documentationxrefxamarinessentials"></a>[API 문서](xref:Xamarin.Essentials)

API 문서에서 Xamarin.Essentials의 모든 기능을 살펴보세요.
