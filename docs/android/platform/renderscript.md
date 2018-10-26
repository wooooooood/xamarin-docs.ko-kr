---
title: Renderscript 소개
description: 이 가이드는 Renderscript를 소개 하 고 내장 Renderscript API의 Xamarin.Android 응용 프로그램에서 해당 대상 API 수준 17 이상을 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 5369542552a41100443c5e91ceca9e110c5c7c3c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108732"
---
# <a name="an-introduction-to-renderscript"></a>Renderscript 소개

_이 가이드는 Renderscript를 소개 하 고 내장 Renderscript API의 Xamarin.Android 응용 프로그램에서 해당 대상 API 수준 17 이상을 사용 하는 방법에 설명 합니다._

## <a name="overview"></a>개요

Renderscript는 광범위 한 계산 리소스를 필요로 하는 Android 응용 프로그램의 성능 향상을 위해 Google에서 만든 프로그래밍 프레임 워크입니다. 낮은 수준의 높은 성능에 따라 API [C99](http://en.wikipedia.org/wiki/C99)합니다. 낮은 수준에서 Cpu, Gpu 또는 Dsp 실행 되는 API 이기 때문에 Renderscript는 다음 중 하나를 수행 해야 할 수 있는 Android 앱에 적합 합니다.

* 그래픽
* 이미지 처리
* 암호화
* 신호 처리
* 수학 루틴

Renderscript 사용할지 `clang` 스크립트 APK에 포함 되는 LLVM 바이트 코드를 컴파일합니다. 앱을 처음으로 실행 하는 경우에 LLVM 바이트 코드 장치의 프로세서에 대 한 기계어 코드로 컴파일됩니다. 이 아키텍처에 기록해 야 각 프로세서에 대 한 장치에서 직접 자신 하지 않고도 컴퓨터 코드의 장점을 악용 하는 Android 응용 프로그램을 수 있습니다.

두 가지 구성 요소는 Renderscript 루틴에:

1. **Renderscript 런타임에서** &ndash; Renderscript를 실행 하는 일을 담당 하는 네이티브 Api입니다. 응용 프로그램에 대 한 기록 모든 Renderscripts 포함 됩니다.

2. **Android 프레임 워크에서 관리 되는 래퍼** &ndash; 관리 되는 Android 앱을 제어 하 고 Renderscript 런타임 및 스크립트와 상호 작용 하는 클래스입니다. Android 도구 체인 Renderscript 런타임 제어에 대 한 제공 된 프레임 워크 클래스 외에도 Renderscript 소스 코드를 살펴보고이 Android 응용 프로그램에서 사용 하 여 관리 되는 래퍼 클래스를 생성 합니다.

다음 다이어그램에서는 이러한 구성 요소 간의 관계를 보여 줍니다.

![Android 프레임 워크 Renderscript 런타임 상호 작용 하는 방법을 보여 주는 다이어그램](renderscript-images/renderscript-01.png)

Renderscripts Android 응용 프로그램에서 사용 하기 위한 세 가지 중요 한 개념이 있습니다.

1. **컨텍스트** &ndash; 관리 되는 API Renderscript에 리소스를 할당 하 고 전달 하 고는 Renderscript에서 데이터를 받으려면 Android 앱을 허용 하는 Android SDK에서 제공 합니다.

2. **A _커널 계산_**  &ndash; 라고도 합니다 _루트 커널_ 또는 _커널_이 작업을 수행 하는 루틴입니다. C 함수; 커널 매우 비슷합니다. 할당 된 메모리의 모든 데이터를 통해 실행 되는 병렬 처리 가능한 루틴을 것입니다.

3. **할당 된 메모리** &ndash; 데이터와 커널을 통해 전달 되는  _[할당](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)_ 합니다. 에 대 한 커널에는 하나의 입력 수 및/또는 할당 출력

합니다 [Android.Renderscripts](https://developer.xamarin.com/api/namespace/Android.Renderscripts/) 네임 스페이스 Renderscript 런타임과 상호 작용 하기 위한 클래스를 포함 합니다. 특히 합니다 [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/) 클래스 수명 주기 및 Renderscript 엔진의 리소스를 관리 합니다. 하나 이상의 Android 앱 초기화 해야 합니다. [`Android.Renderscripts.Allocation`](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)
개체입니다. 할당은 할당 및 Android 앱 및 Renderscript 런타임 간에 공유 되는 메모리에 액세스 하는 일을 담당 하는 관리 되는 API입니다. 일반적으로 하나의 할당 입력에 대해 생성 됩니다 하 고 필요에 따라 다른 할당 커널 출력을 포함할 만들어집니다. Renderscript 런타임 엔진과 연결 된 관리 되는 래퍼 클래스 할당을 보유 한 메모리에 대 한 액세스 관리, 추가 작업을 수행 하는 Android 앱 개발자를 위한 하지 않아도 됩니다.

하나 이상의 할당 될 [Android.Renderscripts.Elements](https://developer.xamarin.com/api/type/Android.Renderscripts.Element/)합니다.
요소는 각 할당의 데이터를 설명 하는 특수화 된 형식입니다.
요소 형식은 출력의 할당 된 입력 요소의 형식과 일치 해야 합니다. Renderscript 병렬로 입력된 할당의 각 요소에 대해 반복 되며 결과 출력에 작성을 실행할 때 할당 합니다. 요소는 다음과 같은 두 종류가 있습니다.

- **단순 형식** &ndash; 개념적으로 동일 C 데이터 형식으로 `float` 또는 `char`합니다.

- **복합 형식** &ndash; 이 형식은 C 비슷합니다 `struct`합니다.

Renderscript 엔진에는 각 할당의 요소를 사용 하 여 커널에 필요한 호환 되는지 확인에 런타임 검사를 수행 합니다. 할당에서 요소의 데이터 형식 커널 예상 되는 데이터 형식이 일치 하지 않으면, 예외가 throw 됩니다.

하위 유형으로 모든 Renderscript 커널 래핑될 합니다 [`Android.Renderscripts.Script`](https://developer.xamarin.com/api/type/Android.Renderscripts.Script/)
포함됩니다. 합니다 `Script` 클래스는 Renderscript에 대 한 매개 변수를 설정, 적절 한 설정 하는 데 사용 됩니다 `Allocations`, Renderscript를 실행 합니다. 두 개의 `Script` Android SDK의 서브 클래스:


- **`Android.Renderscripts.ScriptIntrinsic`** &ndash; Android SDK 묶이는 몇 가지 일반적인 Renderscript 작업 및의 서브 클래스에서 액세스할 수 있습니다 합니다 [ScriptIntrinsic](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic/) 클래스입니다. 이미 제공 되므로 해당 응용 프로그램에서 이러한 스크립트를 사용 하려면 추가 단계를 수행 하는 개발자에 대 한 하지 않아도가 됩니다.

- **`ScriptC_XXXXX`** &ndash; 라고도 _사용자 스크립트_,이 스크립트 개발자가 작성 되 고 APK 패키지입니다. 컴파일 타임에 Android 도구 체인은 스크립트를 Android 앱에서 사용할 수 있도록 관리 되는 래퍼 클래스를 생성 합니다.
  이렇게 생성 된 클래스의 이름이 접두사로 Renderscript 파일의 이름을 `ScriptC_`입니다. 작성 하 고 통합 사용자 스크립트는 Xamarin.Android에서이 가이드에서 다루지을 공식적으로 지원 되지는 않습니다.

이 두 종류의는 `StringIntrinsic` Xamarin.Android에서 지원 됩니다. 이 가이드에서는 Xamarin.Android 응용 프로그램에서 기본 스크립트를 사용 하는 방법을 설명 합니다.

## <a name="requirements"></a>요구 사항

이 가이드는 Xamarin.Android 응용 프로그램에 대 한 해당 대상 API 수준 17 이상. 사용 _사용자 스크립트_ 이 가이드에서는 다루지 않습니다.

합니다 [Xamarin.Android V8 지원 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/) backports 내장 Renderscript API의 이전 버전의 Android SDK를 대상으로 하는 앱에 대 한 합니다. 이 패키지를 Xamarin.Android 프로젝트에 추가 허용 앱 내장 스크립트를 활용 하 여 Android SDK의 대상 이전 버전 해당 해야 합니다.

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>내장 Renderscripts Xamarin.Android에서 사용 하 여

기본 스크립트는 최소한의 추가 코드를 사용 하 여 컴퓨팅 집약적인 작업을 수행할 수 있는 좋은 방법입니다. 이러한 직접 장치의 큰 간 섹션에서 최적의 성능을 제공 하기 위해 조정 되었습니다.
관리 되는 코드 보다 더 빠르게 x 10과 2 ~ 3 배 후 사용자 지정 C 구현 보다 실행 하려면 스크립트를 내장 함수에 대 한 일반적이 지 않은 것입니다. 대부분의 일반적인 처리 시나리오 내장 스크립트에 포함 됩니다. 이 목록은 내장 스크립트 Xamarin.Android에서 현재 스크립트를 설명합니다.

- [ScriptIntrinsic3DLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic3DLUT//) &ndash; 3D 조회 테이블을 사용 하 여 RGBA RGB로 변환 합니다. 

- [ScriptIntrinsicBLAS](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html) &ndash; Provideshigh 성능 Renderscript Api 하 [BLAS](http://www.netlib.org/blas/)합니다. BLAS (기본 선형 대 수 하위 프로그램)는 기본 벡터 및 매트릭스 작업을 수행 하기 위한 표준 구성 요소를 제공 하는 루틴입니다. 

- [ScriptIntrinsicBlend](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlend) &ndash; 두 할당을 함께 혼합 합니다.

- [ScriptIntrinsicBlur](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlur) &ndash; 할당을 가우스 흐림 효과 적용 합니다.

- [ScriptIntrinsicColorMatrix](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicColorMatrix/) &ndash; (즉, 다양 한 색상 변경, 조정 hue) 할당에 색 매트릭스를 적용 합니다.

- [ScriptIntrinsicConvolve3x3](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve3x3/) &ndash; 할당에 3x3 색 매트릭스를 적용 합니다.

- [ScriptIntrinsicConvolve5x5](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve5x5/) &ndash; 할당에 5x5 색 매트릭스를 적용 합니다.

- [ScriptIntrinsicHistogram](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicHistogram/) &ndash; 내장 히스토그램 필터를 합니다.

- [ScriptIntrinsicLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicLUT/) &ndash; 채널별 조회 테이블을 버퍼에 적용 됩니다.

- [ScriptIntrinsicResize](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicResize/) &ndash; 2D 할당의 크기 조정이 수행에 대 한 스크립트입니다.

- [ScriptIntrinsicYuvToRGB](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicYuvToRGB/) &ndash; YUV 버퍼 RGB로 변환 합니다.

각 기본 스크립트의 세부 정보에 대 한 API 설명서를 참조 하세요.

Renderscript를 사용 하 여 Android 응용 프로그램의 기본 단계는 다음과 같습니다.

**Renderscript 컨텍스트를 만듭니다** &ndash; 합니다 [`Renderscript`](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/)
클래스는 관리 되는 래퍼 Renderscript 컨텍스트 및 초기화, 리소스 관리를 제어 하 고 정리 됩니다. Renderscript 개체를 사용 하 여 만들어집니다는 `RenderScript.Create` 팩터리 메서드를 매개 변수로 사용 하는 Android 컨텍스트 (예: 작업)를 사용 합니다. 다음 코드 줄 Renderscript 컨텍스트를 초기화 하는 방법에 설명 합니다.

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

**할당을 만들** &ndash; 내장 스크립트에 따라 하나 또는 두 개를 만들어야 할 수 있습니다 `Allocation`s입니다. 는 [`Android.Renderscripts.Allocation`](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)
클래스에 내장 함수에 대 한 할당을 인스턴스화하고 사용 하는 데 여러 팩터리 메서드가 있습니다. 예를 들어 다음 코드 조각에는 비트맵에 대 한 할당을 만드는 방법을 보여 줍니다.

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

자주 만들어야 할 수는 `Allocation` 스크립트의 출력 데이터를 저장할 수 있습니다. 이 다음 코드 조각을 사용 하는 방법을 보여 줍니다 합니다 `Allocation.CreateTyped` 초를 인스턴스화하는 도우미 `Allocation` 동일한 원래 형식:

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

**스크립트 래퍼를 인스턴스화합니다** &ndash; 내장 스크립트 래퍼 클래스의 각각 있어야 도우미 메서드 (일반적으로 호출 `Create`) 해당 스크립트에 대 한 래퍼 개체를 인스턴스화하기 위한 합니다. 다음 코드 조각은 인스턴스화하는 방법의 예는 `ScriptIntrinsicBlur` 개체 흐리게 표시 합니다. 합니다 `Element.U8_4` 도우미 메서드는 8 비트 부호 없는 정수 값의 데이터를 보관 하기에 적합 한 4 개의 필드는 데이터 형식을 설명 하는 요소를 만듭니다를 `Bitmap` 개체:

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

**할당 Allocation(s), 매개 변수 설정 및 스크립트 실행** &ndash; 는 `Script` 클래스를 제공는 `ForEach` 실제로 Renderscript를 실행 하는 방법입니다. 이 메서드를 각 반복 `Element` 에 `Allocation` 입력된 데이터를 보유 합니다. 일부 경우에 제공 해야 할 수도 `Allocation` 보유 하는 출력입니다.
`ForEach` 출력의 내용을 덮어씁니다 할당 합니다. 이전 단계에서 코드 조각을 사용 하 여 수행할이 예제에서는 입력된 할당 할당, 매개 변수를 설정 하 고, 마지막으로 스크립트를 실행 하는 방법을 보여줍니다 (출력에 결과 복사 할당).

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

체크 아웃 하려고 할 수 있습니다는 [Renderscript 사용 하 여 이미지를 흐리게](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript) 레 것 Xamarin.Android에는 기본 스크립트를 사용 하는 방법의 전체 예제입니다.

## <a name="summary"></a>요약

이 가이드에는 Xamarin.Android 응용 프로그램에서 사용 하는 방법과 Renderscript 도입 되었습니다. 간단 하 게 Renderscript 이란 무엇 이며 Android 응용 프로그램에서 작동 하는 방법을 설명 합니다. Renderscript 및 차이의 주요 구성 요소 중 일부 설명 _사용자 스크립트_ 하 고 _내장 스크립트_합니다. 마지막으로,이 가이드는 기본 스크립트를 사용 하 여 Xamarin.Android 응용 프로그램에서의 단계를 설명 합니다.



## <a name="related-links"></a>관련 링크

- [Android.Renderscripts 네임 스페이스](https://developer.xamarin.com/api/namespace/Android.Renderscripts/)
- [Renderscript 사용 하 여 이미지를 흐리게 표시](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [자습서: Renderscript 시작 합니다.](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)
