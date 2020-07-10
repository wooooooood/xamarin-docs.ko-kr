---
title: Android Kotlin 라이브러리 바인딩
description: 이 문서에서는 Kotlin 코드에 대한 C# 바인딩을 만들어 Xamarin.Android 애플리케이션에서 네이티브 라이브러리를 사용할 수 있도록 하는 방법에 대해 설명합니다.
ms.prod: xamarin
ms.assetid: AB03A6C4-5A5A-4EAD-AD51-D887B20A3551
ms.technology: xamarin-android
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: d2251a0219ca25ecd37e42361e49ac966bac131d
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85852991"
---
# <a name="bind-android-kotlin-libraries"></a>Android Kotlin 라이브러리 바인딩

> [!IMPORTANT]
> 현재 Xamarin 플랫폼에서 사용자 지정 바인딩 사용을 조사하고 있습니다. [**설문 조사**](https://www.surveymonkey.com/r/KKBHNLT)에 참여하여 향후 개발 작업에 대해 알려 주시기 바랍니다.

Android 플랫폼은 해당 네이티브 언어 및 도구와 함께 지속적으로 진화하고 있으며 최신 제품을 사용하여 개발된 타사 라이브러리가 많이 있습니다. 코드 및 구성 요소 재사용을 최대화하는 것은 플랫폼 간 개발의 주요 목표 중 하나입니다. Kotlin을 사용하여 빌드된 구성 요소를 다시 사용하는 기능이 Xamarin 개발자 사이에서 인기가 점점 높아지면서 그 중요성도 높아지고 있습니다. 일반적인 [Java](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/) 라이브러리를 바인딩하는 프로세스는 이미 잘 알고 있을 것입니다. 현재 [Kotlin 라이브러리 바인딩](walkthrough.md) 프로세스를 설명하는 추가 설명서가 제공되므로 Xamarin 애플리케이션에서 동일한 방식으로 라이브러리를 사용할 수 있습니다. 이 문서의 목적은 Xamarin에 대한 Kotlin 바인딩을 만드는 개략적인 방법을 설명하는 것입니다.

## <a name="high-level-approach"></a>개략적인 접근법

Xamarin을 사용하면 Xamarin 애플리케이션에서 사용 가능한 타사 네이티브 라이브러리를 빌드할 수 있습니다. Kotlin은 새 언어이며 이 언어로 빌드된 라이브러리에 대한 바인딩을 만들려면 몇 가지 추가 단계와 도구가 필요합니다. 이 방법은 다음 4단계를 포함합니다.

1. 네이티브 라이브러리 빌드
1. Xamarin 도구에서 클래스를 생성할 수 있도록 하는 Xamarin 메타데이터 준비
1. 네이티브 라이브러리 및 메타데이터를 사용하여 Xamarin 바인딩 라이브러리 빌드
1. Xamarin 애플리케이션에서 Xamarin 바인딩 라이브러리 사용

다음 섹션에서는 추가 정보를 사용하여 이러한 단계를 간략하게 설명합니다.

### <a name="build-the-native-library"></a>네이티브 라이브러리 빌드

첫 번째 단계는 네이티브 Kotlin 라이브러리(Android 압축 파일인 AAR 패키지)를 가져오는 것입니다. 공급업체에 직접 요청하거나 직접 빌드할 수 있습니다.

### <a name="prepare-the-xamarin-metadata"></a>Xamarin 메타데이터 준비

두 번째 단계는 Xamarin 도구에서 해당 C# 클래스를 생성하는 데 사용되는 메타데이터 변환 파일을 준비하는 것입니다. 모범 사례 시나리오에서 이 파일은 비어 있을 수 있으며 Xamarin 도구에서 모든 클래스를 검색하고 생성합니다. 경우에 따라 올바른 및/또는 원하는 C# 코드를 생성하려면 메타데이터 변환을 적용해야 합니다. 대부분의 경우 AAR 디스어셈블러([Java 디컴파일러(JD)](http://java-decompiler.github.io/))를 사용하여 생성될 클래스의 C# 최종 목록에서 제외하려는 숨겨진 종속성 및 원치 않는 클래스를 확인해야 합니다. 최종 메타데이터는 참조하는 Xamarin.Android 애플리케이션이 상호 작용하는 퍼블릭 인터페이스를 나타내야 합니다.

### <a name="build-a-xamarinandroid-binding-library"></a>Xamarin.Android 바인딩 라이브러리 빌드

세 번째 단계는 Xamarin.Android 바인딩 라이브러리와 같은 특수 프로젝트를 만드는 것입니다. 여기에는 Kotlin 라이브러리가 네이티브 참조로 포함되고 이전 단계에서 정의된 메타데이터 변환이 포함됩니다. 작성 시 참조할 각 AAR 패키지에 별도의 Android 바인딩 라이브러리 프로젝트가 필요합니다. 바인딩 라이브러리는 Kotlin 표준 라이브러리를 지원하기 위해 [Xamarin.Kotlin.StdLib](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/) 패키지를 추가해야 합니다.

### <a name="consume-the-xamarin-binding-library"></a>Xamarin 바인딩 라이브러리 사용

네 번째 단계와 마지막 단계는 Xamarin.Android 애플리케이션에서 바인딩 라이브러리를 참조하는 것입니다. Xamarin.Android 바인딩 라이브러리에 대한 참조를 추가하면 Xamarin 애플리케이션이 해당 패키지 내에서 노출된 Kotlin 클래스를 사용할 수 있습니다.

## <a name="walkthrough"></a>연습

위의 접근법은 Xamarin에 대한 Kotlin 바인딩을 만드는 데 필요한 개략적인 단계를 간략하게 설명합니다. 이러한 바인딩을 실제로 준비할 때는 여러 세부적인 단계가 수반되며 네이티브 도구 및 언어의 변경 사항을 적용하는 등 추가 세부 정보를 고려해야 합니다. 이를 통해 이 개념과 이 프로세스와 관련된 개략적인 단계를 보다 깊이 있게 이해할 수 있습니다. 자세한 단계별 가이드는 [Xamarin Kotlin 바인딩 연습](walkthrough.md) 설명서를 참조하세요.

## <a name="related-links"></a>관련 링크

- [Android Studio](https://developer.android.com/studio)
- [Gradle 설치](https://gradle.org/install/)
- [Mac용 Visual Studio](https://visualstudio.microsoft.com/downloads)
- [Java 디컴파일러](http://java-decompiler.github.io/)
- [BubblePicker Kotlin 라이브러리](https://github.com/igalata/Bubble-Picker)
- [Java 라이브러리 바인딩](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/)
- [XPath](https://www.w3.org/TR/xpath/)
- [Java 바인딩 메타데이터](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata)
- [Xamarin.Kotlin.StdLib NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/)
- [샘플 프로젝트 리포지토리](https://github.com/xamcat/xamarin-binding-kotlin-framework)
