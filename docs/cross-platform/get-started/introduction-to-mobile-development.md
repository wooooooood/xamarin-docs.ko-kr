---
title: "모바일 개발 소개"
ms.topic: article
ms.prod: xamarin
ms.assetid: 33C83E13-F3E5-17B4-6512-207F3D3C5AB6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/28/2017
ms.openlocfilehash: d3a39070e17729e99614e26e0a8892c6314512af
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="introduction-to-mobile-development"></a>모바일 개발 소개

모바일 응용 프로그램 빌드는 IDE를 열고, 함께 준비하고, 신속히 테스트하며 앱 스토어에 제출하는 것처럼 손쉽게 수행할 수 있습니다. 모든 작업이 그 날 완료됩니다. 또는 엄격한 사전 설계, 유용성 테스트, 수천 대의 장치에서 QA 테스트, 전체 베타 주기 및 여러 가지 다른 방식의 배포가 포함된 매우 복잡한 프로세스일 수 있습니다.

이 문서는 Xamarin 플랫폼을 소개하기 위해 작성되었습니다. 설계부터 테스트에 이르는 모바일 응용 프로그램을 빌드하는 *프로세스*에 대한 자세한 내용은 [모바일 소프트웨어 개발 수명 주기 소개](~/cross-platform/get-started/introduction-to-mobile-sdlc.md) 문서를 참조하세요.

Xamarin을 설치할 수 있는지 확인하려면 [시스템 요구 사항](~/cross-platform/get-started/requirements.md#mac)을 참조하세요.

## <a name="introduction-to-xamarin"></a>Xamarin 소개

iOS 및 Android 응용 프로그램을 빌드하는 방법을 고려할 때, 많은 사람은 Objective-C, Swift 및 Java와 같은 기본 언어가 유일한 선택이라고 생각합니다. 그러나 지난 몇 년 동안, 모바일 응용 프로그램을 빌드하기 위한 플랫폼의 완전히 새로운 에코시스템이 등장했습니다.

Xamarin은 iOS, Android 및 Windows Phone의 세 가지 모든 모바일 플랫폼에서 작동하는 C#, 클래스 라이브러리 및 런타임이라는 단일 언어를 제공하면서(Windows Phone의 네이티브 언어는 이미 C#임) 까다로운 게임에 대해서도 충분한 성능을 제공하는 컴파일 네이티브(해석되지 않음) 응용 프로그램입니다.

이러한 각 플랫폼은 서로 다른 기능 집합을 가지고 있으며 네이티브 응용 프로그램을 작성하는 기능도 다릅니다. 즉, 네이티브 코드로 컴파일되고 기본 Java 하위 시스템과 원활하게 상호 운용하는 응용 프로그램입니다. 예를 들어, 일부 플랫폼에서는 HTML 및 JavaScript로만 앱을 빌드할 수 있지만 일부는 매우 하위 수준이며 C/C++ 코드만 허용합니다. 일부 플랫폼은 기본 제어 도구 키트를 활용하지 않습니다.

Xamarin은 네이티브 플랫폼의 모든 기능을 결합하고 다음과 같은 고유하고 강력한 여러 기능을 부여한다는 점에서 독보적입니다.

1.   **기본 SDK에 대한 전체적인 바인딩** - Xamarin에는 iOS 및 Android에서 거의 모든 기본 플랫폼 SDK에 대한 바인딩이 포함되어 있습니다. 또한 이러한 바인딩은 강력한 형식이므로 탐색 및 사용하기 쉽고 개발 중에도 강력한 컴파일 시간 형식 확인을 제공합니다. 이로 인해 런타임 오류가 줄어들고 응용 프로그램의 품질이 향상됩니다.
1.   **Objective-C, Java, C 및 C++ Interop** - Xamarin은 Objective-C, Java, C 및 C++ 라이브러리를 직접 호출할 수 있는 기능을 제공하므로 이미 생성된 다양한 타사 코드를 사용할 수 있습니다. 이를 통해 Objective-C, Java 또는 C/C++로 작성된 기존 iOS 및 Android 라이브러리를 활용할 수 있습니다. 또한 Xamarin은 선언적 구문을 사용하여 네이티브 Objective-C 및 Java 라이브러리를 쉽게 바인딩할 수 있는 바인딩 프로젝트를 제공합니다.
1.   **현대적인 언어 구문** - Xamarin 응용 프로그램은 *Lambdas* ,  *LINQ* ,  *병렬 프로그래밍* 기능, 정교한 *제네릭*과 같은 *기능적 구문*, *동적 언어 기능*과 같은 Objective-C 및 Java보다 크게 향상된 기능을 포함하는 현대적인 언어인 C#으로 작성됩니다.
1.   **뛰어난 BCL(기본 클래스 라이브러리)** - Xamarin 응용 프로그램은 강력한 XML, 데이터베이스, 직렬화, IO, 문자열 및 네트워킹 지원 등과 같은 포괄적이고 간소화된 기능을 갖춘 방대한 클래스의 컬렉션인 .NET BCL을 사용합니다. 또한 기존 C# 코드를 응용 프로그램에서 사용할 수 있도록 컴파일할 수 있으며 수천 수만 개의 라이브러리에 액세스할 수 있으므로 BCL에서 다루지 않은 것도 수행할 수 있습니다.
1.   **현대적인 IDE(통합 개발 환경)** - Xamarin은 Mac OS X에서 Mac용 Visual Studio를 사용하고 Windows에서 Visual Studio를 사용합니다. 코드 자동 완성, 정교한 프로젝트 및 솔루션 관리 시스템, 포괄적인 프로젝트 템플릿 라이브러리, 통합 소스 제어 및 기타 여러 기능을 포함하는 현대적인 IDE입니다.
1.   **모바일 플랫폼 간 지원** - Xamarin은 iOS, Android 및 Windows Phone의 세 가지 주요 모바일 플랫폼에 대해 정교한 플랫폼 간 지원을 제공합니다. 응용 프로그램은 코드의 최대 90%를 공유하도록 작성할 수 있으며 Xamarin.Mobile 라이브러리는 세 가지 모든 플랫폼 간에 공통의 리소스에 액세스할 수있는 통합 API를 제공합니다. 이를 통해 가장 널리 사용되는 세 가지 모바일 플랫폼을 대상으로 하는 모바일 개발자의 개발 비용과 출시 시간을 크게 줄일 수 있습니다.


Xamarin의 강력하고 포괄적인 기능 집합으로 인해, 플랫폼 간 모바일 응용 프로그램을 개발하기 위해 현대적인 언어 및 플랫폼을 사용하려는 응용 프로그램 개발자에게 도움이 됩니다.


> [!NOTE]
> 이 시작 시리즈에서는 iOS 및 Android 응용 프로그램 빌드를 시작하는 데 중점을 둡니다. Microsoft는 Windows Phone 개발을 위한 자습서를 [여기](http://dev.windowsphone.com/en-us/develop)에서 제공합니다. Xamarin(Windows용 UWP 앱 포함)을 사용한 플랫폼 간 개발에 대한 자세한 내용은 [플랫폼 간 응용 프로그램 빌드 가이드](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)를 읽어 보세요.



## <a name="how-does-xamarin-work"></a>Xamarin 작동 방식

Xamarin은 Xamarin.iOS 및 Xamarin.Android라는 두 가지 상업용 제품을 제공합니다. 둘 다 게시된 .NET ECMA 표준을 기반으로 한 .NET Framework의 오픈 소스 버전인 *Mono*를 기반으로 합니다. Mono는 .NET Framework만큼 오래되었으며 Linux, Unix, FreeBSD, Mac OS X 등 떠올릴 수 있는 거의 모든 플랫폼에서 실행됩니다.

iOS, Xamarin의 *AOT*(*Ahead-of-Time*) 컴파일러는 Xamarin.iOS 응용 프로그램을 네이티브 ARM 어셈블리 코드로 직접 컴파일합니다. Android에서는 Xamarin의 컴파일러가 *IL*(*중간 언어*)로 컴파일한 후 응용 프로그램이 시작될 때 네이티브 어셈블리로 *JIT*(*Just in Time*) 컴파일됩니다.

두 경우 모두, Xamarin 응용 프로그램은 메모리 할당, 가비지 수집, 기본 플랫폼 상호 운용 등을 자동으로 처리하는 런타임을 활용합니다.



### <a name="xamariniosdll-and-monoandroiddll"></a>Xamarin.iOS.dll 및 Mono.Android.dll

Xamarin 응용 프로그램은 Xamarin Mobile Profile로 알려진 .NET BCL의 하위 집합에 대해 빌드됩니다. 이 프로필은 모바일 응용 프로그램에 맞게 특별히 작성되어 MonoTouch.dll 및 Mono.Android.dll(각각 iOS 및 Android용)에 패키지됩니다 이 방식은 Silverlight/Moonlight .NET 프로필에 대해 Silverlight(및 Moonlight) 응용 프로그램이 빌드된 방식과 매우 유사합니다. 사실, Xamarin Mobile Profile은 BCL 클래스가 다시 추가된 Silverlight 4.0 프로필과 동일합니다.

사용 가능한 어셈블리 및 클래스의 전체 목록은 [Xamarin.iOS 어셈블리 목록](~/cross-platform/internals/available-assemblies.md) 및 [Xamarin.Android 어셈블리 목록](~/cross-platform/internals/available-assemblies.md)을 참조하세요.

BCL 외에도, 이러한 .dll에는 C#에서 기본 SDK API를 직접 호출할 수 있는 거의 모든 iOS SDK 및 Android SDK용 래퍼가 포함되어 있습니다.



### <a name="application-output"></a>응용 프로그램 출력

Xamarin 응용 프로그램이 컴파일되면 결과는 응용 프로그램 패키지(iOS에서는 .app 파일, Android에서는 .apk 파일)로 생성됩니다. 이러한 파일은 플랫폼의 기본 IDE로 작성된 응용 프로그램 패키지와 구별할 수 없으며 동일한 방식으로 배포할 수 있습니다.



## <a name="getting-started"></a>시작

지금까지 Xamarin의 작동 방식에 대해 간단히 알아보았으며 이제 시작해 보겠습니다.

다음 단계는 다음 지침 중 하나를 사용하여 앱을 빌드하는 것입니다.

* [**Hello, iOS**](~/ios/get-started/hello-ios/index.md)

![](introduction-to-mobile-development-images/ios.png "Hello, iOS")


* [**Hello, Android**](~/android/get-started/hello-android/index.md)

![](introduction-to-mobile-development-images/android.png "Hello, Android")


* [**Xamarin.Forms 소개**](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)





## <a name="summary"></a>요약

이 문서에서는 단순히 Xamarin 플랫폼을 소개했습니다. 진정한 재미는 첫 번째 앱을 가동 및 실행시킬 때부터 시작됩니다. 시작하려면 [Hello, iOS](~/ios/get-started/hello-ios/index.md), [Hello, Android](~/android/get-started/hello-android/index.md) 및 [Xamarin.Forms 소개](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md) 가이드를 확인해 보세요.


## <a name="related-links"></a>관련 링크

- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, Android](~/android/get-started/hello-android/index.md)
