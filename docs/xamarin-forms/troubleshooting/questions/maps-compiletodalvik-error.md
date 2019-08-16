---
title: COMPILETODALVIK Android 프로젝트가 예기치 않은 최상위 오류를 표시 하며 실패 하는 이유는 무엇 인가요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C0251EB1-F509-47AD-98D6-846AF46425E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: efa21d3547167996e1a7dcc533caa00e0b1262e6
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69529037"
---
# <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik-unexpected-top-level-error"></a>COMPILETODALVIK Android 프로젝트가 예기치 않은 최상위 오류를 표시 하며 실패 하는 이유는 무엇 인가요?

이 오류는 Mac용 Visual Studio의 오류 패드 또는 Visual Studio의 빌드 출력 창에서 볼 수 있습니다. Xamarin.ios를 사용 하는 Android 프로젝트.

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

