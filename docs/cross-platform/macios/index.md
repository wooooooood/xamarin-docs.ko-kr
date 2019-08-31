---
title: Apple 플랫폼 (iOS 및 Mac)
description: 이 문서에서는 Xamarin.ios 및 Xamarin.ios 개발과 관련 된 다양 한 항목 (코드 공유, Unified API, 바인딩 목표-C 라이브러리, 네이티브 참조, 네이티브 형식 등)에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 67246203-D78E-4DCC-9E55-7D3D93968E54
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 2d57e69f3cb7662b1ff6e1e7fe1645605d7861b9
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2019
ms.locfileid: "70199415"
---
# <a name="apple-platform-ios-and-mac"></a>Apple 플랫폼 (iOS 및 Mac)

## <a name="code-sharing"></a>코드 공유

사용자 인터페이스 요소가 없는 코드 요소의 경우 iOS와 Mac 간에 코드를 공유 하는 가장 좋은 방법은 여전히 [이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md)를 사용 하는 것입니다.

일부 사용자 인터페이스 작업을 수행 해야 하는 코드의 경우를 공유 하려면 단일 프로젝트에 공유 하는 코드를 저장 하 고 참조 시 Mac 및 iOS를 모두 사용 하 여 컴파일하는 데 사용할 수 있는 [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md) 를 사용 해야 합니다.

## <a name="unified-apiunifiedindexmd"></a>[Unified API](unified/index.md)

IOS 및 Mac 프로젝트에 대 한 Unified API 프레임 워크에 동일한 네임 스페이스를 사용 하므로 원활한 코드 공유를 위해 두 플랫폼에서 동일한 코드 파일을 사용할 수 있습니다. 또한 32 및 64 비트 빌드를 모두 사용 하도록 설정 합니다. Unified API은 2015 초기부터 템플릿 기본값 이므로 모든 새 프로젝트에 대해 권장 되며, Unified API 프로젝트를 앱 스토어에 제출할 수 있습니다.

### <a name="classic-apis"></a>클래식 Api

> [!NOTE]
> **클래식 프로필 사용 중단:** 새 플랫폼이 Xamarin.ios에 추가 되 면 클래식 프로필 (monotouch.dialog)에서 점진적으로 사용 중단 기능을 시작 합니다. 예를 들어 NRC (새-ref-count) 옵션이 제거 되었습니다. NRC는 항상 모든 통합 응용 프로그램에 대해 사용 하도록 설정 되어 있습니다. 즉, NRC가 옵션으로 설정 되지 않은 경우에는 알려진 문제가 없습니다. 이후 릴리스에서는 가비지 수집기로 Boehm를 사용 하는 옵션이 제거 됩니다. 이는 통합 응용 프로그램에서 사용할 수 없는 옵션 이기도 했습니다. 클래식 지원의 전체 제거는 Xamarin.ios 10.0의 릴리스와 2016에 대해 예약 되어 있습니다.

기본 프레임 워크 `MonoTouch.` 에는 또는 `MonoMac.` 네임 스페이스 접두사가 있기 때문에 원본 (비 통합) xamarin.ios 및 xamarin.ios api에서 코드 공유를 더 어렵게 만들었습니다.  개발자가 동일한 파일에서 MonoMac 및 monotouch.dialog 네임 스페이스를 모두 참조 `using` 하는 문을 추가 하 여 코드를 공유할 수 있도록 하는 몇 가지 빈 네임 스페이스를 제공 했습니다. Classic API은 내부적으로 배포 되는 레거시 앱 에서만 사용 해야 합니다 (Unified API로 업그레이드 하는 것이 권장 됨).


### <a name="updating-from-classic-to-the-unified-api"></a>클래식에서 Unified API로 업데이트

클래식에서 Unified API로 응용 프로그램을 업데이트 하는 방법에 대 한 자세한 지침이 있습니다.

## <a name="binding-objective-c-librariesbindingindexmd"></a>[Objective-C 라이브러리 바인딩](binding/index.md)

Xamarin을 사용 하면 바인딩을 사용 하 여 네이티브 라이브러리를 앱에 가져올 수 있습니다. 이 섹션에 설명합니다.

- 바인딩 작동 방법
- 목표-C 코드를 Xamarin으로 가져올 수 있는 바인딩 프로젝트를 수동으로 빌드하는 방법
- **목적 Sharpie** 도구를 사용 하 여 프로세스를 자동화 하는 방법을 설명 합니다.

## <a name="native-referencesnative-referencesmd"></a>[네이티브 참조](native-references.md)

## <a name="macios-native-typesnativetypesmd"></a>[Mac/iOS 네이티브 형식](nativetypes.md)

및 C# F#에서 32 및 64 비트 코드를 투명 하 게 지원 하기 위해 새 데이터 형식을 소개 합니다.   여기에서 자세히 알아보세요.

## <a name="building-32-and-64-bit-apps32-and-64indexmd"></a>[32 및 64 비트 앱 빌드](32-and-64/index.md)

32 및 64 비트 응용 프로그램을 지원 하기 위해 알아두어야 할 사항입니다.

## <a name="working-with-native-types-in-cross-platform-appsnative-types-cross-platformmd"></a>[플랫폼 간 앱에서의 네이티브 형식 작업](native-types-cross-platform.md)

이 문서에서는 Android 또는 Windows Phone os와 같은 비 iOS`nint`장치와 `nuint`코드를 공유 하는 플랫폼 간 응용 프로그램에서 새로운 iOS Unified API 네이티브 형식 (,, `nfloat`)을 사용 하는 방법을 설명 합니다.
네이티브 형식을 사용 해야 하는 경우에 대 한 통찰력을 제공 하 고 플랫폼 간 코드에서 새 형식을 사용 해야 하는 경우에 대 한 몇 가지 가능한 해결 방법을 제공 합니다.

## <a name="httpclient-stack-and-ssltls-implementation-selectorhttp-stackmd"></a>[HttpClient 스택 및 SSL/TLS 구현 선택기](http-stack.md)

새 HttpClient Stack 선택기는 Xamarin.ios, tvOS 및 Xamarin.ios 앱에서 사용할 HttpClient 구현을 제어 합니다. 이제 iOS의, tvOS의 또는 os X의 기본 전송을 사용 하는 구현 (`NSUrlSession` 또는 `CFNetwork` os에 따라)로 전환할 수 있습니다.

SSL (Secure Socket Layer) 및 그 후속 TLS (전송 계층 보안)는을 통해 `System.Net.Security.SslStream`HTTP 및 기타 네트워크 연결에 대 한 지원을 제공 합니다. 새 SSL/TLS 구현 빌드 옵션은 Mono의 자체 TLS 스택 간에 전환 되 고 하나는 Mac 및 iOS에 있는 Apple의 TLS 스택으로 구동 됩니다.
