---
title: 어떤 프로젝트 설정은 디버거에서 필요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
author: asb3993
ms.author: amburns
ms.openlocfilehash: 67c4b51a518f5c7dba6ae372dbd9206dd3ef8e9f
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33919991"
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

