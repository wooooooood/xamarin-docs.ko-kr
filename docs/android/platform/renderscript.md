---
title: Renderscript 소개
description: 이 가이드에서는 Renderscript를 소개 하 고 API 레벨 17 이상을 대상으로 하는 Xamarin Android 응용 프로그램에서 내장 Renderscript API를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 9f15ef73e51a2e94e1a1174134f3e69d2cb2c4a3
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511435"
---
# <a name="an-introduction-to-renderscript"></a>Renderscript 소개

_이 가이드에서는 Renderscript를 소개 하 고 API 레벨 17 이상을 대상으로 하는 Xamarin Android 응용 프로그램에서 내장 Renderscript API를 사용 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Renderscript는 광범위 한 계산 리소스를 필요로 하는 Android 응용 프로그램의 성능을 향상 시키기 위해 Google에서 만든 프로그래밍 프레임 워크입니다. [C99](https://en.wikipedia.org/wiki/C99)을 기반으로 하는 낮은 수준의 고성능 API입니다. Cpu, Gpu 또는 Dsp에서 실행 되는 하위 수준 API 이므로 Renderscript는 다음 작업을 수행 해야 할 수 있는 Android 앱에 적합 합니다.

* 그래픽
* 이미지 처리
* 암호화
* 신호 처리
* 수학 루틴

Renderscript는 apk에 번들로 제공 되는 바이트 코드를 LLVM 스크립트를 사용 `clang` 하 고 컴파일합니다. 앱이 처음으로 실행 될 때 LLVM 바이트 코드는 장치의 프로세서에 대 한 기계어 코드로 컴파일됩니다. 이 아키텍처를 통해 Android 응용 프로그램은 개발자가 장치 자체의 각 프로세서에 대 한 코드를 작성 하지 않고도 컴퓨터 코드의 이점을 활용할 수 있습니다.

Renderscript 루틴에는 두 가지 구성 요소가 있습니다.

1. **Renderscript 런타임** &ndash; 이는 renderscript를 실행 하는 네이티브 api입니다. 여기에는 응용 프로그램에 대해 작성 된 모든 Renderscripts가 포함 됩니다.

2. **Android Framework의 관리 되는 래퍼** &ndash; Android 앱이 renderscript 런타임과 스크립트를 제어 하 고 조작할 수 있도록 하는 관리 되는 클래스입니다. Renderscript runtime을 제어 하기 위한 프레임 워크 제공 클래스 외에도 Android 도구 체인는 Renderscript 소스 코드를 검사 하 고 Android 응용 프로그램에서 사용할 관리 되는 래퍼 클래스를 생성 합니다.

다음 다이어그램에서는 이러한 구성 요소가 어떻게 관련 되는지를 보여 줍니다.

![Android Framework가 Renderscript 런타임과 상호 작용 하는 방식을 보여 주는 다이어그램](renderscript-images/renderscript-01.png)

Android 응용 프로그램에서 Renderscripts를 사용 하는 데는 다음과 같은 세 가지 중요 한 개념이 있습니다.

1. **컨텍스트** &ndash; Renderscript에 리소스를 할당 하 고 Android 앱이 renderscript에서 데이터를 전달 하 고 받을 수 있도록 하는 Android SDK에서 제공 하는 관리 되는 API입니다.

2. **_계산 커널_** 루트 커널 또는 _커널이_라고도 하며,이는 작업을 수행 하는 루틴입니다. &ndash; 커널은 C 함수와 매우 유사 합니다. 할당 된 메모리의 모든 데이터에 대해 실행 되는 병렬화 루틴입니다.

3. **할당 된 메모리** &ndash;데이터는 _할당[을 통해 커널에 전달 됩니다](xref:Android.Renderscripts.Allocation)_ . 커널에는 하나의 입력 및/또는 하나의 출력 할당이 있을 수 있습니다.

[Android Renderscripts](xref:Android.Renderscripts) 네임 스페이스는 renderscripts 런타임과 상호 작용 하기 위한 클래스를 포함 합니다. 특히 클래스는 [`Renderscript`](xref:Android.Renderscripts.RenderScript) renderscript 엔진의 수명 주기와 리소스를 관리 합니다. Android 앱은 하나 이상의[`Android.Renderscripts.Allocation`](xref:Android.Renderscripts.Allocation)
개체여야 합니다. 할당은 Android 앱과 Renderscript 런타임 간에 공유 되는 메모리를 할당 하 고 액세스 하는 관리 되는 API입니다. 일반적으로 하나의 할당은 입력에 대해 생성 되 고, 선택적으로 다른 할당을 만들어 커널 출력을 저장할 수 있습니다. Renderscript 런타임 엔진과 연결 된 관리 되는 래퍼 클래스는 할당에 저장 된 메모리에 대 한 액세스를 관리 하므로 Android 앱 개발자가 추가 작업을 수행할 필요가 없습니다.

할당에는 하나 이상의 [Android Renderscripts. 요소가](xref:Android.Renderscripts.Element)포함 됩니다.
요소는 각 할당의 데이터를 설명 하는 특수 한 형식입니다.
출력 할당의 요소 형식은 입력 요소의 형식과 일치 해야 합니다. 실행 시 Renderscript는 입력 할당의 각 요소를 병렬로 반복 하 고 결과를 출력 할당에 씁니다. 다음과 같은 두 가지 유형의 요소가 있습니다.

- **단순 형식** 개념적으로이는 C 데이터 `float` 형식 또는 `char`와 동일 합니다. &ndash;

- **복합 형식** 이 형식은 C `struct`와 유사 합니다. &ndash;

Renderscript 엔진은 각 할당의 요소가 커널에 필요한 항목과 호환 되는지 확인 하기 위해 런타임 검사를 수행 합니다. 할당에 있는 요소의 데이터 형식이 커널에서 예상 하는 데이터 형식과 일치 하지 않는 경우 예외가 throw 됩니다.

모든 Renderscript 커널은의 하위 항목인 형식으로 래핑됩니다.[`Android.Renderscripts.Script`](xref:Android.Renderscripts.Script)
포함됩니다. 클래스는 renderscript에 대 한 매개 변수를 설정 하 고, 적절 `Allocations`한를 설정 하 고, renderscript를 실행 하는 데 사용 됩니다. `Script` Android SDK에는 `Script` 두 개의 하위 클래스가 있습니다.


- **`Android.Renderscripts.ScriptIntrinsic`** &ndash; 보다 일반적인 renderscript 태스크 중 일부는 Android SDK에 번들로 제공 되며 [scriptintrinsic](xref:Android.Renderscripts.ScriptIntrinsic) 클래스의 서브 클래스에서 액세스할 수 있습니다. 개발자가 응용 프로그램에서 이러한 스크립트를 사용 하기 위해 이미 제공 된 추가 단계를 수행할 필요가 없습니다.

- **`ScriptC_XXXXX`** 사용자 스크립트 라고도 하는 이러한 스크립트는 개발자가 작성 하 고 apk에 패키지 된 스크립트입니다. &ndash; 컴파일 시간에 Android 도구 체인는 Android 앱에서 스크립트를 사용할 수 있는 관리 되는 래퍼 클래스를 생성 합니다.
  이러한 생성 된 클래스의 이름은 Renderscript 파일의 이름이 며 접두사가 붙습니다 `ScriptC_`. 사용자 스크립트를 작성 하 고 통합 하는 것은이 가이드의 범위를 벗어나 Xamarin. Android에서 공식적으로 지원 되지 않습니다.

이러한 두 가지 형식 중 하나만 `StringIntrinsic` xamarin.ios에서 지원 됩니다. 이 가이드에서는 Xamarin Android 응용 프로그램에서 내장 스크립트를 사용 하는 방법을 설명 합니다.

## <a name="requirements"></a>요구 사항

이 가이드는 API 레벨 17 이상을 대상으로 하는 Xamarin Android 응용 프로그램을 위한 것입니다. _사용자 스크립트_ 를 사용 하는 방법은이 가이드에서 다루지 않습니다.

V8는 이전 버전의 Android SDK를 대상으로 하는 앱에 대 한 내장 Renderscript API를 [지원](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/) 합니다. 이 패키지를 Xamarin.ios 프로젝트에 추가 하면 이전 버전의 Android SDK를 대상으로 하는 앱이 내장 스크립트를 활용 하도록 허용 해야 합니다.

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>Xamarin.ios에서 내장 Renderscripts 사용

내장 스크립트는 최소한의 추가 코드를 사용 하 여 집약적 컴퓨팅 작업을 수행할 수 있는 좋은 방법입니다. 이러한 항목은 장치에 대 한 다양 한 측면에서 최적의 성능을 제공 하도록 조정 되었습니다.
내장 스크립트는 관리 코드 보다 더 빠르게 실행 되 고, 사용자 지정 C 구현 이후 2 배 10 배 실행 되는 경우가 드뭅니다. 대부분의 일반적인 처리 시나리오는 내장 스크립트에 포함 됩니다. 이 내장 스크립트 목록은 Xamarin.ios의 현재 스크립트에 대해 설명 합니다.

- [ScriptIntrinsic3DLUT](xref:Android.Renderscripts.ScriptIntrinsic3DLUT) &ndash; 3d 조회 테이블을 사용 하 여 RGB를 RGBA로 변환 합니다. 

- [ScriptIntrinsicBLAS](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html) &ndash;Provideshigh 성능 renderscript를 [BLAS](http://www.netlib.org/blas/)로 스크립팅 합니다. BLAS (기본 선형 대 수 Subprograms)은 기본 벡터 및 행렬 작업을 수행 하기 위한 표준 구성 요소를 제공 하는 루틴입니다. 

- [ScriptIntrinsicBlend](xref:Android.Renderscripts.ScriptIntrinsicBlend) &ndash; 두 할당을 함께 혼합 합니다.

- [ScriptIntrinsicBlur](xref:Android.Renderscripts.ScriptIntrinsicBlur) &ndash; 할당에 가우스 흐림 효과를 적용 합니다.

- [ScriptIntrinsicColorMatrix](xref:Android.Renderscripts.ScriptIntrinsicColorMatrix) &ndash; 할당에 색 매트릭스를 적용 합니다 (즉, colours 변경, 색상 조정).

- [ScriptIntrinsicConvolve3x3](xref:Android.Renderscripts.ScriptIntrinsicConvolve3x3) &ndash; 3 색 매트릭스를 할당에 적용 합니다.

- [ScriptIntrinsicConvolve5x5](xref:Android.Renderscripts.ScriptIntrinsicConvolve5x5) &ndash; 할당에 5x5 색 매트릭스를 적용 합니다.

- [ScriptIntrinsicHistogram](xref:Android.Renderscripts.ScriptIntrinsicHistogram) &ndash; 내장 히스토그램 필터입니다.

- [ScriptIntrinsicLUT](xref:Android.Renderscripts.ScriptIntrinsicLUT) &ndash; 채널 당 조회 테이블을 버퍼에 적용 합니다.

- [ScriptIntrinsicResize](xref:Android.Renderscripts.ScriptIntrinsicResize) &ndash; 2d 할당의 크기 조정을 수행 하는 스크립트입니다.

- [ScriptIntrinsicYuvToRGB](xref:Android.Renderscripts.ScriptIntrinsicYuvToRGB) &ndash; YUV 버퍼를 RGB로 변환 합니다.

각 내장 스크립트에 대 한 자세한 내용은 API 설명서를 참조 하세요.

Android 응용 프로그램에서 Renderscript를 사용 하는 기본 단계는 다음에 설명 되어 있습니다.

**Renderscript 컨텍스트 만들기** &ndash; 입니다.[`Renderscript`](xref:Android.Renderscripts.RenderScript)
클래스는 Renderscript 컨텍스트를 중심으로 관리 되는 래퍼로 초기화, 리소스 관리 및 정리를 제어 합니다. Renderscript 개체는 Android 컨텍스트 (예 `RenderScript.Create` : 활동)를 매개 변수로 사용 하는 팩터리 메서드를 사용 하 여 생성 됩니다. 다음 코드 줄에서는 Renderscript 컨텍스트를 초기화 하는 방법을 보여 줍니다.

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

**할당 만들기** 내장 스크립트에 따라 하나 또는 두 개의 `Allocation`를 만들어야 할 수도 있습니다. &ndash; 여[`Android.Renderscripts.Allocation`](xref:Android.Renderscripts.Allocation)
클래스에는 내장 함수에 대 한 할당을 인스턴스화하는 데 도움이 되는 몇 가지 팩터리 메서드가 있습니다. 예를 들어 다음 코드 조각은 비트맵에 대 한 할당을 만드는 방법을 보여 줍니다.

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

스크립트의 출력 데이터를 저장 `Allocation` 하기 위해를 만들어야 하는 경우가 많습니다. 다음 코드 조각에서는 `Allocation.CreateTyped` 도우미를 사용 하 여 원본과 동일한 형식의 두 번째 `Allocation` 를 인스턴스화하는 방법을 보여 줍니다.

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

**스크립트 래퍼 인스턴스화** 각 내장 스크립트 래퍼 클래스에는 해당 스크립트에 대 한 래퍼 개체 `Create`를 인스턴스화하기 위한 도우미 메서드 (일반적으로 라고 함)가 있어야 합니다. &ndash; 다음 코드 조각은 `ScriptIntrinsicBlur` 흐림 개체를 인스턴스화하는 방법의 예입니다. 도우미 `Element.U8_4` 메서드는 `Bitmap` 개체의 데이터를 저장 하는 데 적합 한 8 비트의 부호 없는 정수 값에 해당 하는 4 개의 필드인 데이터 형식을 설명 하는 요소를 만듭니다.

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

**할당 할당, 매개 변수 설정 & 스크립트 실행** &ndash; 클래스 는`Script` renderscript `ForEach` 를 실제로 실행 하는 메서드를 제공 합니다. 이 메서드는 입력 데이터를 `Element` 보유 하 `Allocation` 고 있는에서 각를 반복 합니다. 경우에 따라 출력을 보유 하 `Allocation` 는를 제공 해야 할 수도 있습니다.
`ForEach`는 출력 할당의 내용을 덮어씁니다. 이전 단계에서 코드 조각을 사용 하기 위해이 예제에서는 입력 할당을 할당 하 고, 매개 변수를 설정한 다음, 마지막으로 스크립트를 실행 하 여 결과를 출력 할당으로 복사 하는 방법을 보여 줍니다.

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

[Renderscript 조리법에서 이미지 흐림 효과](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript) 를 체크 아웃할 수 있습니다. xamarin.ios에서 내장 스크립트를 사용 하는 방법의 전체 예제입니다.

## <a name="summary"></a>요약

이 가이드에서는 Renderscript 응용 프로그램에서 사용 하는 방법 및 Renderscript를 소개 했습니다. Renderscript의 정의와 Android 응용 프로그램에서 작동 하는 방식에 대해 간략하게 설명 합니다. Renderscript의 몇 가지 주요 구성 요소와 _사용자 스크립트_ 와 _내장 스크립트_의 차이점에 대해 설명 했습니다. 마지막으로,이 가이드에서는 Xamarin Android 응용 프로그램에서 내장 스크립트를 사용 하는 단계에 대해 설명 했습니다.



## <a name="related-links"></a>관련 링크

- [Android Renderscripts 네임 스페이스](xref:Android.Renderscripts)
- [Renderscript를 사용 하 여 이미지 흐리게](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [자습서: Renderscript 시작](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)
