---
title: Xamarin.Android와 Java 통합
description: Java 에코시스템에는 다양하고 방대한 구성 요소 컬렉션이 포함되어 있습니다. 해당 구성 요소 중 다수가 Android 애플리케이션을 개발하는 데 걸리는 시간을 줄이는 데 사용될 수 있습니다. 이 문서에서는 개발자가 해당 기존 Java 구성 요소를 사용하여 Xamarin.Android 애플리케이션 환경을 개선하기 위한 몇 가지 방법의 개략적인 개요를 소개하고 제공합니다.
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/18/2017
ms.openlocfilehash: ecaa02e036c74074b4fa922ea079355b72ff02e2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73020094"
---
# <a name="java-integration-with-xamarinandroid"></a>Xamarin.Android와 Java 통합

_Java 에코시스템에는 다양하고 방대한 구성 요소 컬렉션이 포함되어 있습니다. 해당 구성 요소 중 다수가 Android 애플리케이션을 개발하는 데 걸리는 시간을 줄이는 데 사용될 수 있습니다. 이 문서에서는 개발자가 해당 기존 Java 구성 요소를 사용하여 Xamarin.Android 애플리케이션 환경을 개선하기 위한 몇 가지 방법의 개략적인 개요를 소개하고 제공합니다._

## <a name="overview"></a>개요

Java 에코 시스템의 방대한 규모를 생각하면 Xamarin.Android 애플리케이션에 필요한 기능은 무엇이든 이미 Java로 코딩되어 있을 가능성이 큽니다. 따라서 Xamarin.Android 애플리케이션을 만들 때 해당 기존 라이브러리를 재사용하는 것은 무척 매력적인 일이 됩니다.

Xamarin.Android 애플리케이션에서 Java 라이브러리를 재사용하는 방법에는 3가지가 있습니다. 

- **Java 바인딩 라이브러리 만들기** &ndash; 이 방법에서는 Xamarin.Android 프로젝트가 Java 형식을 감싸는 C# 래퍼를 만드는 데 사용됩니다. 이렇게 하면 Xamarin.Android 애플리케이션이 이 프로젝트에 의해 만들어진 C# 래퍼를 참조하고 `.jar` 파일을 사용할 수 있습니다. 

- **Java 기본 인터페이스** &ndash; JNI(‘Java 기본’ ‘인터페이스’)는 C++, C#과 같은 Java가 아닌 코드에서 JVM 내에서 실행되는 Java 코드를 호출하거나 JVM 내에서 실행되는 Java 코드에서 호출되도록 지원하는 프레임워크입니다.   

- **코드 이식** &ndash; 이 방법에서는 Java 소스 코드를 C#으로 변환합니다. 이 작업은 수동으로 할 수도 있고 Sharpen과 같은 자동화 도구를 사용하여 할 수도 있습니다. 

처음 두 가지 방법의 핵심에는 JNI(‘Java 기본 인터페이스’)가 있습니다.  JNI는 Java로 작성되지 않은 애플리케이션이 Java Virtual Machine 내에서 실행되는 Java 코드와 상호 작용할 수 있도록 허용하는 프레임워크입니다. Xamarin.Android는 JNI를 사용하여 C# 코드에 대한 ‘바인딩’을 만듭니다.  

첫 번째 방법은 Java 라이브러리를 바인딩하는 보다 자동화되고 선언적인 접근 방식입니다. 이 방법에서는 Mac용 Visual Studio를 사용하거나 Xamarin.Android에서 제공하는 Visual Studio 프로젝트 형식인 Java 바인딩 라이브러리를 사용합니다. 해당 바인딩을 만들려면 Java 바인딩 라이브러리를 수동으로 수정해야 할 수 있으나, 순수한 JNI 접근 방식만큼 수정이 많이 필요하지는 않습니다. Java 바인딩 라이브러리에 대한 자세한 내용은 [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)을 참조하세요. 

JNI를 사용하는 두 번째 방법은 훨씬 더 낮은 수준에서 작동하지만 Java 바인딩 라이브러리를 통해 액세스하기 어려운 Java 메서드에 액세스할 수 있고 세부적인 제어가 가능합니다. 

세 번째 방법은 앞의 두 가지 방법과 크게 다릅니다. 바로 Java에서 C#으로 코드를 이식하는 것입니다. 하나의 언어에서 다른 언어로 코드를 이식하는 것은 매우 까다로운 작업일 수 있지만, *Sharpen*이라는 유용한 도구를 사용하면 여기에 필요한 작업을 줄일 수 있습니다. Sharpen은 오픈 소스 Java-C# 변환기 도구입니다. 

## <a name="summary"></a>요약

이 문서에서는 Java 라이브러리를 Xamarin.Android 애플리케이션에서 사용하는 몇 가지 방법의 개략적인 개요를 제공했습니다. 바인딩 및 관리되는 호출 가능 래퍼의 개념을 소개하고, Java 코드를 C#으로 이식하는 옵션을 설명했습니다. 

## <a name="related-links"></a>관련 링크

- [아키텍처](~/android/internals/architecture.md)
- [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)
- [JNI 사용](~/android/platform/java-integration/working-with-jni.md)
- [선명하게](https://github.com/slluis/sharpen)
- [Java 기본 인터페이스](https://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
