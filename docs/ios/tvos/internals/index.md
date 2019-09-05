---
title: Xamarin-내부 tvOS
description: Xamarin.ios를 기반으로 하는 Xamarin에서 tvOS의 내부 작동을 설명 하는 문서입니다. 링크 콘텐츠는 어셈블리, 대상 프레임 워크 및 관련 iOS 개념을 설명 합니다.
ms.prod: xamarin
ms.assetid: 8C076FED-9C03-44DE-9723-0E20272DD16B
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/07/2016
ms.openlocfilehash: ffcf4d3a491cb6ad865da35d387782b7bd1fca01
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283571"
---
# <a name="tvos-in-xamarin-internals"></a>Xamarin-내부 tvOS 

## <a name="assembliesiostvosinternalsassembliesmd"></a>[어셈블리](~/ios/tvos/internals/assemblies.md)

Xamarin에서 tvOS 응용 프로그램에 대해 지 원하는 어셈블리의 목록입니다.

## <a name="target-frameworksiostvosinternalsframeworksmd"></a>[대상 프레임워크](~/ios/tvos/internals/frameworks.md)

이 문서에서는 tvOS에서 사용할 수 있는 대상 프레임 워크 (기본 클래스 라이브러리)의 유형과 tvOS 응용 프로그램에 대 한 특정 대상을 선택할 때의 의미에 대해 설명 합니다.

## <a name="related-ios-articles"></a>관련 iOS 문서

다음 문서는 iOS에만 해당 되지만 tvOS와 관련이 있습니다 (tvOS 9는 iOS 9의 하위 집합 이기 때문).

### <a name="unified-apicross-platformmaciosunifiedindexmd"></a>[Unified API](~/cross-platform/macios/unified/index.md)

에는 Apple TV와 iOS 코드 베이스 간의 코드를 보다 간단 하 게 공유할 수 있을 뿐만 아니라 64 비트 Api 및 64 비트 컴파일에 대 한 지원을 소개 하는 새로운 통합 Api가 도입 되었습니다.  

### <a name="api-designiosinternalsapi-designindexmd"></a>[API 디자인](~/ios/internals/api-design/index.md)

API 바인딩 뒤의 디자인 원칙을 설명 합니다.

### <a name="limitationsiosinternalslimitationsmd"></a>[제한 사항](~/ios/internals/limitations.md)

이 섹션에서는 xamarin.ios와 관련 하 여 주의 해야 하는 문제 및 제한 사항에 대해 설명 합니다 .이 중 상당수는 tvOS에 적용 됩니다.

### <a name="linkeriosdeploy-testlinkermd"></a>[링커](~/ios/deploy-test/linker.md)

링커가 가장 작은 응용 프로그램 패키지를 확인 하는 방법과 해당 설정 및 사용을 수정 하는 방법을 설명 합니다.

### <a name="localization-and-internationalizationiosapp-fundamentalslocalizationindexmd"></a>[지역화 및 국제화](~/ios/app-fundamentals/localization/index.md)

이 가이드에서는 국제화를 지원 하기 위해 Xamarin.ios 응용 프로그램에 인코딩 추가에 대해 설명 합니다.

### <a name="mtouchiosdeploy-testmtouchmd"></a>[mtouch](~/ios/deploy-test/mtouch.md)

프로젝트를 iOS에서 사용할 수 있는 애플리케이션으로 빌드하는 명령줄 도구인 mtouch.exe에 대한 메모와 정보입니다.

### <a name="linking-native-librariesiosplatformnative-interopmd"></a>[네이티브 라이브러리 연결](~/ios/platform/native-interop.md)

Xamarin.ios는 네이티브 C 라이브러리와 목적-C 라이브러리를 모두 사용 하 여 연결을 지원 합니다. 이 문서에서는 네이티브 C 라이브러리를 Xamarin.ios 프로젝트와 연결 하는 방법을 설명 합니다. 목적-c 라이브러리에 대해 동일한 작업을 수행 하는 방법에&nbsp; 대 한 자세한 내용은 [바인딩 목표-c 형식](~/ios/platform/binding-objective-c/index.md)&nbsp;문서를 참조 하세요.

## <a name="objective-c-selectorsiosinternalsobjective-c-selectorsmd"></a>[목표-C 선택기](~/ios/internals/objective-c-selectors.md)

목표-C 선택기 (메서드)를 직접 호출 하는 데 대 한 메모 및 사용입니다.

### <a name="systemdataiosdata-cloudsystemdatamd"></a>[System.Data](~/ios/data-cloud/system.data.md)

System.object를 사용 하 여 기본 제공 SQLite 데이터베이스 시스템에 액세스 하는 방법에 대 한 정보와 지침을 제공 합니다.

### <a name="threadingiosapp-fundamentalsthreadingmd"></a>[스레딩](~/ios/app-fundamentals/threading.md)

Xamarin.ios 응용 프로그램 내에서 스레딩을 사용 하는 방법에 대해 설명 합니다.

### <a name="xib-code-generationiosinternalsxib-code-generationmd"></a>[XIB 코드 생성](~/ios/internals/xib-code-generation.md)

Mac용 Visual Studio를 Xcode의 Interface Builder와 통합 하 여 Interface Builder를 사용 하 여 UI를 디자인할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
