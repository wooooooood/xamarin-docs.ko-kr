---
title: 플랫폼 간 앱에서의 네이티브 형식 작업
description: 이 문서에서는 Android 또는 Windows Phone Os와 같은 비 iOS 장치와 코드를 공유 하는 플랫폼 간 응용 프로그램에서 새로운 iOS Unified API 네이티브 형식 (nint, nuint, nint)을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: B9C56C3B-E196-4ADA-A1DE-AC10D1001C2A
author: asb3993
ms.author: amburns
ms.date: 04/07/2016
ms.openlocfilehash: deb4caa4d23d23b2997361cca161b218c1ff7b61
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511297"
---
# <a name="working-with-native-types-in-cross-platform-apps"></a>플랫폼 간 앱에서의 네이티브 형식 작업

_이 문서에서는 Android 또는 Windows Phone Os와 같은 비 iOS 장치와 코드를 공유 하는 플랫폼 간 응용 프로그램에서 새로운 iOS Unified API 네이티브 형식 (nint, nuint, nint)을 사용 하는 방법을 설명 합니다._


64 형식의 네이티브 형식은 iOS 및 Mac Api에서 작동 합니다. Android 또는 Windows에서 실행 되는 공유 코드를 작성 하는 경우에는 공유할 수 있는 일반 .NET 형식으로 통합 형식을 변환 해야 합니다.

이 문서에서는 공유/공통 코드에서 Unified API와 상호 작용 하는 다양 한 방법을 설명 합니다.

## <a name="when-to-use-the-native-types"></a>네이티브 형식을 사용 하는 경우

Xamarin.ios 및 xamarin.ios 통합 api에는, `int`및 `PointF` 형식 뿐만 아니라 `float` , `uint` 및 `SizeF` 데이터 형식도 `RectangleF`포함 되어 있습니다. 이러한 기존 데이터 형식은 공유 플랫폼 간 코드에서 계속 사용 해야 합니다. 아키텍처 인식 형식에 대 한 지원이 필요한 Mac 또는 iOS API를 호출할 때에만 새 네이티브 데이터 형식을 사용 해야 합니다.

공유 되는 코드의 특성에 따라 플랫폼 간 코드가 `nint`, `nuint` 및 `nfloat` 데이터 형식을 처리 해야 하는 경우가 있을 수 있습니다. 예: 이전에 xamarin.ios와 xamarin Android 버전의 앱 간 기능을 공유 `System.Drawing.RectangleF` 하는 데 사용 했던 사각형 데이터의 변환을 처리 하는 라이브러리는 iOS에서 네이티브 형식을 처리 하도록 업데이트 해야 합니다.

이러한 변경을 처리 하는 방법은 다음 섹션에서 볼 수 있듯이 응용 프로그램의 크기와 복잡성과 사용 된 코드 공유의 형식에 따라 달라 집니다.

## <a name="code-sharing-considerations"></a>코드 공유 고려 사항

[코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md) 문서에 나와 있는 것 처럼 플랫폼 간 프로젝트 간에 코드를 공유 하는 두 가지 주요 방법이 있습니다. 공유 프로젝트 및 이식 가능한 클래스 라이브러리. 두 가지 형식이 사용 된 경우 플랫폼 간 코드에서 네이티브 데이터 형식을 처리할 때 사용 하는 옵션을 제한 합니다.

### <a name="portable-class-library-projects"></a>이식 가능한 클래스 라이브러리 프로젝트

PCL (이식 가능한 클래스 라이브러리)을 사용 하면 지원 하려는 플랫폼을 대상으로 지정 하 고 인터페이스를 사용 하 여 플랫폼별 기능을 제공할 수 있습니다.

`.DLL` Pcl 프로젝트 형식이로 컴파일되고 Unified API 의미가 없으므로 pcl 소스 코드에서 기존 데이터 형식 (`int`, `uint`, `float`)을 계속 사용 하 고, pcl에 대 한 호출을 형식으로 캐스팅 해야 합니다. 프런트 엔드 응용 프로그램의 클래스 및 메서드. 예:

```csharp
using NativePCL;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

### <a name="shared-projects"></a>공유 프로젝트

공유 자산 프로젝트 형식을 사용 하면 별도의 프로젝트에서 소스 코드를 구성 하 고, 개별 플랫폼별 프런트 엔드 앱에 포함 하 고 컴파일하고, 관리 하는 데 필요한 대로 `#if` 컴파일러 지시문을 사용할 수 있습니다. 플랫폼별 요구 사항.

공유 되는 코드의 크기 및 복잡성과 함께 공유 코드를 사용 하는 프런트 엔드 모바일 응용 프로그램의 크기와 복잡성은 플랫폼 간 네이티브 데이터 형식에 대 한 지원 방법을 선택할 때 고려해 야 합니다. 공유 자산 프로젝트.

이러한 요소에 따라 다음과 같은 유형의 솔루션을 `if __UNIFIED__ ... #endif` 컴파일러 지시문을 사용 하 여 구현 하 여 코드에 대 한 Unified API 특정 변경 내용을 처리할 수 있습니다.

#### <a name="using-duplicate-methods"></a>중복 메서드 사용

위에 지정 된 사각형 데이터에서 변환을 수행 하는 라이브러리의 예를 들어 보겠습니다. 라이브러리에 매우 간단한 메서드를 하나 또는 두 개만 포함 하는 경우 Xamarin.ios 및 Xamarin.ios에 대 한 해당 메서드의 중복 버전을 만들도록 선택할 수 있습니다. 예를 들어:

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

위의 코드에서 `CalculateArea` 루틴은 매우 간단 하므로 조건부 컴파일을 사용 하 고 메서드의 별도 Unified API 버전을 만들었습니다. 반면, 라이브러리에 여러 루틴 또는 복잡 한 루틴이 포함 된 경우이 솔루션은 수정 또는 버그 수정을 위해 모든 메서드를 동기화 상태로 유지 하는 문제를 제공 하기 때문에 불가능 합니다.

#### <a name="using-method-overloads"></a>메서드 오버 로드 사용

이 경우 솔루션은 32 비트 데이터 형식을 `CGRect` 사용 하 여 메서드의 오버 로드 버전을 만들어 매개 변수 및/또는 반환 값으로 사용 하도록 할 수 있습니다 .이 값은 `RectangleF` 에서 `nfloat` 로 변환 하는 것을 알고 있습니다. `float` 는 손실 변환) 이며 원래 버전의 루틴을 호출 하 여 실제 작업을 수행 합니다. 예를 들어:

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

이는 정밀도 손실이 응용 프로그램의 특정 요구에 대 한 결과에 영향을 주지 않는 한 좋은 해결책입니다.

#### <a name="using-alias-directives"></a>별칭 지시문 사용

정밀도 손실이 문제가 되는 영역에 대 한 가능한 또 다른 해결 방법은 지시문을 사용 `using` 하 여 공유 소스 코드 파일의 맨 위에 다음 코드를 포함 하 고를 변환 하 여 네이티브 및 CoreGraphics 데이터 형식에 대 한 별칭을 만드는 것입니다. `int` `nint`필요하거나 또는에대`nuint` 한 값입니다 .`nfloat` `uint` `float`

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

그러면 예제 코드가 다음과 같이 됩니다.

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
    // Map Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Map Unified types to MonoTouch types
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

여기서는 표준 `CalculateArea` `nfloat` 대신를반환하도록메서드를변경했습니다`float`. 이렇게 하면 계산 결과를 `nfloat` _암시적_ 으로 변환 하는 컴파일 오류가 발생 하지 않습니다. (두 값이 모두 `float` 형식이 `nfloat`기 때문에) 반환 값으로 변환 됩니다.

코드가 컴파일되고 Unified API 되지 않은 장치에서 실행 되는 경우은 `using nfloat = global::System.Single;` 를 사용 하는 프런트 엔드 응용 프로그램이를 사용 하지 않고 `float` `CalculateArea` 메서드를 호출할 수 있도록 암시적으로 변환 되는에 `nfloat` 매핑합니다 `Single` . 수정과.


#### <a name="using-type-conversions-in-the-front-end-app"></a>프런트 엔드 앱에서 형식 변환 사용

프런트 엔드 응용 프로그램에서 공유 코드 라이브러리에 대해 몇 개의 호출만 수행 하는 경우 다른 해결 방법은 기존 루틴을 호출할 때 라이브러리를 변경 하지 않고, Xamarin.ios 또는 Xamarin.ios 응용 프로그램에서 형식 캐스팅을 수행 하는 것입니다. 예를 들어:

```csharp
using NativeShared;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

소비 하는 응용 프로그램에서 공유 코드 라이브러리를 수백 번 호출 하는 경우에는이 방법이 좋은 솔루션이 아닐 수 있습니다.

응용 프로그램의 아키텍처에 따라 위의 솔루션 중 하나 이상을 사용 하 여 플랫폼 간 코드에서 네이티브 데이터 형식 (필요한 경우)을 지원할 수 있습니다.


## <a name="xamarinforms-applications"></a>Xamarin Forms 응용 프로그램

다음은 Unified API 응용 프로그램과 공유 되는 플랫폼 간 Ui에 대해 Xamarin.ios를 사용 하는 데 필요 합니다.

- 전체 솔루션은 Xamarin.ios NuGet 패키지 버전을 1.3.1 이상 사용 해야 합니다.
- Xamarin.ios 사용자 지정 렌더링의 경우 UI 코드가 공유 된 방법 (공유 프로젝트 또는 PCL)에 따라 위에 표시 된 것과 동일한 유형의 솔루션을 사용 합니다.

표준 플랫폼 간 응용 프로그램과 마찬가지로, 대부분의 경우 모든 공유 플랫폼 간 코드에서 기존 32 비트 데이터 형식을 사용 해야 합니다. 아키텍처 인식 형식에 대 한 지원이 필요한 Mac 또는 iOS API를 호출할 때에만 새 네이티브 데이터 형식을 사용 해야 합니다.

자세한 내용은 [기존 Xamarin.ios 앱 업데이트](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md) 설명서를 참조 하세요.

## <a name="summary"></a>요약

이 문서에서는 Unified API 응용 프로그램에서 네이티브 데이터 형식을 사용 하는 시기와 플랫폼 간 영향을 살펴보았습니다. 플랫폼 간 라이브러리에서 새 네이티브 데이터 형식을 사용 해야 하는 경우에 사용할 수 있는 여러 가지 솔루션을 제공 합니다. 또한 Xamarin. Forms 플랫폼 간 응용 프로그램에서 통합 Api를 지 원하는 간단한 가이드를 살펴보았습니다.



## <a name="related-links"></a>관련 링크

- [Unified API](~/cross-platform/macios/unified/index.md)
- [네이티브 형식](~/cross-platform/macios/nativetypes.md)
- [코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md)
- [코드 공유 샘플](https://developer.xamarin.com/samples/mobile/SharingCode/)
