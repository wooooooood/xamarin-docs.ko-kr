---
ms.assetid: EA2D979E-9151-4CE9-9289-13B6A979838B
title: Xamarin에서 C/C++ 라이브러리 사용
description: Xamarin 및 C#를 사용하여 플랫폼 간 C/C++ 코드를 빌드하고 Android 및 iOS용 모바일 앱으로 통합하는 데 Mac용 Visual Studio를 사용할 수 있습니다. 이 문서에서는 Xamarin 앱에서 C++ 프로젝트를 설정하고 디버그하는 방법을 설명합니다.
author: mikeparker104
ms.author: miparker
ms.date: 11/07/2019
ms.openlocfilehash: 42a59570d727657b2f3c23bd9d1f37e1205717d0
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73842815"
---
# <a name="use-cc-libraries-with-xamarin"></a>Xamarin에서 C/C++ 라이브러리 사용

## <a name="overview"></a>개요

Xamarin을 통해 개발자는 Visual Studio를 사용하여 플랫폼 간 네이티브 모바일 앱을 만들 수 있습니다. 일반적으로 C# 바인딩은 기존 플랫폼 구성 요소를 개발자에게 노출하는 데 사용됩니다. 그러나 Xamarin 앱에서 기존 코드베이스를 사용해야 하는 경우도 있습니다. 때로는 팀이 잘 테스트되고 최적화된 대규모 코드베이스를 C#로 이식할 시간, 예산 또는 리소스가 없는 경우가 있습니다.

[플랫폼 간 모바일 개발용 Visual C++](https://docs.microsoft.com/visualstudio/cross-platform/visual-cpp-for-cross-platform-mobile-development)를 사용하면 C/C++ 및 C# 코드를 동일한 솔루션의 일부로 작성하여 통합된 디버깅 환경을 비롯한 다양한 이점을 제공할 수 있습니다. Microsoft는 이러한 방식으로C/C++ 및 Xamarin을 사용하여 [Hyperlapse Mobile](https://www.microsoft.com/p/hyperlapse-mobile/9wzdncrd1prw) 및 [Pix Camera](https://www.microsoft.com/microsoftpix)같은 앱을 제공했습니다.

그러나 경우에 따라 기존 C/C++ 도구 및 프로세스를 그대로 유지하고 라이브러리 코드를 애플리케이션에서 분리하여 타사 구성 요소인 것처럼 라이브러리를 처리해야 할 수도 있습니다. 이러한 상황에서는 관련 멤버를 C#에 노출하는 것뿐 아니라 라이브러리를 종속성으로 관리하는 것도 어려운 과제입니다. 물론 이 프로세스를 최대한 자동화할 수 있습니다.  

이 게시물에서는 이 시나리오를 위한 개략적인 접근 방식을 간략하게 설명하고 간단한 예제를 안내합니다.

## <a name="background"></a>배경

C/C++ 는 플랫폼 간 언어로 간주되지만, 모든 대상 컴파일러에서 지원되는 C/C++만 사용하고 조건부로 포함된 플랫폼 또는 컴파일러 관련 코드를 거의 또는 전혀 포함하지 않아 소스 코드가 진정한 플랫폼 간 코드가 되도록 세심한 주의를 기울여야 합니다.

궁극적으로 코드는 모든 대상 플랫폼에서 성공적으로 컴파일되고 실행되어야 하므로 이는 대상으로 지정되는 플랫폼(및 컴파일러)의 공통성으로 귀결됩니다. 컴파일러 간의 사소한 차이로 인해 문제가 발생할 수 있으며, 따라서 각 대상 플랫폼에 대한 철저 한 테스트(가능하면 자동화)가 점차 중요해지고 있습니다.  

## <a name="high-level-approach"></a>상위 수준 접근 방식

아래 그림은 C/C++ 소스 코드를 NuGet을 통해 공유되어 Xamarin.Forms 앱에서 사용되는 플랫폼 간 Xamarin 라이브러리로 변환하는 데 사용되는 방법을 네 가지 단계로 보여 줍니다.

![Xamarin에서 C/C++를 사용하기 위한 상위 수준 접근 방식](images/cpp-steps.jpg)

네 가지 단계는 다음과 같습니다.

1. C/C++ 소스 코드를 플랫폼별 네이티브 라이브러리로 컴파일
2. Visual Studio 솔루션을 사용하여 네이티브 라이브러리 래핑
3. .NET 래퍼용 NuGet 패키지 압축 및 푸시
4. Xamarin 앱에서 NuGet 패키지 사용

### <a name="stage-1-compiling-the-cc-source-code-into-platform-specific-native-libraries"></a>1단계: C/C++ 소스 코드를 플랫폼별 네이티브 라이브러리로 컴파일

이 단계의 목표는 C# 래퍼에 의해 호출될 수 있는 네이티브 라이브러리를 만드는 것입니다. 이는 상황에 따라 관련이 있을 수도, 없을 수도 있습니다. 이 일반적인 시나리오에서 다룰 수 있는 많은 도구와 프로세스는 이 문서의 범위를 벗어나는 것입니다. 핵심 고려 사항은 C/C++ 코드베이스와 모든 네이티브 래퍼 코드 간 동기화 유지, 충분한 단위 테스트, 빌드 자동화입니다. 

이 연습의 라이브러리는 Visual Studio Code 및 동반 셸 스크립트를 사용하여 만들어졌습니다. 이 연습의 확장 버전은 샘플의 이 부분에 대해 보다 자세히 설명하는 [Mobile CAT GitHub 리포지토리](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin)에서 찾을 수 있습니다. 이 경우 네이티브 라이브러리는 타사 종속성으로 처리되지만, 이 단계가 컨텍스트에 대해 설명되어 있습니다.

간단한 설명을 위해 이 연습은 아키텍처의 하위 집합만을 대상으로 합니다. iOS의 경우 lipo 유틸리티를 사용하여 개별 아키텍처 관련 이진 파일에서 단일 fat 이진 파일을 만듭니다. Android는 확장명이 .so인 동적 이진 파일을 사용하고, iOS는 확장명이 .a인 정적 fat 이진 파일을 사용합니다. 

### <a name="stage-2-wrapping-the-native-libraries-with-a-visual-studio-solution"></a>2단계: Visual Studio 솔루션을 사용하여 네이티브 라이브러리 래핑

다음 단계는 .NET에서 쉽게 사용할 수 있도록 네이티브 라이브러리를 래핑하는 것입니다. 이 작업은 네 개의 프로젝트를 포함하는 Visual Studio 솔루션을 사용하여 수행됩니다. 공유 프로젝트에 공용 코드가 포함됩니다. Xamarin.Android, Xamarin.iOS 및 .NET Standard 각각을 대상으로 하는 프로젝트는 플랫폼 제약 없이 라이브러리를 참조할 수 있습니다.

이 래퍼는 Paul Betts가 설명한 '[bait and switch 기법](https://log.paulbetts.org/the-bait-and-switch-pcl-trick/)'을 사용합니다. 이것이 유일한 방법은 아니지만 라이브러리를 쉽게 참조할 수 있도록 하므로 사용 애플리케이션 자체에서 플랫폼별 구현을 명시적으로 관리하지 않아도 됩니다. 이 기법은 기본적으로 대상(.NET Standard, Android, iOS)이 동일한 네임스페이스, 어셈블리 이름 및 클래스 구조를 공유하는지 확인하는 것입니다. NuGet은 항상 플랫폼별 라이브러리를 선호하므로 .NET Standard 버전은 런타임에 사용되지 않습니다.

이 단계의 대부분의 작업에서는 P/Invoke를 사용하여 네이티브 라이브러리 메서드를 호출하고 기본 개체에 대한 참조를 관리하는 데 치중합니다. 목표는 복잡성을 추상화하는 동시에 라이브러리의 기능을 소비자에게 노출하는 것입니다. Xamarin.Forms 개발자는 관리되지 않는 라이브러리의 내부 작동에 대한 실무 지식이 필요하지 않습니다. 다른 관리되는 C# 라이브러리를 사용하는 것과 같을 것입니다.

궁극적으로 이 단계의 출력은 .NET 라이브러리 집합(대상마다 하나씩)과 다음 단계에서 패키지를 작성하는 데 필요한 정보가 포함된 nuspec 문서입니다.

**3단계: .NET 래퍼용 NuGet 패키지 압축 및 푸시**

세 번째 단계는 이전 단계의 빌드 아티팩트를 사용하여 NuGet 패키지를 만드는 것입니다. 이 단계의 결과는 Xamarin 앱에서 사용할 수 있는 NuGet 패키지입니다. 이 연습에서는 로컬 디렉터리를 NuGet 피드로 사용합니다. 프로덕션 환경에서 이 단계는 패키지를 공개 또는 비공개 NuGet 피드에 게시해야 하며 완전히 자동화되어야 합니다.

**4단계: Xamarin.Forms 앱에서 NuGet 패키지 사용**

마지막 단계는 Xamarin.Forms 앱에서 NuGet 패키지를 참조하고 사용하는 것입니다. 이렇게 하려면 Visual Studio에서 NuGet 피드가 이전 단계에서 정의된 피드를 사용하도록 구성해야 합니다.

피드를 구성한 후에는 플랫폼 간 Xamarin.Forms 앱의 각 프로젝트에서 패키지를 참조해야 합니다. 'bait-and-switch 기법'은 동일한 인터페이스를 제공하므로 단일 위치에 정의된 코드를 사용하여 네이티브 라이브러리 기능을 호출할 수 있습니다.

소스 코드 리포지토리에는 Azure DevOps에서 비공개 NuGet 피드를 설정하는 방법 및 해당 피드에 패키지를 푸시하는 방법에 대한 문서가 포함된 [추가 정보 목록](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin#wrapping-up)이 있습니다. 이 유형의 피드는 로컬 디렉터리보다 설정 시간이 약간 더 필요하지만 팀 개발 환경에서 더 효율적입니다.

## <a name="walk-through"></a>연습

제공된 단계는 **Mac용 Visual Studio**에만 적용되지만, 구조는 **Visual Studio 2017**에서도 유효합니다.

### <a name="prerequisites"></a>사전 요구 사항

계속하려면 다음이 필요합니다.

- [NuGet CLI(명령줄)](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)

- [*Mac용* *Visual Studio*](https://visualstudio.microsoft.com/downloads)

> [!NOTE]
> iPhone에 앱을 배포하려면 활성 [**Apple 개발자 계정**](https://developer.apple.com/)이 필요합니다.

## <a name="creating-the-native-libraries-stage-1"></a>네이티브 라이브러리 만들기(1단계)

네이티브 라이브러리 기능은 [연습: 정적 라이브러리 만들기 및 사용(C++)](https://docs.microsoft.com/cpp/windows/walkthrough-creating-and-using-a-static-library-cpp?view=vs-2017)의 예제를 기반으로 합니다.

이 시나리오에서는 라이브러리가 타사 종속성으로 제공되기 때문에 이 연습에서 네이티브 라이브러리를 빌드하는 첫 번째 단계를 건너뜁니다. 미리 컴파일된 네이티브 라이브러리는 [샘플 코드](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin)에 포함되어 있거나 직접 [다운로드](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin/Sample/Artefacts)할 수 있습니다.

### <a name="working-with-the-native-library"></a>네이티브 라이브러리 작업

원래 *MathFuncsLib* 예제에는 다음 정의를 사용하는 `MyMathFuncs`라는 단일 클래스가 포함됩니다.

```cpp
namespace MathFuncs
{
    class MyMathFuncs
    {
    public:
        double Add(double a, double b);
        double Subtract(double a, double b);
        double Multiply(double a, double b);
        double Divide(double a, double b);
    };
}
```

추가 클래스는 .NET 소비자가 기본 네이티브 `MyMathFuncs` 클래스를 만들고, 삭제하고, 상호 작용할 수 있는 래퍼 함수를 정의합니다.

```cpp
#include "MyMathFuncs.h"
using namespace MathFuncs;

extern "C" {
    MyMathFuncs* CreateMyMathFuncsClass();
    void DisposeMyMathFuncsClass(MyMathFuncs* ptr);
    double MyMathFuncsAdd(MyMathFuncs *ptr, double a, double b);
    double MyMathFuncsSubtract(MyMathFuncs *ptr, double a, double b);
    double MyMathFuncsMultiply(MyMathFuncs *ptr, double a, double b);
    double MyMathFuncsDivide(MyMathFuncs *ptr, double a, double b);
}
```

이들 래퍼 함수는 [Xamarin](https://visualstudio.microsoft.com/xamarin/) 쪽에서 사용됩니다.

## <a name="wrapping-the-native-library-stage-2"></a>네이티브 라이브러리 래핑(2단계)

이 단계를 수행하려면 [이전 섹션](#creating-the-native-libraries-stage-1)에서 설명한 [미리 컴파일된 라이브러리](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin/Sample/Artefacts)가 필요합니다.

### <a name="creating-the-visual-studio-solution"></a>Visual Studio 솔루션 만들기

1. **Mac용 Visual Studio**에서 **새 프로젝트**(*시작 페이지*에서) 또는 **새 솔루션**(*파일* 메뉴에서)을 클릭합니다.
2. **새 프로젝트** 창에서 **공유 프로젝트**(*다중 플랫폼 > 라이브러리* 내)를 선택한 후 **다음**을 클릭합니다.
3. 다음 필드를 업데이트한 다음 **만들기**를 클릭합니다.

    - **프로젝트 이름:** MathFuncs.Shared  
    - **솔루션 이름:** MathFuncs  
    - **위치:** 기본 저장 위치를 사용합니다(또는 대체 위치를 선택).   
    - **솔루션 디렉터리 내에 프로젝트 디렉터리 만들기:** 이 확인란을 선택합니다.
4. **솔루션 탐색기**에서 **MathFuncs.Shared** 프로젝트를 두 번 클릭하고 **기본 설정**으로 이동합니다.
5. **기본 네임스페이스**에서 **MathFuncs**만 설정되도록 **.Shared**를 제거한 다음 **확인**을 클릭합니다.
6. **MyClass.cs**(템플릿에서 만듦)를 연 다음 클래스 및 파일 이름 모두의 이름을 **MyMathFuncsWrapper**로 변경하고 네임스페이스를 **MathFuncs**로 변경합니다.
7. 솔루션 **MathFuncs**를 **CONTROL + 클릭**한 다음 **추가** 메뉴에서 **새 프로젝트 추가...** 를 선택합니다.
8. **새 프로젝트** 창에서 **.NET Standard 라이브러리**(*다중 플랫폼 > 라이브러리* 내)를 선택한 후 **다음**을 클릭합니다.
9. **.NET Standard 2.0**을 선택한 후 **다음**을 클릭합니다.
10. 다음 필드를 업데이트한 다음 **만들기**를 클릭합니다.

    - **프로젝트 이름:** MathFuncs.Standard  
    - **위치:** 공유 프로젝트와 동일한 저장 위치를 사용합니다.   

11. **솔루션 탐색기**에서 **MathFuncs** 프로젝트를 두 번 클릭합니다.
12. **기본 설정**으로 이동하여 **기본 네임스페이스**를 **MathFuncs**으로 업데이트합니다.
13. **출력** 설정으로 이동하여 **어셈블리 이름**을 **MathFuncs**으로 업데이트합니다.
14. **컴파일러** 설정으로 이동하여 **구성**을 **릴리스**로 변경하고, **디버그 정보**를 **기호만**으로 설정한 다음 **확인**을 클릭합니다.
15. 프로젝트에서 **Class1.cs/Getting Started**를 삭제합니다(이러한 항목 중 하나가 템플릿의 일부로 포함된 경우).
16. 프로젝트 **Dependencies/References** 폴더를 **CONTROL + 클릭**한 다음 **참조 편집**을 선택합니다.
17. **프로젝트** 탭에서 **MathFuncs.Shared**를 선택한 다음 **확인**을 클릭합니다.
18. 다음 구성을 사용하여 7~17단계를 반복합니다(9단계 무시).

    | **프로젝트 이름**  | **템플릿 이름**   | **프로젝트 메뉴**   |
    |-------------------| --------------------| -----------------------|
    | MathFuncs.Android | 클래스 라이브러리       | Android > Library      |
    | MathFuncs.iOS     | 바인딩 라이브러리     | iOS > Library          |

19. **솔루션 탐색기**에서 **MathFuncs.Android** 프로젝트를 두 번 클릭한 다음 **컴파일러** 설정으로 이동합니다.

20. **구성**이 **디버그**로 설정된 상태에서 **Android;** 를 포함하도록 **기호 정의**를 편집합니다.

21. **구성**을 **릴리스**로 변경한 다음 **Android;** 도 포함하도록 **기호 정의**를 편집합니다.

22. **MathFuncs.iOS**에 대해 19~20단계를 반복하여 두 경우 모두 **Android;** 대신 **iOS;** 를 포함하도록 **기호 정의**를 편집합니다.

23. **릴리스** 구성에서 솔루션을 빌드하고(**CONTROL + COMMAND + B**) 해당 프로젝트 bin 폴더에 있는 세 개의 출력 어셈블리(Android, iOS, .NET Standard)가 모두 같은 이름 **MathFuncs.dll**을 공유하는지 확인합니다.

이 단계에서 솔루션에는 Android, iOS 및 .NET Standard에 하나씩 세 개의 대상과 세 대상이 각각 참조하는 하나의 공유 프로젝트가 있어야 합니다. 이들은 동일한 기본 네임스페이스와 동일한 이름을 가진 출력 어셈블리를 사용하도록 구성해야 합니다. 이는 앞서 언급한 'bait and switch' 접근 방식에 필요합니다.

### <a name="adding-the-native-libraries"></a>네이티브 라이브러리 추가

네이티브 라이브러리를 래퍼 솔루션에 추가하는 프로세스는 Android와 iOS 간에 약간 다릅니다.

#### <a name="native-references-for-mathfuncsandroid"></a>MathFuncs.Android용 네이티브 참조

1. **MathFuncs.Android** 프로젝트를 **CONTROL + 클릭**한 다음 **추가** 메뉴에서 **새 폴더**를 선택하고 이름을 **libs**로 지정합니다.

2. 각 **ABI**(애플리케이션 이진 인터페이스)에 대해, **libs** 폴더를 **CONTROL + 클릭**한 다음 **추가** 메뉴에서 **새 폴더**를 선택하고 각 **ABI**를 따라 이름을 지정합니다. 이 경우:

    - arm64-v8a
    - armeabi-v7a
    - x86
    - x86_64  

    > [!NOTE]
    > 자세한 개요는 [NDK 개발자 가이드](https://developer.android.com/ndk/guides/)에서 [아키텍처 및 CPU](https://developer.android.com/ndk/guides/arch) 항목, 특히 [앱 패키지의 네이티브 코드](https://developer.android.com/ndk/guides/abis#native-code-in-app-packages)에 대한 섹션을 참조하세요.

3. 폴더 구조를 확인합니다.  

    ```folders
    - lib
        - arm64-v8a
        - armeabi-v7a
        - x86
        - x86_64
    ```

4. 다음 매핑에 따라 **ABI** 폴더 각각에 해당하는 **.so** 라이브러리를 추가합니다.

    **arm64-v8a:** libs/Android/arm64

    **armeabi-v7a:** libs/Android/arm  

    **x86:** libs/Android/x86

    **x86_64:** libs/Android/x86_64

    > [!NOTE]
    > 파일을 추가하려면 각 **ABI**를 나타내는 폴더를 **CONTROL + 클릭**한 다음 **추가** 메뉴에서 **파일 추가...** 를 선택합니다. **PrecompiledLibs** 디렉터리에서 적절한 라이브러리를 선택하고 **열기**를 클릭한 다음 기본 옵션을 *파일을 디렉터리로 복사* 그대로 두고 **확인**을 클릭합니다.

5. 각 **.so** 파일에 대해 **CONTROL + 클릭**한 다음 **빌드 작업** 메뉴에서 **EmbeddedNativeLibrary** 옵션을 선택합니다.

이제 **libs** 폴더가 다음과 같이 표시됩니다.

```folders
- lib
    - arm64-v8a
        - libMathFuncs.so
    - armeabi-v7a
        - libMathFuncs.so
    - x86
        - libMathFuncs.so
    - x86_64
        - libMathFuncs.so
```

#### <a name="native-references-for-mathfuncsios"></a>MathFuncs.iOS용 네이티브 참조

1. **MathFuncs.iOS** 프로젝트를 **CONTROL + 클릭**한 다음 **추가** 메뉴에서 **네이티브 참조 추가**를 선택합니다. 
2. **PrecompiledLibs** 디렉터리 아래의 libs/ios에서 **libMathFuncs.a** 라이브러리를 선택한 다음 **열기**를 클릭합니다. 
3. **libMathFuncs** 파일(**기본 참조** 폴더 내)을 **CONTROL + 클릭**한 다음, 메뉴에서 **속성** 옵션을 선택합니다.  
4. **속성** 패드에서 선택(체크 아이콘 표시)되도록 **네이티브 참조** 속성을 구성합니다.

    - Force Load
    - Is C++
    - Smart Link

    > [!NOTE]
    > [네이티브 참조](https://docs.microsoft.com/xamarin/cross-platform/macios/native-references)와 함께 바인딩 라이브러리 프로젝트 형식을 사용하여 정적 라이브러리를 포함하고 이를 참조하는 Xamarin.iOS 앱과 자동으로 연결되도록 할 수 있습니다(NuGet 패키지를 통해 포함된 경우에도).

5. **ApiDefinition.cs**를 열고 템플릿 기반 주석 처리된 코드를 삭제한 다음(`MathFuncs` 네임스페이스만 남겨 둠) **Structs.cs**에 대해 동일한 단계를 수행합니다. 

    > [!NOTE]
    > 바인딩 라이브러리 프로젝트는 빌드하려면 이러한 파일(*ObjCBindingApiDefinition* 및 *ObjCBindingCoreSource* 빌드 작업 포함)이 필요합니다. 그러나 우리는 표준 P/Invoke를 사용하여 Android 및 iOS 라이브러리 대상 모두에서 공유할 수 있는 방식으로 이러한 파일 외부에서 네이티브 라이브러리를 호출하는 코드를 작성합니다.

### <a name="writing-the-managed-library-code"></a>관리되는 라이브러리 코드 작성

이제 네이티브 라이브러리를 호출하는 C# 코드를 작성합니다. 목표는 기본적인 복잡성을 숨기는 것입니다. 소비자는 네이티브 라이브러리 내부 요소 또는 P/Invoke 개념에 대한 실무 지식이 필요하지 않습니다.  

#### <a name="creating-a-safehandle"></a>SafeHandle 만들기

1. **MathFuncs.Shared** 프로젝트를 **CONTROL + 클릭**한 다음 **추가** 메뉴에서 **파일 추가...** 를 선택합니다. 
2. **새 파일** 창에서 **빈 클래스**를 선택하고 이름을 **MyMathFuncsSafeHandle**로 지정한 다음 **새로 만들기**를 클릭합니다.
3. **MyMathFuncsSafeHandle** 클래스를 구현합니다.

    ```csharp
    using System;
    using Microsoft.Win32.SafeHandles;

    namespace MathFuncs
    {
        internal class MyMathFuncsSafeHandle : SafeHandleZeroOrMinusOneIsInvalid
        {
            public MyMathFuncsSafeHandle() : base(true) { }

            public IntPtr Ptr => handle;

            protected override bool ReleaseHandle()
            {
                // TODO: Release the handle here
                return true;
            }
        }
    }
    ```

    > [!NOTE]
    > [SafeHandle](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.safehandle?view=netframework-4.7.2)은 관리 코드에서 관리되지 않는 리소스로 작업하는 데 선호되는 방법입니다. 이는 중요한 종료 및 개체 수명 주기와 관련된 많은 상용구 코드를 추상화합니다. 이 핸들의 소유자는 나중에 이 핸들을 다른 관리되는 리소스처럼 취급할 수 있으며 전체 [삭제 가능한 패턴](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose)을 구현할 필요가 없습니다. 

#### <a name="creating-the-internal-wrapper-class"></a>내부 래퍼 클래스 만들기

1. **MyMathFuncsWrapper.cs**를 열고 내부 정적 클래스로 변경합니다.

    ```csharp
    namespace MathFuncs
    {
        internal static class MyMathFuncsWrapper
        {
        }
    }
    ```

2. 동일한 파일에서 다음 조건문을 클래스에 추가합니다.

    ```csharp
    #if Android
        const string DllName = "libMathFuncs.so";
    #else
        const string DllName = "__Internal";
    #endif
    ```

    > [!NOTE]
    > 이렇게 하면 **Android** 또는 **iOS**에 대해 라이브러리를 빌드하는지 여부에 따라 **DllName** 상수 값이 설정됩니다. 이는 각 플랫폼에서 사용되는 다양한 명명 규칙을 해결하기 위한 것이지만, 이 경우에는 사용되는 라이브러리의 형식이기도 합니다. Android는 동적 라이브러리를 사용하므로 확장명을 포함하는 파일 이름이 필요합니다. iOS의 경우, 정적 라이브러리를 사용하고 있으므로 ' *__Internal*'이 필요합니다.

3. **MyMathFuncsWrapper.cs** 파일의 맨 위에 **System.Runtime.InteropServices**에 대한 참조를 추가합니다.

    ```csharp
    using System.Runtime.InteropServices;
    ```

4. **MyMathFuncs** 클래스의 생성 및 삭제를 처리하는 래퍼 메서드를 추가합니다.

    ```csharp
    [DllImport(DllName, EntryPoint = "CreateMyMathFuncsClass")]
    internal static extern MyMathFuncsSafeHandle CreateMyMathFuncs();

    [DllImport(DllName, EntryPoint = "DisposeMyMathFuncsClass")]
    internal static extern void DisposeMyMathFuncs(MyMathFuncsSafeHandle ptr);
    ```

    > [!NOTE]
    > 우리는 상수 **DllName**을 .NET 런타임에 해당 라이브러리에서 호출할 함수의 이름을 명시적으로 지시하는 **EntryPoint**와 함께 **DllImport** 특성에 전달합니다. 기술적으로, 관리되는 메서드 이름이 관리되지 않는 메서드와 동일한 경우 **EntryPoint** 값을 제공할 필요가 없습니다. 이 값을 제공하지 않으면 관리되는 메서드 이름이 대신 **EntryPoint**로 사용됩니다. 그러나 명시적으로 지정하는 것이 더 좋습니다.  

5. 다음 코드를 사용하여 **MyMathFuncs** 클래스로 작업할 수 있도록 래퍼 메서드를 추가합니다.

    ```csharp
    [DllImport(DllName, EntryPoint = "MyMathFuncsAdd")]
    internal static extern double Add(MyMathFuncsSafeHandle ptr, double a, double b);

    [DllImport(DllName, EntryPoint = "MyMathFuncsSubtract")]
    internal static extern double Subtract(MyMathFuncsSafeHandle ptr, double a, double b);

    [DllImport(DllName, EntryPoint = "MyMathFuncsMultiply")]
    internal static extern double Multiply(MyMathFuncsSafeHandle ptr, double a, double b);

    [DllImport(DllName, EntryPoint = "MyMathFuncsDivide")]
    internal static extern double Divide(MyMathFuncsSafeHandle ptr, double a, double b);
    ```

    > [!NOTE]
    > 이 예제에서는 매개 변수에 대해 단순한 형식을 사용합니다. 이 경우에는 마샬링이 비트 복사이므로 추가 작업이 필요하지 않습니다. 또한 표준 **IntPtr** 대신 **MyMathFuncsSafeHandle** 클래스를 사용하는 데 유의하세요. **IntPtr**는 마샬링 프로세스의 일부로 **SafeHandle**에 자동으로 매핑됩니다.

6. 완성된 **MyMathFuncsWrapper** 클래스가 아래와 같이 표시되는지 확인합니다.

    ```csharp
    using System.Runtime.InteropServices;

    namespace MathFuncs
    {
        internal static class MyMathFuncsWrapper
        {
            #if Android
                const string DllName = "libMathFuncs.so";
            #else
                const string DllName = "__Internal";
            #endif

            [DllImport(DllName, EntryPoint = "CreateMyMathFuncsClass")]
            internal static extern MyMathFuncsSafeHandle CreateMyMathFuncs();

            [DllImport(DllName, EntryPoint = "DisposeMyMathFuncsClass")]
            internal static extern void DisposeMyMathFuncs(MyMathFuncsSafeHandle ptr);

            [DllImport(DllName, EntryPoint = "MyMathFuncsAdd")]
            internal static extern double Add(MyMathFuncsSafeHandle ptr, double a, double b);

            [DllImport(DllName, EntryPoint = "MyMathFuncsSubtract")]
            internal static extern double Subtract(MyMathFuncsSafeHandle ptr, double a, double b);

            [DllImport(DllName, EntryPoint = "MyMathFuncsMultiply")]
            internal static extern double Multiply(MyMathFuncsSafeHandle ptr, double a, double b);

            [DllImport(DllName, EntryPoint = "MyMathFuncsDivide")]
            internal static extern double Divide(MyMathFuncsSafeHandle ptr, double a, double b);
        }
    }
    ```

#### <a name="completing-the-mymathfuncssafehandle-class"></a>MyMathFuncsSafeHandle 클래스 완료

1. **MyMathFuncsSafeHandle** 클래스를 열고 **ReleaseHandle** 메서드에서 자리 표시자 **TODO** 주석으로 이동합니다.

    ```csharp
    // TODO: Release the handle here
    ```

1. **TODO** 줄을 바꿉니다.

    ```csharp
    MyMathFuncsWrapper.DisposeMyMathFuncs(handle);
    ```

#### <a name="writing-the-mymathfuncs-class"></a>MyMathFuncs 클래스 작성

이제 래퍼가 완성되었으므로 관리되지 않는 C++ MyMathFuncs 개체에 대한 참조를 관리하는 MyMathFuncs 클래스를 만듭니다.  

1. **MathFuncs.Shared** 프로젝트를 **CONTROL + 클릭**한 다음 **추가** 메뉴에서 **파일 추가...** 를 선택합니다. 
2. **새 파일** 창에서 **빈 클래스**를 선택하고 이름을 **MyMathFuncs**로 지정한 다음 **새로 만들기**를 클릭합니다.
3. **MyMathFuncs** 클래스에 다음 멤버를 추가합니다.

    ```csharp
    readonly MyMathFuncsSafeHandle handle;
    ```

4. 클래스가 인스턴스화될 때 네이티브 **MyMathFuncs** 개체에 대한 핸들을 만들어 저장하도록 클래스 생성자를 구현합니다.

    ```csharp
    public MyMathFuncs()
    {
        handle = MyMathFuncsWrapper.CreateMyMathFuncs();
    }
    ```

5. 다음 코드를 사용하여 **IDisposable** 인터페이스를 구현합니다.

    ```csharp
    public class MyMathFuncs : IDisposable
    {
        ...

        protected virtual void Dispose(bool disposing)
        {
            if (handle != null && !handle.IsInvalid)
                handle.Dispose();
        }

        public void Dispose()
        {
            Dispose(true);
            GC.SuppressFinalize(this);
        }

        // ...
    }
    ```

6. 관리되지 않는 기본 개체에 저장한 포인터를 전달하여 내부적으로 실제 작업을 수행하도록 **MyMathFuncsWrapper** 클래스를 사용하여 **MyMathFuncs** 메서드를 구현합니다. 코드는 다음과 같이 표시됩니다.

    ```csharp
    public double Add(double a, double b)
    {
        return MyMathFuncsWrapper.Add(handle, a, b);
    }

    public double Subtract(double a, double b)
    {
        return MyMathFuncsWrapper.Subtract(handle, a, b);
    }

    public double Multiply(double a, double b)
    {
        return MyMathFuncsWrapper.Multiply(handle, a, b);
    }

    public double Divide(double a, double b)
    {
        return MyMathFuncsWrapper.Divide(handle, a, b);
    }
    ```

#### <a name="creating-the-nuspec"></a>nuspec 만들기

NuGet을 통해 라이브러리를 패키지하고 배포하기 위해 솔루션에 **nuspec** 파일이 필요합니다. 그러면 지원되는 각 플랫폼에 대해 포함되는 결과 어셈블리를 식별할 수 있습니다.

1. 솔루션 **MathFuncs**를 **CONTROL + 클릭**한 다음 **추가** 메뉴에서 **솔루션 폴더 추가**를 선택하여 이름을 **SolutionItems**로 지정합니다.
2. **SolutionItems** 폴더를 **CONTROL + 클릭**한 다음 **추가** 메뉴에서 **새 파일...** 을 선택합니다.
3. **새 파일** 창에서 **빈 XML 파일**을 선택하고 이름을 **MathFuncs.nuspec**으로 지정한 다음 **새로 만들기**를 클릭합니다.
4. **NuGet** 소비자에게 표시되는 기본 패키지 메타데이터를 사용하여 **MathFuncs.nuspec**을 업데이트합니다. 예를 들어:

    ```xml
    <?xml version="1.0"?>
    <package>
        <metadata>
            <id>MathFuncs</id>
            <version>$version$</version>
            <authors>Microsoft Mobile Customer Advisory Team</authors>
            <description>Sample C++ Wrapper Library</description>
            <requireLicenseAcceptance>false</requireLicenseAcceptance>
            <copyright>Copyright 2018</copyright>
        </metadata>
    </package>
    ```

    > [!NOTE]
    > 이 매니페스트에 사용된 스키마에 대한 자세한 내용은 [nuspec 참조](https://docs.microsoft.com/nuget/reference/nuspec) 문서를 참조하세요.

5. `<files>` 요소를 `<package>` 요소의 자식으로 추가하여(`<metadata>` 바로 아래) 별도의 `<file>` 요소를 사용해 각 파일을 식별합니다.

    ```xml
    <files>

        <!-- Android -->

        <!-- iOS -->

        <!-- netstandard2.0 -->

    </files>
    ```

    > [!NOTE]
    > 패키지를 프로젝트에 설치하고 여러 어셈블리가 동일한 이름으로 지정된 경우 NuGet은 지정된 플랫폼과 가장 고유한 어셈블리를 유효하게 선택합니다.

6. **Android** 어셈블리에 대한 `<file>` 요소를 추가합니다.

    ```xml
    <file src="MathFuncs.Android/bin/Release/MathFuncs.dll" target="lib/MonoAndroid81/MathFuncs.dll" />
    <file src="MathFuncs.Android/bin/Release/MathFuncs.pdb" target="lib/MonoAndroid81/MathFuncs.pdb" />
    ```

7. **iOS** 어셈블리에 대한 `<file>` 요소를 추가합니다.

    ```xml
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.dll" target="lib/Xamarin.iOS10/MathFuncs.dll" />
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.pdb" target="lib/Xamarin.iOS10/MathFuncs.pdb" />
    ```

8. **netstandard2.0** 어셈블리에 대한 `<file>` 요소를 추가합니다.

    ```xml
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.dll" target="lib/netstandard2.0/MathFuncs.dll" />
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.pdb" target="lib/netstandard2.0/MathFuncs.pdb" />
    ```

9. **nuspec** 매니페스트를 확인합니다.

    ```xml
    <?xml version="1.0"?>
    <package>
    <metadata>
        <id>MathFuncs</id>
        <version>$version$</version>
        <authors>Microsoft Mobile Customer Advisory Team</authors>
        <description>Sample C++ Wrapper Library</description>
        <requireLicenseAcceptance>false</requireLicenseAcceptance>
        <copyright>Copyright 2018</copyright>
    </metadata>
    <files>

        <!-- Android -->
        <file src="MathFuncs.Android/bin/Release/MathFuncs.dll" target="lib/MonoAndroid81/MathFuncs.dll" />
        <file src="MathFuncs.Android/bin/Release/MathFuncs.pdb" target="lib/MonoAndroid81/MathFuncs.pdb" />
        
        <!-- iOS -->
        <file src="MathFuncs.iOS/bin/Release/MathFuncs.dll" target="lib/Xamarin.iOS10/MathFuncs.dll" />
        <file src="MathFuncs.iOS/bin/Release/MathFuncs.pdb" target="lib/Xamarin.iOS10/MathFuncs.pdb" />
        
        <!-- netstandard2.0 -->
        <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.dll" target="lib/netstandard2.0/MathFuncs.dll" />
        <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.pdb" target="lib/netstandard2.0/MathFuncs.pdb" />

    </files>
    </package>
    ```

    > [!NOTE]
    > 이 파일은 **릴리스** 빌드의 어셈블리 출력 경로를 지정하므로 해당 구성을 사용하여 솔루션을 빌드해야 합니다.

이 시점에서 솔루션에는 3개의 .NET 어셈블리와 1개의 지원 **nuspec** 매니페스트가 포함됩니다.

## <a name="distributing-the-net-wrapper-with-nuget"></a>NuGet을 사용하여 .NET 래퍼 배포

다음 단계는 NuGet 패키지를 앱에서 쉽게 사용하고 종속성으로 관리할 수 있도록 패키지 및 배포하는 것입니다. 래핑과 사용은 모두 단일 솔루션에서 수행할 수 있지만 NuGet 지원을 통해 라이브러리를 배포하면 이러한 코드베이스를 독립적으로 관리할 수 있습니다.

### <a name="preparing-a-local-packages-directory"></a>로컬 패키지 디렉터리 준비

NuGet 피드의 가장 간단한 형태는 로컬 디렉터리입니다.

1. **Finder**에서 편리한 디렉터리로 이동합니다. 예: **/Users**.
2. **파일** 메뉴에서 **새 폴더**를 선택하여 **local-nuget-feed**와 같은 의미 있는 이름을 제공합니다.

### <a name="creating-the-package"></a>패키지 만들기

1. **빌드 구성**을 **릴리스**로 설정하고 **COMMAND + B**를 사용하여 빌드를 실행합니다.
2. **터미널**을 열고 디렉터리를 **nuspec** 파일이 포함된 폴더로 변경합니다.
3. **터미널**에서 **nuspec** 파일, **버전**(예: 1.0.0), **OutputDirectory**([이전 단계](https://docs.microsoft.com/xamarin/cross-platform/cpp/index#creating-a-local-nuget-feed)에서 만든 폴더, 즉 **local-nuget-feed**를 사용)를 지정하여 **nuget pack** 명령을 실행합니다. 예를 들어:

    ```bash
    nuget pack MathFuncs.nuspec -Version 1.0.0 -OutputDirectory ~/local-nuget-feed
    ```

4. **local-nuget-feed** 디렉터리에서 **MathFuncs.1.0.0.nupkg**가 만들어졌는지 **확인**합니다.

### <a name="optional-using-a-private-nuget-feed-with-azure-devops"></a>[선택 사항] Azure DevOps에서 비공개 NuGet 피드 사용

[Azure DevOps에서 NuGet 패키지 시작](https://docs.microsoft.com/azure/devops/artifacts/get-started-nuget?view=vsts&tabs=new-nav#publish-a-package)에 보다 강력한 기법이 설명되어 있습니다. 여기서는 비공개 피드를 만들고 패키지(이전 단계에서 생성됨)를 해당 피드에 푸시하는 방법을 보여 줍니다.

예를 들어 [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/index?view=vsts)를 사용하여 이 워크플로를 완전히 자동화하는 것이 이상적입니다. 자세한 내용은 [Azure Pipelines 시작](https://docs.microsoft.com/azure/devops/pipelines/get-started/index?view=vsts)을 참조하세요.

## <a name="consuming-the-net-wrapper-from-a-xamarinforms-app"></a>Xamarin.Forms 앱에서 .NET 래퍼 사용

연습을 완료하려면 로컬 **NuGet** 피드에 방금 게시한 패키지를 사용하는 **Xamarin.Forms** 앱을 만듭니다.

### <a name="creating-the-xamarinforms-project"></a>**Xamarin.Forms** 프로젝트 만들기

1. **Mac용 Visual Studio**의 새 인스턴스를 엽니다. **터미널**에서 이 작업을 수행할 수 있습니다.

    ```bash
    open -n -a "Visual Studio"
    ```

2. **Mac용 Visual Studio**에서 **새 프로젝트**(*시작 페이지*에서) 또는 **새 솔루션**(*파일* 메뉴에서)을 클릭합니다.
3. **새 프로젝트** 창에서 **빈 Forms 앱**(*다중 플랫폼 > 앱* 내)를 선택한 후 **다음**을 클릭합니다.
4. 다음 필드를 업데이트한 후 **다음**을 클릭합니다.

    - **앱 이름:** MathFuncsApp.
    - **조직 식별자:** 예를 들어 _com.{your_org}_ 같은 역방향 네임스페이스를 사용합니다.
    - **대상 플랫폼:** 기본값을 사용합니다(Android 및 iOS 대상 모두).
    - **공유 코드:** 이 필드를 .NET Standard로 설정합니다("공유 라이브러리" 솔루션은 가능하지만 이 연습에서는 다루지 않음).

5. 다음 필드를 업데이트한 다음 **만들기**를 클릭합니다.

    - **프로젝트 이름:** MathFuncsApp.
    - **솔루션 이름:** MathFuncsApp.  
    - **위치:** 기본 저장 위치를 사용합니다(또는 대체 위치를 선택).

6. **솔루션 탐색기**에서 초기 테스트를 위한 대상(**MathFuncsApp.Android** 또는 **MathFuncs.iOS**)을 **CONTROL + 클릭**한 다음 **시작 프로젝트로 설정**을 선택합니다.
7. 기본 **디바이스** 또는 **시뮬레이터**/**에뮬레이터**를 선택합니다. 
8. 솔루션을 실행하여(**COMMAND + RETURN**) 템플릿 기반 **Xamarin.Forms** 프로젝트가 빌드되고 제대로 실행되는지 확인합니다. 

    > [!NOTE]
    > **iOS**(특히 시뮬레이터)는 빌드/배포 시간이 가장 빠릅니다.

### <a name="adding-the-local-nuget-feed-to-the-nuget-configuration"></a>NuGet 구성에 로컬 NuGet 피드 추가

1. **Visual Studio**에서 **기본 설정**(**Visual Studio** 메뉴에서)을 선택합니다.
2. **NuGet** 섹션 아래에서 **소스**를 선택한 다음 **추가**를 클릭합니다.
3. 다음 필드를 업데이트한 다음 **소스 추가**를 클릭합니다.

    - **Name:** Local-Packages와 같은 의미 있는 이름을 제공합니다.  
    - **위치:** [이전 단계](#preparing-a-local-packages-directory)에서 만든 **local-nuget-feed** 폴더를 지정합니다.

    > [!NOTE]
    > 이 경우 **사용자 이름** 및 **암호**를 지정할 필요가 없습니다. 

4. **확인**을 클릭합니다.

### <a name="referencing-the-package"></a>패키지 참조

각 프로젝트(**MathFuncsApp**, **MathFuncsApp.Android**, **MathFuncsApp.iOS**)에 대해 다음 단계를 반복합니다.

1. 프로젝트를 **CONTROL + 클릭**한 다음 **추가** 메뉴에서 **NuGet 패키지 추가...** 를 선택합니다.
2. **MathFuncs**를 검색합니다. 
3. 패키지의 **버전**이 **1.0.0**이고 **제목** 및 **설명** 같은 다른 세부 정보가 예상대로, 즉 *MathFuncs* 및 *샘플 C++ 래퍼 라이브러리*로 표시되는지 확인합니다. 
4. **MathFuncs** 패키지를 선택한 다음 **패키지 추가**를 클릭합니다.

### <a name="using-the-library-functions"></a>라이브러리 함수 사용

이제 각 프로젝트의 **MathFuncs** 패키지에 대 한 참조를 사용하여 함수를 C# 코드에 사용할 수 있습니다.

1. **MathFuncsApp** 공통 **Xamarin.Forms** 프로젝트(**MathFuncsApp.Android** 및 **MathFuncsApp.iOS** 모두에 의해 참조됨)에서 **MainPage.xaml.cs**를 엽니다.
2. 파일 맨 위에 **System.Diagnostics** 및 **MathFuncs**에 대한 **using** 문을 추가합니다.

    ```csharp
    using System.Diagnostics;
    using MathFuncs;
    ```

3. `MainPage` 클래스 맨 위에서 `MyMathFuncs` 클래스의 인스턴스를 선언합니다.

    ```csharp
    MyMathFuncs myMathFuncs;
    ```

4. `ContentPage` 기본 클래스에서 `OnAppearing` 및 `OnDisappearing` 메서드를 재정의합니다.

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
    }
    ```

5. `OnAppearing` 메서드를 업데이트하여 이전에 선언된 `myMathFuncs` 변수를 초기화합니다.

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();
        myMathFuncs = new MyMathFuncs();
    }
    ```

6. `OnDisappearing` 메서드를 업데이트하여 `myMathFuncs`에 대해 `Dispose` 메서드를 호출합니다.

    ```csharp
    protected override void OnDisappearing()
    {
        base.OnAppearing();
        myMathFuncs.Dispose();
    }
    ```

7. 다음과 같이 **TestMathFuncs**라는 프라이빗 메서드를 구현합니다.

    ```csharp
    private void TestMathFuncs()
    {
        var numberA = 1;
        var numberB = 2;

        // Test Add function
        var addResult = myMathFuncs.Add(numberA, numberB);

        // Test Subtract function
        var subtractResult = myMathFuncs.Subtract(numberA, numberB);

        // Test Multiply function
        var multiplyResult = myMathFuncs.Multiply(numberA, numberB);

        // Test Divide function
        var divideResult = myMathFuncs.Divide(numberA, numberB);

        // Output results
        Debug.WriteLine($"{numberA} + {numberB} = {addResult}");
        Debug.WriteLine($"{numberA} - {numberB} = {subtractResult}");
        Debug.WriteLine($"{numberA} * {numberB} = {multiplyResult}");
        Debug.WriteLine($"{numberA} / {numberB} = {divideResult}");
    }
    ```

8. 마지막으로 `OnAppearing` 메서드의 끝에서 `TestMathFuncs`를 호출합니다.

    ```csharp
    TestMathFuncs();
    ```

9. 각 대상 플랫폼에서 앱을 실행하고 **애플리케이션 출력** 패드에서 출력이 다음과 같이 표시되는지 확인합니다.

    ```csharp
    1 + 2 = 3
    1 - 2 = -1
    1 * 2 = 2
    1 / 2 = 0.5
    ```

    > [!NOTE]
    > Android에서 테스트할 때 '*DLLNotFoundException*'이 발생하거나 iOS에서 빌드 오류가 발생하는 경우 사용 중인 디바이스/에뮬레이터/시뮬레이터의 CPU 아키텍처가 지원하도록 선택한 하위 집합과 호환되는지 확인해야 합니다. 

## <a name="summary"></a>요약

이 문서에서는 NuGet 패키지를 통해 배포되는 공용 .NET 래퍼를 통해 네이티브 라이브러리를 사용하는 Xamarin.Forms 앱을 만드는 방법을 설명했습니다. 이 연습에서 제공하는 예제는 보다 간단하게 방법을 보여 주기 위해 의도적으로 매우 간단하게 작성되었습니다. 실제 애플리케이션은 예외 처리, 콜백, 더 복잡한 형식의 마샬링, 다른 종속성 라이브러리와의 연결 등의 복잡성을 처리해야 합니다. 핵심 고려 사항은 C++ 코드의 진화를 조정하고 래퍼 및 클라이언트 애플리케이션과 동기화하는 프로세스입니다. 이 프로세스는 이러한 문제를 하나 또는 둘 모두 단일 팀에서 담당하는지 여부에 따라 달라질 수 있습니다. 어느 방법이든 자동화가 진정한 장점입니다. 다음은 관련 다운로드와 함께 몇 가지 주요 개념에 대한 추가 정보를 제공하는 리소스입니다. 

### <a name="downloads"></a>다운로드

- [NuGet CLI(명령줄 ) 도구](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)
- [Visual Studio](https://visualstudio.microsoft.com/vs)

### <a name="examples"></a>예

- [C++를 사용한 Hyperlapse 플랫폼 간 모바일 개발](https://blogs.msdn.microsoft.com/vcblog/2015/06/26/hyperlapse-cross-platform-mobile-development-with-visual-c-and-xamarin/)
- [Microsoft Pix(C++ 및 Xamarin)](https://devblogs.microsoft.com/xamarin/microsoft-research-ships-intelligent-apps-with-the-power-of-c-and-ai/)
- [Mono San Angeles 샘플 포트](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk/)

### <a name="further-reading"></a>추가 정보

[이 게시물의 내용과 관련된 문서](https://github.com/xamcat/mobcat-samples/tree/master/cpp_with_xamarin#wrapping-up)