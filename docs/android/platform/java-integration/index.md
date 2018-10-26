---
title: Java 통합 개요
description: Java 에코 시스템의 구성 요소 거 대 한 다양 한 컬렉션을 포함합니다. 이러한 구성 요소에는 Android 응용 프로그램을 개발 하는 데 걸리는 시간을 줄이기 위해 사용할 수 있습니다. 이 문서는 소개 하 고 개발자가 Xamarin.Android 응용 프로그램 개발 경험을 개선 하는 데 이러한 기존 Java 구성 요소를 사용할 수 있는 방법 중 일부의 대략적인 개요를 제공 합니다.
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/18/2017
ms.openlocfilehash: 3ab31fb7cac97fbae3315f51daf3dd4b1edbcc1d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112400"
---
# <a name="java-integration-overview"></a>Java 통합 개요

_Java 에코 시스템의 구성 요소 거 대 한 다양 한 컬렉션을 포함합니다. 이러한 구성 요소에는 Android 응용 프로그램을 개발 하는 데 걸리는 시간을 줄이기 위해 사용할 수 있습니다. 이 문서는 소개 하 고 개발자가 Xamarin.Android 응용 프로그램 개발 경험을 개선 하는 데 이러한 기존 Java 구성 요소를 사용할 수 있는 방법 중 일부의 대략적인 개요를 제공 합니다._


## <a name="overview"></a>개요

Xamarin.Android 응용 프로그램에 필요한 특정된 기능에 이미 Java로 코딩 된 가능성이 높습니다 Java 에코 시스템의 범위를 지정 합니다. 이 인해 매력적인 시도 하 고 Xamarin.Android 응용 프로그램을 만들 때 이러한 기존 라이브러리를 다시 사용 됩니다. 

세 가지 가능한 Xamarin.Android 응용 프로그램에서 Java 라이브러리를 다시 사용 합니다. 

-   **Java 바인딩 라이브러리를 만듭니다** &ndash; 이 기법을 Xamarin.Android 프로젝트를 만드는 데 사용 됩니다 C# Java 형식에 대 한 래퍼입니다. Xamarin.Android 응용 프로그램을 참조할 수 있습니다는 C# 래퍼가이 프로젝트에서 생성 한 다음 사용 합니다 `.jar` 파일입니다. 

-   **Java 기본 인터페이스** &ndash; 는 *Java 네이티브* *인터페이스* (JNI)는 Java가 아닌 코드를 허용 하는 프레임 워크 (c + +와 같은 또는 C#)를 호출 하거나 실행 하는 Java 코드에서 호출할 수 JVM 내에서. 

-   **코드를 이식할** &ndash; 이 방법은 Java 소스 코드 가져와으로 변환한 다음 C#합니다. 수동으로 또는 선명 하 게 같은 자동화 된 도구를 사용 하 여 수행할 수 있습니다. 

처음 두 기술의 핵심은는 *Java 기본 인터페이스* (JNI). JNI는 응용 프로그램을 Java로 작성 하지는 Java 가상 컴퓨터에서 실행 하는 Java 코드와 상호 작용 하는 프레임 워크입니다. Xamarin.Android JNI 사용 하 여 만든 *바인딩을* 에 대 한 C# 코드입니다. 

첫 번째 방법은 Java 라이브러리 바인딩 좀 더 자동화 된 된 선언적 방법입니다. Mac 또는 Xamarin.Android에서 제공 하는 Visual Studio 프로젝트 형식에 대 한 Visual Studio를 사용 하는 것이 &ndash; Java 바인딩 라이브러리입니다. 이러한 바인딩은 성공적으로 만들려면 Java 바인딩 라이브러리 여전히 필요할 수 있습니다 일부 수동 수정 하지만 그리 많지 순수 JNI 방법이 됩니다. 참조 [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md) Java 바인딩 라이브러리에 대 한 자세한 내용은 합니다. 

JNI를 사용 하 여, 두 번째 방법은 훨씬 더 낮은 수준에서 작동 하지만 보다 세부적으로 제어를 제공 하 일반적으로 액세스할 수 없는 Java 바인딩 라이브러리를 통해 Java 메서드에 액세스 합니다. 

세 번째 방법은 이전 두 근본적으로 다릅니다: 코드를 포팅하는 Java에서 C#. 다른 언어에서 코드를 이식할 수 매우 힘든 프로세스 이지만 노력을 활용 하는 도구를 사용 하 여 호출 되도록 줄일 수 있습니다 *선명 하 게*합니다. 선명 하 게는 Java는 오픈 소스 도구-에-C# 변환기입니다. 



## <a name="summary"></a>요약

이 문서에는 Xamarin.Android 응용 프로그램에서 Java 라이브러리를 다시 사용할 수는 다양 한 방법 중 일부의 대략적인 개요를 제공 합니다. 바인딩의 개념을 도입 하 고 호출 가능 래퍼를 관리 하 고 Java 코드를 이식 하는 것에 대 한 옵션을 설명 C#입니다. 


## <a name="related-links"></a>관련 링크

- [아키텍처](~/android/internals/architecture.md)
- [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)
- [Jni 작업](~/android/platform/java-integration/working-with-jni.md)
- [선명하게](https://github.com/slluis/sharpen)
- [Java 기본 인터페이스](http://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
