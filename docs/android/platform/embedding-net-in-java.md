---
title: "Java에서.NET 포함"
description: "Java 기반 네이티브 Android 프로젝트에서 Xamarin.NET 라이브러리를 사용 하는 방법"
ms.topic: article
ms.prod: xamarin
ms.assetid: A489EEF3-1008-4257-BF63-FE21D8C23821
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 1a25f4bc39e39ce58a07ed399082bf13284c16e9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="embedding-net-in-java"></a>Java에서.NET 포함

경우에 따라 기존 네이티브 Android 프로젝트에 Xamarin.NET 라이브러리를 추가 하는 것이 좋습니다. 이 위해 사용할 수 있습니다는 [Embeddinator 4000](https://mono.github.io/Embeddinator-4000/) 을 네이티브 Java 기반 Android 앱에 통합할 수 있는 네이티브 라이브러리를.NET 라이브러리는 도구입니다.

 
## <a name="requirements"></a>요구 사항

Embeddinator 4000 Android에서 Java를 사용 하려면 다음이 필요 합니다.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/preview/index.html) 나중에 설치 되어 있어야 합니다.

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.4.99](https://jenkins.mono-project.com/view/Xamarin.Android/job/xamarin-android/lastSuccessfulBuild/Azure/) 나중에 설치 되어 있어야 합니다.

-   **Java 개발자 키트** &ndash; [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 나중에 설치 되어 있어야 합니다.

-   **모노** &ndash; [모노 5.0](http://www.mono-project.com/download/) 나중에 설치 되어 있어야 합니다.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio를 사용 하 여를 편집 하 여 C# 코드를 컴파일할 수 있습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

편집 하 여 C# 코드를 컴파일할 Mac 용 Visual Studio를 사용할 수 있습니다.

-----

 
## <a name="using-the-embeddinator-4000"></a>Embeddinator 4000를 사용 하 여

네이티브 Android 프로젝트에서.NET 라이브러리를 사용 하려면 다음 단계를 사용 합니다.

1.  C# Android 라이브러리 프로젝트를 만듭니다.

2.  NuGet 통해 Embeddinator 4000를 설치 합니다.

3.  Android 라이브러리 어셈블리에서 Embeddinator를 실행 합니다.

4.  Android Studio에서 Java 프로젝트에서 생성 된 AAR 파일을 사용 합니다.

이러한 단계에서 자세히 설명 된 [Embeddinator 4000](https://mono.github.io/Embeddinator-4000/getting-started-java-android.html) 설명서입니다.
