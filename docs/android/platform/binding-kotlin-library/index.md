---
title: Android Kotlin 라이브러리 바인딩
description: 이 문서에서는 Kotlin 코드에 C# 대 한 바인딩을 만들어 xamarin.ios 응용 프로그램에서 네이티브 라이브러리를 사용할 수 있도록 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: AB03A6C4-5A5A-4EAD-AD51-D887B20A3551
ms.technology: xamarin-android
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: ec7d154b0d7fcb055bd398089e142fe8b1d9f60e
ms.sourcegitcommit: b751605179bef8eee2df92cb484011a7dceb6fda
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77497967"
---
# <a name="bind-android-kotlin-libraries"></a>Android Kotlin 라이브러리 바인딩

Android 플랫폼은 해당 네이티브 언어 및 도구와 함께 지속적으로 진화 하 고 있으며 최신 제품을 사용 하 여 개발 된 많은 타사 라이브러리도 있습니다. 코드 및 구성 요소 재사용을 최대화 하는 것은 플랫폼 간 개발의 주요 목표 중 하나입니다. Kotlin를 사용 하 여 빌드된 구성 요소를 다시 사용 하는 기능은 개발자의 인기를 지속적으로 성장 하는 Xamarin 개발자에 게 점점 더 중요 해지고 있습니다. 이미 일반적인 [Java](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/) 라이브러리를 바인딩하는 프로세스에 대해 잘 알고 있을 수 있습니다. 이제 [Kotlin 라이브러리 바인딩](walkthrough.md)프로세스를 설명 하는 추가 설명서를 사용할 수 있으므로 동일한 방식으로 Xamarin 응용 프로그램에서 사용할 수 있습니다. 이 문서의 목적은 Xamarin에 대 한 Kotlin 바인딩을 만드는 개략적인 방법을 설명 하는 것입니다.

## <a name="high-level-approach"></a>상위 수준 접근 방식

Xamarin을 사용 하면 Xamarin 응용 프로그램에서 사용할 수 있도록 타사 네이티브 라이브러리를 바인딩할 수 있습니다. Kotlin는 새 언어 이며이 언어로 작성 된 라이브러리에 대 한 바인딩을 만들려면 몇 가지 추가 단계와 도구가 필요 합니다. 이 방법은 다음 네 단계를 포함 합니다.

1. 네이티브 라이브러리 빌드
1. Xamarin 도구에서 클래스를 생성할 C# 수 있도록 하는 xamarin 메타 데이터 준비
1. 네이티브 라이브러리 및 메타 데이터를 사용 하 여 Xamarin 바인딩 라이브러리 빌드
1. Xamarin 응용 프로그램에서 Xamarin 바인딩 라이브러리 사용

다음 섹션에서는 추가 정보를 사용 하 여 이러한 단계를 간략하게 설명 합니다.

### <a name="build-the-native-library"></a>네이티브 라이브러리 빌드

첫 번째 단계는 네이티브 Kotlin 라이브러리 (Android archive AAR package)를 가져오는 것입니다. 공급 업체에서 직접 요청 하거나 직접 빌드할 수 있습니다.

### <a name="prepare-the-xamarin-metadata"></a>Xamarin 메타 데이터 준비

두 번째 단계는 Xamarin 도구에서 해당 C# 클래스를 생성 하는 데 사용 되는 메타 데이터 변환 파일을 준비 하는 것입니다. 모범 사례 시나리오에서이 파일은 모든 클래스가 Xamarin 도구에서 검색 되 고 생성 되는 비어 있을 수 있습니다. 경우에 따라 올바른 및/또는 원하는 C# 코드를 생성 하려면 메타 데이터 변환을 적용 해야 합니다. 대부분의 경우 [Java 디컴파일러 (JD)](http://java-decompiler.github.io/)와 같은 AAR 디스어셈블러를 사용 하 여 생성 될 클래스의 C# 최종 목록에서 제외 하려는 숨겨진 종속성 및 원치 않는 클래스를 식별 해야 합니다. 최종 메타 데이터는 참조 하는 Xamarin.ios 응용 프로그램이와 상호 작용 하는 공용 인터페이스를 나타내야 합니다.

### <a name="build-a-xamarinandroid-binding-library"></a>Xamarin Android 바인딩 라이브러리 빌드

세 번째 단계는 Xamarin Android 바인딩 라이브러리와 같은 특수 프로젝트를 만드는 것입니다. Kotlin 라이브러리를 네이티브 참조로 포함 하 고 이전 단계에서 정의 된 메타 데이터 변환을 포함 합니다. 문서를 작성할 때 참조할 각 AAR 패키지에 별도의 Android 바인딩 라이브러리 프로젝트가 필요 합니다. 바인딩 라이브러리는 Kotlin 표준 라이브러리를 지원 하기 위해 [Kotlin stdlib.h](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/) 패키지를 추가 해야 합니다.

### <a name="consume-the-xamarin-binding-library"></a>Xamarin 바인딩 라이브러리 사용

네 번째 단계와 마지막 단계는 Xamarin.ios 응용 프로그램에서 바인딩 라이브러리를 참조 하는 것입니다. Xamarin Android 바인딩 라이브러리에 대 한 참조를 추가 하면 Xamarin 응용 프로그램이 해당 패키지 내에서 노출 된 Kotlin 클래스를 사용할 수 있습니다.

## <a name="walkthrough"></a>연습

위의 방법은 Xamarin에 대 한 Kotlin 바인딩을 만드는 데 필요한 개략적인 단계를 간략하게 설명 합니다. 기본 도구 및 언어의 변경 사항을 적용 하는 것을 포함 하 여 이러한 바인딩을 준비 하는 경우에는 더 낮은 수준의 단계와 관련 된 추가 정보를 확인할 수 있습니다. 이를 통해이 개념과이 프로세스와 관련 된 개략적인 단계를 보다 깊이 있게 이해할 수 있습니다. 자세한 단계별 가이드는 [Xamarin Kotlin Binding 연습](walkthrough.md) 설명서를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [Android Studio](https://developer.android.com/studio)
- [Gradle 설치](https://gradle.org/install/)
- [Mac용 Visual Studio](https://visualstudio.microsoft.com/downloads)
- [Java 디컴파일러](http://java-decompiler.github.io/)
- [BubblePicker Kotlin 라이브러리](https://github.com/igalata/Bubble-Picker)
- [Java 라이브러리 바인딩](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/)
- [XPath](https://www.w3.org/TR/xpath/)
- [Java 바인딩 메타 데이터](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata)
- [Kotlin. Stdlib.h NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/)
- [샘플 프로젝트 리포지토리](https://github.com/xamcat/xamarin-binding-kotlin-framework)
