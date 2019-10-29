---
title: Xamarin.ios 분석 규칙
description: 이 문서에서는 Xamarin.ios 프로젝트 설정을 확인 하는 분석 규칙 집합에 대해 설명 합니다 .이를 통해 더 이상 최적화 된 설정을 사용할 수 있는지 여부를 확인할 수 있습니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C29B69F5-08E4-4DCC-831E-7FD692AB0886
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/06/2018
ms.openlocfilehash: c0f40ea6fc7d429867f90d3d3c1b49dacb63acb5
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030860"
---
# <a name="xamarinios-analysis-rules"></a>Xamarin.ios 분석 규칙

Xamarin.ios 분석은 프로젝트 설정을 검사 하 여 더 나은/더 최적화 된 설정을 사용할 수 있는지 여부를 확인 하는 규칙 집합입니다.

가능한 한 빠르게 분석 규칙을 실행 하 여 가능한 한 향상 된 기능을 찾고 개발 시간을 절약 합니다.

규칙을 실행 하려면 Mac용 Visual Studio 메뉴에서 **프로젝트 > 코드 분석 실행**을 선택 합니다.

> [!NOTE]
> Xamarin.ios 분석은 현재 선택한 구성 에서만 실행 됩니다. 디버그 **및** 릴리스 구성에 대해이 도구를 실행 하는 것이 좋습니다.

<a name="XIA0001" />

## <a name="xia0001-disabledlinkerrule"></a>XIA0001: DisabledLinkerRule

- **문제:** 링커가 디버그 모드에 대해 장치에서 사용 하지 않도록 설정 되었습니다.
- **해결 방법:** 예기치 않은 현상을 방지 하기 위해 링커를 사용 하 여 코드를 실행 하려고 합니다.
설정 하려면 Project > iOS 빌드 > 링커 동작으로 이동 합니다.

<a name="XIA0002" />

## <a name="xia0002-testcloudagentreleaserule"></a>XIA0002: TestCloudAgentReleaseRule

- **문제:** Test Cloud 에이전트를 초기화 하는 앱 빌드는 개인 API를 사용 하므로 전송 시 Apple에서 거부 됩니다.
- **해결 방법:** 필요한 #if를 추가 하거나 수정 하 고 코드에서를 정의 합니다.

<a name="XIA0003" />

## <a name="xia0003-ipadebugbuildsrule"></a>XIA0003: IPADebugBuildsRule

- **문제:** 개발자 서명 키를 사용 하는 디버그 구성은 배포에만 필요 하므로 IPA을 생성 하면 안 됩니다 .이는 이제 게시 마법사를 사용 합니다.
- **해결 방법:** 디버그 구성에 대 한 프로젝트 옵션에서 IPA 빌드를 사용 하지 않도록 설정 합니다.

<a name="XIA0004" />

## <a name="xia0004-missing64bitsupportrule"></a>XIA0004: Missing64BitSupportRule

- **문제:** "릴리스"에 대해 지원 되는 아키텍처 "장치"는 64 비트와 호환 되지 않습니다. ARM64 없습니다. Apple은 AppStore에서 32 비트 전용 iOS 앱을 허용 하지 않기 때문에 문제가 발생 합니다.
- **해결 방법:** IOS 프로젝트를 두 번 클릭 하 고 빌드 > iOS 빌드로 이동 하 여 ARM64를 포함 하도록 지원 되는 아키텍처를 변경 합니다.

<a name="XIA0005" />

## <a name="xia0005-float32rule"></a>XIA0005: Float32Rule

- **문제:** Float32 옵션 (--aot =-O = float32)을 사용 하지 않으면 특히 모바일의 성능 비용을 hefty 수 있습니다. 단, 배정밀도 수치는 성능이 크게 느려집니다. .NET에서는 float의 경우에도 내부적으로 배정밀도를 사용 하므로이 옵션을 사용 하도록 설정 하면 전체 자릿수 및 호환성에 영향을 줍니다.
- **해결 방법:** IOS 프로젝트를 두 번 클릭 하 고 빌드 > iOS 빌드로 이동 하 고 "모든 32 비트 float 작업을 64 비트 float로 수행"을 선택 취소 합니다.

<a name="XIA0006" />

## <a name="xia0006-httpclientavoidmanaged"></a>XIA0006: HttpClientAvoidManaged

- **문제:** 더 나은 성능, 작은 실행 크기 및 최신 표준을 쉽게 지원 하기 위해 관리 되는 것이 아니라 네이티브 HttpClient 처리기를 사용 하는 것이 좋습니다.
- **해결 방법:** IOS 프로젝트를 두 번 클릭 하 고 빌드 > iOS 빌드로 이동 하 여 HttpClient 구현을 NSUrlSession (iOS 7 이상) 또는 CFNetwork로 변경 하 여 iOS 7 이전 버전을 지원 합니다.

<a name="XIA0007" />

## <a name="xia0007-usellvmrule"></a>XIA0007: UseLLVMRule

- **문제:** 릴리스 | iPhone 구성의 경우 빌드 시간에 더 빠르게 실행 되는 코드를 생성 하는 LLVM 컴파일러를 사용 하는 것이 좋습니다.
- **해결 방법:** IOS 프로젝트를 두 번 클릭 하 고 빌드 > iOS 빌드로 이동 하 고 릴리스 | iPhone로 이동 하 고 LLVM 최적화 컴파일러 옵션을 선택 합니다.
