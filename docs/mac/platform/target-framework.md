---
title: Xamarin.ios 용 대상 프레임 워크
description: 이 문서에서는 Xamarin.ios에 사용할 수 있는 대상 프레임 워크 (기본 클래스 라이브러리)와 Xamarin.ios 프로젝트에서 사용 하는 경우의 영향을 다룹니다.
ms.prod: xamarin
ms.assetid: AF21BE16-3F92-4121-AB4C-D51AC863D92D
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 11/10/2017
ms.openlocfilehash: 4ae8834427580c387de7a38a69d711207b04821e
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290882"
---
# <a name="target-framework-for-xamarinmac"></a>Xamarin.ios 용 대상 프레임 워크

_이 문서에서는 Xamarin.ios에 사용할 수 있는 대상 프레임 워크 (기본 클래스 라이브러리)와 Xamarin.ios 프로젝트에서 사용 하는 경우의 영향을 다룹니다._

![Xamarin.ios에 대 한 대상 프레임 워크 옵션](target-framework-images/select-target.png "Xamarin.ios에 대 한 대상 프레임 워크 옵션")

## <a name="background"></a>배경

모든 .NET 프로그램 또는 라이브러리는 BCL (기본 클래스 라이브러리)에서 제공 하는 기능에 따라 달라 집니다. 이 BCL에는 모든 .NET 언어에 기본 제공 되는 공통 기능을 제공 하는 mscorlib, System, System .Net. Http 및 System.xml 등의 어셈블리가 포함 되어 있습니다.

몇 년 동안 다양 한 사용 사례에 맞게 최적화 된이 BCL의 여러 다른 버전을 개발 했습니다. "데스크톱" BCL에는 다른 사용 사례에 비해 너무 많은 라이브러리 집합이 포함 되어 있습니다. 반면, 모바일은 Api를 연결 하는 데 안전 하 게 유지 하는 데 중점을 둔 반면 사용 하지 않는 코드를 제거 하 여 응용 프로그램 공간을 줄입니다.

이러한 서로 다른 대상 프레임 워크의 중요 한 영향 중 하나는 지정 된 프로그램의 모든 어셈블리가 호환 BCL 어셈블리를 대상으로 *해야* 한다는 것입니다. 그렇지 않은 경우에는 지정 된 형식의 서명에 동의 하지 않는 두 개의 어셈블리를 서로 다른 버전의 **system.object** 에 연결할 수 있습니다. 공유 라이브러리는 대상 프레임 워크 또는 특정 대상 프레임 워크의 공통 하위 집합인 [.NET Standard 2](https://blog.xamarin.com/share-code-net-standard-2-0/)를 대상으로 지정할 수 있습니다.

Xamarin.ios에 사용할 수 있는 세 가지 대상 프레임 워크 옵션은 각각 서로 다른 장점과 장단점이 있습니다.

- **최신** (이전 설명서에서 Mobile 이라고 함) – Xamarin. iOS의 성능과 크기에 대해 매우 유사한 하위 집합입니다. 이 대상 프레임 워크는 링커에서 안전 하므로 사용 하지 않는 코드를 제거 하 여 최종 공간을 크게 줄일 수 있습니다.

- **전체** (이전 설명서에서 XM 4.5 이라고 함) – 약간의 제거를 포함 하 여 "데스크톱" BCL과 매우 유사한 하위 집합입니다. 대상 프레임 워크는 net45 이상과 거의 동일 하므로 netstandard2 또는 특정 Xamarin.ios 빌드를 제공 하지 않는 많은 nuget를 쉽게 사용할 수 있습니다. 그러나 System. 구성 사용으로 인해 연결과 호환 되지 않습니다.

- **지원 되지 않음** (이전 설명서에서 시스템 이라고 함) – Xamarin.ios에서 제공 하는 BCL에 연결 하는 대신 현재 시스템에 설치 된 mono를 사용 합니다. 이를 통해 문제가 있는 것으로 알려진 일부 (예: Drawing)를 비롯 하 여 어셈블리를 최대한 활용할 수 있습니다. 이 옵션은 "최후의 수단"만을 가지 며 사용 하기 전에 다른 옵션을 사용 하는 것이 좋습니다. 이름에서 알 수 있듯이 공식 지원 채널에서 사용이 지원 되지 않습니다.

## <a name="setting-the-target-framework"></a>대상 프레임 워크 설정

Xamarin.ios 프로젝트에 대 한 대상 프레임 워크 형식으로 변경 하려면 다음을 수행 합니다.

1. Mac용 Visual Studio에서 Xamarin.Mac 프로젝트를 엽니다.
2. **솔루션 탐색기**에서 프로젝트 파일을 두 번 클릭하여 **프로젝트 옵션** 대화 상자를 엽니다.
3. **일반** 탭에서 응용 프로그램의 요구에 맞는 **대상 프레임 워크** 의 형식을 선택 합니다.

    [![프로젝트 옵션 창을 사용 하 여 대상 프레임 워크 선택](target-framework-images/select-target-full.png "프로젝트 옵션 창을 사용 하 여 대상 프레임 워크 선택")](target-framework-images/select-target-full-large.png#lightbox)

4. **확인** 단추를 클릭하여 변경 내용을 저장합니다.

대상 프레임 워크 유형을 전환한 후에 Xamarin 프로젝트를 **정리** 하 고 **다시 빌드해야** 합니다.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 응용 프로그램에 사용할 수 있는 다양 한 대상 프레임 워크 (기본 클래스 라이브러리)와 각 유형의 프레임 워크를 사용 해야 하는 경우에 대해 간략하게 설명 합니다.


## <a name="related-links"></a>관련 링크

- [iOS 및 Mac 코드 공유](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md)
- [어셈블리](~/cross-platform/internals/available-assemblies.md)
- [기존 Mac 앱 업데이트](~/cross-platform/macios/unified/updating-mac-apps.md)
