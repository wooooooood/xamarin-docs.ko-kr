---
title: 디버거에 필요한 프로젝트 설정은 무엇인가요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
ms.openlocfilehash: 856c04d129058e8cbac30dcdf619e8b2b5a66cb6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014261"
---
# <a name="what-project-settings-are-required-for-the-debugger"></a>디버거에 필요한 프로젝트 설정은 무엇인가요?

디버거가 정상적으로 작동 하려면 (중단점 적중, 디버그 로그 표시 등), 개발자 계측 및 디버그 정보 표시를 모두 사용 하도록 설정 해야 합니다.

다음 단계를 수행 하 여 환경 설정을 확인 하세요.

## <a name="visual-studio"></a>Visual Studio

1. 프로젝트 옵션 열기
2. **빌드 > 고급** 으로 이동 ... 디버그 정보를 **Full** 로 설정
3. 각 플랫폼에 대 한 설정:
   - **Android 옵션 > 디버깅 옵션**으로 이동 합니다. [ **개발자 계측 사용** ] 상자를 사용 합니다.
   - **IOS 디버그 > 디버깅 & 계측**으로 이동 합니다. **디버깅 사용** 상자를 사용 합니다.

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. 프로젝트 옵션 열기
2. **빌드 > 컴파일러 > 일반 옵션**으로 이동 합니다. 디버그 정보를 **Full** 로 설정
3. 각 플랫폼에 대 한 설정:
    - **빌드 > Android 빌드 > 디버깅 옵션**으로 이동 합니다. [ **개발자 계측 사용** ] 상자를 사용 합니다.
    - **빌드 > IOS 디버그**로 이동 합니다. **디버깅 사용** 상자를 사용 합니다.
