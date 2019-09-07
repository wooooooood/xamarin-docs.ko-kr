---
title: Xamarin.ios와 Java 통합
description: Java 에코 시스템에는 다양 한 때 엄청난 구성 요소 컬렉션이 포함 되어 있습니다. 이러한 많은 구성 요소를 사용 하 여 Android 응용 프로그램을 개발 하는 데 걸리는 시간을 줄일 수 있습니다. 이 문서에서는 개발자가 이러한 기존 Java 구성 요소를 사용 하 여 Xamarin Android 응용 프로그램 개발 환경을 개선할 수 있는 몇 가지 방법에 대 한 개략적인 개요를 소개 합니다.
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/18/2017
ms.openlocfilehash: a9d239140cee9eb600414a1bfb0733c9af6488bc
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761420"
---
# <a name="java-integration-with-xamarinandroid"></a>Xamarin.ios와 Java 통합

_Java 에코 시스템에는 다양 한 때 엄청난 구성 요소 컬렉션이 포함 되어 있습니다. 이러한 많은 구성 요소를 사용 하 여 Android 응용 프로그램을 개발 하는 데 걸리는 시간을 줄일 수 있습니다. 이 문서에서는 개발자가 이러한 기존 Java 구성 요소를 사용 하 여 Xamarin Android 응용 프로그램 개발 환경을 개선할 수 있는 몇 가지 방법에 대 한 개략적인 개요를 소개 합니다._

## <a name="overview"></a>개요

Java 에코 시스템의 범위가 지정 된 경우 Xamarin.ios 응용 프로그램에 필요한 모든 기능이 이미 Java로 코딩 되었을 가능성이 높습니다. 따라서 Xamarin Android 응용 프로그램을 만들 때 이러한 기존 라이브러리를 사용해 보고 다시 사용 하는 것이 좋습니다.

Xamarin Android 응용 프로그램에서 Java 라이브러리를 다시 사용할 수 있는 세 가지 방법이 있습니다. 

- **Java 바인딩 라이브러리 만들기** 이 기술을 사용 하면 Xamarin Android 프로젝트를 사용 하 여 Java 형식 C# 주위에 래퍼를 만들 수 있습니다. &ndash; 그런 다음 Xamarin Android 응용 프로그램은이 프로젝트 C# 에서 만든 래퍼를 참조 한 다음 `.jar` 파일을 사용할 수 있습니다. 

- **Java Native 인터페이스** C++ C# JNI (java Native Interface)는 JVM 내에서 실행 되는 java 코드에 의해 호출 되거나 호출 될 수 있는 비 java 코드 (예: 또는)를 허용 하는 프레임 워크입니다. &ndash; 

- **코드를 이식 합니다** . 이 메서드는 Java 소스 코드를 가져온 다음로 변환 하 C#는 과정을 포함 합니다. &ndash; 이러한 작업은 수동으로 수행 하거나 선명도와 같은 자동화 된 도구를 사용 하 여 수행할 수 있습니다. 

처음 두 기술의 핵심은 *Java Native Interface* (JNI)입니다. JNI은 Java로 작성 되지 않은 응용 프로그램을 Java Virtual Machine에서 실행 되는 Java 코드와 상호 작용 하는 데 사용할 수 있는 프레임 워크입니다. Xamarin.ios는 JNI을 사용 하 여 코드에 C# 대 한 바인딩을 만듭니다. 

첫 번째 방법은 Java 라이브러리를 바인딩하는 보다 자동화 된 선언적 방법입니다. Mac용 Visual Studio 또는 xamarin.ios &ndash; 에서 제공 하는 Visual Studio 프로젝트 형식 (Java 바인딩 라이브러리)을 사용 해야 합니다. 이러한 바인딩을 성공적으로 만들려면 Java 바인딩 라이브러리에 몇 가지 수동 수정이 필요 하지만 순수한 JNI 접근 방식은 많지 않습니다. Java 바인딩 라이브러리에 대 한 자세한 내용은 [Java 라이브러리 바인딩을](~/android/platform/binding-java-library/index.md) 참조 하세요. 

JNI를 사용 하는 두 번째 기술은 매우 낮은 수준에서 작동 하지만 일반적으로 Java 바인딩 라이브러리를 통해 액세스할 수 없는 Java 메서드에 대 한 보다 세부적인 제어 및 액세스를 제공할 수 있습니다. 

세 번째 기술은 이전 두 가지 방법 (Java에서로 C#코드 포팅)과 완전히 다릅니다. 코드를 한 언어에서 다른 언어로 이식 하는 것은 매우 힘든 프로세스 일 수 있지만, *선명*하 게 하는 도구를 사용 하 여 이러한 작업을 줄일 수 있습니다. 선명 효과는 Java에서C# 변환기 인 오픈 소스 도구입니다. 

## <a name="summary"></a>요약

이 문서에서는 Xamarin Android 응용 프로그램에서 Java의 라이브러리를 다시 사용할 수 있는 여러 가지 방법에 대 한 개략적인 개요를 제공 했습니다. 바인딩 및 관리 되는 호출 가능 래퍼의 개념과 Java 코드를로 C#이식 하기 위한 옵션에 대해 소개 했습니다. 

## <a name="related-links"></a>관련 링크

- [아키텍처](~/android/internals/architecture.md)
- [Java 라이브러리 바인딩](~/android/platform/binding-java-library/index.md)
- [JNI 사용](~/android/platform/java-integration/working-with-jni.md)
- [선명하게](https://github.com/slluis/sharpen)
- [Java Native 인터페이스](http://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
