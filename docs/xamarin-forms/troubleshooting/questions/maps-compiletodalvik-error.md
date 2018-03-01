---
title: "예기치 않은 최상위 오류 COMPILETODALVIK Xamarin.Forms.Maps Android 프로젝트 실패 하는 이유"
ms.topic: article
ms.prod: xamarin
ms.assetid: C0251EB1-F509-47AD-98D6-846AF46425E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 41cb955ba22a83d0ae2a11ac5c5a114b465ca9b2
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik-unexpected-top-level-error"></a>예기치 않은 최상위 오류 COMPILETODALVIK Xamarin.Forms.Maps Android 프로젝트 실패 하는 이유

이 오류에서에서 볼 수 있는 Visual Studio의 오류 패드 Mac 용 또는; Visual Studio의 빌드 출력 창 Xamarin.Forms.Maps를 사용 하 여 Android 프로젝트입니다.

이 오류를 Xamarin.Android 프로젝트에 대 한 Java 힙 크기를 늘려 가장 일반적으로 해결 됩니다. 힙 크기를 늘리려면 다음이 단계를 수행 합니다.

## <a name="visual-studio"></a>Visual Studio

1. Android 프로젝트를 마우스 오른쪽 단추로 클릭 및 프로젝트 옵션을 엽니다.
2. 로 이동 **Android 옵션-> 고급**
3. Java 힙 크기 텍스트 상자에 1 G을 입력 합니다.
4. 프로젝트를 다시 빌드합니다.

![Visual Studio 프로젝트 옵션의 스크린 샷](maps-compiletodalvik-error-images/vsjavaheap.png "Android 빌드 Visual Studio에서 옵션")

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1.  Android 프로젝트를 마우스 오른쪽 단추로 클릭 및 프로젝트 옵션을 엽니다.
2.  로 이동 **빌드 Android 빌드->-> 고급**
3.  Java 힙 크기 텍스트 상자에 1 G을 입력 합니다.
4.  프로젝트를 다시 빌드합니다.  

![Mac 프로젝트 옵션에 대 한 Visual Studio의 스크린 샷](maps-compiletodalvik-error-images/xsjavaheap.png "Android 빌드 Mac 용 Visual Studio에서 옵션")

