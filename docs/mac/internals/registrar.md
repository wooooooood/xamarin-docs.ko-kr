---
title: Xamarin.Mac registrar
description: 이 문서는 Xamarin.Mac 등록 기관 및 해당 사용 구성의 용도 설명 합니다.
ms.prod: xamarin
ms.assetid: 7CAAA6B7-D654-4AD3-BAEC-9DD01210978A
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: 4b70ac2271b23b54e7942fdc870e0f49548e6154
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinmac-registrar"></a>Xamarin.Mac registrar

_이 문서는 Xamarin.Mac 등록 기관 및 해당 사용 구성의 용도 설명 합니다._

## <a name="overview"></a>개요

Xamarin.Mac는 Cocoa의 런타임, 관리 되지 않는 Objective-c 클래스를 호출 하는 이벤트가 발생할 때 호출할 수 있는 관리 되는 클래스를 허용 하 고 관리 (.NET) 환경 사이의 간격을 잇는 합니다. 이 "매직"를 수행 하는 데 필요한 등록자에서 처리 되 고, 일반적으로 보기에서 숨겨집니다.

응용 프로그램 시작 시간에 대해 구체적으로 이러한 등록의 성능에 미치는 영향 있으며 약간의 "내부"에서 진행 상황을 이해 경우가 유용할 수 있습니다.

## <a name="configurations"></a>구성

기본적으로 등록 기관의 작업 시작 시 두 가지 범주로 구분할 수 있습니다.

- NSObject에서 파생 되는 것에 대 한 모든 관리 되는 클래스를 검색 하 고 Objective-c 런타임에 노출 해야 하는 항목 목록이 수집 합니다.
- 이 정보 Objective C 런타임에 등록 합니다.

시간이 지남에 따라 다음 세 가지 다른 등록 기관 구성 다른 사용 사례를 포함 하도록 생성 된 합니다. 각각 다른 빌드 개이고 시간 결과 실행:

- **동적 등록 기관** – 시작 하는 동안 리플렉션을 사용 하 여.NET 검색 로드 된 모든 유형, 관련 된 항목의 목록을 확인 하 고 네이티브 런타임에 알립니다. 이 옵션으로 전환 하지는 빌드에 추가 하지만 여러 초) (최대 시작 하는 동안 계산 하는 데 비용이 많이 드는.
- **정적 등록 기관** – 빌드하는 동안 등록 및 등록을 처리 하기 위한 Objective C 코드를 생성 하는 항목 집합을 계산 합니다. 이 코드는 모든 항목을 신속 하 게 등록을 위해 시작할 때 호출 됩니다. 빌드 하지만 있습니다 일시 중지를 중요 한 응용 프로그램 시작에서 상당한 시간이 잘라내기 추가 합니다.
- **"부분" 정적** – 둘 다의 장점 중 대부분을 표시 하는 최신 "하이브리드" 접근 방식입니다. 내보내기를 이후 **Xamarin.Mac.dll** 상수인 미리 계산 된 라이브러리의 등록을 처리 하 고에 있는 연결를 저장 합니다. 리플렉션을 사용 하 여 사용자 라이브러리로 플랫폼 바인딩을 인지 종종 아니라 빠른 훨씬 더 적은 유형을 내보낼 수는 사용자 라이브러리를 처리 합니다. Neglectable는 드는 시간을 빌드 및 대부분의 동적 "cost"의 감소 합니다.

오늘 부분 정적 디버그 구성에 대 한 기본값 이므로 정적 릴리스 구성에 대 한 기본값입니다.

일부의 시나리오가 있습니다.

- 플러그 인 NSObject에서 파생 된 클래스와 함께 출시 후 로드
- 동적으로 생성 된 NSObject에서 파생 된 클래스 인스턴스

여기서 등록자 시작 될 때 일부 형식을 등록 해야 함을 알 수 없으면입니다. `ObjCRuntime.Runtime.RegisterAssembly` 메서드 고려해 야 할 추가 형식을 등록자에 게 알리기 위해 제공 됩니다.
