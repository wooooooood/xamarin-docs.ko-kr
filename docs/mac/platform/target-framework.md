---
title: "대상 프레임워크"
description: "이 문서에서는 Xamarin.Mac에 사용할 수 있는 대상 프레임 (기본 클래스 라이브러리) 및 Xamarin.Mac 프로젝트에서 사용 하 여의 의미를 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: AF21BE16-3F92-4121-AB4C-D51AC863D92D
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: f657fc3dd87d5c39d442a863e4acc00ac320b00d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="target-framework"></a>대상 프레임워크

_이 문서에서는 Xamarin.Mac에 사용할 수 있는 대상 프레임 (기본 클래스 라이브러리) 및 Xamarin.Mac 프로젝트에서 사용 하 여의 의미를 설명 합니다._

![대상 프레임 워크 옵션에 대 한 Xamarin.Mac](target-framework-images/select-target.png "Target Xamarin.Mac에 대 한 프레임 워크 옵션")

## <a name="background"></a>배경

모든.NET 프로그램 또는 라이브러리에서 클래스 라이브러리 BCL (기본)을 제공 하는 기능에 따라 다릅니다. 이 BCL mscorlib, 시스템, System.Net.Http, System.Xml 등 모든.NET 언어에 내장 된 공통 기능을 제공 하는 어셈블리를 포함 합니다.

수 년에 걸쳐 여러 다른 버전의 다른 사용 사례에 대 한 액세스에 최적화 된이 BCL 개발 했습니다. "데스크톱" BCL 응용 프로그램 공간을 줄이고 사용 하지 않는 코드를 제거 하는 다양 한 모바일을 중심으로 Api를 연결에 대 한 안전한 지를 확인 하는 동안 다른 사용 사례에 대 한 너무 고중량 될 수 있는 라이브러리 집합이 포함 되어 있습니다.

더 중요 한 영향 이러한 다른 대상 프레임 워크의 한 가지 이유의 모든 프로그램에서 특정된 어셈블리 *해야* 호환 BCL 어셈블리 대상으로 합니다. 그렇지 않은 경우에 두 어셈블리의 서로 다른 버전에 대 한 연결 있을 수 있습니다는 **System.dll** disagreeing 서명에 지정 된 형식의 대 한 합니다. 공유 라이브러리 수 어떤 대상 [.NET 표준 2](https://blog.xamarin.com/share-code-net-standard-2-0/), 대상 프레임 워크 또는 특정 대상 프레임 워크의 공통 하위 집합인 합니다.

옵션에는 세 개의 대상 프레임 워크 Xamarin.Mac 각각 서로 다른 이점 및 절충 작업에 사용할 수 있는

- **최신** (이전 설명서에서는 모바일을 라고 함) – 매우 유사한 항목을 어떤 powers Xamarin.iOS, 성능 및 크기에 맞게 최적화 합니다. 이 대상 프레임 워크 링커 안전 이므로 이러한 프로젝트는 사용 되지 않는 코드를 제거 하 여 크게 감소의 최종 사용 공간이 있을 수 있습니다.

- **전체** (이전 설명서에서는 XM 4.5를 라고 함)-몇 가지 작은 제거 된 "데스크톱" BCL 매우 유사한 하위 집합입니다. Net45를 (이상)의 대상 프레임 워크와 거의 동일 대로 netstandard2 중 하나를 제공 하지 않는 많은 nugets을 쉽게 사용할 수 있습니다 또는 특정 Xamarin.Mac 작성 합니다. 그러나 System.Configuration 사용으로 인해 하기와 호환 되지 않습니다 연결 합니다.

- **지원 되지 않는** (시스템에서에서 호출 이전 설명서) – 대신 사용 하 여 현재 설치 된 시스템 모노 Xamarin.Mac 제공한 BCL에 연결 합니다. 이 어셈블리의 문제가 있는 것으로 알려진 일부를 포함 하 여 (예: System.Drawing) 최대한 집합을 제공합니다. 이 옵션은만 존재 "최후의 수단" 개이고 사용 하기 전에 다른 옵션을 모두 좋습니다. 이름에서 알 수 있듯이 공식 지원 채널을 통해 사용 하지 않는 합니다.

## <a name="setting-the-target-framework"></a>대상 프레임 워크를 설정합니다.

Xamarin.Mac 프로젝트에 대 한 대상 프레임 워크 종류를 변경 하려면 다음을 수행 합니다.

1. Mac용 Visual Studio에서 Xamarin.Mac 프로젝트를 엽니다.
2. **솔루션 탐색기**에서 프로젝트 파일을 두 번 클릭하여 **프로젝트 옵션** 대화 상자를 엽니다.
3. **일반** 탭의 형식을 선택 하 고 **대상 프레임 워크** 하는 응용 프로그램의 요구에 적합 한:

  [![프로젝트 옵션 창을 사용 하 여 대상 프레임 워크 선택](target-framework-images/select-target-full.png "프로젝트 옵션 창을 사용 하 여 대상 프레임 워크 선택")](target-framework-images/select-target-full-large.png#lightbox)

4. **확인** 단추를 클릭하여 변경 내용을 저장합니다.

수행 해야 **Clean** 차례로 **다시 작성** Xamarin.Mac 프로젝트 대상 프레임 워크 형식이 전환한 후 합니다.

## <a name="summary"></a>요약

이 문서 대상 프레임 워크 (기본 클래스 라이브러리)을 Xamarin.Mac 응용 프로그램 및 각 유형의 프레임 워크를 사용 해야 하는 경우 사용할 수 있는 다양 한 유형의 검사가 수행 간단 하 게 합니다.


## <a name="related-links"></a>관련 링크

- [iOS 및 Mac 코드 공유](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md)
- [어셈블리](~/cross-platform/internals/available-assemblies.md)
- [기존 Mac 응용 프로그램 업데이트](~/cross-platform/macios/unified/updating-mac-apps.md)
