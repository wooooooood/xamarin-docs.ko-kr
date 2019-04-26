---
title: Target Framework for Xamarin.Mac
description: 이 문서에서는 대상 프레임 워크 (기본 클래스 라이브러리)을 Xamarin.Mac에 사용할 수 있는 및 Xamarin.Mac 프로젝트에서 사용 하 여 의미를 설명 합니다.
ms.prod: xamarin
ms.assetid: AF21BE16-3F92-4121-AB4C-D51AC863D92D
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 11/10/2017
ms.openlocfilehash: 15c93126f80917df45a5b80fb84397dc6ef0d5fd
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075630"
---
# <a name="target-framework-for-xamarinmac"></a>Target Framework for Xamarin.Mac

_이 문서에서는 대상 프레임 워크 (기본 클래스 라이브러리)을 Xamarin.Mac에 사용할 수 있는 및 Xamarin.Mac 프로젝트에서 사용 하 여 의미를 설명 합니다._

![Xamarin.Mac 용 프레임 워크 옵션 대상](target-framework-images/select-target.png "Target Xamarin.Mac 용 프레임 워크 옵션")

## <a name="background"></a>배경

모든.NET 프로그램 또는 라이브러리는 기본 클래스 라이브러리 (BCL)에서 제공 하는 기능에 따라 달라 집니다. 이 BCL mscorlib, System, System.Net.Http, System.Xml 등 모든.NET 언어로 작성 하는 일반적인 기능을 제공 하는 어셈블리를 포함 합니다.

년에 걸쳐 다양 한 사용 사례에 대 한 액세스에 최적화 된이 BCL의 여러 다른 버전 개발 했습니다. "데스크톱" BCL 응용 프로그램 공간을 줄이기 위해 사용 되지 않는 코드를 제거 하는 다양 한 모바일 Api에 연결 하는 것에 대 한 안전한 지를 확인 주력 다른 사용 사례에 대 한 너무 고중량 될 수 있는 라이브러리 집합을 포함 합니다.

이러한 다양 한 대상 프레임 워크의 더 중요 한 영향 중 하나는 특정된 응용 프로그램에 있는 어셈블리의 모든 *해야* 호환 되는 BCL 어셈블리가 대상으로 합니다. 두 어셈블리의 서로 다른 버전에 대 한 연결 하지 않았으면 바라는 **System.dll** disagreeing 서명에 지정 된 형식의 대 한 합니다. 공유 라이브러리에는 두 대상을 지정할 수 [.NET 표준 2](https://blog.xamarin.com/share-code-net-standard-2-0/), 대상 프레임 워크 또는 특정 대상 프레임 워크의 일반적인 하위 집합인 합니다.

각각의 장점과 단점을 사용 하 여 각 Xamarin.Mac에 사용할 수 있는 가지 세 대상 프레임 워크 옵션이 있습니다.

- **최신** (이전 문서의 Mobile을 라고 함)-성능 및 크기에 맞게 최적화 어떤 거듭제곱 Xamarin.iOS와 매우 유사한 하위 집합입니다. 이 대상 프레임 워크 링커 안전 하 게 되므로 이러한 프로젝트는 크게 줄였습니다 사용 되지 않는 코드를 제거 하 여 해당 최종 공간을 가질 수 있습니다.

- **전체** (XM 4.5 이전 문서의) – 비슷하지만 몇 가지 작은 제거를 사용 하 여 "데스크톱" BCL 하위 집합입니다. 대상 프레임 워크가 net45를 (이상)와 거의 동일 합니다 netstandard2 중 하나를 제공 하지 않는 여러 nuget을 쉽게 사용할 수 있습니다 또는 특정 Xamarin.Mac 빌드. 그러나 System.Configuration 사용량으로 인 한 호환 되지 않습니다 링크를 사용 합니다.

- **지원 되지 않는** (시스템에서에서 호출 이전 설명서) – 대신에 Xamarin.Mac에서 제공 하는 BCL을 링크의 현재 설치 된 시스템 mono를 사용 합니다. (예: System.Drawing)에서는 문제가 있는 것으로 알려진 일부를 포함 하 여 어셈블리의 전체 집합을 제공합니다. 이 옵션 존재만 "최후의 수단" 있으며 사용 하기 전에 다른 옵션을 모두 소모 하는 것이 좋습니다 강력 하 게 합니다. 이름에서 알 수 있듯이 사용 지원 하지 않는 공식 지원 채널입니다.

## <a name="setting-the-target-framework"></a>대상 프레임 워크를 설정합니다.

Xamarin.Mac 프로젝트에 대 한 대상 프레임 워크 형식으로 변경 하려면 다음을 수행 합니다.

1. Mac용 Visual Studio에서 Xamarin.Mac 프로젝트를 엽니다.
2. **솔루션 탐색기**에서 프로젝트 파일을 두 번 클릭하여 **프로젝트 옵션** 대화 상자를 엽니다.
3. **일반적인** 탭의 형식을 선택 합니다 **대상 프레임 워크** 는 응용 프로그램의 요구에 적합 한:

  [![프로젝트 옵션 창을 사용 하 여 대상 프레임 워크를 선택할](target-framework-images/select-target-full.png "프로젝트 옵션 창을 사용 하 여 대상 프레임 워크를 선택 합니다.")](target-framework-images/select-target-full-large.png#lightbox)

4. **확인** 단추를 클릭하여 변경 내용을 저장합니다.

수행 해야 합니다 **정리** 차례로 **Rebuild** Xamarin.Mac 프로젝트 대상 프레임 워크 형식 전환 후 합니다.

## <a name="summary"></a>요약

이 문서에서는 다양 한 대상 프레임 워크 (기본 클래스 라이브러리)을 Xamarin.Mac 응용 프로그램 및 프레임 워크의 각 형식을 사용 해야 하는 경우 사용 가능한에서는 이러한 부분이 간단 하 게 합니다.


## <a name="related-links"></a>관련 링크

- [iOS 및 Mac 코드 공유](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md)
- [어셈블리](~/cross-platform/internals/available-assemblies.md)
- [기존 Mac 앱 업데이트](~/cross-platform/macios/unified/updating-mac-apps.md)
