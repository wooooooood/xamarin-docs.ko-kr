---
title: "Xamarin.iOS 분석 규칙"
ms.topic: article
ms.prod: xamarin
ms.assetid: C29B69F5-08E4-4DCC-831E-7FD692AB0886
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/26/2017
ms.openlocfilehash: 7cf627f369b666bb54d0f512dc1361d2a685a057
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinios-analysis-rules"></a>Xamarin.iOS 분석 규칙


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

- **문제:** float32 옵션을 사용 하지 (-aot-옵션 =-O = float32) 이중 정밀도 수치는 눈에 띄게 느린 비용, 모바일에 특수 물게 성능으로 이어집니다. 참고 된.NET 배정밀도 내부적으로, float, 경우에 하므로이 옵션을 사용 하면 전체 자릿수 및 예를 들어, 호환성에 영향을 줍니다.
- **Fix:** Double iOS 프로젝트를 클릭 하 여 빌드로 이동 하세요 > iOS 빌드 및 선택 취소 된 "64 비트 float로 모든 32 비트 부동 소수점 작업을 수행할"입니다.
