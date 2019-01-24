---
ms.assetid: EA2D979E-9151-4CE9-9289-13B6A979838B
title: Xamarin을 사용 하 여 C/c + + 라이브러리를 사용 합니다.
description: 'Xamarin을 사용 하 여 구축 하 고 Android 및 iOS 용 모바일 앱 플랫폼 간 C/c + + 코드를 통합 Mac 용 visual Studio를 사용할 수 있습니다 하 고 C#입니다. 이 문서에는 설정 하 고 Xamarin 앱에서 c + + 프로젝트를 디버그 하는 방법을 설명 합니다.'
author: mikeparker104
ms.author: miparker
ms.date: 12/17/20178
---
# <a name="use-cc-libraries-with-xamarin"></a>Xamarin을 사용 하 여 C/c + + 라이브러리를 사용 합니다.

## <a name="overview"></a>개요

Xamarin은 개발자가 Visual Studio를 사용 하 여 플랫폼 간 네이티브 모바일 앱을 만들 수 있습니다. 일반적으로 C# 바인딩은 기존 플랫폼 구성 요소 개발자에 게 노출 하는 데 사용 됩니다. 그러나 기존 작업 Xamarin 앱에 필요가 코드 베이스의 경우 경우가 있습니다. 팀은 단순히 없는 경우도 코드 베이스를 포트 a에 크고 잘 테스트 된 고도로 최적화 된 리소스, 예산, 시간 또는 C#입니다.

[플랫폼 간 모바일 개발용 visual c + +](https://docs.microsoft.com/visualstudio/cross-platform/visual-cpp-for-cross-platform-mobile-development) C/c + +를 사용 하도록 설정 하 고 C# 통합 된 디버깅 환경을 포함 하 여 많은 이점을 제공 합니다. 동일한 솔루션의 일부로 빌드해야 하는 코드입니다. Microsoft에 사용 되는 C/c + + 및 Xamarin와 같은 앱을 제공 하려면 이렇게에서 [Hyperlapse Mobile](https://www.microsoft.com/p/hyperlapse-mobile/9wzdncrd1prw) 하 고 [Pix 카메라](https://www.microsoft.com/microsoftpix)합니다.

그러나 경우에 따라 있습니다 인지 원하는 (요구 사항) 되도록 기존 C/c + + 도구 및 프로세스를 진행에서 하 고 타사 구성 요소에 비슷한 것 처럼 라이브러리를 처리 하는 응용 프로그램에서 분리 하는 라이브러리 코드를 유지 합니다. 이러한 상황에서 챌린지 없습니다만 공개 하는 관련 멤버 C# 종속성으로 라이브러리를 관리 하지만 합니다. 물론,이 프로세스를 가능한 한 많은 자동화를 합니다.  

이 게시물은이 시나리오에 대 한 높은 수준의 방법에 간략하게 설명 하 고 간단한 예제를 안내 합니다.

## <a name="background"></a>배경

C/c + + 플랫폼 간 언어에서 것으로 간주 됩니다 있지만 유용한 주의 해야 실제로 플랫폼 간 C/c + +만 모든 대상 컴파일러에서 지 원하는 거의 또는 전혀 조건부로 포함 된 플랫폼을 포함 하 고 사용 하 여 소스 코드 인지 확인 하거나 컴파일러 별 코드입니다.

궁극적으로 코드를 컴파일하고 하므로이를 나타낸다고 공통성 대상 플랫폼 (및 컴파일러)에서 모든 대상 플랫폼에서 성공적으로 실행 해야 합니다. 문제가 사소한 차이점 컴파일러에서에서 여전히 발생할 수 있습니다 및 따라서 철저 한 테스트 (가급적 자동) 각 대상 플랫폼이 점점 더 중요 합니다.  

## <a name="high-level-approach"></a>고급 접근 방식

다음 그림에서는 NuGet을 통해 공유 및 Xamarin.Forms 앱에서 사용 되는 플랫폼 간 Xamarin 라이브러리에 C/c + + 소스 코드를 변환 하는 데 4 단계 접근 방식을 나타냅니다.
 

![Xamarin을 사용 하 여 C/c + +를 사용 하기 위한 고급 방법](images/cpp-steps.jpg)

4 단계는 다음과 같습니다.

1.  플랫폼별 기본 라이브러리를 C/c + + 소스 코드를 컴파일할 때.
2.  Visual Studio 솔루션을 사용 하 여 네이티브 라이브러리를 배치 합니다.
3.  압축 하 고.NET 래퍼에 대 한 NuGet 패키지를 푸시합니다.
4.  Xamarin 앱에서 NuGet 패키지를 사용 합니다.

### <a name="stage-1-compiling-the-cc-source-code-into-platform-specific-native-libraries"></a>1 단계: 플랫폼별 기본 라이브러리를 C/c + + 소스 코드를 컴파일할 때

이 단계의 목표에서 호출할 수 있는 네이티브 라이브러리를 만드는 것을 C# 래퍼입니다. 이 수 또는 상황에 따라 관련 되지 않을 수 있습니다. 많은 도구 및 일반적인 시나리오에서 제공 될 수 있습니다 하는 프로세스를이 문서에서 다루지 않습니다. 주요 고려 사항이 C/c + + codebase 네이티브 래퍼 코드에 충분 한 단위 테스트와 동기화 하는 유지 되며 빌드 자동화 합니다. 

이 연습의의 라이브러리를 함께 제공 되는 셸 스크립트를 사용 하 여 Visual Studio Code를 사용 하 여 생성 되었습니다. 이 연습의 확장된 버전에서 찾을 수 있습니다 합니다 [Mobile CAT GitHub 리포지토리](https://github.com/xamarin/mobcat/blob/dev/samples/cppwithxamarin/README.md) 자세히 샘플의이 부분에 설명 하는 합니다. 하지만 네이티브 라이브러리는 처리 될 타사 종속성으로이 경우 컨텍스트에 대해이 단계를 보여 줍니다.


간단히 하기 위해이 연습에는 아키텍처의 하위 집합만 대상 으로합니다. Ios의 경우 개별 아키텍처별 이진 파일에서 단일 fat 이진 만들려면 lipo 유틸리티를 사용 합니다. Android 동적 이진 파일을 사용 하 여.so 확장명이 및 iOS는 정적 fat 이진을 사용 하 여.a 확장명으로 합니다. 

### <a name="stage-2-wrapping-the-native-libraries-with-a-visual-studio-solution"></a>2 단계: Visual Studio 솔루션을 사용 하 여 네이티브 라이브러리 래핑

다음 단계로을.NET에서 손쉽게 사용할 수 있도록 네이티브 라이브러리를 래핑하는 것입니다. 4 개 프로젝트가 Visual Studio 솔루션을 사용 하 여 수행 됩니다. 공유 프로젝트에는 일반적인 코드를 포함합니다. 각각의 Xamarin.Android, Xamarin.iOS 및.NET Standard를 대상으로 하는 프로젝트 라이브러리 플랫폼과 상관 없는 방식으로 참조할 수를 허용 합니다.

래퍼는[bait 및 스위치 트릭](https://log.paulbetts.org/the-bait-and-switch-pcl-trick/),' Paul Betts에서 설명 합니다. 유일한 방법은 아닙니다. 하지만 쉽게 라이브러리 참조 및 플랫폼별 구현을 소비 응용 프로그램 자체 내에서 명시적으로 관리할 필요를 방지 하기. 중요 한 것은 기본적으로 대상 (.NET Standard, Android, iOS) 동일한 네임 스페이스, 어셈블리 이름 및 클래스 구조를 공유 하는 보장 됩니다. NuGet은 플랫폼별 라이브러리를 선호 항상 하므로.NET 표준 버전을 런타임에 사용 되지 않습니다.

이 단계에서는 작업의 대부분을 중점적으로 P/Invoke를 사용 하 여 네이티브 라이브러리 메서드를 호출 하 고 기본 개체에 대 한 참조를 관리 합니다. 목표는 모든 복잡성을 추상화 하는 동안 소비자에 게 라이브러리의 기능을 노출 하는 것입니다. Xamarin.Forms 개발자는 관리 되지 않는 라이브러리의 내부 작업에 대 한 작업 지식이 필요가 없습니다. 다른 관리를 사용 하는 같은 있어야 C# 라이브러리입니다.

궁극적으로이 단계의 출력은 다음 단계에서 패키지를 빌드하기 위해 필요한 정보를 포함 하는 nuspec 문서와 함께 대상 하나씩.NET 라이브러리 집합입니다.

**3 단계: 압축 및.NET 래퍼에 대 한 NuGet 패키지를 푸시**

세 번째 단계는 이전 단계에서 빌드 아티팩트를 사용 하 여 NuGet 패키지를 만드는 것입니다. 이 단계의 결과 Xamarin 앱에서 사용할 수 있는 NuGet 패키지. 이 연습에서는 NuGet 피드로 사용할 로컬 디렉터리를 사용 합니다. 프로덕션 환경에서는이 단계는 public로 패키지를 게시 해야 하거나 개인 NuGet 피드 및 완전히 자동화 되어야 합니다.

**4 단계: Xamarin.Forms 앱에서 NuGet 패키지를 사용합니다.**

마지막 단계는 참조 하 고 Xamarin.Forms 앱에서 NuGet 패키지를 사용 하는 것입니다. 그러려면 이전 단계에서 정의 된 피드를 사용 하도록 Visual Studio에서 피드의 NuGet 구성 합니다.

피드 구성 되 면 패키지는 플랫폼 간 Xamarin.Forms 앱에서 각 프로젝트에서 참조할 수 해야 합니다. 'Bait 및 스위치 트릭' 네이티브 라이브러리 기능을 한 곳에 정의 된 코드를 사용 하 여 호출할 수 있도록 동일한 인터페이스를 제공 합니다.

소스 코드 리포지토리를 포함 한 [추가 참고 자료 목록](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin#wrapping-up) 개인 NuGet 피드 Azure DevOps를 설정 하는 방법 및 해당 피드에 패키지를 푸시하는 방법에 대 한 문서를 포함 하는. 로컬 디렉터리 보다 좀 더 많은 설치 시간을 필요로 하는 동안이 피드의 유형은 팀 개발 환경에서 더 합니다.

## <a name="walk-through"></a>연습

제공 된 단계에 따라 다릅니다 **Mac 용 Visual Studio**, 구조에서 작동 하지만 **Visual Studio 2017** 도 합니다.

### <a name="prerequisites"></a>전제 조건

작업을 수행 하기 위해 개발자가 필요 합니다.

-   [NuGet 명령줄 (CLI)](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)

-   [*Visual Studio* *for Mac*](https://visualstudio.microsoft.com/downloads)

> [!NOTE]
> 활성 [**Apple 개발자 계정**](https://developer.apple.com/) iPhone에 앱을 배포 하기 위해 필요 합니다.

## <a name="creating-the-native-libraries-stage-1"></a>네이티브 라이브러리 (1 단계) 만들기

예제를 기반으로 하는 네이티브 라이브러리 기능 [연습: 만들기 및 정적 라이브러리 (c + +)를 사용 하 여](https://docs.microsoft.com/cpp/windows/walkthrough-creating-and-using-a-static-library-cpp?view=vs-2017)입니다.

이 연습에서는 라이브러리는이 시나리오에서는 타사 종속성으로 제공 하므로 네이티브 라이브러리를 빌드하는 첫 번째 단계를 건너뜁니다. 미리 컴파일된 네이티브 라이브러리와 함께 포함 된 합니다 [샘플 코드](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin) 수 있습니다 [다운로드](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin/Sample/Artefacts) 직접.

### <a name="working-with-the-native-library"></a>네이티브 라이브러리 사용

원래 *MathFuncsLib* 예제에서는 다음 정의 사용 하 여 MyMathFuncs 라는 단일 클래스를 포함 합니다. 

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

추가 클래스를 만들고, dispose, 기본 네이티브 MyMathFuncs 클래스를 사용 하 여 상호 작용 하는.NET 소비자를 허용 하는 래퍼 함수를 정의 합니다.

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

이러한 래퍼 함수에서 사용 되는 것은 [Xamarin](https://visualstudio.microsoft.com/xamarin/) 쪽입니다.

## <a name="wrapping-the-native-library-stage-2"></a>네이티브 라이브러리 (2 단계)를 배치합니다.

이 단계에 필요 합니다 [라이브러리를 미리 컴파일된](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin/Sample/Artefacts) 에 설명 된 합니다 [이전 섹션](https://docs.microsoft.com/xamarin/cross-platform/cpp/index).

### <a name="creating-the-visual-studio-solution"></a>Visual Studio 솔루션 만들기

1. **Mac 용 Visual Studio**, 클릭 **새 프로젝트** (에서 합니다 *홈페이지*) 또는 **새 솔루션** (에서 *파일* 메뉴).
2. **새 프로젝트** 창 선택 **공유 프로젝트** (내에서 *다중 플랫폼 > 라이브러리*)을 클릭 한 다음 **다음**합니다.
3. 다음 필드를 업데이트 한 다음 클릭 **만들기**:

    - **프로젝트 이름:** MathFuncs.Shared  
    - **솔루션 이름:** MathFuncs  
    - **위치:** 기본값을 사용 하 여 저장 위치 (또는 대 안으로 선택)   
    - **솔루션 디렉터리 내에서 프로젝트를 만듭니다.** 설정 확인
4. **솔루션 탐색기**를 두 번 클릭 합니다 **MathFuncs.Shared** 프로젝트를 탐색 합니다 **주 설정**합니다.
5. 제거 **합니다. 공유** 에서 합니다 **Default Namespace** 로 설정 되어 있으므로 **MathFuncs** 만 클릭 **확인**합니다.
6. 오픈 **MyClass.cs** (템플릿에서 생성), 클래스 및 파일 이름입니다. 다음 이름을 **MyMathFuncsWrapper** 네임 스페이스를 변경 하 고 **MathFuncs**합니다.
7. **컨트롤에서 클릭** 솔루션 **MathFuncs**를 선택한 **새 프로젝트 추가...**  에서 합니다 **추가** 메뉴.
8. **새 프로젝트** 창 선택 **.NET 표준 라이브러리** (내에서 *다중 플랫폼 > 라이브러리*)을 클릭 한 다음 **다음**합니다.
9. 선택할 **.NET Standard 2.0** 을 클릭 한 다음 **다음**합니다.
10. 다음 필드를 업데이트 한 다음 클릭 **만들기**:

    - **프로젝트 이름:** MathFuncs.Standard  
    - **위치:** 저장 위치 공유 프로젝트와 동일한 것을 사용 합니다.   

11. **솔루션 탐색기**를 두 번 클릭 합니다 **MathFuncs.Standard** 프로젝트입니다.
12. 이동할 **기본 설정**를 업데이트 한 다음 **Default Namespace** 에 **MathFuncs**합니다.
13. 로 이동 합니다 **출력** 설정을 업데이트 **어셈블리 이름** 에 **MathFuncs** 클릭 **확인**합니다.
14. 이동할를 **컴파일러** 설정을 변경 합니다 **구성** 에 **릴리스**설정 **디버그 정보** 에  **만 기호**합니다.
15. 삭제할 **Class1.cs/Getting 시작** (템플릿의 일부로 포함 된 다음 중 하나) 하는 경우 프로젝트에서.
16. **컨트롤에서 클릭** 프로젝트 **종속성/References** 폴더를 선택한 **참조 편집**합니다.
17. 선택 **MathFuncs.Shared** 에서 합니다 **프로젝트** 탭을 클릭 한 다음 **확인**합니다.
18. 7-17 (무시 하 고 단계 9) 단계를 반복 다음 구성을 사용 하 여:

    | **프로젝트 이름**  | **템플릿 이름**   | **새 프로젝트 메뉴**   |
    |-------------------| --------------------| -----------------------|
    | MathFuncs.Android | 클래스 라이브러리       | Android > 라이브러리      |
    | MathFuncs.iOS     | 바인딩 라이브러리     | iOS > 라이브러리          |

19. **솔루션 탐색기**, 두 번 클릭 합니다 **MathFuncs.Android** 프로젝트를 선택한 다음로 이동 합니다 **컴파일러** 설정.

20. 사용 하 여는 **구성** 로 설정 **디버그**, 편집 **기호 정의** 하기로 **Android;** 합니다.

21. 변경 된 **구성** 에 **릴리스**, 다음 편집 **기호 정의** 포함 하도록 **Android;** 합니다.

22. 19 ~ 20에 대 한 단계를 반복 **MathFuncs.iOS**, 편집 **기호 정의** 하기로 **iOS;** 대신 **Android;** 두 경우 모두 합니다.

23. 솔루션을 빌드합니다 **릴리스** configuration (**컨트롤 + COMMAND + B**) (각 프로젝트 bin 폴더)의 모든 3 개의 출력 어셈블리 (Android, iOS,.NET Standard)는 동일한 공유의 유효성을 검사 하 고 이름을 **MathFuncs.dll**합니다.

이 단계에서는 솔루션에 3 개 대상, Android, iOS 및.NET Standard 및 각 3 개 대상에서 참조 되는 공유 프로젝트에 대해 각각 하나씩 있어야 합니다. 이러한 동일한 이름의 동일한 기본 네임 스페이스 및 출력 어셈블리를 사용 하도록 구성 되어야 합니다. 앞서 언급 했 듯이 ' bait 및 스위치 ' 접근 방식에 대 한 필요한 경우

### <a name="adding-the-native-libraries"></a>네이티브 라이브러리를 추가합니다.

네이티브 라이브러리 래퍼 솔루션에 추가 하는 프로세스는 Android와 iOS 간에 약간 다릅니다.

#### <a name="native-references-for-mathfuncsandroid"></a>MathFuncs.Android에 대 한 네이티브 참조

1. **컨트롤에서 클릭** 에 **MathFuncs.Android** 프로젝트를 선택한 **새 폴더** 에서 **추가** 이름을 지정 하는 메뉴 **libs**.

2. 각 **ABI** (응용 프로그램 이진 인터페이스) **컨트롤 + 클릭** 에 **libs** 폴더를 선택한 다음 ** 새 폴더** 에서 합니다 **추가** 메뉴에서 그 이름을 해당 **ABI**. 이 경우:

    - arm64-v8a
    - armeabi-v7a
    - x86
    - x86_64  

    > [!NOTE]
    > 자세한 개요를 참조 하세요.는 [아키텍처 및 Cpu](https://developer.android.com/ndk/guides/arch) 에서 항목을 [NDK 개발자 가이드](https://developer.android.com/ndk/guides/)를 주소 지정에 대 한 섹션 특히 [앱 패키지의 네이티브 코드 ](https://developer.android.com/ndk/guides/abis#native-code-in-app-packages).

3. 폴더 구조를 확인 합니다.  

    ```
    - lib
        - arm64-v8a
        - armeabi-v7a
        - x86
        - x86_64
    ```

4. 해당 추가 **.so** 의 각 라이브러리는 **ABI** 다음 매핑을 기준으로 폴더:

    **arm64-v8a:** libs/Android/arm64

    **armeabi-v7a:** libs/Android/arm  

    **x86:** libs/Android/x86

    **x86_64:** libs/Android/x86_64

    > [!NOTE]
    > 파일을 추가 하려면 **컨트롤에서 클릭** 각각 나타내는 폴더 **ABI**를 선택한 **파일 추가...**  에서 합니다 **추가** 메뉴. 적절 한 라이브러리를 선택 (에서 합니다 **PrecompiledLibs** 디렉터리) 클릭 **열기** 클릭 하 고 **확인** 기본 옵션을 그대로 두고 *복사는 파일을 디렉터리*합니다.

5. 각각에 대 한는 **.so** 파일을 **컨트롤 + 클릭** 선택한를 **EmbeddedNativeLibrary** 에서 옵션을 **빌드 작업** 메뉴.

이제는 **libs** 폴더가 다음과 같이 표시 되어야 합니다.

```bash
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

#### <a name="native-references-for-mathfuncsios"></a>MathFuncs.iOS에 대 한 네이티브 참조

1. **컨트롤에서 클릭** 에 **MathFuncs.iOS** 프로젝트를 선택한 **네이티브 참조 추가** 에서 합니다 **추가** 메뉴. 
2. 선택 된 **libMathFuncs.a** 라이브러리 (libs/ios 아래에서 **PrecompiledLibs** 디렉터리) 클릭 **열기** 
3. **컨트롤 + 클릭** 에 **libMathFuncs** 파일 (내는 **네이티브 참조** 폴더를 선택한는 **속성** 메뉴에서 옵션  
4. 구성 된 **네이티브 참조** 속성 (눈금 아이콘을 표시) 확인에 **속성** 패드:
        
    - 강제 로드
    - c + +
    - 스마트 링크 

    > [!NOTE]
    > 바인딩 라이브러리 프로젝트 형식을 함께 사용 하는 [네이티브 참조](https://docs.microsoft.com/xamarin/cross-platform/macios/native-references) 정적 라이브러리를 포함 하 고 (경우에 해당 NuGet 패키지를 통해 포함)을 참조 하는 Xamarin.iOS 앱을 사용 하 여 자동으로 연결할 수 있도록 합니다. 

5. 오픈 **ApiDefinition.cs**, 주석 처리 된 코드 템플릿 삭제 (만 남아 합니다 **MathFuncs** 네임 스페이스), 다음에 대 한 동일한 단계를 수행 **Structs.cs** 

    > [!NOTE]
    > 바인딩 라이브러리 프로젝트에 이러한 파일 (사용 하 여는 *ObjCBindingApiDefinition* 하 고 *ObjCBindingCoreSource* 빌드 작업) 빌드해야 합니다. 그러나 표준 P/Invoke를 사용 하 여 Android와 iOS 라이브러리 대상 간에 공유할 수 있는 방식으로 이러한 파일 외부에서 네이티브 라이브러리를 호출 하기 위한 코드를 작성 합니다.

### <a name="writing-the-managed-library-code"></a>관리 되는 라이브러리 코드를 작성합니다.

이제 작성 된 C# 네이티브 라이브러리를 호출 하는 코드입니다. 목표는 내재 된 복잡성을 숨기려면 것입니다. 소비자는 모든 지식을 네이티브 라이브러리 내부 요소 또는 P/Invoke 개념 필요는 없습니다.  

#### <a name="creating-a-safehandle"></a>SafeHandle 만들기

1. **컨트롤에서 클릭** 에 **MathFuncs.Shared** 프로젝트를 선택한 **파일 추가...**  에서 합니다 **추가** 메뉴. 
2. 선택 **빈 클래스** 에서 합니다 **새 파일** 창에서 이름을 **MyMathFuncsSafeHandle** 클릭 하 고 **새로 만들기**
3. 구현 된 **MyMathFuncsSafeHandle** 클래스:

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
    > A [SafeHandle](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.safehandle?view=netframework-4.7.2) 관리 코드에서 관리 되지 않는 리소스를 사용 하 여 작업에 대 한 기본 방법입니다. 많은 중요 한 종료 및 개체 수명 주기 관련 상용구 코드를 추상화 합니다. 이 핸들의 소유자 이후에를 처리할 수 같은 기타 리소스를 관리 하 고 전체를 구현할 필요가 없습니다 [삭제 가능한 패턴](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose)합니다. 

#### <a name="creating-the-internal-wrapper-class"></a>내부 래퍼 클래스 만들기

1. 오픈 **MyMathFuncsWrapper.cs**, 내부 정적 클래스에 변경

    ```csharp
    namespace MathFuncs
    {
        internal static class MyMathFuncsWrapper
        {
        }
    }
    ```

2. 동일한 파일에서 클래스에 다음 조건문을 추가 합니다.

    ```csharp
    #if Android
        const string DllName = "libMathFuncs.so";
    #else
        const string DllName = "__Internal";
    #endif
    ```

    > [!NOTE]
    > 이 설정 된 **DllName** 상수 값에 대 한 라이브러리를 작성 중인 여부에 따라 **Android** 또는 **iOS**합니다. 이 해당 각 플랫폼 뿐만 아니라이 경우에 사용 되는 라이브러리의 형식에서 사용 하는 다양 한 명명 규칙을 해결 하는 것입니다. Android은 동적 라이브러리를 사용 하며 따라서 확장명을 포함 하는 파일 이름입니다. IOS에 대 한 '*__Internal*' 정적 라이브러리를 사용 하 고 있으므로 필요 합니다.

3. 에 대 한 참조를 추가 **System.Runtime.InteropServices** 맨 위에 있는 합니다 **MyMathFuncsWrapper.cs** 파일

    ```csharp
    using System.Runtime.InteropServices;
    ```

4. 생성 및 삭제를 처리 하기 위한 래퍼 메서드를 추가 합니다 **MyMathFuncs** 클래스:

    ```csharp
    [DllImport(DllName, EntryPoint = "CreateMyMathFuncsClass")]
    internal static extern MyMathFuncsSafeHandle CreateMyMathFuncs();

    [DllImport(DllName, EntryPoint = "DisposeMyMathFuncsClass")]
    internal static extern void DisposeMyMathFuncs(MyMathFuncsSafeHandle ptr);
    ```

    > [!NOTE]
    > 이 상수에 전달 하는 것 **DllName** 에 **DllImport** 함께 특성을 **EntryPoint** 명시적으로 지시 하는.NET 런타임이 호출할 함수의 이름입니다 해당 라이브러리입니다. 제공할 필요가 없습니다 기술적으로 **EntryPoint** 우리 관리 되는 메서드 이름 된 관리 되지 않는 한 동일한 값입니다. 관리 되는 메서드 이름으로 사용할 하나를 제공 하지 않으면 경우는 **EntryPoint** 대신 합니다. 그러나 명시적 이어야 하는 것이 좋습니다.  

5. 작업을 사용 하 여 사용할 수 있도록 래퍼 메서드를 추가 합니다 **MyMathFuncs** 다음 코드를 사용 하 여 클래스:

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
    > 이 예제에서 매개 변수에 대 한 단순 형식을 사용 하는 것입니다. 비트 복사입니다 마샬링 되므로 부분은의 추가 작업 없이 사용 하이 경우에 필요 합니다. 또한 사용을 확인 합니다 **MyMathFuncsSafeHandle** 표준 대신 클래스 **IntPtr**. 합니다 **IntPtr** 자동으로 매핑되는 **SafeHandle** 마샬링 프로세스의 일부로.

6. 확인 완료 합니다 **MyMathFuncsWrapper** 클래스도 표시 됩니다. 아래:

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

#### <a name="completing-the-mymathfuncssafehandle-class"></a>MyMathFuncsSafeHandle 클래스를 완료합니다.
1. 엽니다는 **MyMathFuncsSafeHandle** 클래스, 자리 표시자로 이동 **TODO** 내 주석 합니다 **ReleaseHandle** 메서드:
    ```csharp
    // TODO: Release the handle here
    ```
2. 대체는 **TODO** 줄:

    ```csharp
    MyMathFuncsWrapper.DisposeMyMathFuncs(this);
    ```

#### <a name="writing-the-mymathfuncs-class"></a>MyMathFuncs 클래스를 작성합니다.

래퍼를 완료 했으므로 관리 되지 않는 c + + MyMathFuncs 개체에 대 한 참조를 관리 하는 MyMathFuncs 클래스를 만듭니다.  

1. **컨트롤에서 클릭** 에 **MathFuncs.Shared** 프로젝트를 선택한 **파일 추가...**  에서 합니다 **추가** 메뉴. 
2. 선택 **빈 클래스** 에서 합니다 **새 파일** 창에서 이름을 **MyMathFuncs** 클릭 하 고 **새로 만들기**
3. 다음 멤버를 추가 합니다 **MyMathFuncs** 클래스:

    ```csharp
    readonly MyMathFuncsSafeHandle handle;
    ```

4. 만들고 네이티브에 대 한 핸들을 저장 하므로 클래스의 생성자를 구현 **MyMathFuncs** 클래스를 인스턴스화할 때 개체:

    ```csharp
    public MyMathFuncs()
    {
        handle = MyMathFuncsWrapper.CreateMyMathFuncs();
    }
    ```

5. 구현 된 **IDisposable** 다음 코드를 사용 하 여 인터페이스:

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

6. 구현 된 **MyMathFuncs** 메서드를 사용 하 여를 **MyMathFuncsWrapper** 에서는 저장 된 포인터에서 기본 관리 되지 않는 개체를 전달 하 여 내부에서 실제 작업을 수행 하는 클래스입니다. 코드는 다음과 같아야 합니다.

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

#### <a name="creating-the-nuspec"></a>Nuspec

솔루션 패키지 되어 NuGet을 통해 배포 되며 라이브러리를가 하기 위해 필요한를 **nuspec** 파일입니다. 이 지원 되는 각 플랫폼에 대해 포함 되는 결과 어셈블리를 식별 합니다.

1.  **컨트롤에서 클릭** 솔루션 **MathFuncs**를 선택한 **솔루션 폴더 추가** 에서 합니다 **추가** 이름을 지정 하는 메뉴  **SolutionItems**합니다.
2.  **컨트롤에서 클릭** 에 **SolutionItems** 폴더를 선택한 **새 파일... **  에서 합니다 **추가** 메뉴.
3.  선택 **빈 XML 파일** 에서 합니다 **새 파일** 창에서 이름을 **MathFuncs.nuspec** 클릭및 **새**합니다.
4.  업데이트 **MathFuncs.nuspec** 표시할 기본 패키지 메타 데이터를 사용 하 여 합니다 **NuGet** 소비자입니다. 예를 들어:


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
    >  참조 된 [nuspec 참조](https://docs.microsoft.com/nuget/reference/nuspec) 이 매니페스트에 대 한 사용 하는 스키마에 대 한 자세한 내용은 문서입니다.

5. 추가 `<files>` 의 자식 요소로 합니다 `<package>` 요소 (바로 아래 `<metadata>`), 각 파일을 식별 하는 별도의 `<file>` 요소:

    ```xml
    <files>

        <!-- Android -->

        <!-- iOS -->        

        <!-- netstandard2.0 -->

    </files>
    ```

    > [!NOTE]
    > 패키지를 프로젝트에 설치 될 때 및 여러 개의 어셈블리가 동일한 이름으로 지정 하는 경우 NuGet를 효과적으로 지정 된 플랫폼에만 한정 되는 어셈블리를 선택 합니다.

6. 추가 된 `<file>` 에 대 한 요소를 **Android** 어셈블리:

    ```xml
    <file src="MathFuncs.Android/bin/Release/MathFuncs.dll" target="lib/MonoAndroid81/MathFuncs.dll" />
    <file src="MathFuncs.Android/bin/Release/MathFuncs.pdb" target="lib/MonoAndroid81/MathFuncs.pdb" />
    ```

7. 추가 된 `<file>` 에 대 한 요소를 **iOS** 어셈블리:

    ```xml
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.dll" target="lib/Xamarin.iOS10/MathFuncs.dll" />
    <file src="MathFuncs.iOS/bin/Release/MathFuncs.pdb" target="lib/Xamarin.iOS10/MathFuncs.pdb" />
    ```

8. 추가 된 `<file>` 에 대 한 요소를 **netstandard2.0** 어셈블리:

    ```xml
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.dll" target="lib/netstandard2.0/MathFuncs.dll" />
    <file src="MathFuncs.Standard/bin/Release/netstandard2.0/MathFuncs.pdb" target="lib/netstandard2.0/MathFuncs.pdb" />
    ```

9. 확인 합니다 **nuspec** 매니페스트:

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
    > 이 파일에서 어셈블리 출력 경로 지정 된 **릴리스** 빌드, 해당 구성을 사용 하 여 솔루션을 구축 해야 합니다.

이 시점에서 솔루션에 포함 된 세 명의.NET 어셈블리 및을 지 원하는 **nuspec** 매니페스트 합니다.

## <a name="distributing-the-net-wrapper-with-nuget"></a>NuGet 사용 하 여.NET 래퍼를 배포합니다.

다음 단계 패키지 있습니다 수 쉽게 앱에서 사용 되 고 있도록 종속성으로 관리 되는 NuGet 패키지를 배포 하는 것입니다. 줄 바꿈 및 소비 수 모두를 단일 솔루션 내에 있지만 분리 하 고 사용 하도록 설정의 NuGet 지원을 통해 라이브러리 배포 관리를 코드 베이스 하지 독립적으로 합니다.

### <a name="preparing-a-local-packages-directory"></a>로컬 패키지 디렉터리 준비

가장 간단한 형태의 NuGet 피드는 로컬 디렉터리:

1.   **Finder**, 편리한 디렉터리로 이동 합니다. 예를 들어 **/Users**합니다.
2.  선택 **새 폴더** 에서 합니다 **파일** 메뉴와 같은 의미 있는 이름을 제공 **로컬 nuget 바꿈**합니다.

### <a name="creating-the-package"></a>패키지 만들기

1.  설정 합니다 **빌드 구성** 에 **릴리스**를 사용 하 여 빌드 실행 **COMMAND + B**합니다.
2.  오픈 **터미널** 포함 된 폴더로 디렉터리를 변경 합니다 **nuspec** 파일입니다.
3.   **터미널**, 실행 합니다 **nuget 팩** 명령을 지정 하는 **nuspec** 파일인을 **버전**  (예: 1.0.0), 및 **OutputDirectory** 에서 만든 폴더를 사용 하는 [이전 단계](https://docs.microsoft.com/xamarin/cross-platform/cpp/index#creating-a-local-nuget-feed), 즉 ** 로컬 nuget 바꿈**합니다. 예를 들어:

    ```bash
    nuget pack MathFuncs.nuspec -Version 1.0.0 -OutputDirectory ~/local-nuget-feed
    ```

4. **확인** 는 **MathFuncs.1.0.0.nupkg** 에서 만들어진 것을 **로컬 nuget 공급** 디렉터리.

### <a name="optional-using-a-private-nuget-feed-with-azure-devops"></a>[선택 사항] Azure DevOps를 사용 하 여 개인을 사용 하 여 NuGet 피드

설명 하는 보다 강력한 기법 [Azure DevOps에서 NuGet 패키지를 사용 하 여 시작](https://docs.microsoft.com/azure/devops/artifacts/get-started-nuget?view=vsts&tabs=new-nav#publish-a-package), 해당 피드에 개인 피드를 만들고 패키지를 푸시하는 (이전 단계에서 생성) 하는 방법을 보여 줍니다.

완전히 자동화를 사용 하 여 예를 들어이 워크플로를 적합 [Azure 파이프라인](https://docs.microsoft.com/azure/devops/pipelines/index?view=vsts)합니다. 자세한 내용은 [Azure 파이프라인 시작](https://docs.microsoft.com/azure/devops/pipelines/get-started/index?view=vsts)합니다.

## <a name="consuming-the-net-wrapper-from-a-xamarinforms-app"></a>Xamarin.Forms 앱에서.NET 래퍼를 사용합니다.
연습을 완료 하려면 만듭니다는 **Xamarin.Forms** 앱 패키지를 바로 사용을 로컬에 게시 **NuGet** 피드 합니다.

### <a name="creating-the-xamarinforms-project"></a>만들기는 **Xamarin.Forms** 프로젝트

1. 새 인스턴스를 엽니다 **Mac 용 Visual Studio**합니다. 이 작업을 수행할 수 있습니다 **터미널**:

    ```bash
    open -n -a "Visual Studio"
    ```

2. **Mac 용 Visual Studio**, 클릭 **새 프로젝트** (에서 합니다 *홈페이지*) 또는 **새 솔루션** (에서 *파일* 메뉴).
3. **새 프로젝트** 창 선택 **빈 Forms 앱** (내에서 *다중 플랫폼 > 앱*)을 클릭 한 다음 **다음**합니다.
4. 다음 필드를 업데이트 한 다음 클릭 **다음**:

    - **앱 이름:** MathFuncsApp.
    - **조직 식별자:** 예를 들어 역방향 네임 스페이스를 사용 하 여 _com. {your_org}_ 합니다.
    - **대상 플랫폼:** 기본값 (Android 및 iOS 모두 대상)를 사용 합니다.
    - **공유 코드:** .NET 표준 ("공유 라이브러리" 솔루션은 가능 하지만이 연습에서 다루지)로 설정 합니다.

5. 다음 필드를 업데이트 한 다음 클릭 **만들기**:

    - **프로젝트 이름:** MathFuncsApp.
    - **솔루션 이름:** MathFuncsApp.  
    - **위치:** 기본값을 사용 하 여 저장 위치 (또는 대 안으로 선택).

6. **솔루션 탐색기**, **컨트롤 + 클릭** 대상에서 (**MathFuncsApp.Android** 하거나 **MathFuncs.iOS**) 다음 초기 테스트를 위해 선택할 **시작 프로젝트로 설정**합니다.
7. 원하는 선택 **장치** 하거나 **시뮬레이터**/**에뮬레이터**합니다. 
8. 솔루션을 실행 합니다 (**명령 + RETURN**) 되었는지 확인 하려면 템플릿 **Xamarin.Forms** 프로젝트가 빌드되고 실행 해도 됩니다. 

    > [!NOTE]
    > **iOS** (특히 시뮬레이터) 빠른 빌드/배포 시간을 포함 하려는 경향이 있습니다.

### <a name="adding-the-local-nuget-feed-to-the-nuget-configuration"></a>NuGet 구성에 피드 로컬 NuGet 추가

1. **Visual Studio**, 선택 **기본 설정** (에서 합니다 **Visual Studio** 메뉴).
2. 선택 **원본을** 아래에서 합니다 **NuGet** 섹션을 선택한 다음 클릭 **추가**합니다.
3. 다음 필드를 업데이트 한 다음 클릭 **소스 추가**:

    - **이름:** 예를 들어, 로컬 패키지 의미 있는 이름을 제공 합니다.  
    - **위치:** 지정 된 **로컬 nuget 공급** 에서 만든 폴더를 [이전 단계](#preparing-a-local-packages-directory).

    > [!NOTE]
    > 이 있는 경우 지정할 필요가 없습니다를 **사용자 이름** 하 고 **암호**합니다. 

4. **확인**을 클릭합니다.

### <a name="referencing-the-package"></a>패키지 참조

각 프로젝트에 대해 다음 단계를 반복 (**MathFuncsApp**하십시오 **MathFuncsApp.Android**, 및 **MathFuncsApp.iOS**).

1. **컨트롤에서 클릭** 프로젝트에서 선택한 **NuGet 패키지 추가...**  에서 합니다 **추가** 메뉴.
2. 검색할 **MathFuncs**합니다. 
3. 확인 합니다 **버전** 패키지입니다 **1.0.0** 기타 세부 정보 같은 예상 대로 나타나고를 **제목** 및 **설명**, 즉 를 *MathFuncs* 하 고 *c + + 래퍼 라이브러리 샘플*합니다. 
4. 선택 된 **MathFuncs** 패키지를 클릭 **패키지 추가**합니다.

### <a name="using-the-library-functions"></a>라이브러리 함수를 사용 하 여

에 대 한 참조를 사용 하 여 이제는 **MathFuncs** 함수 프로젝트를 각 패키지에 사용할 수는 C# 코드입니다.

1.  열기 **MainPage.xaml.cs** 내에서 **MathFuncsApp** 공통 **Xamarin.Forms**프로젝트 (둘 다에서참조 **MathFuncsApp.Android** 하 고 **MathFuncsApp.iOS**).
2.  추가 **를 사용 하 여** 에 대 한 문을 **System.Diagnostics** 고 **MathFuncs** 파일의 맨 위에 있는:

    ```csharp
    using System.Diagnostics;
    using MathFuncs;
    ```

3. 인스턴스를 선언 합니다 `MyMathFuncs` 맨 위에 있는 클래스는 `MainPage` 클래스:

    ```csharp
    MyMathFuncs myMathFuncs;
    ```

4. 재정의 `OnAppearing` 하 고 `OnDisappearing` 메서드에서 `ContentPage` 기본 클래스:

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

5. 업데이트를 `OnAppearing` 초기화 하는 방법의 `myMathFuncs` 변수 선언 이전에:

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();
        myMathFuncs = new MyMathFuncs();
    }
    ```

6. 업데이트를 `OnDisappearing` 메서드를 호출 합니다 `Dispose` 메서드를 `myMathFuncs`:

    ```csharp
    protected override void OnDisappearing()
    {
        base.OnAppearing();
        myMathFuncs.Dispose();
    }
    ```

7. 라는 private 메서드를 구현 **TestMathFuncs** 다음과 같습니다.

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

8. 마지막으로 호출 `TestMathFuncs` 끝에 `OnAppearing` 메서드:

    ```csharp
    TestMathFuncs();
    ```

9. 각 대상 플랫폼에서 앱을 실행 하 고 출력의 유효성을 검사 합니다 **응용 프로그램 출력** 패드 다음과 같이 나타납니다.

    ```csharp
    1 + 2 = 3
    1 - 2 = -1
    1 * 2 = 2
    1 / 2 = 0.5
    ```

    > [!NOTE]
    > 발생 하는 경우는 '*DLLNotFoundException*' 때 Android 또는 ios의 경우 빌드 오류에 대 한 테스트 해야 장치/에뮬레이터/시뮬레이터를 사용 하는 기능의 CPU 아키텍처가 하기로 결정 하는 하위 집합을 사용 하 여 호환 되는지 확인 합니다. 이 옵션을 지원 합니다. 

## <a name="summary"></a>요약

이 문서에서는 NuGet 패키지를 통해 배포 되는 일반적인.NET 래퍼를 통해 네이티브 라이브러리를 사용 하는 Xamarin.Forms 앱을 만드는 방법을 설명 합니다. 이 연습에 제공 된 예제 보다 쉽게 방법을 보여 주기 위해 의도적으로 매우 단순한입니다. 실제 응용 프로그램 예외 처리와 같은 복잡성, 콜백, 더 복잡 한 형식의 마샬링 및 다른 종속 라이브러리를 사용 하 여 링크를 사용 하 여 처리 해야 합니다. 주요 고려 사항인 경우 c + + 코드의 발전은 조정 하 고는 래퍼 및 클라이언트 응용 프로그램을 사용 하 여 동기화 프로세스 이 프로세스는 있는지 여부에 따라 이러한 문제 중 하나 또는 모두를 단일 팀의 책임 달라질 수 있습니다. 어느 방법이 든 자동화는 실제 혜택입니다. 다음은 관련 다운로드와 함께 주요 개념 중 일부를 관련 추가 정보를 제공 하는 몇 가지 리소스입니다. 

### <a name="downloads"></a>다운로드

- [NuGet 명령줄 (CLI) 도구](https://docs.microsoft.com/nuget/tools/nuget-exe-cli-reference#macoslinux)
- [Visual Studio](https://visualstudio.microsoft.com/vs)

### <a name="examples"></a>예제

- [C + +를 사용 하 여 Hyperlapse 플랫폼 간 모바일 개발](https://blogs.msdn.microsoft.com/vcblog/2015/06/26/hyperlapse-cross-platform-mobile-development-with-visual-c-and-xamarin/)
- [Microsoft Pix (c + + 및 Xamarin)](https://blog.xamarin.com/microsoft-research-ships-intelligent-apps-with-the-power-of-c-and-ai/)
- [Mono San Angeles 샘플 포트](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)

### <a name="further-reading"></a>추가 정보

[이 게시물의 내용에 관련 된 문서](https://github.com/xamarin/mobcat/tree/master/samples/cpp_with_xamarin#wrapping-up)
