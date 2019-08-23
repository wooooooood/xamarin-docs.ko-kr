---
ms.assetid: EA2D979E-9151-4CE9-9289-13B6A979838B
title: Xamarin에서 CC++ /라이브러리 사용
description: Xamarin 및C++ C#를 사용 하 여 플랫폼 간 C/코드를 빌드하고 Android 및 iOS 용 모바일 앱으로 통합 하는 데 Mac용 Visual Studio 사용할 수 있습니다. 이 문서에서는 Xamarin 앱에서 프로젝트를 C++ 설정 하 고 디버그 하는 방법을 설명 합니다.
author: mikeparker104
ms.author: miparker
ms.date: 12/17/2018
ms.openlocfilehash: a6c5e172fa9fe41e210f332d351adc307d0c7df3
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648188"
---
# <a name="use-cc-libraries-with-xamarin"></a>Xamarin에서 CC++ /라이브러리 사용

## <a name="overview"></a>개요

Xamarin을 통해 개발자는 Visual Studio를 사용 하 여 플랫폼 간 네이티브 모바일 앱을 만들 수 있습니다. 일반적으로 C# 바인딩은 기존 플랫폼 구성 요소를 개발자에 게 노출 하는 데 사용 됩니다. 그러나 Xamarin 앱에서 기존 코드 베이스를 사용 해야 하는 경우도 있습니다. 때로는 팀은 매우 크고 잘 테스트 되 고 최적화 된 코드 베이스를로 C#이식 하는 시간, 예산 또는 리소스가 없는 경우가 있습니다.

[플랫폼 C++ 간 모바일 개발용 시각적 개체](https://docs.microsoft.com/visualstudio/cross-platform/visual-cpp-for-cross-platform-mobile-development) 를 사용 하면 C/C++ 및 C# 코드를 동일한 솔루션의 일부로 작성 하 여 통합 된 디버깅 환경을 비롯 한 다양 한 이점을 제공할 수 있습니다. Microsoft는이 방식으로C++ 하이퍼 [경과 모바일](https://www.microsoft.com/p/hyperlapse-mobile/9wzdncrd1prw) 및 [Pix 카메라](https://www.microsoft.com/microsoftpix)와 같은 앱을 제공 하는 C/및 Xamarin을 사용 했습니다.

그러나 경우에 따라 기존 C/C++ 도구 및 프로세스를 그대로 유지 하 고 라이브러리 코드를 응용 프로그램에서 분리 된 상태로 유지 해야 하는 경우도 있습니다 .이는 타사 구성 요소와 유사한 것 처럼 라이브러리를 처리 하는 것입니다. 이러한 상황에서 챌린지는 관련 멤버를에 표시 하는 것 뿐 C# 아니라 종속성으로 라이브러리를 관리 하는 것입니다. 물론 최대한 많은이 프로세스를 자동화할 수 있습니다.  

이 게시물에서는이 시나리오에 대 한 개략적인 접근 방식을 간략하게 설명 하 고 간단한 예제를 안내 합니다.

## <a name="background"></a>배경

C/C++ 는 플랫폼 간 언어로 간주 되지만, 소스 코드는 모든 대상 컴파일러에서 지원 되는 c/C++ 를 사용 하 고 조건부로 포함 된 플랫폼을 포함 하거나 포함 하지 않는 플랫폼을 사용 하 여 실제로 플랫폼 간 이어야 합니다. 컴파일러 관련 코드입니다.

궁극적으로 코드는 모든 대상 플랫폼에서 성공적으로 컴파일되고 실행 되어야 하므로 대상으로 지정 되는 플랫폼 (및 컴파일러)에서 구축 됩니다. 컴파일러 간의 사소한 차이로 인해 문제가 발생 하 여 각 대상 플랫폼에 대 한 철저 한 테스트 (가능한 자동화)가 점점 더 중요 해지고 있습니다.  

## <a name="high-level-approach"></a>상위 수준 접근 방식

아래 그림은 C/C++ 소스 코드를 NuGet을 통해 공유 되는 플랫폼 간 xamarin 라이브러리로 변환 하는 데 사용 되는 4 단계 방법을 보여 줍니다. 그러면 xamarin.ios 앱에서 사용 됩니다.
 
![Xamarin을 사용 하 여 C/C++ 를 사용 하는 상위 수준 접근 방법](images/cpp-steps.jpg)

4 단계는 다음과 같습니다.

1. C/C++ 소스 코드를 플랫폼별 네이티브 라이브러리로 컴파일합니다.
2. Visual Studio 솔루션을 사용 하 여 네이티브 라이브러리 래핑
3. .NET 래퍼에 대 한 NuGet 패키지를 압축 하 고 푸시합니다.
4. Xamarin 앱에서 NuGet 패키지 사용

### <a name="stage-1-compiling-the-cc-source-code-into-platform-specific-native-libraries"></a>1 단계: C/C++ 소스 코드를 플랫폼별 네이티브 라이브러리로 컴파일

이 단계의 목표는 C# 래퍼에 의해 호출 될 수 있는 네이티브 라이브러리를 만드는 것입니다. 이는 상황에 따라 달라질 수도 있고 관련이 없을 수도 있습니다. 이 일반적인 시나리오에서 다룰 수 있는 많은 도구와 프로세스는이 문서의 범위를 벗어나는 것입니다. 핵심 고려 사항은 C/C++ 코드 베이스를 모든 네이티브 래퍼 코드, 충분 한 단위 테스트 및 빌드 자동화와 동기화 하는 것입니다. 

연습에서 라이브러리는 함께 제공 되는 셸 스크립트와 함께 Visual Studio Code를 사용 하 여 만들어졌습니다. 이 연습에 대 한 확장 버전은 샘플의이 부분에 대해 자세히 설명 하는 [모바일 CAT GitHub 리포지토리에서](https://github.com/xamarin/mobcat/blob/dev/samples/cpp_with_xamarin/) 찾을 수 있습니다. 이 경우 네이티브 라이브러리는 타사 종속성으로 처리 되지만이 단계는 컨텍스트에 대해 설명 되어 있습니다.

간단히 하기 위해이 연습은 아키텍처의 하위 집합만을 대상으로 합니다. IOS의 경우 lipo 유틸리티를 사용 하 여 개별 아키텍처 관련 이진 파일에서 단일 fat 이진 파일을 만듭니다. Android는와 함께 동적 이진 파일을 사용 하므로 확장명 및 iOS는 확장명이 인 정적 fat 이진 파일을 사용 합니다. 

### <a name="stage-2-wrapping-the-native-libraries-with-a-visual-studio-solution"></a>2 단계: Visual Studio 솔루션을 사용 하 여 네이티브 라이브러리 래핑

다음 단계는 .NET에서 쉽게 사용할 수 있도록 네이티브 라이브러리를 래핑하는 것입니다. 이 작업은 네 개의 프로젝트를 포함 하는 Visual Studio 솔루션을 사용 하 여 수행 됩니다. 공유 프로젝트는 공용 코드를 포함 합니다. 각 Xamarin.ios, Xamarin.ios 및 .NET Standard를 대상으로 하는 프로젝트는 라이브러리를 플랫폼에 관계 없이 참조할 수 있습니다.

이 래퍼는 Paul bait에서 설명 하는 ' the[and switch 트릭](https://log.paulbetts.org/the-bait-and-switch-pcl-trick/)'을 사용 합니다. 이는 유일한 방법이 아니라 라이브러리를 쉽게 참조할 수 있도록 하므로 소비 응용 프로그램 자체 내에서 플랫폼별 구현을 명시적으로 관리 하지 않아도 됩니다. 트릭은 기본적으로 대상 (.NET Standard, Android, iOS)이 동일한 네임 스페이스, 어셈블리 이름 및 클래스 구조를 공유 하는지 확인 하는 것입니다. NuGet은 항상 플랫폼별 라이브러리를 선호 하므로 .NET Standard 버전은 런타임에 사용 되지 않습니다.

이 단계의 대부분의 작업에서는 P/Invoke를 사용 하 여 네이티브 라이브러리 메서드를 호출 하 고 기본 개체에 대 한 참조를 관리 하는 데 중점을 둡니다. 목표는 복잡성을 추상화 하는 동시에 라이브러리의 기능을 소비자에 게 노출 하는 것입니다. Xamarin.ios 개발자는 관리 되지 않는 라이브러리의 내부 작업에 대 한 작업 지식을 가질 필요가 없습니다. 다른 관리 되 C# 는 라이브러리를 사용 하는 것과 같습니다.

궁극적으로이 단계의 출력은 다음 단계에서 패키지를 작성 하는 데 필요한 정보가 포함 된 nuspec 문서와 함께 대상 마다 하나씩 .NET 라이브러리 집합입니다.

**3 단계: .NET 래퍼에 대 한 NuGet 패키지 압축 및 푸시**

세 번째 단계는 이전 단계의 빌드 아티팩트를 사용 하 여 NuGet 패키지를 만드는 것입니다. 이 단계의 결과는 Xamarin 앱에서 사용할 수 있는 NuGet 패키지입니다. 이 연습에서는 로컬 디렉터리를 사용 하 여 NuGet 피드 역할을 수행 합니다. 프로덕션 환경에서이 단계는 패키지를 공용 또는 개인 NuGet 피드에 게시 해야 하며 완전히 자동화 되어야 합니다.

**4 단계: Xamarin Forms 앱에서 NuGet 패키지 사용**

마지막 단계는 Xamarin.ios 앱에서 NuGet 패키지를 참조 하 고 사용 하는 것입니다. 이렇게 하려면 이전 단계에서 정의한 피드를 사용 하도록 Visual Studio에서 NuGet 피드를 구성 해야 합니다.

피드가 구성 되 면 플랫폼 간 Xamarin.ios 앱의 각 프로젝트에서 패키지를 참조 해야 합니다. ' Bait-switch 트릭 '은 동일한 인터페이스를 제공 하므로 단일 위치에 정의 된 코드를 사용 하 여 네이티브 라이브러리 기능을 호출할 수 있습니다.

소스 코드 리포지토리에는 Azure DevOps에서 개인 NuGet 피드를 설정 하는 방법 및 해당 피드에 패키지를 푸시하는 방법에 대 한 문서가 포함 된 [추가 읽기 목록이](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin#wrapping-up) 포함 되어 있습니다. 로컬 디렉터리 보다 설치 시간이 약간 더 필요 하지만이 유형의 피드는 팀 개발 환경에서 더 효율적입니다.

## <a name="walk-through"></a>연습

제공 된 단계는 **Mac용 Visual Studio**에만 해당 되지만 구조는 **Visual Studio 2017** 에서도 작동 합니다.

### <a name="prerequisites"></a>필수 구성 요소

이를 수행 하려면 개발자가 다음을 수행 해야 합니다.

- [NuGet 명령줄 (CLI)](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)

- [*Visual Studio* *for Mac*](https://visualstudio.microsoft.com/downloads)

> [!NOTE]
> IPhone에 앱을 배포 하려면 활성 [**Apple 개발자 계정이**](https://developer.apple.com/) 필요 합니다.

## <a name="creating-the-native-libraries-stage-1"></a>네이티브 라이브러리 만들기 (1 단계)

네이티브 라이브러리 기능은 [연습의 예제를 기반으로 합니다. 정적 라이브러리 만들기 및 사용 (C++)](https://docs.microsoft.com/cpp/windows/walkthrough-creating-and-using-a-static-library-cpp?view=vs-2017)을 사용 합니다.

이 연습에서는이 시나리오에서 라이브러리가 타사 종속성으로 제공 되기 때문에 기본 라이브러리를 빌드하는 첫 번째 단계를 건너뜁니다. 미리 컴파일된 네이티브 라이브러리는 [샘플 코드](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin) 와 함께 포함 되거나 직접 [다운로드할](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin/Sample/Artefacts) 수 있습니다.

### <a name="working-with-the-native-library"></a>네이티브 라이브러리 작업

원래 *MathFuncsLib* 예제에는 다음 정의를 사용 `MyMathFuncs` 하 여 라는 단일 클래스가 포함 됩니다.

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

추가 클래스는 .net 소비자가 기본 네이티브 `MyMathFuncs` 클래스를 만들고, 삭제 하 고, 상호 작용할 수 있도록 하는 래퍼 함수를 정의 합니다.

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

[Xamarin](https://visualstudio.microsoft.com/xamarin/) 쪽에서 사용 되는 이러한 래퍼 함수가 됩니다.

## <a name="wrapping-the-native-library-stage-2"></a>네이티브 라이브러리 래핑 (2 단계)

이 단계를 수행 하려면 [이전 섹션](##creating-the-native-libraries-stage-1)에서 설명 하는 [미리 컴파일된 라이브러리가](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin/Sample/Artefacts) 필요 합니다.

### <a name="creating-the-visual-studio-solution"></a>Visual Studio 솔루션 만들기

1. **Mac용 Visual Studio**에서 *시작 페이지*에서 **새 프로젝트** 를 클릭 하거나 *파일* 메뉴에서 새 **솔루션** 을 클릭 합니다.
2. **새 프로젝트** 창에서 **공유 프로젝트** ( *다중 플랫폼 > 라이브러리*에서)를 선택 하 고 **다음**을 클릭 합니다.
3. 다음 필드를 업데이트 한 후 **만들기**를 클릭 합니다.

    - **프로젝트 이름:** MathFuncs.Shared  
    - **솔루션 이름:** MathFuncs  
    - **위치도** 기본 저장 위치 사용 (또는 대안 선택)   
    - **솔루션 디렉터리 내에 프로젝트를 만듭니다.** 이 확인란을 선택 된 상태로 설정 합니다.
4. **솔루션 탐색기**에서 **MathFuncs** 프로젝트를 두 번 클릭 하 고 **주 설정**으로 이동 합니다.
5. 제거 **.**  **MathFuncs** only로 설정 되도록 **기본 네임 스페이스** 에서 공유 하 고 **확인**을 클릭 합니다.
6. 템플릿에서 만든 **MyClass.cs** 을 연 다음 클래스와 파일 이름을 모두 **MyMathFuncsWrapper** 로 바꾸고 네임 스페이스를 **MathFuncs**로 변경 합니다.
7. **MathFuncs**솔루션을 **제어** 하 고 클릭 한 다음 **추가** 메뉴에서 **새 프로젝트 추가** ...를 선택 합니다.
8. **새 프로젝트** 창에서 **.NET Standard 라이브러리** ( *다중 플랫폼 > 라이브러리*에서)를 선택 하 고 **다음**을 클릭 합니다.
9. **.NET Standard 2.0** 을 선택 하 고 **다음**을 클릭 합니다.
10. 다음 필드를 업데이트 한 후 **만들기**를 클릭 합니다.

    - **프로젝트 이름:** MathFuncs.Standard  
    - **위치도** 공유 프로젝트와 동일한 저장 위치 사용   

11. **솔루션 탐색기**에서 **MathFuncs** 프로젝트를 두 번 클릭 합니다.
12. **주 설정**으로 이동 하 고 **기본 네임 스페이스** 를 **MathFuncs**로 업데이트 합니다.
13. **출력** 설정으로 이동한 다음 **어셈블리 이름을** **MathFuncs**로 업데이트 합니다.
14. **컴파일러** 설정으로 이동 하 여 **구성을** **릴리스**로 변경 하 고 **디버그 정보** 를 **기호로** 설정한 다음 **확인**을 클릭 합니다.
15. 프로젝트에서 **시작 된 Class1.cs/Getting** 를 삭제 합니다 (이러한 항목 중 하나가 템플릿의 일부로 포함 된 경우).
16. 프로젝트 **종속성/참조** 폴더를 제어 하 고 **클릭** 한 다음 **참조 편집**을 선택 합니다.
17. **프로젝트** 탭에서 **MathFuncs** 를 선택 하 고 **확인**을 클릭 합니다.
18. 다음 구성을 사용 하 여 7-17 단계 (9 단계 무시)를 반복 합니다.

    | **프로젝트 이름**  | **템플릿 이름**   | **새 프로젝트 메뉴**   |
    |-------------------| --------------------| -----------------------|
    | MathFuncs.Android | 클래스 라이브러리       | Android > 라이브러리      |
    | MathFuncs.iOS     | 바인딩 라이브러리     | iOS > 라이브러리          |

19. **솔루션 탐색기**에서 **MathFuncs** 프로젝트를 두 번 클릭 한 다음 **컴파일러** 설정으로 이동 합니다.

20. **구성이** **디버그**로 설정 된 상태에서 **Android**를 포함 하도록 **기호 정의** 를 편집 합니다.

21. **구성을** **릴리스로**변경한 다음 **기호 정의** 를 편집 하 여 **Android;** 도 포함 합니다.

22. **MathFuncs**에 대해 19-20 단계를 반복 하 고, 두 경우 모두 **Android** 대신 **IOS** 를 포함 하도록 **기호 정의** 를 편집 합니다.

23. **릴리스** 구성 (**컨트롤 + 명령 + B**)에서 솔루션을 빌드하고 세 개의 출력 어셈블리 (Android, iOS, .NET Standard) (각 프로젝트 bin 폴더)가 동일한 이름 **MathFuncs**를 공유 하는지 확인 합니다.

이 단계에서 솔루션에는 Android, iOS 및 .NET Standard에 대 한 하나의 각각 세 개의 대상이 각각 참조 하는 공유 프로젝트인 3 개의 대상이 있어야 합니다. 동일한 이름을 가진 동일한 기본 네임 스페이스와 출력 어셈블리를 사용 하도록 구성 해야 합니다. 이는 앞에서 언급 한 ' bait 및 switch ' 접근 방식에 필요 합니다.

### <a name="adding-the-native-libraries"></a>네이티브 라이브러리 추가

네이티브 라이브러리를 래퍼 솔루션에 추가 하는 프로세스는 Android와 iOS 사이에서 약간 다릅니다.

#### <a name="native-references-for-mathfuncsandroid"></a>MathFuncs에 대 한 네이티브 참조

1. **MathFuncs** 프로젝트를 **제어** 하 고 클릭 한 다음 **추가** 메뉴 이름 지정 **라이브러리**에서 **새 폴더** 를 선택 합니다.

2. 각 **abi** (응용 프로그램 이진 인터페이스)에 대해 **라이브러리** 폴더를 **Ctrl + 클릭** 한 다음 **추가** 메뉴에서 **새 폴더** 를 선택 하 여 해당 **ABI**뒤의 이름을 지정 합니다. 이 경우:

    - arm64-v8a
    - armeabi-v7a
    - x86
    - x86_64  

    > [!NOTE]
    > 자세한 개요는 [NDK 개발자 가이드](https://developer.android.com/ndk/guides/)의 [아키텍처 및 cpu](https://developer.android.com/ndk/guides/arch) 항목, 특히 [앱 패키지의 네이티브 코드](https://developer.android.com/ndk/guides/abis#native-code-in-app-packages)주소 지정 섹션을 참조 하세요.

3. 폴더 구조를 확인 합니다.  

    ```folders
    - lib
        - arm64-v8a
        - armeabi-v7a
        - x86
        - x86_64
    ```

4. 다음 매핑을 기반으로 각 **ABI** 폴더에 해당 하는 라이브러리를 추가 **합니다** .

    **arm64-arm64-v8a:** Lib/Android/arm64

    **armeabi-armeabi-v7a:** Lib/Android/arm  

    **x86:** Lib/Android/x86

    **x86_64:** Lib/Android/x86_64

    > [!NOTE]
    > 파일을 추가 하려면 해당 **ABI**를 나타내는 폴더를 **ctrl + 클릭** 한 다음 **추가** 메뉴에서 **파일 추가 ...** 를 선택 합니다. **PrecompiledLibs** 디렉터리에서 적절 한 라이브러리를 선택 하 고 **열기** 를 클릭 한 다음 **확인** 을 클릭 하 여 *디렉터리에 파일을 복사*합니다.

5. 각 파일에 대해 **ctrl + 클릭** 을 누른 다음 **빌드 작업** 메뉴에서 **EmbeddedNativeLibrary** 옵션을 선택 합니다 **.**

이제 **라이브러리** 폴더가 다음과 같이 표시 됩니다.

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

#### <a name="native-references-for-mathfuncsios"></a>MathFuncs에 대 한 네이티브 참조

1. **MathFuncs** 프로젝트를 **제어** 하 고 클릭 한 다음 **추가** 메뉴에서 **네이티브 참조 추가** 를 선택 합니다. 
2. LibMathFuncs 라이브러리 ( **PrecompiledLibs** 디렉터리 아래의 lib/ios에서)를 선택 하 고 **열기** 를 클릭 **합니다.** 
3. **네이티브 참조** 폴더에서 **LibMathFuncs** 파일을 제어 하 고 **클릭** 한 다음 메뉴에서 **속성** 옵션을 선택 합니다.  
4. **속성** 패드에서 체크 인 되는 **기본 참조** 속성 (눈금 아이콘 표시)을 구성 합니다.

    - 강제 로드
    - 되었습니다C++
    - 스마트 링크

    > [!NOTE]
    > 바인딩 라이브러리 프로젝트 형식을 [네이티브 참조](https://docs.microsoft.com/xamarin/cross-platform/macios/native-references) 와 함께 사용 하면 정적 라이브러리가 포함 되 고,이 라이브러리를 참조 하는 xamarin.ios 앱과 자동으로 연결 될 수 있습니다 (NuGet 패키지를 통해 포함 된 경우에도).

5. **ApiDefinition.cs**를 열고 템플릿 처리 된 코드 ( `MathFuncs` 네임 스페이스만 남겨 둡니다)를 삭제 한 다음 **Structs.cs** 에 대해 동일한 단계를 수행 합니다. 

    > [!NOTE]
    > 바인딩 라이브러리 프로젝트를 빌드하려면 이러한 파일 ( *Objcbindingapidefinition* 및 *Objcbindingcor9build* 작업 포함)이 필요 합니다. 그러나 표준 P/Invoke를 사용 하 여 Android 및 iOS 라이브러리 대상 모두에서 공유할 수 있는 방식으로 이러한 파일 외부에서 네이티브 라이브러리를 호출 하는 코드를 작성 합니다.

### <a name="writing-the-managed-library-code"></a>관리 되는 라이브러리 코드 작성

이제 네이티브 라이브러리를 C# 호출 하는 코드를 작성 합니다. 목표는 기본적인 복잡성을 숨기는 것입니다. 소비자는 네이티브 라이브러리 내부 또는 P/Invoke 개념에 대 한 작업 지식이 필요 하지 않습니다.  

#### <a name="creating-a-safehandle"></a>SafeHandle 만들기

1. **MathFuncs** 프로젝트를 **제어** 하 고 클릭 한 다음 **추가** 메뉴에서 **파일 추가 ...** 를 선택 합니다. 
2. **새 파일** 창에서 **빈 클래스** 를 선택 하 고 이름을 **MyMathFuncsSafeHandle** 로 지정한 다음 **새로 만들기** 를 클릭 합니다.
3. **MyMathFuncsSafeHandle** 클래스를 구현 합니다.

    ```csharp
    using System;
    using Microsoft.Win32.SafeHandles;

    namespace MathFuncs
    {
        internal class MyMathFuncsSafeHandle : SafeHandleZeroOrMinusOneIsInvalid
        {
            public MyMathFuncsSafeHandle() : base(true) { }

            public IntPtr Ptr => this.handle;

            protected override bool ReleaseHandle()
            {
                // TODO: Release the handle here
                return true;
            }
        }
    }
    ```

    > [!NOTE]
    > [SafeHandle](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.safehandle?view=netframework-4.7.2) 은 관리 코드에서 관리 되지 않는 리소스로 작업 하는 데 선호 되는 방법입니다. 이는 중요 한 종료 및 개체 수명 주기와 관련 된 많은 상용구 코드를 요약 합니다. 이 핸들의 소유자는 나중에 다른 관리 되는 리소스와 같이 취급할 수 있으며 전체 [삭제 가능 패턴](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose)을 구현 하지 않아도 됩니다. 

#### <a name="creating-the-internal-wrapper-class"></a>내부 래퍼 클래스 만들기

1. **MyMathFuncsWrapper.cs**를 열고 내부 정적 클래스로 변경 합니다.

    ```csharp
    namespace MathFuncs
    {
        internal static class MyMathFuncsWrapper
        {
        }
    }
    ```

2. 동일한 파일에서 다음 조건문을 클래스에 추가 합니다.

    ```csharp
    #if Android
        const string DllName = "libMathFuncs.so";
    #else
        const string DllName = "__Internal";
    #endif
    ```

    > [!NOTE]
    > 그러면 라이브러리가 **Android** 또는 **iOS**용으로 빌드 되었는지 여부에 따라 **DllName** 상수 값이 설정 됩니다. 이는 각 플랫폼에서 사용 되는 다양 한 명명 규칙을 해결 하는 것입니다 .이 경우에 사용 되는 라이브러리의 형식 이기도 합니다. Android는 동적 라이브러리를 사용 하므로 확장명을 포함 하는 파일 이름이 필요 합니다. IOS의 경우 정적 라이브러리를 사용 하므로 ' *__Internal*'이 필요 합니다.

3. **MyMathFuncsWrapper.cs** 파일의 맨 위에 t e m에 대 한 참조를 추가 합니다 **.**

    ```csharp
    using System.Runtime.InteropServices;
    ```

4. **MyMathFuncs** 클래스의 생성 및 삭제를 처리 하는 래퍼 메서드를 추가 합니다.

    ```csharp
    [DllImport(DllName, EntryPoint = "CreateMyMathFuncsClass")]
    internal static extern MyMathFuncsSafeHandle CreateMyMathFuncs();

    [DllImport(DllName, EntryPoint = "DisposeMyMathFuncsClass")]
    internal static extern void DisposeMyMathFuncs(MyMathFuncsSafeHandle ptr);
    ```

    > [!NOTE]
    > .NET 런타임에 해당 라이브러리 내에서 호출할 함수의 이름을 명시적으로 알려주는 **EntryPoint** 와 함께 상수 **DllName** 을 **DllImport** 특성에 전달 합니다. 기술적으로 관리 되는 메서드 이름이 관리 되지 않는 메서드와 동일한 경우 **EntryPoint** 값을 제공할 필요가 없습니다. 제공 되지 않은 경우 관리 되는 메서드 이름이 대신 **EntryPoint** 로 사용 됩니다. 그러나 명시적으로 하는 것이 좋습니다.  

5. 다음 코드를 사용 하 여 **MyMathFuncs** 클래스를 사용할 수 있도록 래퍼 메서드를 추가 합니다.

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
    > 이 예제에서는 매개 변수에 대해 단순 유형을 사용 합니다. 이 경우에는 마샬링이 비트 복사 이므로 파트에 대 한 추가 작업이 필요 하지 않습니다. 또한 표준 **IntPtr**대신 **MyMathFuncsSafeHandle** 클래스를 사용 하는 것을 확인 합니다. **IntPtr** 은 마샬링 프로세스의 일부로 자동으로 **SafeHandle** 에 매핑됩니다.

6. 완성 된 **MyMathFuncsWrapper** 클래스가 아래와 같이 표시 되는지 확인 합니다.

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

1. **MyMathFuncsSafeHandle** 클래스를 열고 **releasehandle** 메서드 내의 자리 표시자 **TODO** 주석으로 이동 합니다.

    ```csharp
    // TODO: Release the handle here
    ```

1. **TODO** 줄을 바꿉니다.

    ```csharp
    MyMathFuncsWrapper.DisposeMyMathFuncs(this);
    ```

#### <a name="writing-the-mymathfuncs-class"></a>MyMathFuncs 클래스 작성

이제 래퍼가 완성 되었으므로 관리 되지 않는 C++ MyMathFuncs 개체에 대 한 참조를 관리 하는 MyMathFuncs 클래스를 만듭니다.  

1. **MathFuncs** 프로젝트를 **제어** 하 고 클릭 한 다음 **추가** 메뉴에서 **파일 추가 ...** 를 선택 합니다. 
2. **새 파일** 창에서 **빈 클래스** 를 선택 하 고 이름을 **MyMathFuncs** 로 지정한 다음 **새로 만들기** 를 클릭 합니다.
3. **MyMathFuncs** 클래스에 다음 멤버를 추가 합니다.

    ```csharp
    readonly MyMathFuncsSafeHandle handle;
    ```

4. 클래스가 인스턴스화될 때 네이티브 **MyMathFuncs** 개체에 대 한 핸들을 만들어 저장 하도록 클래스에 대 한 생성자를 구현 합니다.

    ```csharp
    public MyMathFuncs()
    {
        handle = MyMathFuncsWrapper.CreateMyMathFuncs();
    }
    ```

5. 다음 코드를 사용 하 여 **IDisposable** 인터페이스를 구현 합니다.

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

6. **MyMathFuncsWrapper** 클래스를 사용 하 여 **MyMathFuncs** 메서드를 구현 하 여 내부 관리 되지 않는 개체에 저장 한 포인터를 전달 함으로써 내부적으로 실제 작업을 수행 합니다. 코드는 다음과 같아야 합니다.

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

#### <a name="creating-the-nuspec"></a>Nuspec 만들기

NuGet을 통해 라이브러리를 패키지 하 고 배포 하기 위해 솔루션에는 **nuspec** 파일이 필요 합니다. 그러면 지원 되는 각 플랫폼에 대해 포함 되는 결과 어셈블리를 식별할 수 있습니다.

1. **MathFuncs**솔루션을 **제어** 하 고 클릭 한 다음 **추가** 메뉴에서 **솔루션 폴더 추가** 를 선택 하 여 솔루션 폴더 이름을 지정 **합니다.**
2. **솔루션에서 솔루션** 을 마우스 단추로 **클릭** 한 다음 **추가** 메뉴에서 **새 파일 ...** 을 선택 합니다.
3. **새 파일** 창에서 **빈 XML 파일** 을 선택 하 고 이름을 **MathFuncs. Nuspec** 로 지정한 후 **새로 만들기**를 클릭 합니다.
4. **NuGet** 소비자에 게 표시 되는 기본 패키지 메타 데이터를 사용 하 여 **MathFuncs. nuspec** 를 업데이트 합니다. 예를 들어:

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
    > 이 매니페스트에 사용 된 스키마에 대 한 자세한 내용은 [nuspec 참조](https://docs.microsoft.com/nuget/reference/nuspec) 문서를 참조 하세요.

5. 요소를 `<files>` `<package>` 요소의 `<file>` 자식으로 추가 하 여 각 파일을 개별 요소로 식별 합니다. `<metadata>`

    ```xml
    <files>

        <!-- Android -->

        <!-- iOS -->

        <!-- netstandard2.0 -->

    </files>
    ```

    > [!NOTE]
    > 패키지를 프로젝트에 설치 하 고 동일한 이름으로 여러 어셈블리를 지정 하는 경우 NuGet은 지정 된 플랫폼과 가장 고유한 어셈블리를 효과적으로 선택 합니다.

6. Android 어셈블리에 대 한 요소를추가`<file>` 합니다.

    ```xml
    <file src="MathFuncs.Android/bin/Release/MathFuncs.dll" target="lib/MonoAndroid81/MathFuncs.dll" />
    <file src="MathFuncs.Android/bin/Release/MathFuncs.pdb" target="lib/MonoAndroid81/MathFuncs.pdb" />
    ```

7. IOS 어셈블리 `<file>` 에 대 한 요소를 추가 합니다.

    ```xml
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.dll" target="lib/Xamarin.iOS10/MathFuncs.dll" />
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.pdb" target="lib/Xamarin.iOS10/MathFuncs.pdb" />
    ```

8. `<file>` **Netstandard 2.0** 어셈블리에 대 한 요소를 추가 합니다.

    ```xml
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.dll" target="lib/netstandard2.0/MathFuncs.dll" />
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.pdb" target="lib/netstandard2.0/MathFuncs.pdb" />
    ```

9. **Nuspec** 매니페스트를 확인 합니다.

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
    > 이 파일은 **릴리스** 빌드에서 어셈블리 출력 경로를 지정 하므로 해당 구성을 사용 하 여 솔루션을 빌드해야 합니다.

이 시점에서 솔루션은 3 개의 .NET 어셈블리와 지원 **nuspec** 매니페스트를 포함 합니다.

## <a name="distributing-the-net-wrapper-with-nuget"></a>NuGet을 사용 하 여 .NET 래퍼 배포

다음 단계는 NuGet 패키지를 패키지 및 배포 하 여 앱에서 쉽게 사용 하 고 종속성으로 관리할 수 있도록 하는 것입니다. 래핑 및 소비는 모두 단일 솔루션 내에서 수행할 수 있지만 NuGet 지원을 통해 라이브러리를 배포 하면 이러한 코드 베이스를 독립적으로 관리할 수 있습니다.

### <a name="preparing-a-local-packages-directory"></a>로컬 패키지 디렉터리 준비

NuGet 피드의 가장 간단한 형태는 로컬 디렉터리입니다.

1. **Finder**에서 편리한 디렉터리로 이동 합니다. 예를 들면 **/사용자**입니다.
2. **파일** 메뉴에서 **새 폴더** 를 선택 하 여 **로컬 nuget 피드와**같은 의미 있는 이름을 제공 합니다.

### <a name="creating-the-package"></a>패키지 만들기

1. **빌드 구성을** **릴리스**로 설정 하 고 **명령 + B**를 사용 하 여 빌드를 실행 합니다.
2. **터미널** 을 열고 **nuspec** 파일이 포함 된 폴더로 디렉터리를 변경 합니다.
3. **터미널**에서 **Nuspec** 파일, **버전** (예: 1.0.0) 및 **OutputDirectory** ( [이전 단계](https://docs.microsoft.com/xamarin/cross-platform/cpp/index#creating-a-local-nuget-feed)에서 만든 폴더를 사용 하 여)를 지정 하는 **nuget pack** 명령을 실행 합니다. 즉,  **로컬 nuget 피드**. 예:

    ```bash
    nuget pack MathFuncs.nuspec -Version 1.0.0 -OutputDirectory ~/local-nuget-feed
    ```

4. **MathFuncs nupkg** 가 **로컬 nuget 피드** 디렉터리에 만들어졌는지 **확인** 합니다.

### <a name="optional-using-a-private-nuget-feed-with-azure-devops"></a>필드 Azure DevOps에서 개인 NuGet 피드 사용

보다 강력한 기법은 [Azure DevOps의 NuGet 패키지 시작](https://docs.microsoft.com/azure/devops/artifacts/get-started-nuget?view=vsts&tabs=new-nav#publish-a-package)에 설명 되어 있습니다. 여기에는 개인 피드를 만들고 이전 단계에서 생성 된 패키지를 해당 피드에 푸시하는 방법을 보여 줍니다.

이 워크플로를 완전히 자동화 하는 것이 좋습니다 (예: [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/index?view=vsts)사용). 자세한 내용은 [Azure Pipelines 시작](https://docs.microsoft.com/azure/devops/pipelines/get-started/index?view=vsts)을 참조 하세요.

## <a name="consuming-the-net-wrapper-from-a-xamarinforms-app"></a>Xamarin Forms 앱에서 .NET 래퍼 사용

이 연습을 완료 하려면 로컬 **NuGet** 피드에 게시 된 패키지를 사용 하는 **Xamarin Forms** 앱을 만듭니다.

### <a name="creating-the-xamarinforms-project"></a>**Xamarin Forms** 프로젝트 만들기

1. **Mac용 Visual Studio**의 새 인스턴스를 엽니다. **터미널**에서이 작업을 수행할 수 있습니다.

    ```bash
    open -n -a "Visual Studio"
    ```

2. **Mac용 Visual Studio**에서 *시작 페이지*에서 **새 프로젝트** 를 클릭 하거나 *파일* 메뉴에서 새 **솔루션** 을 클릭 합니다.
3. **새 프로젝트** 창에서 *다중 플랫폼 > 앱*내에서 **빈 양식 앱** 을 선택 하 고 **다음**을 클릭 합니다.
4. 다음 필드를 업데이트 하 고 다음 **을 클릭 합니다**.

    - **앱 이름:** MathFuncsApp.
    - **조직 식별자:** 역방향 네임 스페이스 (예: _com. {your_org}_ )를 사용 합니다.
    - **대상 플랫폼:** 기본 (Android 및 iOS 대상 모두)을 사용 합니다.
    - **공유 코드:** 이를 .NET Standard ("공유 라이브러리" 솔루션은 가능 하지만이 연습에서는 다루지 않음)로 설정 합니다.

5. 다음 필드를 업데이트 한 후 **만들기**를 클릭 합니다.

    - **프로젝트 이름:** MathFuncsApp.
    - **솔루션 이름:** MathFuncsApp.  
    - **위치도** 기본 저장 위치를 사용 하거나 다른 위치를 선택 합니다.

6. **솔루션 탐색기**에서 초기 테스트를 위해 대상 (**MathFuncsApp** 또는 **MathFuncs**)을 제어 하 고 **클릭** 한 다음 **시작 프로젝트로 설정**을 선택 합니다.
7. 선호 하는 **장치** 또는 **시뮬레이터**/**에뮬레이터**를 선택 합니다. 
8. 솔루션 (**명령 + 반환**)을 실행 하 여 템플릿 **xamarin.ios** 프로젝트를 빌드하고 실행 하는지 확인 합니다. 

    > [!NOTE]
    > **iOS** (특히 시뮬레이터)는 빌드/배포 시간이 가장 빠릅니다.

### <a name="adding-the-local-nuget-feed-to-the-nuget-configuration"></a>NuGet 구성에 로컬 NuGet 피드 추가

1. **Visual studio**의 **visual Studio** 메뉴에서 **기본 설정** 을 선택 합니다.
2. **NuGet** 섹션에서 **소스** 를 선택 하 고 **추가**를 클릭 합니다.
3. 다음 필드를 업데이트 한 다음 **원본 추가**를 클릭 합니다.

    - **Name:** 로컬 패키지와 같은 의미 있는 이름을 제공 합니다.  
    - **위치도** [이전 단계](#preparing-a-local-packages-directory)에서 만든 **로컬 nuget 피드** 폴더를 지정 합니다.

    > [!NOTE]
    > 이 경우 **사용자 이름과** **암호**를 지정할 필요가 없습니다. 

4. **확인**을 클릭합니다.

### <a name="referencing-the-package"></a>패키지 참조

각 프로젝트에 대해 다음 단계를 반복 합니다 (**MathFuncsApp**, **MathFuncsApp**및 **MathFuncsApp**).

1. 프로젝트를 제어 하 고 **클릭** 한 다음 **추가** 메뉴에서 **NuGet 패키지 추가 ...** 를 선택 합니다.
2. **MathFuncs**를 검색 합니다. 
3. 패키지 **버전이** **1.0.0** 인지 확인 하 고 다른 세부 정보는 **제목** 및 **설명**, 즉 *MathFuncs* 및  *C++ 샘플 래퍼 라이브러리*와 같이 예상 대로 표시 됩니다. 
4. **MathFuncs** 패키지를 선택 하 고 **패키지 추가**를 클릭 합니다.

### <a name="using-the-library-functions"></a>라이브러리 함수 사용

이제 각 프로젝트의 **MathFuncs** 패키지에 대 한 참조를 사용 하 여 C# 코드에 함수를 사용할 수 있습니다.

1. **MathFuncsApp** 일반 **xamarin.ios**프로젝트 내에서 **MainPage.xaml.cs** 를 엽니다 ( **MathFuncsApp** 및 **MathFuncsApp**둘 다에서 참조).
2. 파일의 맨 위에 있는 Diagnostics 및 **MathFuncs** 에 **using** 문을 추가 **합니다** .

    ```csharp
    using System.Diagnostics;
    using MathFuncs;
    ```

3. 클래스의 맨 위에 `MyMathFuncs` `MainPage` 클래스의 인스턴스를 선언 합니다.

    ```csharp
    MyMathFuncs myMathFuncs;
    ```

4. 기본 클래스 `OnAppearing` 에서 `OnDisappearing` 및 메서드를 재정의 합니다. `ContentPage`

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

5. 메서드를 `OnAppearing` 업데이트 하 여 이전 `myMathFuncs` 에 선언 된 변수를 초기화 합니다.

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();
        myMathFuncs = new MyMathFuncs();
    }
    ```

6. 메서드를 업데이트 하 여에서 `Dispose` `myMathFuncs`메서드를 호출 합니다. `OnDisappearing`

    ```csharp
    protected override void OnDisappearing()
    {
        base.OnAppearing();
        myMathFuncs.Dispose();
    }
    ```

7. 다음과 같이 **TestMathFuncs** 라는 private 메서드를 구현 합니다.

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

8. 마지막으로 `OnAppearing` 메서드 `TestMathFuncs` 끝에서를 호출 합니다.

    ```csharp
    TestMathFuncs();
    ```

9. 각 대상 플랫폼에서 앱을 실행 하 고 다음과 같이 **응용 프로그램 출력** 패드에서 출력이 표시 되는지 확인 합니다.

    ```csharp
    1 + 2 = 3
    1 - 2 = -1
    1 * 2 = 2
    1 / 2 = 0.5
    ```

    > [!NOTE]
    > Android에서 테스트할 때 '*system.dllnotfoundexception*'가 발생 하거나 iOS에서 빌드 오류가 발생 하는 경우 사용 중인 장치/에뮬레이터/시뮬레이터의 CPU 아키텍처가 지원 하도록 선택한 하위 집합과 호환 되는지 확인 해야 합니다. 

## <a name="summary"></a>요약

이 문서에서는 NuGet 패키지를 통해 배포 되는 공용 .NET 래퍼를 통해 네이티브 라이브러리를 사용 하는 Xamarin Forms 앱을 만드는 방법을 설명 했습니다. 이 연습에서 제공 하는 예제는 의도적으로 간단 하 게 방법을 보여 주는 매우 간단한 방법입니다. 실제 응용 프로그램은 예외 처리, 콜백, 더 복잡 한 형식의 마샬링, 다른 종속성 라이브러리와의 연결 등의 복잡성을 처리 해야 합니다. 핵심 고려 사항은 C++ 코드의 진화를 조정 하 고 래퍼 및 클라이언트 응용 프로그램과 동기화 하는 프로세스입니다. 이 프로세스는 이러한 문제 중 하나 또는 둘 다가 단일 팀에서 담당 하는지 여부에 따라 달라질 수 있습니다. 어느 방법이 든 자동화는 진정한 장점입니다. 다음은 관련 다운로드와 함께 몇 가지 주요 개념에 대 한 추가 정보를 제공 하는 리소스입니다. 

### <a name="downloads"></a>다운로드

- [NuGet 명령줄 (CLI) 도구](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)
- [Visual Studio](https://visualstudio.microsoft.com/vs)

### <a name="examples"></a>예

- [를 사용한 하이퍼 경과 플랫폼 간 모바일 개발C++](https://blogs.msdn.microsoft.com/vcblog/2015/06/26/hyperlapse-cross-platform-mobile-development-with-visual-c-and-xamarin/)
- [Microsoft Pix (C++ 및 Xamarin)](https://devblogs.microsoft.com/xamarin/microsoft-research-ships-intelligent-apps-with-the-power-of-c-and-ai/)
- [Mono San, 샘플 포트](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sanangeles-ndk/)

### <a name="further-reading"></a>추가 정보

[이 게시물의 내용과 관련 된 문서](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin#wrapping-up)
