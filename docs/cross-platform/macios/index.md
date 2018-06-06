---
title: Apple 플랫폼 (iOS 및 Mac)
description: '이 문서에서는 Xamarin.iOS 및 Xamarin.Mac 개발과 관련 된 다양 한 주제: 코드 공유, 통합 API, 바인딩 Objective-c 라이브러리, 네이티브 참조, 네이티브 유형 및 더 합니다.'
ms.prod: xamarin
ms.assetid: 67246203-D78E-4DCC-9E55-7D3D93968E54
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: b40758fa562e57415cd3c0818763ef0a7ce5dcca
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781535"
---
# <a name="apple-platform-ios-and-mac"></a>Apple 플랫폼 (iOS 및 Mac)

## <a name="code-sharing"></a>코드 공유

공유 하는 가장 좋은 방법은 사용자 인터페이스 요소가 없는 있는 코드 요소에 대 한 iOS 및 Mac 사이 코드는 여전히 사용 [이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md)합니다.

일부 사용자 인터페이스 작업을 수행할 수 있는 코드에 대 한를 공유 하려는 아직 사용할지 [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md) 단일 프로젝트에서 공유할을 하 게 참조 될 때 iOS와 Mac을 사용 하 여 컴파일된 코드를 배치할 수 있도록 허용 합니다.

##  <a name="unified-apiunifiedindexmd"></a>[Unified API](unified/index.md)

IOS 및 Mac 프로젝트에 대 한 통합 API 같은 코드 파일 원활한 코드 공유에 대 한 두 플랫폼에서 사용 될 수 있도록 프레임 워크에 대해 동일한 네임 스페이스를 사용 합니다. 또한 32와 64 비트 빌드 수 있습니다. 통합 API 템플릿 기본값 초기 2015 이후의 되었으며 모든 새 프로젝트-권장 *만* 통합 API 프로젝트는 앱 스토어에 제출할 수 있습니다.

### <a name="classic-apis"></a>클래식 Api

> [!NOTE]
> **클래식 프로필 사용 중단:** 클래식 프로필 (monotouch.dll)에서 기능을 사용 하지 않으려는 점진적으로 시작 되며 새 플랫폼 Xamarin.iOS에 추가 됩니다. 예를 들어 비 NRC (새-ref-개수) 옵션이 제거 되었습니다. NRC 모두 통합 된 응용 프로그램을 항상 사용할 수 있는 (즉, 비 NRC 않은 옵션) 및에 알려진된 문제가 없습니다. 이후 릴리스에서 가비지 수집기 Boehm를 사용 하는 옵션이 제거 됩니다. 절대 통합 된 응용 프로그램에 사용할 수 없는 옵션 이기도 합니다. 클래식 지원을 완전히 제거할 Xamarin.iOS 10.0의 릴리스를 2016 년 동안 예약 됩니다.

원래 (비 통합) Xamarin.iOS 및 Xamarin.Mac Api 조금 코드 공유 어렵습니다 네이티브 프레임 워크 중 하나 때문에 `MonoTouch.` 또는 `MonoMac.` 네임 스페이스 접두사입니다.  개발자가 추가 하 여 코드를 공유할 수 있는 일부 빈 네임 스페이스를 제공 해 드립니다 `using` MonoMac와 MonoTouch 네임 스페이스에는 동일한 파일에 있지만이 참조 하는 문을 약간 까다롭고 되었습니다. 내부적으로 배포 되는 레거시 앱에서 사용할 수만 계속할지 클래식 API (통합 API에 업그레이드 권장).


### <a name="updating-from-classic-to-the-unified-api"></a>통합 된 API에 클래식에서 업데이트

통합 된 API를 이전의에서 응용 프로그램 업데이트에 대 한 자세한 지침이 표시 됩니다.

## <a name="binding-objective-c-librariesbindingindexmd"></a>[Objective-C 라이브러리 바인딩](binding/index.md)

Xamarin은 바인딩을 사용 하는 앱으로 네이티브 라이브러리를 가져올 수 있습니다. 이 섹션에 설명합니다.

- 바인딩 작동 방식
- Xamarin으로 Objective C 코드를 가져올 수 있는 바인딩 프로젝트를 수동으로 작성 하는 방법 및
- 사용 하는 방법을 우리의 **목표 Sharpie** 프로세스를 자동화 하기 위해 도구입니다.

## <a name="native-referencesnative-referencesmd"></a>[네이티브 참조](native-references.md)

##  <a name="macios-native-typesnativetypesmd"></a>[Mac/iOS 네이티브 형식](nativetypes.md)

32와 C# 및 F #에서 투명 하 게 64 비트 코드를 지원 하기 위해 새로운 데이터 형식을 도입 했습니다.   여기에 대해 알아봅니다.

##  <a name="building-32-and-64-bit-apps32-and-64indexmd"></a>[32와 64 비트 응용 프로그램 구축](32-and-64/index.md)

필요한 32와 64 비트 응용 프로그램을 지원 하기 위해 알아야 합니다.

## <a name="working-with-native-types-in-cross-platform-appsnative-types-cross-platformmd"></a>[플랫폼 간 앱에서의 네이티브 형식 작업](native-types-cross-platform.md)

이 문서에서는 새 iOS 통합 API 네이티브 형식을 사용 하 여 (`nint`, `nuint`, `nfloat`) Android 또는 Windows Phone Os와 같은 비 iOS 장치 코드는 공유 하는 플랫폼 간 응용 프로그램에서 합니다.
네이티브 형식을 사용 해야 하는 시기에 대 한 정보를 제공 하 고 새 형식의 플랫폼 간 코드를 함께 사용 해야 하는 위치 하는 경우에 몇 가지 가능한 해결 방법을 제공 합니다.

## <a name="httpclient-stack-and-ssltls-implementation-selectorhttp-stackmd"></a>[HttpClient 스택 및 SSL/TLS 구현 선택기](http-stack.md)

새 HttpClient 스택 선택기 Xamarin.iOS, Xamarin.tvOS 및 Xamarin.Mac 응용 프로그램에서 사용 하는 데 어떤 HttpClient 구현을 제어 합니다. TvOS의 또는 OS X 기본 전송 이제 iOS의를 사용 하는 구현으로 전환할 수 (`NSUrlSession` 또는 `CFNetwork` 운영 체제에 따라).

SSL (Secure Socket Layer) 및 그 후속 TLS (전송 계층 보안), HTTP 및를 통해 다른 네트워크 연결에 대 한 지원을 제공 `System.Net.Security.SslStream`합니다. 전원이 Apple의 TLS 스택에 Mac 및 iOS에 의해 개와 모노의 TLS 스택을 전환 하는 새 SSL/TLS 구현 빌드 옵션입니다.
