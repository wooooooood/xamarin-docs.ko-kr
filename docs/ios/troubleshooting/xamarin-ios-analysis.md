---
title: "Xamarin.iOS 분석 규칙"
ms.topic: article
ms.prod: xamarin
ms.assetid: C29B69F5-08E4-4DCC-831E-7FD692AB0886
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: c7dc63cbed0dbdc13dfd2d32a0859c0fe7a29196
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="xamarinios-analysis-rules"></a>Xamarin.iOS 분석 규칙

Xamarin.iOS 분석은 더 나은/높음 최적화 된 설정을 사용할 수 있는 경우를 결정할 수 있도록 프로젝트 설정을 확인 하는 규칙 집합입니다.

가능한 개선 사항을 초기에 찾을 개발 시간을 저장 하는 가능한 한 자주 분석 규칙을 실행 합니다.

선택 된 규칙을 Mac의 메뉴에 대 한 Visual Studio에서를 실행 하려면 **프로젝트 > 코드 분석 실행**합니다.

> [!NOTE]
> 현재 선택 된 구성에 Xamarin.iOS 분석만 실행 됩니다. 디버그에 대 한 도구를 실행 하는 것이 좋습니다 **및** 릴리스 구성 합니다.

## <a name="a-namexia0001xia0001-disabledlinkerrule"></a><a name="XIA0001"/>XIA0001: DisabledLinkerRule

- **문제:** 링커는 디버그 모드에 대 한 장치에서 사용할 수 없습니다.
- **Fix:** 링커는 예기치 않은 문제를 방지 하기 위해 코드를 실행 하는 것이 좋습니다.
설치, 프로젝트로 이동 > iOS 빌드 > 링커 동작 합니다.

## <a name="a-namexia0002xia0002-testcloudagentreleaserule"></a><a name="XIA0002"/>XIA0002: TestCloudAgentReleaseRule

- **문제:** 개인 API를 사용 하는 대로 테스트 클라우드 에이전트를 초기화 하는 응용 프로그램 빌드 전송 될 때 Apple에 의해 거부 됩니다.
- **Fix:** 추가 하거나 필요한 #if 수정 하 고 코드에서 정의 합니다.

## <a name="a-namexia0003xia0003-ipadebugbuildsrule"></a><a name="XIA0003"/>XIA0003: IPADebugBuildsRule

- **문제:** 개발자 서명 키를 사용 하 여 디버그 구성 IPA 생성할지 이제 게시 마법사를 사용 하 여 배포에 필요 합니다.
- **Fix:** 디버그 구성에 대 한 프로젝트 옵션에서 IPA 빌드를 사용 하지 않도록 설정 합니다.

## <a name="a-namexia0004xia0004-missing64bitsupportrule"></a><a name="XIA0004"/>XIA0004: Missing64BitSupportRule

- **문제:** 에 대 한 지원 되는 아키텍처 "릴리스 | 장치"64 비트 ARM64 누락 된 호환 되지 않습니다. 이 Apple AppStore에서 32 비트 유일한 iOS 앱을 허용 하지 않습니다는 문제입니다.
- **Fix:** Double iOS 프로젝트를 클릭 하 여 빌드로 이동 하세요 > iOS 빌드 및 ARM64 않았으므로 지원 되는 아키텍처를 변경 합니다.

## <a name="a-namexia0005xia0005-float32rule"></a><a name="XIA0005"/>XIA0005: Float32Rule

- **문제:** float32 옵션을 사용 하지 (-aot-옵션 =-O = float32) 이중 정밀도 수치는 눈에 띄게 느린 비용, 모바일, 특히에서 물게 성능으로 이어집니다. 참고 된.NET 배정밀도 내부적으로, float, 경우에 하므로이 옵션을 사용 하면 전체 자릿수 및 예를 들어, 호환성에 영향을 줍니다.
- **Fix:** Double iOS 프로젝트를 클릭 하 여 빌드로 이동 하세요 > iOS 빌드 및 선택 취소 된 "64 비트 float로 모든 32 비트 부동 소수점 작업을 수행할"입니다.

## <a name="a-namexia0006xia0006-httpclientavoidmanaged"></a><a name="XIA0006"/>XIA0006: HttpClientAvoidManaged

- **문제:** 작은 실행 파일 크기 성능 향상을 위해 관리 되는 대신 네이티브 HttpClient 처리기를 사용 하는 것이 좋습니다를 쉽게 최신 표준을 지원 합니다.
- **Fix:** Double iOS 프로젝트를 클릭 하 여 빌드로 이동 하세요 > iOS 빌드 및 HttpClient 구현 NSUrlSession (iOS 7 이상) 또는 CFNetwork iOS 7 이전 버전을 지원 하도록 변경 합니다.
