---
title: Windows 및 macOS에서 Visual Studio 2019 미리 보기 연결
description: 이 가이드는 Windows에서 Visual Studio 2019 미리 보기를 사용하여 iOS 앱을 빌드하는 방법과 macOS에서 Mac용 Visual Studio 2019 미리 보기를 사용하여 빌드를 호스팅하는 방법에 대한 지침을 제공합니다.
ms.prod: xamarin
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 11/04/2018
ms.openlocfilehash: 1eeb79737bd712da64ce34a6b4fe83ce2823e3eb
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52899366"
---
# <a name="visual-studio-2019-preview-pairing"></a>Visual Studio 2019 미리 보기 페어링

![미리 보기](~/media/shared/preview.png)

[Mac용 Visual Studio 2019 미리 보기](https://docs.microsoft.com/visualstudio/mac/install-preview)는 macOS에서 Visual Studio Side-by-Side 설치가 완전히 지원되는 첫 번째 버전입니다. Mac Build Host에 Mac용 Visual Studio 2017(버전 7.x)과 Mac용 Visual Studio 2019(버전 8.0)의 미리 보기 모두가 있는 경우, iOS 애플리케이션을 빌드하는 Windows 개발자에게 새로운 상황이 발생합니다. 어느 것이 해당 Windows 설치를 위한 빌드 호스트로 수행되고 있나요?

_기본적으로_ 안정적인 릴리스 &ndash;Mac용 Visual Studio 2017(버전 7.7)&ndash;은 Windows에서 Visual Studio 2017 또는 Visual Studio 2019 미리 보기를 사용하는지 여부에 관계없이 Windows의 Visual Studio에서 사용됩니다.

Windows와 macOS 모두에서 Visual Studio 2019 미리 보기를 올바르게 테스트하려면:

1. Windows 컴퓨터에 Visual Studio 2019(버전 16.0) 미리 보기를 설치하고 사용하세요.
2. Mac에 Mac용 Visual Studio 2019(버전 8.0) 미리 보기를 설치하세요.
3. **Mac용 Visual Studio 2019**에서:

    a. **Visual Studio > 업데이트 확인** 메뉴 항목을 선택합니다.

    b. **Visual Studio 업데이트** 창의 업데이트 채널에서 **Xamarin 미리 보기**를 선택합니다. 다음 경고가 나타납니다.

    > [!WARNING]
    > 최신 Mono 런타임 및 Xamarin SDK가 포함된 최신 Mac용 Visual Studio 2019 미리 보기 빌드를 사용해 보세요. **이 채널에서 빌드를 설치하면 Mac용 Visual Studio 2017 릴리스가 중단됩니다.** Visual Studio 2019 미리 보기 릴리스를 사용하여 Windows 운영 체제에서 Xamarin 앱을 빌드하는 경우에만 이 채널을 사용합니다.

    c. **채널 전환** 단추를 눌러 미리 보기 빌드 호스트를 사용하도록 설정합니다.

4. [Xamarin.iOS 개발을 위한 Mac에 페어링](index.md) 지침 및 [문제 해결 팁](troubleshooting.md)(필요한 경우)에 따릅니다.