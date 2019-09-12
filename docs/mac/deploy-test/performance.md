---
title: Xamarin.Mac 성능
description: 이 문서에서는 Xamarin.Mac 앱에 대한 몇 가지 성능 고려 사항 정보를 제공합니다. 최신 대상 프레임워크, 링커, AOT, 대리자, 뷰를 재사용하기 위한 Cocoa API 및 동기화 코드를 설명합니다.
ms.prod: xamarin
ms.assetid: 54B07DED-FDF2-49B2-A5FB-3A9357E65922
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 11/10/2017
ms.openlocfilehash: 12a2152424fac4024d8b83adb0c80c2499ec8b1d
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770097"
---
# <a name="xamarinmac-performance"></a>Xamarin.Mac 성능

## <a name="overview"></a>개요

Xamarin.Mac 애플리케이션은 Xamarin.iOS와 비슷하며 다수의 동일한 성능 제안 사항이 적용됩니다.

- [Xamarin.iOS 성능](~/ios/deploy-test/performance.md)
- [플랫폼 간 성능](~/cross-platform/deploy-test/memory-perf-best-practices.md)

하지만 유용한 macOS 관련 제안 사항이 많이 있습니다.

## <a name="prefer-modern-target-framework"></a>Modern Target Framework 권장

Xamarin.Mac 애플리케이션에는 다양한 성능 특성과 기능을 가진 여러 [대상 프레임워크](~/mac/platform/target-framework.md)를 사용할 수 있습니다.

가능할 경우 Modern을 권장하며, 종속 라이브러리를 사용하여 지원을 추가하세요. Modern Target Framework만 어셈블리 크기를 크게 줄일 수 있는 연결을 허용합니다. 이는 AOT를 활성화할 경우에 특히 중요합니다. 전체 어셈블리의 AOT 컴파일로 대규모 최종 번들이 생성될 수 있기 때문입니다.

## <a name="enable-the-linker"></a>링커 활성화

로드의 "Just In Time"(JIT)의 시작 시간은 최종 바이너리의 크기에 다소 비례하여 확장됩니다. 이를 개선하는 가장 쉬운 방법은 [링커](~/mac/deploy-test/linker.md)를 사용하여 데드 코드를 제거하는 것입니다.

이 제안 사항은 주로 Modern Target Framework 사용자에게 적용되지만 [플랫폼 연결](~/mac/deploy-test/linker.md)을 사용하면 제한된 성능 향상도 얻을 수 있습니다.

## <a name="enable-aot-when-appropriate"></a>적절한 경우 AOT 활성화

시작 성능의 또 다른 측면은 기계어 코드로의 어셈블리 JIT 컴파일입니다. AOT(Ahead of Time) 컴파일은 시작 시간을 상당히 절약할 수 있지만 [AOT 설명서](~/mac/internals/aot.md)에 나와 있는 이로 인해 여러 가지 문제점이 생깁니다.

## <a name="ensure-performant-delegates"></a>고성능 대리자 보장

여러 Xamarin.Mac 애플리케이션은 `NSCollectionView`, `NSOutlineView` 또는 `NSTableView` 같은 Cocoa 뷰를 기반으로 합니다. 이러한 보기는 표시할 항목에 대한 질문에 답하기 위해 개발자가 Cocoa에 입력하는 `Delegate` 및 `DataSource` 클래스로 지원되는 경우가 많습니다.

이러한 진입점은 대부분 자주 호출되며, 스크롤 시 초당 몇 번씩 호출되기도 합니다.

이러한 함수는 쉽게 계산할 수 있거나 이미 캐시된 정보를 사용하는 값을 반환하여 사용자 인터페이스의 차단을 방지합니다.

## <a name="use-cocoa-provided-apis-for-reusing-views"></a>Cocoa에서 제공하는 API를 사용하여 뷰 재사용

여러 자식 뷰 또는 셀을 포함하는 많은 Cocoa 뷰(예: `NSCollectionView`, `NSOutlineView` 및 `NSTableView`)는 뷰를 만들고 재사용하는 데 사용되는 API를 제공합니다. 이는 공유 항목 풀을 만들고, 뷰를 빠르게 스크롤할 때 성능 문제가 발생하지 않도록 합니다.

## <a name="use-async-and-do-not-block-the-ui"></a>비동기 방식을 사용하고 UI 차단 안 함

데스크톱 애플리케이션은 종종 많은 양의 데이터를 처리하며, 동기 작업에서 대기 중인 UI 스레드를 차단하기 일쑤입니다.

사용 가능한 경우 항상 [비동기](~/cross-platform/platform/async.md) 및 스레드를 사용하여 UI 차단을 방지하도록 합니다.

장기 실행 작업의 경우 [NSProgressIndicator](https://docs.microsoft.com/samples/xamarin/mac-samples/progressbarexample) 또는 Apple의 [HIG](https://developer.apple.com/macos/human-interface-guidelines/indicators/progress-indicators/)에 언급된 다른 옵션을 사용하여 사용자에게 알리는 것이 좋습니다.

## <a name="related-links"></a>관련 링크

- [플랫폼 간 성능](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Xamarin.iOS 성능](~/ios/deploy-test/performance.md)
