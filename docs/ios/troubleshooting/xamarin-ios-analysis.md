---
title: Xamarin.iOS 분석 규칙
description: 이 문서에서는 더/better optimized 설정을 사용할 경우를 결정 하려면 Xamarin.iOS 프로젝트 설정을 확인 하는 분석 규칙 집합을 설명 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C29B69F5-08E4-4DCC-831E-7FD692AB0886
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/06/2018
ms.openlocfilehash: 8a4990ce7b2bcacbd4b97b214458531b3d94122e
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121037"
---
# <a name="xamarinios-analysis-rules"></a>Xamarin.iOS 분석 규칙

Xamarin.iOS 분석은 더 나은 이상의 최적화 된 설정을 사용할 수 있는지를 확인 하는 데 프로젝트 설정을 확인 하는 규칙의 집합입니다.

초기에 가능한 개선 사항을 찾아 개발 시간을 절약 하려면 가능한 한 자주 분석 규칙을 실행 합니다.

Visual Studio for Mac의 메뉴에서에서 규칙을 실행 하려면 선택한 **프로젝트 > 코드 분석 실행**합니다.

> [!NOTE]
> Xamarin.iOS 분석은 현재 선택 된 구성에만 실행 됩니다. 디버그 도구를 실행 하는 것이 좋습니다 **고** 릴리스 구성 합니다.

<a name="XIA0001" />

## <a name="xia0001-disabledlinkerrule"></a>XIA0001: DisabledLinkerRule

- **문제:** 링커는 디버그 모드에 대 한 장치에서 사용 하지 않도록 설정 합니다.
- **해결 방법:** 놀라지 링커를 사용 하 여 코드를 실행 하는 것이 좋습니다.
이 설정 하려면 프로젝트로 이동 > iOS 빌드 > 링커 동작 합니다.

<a name="XIA0002" />

## <a name="xia0002-testcloudagentreleaserule"></a>XIA0002: TestCloudAgentReleaseRule

- **문제:** Test Cloud agent를 초기화 하는 앱 빌드 전용 API를 사용 하므로 제출 하는 경우 Apple에서 거부 됩니다.
- **해결 방법:** 추가 하거나 필요한 #if를 수정 하 고 코드에서 정의 합니다.

<a name="XIA0003" />

## <a name="xia0003-ipadebugbuildsrule"></a>XIA0003: IPADebugBuildsRule

- **문제:** 이제 게시 마법사를 사용 하는 배포만 필요할 때 개발자 서명 키를 사용 하는 디버그 구성을 IPA를 생성 하지 말아야 합니다.
- **해결 방법:** 디버그 구성에 대 한 프로젝트 옵션에서 IPA 빌드를 사용 하지 않도록 설정 합니다.

<a name="XIA0004" />

## <a name="xia0004-missing64bitsupportrule"></a>XIA0004: Missing64BitSupportRule

- **문제:** 지원 되는 아키텍처에 대 한 "릴리스 | 장치"64 비트 ARM64 누락 된 호환 되지 않습니다. 이 문제가 Apple AppStore에서 32 비트 전용 iOS 앱을 허용 하지 않습니다.
- **해결 방법:** iOS 프로젝트를 클릭 하는 Double, 빌드로 이동 > iOS 빌드 및 ARM64 되므로 지원 되는 아키텍처를 변경 합니다.

<a name="XIA0005" />

## <a name="xia0005-float32rule"></a>XIA0005: Float32Rule

- **문제:** float32 옵션을 사용 하지 (-aot-옵션 =-O = float32) 여기서 이중 정밀도 수치는 눈에 띄게 느려집니다 비용, 특히 모바일에서 상당한 성능으로 이어집니다. .NET 사용 배정밀도 내부적으로 float에 대해서도 있으므로이 옵션을 사용 하면 전체 자릿수 및 호환성에 영향을 줍니다.
- **해결 방법:** Double iOS 프로젝트를 클릭 하 여 빌드 사이트로 이동 하십시오 > iOS 빌드 및의 선택을 취소는 "모든 32 비트 부동 연산으로 수행 64 비트 부동 소수점".

<a name="XIA0006" />

## <a name="xia0006-httpclientavoidmanaged"></a>XIA0006: HttpClientAvoidManaged

- **문제:** 작은 실행 파일 크기, 성능 향상을 위해 관리 되는 대신 네이티브 HttpClient 처리기를 사용 하는 것이 좋습니다를 쉽게 최신 표준을 지원 합니다.
- **해결 방법:** iOS 프로젝트를 클릭 하는 Double, 빌드로 이동 > iOS 빌드 및 HttpClient 구현 NSUrlSession (iOS 7 이상) 또는 CFNetwork iOS 7 이전 버전을 지원 하도록 변경 합니다.
