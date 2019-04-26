---
title: Xamarin.Mac registrar
description: 이 문서에서는 Xamarin.Mac 등록 기관 및 해당 동적, 고정 및 부분 정적 (하이브리드)의 용도 설명 구성을 사용 합니다.
ms.prod: xamarin
ms.assetid: 7CAAA6B7-D654-4AD3-BAEC-9DD01210978A
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 11/10/2017
ms.openlocfilehash: 21e1a2c6ae5a9ad8b6acf520851ea92a340da887
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61032500"
---
# <a name="xamarinmac-registrar"></a>Xamarin.Mac registrar

_이 문서는 Xamarin.Mac 등록자 및 다른 사용 구성을의 용도 설명 합니다._

## <a name="overview"></a>개요

Xamarin.Mac 간극 관리 (.NET) 환경 사이의 Cocoa의 런타임, 관리 되는 클래스를 관리 되지 않는 Objective-c 클래스를 호출 하 여 이벤트가 발생할 때 호출할 수 있습니다. 이 "매직"을 수행 하는 데 필요한 작업 등록 기관에 의해 처리 되 고, 일반적으로 보기에서 숨겨집니다.

응용 프로그램 시작 시간에이 등록의 성능에 미치는 영향 및 이해 "내부적인" 무슨 많은 경우에 따라 유용할 수 있습니다.

## <a name="configurations"></a>구성

기본적으로 기관의 작업 시작 시 두 가지 범주로 구분할 수 있습니다.

- NSObject에서 파생 된 모든 관리 되는 클래스를 검색 및 Objective-c 런타임에 노출 될 항목의 목록을 수집 합니다.
- Objective-c 런타임에서 사용 하 여이 정보를 등록 합니다.

시간이 지남에 따라 세 가지 다른 등록 기관 구성은 다양 한 사용 사례에 맞게 만들어졌습니다. 각각 다른 빌드에 한 시간 결과 실행 합니다.

- **동적 등록 기관** – 시작 하는 동안 리플렉션을 사용 하 여.NET 검색 모든 로드 유형, 관련 항목 목록을 확인 하 고 네이티브 런타임에 알려야 합니다. 이 옵션으로 전환 하지 않고 빌드에 추가 하지만 (최대 여러 초) 시작 하는 동안 계산 하는 데 비용이 많이 드는.
- **정적 등록 기관** – 빌드 중에 등록 되 고 등록을 처리 하는 Objective-c 코드를 생성 하는 항목 집합을 계산 합니다. 이 코드는 신속 하 게 모든 항목을 등록 하는 시작 하는 동안 호출 됩니다. 빌드 수를 크게 일시 중지 시작 응용 프로그램에서에서 상당한 양의 시간 잘라내기를 추가 합니다.
- **"부분" 정적** – 둘 다의 장점 중 대부분을 제공 하는 새로운 "하이브리드" 접근 방식입니다. 내보내기는 이후 **Xamarin.Mac.dll** 일정 처리는 등록에 연결 하는 사전 계산된 라이브러리에 저장 됩니다. 리플렉션을 사용 하 여 사용자 라이브러리를 처리 하지만 사용자 라이브러리로 플랫폼 바인딩을이 종종 대신 빠른 훨씬 더 적은 유형을 내보냅니다. neglectable 시간 영향을 빌드 및 동적 "비용"의 대부분을 줄입니다.

현재 일부 정적 디버그 구성에 대 한 기본 이며 정적 릴리스 구성에 대 한 기본값입니다.

일부의 시나리오가 있습니다.

- NSObject에서 파생 된 클래스를 사용 하 여 실행 한 후 플러그 인 로드
- NSObject에서 파생 된 클래스 인스턴스를 동적으로 생성

등록 기관은 사실을 수 없는 경우 시작 시 일부 형식을 등록 해야 합니다. `ObjCRuntime.Runtime.RegisterAssembly` 메서드는 고려해 야 할 추가 형식에 등록자를 알리기 위해 제공 됩니다.
