---
title: Apple 플랫폼 (iOS 및 Mac)
description: '이 문서에서는 Xamarin.iOS 및 Xamarin.Mac 개발과 관련 된 다양 한 주제를 설명 합니다: 코드를 공유 하 고 통합 API, 바인딩 Objective-c 라이브러리, 네이티브 참조를 네이티브 형식 및 더 합니다.'
ms.prod: xamarin
ms.assetid: 67246203-D78E-4DCC-9E55-7D3D93968E54
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: c30d70d8a36c0e5a9b9ff6ddc74710dec4fb86a4
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2019
ms.locfileid: "67864394"
---
# <a name="apple-platform-ios-and-mac"></a>Apple 플랫폼 (iOS 및 Mac)

## <a name="code-sharing"></a>코드 공유

공유에 대 한 가장 좋은 방법은 없는 사용자 인터페이스 요소에 있는 코드의 요소에 대 한 iOS 및 Mac 간에 코드는 여전히 사용 [이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md)합니다.

일부 사용자 인터페이스 작업을 수행 하는 코드를 공유 하려는 아직 사용 해야 [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md) 단일 프로젝트에서 공유을 하 게 참조 하는 경우 iOS 및 Mac을 사용 하 여 컴파일된 코드를 배치할 수 있습니다.

## <a name="unified-apiunifiedindexmd"></a>[Unified API](unified/index.md)

IOS 및 Mac 프로젝트에 대 한 통합 API는 동일한 코드 파일을 원활 하 게 코드 공유에 대 한 두 플랫폼에서 사용 될 수 있도록 프레임 워크에 대 한 동일한 네임 스페이스를 사용 합니다. 또한 모두 32 비트 및 64 비트 빌드 수 있습니다. Unified API 템플릿 기본 초기 2015 년부터 되었으며 모든 새 프로젝트-것이 좋습니다 *만* 통합 API 프로젝트는 앱 스토어에 제출할 수 있습니다.

### <a name="classic-apis"></a>클래식 Api

> [!NOTE]
> **클래식 프로필 사용 중단:** Xamarin.iOS에서 새 플랫폼 추가 됨에 따라 점진적으로 클래식 프로필 (monotouch.dll)에서 기능 사용을 중단 하기 시작 했습니다. 예를 들어 비 NRC (새 ref-개수) 옵션이 제거 되었습니다. NRC 모두 통합 된 응용 프로그램을 항상 사용할 수 있는 (즉, 비 NRC 않은 옵션) 있고 알려진된 문제가 없습니다. 향후는 Boehm 가비지 수집기로 사용 하는 옵션이 제거 됩니다. 이 또한 절대 통합된 응용 프로그램에 사용할 수 없는 옵션입니다. Xamarin.iOS 10.0의 릴리스를 사용 하 여 2016 년에 대 한 클래식 지원을 완전히 제거할 예정입니다.

원래 (통합 되지 않은) Xamarin.iOS 및 Xamarin.Mac Api를 조금 코드 공유 어렵습니다 네이티브 프레임 워크 중 하나 있어서 `MonoTouch.` 또는 `MonoMac.` 네임 스페이스 접두사입니다.  개발자를 추가 하 여 코드를 공유할 수 있도록 일부 빈 네임 스페이스를 제공 했습니다 `using` 동일한 파일에 있지만이 MonoMac와 MonoTouch 네임 스페이스를 참조 하는 문이 약간 좋지 않은 되었습니다. 클래식 API는 데 내부적으로 배포 되는 레거시 앱만 계속할지 (Unified API로 업그레이드 권장).


### <a name="updating-from-classic-to-the-unified-api"></a>클래식에서 Unified API로 업데이트 하는 중

통합 API로 클래식에서 응용 프로그램 업데이트에 대 한 자세한 지침은 있습니다.

## <a name="binding-objective-c-librariesbindingindexmd"></a>[Objective-C 라이브러리 바인딩](binding/index.md)

Xamarin은 바인딩을 사용 하 여 앱에 네이티브 라이브러리를 가져올 수 있습니다. 이 섹션에 설명합니다.

- 바인딩이 작동 하는 방법
- Xamarin으로 Objective-c 코드를 가져올 수 있는 바인딩 프로젝트를 수동으로 빌드하는 방법 및
- 사용 하는 방법을 우리 **목표 Sharpie** 프로세스를 자동화 하는 데 유용한 도구입니다.

## <a name="native-referencesnative-referencesmd"></a>[네이티브 참조](native-references.md)

## <a name="macios-native-typesnativetypesmd"></a>[Mac/iOS 네이티브 형식](nativetypes.md)

32 비트 및 64 비트 코드에서 투명 하 게 지 원하는 C# 및 F#에 새 데이터 형식이 도입 되었습니다.   여기에서 알아보세요.

## <a name="building-32-and-64-bit-apps32-and-64indexmd"></a>[32 비트 및 64 비트 앱 빌드](32-and-64/index.md)

필요한 32 비트 및 64 비트 응용 프로그램을 지원 하기 위해 알아야 합니다.

## <a name="working-with-native-types-in-cross-platform-appsnative-types-cross-platformmd"></a>[플랫폼 간 앱에서의 네이티브 형식 작업](native-types-cross-platform.md)

이 문서에서는 새 iOS API 네이티브 통합 형식을 사용 하 여 (`nint`, `nuint`, `nfloat`) 예: Android 또는 Windows Phone Os 비 iOS 장치를 사용 하 여 코드 공유 되는 플랫폼 간 응용 프로그램에서 합니다.
네이티브 형식을 사용할 경우에 대 한 정보를 제공 하 고 있는 플랫폼 간 코드를 사용 하 여 새 형식을 사용 해야 하는 경우에는 몇 가지 가능한 솔루션을 제공 합니다.

## <a name="httpclient-stack-and-ssltls-implementation-selectorhttp-stackmd"></a>[HttpClient 스택 및 SSL/TLS 구현 선택기](http-stack.md)

새 HttpClient 스택 선택기는 Xamarin.iOS, Xamarin.tvOS 및 Xamarin.Mac 앱에서 사용 하는 HttpClient 구현을 제어 합니다. TvOS의 또는 OS X 기본 전송 이제 iOS의를 사용 하는 구현에 전환할 수 있습니다 (`NSUrlSession` 또는 `CFNetwork` OS에 따라).

SSL (Secure Socket Layer) 및 그 후속 버전인 TLS (Transport Layer Security)에 대해 지원을 제공 HTTP 통해 다른 네트워크 연결과 `System.Net.Security.SslStream`합니다. Mono의 TLS 스택 및 Apple의 TLS 스택을 Mac 및 iOS에서 제공 하는 하나를 전환 하는 새 SSL/TLS 구현 빌드 옵션입니다.
