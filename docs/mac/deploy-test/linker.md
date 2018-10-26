---
title: Xamarin.Mac 링커 옵션
description: 이 문서에서는 Xamarin.Mac에서 연결을 설명합니다. 연결은 사용되지 않는 코드를 제거하여 응용 프로그램의 크기를 줄이는 강력한 최적화 도구입니다.
ms.prod: xamarin
ms.assetid: F03176C3-F8D4-4DE8-870C-7F27D8CE525A
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 11/10/2017
ms.openlocfilehash: 73f652be32c72ef51170f44c28ce1590e6a0e92b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106848"
---
# <a name="xamarinmac-linker-options"></a>Xamarin.Mac 링커 옵션

_연결은 사용되지 않는 코드를 제거하여 응용 프로그램의 크기를 줄이는 강력한 최적화 도구입니다._

## <a name="overview"></a>개요

프로젝트에서 사용하는 [대상 프레임워크](~/mac/platform/target-framework.md)에 따라 사용 가능한 링커 옵션이 제한될 수 있습니다. 연결하려면 응용 프로그램에서 사용하는 모든 형식의 개체 그래프를 만들어야 하는데, System.Configuration 때문에 전체(또는 지원되지 않음)에서는 이것이 불가능하기 때문입니다.

사용 가능한 네 가지 옵션이 있습니다.

- **없음** – 모든 연결을 사용하지 않습니다. Modern의 디버그 구성과 [전체]의 모든 구성에서 기본값입니다.
- **SDK** – 사용자 어셈블리를 제외한 모든 SDK 어셈블리를 연결합니다. [Modern]의 릴리스 구성에서 기본값입니다. [전체]에서는 사용할 수 없습니다.
- **전체** – 모든 어셈블리를 연결합니다. 이렇게 하려면 사용자 코드가 링커로부터 안전해야 하며, 자세한 내용은 [참고](~/ios/deploy-test/linker.md)에서 확인할 수 있습니다. [전체]에서는 사용할 수 없습니다.
- **플랫폼** – Xamarin.Mac.dll만 연결합니다. 자세한 내용은 다음을 참조하십시오.

## <a name="platform-linking"></a>플랫폼 연결

전체 [대상 프레임워크](~/mac/platform/target-framework.md)를 사용하여 응용 프로그램을 연결하면 일반적으로 안전하지 않지만, 매우 제한된 형태의 연결이 필요한 여러 시나리오가 있습니다.

예를 들어 macOS 앱 스토어에 제출된 응용 프로그램은 사용되지 않는 API(예: QTKit)를 참조하면 안 되며, Xamarin.Mac은 그 중 일부의 바인딩을 포함하고 있습니다. 응용 프로그램이 해당 바인딩을 호출하지 않더라도 호출이 SDK에 존재하며 거부됩니다.

플랫폼 연결은 응용 프로그램 및 BCL이 링커로부터 안전하지 않은 것으로 간주하고 Xamarin.Mac.dll에서 사용되지 않는 코드만 제거합니다. 

Xamarin.Mac.dll 형식에 반영되지 않는 응용 프로그램은 불필요한 형식을 제거하면 시작 속도가 약간 개선됩니다.

플랫폼 연결은 일반적으로 전체 대상 프레임워크를 사용하는 응용 프로그램에만 유용합니다. Modern 응용 프로그램은 좀 더 강력한 SDK 옵션을 사용할 수 있기 때문입니다.

## <a name="setting-the-linker-configuration"></a>링커 구성 설정

Xamarin.Mac 프로젝트에 대한 링커 구성을 변경하려면 다음을 수행합니다.

1. Mac용 Visual Studio에서 Xamarin.Mac 프로젝트를 엽니다.
2. **솔루션 탐색기**에서 프로젝트 파일을 두 번 클릭하여 **프로젝트 옵션** 대화 상자를 엽니다.
3. **Mac 빌드** 탭에서 응용 프로그램의 요구 사항에 적합한 **링커 동작** 형식을 선택합니다.

  ![사용할 링커 동작 선택](linker-images/link-behavior.png "사용할 링커 동작 선택")

4. 전체 대상 프레임워크에 대한 플랫폼 연결은 향후 업데이트될 때까지 IDE에 나타나지 않습니다. 그 전에는 **추가 mmp 인수**에 `--linkplatform`을 추가합니다.
5. **확인** 단추를 클릭하여 변경 내용을 저장합니다.


## <a name="related-links"></a>관련 링크

- [iOS에서 연결](~/ios/deploy-test/linker.md)
