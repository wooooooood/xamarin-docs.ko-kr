---
title: 플랫폼 간 앱에서의 네이티브 형식 작업
description: 이 문서에서는 Android 또는 Windows Phone Os와 같은 비 iOS 장치를 사용 하 여 코드 공유 되는 위치 하는 플랫폼 간 응용 프로그램에서 새 iOS API 네이티브 통합 형식 (nint, nuint nfloat)를 사용 하 여 설명 합니다.
ms.prod: xamarin
ms.assetid: B9C56C3B-E196-4ADA-A1DE-AC10D1001C2A
author: asb3993
ms.author: amburns
ms.date: 04/07/2016
ms.openlocfilehash: 489d2a76e6eff661360b24d1872ed1343c74b85e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61261183"
---
# <a name="working-with-native-types-in-cross-platform-apps"></a>플랫폼 간 앱에서의 네이티브 형식 작업

_이 문서에서는 Android 또는 Windows Phone Os와 같은 비 iOS 장치를 사용 하 여 코드 공유 되는 위치 하는 플랫폼 간 응용 프로그램에서 새 iOS API 네이티브 통합 형식 (nint, nuint nfloat)를 사용 하 여 설명 합니다._


IOS 및 Mac Api를 사용 하는 64 형식을 네이티브 형식입니다. 에서도 Android 또는 Windows에서 실행 되는 공유 코드를 작성 하는 경우 관리 통합 형식 공유할 수 있는 일반.NET 형식으로 변환 해야 합니다.

이 문서에서는 일반적인 공유 코드에서 통합 API를 사용 하 여 상호 운용 하는 다양 한 방법 설명.

## <a name="when-to-use-the-native-types"></a>네이티브 형식을 사용 하는 경우

Xamarin.iOS 및 Xamarin.Mac 통합 Api는 계속 포함 합니다 `int`, `uint` 및 `float` 데이터 형식 뿐만 `RectangleF`, `SizeF` 및 `PointF` 형식. 이러한 기존 데이터 형식 공유 되는 플랫폼 간 코드에서 사용할 계속 해야 합니다. 새 네이티브 데이터 형식은 여기서 아키텍처 인식 형식이 필요한 지에 대 한 지원 iOS 또는 Mac API를 호출 하는 경우에 사용 해야 합니다.

공유 되는 코드의 특성에 따라 경우도 플랫폼 간 코드를 처리 해야 할 위치를 `nint`, `nuint` 고 `nfloat` 데이터 형식입니다. 예를 들어: 이전에 사용 하는 사각형 데이터에 변환을 처리 하는 라이브러리 `System.Drawing.RectangleF` Xamarin.iOS 및 Xamarin.Android 앱 버전 간의 기능을 공유 해야 iOS의 기본 유형을 처리 하도록 업데이트 되어야 합니다.

코드를 공유 하는 형식의 다음 섹션에서 보듯이 사용 된 및 이러한 변경 내용을 처리 하는 방법을 크기와 응용 프로그램의 복잡성에 따라 다릅니다.

## <a name="code-sharing-considerations"></a>코드 공유 고려 사항

에 명시 된 대로 합니다 [코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md) 문서, 플랫폼 간 프로젝트 간에 코드를 공유 하는 방법에 두 가지가 있습니다. 프로젝트 및 이식 가능한 클래스 라이브러리를 공유 합니다. 두 형식 중 사용 된, 옵션 플랫폼 간 코드에서 네이티브 데이터 형식을 처리 하는 경우에 제한 됩니다.

### <a name="portable-class-library-projects"></a>이식 가능한 클래스 라이브러리 프로젝트

이식 가능한 클래스 라이브러리 (PCL)를 지원 하 고 인터페이스를 사용 하 여 플랫폼 특정 기능을 제공 하려는 플랫폼을 대상으로 수 있습니다.

PCL 프로젝트 형식으로 컴파일된 이후를 `.DLL` Unified API의 당면한 있기를 기존 데이터 형식을 사용 하 여 유지 해야 할 수 있습니다 (`int`를 `uint`, `float`) PCL에서 소스 코드 형식으로 캐스팅 Pcl에 대 한 호출 클래스 및 프런트 엔드 응용 프로그램의 메서드입니다. 예를 들면 다음과 같습니다.

```csharp
using NativePCL;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

### <a name="shared-projects"></a>공유 프로젝트

공유 자산 프로젝트 형식을 사용 하면 개별 플랫폼 특정 프런트 엔드 앱에 다음 포함 가져오고 컴파일되는 별도 프로젝트에서 소스 코드를 구성 하 고 사용할 `#if` 관리 하는 데 필요한 컴파일러 지시문 플랫폼 특정 요구 사항입니다.

크기와 복잡성 앞의 크기와 공유 되는 코드의 복잡성과 함께 공유 코드를 사용 하는 플랫폼 간 호환 형식을 네이티브 데이터 지원의 메서드를 선택 하는 경우 계정에 수행 해야 하는 모바일 응용 프로그램을 종료 합니다 공유 자산 프로젝트 유형입니다.

이러한 요인에 따라 다음과 같은 유형의 솔루션을 구현할 수 있습니다를 사용 하 여는 `if __UNIFIED__ ... #endif` 컴파일러 지시문을 코드를 Unified API 특정 변경을 처리할 수 있도록 합니다.

#### <a name="using-duplicate-methods"></a>중복 된 메서드를 사용 하 여

위에 지정 된 사각형 데이터 변환을 수행 하는 라이브러리를 예로 들어를 보겠습니다. 라이브러리에는 하나 또는 두 개의 간단한 방법만 포함 된, Xamarin.iOS 및 Xamarin.Android에 대 한 해당 메서드 중 중복 버전을 만들 수 있습니다. 예를 들어:

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

위의 코드에서 이후는 `CalculateArea` 루틴은 매우 간단 하며 조건부 컴파일을 사용 하 고 통합 API 버전의 메서드를 별도 생성 합니다. 반면, 라이브러리는 많은 루틴 또는 여러 복잡 한 루틴에 포함 하는 경우이 솔루션은 되지 실현 가능한 경우 모든 메서드의 수정 또는 버그 수정에 대 한 동기화를 유지 하는 문제를 나타낼.

#### <a name="using-method-overloads"></a>메서드를 사용 하 여 오버 로드

이 경우, 솔루션 있도록 이제 32 비트 데이터 형식을 사용 하 여 메서드의 오버 로드 버전을 만들어 수 있습니다 `CGRect` 매개 변수 및/또는 반환 값을 해당 값을 변환할를 `RectangleF` (알면 해당 변환에서 `nfloat` 를`float` 손실 변환이), 실제 작업을 수행 하는 루틴 원래 버전을 호출 합니다. 예를 들어:

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

마찬가지로이 적합 한 솔루션 한 정밀도 손실이 응용 프로그램의 특정 요구 사항에 대 한 결과 영향을 주지 않습니다.

#### <a name="using-alias-directives"></a>Using 별칭 지시문

정밀도의 손실 되는 문제 영역에 대 한 다른 가능한 솔루션 사용 방법은 `using` 공유 소스 코드 파일의 맨 위에 다음 코드를 포함 하 고 변환 네이티브 및 CoreGraphics 데이터 형식에 대 한 별칭을 만들려면 지시문 필요한 `int`, `uint` 또는 `float` 값을 `nint`하십시오 `nuint` 또는 `nfloat`:

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

그러면 예제 코드 있도록:

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

변경한 다음 합니다 `CalculateArea` 반환 메서드를 `nfloat` 표준 대신 `float`. 가 이렇게 하는 동안 컴파일 오류가 발생 하지 얻게 _암시적으로_ 변환의 `nfloat` 계산의 결과 (곱하는 값이 모두 있으므로 `nfloat`)에 `float` 값을 반환 합니다.

코드 컴파일 및 비 통합 API 장치에서 실행 하는 경우는 `using nfloat = global::System.Single;` 매핑합니다를 `nfloat` 에 `Single` 는 암시적으로 변환 됩니다는 `float` 프런트 엔드 응용 프로그램을 사용 하려면 허용를 `CalculateArea` 없이 메서드 수정 내용입니다.


#### <a name="using-type-conversions-in-the-front-end-app"></a>프런트 엔드 앱에서 형식 변환 사용

프런트 엔드 응용 프로그램에만 몇 가지 공유 코드 라이브러리에 대 한 호출을, 하는 경우에 다른 라이브러리 변경 되지 않은 상태로 유지 하는 것입니다 솔루션과 수행 형식 캐스팅 Xamarin.iOS 또는 Xamarin.Mac 응용 프로그램에서 기존 루틴을 호출 하는 경우. 예를 들면 다음과 같습니다.

```csharp
using NativeShared;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

응용 프로그램을 사용 하면 수백 개의 공유 코드 라이브러리를 호출 하는 경우이 다시 아닐 적합 한 솔루션입니다.

응용 프로그램의 아키텍처에 따라 수를 얻게 네이티브 데이터를 지원 하기 위해 위의 솔루션 중 하나 이상을 사용 하 여 플랫폼 간 코드에서 (필수) 되는 형식입니다.


## <a name="xamarinforms-applications"></a>Xamarin.Forms 응용 프로그램

다음 해야도 공유 하는 통합 API 응용 프로그램을 사용 하 여 플랫폼 간 Ui에 대 한 Xamarin.Forms를 사용 합니다.

- 전체 솔루션을 버전 1.3.1 사용 해야 합니다 (또는 이상)의 Xamarin.Forms NuGet 패키지.
- 모든 Xamarin.iOS 사용자 지정 렌더링에 대 한 UI 코드 공유 (공유 프로젝트 또는 PCL) 되었으면 하는 방법에 따라 위의 솔루션의 동일한 형식을 사용 합니다.

표준 플랫폼 간 응용 프로그램에서와 같이 기존 32 비트 데이터 형식은 사용할 공유 되는 플랫폼 간 코드에서 대부분의 모든 상황에 대 한 합니다. 새 네이티브 데이터 형식은 여기서 아키텍처 인식 형식이 필요한 지에 대 한 지원 iOS 또는 Mac API를 호출 하는 경우에 사용 해야 합니다.

자세한 내용은 참조 하세요. 당사의 [기존 Xamarin.Forms 앱 업데이트](https://developer.xamarin.com/guides/cross-platform/macios/updating-xamarin-forms-apps/) 설명서.

## <a name="summary"></a>요약

이 문서는 통합 API 응용 프로그램 및 해당 의미 플랫폼의 네이티브 데이터 형식을 사용 해야 하는 경우 참조를 해야 합니다. 플랫폼 간 라이브러리에서 새 기본 데이터 형식을 사용 해야 합니다는 상황에서 사용할 수 있는 몇 가지 솔루션을 설명 합니다. 및 Xamarin.Forms 플랫폼 간 응용 프로그램에서 통합 Api를 지원 하기 위한 요약 가이드가 살펴보았습니다.



## <a name="related-links"></a>관련 링크

- [Unified API](~/cross-platform/macios/unified/index.md)
- [네이티브 형식](~/cross-platform/macios/nativetypes.md)
- [코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md)
- [코드 공유 샘플](https://developer.xamarin.com/samples/mobile/SharingCode/)
