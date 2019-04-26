---
title: COMPILETODALVIK UNEXPECTED TOP-LEVEL ERROR를 사용 하 여 Xamarin.Forms.Maps Android 프로젝트가 실패 하는 이유
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C0251EB1-F509-47AD-98D6-846AF46425E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 9df9e348440b9dd4b18b3859d64cbe47bd05b24c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61250494"
---
# <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik-unexpected-top-level-error"></a>COMPILETODALVIK UNEXPECTED TOP-LEVEL ERROR를 사용 하 여 Xamarin.Forms.Maps Android 프로젝트가 실패 하는 이유

Mac 또는 Visual Studio의 빌드 출력 창의 오류 패드의 Visual Studio에서이 오류가 표시 될 수 있습니다. Xamarin.Forms.Maps를 사용 하 여 Android 프로젝트입니다.

이 오류를 Xamarin.Android 프로젝트에 대 한 Java 힙 크기를 늘려 가장 일반적으로 해결 됩니다. 힙 크기를 늘리려면 다음이 단계를 수행 합니다.

## <a name="visual-studio"></a>Visual Studio

1. Android 프로젝트를 마우스 오른쪽 단추로 클릭 & 프로젝트 옵션을 엽니다.
2. 로 **Android 옵션-> 고급**
3. Java 힙 크기의 입력란에 1 G를 입력 합니다.
4. 프로젝트를 다시 빌드합니다.

![Visual Studio 프로젝트 옵션의 스크린샷](maps-compiletodalvik-error-images/vsjavaheap.png "Android 빌드 옵션 Visual Studio에서")

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1.  Android 프로젝트를 마우스 오른쪽 단추로 클릭 & 프로젝트 옵션을 엽니다.
2.  로 **빌드 Android 빌드->-> 고급**
3.  Java 힙 크기의 입력란에 1 G를 입력 합니다.
4.  프로젝트를 다시 빌드합니다.  

![Visual Studio for Mac 프로젝트 옵션의 스크린샷](maps-compiletodalvik-error-images/xsjavaheap.png "Android 빌드 Mac 용 Visual Studio의 옵션")

