---
title: Java 통합 개요
description: Java 생태계는 다양 하 고 여러 구성 요소 컬렉션을 포함합니다. 이러한 구성 요소에 Android 응용 프로그램을 개발 하는 데 걸리는 시간을 줄이기 위해 사용할 수 있습니다. 이 문서는 소개 하 고는 몇 가지 개발자가 자신의 Xamarin.Android 응용 프로그램 개발 환경을 개선 하기 위해 이러한 기존 Java 구성 요소를 사용할 수는 상위 수준 개요를 제공 됩니다.
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/18/2017
ms.openlocfilehash: dbaf17479ae077fced425df5ac31bdbbc4e06b64
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="java-integration-overview"></a>Java 통합 개요

_Java 생태계는 다양 하 고 여러 구성 요소 컬렉션을 포함합니다. 이러한 구성 요소에 Android 응용 프로그램을 개발 하는 데 걸리는 시간을 줄이기 위해 사용할 수 있습니다. 이 문서는 소개 하 고는 몇 가지 개발자가 자신의 Xamarin.Android 응용 프로그램 개발 환경을 개선 하기 위해 이러한 기존 Java 구성 요소를 사용할 수는 상위 수준 개요를 제공 됩니다._


## <a name="overview"></a>개요

Java 에코 시스템의 범위를 매개 변수로 받아 매우 높습니다 Xamarin.Android 응용 프로그램에 필요한 기능을 지정 된 java에서 코딩 이미 있습니다. 이 때문에 매력적인 시도 하 고 Xamarin.Android 응용 프로그램을 만들 때 이러한 기존 라이브러리를 다시 사용 됩니다. 

Xamarin.Android 응용 프로그램에서 Java 라이브러리를 다시 사용 가능한 세 가지가 있습니다. 

-   **Java 바인딩 라이브러리를 만들어** &ndash; 이 기술을 사용 하 여 Xamarin.Android 프로젝트 C# Java 형식 둘러싼 래퍼를 만드는 데 사용 됩니다. Xamarin.Android 응용 프로그램에서 다음이 프로젝트에서 만든 C# 래퍼를 참조 하 고 다음 사용할 수는 `.jar` 파일입니다. 

-   **Java 기본 인터페이스** &ndash; 는 *Java 네이티브* *인터페이스* (JNI)은 비 Java 코드 (예: c + + 또는 C#)를 호출 하거나 JVM 내에서 실행 되는 Java 코드에서 호출할 수 있도록 프레임 워크 . 

-   **코드를 포팅하** &ndash; 이 방법은 Java 소스 코드를 작성 하 고 다음 C#으로 변환 합니다. 수동으로 또는 선명 같은 자동화 된 도구를 사용 하 여 수행할 수 있습니다. 

처음 두 기술의 핵심은는 *Java 기본 인터페이스* (JNI). JNI 응용 프로그램을 Java로 작성 된 하지 Java 가상 컴퓨터에서 실행 되는 Java 코드와 상호 작용 하는 프레임 워크입니다. Xamarin.Android JNI를 사용 하 여 만든 *바인딩* C# 코드에 대 한 합니다. 

첫 번째 방법은 Java 라이브러리 바인딩 하는 자동화 된, 선언적 접근 방식입니다. Visual Studio를 이용 하 여 Mac 또는 Xamarin.Android에서 제공 되는 Visual Studio 프로젝트 형식에 대 한 &ndash; Java 바인딩 라이브러리입니다. 이러한 바인딩은 성공적으로 만들려면 Java 바인딩 라이브러리 여전히 필요할 수 있습니다 수동 약간 수정 하지만 마찬가지로 개수 만큼은 순수 JNI 방법을 합니다. 참조 [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md) Java 바인딩 라이브러리에 대 한 자세한 내용은 합니다. 

JNI를 사용 하 여 두 번째 기술은 훨씬는 더 낮은 수준에서 작동 하지만 자세히 제어 하려면을 제공 하지 일반적으로 액세스할 수는 Java 바인딩 라이브러리를 통해 Java 메서드 액세스 합니다. 

세 번째 방법은 이전 두 근본적으로 다릅니다: C#에 대 한 Java 코드를 이식 합니다. 다른 언어에서 코드 포팅에 매우 어려운 프로세스 일 수 있습니다 하지만 도구를 사용 하 여 노력에 호출 되도록 줄일 수 *선명*합니다. 선명 하 게는 Java 되는 오픈 소스 도구-에-C# 변환기입니다. 



## <a name="summary"></a>요약

이 문서는 Xamarin.Android 응용 프로그램에서 Java에서 라이브러리를 다시 사용할 수는 여러 가지 방법 중 일부 상위 수준 개요를 제공 합니다. 플랫폼 호출 가능 래퍼를 관리 및 바인딩의 개념을 도입 Java 코드를 C# 이식에 대 한 옵션을 설명 합니다. 


## <a name="related-links"></a>관련 링크

- [아키텍처](~/android/internals/architecture.md)
- [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)
- [JNI 작업](~/android/platform/java-integration/working-with-jni.md)
- [선명하게](https://github.com/slluis/sharpen)
- [Java 기본 인터페이스](http://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
