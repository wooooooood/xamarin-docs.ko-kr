---
title: 그 이유는 Xamarin.Forms 무엇 인가요? 지도 Android 프로젝트가 예기치 않은 최상위 오류 COMPILETODALVIK으로 실패 하나요?
ms.topic: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e29535e71cb77b05da41c043c6fd932ae4f5ce95
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84135852"
---
# <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik-unexpected-top-level-error"></a>그 이유는 Xamarin.Forms 무엇 인가요? 지도 Android 프로젝트가 예기치 않은 최상위 오류 COMPILETODALVIK으로 실패 하나요?

이 오류는 Mac용 Visual Studio의 오류 패드 또는 Visual Studio의 빌드 출력 창에서 볼 수 있습니다. Android 프로젝트에서를 사용 Xamarin.Forms 합니다. 맵.

이는 일반적으로 Xamarin Android 프로젝트에 대 한 Java 힙 크기를 늘려서 해결 됩니다. 다음 단계를 수행 하 여 힙 크기를 늘립니다.

## <a name="visual-studio"></a>Visual Studio

1. Android 프로젝트를 마우스 오른쪽 단추로 클릭 하 & 프로젝트 옵션을 엽니다.
2. **Android 옵션으로 이동-> 고급**
3. Java 힙 크기 텍스트 상자에 1G를 입력 합니다.
4. 프로젝트를 다시 빌드합니다.

![Visual Studio 프로젝트 옵션의 스크린샷](maps-compiletodalvik-error-images/vsjavaheap.png "Visual Studio의 Android 빌드 옵션")

## <a name="visual-studio-for-mac"></a>Mac용 Visual Studio

1. Android 프로젝트를 마우스 오른쪽 단추로 클릭 하 & 프로젝트 옵션을 엽니다.
2. **빌드-> Android 빌드-> 고급** 으로 이동
3. Java 힙 크기 텍스트 상자에 1G를 입력 합니다.
4. 프로젝트를 다시 빌드합니다.  

![Mac용 Visual Studio 프로젝트 옵션의 스크린샷](maps-compiletodalvik-error-images/xsjavaheap.png "Mac용 Visual Studio의 Android 빌드 옵션")
