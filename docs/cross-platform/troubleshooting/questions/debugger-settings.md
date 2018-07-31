---
title: 디버거에 필요한 프로젝트 설정은 어떤?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 646ef7f708be2de6a851ace25d69a7c2f0b18a83
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350809"
---
# <a name="what-project-settings-are-required-for-the-debugger"></a>디버거에 필요한 프로젝트 설정은 어떤?

(중단점 적중 횟수, 디버그 로그 표시 등)를 예상 대로 작동 하도록 디버거에 대 한 순서로 개발자 계측 및 디버그 정보 표시가 모두 활성화 되어야 합니다.

환경 설정을 확인 하려면 다음이 단계를 따르세요.

## <a name="visual-studio"></a>Visual Studio
1. 프로젝트 옵션을 열려면
2. 로 **빌드 > 고급...** 디버그 정보로 **전체**
3. 각 플랫폼에 대 한 설정은 다음과 같습니다.
   - 로 이동 **Android 옵션 > 디버깅 옵션**합니다. 눈금은 **개발자 계측을 사용 하도록 설정** 상자입니다.
   - 로 이동 **iOS 빌드 > 디버깅 옵션**합니다. 눈금은 **디버깅 사용** 상자입니다.

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac
1. 프로젝트 옵션을 열려면
2. 로 이동 **빌드 > 컴파일러 > 일반 옵션**합니다. 디버그 정보로 **전체**
3. 각 플랫폼에 대 한 설정은 다음과 같습니다.
  - 로 이동 **빌드 > Android 빌드 > 디버깅 옵션**합니다. 눈금은 **개발자 계측을 사용 하도록 설정** 상자입니다.
  - 로 이동 **빌드 > iOS 디버그**합니다. 눈금은 **디버깅 사용** 상자입니다.

