---
title: Renderscript 소개
description: 이 가이드는 Renderscript를 소개 하 고는 내장 Renderscript API의 Xamarin.Android 응용 프로그램에서 해당 대상 API 수준 17 이상인를 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: f9e21a51c409c5444f137a63eb2c6fadfef03cbe
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="an-introduction-to-renderscript"></a>Renderscript 소개

_이 가이드는 Renderscript를 소개 하 고는 내장 Renderscript API의 Xamarin.Android 응용 프로그램에서 해당 대상 API 수준 17 이상인를 사용 하는 방법에 설명 합니다._

## <a name="overview"></a>개요

Renderscript은 광범위 한 계산 리소스를 필요로 하는 Android 응용 프로그램의 성능을 향상 하기 위해 Google에서 만든 프로그래밍 프레임 워크입니다. 낮은 수준의 높은 성능에 따라 API [C99](http://en.wikipedia.org/wiki/C99)합니다. 낮은 수준에서 Cpu, Gpu 또는 Dsp 실행 되는 API 이기 때문에 Renderscript은 다음 중 하나를 수행 해야 할 수 있는 Android 앱에 적합 합니다.

* 그래픽
* 이미지 처리
* 암호화
* 신호 처리
* 수학 루틴

Renderscript ´ ֲ `clang` 스크립트 APK에 제공 되는 원하는 LLVM 바이트 코드를 컴파일합니다. 앱이 처음으로 실행 되는 경우 원하는 LLVM 바이트 코드 프로세서 장치에 대 한 기계어 코드로 컴파일됩니다. 이 구조를 사용 하지 않고도 기계어 코드의 장점에 기록해 야 하 각 프로세서에 대 한 장치에 자신 자체 악용 하는 Android 응용 프로그램입니다.

두 구성 요소는 Renderscript 루틴에 있습니다.

1. **Renderscript 런타임** &ndash; 는 Renderscript를 실행 하는 일을 담당 하는 기본 Api입니다. 응용 프로그램에 대해 작성 된 모든 Renderscripts 포함 됩니다.

2. **Android 프레임 워크에서 관리 되는 래퍼** &ndash; 관리 되는 Android 앱을 제어 하 고 Renderscript 런타임 및 스크립트와 상호 작용 하는 클래스입니다. Renderscript 런타임 제어 하기 위한 제공 프레임 워크 클래스, Android 도구 체인 Renderscript 소스 코드를 검사 되며 Android 응용 프로그램에서 사용 하기 위해 관리 되는 래퍼 클래스를 생성 합니다.

다음 다이어그램에서는 이러한 구성 요소 간의 관계를 보여 줍니다.

![Android 프레임 워크 Renderscript 런타임 상호 작용 하는 방법을 보여 주는 다이어그램](renderscript-images/renderscript-01.png)

Renderscripts Android 응용 프로그램에서 사용 하기 위한 세 가지 중요 한 개념 가지가 있습니다.

1. **컨텍스트** &ndash; 관리 되는 API Renderscript에 리소스를 할당 하 고 전달 하 고는 Renderscript에서 데이터를 받으려면 Android 앱을 허용 하는 Android SDK에서 제공 합니다.

2. **A _계산 커널_**  &ndash; 라고도 _루트 커널_ 또는 _커널_,이 작업을 수행 하는 루틴입니다. 커널 매우 비슷합니다 C 독립적입니다. 이 할당 된 메모리에 모든 데이터에 대해 실행 되는 병렬화 루틴입니다.

3. **할당 된 메모리** &ndash; 데이터와 커널을 통해 전달 되는  _[할당](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/)_합니다. 에 대 한 커널은 하나의 입력이 될 수 있습니다 및/또는 출력 1 개인 할당

[Android.Renderscripts](https://developer.xamarin.com/api/namespace/Android.Renderscripts/) 네임 스페이스 Renderscript 런타임과 상호 작용 하기 위한 클래스를 포함 합니다. 특히는 [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/) 클래스는 수명 주기 및 리소스 Renderscript 엔진을 관리 합니다. 하나 이상의 Android 앱 초기화 해야 [ `Android.Renderscripts.Allocation` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/) 개체입니다. 할당은 할당 및 Android 앱 및 Renderscript 런타임 간에 공유 되는 메모리에 액세스 하는 일을 담당 하는 관리 되는 API입니다. 일반적으로 중 하나의 할당 입력의 경우 만들어지고 필요에 따라 다른 할당 커널의 출력을 만듭니다. Renderscript 런타임 엔진과 연결 된 관리 되는 래퍼 클래스는 할당에 의해 사용 되는 메모리에 대 한 액세스 관리, 추가 작업을 수행 하는 Android 응용 프로그램 개발자에 대 한 필요가 없습니다.

하나 이상의 할당 포함 될 [Android.Renderscripts.Elements](https://developer.xamarin.com/api/type/Android.Renderscripts.Element/)합니다.
요소는 각 할당의 데이터를 설명 하는 특수화 된 형식입니다.
요소에 대 한 유형의 할당 일치 해야 출력은 입력 요소의 형식입니다. Renderscript 각 요소를 병렬로 입력된 할당에 대해 반복 되며 결과 출력에 쓸를 실행할 때 할당 합니다. 요소는 다음과 같은 두 종류가 있습니다.

- **단순 유형** &ndash; C 데이터 형식과 동일 이것이 개념적으로 `float` 또는 `char`합니다.

- **복합 형식** &ndash; 이 형식은 C 비슷합니다 `struct`합니다.

Renderscript 엔진은 각 할당의 요소가 커널에서 요구 하는와 호환 되도록 런타임 검사를 수행 합니다. 할당에서 요소의 데이터 형식과 커널에 필요한 데이터 형식이 일치 하지 않으면, 예외가 throw 됩니다.

모든 Renderscript 커널의 하위 유형으로 래핑됩니다는 [ `Android.Renderscripts.Script` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Script/) 클래스입니다. `Script` 를 설정 하려면 매개 변수는 Renderscript에 대 한 적절 한 클래스는 `Allocations`는 Renderscript 실행 합니다. 두 개의 `Script` Android SDK의 서브 클래스:


- **`Android.Renderscripts.ScriptIntrinsic`** &ndash; Android SDK 작업에 제공 하 고의 서브 클래스에서 액세스할 수 더 일반적인 Renderscript 작업 중 일부는 [ScriptIntrinsic](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic/) 클래스입니다. 이미 제공 되는 대로 해당 응용 프로그램에서 이러한 스크립트를 사용 하는 추가 단계를 수행 하는 개발자에 대 한에 필요가 없습니다.

- **`ScriptC_XXXXX`** &ndash; 라고도 _사용자 스크립트_,이 스크립트 개발자가 작성 되 고 APK 패키지입니다. 컴파일 타임에 Android 도구 체인 스크립트를 Android 앱에서 사용할 수 있도록 관리 되는 래퍼 클래스를 생성 합니다.
  이러한 생성 된 클래스 이름이 접두사로 Renderscript 파일의 이름이 `ScriptC_`합니다. 작성 하 고 사용자 스크립트 통합 공식적으로 사용할 수 없습니다 Xamarin.Android 하 고이 가이드의 범위를 벗어나지만 합니다.

이 두 종류의는 `StringIntrinsic` Xamarin.Android로 사용할 수 있습니다. 이 가이드는 Xamarin.Android 응용 프로그램의 기본 스크립트를 사용 하는 방법에 설명 합니다.

## <a name="requirements"></a>요구 사항

이 가이드는 해당 대상 API 수준 17 이상인 Xamarin.Android 응용 프로그램에 대 한 합니다. 사용 _사용자 스크립트_ 이 가이드에서 다루지 않습니다.

[Xamarin.Android V8 지원 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/) backports Android SDK의 이전 버전을 대상으로 하는 앱에 대 한 내장 Renderscript API입니다. Xamarin.Android 프로젝트에이 패키지를 추가 해야 앱에서 허용 내장 스크립트를 활용 하 여 Android SDK의 해당 대상 이전 버전입니다.

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>Xamarin.Android에서 Renderscripts 내장 함수를 사용 하 여

내장 스크립트는 최소한의 추가 코드와 컴퓨팅 집약적인 작업을 수행 하는 유용한 방법입니다. 장치의 큰 크로스 섹션에 맞춰져 있으며 최적의 성능을 제공 하도록 조정 하는 손 있다가 합니다.
10 배 더 관리 코드 및 사용자 지정 C 구현 보다 후 2-3 x 시간을 실행 하는 기본 스크립트에 대 한 일반적이 지 않습니다. 일반적인 처리 시나리오에서 내장 함수는 스크립트에서 다룹니다. 이 목록은 내장 스크립트 Xamarin.Android에서 현재 스크립트를 설명합니다.

- [ScriptIntrinsic3DLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsic3DLUT//) &ndash; 3D 조회 테이블을 사용 하 여 RGBA 하려면 RGB를 변환 합니다. 

- [ScriptIntrinsicBLAS](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html) &ndash; Provideshigh 성능 Renderscript Api를 [BLAS](http://www.netlib.org/blas/)합니다. BLAS (기본 선형 대 수 하위 프로그램)는 기본 벡터 및 매트릭스 작업을 수행 하기 위한 표준 구성 요소를 제공 하는 루틴입니다. 

- [ScriptIntrinsicBlend](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlend) &ndash; 두 개의 할당을 함께 혼합 합니다.

- [ScriptIntrinsicBlur](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicBlur) &ndash; 할당을 가우스 흐림 효과 적용 합니다.

- [ScriptIntrinsicColorMatrix](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicColorMatrix/) &ndash; (즉, 변경 색 조정 색상) 할당에 색 매트릭스를 적용 합니다.

- [ScriptIntrinsicConvolve3x3](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve3x3/) &ndash; 할당에 3 x 3 색 매트릭스를 적용 합니다.

- [ScriptIntrinsicConvolve5x5](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicConvolve5x5/) &ndash; 5 x 5 색 매트릭스 할당에 적용 합니다.

- [ScriptIntrinsicHistogram](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicHistogram/) &ndash; 내장 히스토그램 필터입니다.

- [ScriptIntrinsicLUT](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicLUT/) &ndash; 채널별 조회 테이블 버퍼에 적용 됩니다.

- [ScriptIntrinsicResize](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicResize/) &ndash; 2D 할당의 크기 조정을 수행 하는 것에 대 한 스크립트입니다.

- [ScriptIntrinsicYuvToRGB](https://developer.xamarin.com/api/type/Android.Renderscripts.ScriptIntrinsicYuvToRGB/) &ndash; YUV 버퍼 RGB를 변환 합니다.

각 기본 스크립트에 대 한 자세한 내용은 API 설명서를 참조 하세요.

Renderscript Android 응용 프로그램에서 사용 하기 위한 기본 단계는 다음과 같습니다.

**Renderscript 컨텍스트를 만들어** &ndash; 는 [ `Renderscript` ](https://developer.xamarin.com/api/type/Android.Renderscripts.RenderScript/) 클래스는 관리 되는 래퍼 Renderscript 컨텍스트 및 초기화, 리소스 관리를 제어 하 고 정리 됩니다. Renderscript 개체가 사용 하 여 생성 되는 `RenderScript.Create` 팩터리 메서드를 매개 변수로 Android 컨텍스트 (예: 작업)를 사용 합니다. 다음 코드 줄 Renderscript 컨텍스트를 초기화 하는 방법을 보여 줍니다.

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

**할당을 만들** &ndash; 내장 스크립트에 따라 하나 또는 두 개의 만들어야 할 수도 `Allocation`s입니다. [ `Android.Renderscripts.Allocation` ](https://developer.xamarin.com/api/type/Android.Renderscripts.Allocation/) 클래스에는 내장 함수에 대 한 인스턴스화에 도움이 되도록 여러 팩터리 메서드가 있습니다. 예를 들어, 다음 코드 조각에는 비트맵에 대 한 할당을 만드는 방법을 보여 줍니다.

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

자주 만들어야 할 수는 `Allocation` 스크립트의 출력 데이터를 보관 합니다. 다음이 코드 조각에서는 사용 하는 `Allocation.CreateTyped` 초를 인스턴스화하는 도우미 `Allocation` 원래과 같은 유형:

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

**스크립트 래퍼를 인스턴스화하** &ndash; 내장 스크립트 래퍼 클래스의 각 있어야 도우미 메서드 (일반적으로 호출 `Create`) 해당 스크립트에 대 한 래퍼 개체를 인스턴스화하기 위한 합니다. 다음 코드 조각은 인스턴스화하는 방법의 예는 `ScriptIntrinsicBlur` 흐림 개체입니다. `Element.U8_4` 도우미 메서드는 8 비트 부호 없는 정수 값의 데이터를 보관 하기에 적합 한 4 개의 필드는 데이터 형식을 설명 하는 요소 만듭니다는 `Bitmap` 개체:

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

**Allocation(s), 매개 변수 설정 및 스크립트 실행을 할당** &ndash; 는 `Script` 클래스를 제공는 `ForEach` 메서드를 실제로 Renderscript를 실행 합니다. 이 메서드는 각 반복 `Element` 에 `Allocation` 입력된 데이터를 보유 합니다. 일부 경우에 제공 해야 할 수도 있는 `Allocation` 출력을 보유 하는 합니다.
`ForEach` 출력의 내용을 덮어씁니다 할당 합니다. 이 예제를 수행 하려면 이전 단계의 코드 조각이 있는 입력된 할당 할당 매개 변수를 설정 하 고 마지막으로 스크립트를 실행 하는 방법을 보여 줍니다 (출력에 결과 복사 할당):

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

체크 아웃 하는 것이 좋습니다는 [Renderscript 사용 하 여 이미지 흐림](https://developer.xamarin.com/recipes/android/other_ux/drawing/blur_an_image_with_renderscript/) 레 Xamarin.Android에 내장 함수는 스크립트를 사용 하는 방법의 전체 예제는 것이 있습니다.

## <a name="summary"></a>요약

이 가이드는 Xamarin.Android 응용 프로그램에서 사용 하는 방법과 Renderscript 도입 되었습니다. 간단 하 게 Renderscript 란 무엇이 고 Android 응용 프로그램의 작동 방법을 설명 합니다. 설명 된 Renderscript 및 간의 차이의 주요 구성 요소 중 일부 _사용자 스크립트_ 및 _내장 스크립트_합니다. 마지막으로,이 가이드는 내장 함수는 스크립트를 사용 하 여 Xamarin.Android 응용 프로그램에서의 단계를 설명 합니다.



## <a name="related-links"></a>관련 링크

- [Android.Renderscripts 네임 스페이스](https://developer.xamarin.com/api/namespace/Android.Renderscripts/)
- [흐림 Renderscript 사용 하 여 이미지](https://developer.xamarin.com/recipes/android/other_ux/drawing/blur_an_image_with_renderscript/)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [자습서: Renderscript 시작](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)
