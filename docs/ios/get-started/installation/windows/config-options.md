---
title: iOS 개발을 위한 Visual Studio 구성
description: 이 문서에서는 Xamarin.iOS 개발을 위해 Visual Studio 2019를 구성하는 방법을 설명합니다. 특히 Xamarin.iOS의 설치된 버전, iOS 도구 모음 및 솔루션 플랫폼 드롭다운 메뉴를 구성하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 22D82244-890D-4325-B3CC-C0AC49130BCA
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/16/2018
ms.openlocfilehash: b3910bd096f2a8dd301a9ba6e200028d3121c8df
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70279220"
---
# <a name="configuring-visual-studio-for-ios-development"></a>iOS 개발을 위한 Visual Studio 구성

_이 문서에서는 Visual Studio에 대한 다양한 Xamarin.iOS 구성 옵션에 대해 설명합니다._

## <a name="using-matching-xamarinios-versions"></a>일치하는 Xamarin.iOS 버전 사용

Visual Studio 2019 또는 Visual Studio 2017은 Mac 빌드 호스트에 설치된 것과 동일한 버전의 Xamarin.iOS를 사용해야 합니다. 이를 확인하려면 다음을 수행합니다.

- Visual Studio 2019 또는 Visual Studio 2017을 사용하는 경우 Mac용 Visual Studio에서 **안정적인** 업데이트 채널을 선택합니다.

- Visual Studio 2019 Preview를 사용하는 경우 Mac용 Visual Studio에서 **알파** 업데이트 채널을 선택합니다.

> [!NOTE]
> [Visual Studio 2017 버전 15.6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning)부터 Visual Studio 2017은 Mac 빌드 호스트에서 Windows와 동일한 버전의 Xamarin.iOS를 사용하고 있는지 자동으로 검색합니다. 버전이 일치하지 않으면 Visual Studio 2017에서 Mac 빌드 호스트에 올바른 버전을 원격으로 설치합니다. 자세한 내용은 [Mac에 페어링](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 가이드의 [자동 Mac 프로비전](~/ios/get-started/installation/windows/connecting-to-mac/index.md#automatic-mac-provisioning) 섹션을 살펴보세요.

## <a name="ios-toolbar"></a>iOS 도구 모음

Visual Studio 2019 또는 Visual Studio 2017에서 iOS 프로젝트가 열리면 iOS 도구 모음이 표시됩니다.  기본적으로 Xamarin.iOS 개발에 유용한 네 개의 단추가 있습니다.

![Visual Studio 2019의 iOS 도구 모음](config-options-images/ios-toolbar.png)

- **Mac에 페어링** – [Mac에 페어링] 대화 상자를 엽니다. Visual Studio 2019 또는 Visual Studio 2017에서 iOS 프로젝트가 열리면 이 단추가 활성화됩니다.
- **iOS 시뮬레이터 표시** – Mac 빌드 호스트에서 iOS 시뮬레이터를 앞으로 가져옵니다. Visual Studio 2019 또는 Visual Studio 2017에서 iOS 프로젝트가 열리면 이 단추가 활성화됩니다.
- **디바이스 로그** – 디바이스 로그를 검사할 수 있는 창을 표시합니다. Visual Studio 2019 또는 Visual Studio 2017에서 iOS 프로젝트가 열리면 이 단추가 활성화됩니다.
- **빌드 서버에 IPA 파일 표시** – Mac 빌드 호스트에서 응용 프로그램에 대한 .ipa 파일 위치를 표시하는 창을 엽니다. .ipa를 만든 빌드가 완료되면 이 단추가 활성화됩니다.

이 도구 모음이 표시되지 않으면 Visual Studio 2019 또는 Visual Studio 2017에서 **보기** 메뉴를 열고 **도구 모음 > iOS**를 선택합니다.

![iOS 도구 모음 활성화](config-options-images/ios-toolbar-enable.png "iOS 도구 모음 활성화")

## <a name="solution-platforms-drop-down-menu"></a>솔루션 플랫폼 드롭다운 메뉴

**솔루션 플랫폼** 드롭다운 메뉴를 사용하면 다음 빌드에서 물리적 디바이스 또는 시뮬레이터를 대상으로 할지 여부를 선택할 수 있습니다.

이 드롭다운 메뉴가 [표준] 도구 모음에 표시되는지 확인하려면 다음을 수행합니다.

- Visual Studio 2019 또는 Visual Studio 2017에서 [표준] 도구 모음의 오른쪽 가장자리에 있는 아래쪽 화살표를 클릭합니다.
- **단추 추가 또는 제거** 선택 
- **솔루션 플랫폼** 항목이 선택되어 있는지 확인합니다.

![솔루션 플랫폼 드롭다운 메뉴 활성화](config-options-images/solution-platforms-enable.png "솔루션 플랫폼 드롭다운 메뉴 활성화")

iOS 프로젝트를 열면 **표준** 및 **iOS** 도구 모음이 다음 스크린샷과 비슷합니다.

![표준 및 iOS 도구 모음](config-options-images/toolbars.png "표준 및 iOS 도구 모음")
