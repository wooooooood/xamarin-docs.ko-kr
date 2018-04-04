---
title: 어떤 프로젝트 설정은 디버거에서 필요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 7dff69e90b5105401da162c52053d884ed9b038b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="what-project-settings-are-required-for-the-debugger"></a>어떤 프로젝트 설정은 디버거에서 필요?

(적중 횟수 중단점, 디버그 로그 표시, 등)를 예상 대로 작동 하도록 디버거에 대 한 순서, 개발자 계측 및 디버그 정보 표시가 둘 다 사용할 수 있어야 합니다.

환경 설정을 확인 하려면 다음이 단계를 수행 하십시오.

## <a name="visual-studio"></a>Visual Studio
1. 프로젝트 옵션 열기
2. 로 이동 **빌드 > 고급...** 디버그 정보를 설정 **전체**
3. 각 플랫폼에 대 한 설정은 다음과 같습니다.
   - 로 이동 **Android 옵션 > 디버깅 옵션**합니다. 눈금의 **개발자 계측을 사용 하도록 설정** 상자입니다.
   - 로 이동 **iOS 빌드 > 디버깅 옵션**합니다. 눈금의 **디버깅 사용** 상자입니다.

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac
1. 프로젝트 옵션 열기
2. 로 이동 **빌드 > 컴파일러 > 일반 옵션**합니다. 디버그 정보를 설정 **전체**
3. 각 플랫폼에 대 한 설정은 다음과 같습니다.
  - 로 이동 **빌드 > Android 빌드 > 디버깅 옵션**합니다. 눈금의 **개발자 계측을 사용 하도록 설정** 상자입니다.
  - 로 이동 **빌드 > iOS 디버그**합니다. 눈금의 **디버깅 사용** 상자입니다.

