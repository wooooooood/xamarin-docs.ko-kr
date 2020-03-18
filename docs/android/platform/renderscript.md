---
title: Renderscript 소개
description: 이 가이드에서는 Renderscript를 소개하고 API 레벨 17 이상을 대상으로 하는 Xamarin.Android 애플리케이션에서 내장 Renderscript API를 사용하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 884b69b0cdecf4f979cec314b6440974c5bac97d
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73019804"
---
# <a name="an-introduction-to-renderscript"></a>Renderscript 소개

이 가이드에서는 Renderscript를 소개하고 API 레벨 17 이상을 대상으로 하는 Xamarin.Android 애플리케이션에서 내장 Renderscript API를 사용하는 방법을 설명합니다. 

## <a name="overview"></a>개요

Renderscript는 많은 양의 컴퓨팅 리소스가 필요한 Android 애플리케이션의 성능을 개선하기 위해 Google에서 만든 프로그래밍 프레임워크입니다. Renderscript는 [C99](https://en.wikipedia.org/wiki/C99)를 기반으로 하는 로우 레벨 고성능 API입니다. Renderscript는 CPU, GPU 또는 DSP에서 실행되는 로우 레벨 API이므로 다음과 같은 작업을 수행해야 하는 Android 앱에 적합합니다.

- 그래픽
- 이미지 처리
- 암호화
- 신호 처리
- 수학 루틴

Renderscript는 `clang` 을 사용하여 스크립트를 APK에 번들된 LLVM 바이트 코드로 컴파일합니다. 앱을 처음으로 실행하면 LLVM 바이트 코드가 디바이스상의 프로세서에 맞게 기계 코드로 컴파일됩니다. 이 아키텍처는 개발자가 디바이스상의 각 프로세서에 맞게 직접 기계 코드를 작성하지 않아도 Android 애플리케이션이 기계 코드의 장점을 활용할 수 있도록 지원합니다.

Renderscript 루틴은 다음과 같은 두 가지 구성 요소로 이루어집니다.

1. **Renderscript 런타임** &ndash; Renderscript 실행을 담당하는 네이티브 API입니다. 여기에는 해당 애플리케이션을 위해 작성된 모든 Renderscript가 포함됩니다.

2. **Android 프레임워크의 관리형 래퍼** &ndash; Android 앱이 Renderscript 런타임 및 스크립트를 제어하고 상호 작용할 수 있도록 지원하는 관리형 클래스입니다. Android 도구 체인은 Renderscript 런타임의 제어를 위해 프레임워크에서 제공하는 클래스 외에도 Renderscript 소스 코드를 검사하여 Android 애플리케이션에서 사용할 관리형 래퍼 클래스를 생성합니다.

다음 다이어그램에서는 해당 구성 요소가 어떤 관계를 갖는지 보여 줍니다.

![Android 프레임워크가 Renderscript 런타임과 상호 작용하는 방식을 보여 주는 다이어그램](renderscript-images/renderscript-01.png)

Android 애플리케이션에서 Renderscript를 사용할 때 고려해야 하는 중요한 개념 3가지가 있습니다.

1. **컨텍스트** &ndash; Android SDK에서 제공하며 Renderscript에 리소스를 할당하고 Android 앱이 Renderscript와 데이터를 주고받을 수 있도록 허용하는 관리형 API입니다.

2. **컴퓨팅 커널** &ndash; 작업을 처리하는 루틴으로, ‘루트 커널’ 또는 ‘커널’이라고도 합니다.    커널은 C 함수와 매우 비슷합니다. 커널은 할당된 메모리에 있는 모든 데이터에 대해 실행되는 병렬 처리 가능한 루틴입니다.

3. **할당된 메모리** &ndash; 데이터는 _[Allocation](xref:Android.Renderscripts.Allocation)_ 을 통해 커널로/커널에서 전달됩니다. 커널에는 하나의 입력 Allocation 및/또는 하나의 출력 Allocation이 있습니다.

[Android.Renderscripts](xref:Android.Renderscripts) 네임 스페이스는 Renderscript 런타임과 상호 작용하기 위한 클래스를 포함합니다. 구체적으로, [`Renderscript`](xref:Android.Renderscripts.RenderScript) 클래스는 Renderscript 엔진의 수명 주기와 리소스를 관리합니다. Android 앱은 하나 이상의 [`Android.Renderscripts.Allocation`](xref:Android.Renderscripts.Allocation) 개체를 초기화해야 합니다.
개체여야 합니다. Allocation은 Android 앱과 Renderscript 런타임 사이에 공유되는 메모리의 할당 및 액세스를 담당하는 관리형 API입니다. 일반적으로, 입력에 대해 하나의 Allocation이 있고, 선택적으로 커널의 출력을 저장하기 위한 또 하나의 Allocation이 만들어집니다. Renderscript 런타임 엔진 및 관련 관리형 래퍼 클래스가 Allocation에 의해 할당되는 메모리에 대한 액세스를 관리하므로 Android 앱 개발자가 추가 작업을 수행할 필요가 없습니다.

하나의 Allocation은 하나 이상의 [Android.Renderscripts.Element](xref:Android.Renderscripts.Element)를 포함합니다.
Element는 각 Allocation에 있는 데이터를 설명하는 특수한 형식입니다.
출력 Allocation의 Element 형식은 입력 Element의 형식과 일치해야 합니다. Renderscript가 실행되면 입력 Allocation에 있는 각 Element에 대해 병렬로 반복되며 결과를 출력 Allocation에 씁니다. Element에는 다음과 같은 두 가지 형식이 있습니다.

- **단순 형식** &ndash; C 데이터 형식 `float` 또는 `char`과 개념적으로 동일합니다.

- **복합 형식** &ndash; C `struct`와 비슷합니다.

Renderscript 엔진은 각 Allocation의 Element가 커널에서 요구되는 바와 호환되는지 확인하는 런타임 검사를 수행합니다. Allocation에 있는 Element의 데이터 형식이 커널에 필요한 데이터 형식과 일치하지 않으면 예외가 throw됩니다.

모든 Renderscript 커널은 [`Android.Renderscripts.Script`](xref:Android.Renderscripts.Script)
클래스의 하위 항목인 형식으로 래핑됩니다. `Script` 클래스는 Renderscript의 매개 변수를 설정하고, 적절한 `Allocations`를 설정하고, Renderscript를 실행하는 데 사용됩니다. Android SDK에는 다음과 같은 두 개의 `Script` 서브클래스가 있습니다.

- **`Android.Renderscripts.ScriptIntrinsic`** &ndash; 보다 일반적으로 사용되는 Renderscript 작업은 Android SDK로 번들되어 있고 [ScriptIntrinsic](xref:Android.Renderscripts.ScriptIntrinsic) 클래스의 서브클래스를 사용하여 액세스할 수 있습니다. 이는 이미 제공되어 있으므로 개발자가 애플리케이션에서 이 스크립트를 사용하도록 추가 단계를 수행할 필요가 없습니다.

- **`ScriptC_XXXXX`** &ndash; _사용자 스크립트_라고도 하는 이 스크립트는 개발자가 작성하여 APK로 패키징합니다. 컴파일 시간에, Android 도구 체인이 Android 앱에서 해당 스크립트가 사용되도록 허용하는 관리형 래퍼 클래스를 생성합니다.
  이렇게 생성되는 클래스의 이름은 앞에 `ScriptC_`가 붙은 Renderscript 파일 이름입니다. 사용자 스크립트를 작성하고 통합하는 것은 Xamarin.Android에서 공식적으로 지원되지 않으며, 이 가이드의 범위를 벗어납니다.

위의 두 가지 형식 중에 `StringIntrinsic`만 Xamarin.Android에서 지원됩니다. 이 가이드에서는 Xamarin.Android 애플리케이션에서 내장 스크립트를 사용하는 방법을 설명합니다.

## <a name="requirements"></a>요구 사항

이 가이드는 API 레벨 17 이상을 대상으로 하는 Xamarin.Android 애플리케이션용입니다. 이 가이드에서는 _사용자 스크립트_의 사용법을 다루지 않습니다.

[Xamarin.Android V8 지원 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/)는 이전 버전의 Android SDK를 대상으로 하는 앱을 위한 내장 Renderscript API를 지원합니다. 이 패키지를 Xamarin.Android 프로젝트에 추가하면 이전 버전의 Android SDK를 대상으로 하는 앱에서 내장 스크립트를 사용하도록 허용할 수 있습니다.

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>Xamarin.Android의 내장 Renderscript 사용

내장 스크립트는 추가 코드를 최소한으로 작성하면서 계산 집약적인 작업을 수행하는 좋은 방법입니다. 내장 스크립트는 방대한 디바이스 교집합에서 최적의 성능을 제공하도록 세밀하게 조정되었습니다.
내장 스크립트는 보통 관리 코드보다 10배 빠르게 실행되며 사용자 지정 C 구현보다 2~3배 빠르게 실행됩니다. 내장 스크립트는 대부분의 일반적인 처리 시나리오에 적용됩니다. 다음과 같은 내장 스크립트 목록은 Xamarin.Android에 포함된 현재 스크립트를 설명합니다.

- [ScriptIntrinsic3DLUT](xref:Android.Renderscripts.ScriptIntrinsic3DLUT) &ndash; 3D 조회 테이블을 사용하여 RGB를 RGBA로 변환합니다. 

- [ScriptIntrinsicBLAS](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html) &ndash; [BLAS](http://www.netlib.org/blas/)에 고성능 Renderscript API를 제공합니다. BLAS(Basic Linear Algebra Subprograms)는 기본적인 벡터 및 행렬 연산을 수행하기 위한 표준 구성 요소를 제공하는 루틴입니다. 

- [ScriptIntrinsicBlend](xref:Android.Renderscripts.ScriptIntrinsicBlend) &ndash; 2개의 Allocation을 하나로 혼합합니다.

- [ScriptIntrinsicBlur](xref:Android.Renderscripts.ScriptIntrinsicBlur) &ndash; Allocation에 가우스 흐림을 적용합니다.

- [ScriptIntrinsicColorMatrix](xref:Android.Renderscripts.ScriptIntrinsicColorMatrix) &ndash; Allocation에 색 행렬을 적용(색 변경, 채도 조정)합니다.

- [ScriptIntrinsicConvolve3x3](xref:Android.Renderscripts.ScriptIntrinsicConvolve3x3) &ndash; Allocation에 3x3 색 행렬을 적용합니다.

- [ScriptIntrinsicConvolve5x5](xref:Android.Renderscripts.ScriptIntrinsicConvolve5x5) &ndash; Allocation에 5x5 색 행렬을 적용합니다.

- [ScriptIntrinsicHistogram](xref:Android.Renderscripts.ScriptIntrinsicHistogram) &ndash; 내장 히스토그램 필터입니다.

- [ScriptIntrinsicLUT](xref:Android.Renderscripts.ScriptIntrinsicLUT) &ndash; 버퍼에 채널당 조회 테이블을 적용합니다.

- [ScriptIntrinsicResize](xref:Android.Renderscripts.ScriptIntrinsicResize) &ndash; 2D 할당의 크기 조정을 수행하는 스크립트입니다.

- [ScriptIntrinsicYuvToRGB](xref:Android.Renderscripts.ScriptIntrinsicYuvToRGB) &ndash; YUV 버퍼를 RGB로 변환합니다.

각 내장 스크립트의 세부 정보는 API 설명서를 참조하세요.

아래에서는 Android 애플리케이션에서 Renderscript를 사용하기 위한 기본적인 방법을 설명합니다.

**Renderscript 컨텍스트 만들기** &ndash; [`Renderscript`](xref:Android.Renderscripts.RenderScript)
클래스는 Renderscript 컨텍스트를 둘러싸는 관리형 래퍼로, 초기화, 리소스 관리 및 정리를 제어합니다. Renderscript 개체는 Android 컨텍스트(예: 활동)를 매개 변수로 간주하는 `RenderScript.Create` 팩터리 메서드를 사용하여 만들어집니다. 다음 코드 줄에서는 Renderscript 컨텍스트를 초기화하는 방법을 보여 줍니다.

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

**Allocation 만들기** &ndash; 내장 스크립트에 따라 하나 또는 두 개의 `Allocation`을 만들어야 할 수 있습니다. [`Android.Renderscripts.Allocation`](xref:Android.Renderscripts.Allocation)
클래스에는 내장 스크립트의 할당을 인스턴스화하는 데 도움이 되는 몇 가지 팩터리 메서드가 있습니다. 일례로, 다음 코드 조각에서는 비트맵을 위한 Allocation을 만드는 방법을 보여 줍니다.

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

종종 스크립트의 출력 데이터를 저장하기 위해 `Allocation`을 만들어야 할 경우가 생깁니다. 다음 조각에서는 `Allocation.CreateTyped` 도우미를 사용하여 원본과 동일한 형식을 갖는 두 번째 `Allocation`을 인스턴스화하는 방법을 보여 줍니다.

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

**스크립트 래퍼 인스턴스화** &ndash; 각 내장 스크립트 래퍼 클래스는 해당 스크립트에 대해 래퍼 개체를 인스턴스화하기 위한 도움이 메서드(`Create`)를 가져야 합니다. 다음 코드 조각에서는 `ScriptIntrinsicBlur` blur 개체를 인스턴스화하는 예를 보여 줍니다. `Element.U8_4` 도우미 메서드는 `Bitmap` 개체의 데이터를 저장하기에 적합한 부호 없는 8비트 정수로 구성된 4개 필드인 데이터 형식을 설명하는 Element를 만듭니다.

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

**Allocation 할당, 매개 변수 설정 및 스크립트 실행** &ndash; `Script` 클래스는 Renderscript를 실제로 실행하는 `ForEach` 메서드를 제공합니다. 이 메서드는 입력 데이터를 저장하는 `Allocation`의 각 `Element`에 대해 반복됩니다. 경우에 따라 출력을 저장하는 `Allocation`을 제공해야 할 수도 있습니다.
`ForEach`는 출력 Allocation의 콘텐츠를 덮어씁니다. 이전 단계에 나온 코드 조각에 이어, 이 예제에서는 입력 Allocation을 할당하고, 매개 변수를 설정하고, 마지막으로 스크립트를 실행(결과를 출력 Allocation에 복사)하는 방법을 보여 줍니다.

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

Xamarin.Android에서 내장 스크립트를 사용하는 방법을 종합적으로 보여 주는 [Blur an Image with Renderscript](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript)(Renderscript를 사용하여 이미지 흐리게 만들기) 레시피도 참조하세요.

## <a name="summary"></a>요약

이 가이드에서는 Renderscript를 소개하고 Xamarin.Android 애플리케이션에서 Renderscript를 사용하는 방법을 설명했습니다. Renderscript의 정의와 Android 애플리케이션에서 Renderscript가 작동하는 방식을 간략히 살펴보고, Renderscript의 몇 가지 주요 구성 요소에 대해 설명하고, _사용자 스크립트_와 _내장 스크립트_의 차이점을 알아보았습니다. 마지막으로, Xamarin.Android 애플리케이션에서 내장 스크립트를 사용하는 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [Android.Renderscripts namespace](xref:Android.Renderscripts)(Android.Renderscripts 네임스페이스)
- [Blur an Image with Renderscript](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript)(Renderscript를 사용하여 이미지 흐리게 만들기)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [자습서: Getting Started with Renderscript](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)(Renderscript 시작)
