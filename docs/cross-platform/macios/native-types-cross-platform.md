---
title: 플랫폼 간 앱에서 네이티브 형식 사용
description: 이 문서에서는 Android 또는 Windows Phone Os와 같은 비 iOS 장치 코드는 공유 하는 하는 플랫폼 간 응용 프로그램에서 새 iOS API 네이티브 통합 유형 (nint, nuint, nfloat)를 사용 하 여 설명 합니다.
ms.prod: xamarin
ms.assetid: B9C56C3B-E196-4ADA-A1DE-AC10D1001C2A
author: asb3993
ms.author: amburns
ms.date: 04/07/2016
ms.openlocfilehash: eb6979691eeef6dd7436d74fdfd501c747d9b3c6
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33918159"
---
# <a name="working-with-native-types-in-cross-platform-apps"></a>플랫폼 간 앱에서 네이티브 형식 사용

_이 문서에서는 Android 또는 Windows Phone Os와 같은 비 iOS 장치 코드는 공유 하는 하는 플랫폼 간 응용 프로그램에서 새 iOS API 네이티브 통합 유형 (nint, nuint, nfloat)를 사용 하 여 설명 합니다._


IOS 및 Mac Api를 사용 하는 64 형식 네이티브 형식입니다. 도 Android 또는 Windows에서 실행 되는 공유 코드를 작성 하는 경우 관리 통합 형식 공유할 수 있는.NET 형식의 일반으로 변환 해야 합니다.

이 문서에서는 공유/공통 코드에서 통합 API와 상호 운용 하도록 다양 한 방법에 설명 합니다.

## <a name="when-to-use-the-native-types"></a>네이티브 형식을 사용 하는 경우

Xamarin.iOS 및 Xamarin.Mac 통합 Api 여전히 포함 되어는 `int`, `uint` 및 `float` 데이터 형식으로 `RectangleF`, `SizeF` 및 `PointF` 형식입니다. 이러한 기존 데이터 형식은 모든 공유, 플랫폼 간 코드에서 사용할 계속 되어야 합니다. 새 네이티브 데이터 형식이 있는 아키텍처 인식 형식이 필요한 지에 대 한 지원 iOS 또는 Mac API에 대 한 호출을 만들 때에 사용 해야 합니다.

공유 되는 코드의 특성에 따라 경우도 플랫폼 간 코드를 다루는 데 해야는 `nint`, `nuint` 및 `nfloat` 데이터 형식입니다. 예를 들어: 이전에 사용 하는 사각형 데이터에 변환을 처리 하는 라이브러리 `System.Drawing.RectangleF` Xamarin.iOS 및 Xamarin.Android 응용 프로그램 버전 간의 기능을 공유 하는 업데이트 해야 할 iOS에서 네이티브 유형을 처리 하도록 합니다.

코드 공유를 형식으로 모두 다음 섹션에 나와 있듯이 사용 된 및 이러한 변경 내용을 처리 하는 방법을 크기와 응용 프로그램의 복잡성에 따라 다릅니다.

## <a name="code-sharing-considerations"></a>코드 공유 고려 사항

설명한 것 처럼는 [코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md) 문서, 두 가지 주요 플랫폼 간 프로젝트 간에 코드 공유를: 공유 프로젝트 및 이식 가능한 클래스 라이브러리입니다. 두 형식 중 사용 된, 플랫폼 간 코드에서 네이티브 데이터 형식을 처리할 때 가능한 옵션을 제한 합니다.

### <a name="portable-class-library-projects"></a>이식 가능한 클래스 라이브러리 프로젝트

이식 가능한 클래스 라이브러리 (PCL)를 지원 하 고 플랫폼 특정 기능을 제공 하도록 인터페이스를 사용 하려면 플랫폼을 대상으로 수 있습니다.

PCL 프로젝트 형식으로 컴파일된 이후는 `.DLL` 및 통합 API 필요도 있기, 기존 데이터 형식을 사용 하 여 유지 해야 할 수 있습니다 (`int`, `uint`, `float`) PCL에 유형과 소스 코드는 PCLs에 대 한 호출 캐스팅 됩니다. 클래스와 프런트 엔드 응용 프로그램의 메서드를 제공 합니다. 예를 들면 다음과 같습니다.

```csharp
using NativePCL;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

### <a name="shared-projects"></a>공유 프로젝트

공유 자산 프로젝트 형식에서는 개별 플랫폼별 프런트 엔드 응용 프로그램에 다음 포함 가져오고 컴파일된 하는 별도 프로젝트의 소스 코드를 구성 하 고 사용할 수 있습니다 `#if` 컴파일러 지시문을 관리 하는 데 필요 하므로 플랫폼 특정 요구 사항입니다.

크기와 복잡성 앞의 크기와 공유 되는 코드의 복잡성과 함께 공유 코드를 사용 하는, 플랫폼 간 호환 형식을 지원 네이티브 데이터의 방법을 선택할 때 고려해 야 하는 모바일 응용 프로그램을 끝내는 공유 자산 프로젝트 유형입니다.

이러한 요소에 따라 다음과 같은 유형의 솔루션 구현 하 고 사용 하 여는 `if __UNIFIED__ ... #endif` 컴파일러 지시문을 코드에는 통합 API 특정 변경 내용을 처리 하도록 합니다.

#### <a name="using-duplicate-methods"></a>중복 메서드를 사용 하 여

위의 사각형 데이터에 변환을 수행 하는 작업 하는 라이브러리를 예로 들어를 보겠습니다. 라이브러리에는 하나 또는 두 개의 매우 간단한 방법만 포함 된, Xamarin.iOS 및 Xamarin.Android에 대 한 해당 방법의 중복 버전을 만들 수 있습니다. 예를 들어:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
using CoreGraphics;
#endif

namespace NativeShared
{
    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        #if __UNIFIED__
            public static nfloat CalculateArea(CGRect rect) {

                // Calculate area...
                return (rect.Width * rect.Height);

            }
        #else
            public static float CalculateArea(RectangleF rect) {

                // Calculate area...
                return (rect.Width * rect.Height);

            }
        #endif
        #endregion
    }
}
```

위의 코드에서 이후는 `CalculateArea` 루틴은 매우 간단 하며 조건부 컴파일을 사용 했으며, 개별 통합 API 버전의 메서드를 생성 합니다. 반면에 라이브러리는 많은 루틴 또는 여러 개의 복잡 한 루틴에 포함 된 경우이 솔루션은 가능 하지, 모든 메서드 수정 또는 버그 수정에 대 한 동기화를 유지 하는 문제를 나타낼 때.

#### <a name="using-method-overloads"></a>메서드를 사용 하는 오버 로드

지금 가지도록 32 비트 데이터 형식을 사용 하 여 메서드의 오버 로드 버전을 만들어 솔루션 수 경우 `CGRect` 매개 변수 또는 반환 값으로 해당 값을 변환할는 `RectangleF` (에서 해당 변환 알고 있으면 `nfloat` 를`float` 손실 변환이), 원래 버전 실제 작업을 수행 하는 루틴의 호출 합니다. 예를 들어:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
using CoreGraphics;
#endif

namespace NativeShared
{
    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        #if __UNIFIED__
            public static nfloat CalculateArea(CGRect rect) {

            // Call original routine to calculate area
            return (nfloat)CalculateArea((RectangleF)rect);

            }
        #endif

        public static float CalculateArea(RectangleF rect) {

            // Calculate area...
            return (rect.Width * rect.Height);

        }

        #endregion
    }
}

```

이 역시 적합 한 솔루션으로 정밀도의 손실에는 응용 프로그램의 특정 요구에 대 한 결과 영향을 주지 않습니다.

#### <a name="using-alias-directives"></a>Using 별칭 지시문

정밀도의 손실 인 문제 영역에 대 한 또 다른 가능한 방법은 사용 하는 `using` 공유 소스 코드 파일의 맨 위에 다음 코드를 포함 하 고 하나를 변환 하 여 기본 모드와 CoreGraphics 데이터 형식에 대 한 별칭을 만들려면 지시문 필요한 `int`, `uint` 또는 `float` 값을 `nint`, `nuint` 또는 `nfloat`:

```csharp
#if __UNIFIED__
    // Mappings Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Mappings Unified types to MonoTouch types
    using nfloat = global::System.Single;
    using nint = global::System.Int32;
    using nuint = global::System.UInt32;
#endif
```

이 예제 코드에서는 다음 되 되도록:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
    // Mappings Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Mappings Unified types to MonoTouch types
    using nfloat = global::System.Single;
    using nint = global::System.Int32;
    using nuint = global::System.UInt32;
#endif

namespace NativeShared
{

    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        public static nfloat CalculateArea(RectangleF rect) {

            // Calculate area...
            return (rect.Width * rect.Height);

        }
        #endregion
    }
}
```

변경한 여기는 `CalculateArea` 메서드가 반환 된 `nfloat` 표준 대신 `float`합니다. 에서는 컴파일 오류가 발생 하는 동안 발생 하지 않습니다 있도록 이렇게 _암시적으로_ 변환는 `nfloat` 우리는 계산의 결과 (곱한 되 고 두 값이 이므로 `nfloat`)에 `float` 값을 반환 합니다.

코드를 컴파일하여 아닌 통합 API 장치에서 실행 하는 경우는 `using nfloat = global::System.Single;` 매핑하는 `nfloat` 에 `Single` 는 암시적으로 변환 됩니다는 `float` 프런트 엔드 응용 프로그램을 사용 하려면 호출을 허용는 `CalculateArea` 없이 메서드 수정 내용입니다.


#### <a name="using-type-conversions-in-the-front-end-app"></a>형식 변환을 사용 하 여 프런트 엔드 응용 프로그램에서

프런트 엔드 응용 프로그램에만 공유 코드 라이브러리를 호출 하는 소수의, 다른 솔루션 라이브러리 변경 되지 않은 상태로 유지 하는 것입니다 하 고 기존 루틴을 호출할 때 Xamarin.iOS 또는 Xamarin.Mac 응용 프로그램에서 캐스팅을 입력 수행 합니다. 예를 들면 다음과 같습니다.

```csharp
using NativeShared;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

응용 프로그램을 사용 하면 수백 개의 공유 코드 라이브러리를 호출 하는 경우이 다시 아닐 적합 한 솔루션.

이제 응용 프로그램의 아키텍처에 따라, 우리 생길 수 원시 데이터를 지원 하기 위해 위의 솔루션 중 하나 이상을 사용 하 여 플랫폼 간 코드에서 (필수) 되는 형식입니다.


## <a name="xamarinforms-applications"></a>Xamarin.Forms 응용 프로그램

다음은 또한 공유 하는 통합 API 응용 프로그램으로 플랫폼 간 Ui에 대 한 Xamarin.Forms를 사용 하도록 필요 합니다.

- 전체 솔루션을 버전 1.3.1 사용 해야 합니다 (또는 이상)의 Xamarin.Forms NuGet 패키지 합니다.
- Xamarin.iOS 사용자 지정 렌더링에 대 한 UI 코드 공유 (공유 프로젝트 또는 PCL) 된 방식에 따라 위의 솔루션의 동일한 형식을 사용 합니다.

표준 플랫폼 간 응용 프로그램에서와 같이 기존 32 비트 데이터 형식은 사용할지 모든 공유, 플랫폼 간 코드에서 대부분 모든 상황에 맞는 합니다. 새 네이티브 데이터 형식이 있는 아키텍처 인식 형식이 필요한 지에 대 한 지원 iOS 또는 Mac API에 대 한 호출을 만들 때에 사용 해야 합니다.

자세한 내용은 참조 하십시오. 우리의 [기존 Xamarin.Forms 앱 업데이트](http://developer.xamarin.com/guides/cross-platform/macios/updating-xamarin-forms-apps/) 설명서입니다.

## <a name="summary"></a>요약

이 문서에는 통합 API 응용 프로그램 및 해당 의미 플랫폼에 네이티브 데이터 형식을 사용 해야 하는 경우 참조는 합니다. 플랫폼 간 라이브러리에는 새로운 네이티브 데이터 형식에 사용할 수 해야 경우에 사용할 수 있는 여러 가지 솔루션을 제시한 것입니다. 고 Xamarin.Forms는 플랫폼 간 응용 프로그램에서 통합 된 Api를 지원 하기 위한 요약 가이드가 살펴보았습니다.



## <a name="related-links"></a>관련 링크

- [Unified API](~/cross-platform/macios/unified/index.md)
- [네이티브 형식](~/cross-platform/macios/nativetypes.md)
- [코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md)
- [코드 공유 샘플](https://developer.xamarin.com/samples/mobile/SharingCode/)
