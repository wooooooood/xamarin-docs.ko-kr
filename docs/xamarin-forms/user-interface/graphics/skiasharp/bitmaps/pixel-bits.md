---
title: SkiaSharp 비트맵 픽셀 비트 액세스
description: SkiaSharp 비트맵의 픽셀 비트에 액세스 하 고 수정 하는 다양 한 기술을 알아봅니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: DBB58522-F816-4A8C-96A5-E0236F16A5C6
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9018cbe6e41350b22a0f1f91858017531c75a0ac
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84135583"
---
# <a name="accessing-skiasharp-bitmap-pixel-bits"></a>SkiaSharp 비트맵 픽셀 비트 액세스

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

[**SkiaSharp 비트맵을 파일에 저장**](saving.md)문서에서 보았듯이 비트맵은 일반적으로 JPEG 또는 PNG와 같은 압축 된 형식으로 파일에 저장 됩니다. 와 달리에서는 메모리에 저장 된 SkiaSharp 비트맵이 압축 되지 않습니다. 순차적인 일련의 픽셀로 저장 됩니다. 압축 되지 않은이 형식은 비트맵을 표시 화면으로 전송 하는 것을 용이 하 게 합니다.

SkiaSharp 비트맵에서 사용 되는 메모리 블록은 매우 간단한 방식으로 구성 됩니다. 즉, 왼쪽에서 오른쪽으로 픽셀의 첫 번째 행부터 시작 하 여 두 번째 행으로 계속 됩니다. 전체 색 비트맵의 경우 각 픽셀은 4 바이트로 구성 됩니다. 즉, 비트맵에 필요한 총 메모리 공간이 너비와 높이의 4 배가 됩니다.

이 문서에서는 응용 프로그램이 비트맵의 픽셀 메모리 블록에 액세스 하거나 간접적으로 액세스 하 여 해당 픽셀에 대 한 액세스를 얻는 방법을 설명 합니다. 일부 경우에 프로그램에서 이미지의 픽셀을 분석 하 고 일종의 히스토그램을 생성할 수 있습니다. 더 일반적으로 응용 프로그램은 비트맵을 구성 하는 픽셀을 알고리즘 방식으로 하 여 고유한 이미지를 생성할 수 있습니다.

![픽셀 비트 샘플](pixel-bits-images/PixelBitsSample.png "픽셀 비트 샘플")

## <a name="the-techniques"></a>기술

SkiaSharp는 비트맵의 픽셀 비트에 액세스 하기 위한 몇 가지 방법을 제공 합니다. 사용자가 선택 하는 것은 일반적으로 코딩 편의 (유지 관리와의 용이성과 관련 됨)와 성능 간에 절충 하는 것입니다. 대부분의 경우 다음 메서드 및 속성 중 하나를 사용 하 여 비트맵의 `SKBitmap` 픽셀에 액세스 합니다.

- `GetPixel`및 메서드를 사용 하 여 `SetPixel` 단일 픽셀의 색을 얻거나 설정할 수 있습니다.
- `Pixels`속성은 전체 비트맵의 픽셀 색 배열을 얻거나 색 배열을 설정 합니다.
- `GetPixels`비트맵에서 사용 하는 픽셀 메모리의 주소를 반환 합니다.
- `SetPixels`비트맵에서 사용 하는 픽셀 메모리의 주소를 바꿉니다.

처음 두 가지 기술을 "상위 수준"으로 생각 하 고 두 번째 기술을 "낮은 수준"으로 생각할 수 있습니다. 사용할 수 있는 몇 가지 다른 메서드 및 속성이 있지만 가장 중요 합니다.

이러한 기술 간의 성능 차이를 확인할 수 있도록 [**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) 응용 프로그램에는 빨강 및 파랑 음영을 결합 하 여 그라데이션을 만드는 픽셀을 사용 하 여 비트맵을 만드는 **그라데이션 비트맵** 페이지가 포함 되어 있습니다. 프로그램은 비트맵 픽셀을 설정 하는 다양 한 기술을 사용 하 여이 비트맵의 8 가지 복사본을 만듭니다. 이러한 8 개 비트맵은 각각이 기술에 대 한 간략 한 텍스트 설명도 설정 하 고 모든 픽셀을 설정 하는 데 필요한 시간을 계산 하는 별도의 메서드로 생성 됩니다. 각 메서드는 픽셀 설정 논리를 100 번 반복 하 여 더 나은 성능 예측을 제공 합니다.

### <a name="the-setpixel-method"></a>SetPixel 메서드

몇 개의 개별 픽셀만 설정 하거나 가져올 수 있는 경우 [`SetPixel`](xref:SkiaSharp.SKBitmap.SetPixel(System.Int32,System.Int32,SkiaSharp.SKColor)) 및 [`GetPixel`](xref:SkiaSharp.SKBitmap.GetPixel(System.Int32,System.Int32)) 메서드가 적합 합니다. 이러한 두 가지 방법에 대해 정수 열과 행을 지정 합니다. 이러한 두 메서드는 픽셀 형식에 관계 없이 픽셀을 값으로 얻거나 설정할 수 있도록 합니다 `SKColor` .

```csharp
bitmap.SetPixel(col, row, color);

SKColor color = bitmap.GetPixel(col, row);
```

`col`인수는 0에서 1 사이의 값 사이 여야 하 `Width` 고, `row` 0에서 1 사이의 값 사이 여야 합니다 `Height` .

다음은 메서드를 사용 하 여 비트맵의 콘텐츠를 설정 하는 **그라데이션 비트맵** 의 메서드입니다 `SetPixel` . 비트맵은 256 x 256 픽셀 이며 `for` 루프는 값의 범위로 하드 코딩 됩니다.

```csharp
public class GradientBitmapPage : ContentPage
{
    const int REPS = 100;

    Stopwatch stopwatch = new Stopwatch();
    ···
    SKBitmap FillBitmapSetPixel(out string description, out int milliseconds)
    {
        description = "SetPixel";
        SKBitmap bitmap = new SKBitmap(256, 256);

        stopwatch.Restart();

        for (int rep = 0; rep < REPS; rep++)
            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    bitmap.SetPixel(col, row, new SKColor((byte)col, 0, (byte)row));
                }

        milliseconds = (int)stopwatch.ElapsedMilliseconds;
        return bitmap;
    }
    ···
}
```

각 픽셀의 색 집합에는 빨강 구성 요소가 비트맵 열과 같으며 파랑 구성 요소는 행과 동일 합니다. 결과 비트맵은 왼쪽 위, 오른쪽 위의 빨강, 왼쪽 아래에 파란색, 오른쪽 아래에 그라데이션이 있는 검정색으로 나타납니다.

`SetPixel`메서드를 65536 번 호출 하 고,이 메서드를 효율적으로 사용할 수 있는지 여부에 관계 없이, 대체를 사용할 수 있는 경우 많은 API를 호출 하는 것이 좋습니다. 다행히 몇 가지 대안이 있습니다.

### <a name="the-pixels-property"></a>픽셀 속성

`SKBitmap`[`Pixels`](xref:SkiaSharp.SKBitmap.Pixels)전체 비트맵의 값 배열을 반환 하는 속성을 정의 합니다 `SKColor` . 를 사용 하 여 `Pixels` 비트맵의 색 값 배열을 설정할 수도 있습니다.

```csharp
SKColor[] pixels = bitmap.Pixels;

bitmap.Pixels = pixels;
```

픽셀은 첫 번째 행부터 왼쪽부터 오른쪽, 두 번째 행 등으로 시작 하는 배열에 정렬 됩니다. 배열의 총 색 수는 비트맵 너비와 높이의 곱과 같습니다.

이 속성은 효율적 이기는 하지만 비트맵이 비트맵에서 배열로 복사 되 고 배열에서 비트맵으로 다시 복사 되는 것을 염두에 두고 픽셀은 및에서 값으로 변환 됩니다 `SKColor` .

`GradientBitmapPage`속성을 사용 하 여 비트맵을 설정 하는 클래스의 메서드는 다음과 같습니다 `Pixels` . 메서드는 `SKColor` 필요한 크기의 배열을 할당 하지만, 속성을 사용 `Pixels` 하 여 해당 배열을 만들 수 있습니다.

```csharp
SKBitmap FillBitmapPixelsProp(out string description, out int milliseconds)
{
    description = "Pixels property";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    SKColor[] pixels = new SKColor[256 * 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                pixels[256 * row + col] = new SKColor((byte)col, 0, (byte)row);
            }

    bitmap.Pixels = pixels;

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

배열의 인덱스는 `pixels` 및 변수에서 계산 해야 `row` `col` 합니다. 행에 각 행의 픽셀 수 (이 경우 256)를 곱한 다음 열이 추가 됩니다.

`SKBitmap`는 또한 `Bytes` 전체 비트맵의 바이트 배열을 반환 하는 유사한 속성을 정의 하지만 전체 색 비트맵의 경우 더 복잡 합니다.

### <a name="the-getpixels-pointer"></a>GetPixels 포인터

비트맵 픽셀에 액세스할 수 있는 가장 강력한 방법은 [`GetPixels`](xref:SkiaSharp.SKBitmap.GetPixels) 메서드 또는 속성과 혼동 하지 않는 것입니다 `GetPixel` `Pixels` . `GetPixels`C # 프로그래밍에서 그다지 일반적이 지 않은 것을 반환 한다는 점에서와의 차이점을 즉시 알 수 있습니다.

```csharp
IntPtr pixelsAddr = bitmap.GetPixels();
```

.NET [`IntPtr`](xref:System.IntPtr) 형식은 포인터를 나타냅니다. `IntPtr`프로그램이 실행 되는 컴퓨터의 네이티브 프로세서에서의 정수 길이 이기 때문에 호출 됩니다 (일반적으로 32 비트 또는 64 비트 길이). 가 반환 되는는 `IntPtr` `GetPixels` 비트맵 개체가 픽셀을 저장 하는 데 사용 하는 실제 메모리 블록의 주소입니다.

`IntPtr`메서드를 사용 하 여를 c # 포인터 형식으로 변환할 수 있습니다 [`ToPointer`](xref:System.IntPtr.ToPointer) . C # 포인터 구문은 C 및 c + +와 동일 합니다.

```csharp
byte* ptr = (byte*)pixelsAddr.ToPointer();
```

`ptr`변수는 _바이트 포인터_유형입니다. 이 `ptr` 변수를 사용 하면 비트맵의 픽셀을 저장 하는 데 사용 되는 개별 메모리 바이트에 액세스할 수 있습니다. 다음과 같은 코드를 사용 하 여이 메모리에서 바이트를 읽거나 메모리에 바이트를 씁니다.

```csharp
byte pixelComponent = *ptr;

*ptr = pixelComponent;
```

이 컨텍스트에서 별표는 c # _간접 연산자_ 이 고가 가리키는 메모리의 콘텐츠를 참조 하는 데 사용 됩니다 `ptr` . 처음에는 비트맵 첫 번째 행의 첫 번째 픽셀에 대 한 첫 번째 `ptr` 바이트를 가리키며 변수에 대 한 산술 연산을 수행 `ptr` 하 여 비트맵 내의 다른 위치로 이동할 수 있습니다.

한 가지 단점은 `ptr` 키워드로 표시 된 코드 블록 에서만이 변수를 사용할 수 있다는 것입니다 `unsafe` . 또한 어셈블리는 안전 하지 않은 블록을 허용 하도록 플래그를 지정 해야 합니다. 이 작업은 프로젝트의 속성에서 수행 됩니다.

C #에서 포인터를 사용 하는 것은 매우 강력 하지만 매우 위험 합니다. 포인터가 참조 하는 것 이상의 메모리에 액세스 하지 않도록 주의 해야 합니다. 포인터 사용이 "unsafe" 라는 단어와 연결 되는 이유입니다.

다음은 `GradientBitmapPage` 메서드를 사용 하는 클래스의 메서드입니다 `GetPixels` . `unsafe`바이트 포인터를 사용 하 여 모든 코드를 포함 하는 블록을 확인 합니다.

```csharp
SKBitmap FillBitmapBytePtr(out string description, out int milliseconds)
{
    description = "GetPixels byte ptr";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    IntPtr pixelsAddr = bitmap.GetPixels();

    unsafe
    {
        for (int rep = 0; rep < REPS; rep++)
        {
            byte* ptr = (byte*)pixelsAddr.ToPointer();

            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    *ptr++ = (byte)(col);   // red
                    *ptr++ = 0;             // green
                    *ptr++ = (byte)(row);   // blue
                    *ptr++ = 0xFF;          // alpha
                }
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

변수를 `ptr` 메서드에서 처음 가져오는 경우이 변수는 `ToPointer` 비트맵 첫 번째 행의 맨 왼쪽 픽셀에 대 한 첫 번째 바이트를 가리킵니다. `for`및에 대 한 루프는 `row` `col` `ptr` `++` 각 픽셀의 각 바이트가 설정 된 후 연산자를 사용 하 여 증가 시킬 수 있도록 설정 됩니다. 픽셀을 통한 다른 99 루프의 경우를 `ptr` 비트맵의 시작 부분으로 다시 설정 해야 합니다.

각 픽셀은 4 바이트의 메모리 이므로 각 바이트를 별도로 설정 해야 합니다. 이 코드에서는 해당 바이트가 색 유형과 일치 하는 빨강, 녹색, 파랑 및 알파 순서로 되어 있다고 가정 합니다 `SKColorType.Rgba8888` . 이는 iOS 및 Android의 기본 색 유형 이지만 유니버설 Windows 플랫폼에 대 한 것은 아닙니다. 기본적으로 UWP는 색 형식을 사용 하 여 비트맵을 만듭니다 `SKColorType.Bgra8888` . 이러한 이유로 해당 플랫폼에서 다른 결과가 표시 될 것입니다.

에서 반환 된 값을 포인터가 아닌 포인터로 캐스팅할 수 있습니다 `ToPointer` `uint` `byte` . 이를 통해 한 문에서 전체 픽셀에 액세스할 수 있습니다. 연산자를 `++` 해당 포인터에 적용 하면 다음 픽셀을 가리키도록 4 바이트로 증가 합니다.

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    SKBitmap FillBitmapUintPtr(out string description, out int milliseconds)
    {
        description = "GetPixels uint ptr";
        SKBitmap bitmap = new SKBitmap(256, 256);

        stopwatch.Restart();

        IntPtr pixelsAddr = bitmap.GetPixels();

        unsafe
        {
            for (int rep = 0; rep < REPS; rep++)
            {
                uint* ptr = (uint*)pixelsAddr.ToPointer();

                for (int row = 0; row < 256; row++)
                    for (int col = 0; col < 256; col++)
                    {
                        *ptr++ = MakePixel((byte)col, 0, (byte)row, 0xFF);
                    }
            }
        }

        milliseconds = (int)stopwatch.ElapsedMilliseconds;
        return bitmap;
    }
    ···
    [MethodImpl(MethodImplOptions.AggressiveInlining)]
    uint MakePixel(byte red, byte green, byte blue, byte alpha) =>
            (uint)((alpha << 24) | (blue << 16) | (green << 8) | red);
    ···
}
```

픽셀은 `MakePixel` 빨간색, 녹색, 파랑 및 알파 구성 요소에서 정수 픽셀을 생성 하는 메서드를 사용 하 여 설정 됩니다. `SKColorType.Rgba8888`형식은 다음과 같이 픽셀 바이트 순서를 갖습니다.

RR GG BB AA

그러나 해당 바이트에 해당 하는 정수는 다음과 같습니다.

AABBGGRR

정수의 최하위 바이트는 먼저 작은 endian 아키텍처에 따라 저장 됩니다. 이 `MakePixel` 메서드는 색 형식의 비트맵에서는 제대로 작동 하지 않습니다 `Bgra8888` .

`MakePixel`메서드는 [`MethodImplOptions.AggressiveInlining`](xref:System.Runtime.CompilerServices.MethodImplOptions) 별도의 메서드로 설정 하지 않고 메서드가 호출 되는 코드를 컴파일하는 것을 방지 하기 위해 컴파일러를 권장 하는 옵션으로 플래그가 지정 됩니다. 이렇게 하면 성능이 향상 됩니다.

흥미롭게도 구조체는 `SKColor` 에서 부호 없는 정수로의 명시적 변환을 정의 합니다 `SKColor` . 즉, `SKColor` 값을 만들 수 있고 `uint` 대신로의 변환을 사용할 수 있습니다 `MakePixel` .

```csharp
SKBitmap FillBitmapUintPtrColor(out string description, out int milliseconds)
{
    description = "GetPixels SKColor";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    IntPtr pixelsAddr = bitmap.GetPixels();

    unsafe
    {
        for (int rep = 0; rep < REPS; rep++)
        {
            uint* ptr = (uint*)pixelsAddr.ToPointer();

            for (int row = 0; row < 256; row++)
                for (int col = 0; col < 256; col++)
                {
                    *ptr++ = (uint)new SKColor((byte)col, 0, (byte)row);
                }
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

유일한 질문은 `SKColor` `SKColorType.Rgba8888` 색 유형 또는 색 유형 순서로 값의 정수 형식 `SKColorType.Bgra8888` 이거나 완전히 다른 것 입니까?입니다. 이 질문에 대 한 답은 곧 표시 될 것입니다.

### <a name="the-setpixels-method"></a>SetPixels 메서드

`SKBitmap`는 [`SetPixels`](xref:SkiaSharp.SKBitmap.SetPixels(System.IntPtr)) 다음과 같이 호출 하는 라는 메서드도 정의 합니다.

```csharp
bitmap.SetPixels(intPtr);
```

회수할 때 `GetPixels` `IntPtr` 비트맵에서 픽셀을 저장 하는 데 사용 하는 메모리 블록을 참조 합니다. `SetPixels`호출은 지정 된에서 참조 하는 메모리 블록을 인수로 사용 하 여 메모리 블록을 _바꿉니다_ `IntPtr` `SetPixels` . 그런 다음 비트맵은 이전에 사용 된 메모리 블록을 해제 합니다. 다음에 호출 되 면를 사용 하 여 `GetPixels` 메모리 블록 집합을 가져옵니다 `SetPixels` .

처음에 `SetPixels` 는가 그다지 편리 하지 않을 때 보다 더 많은 기능과 성능을 제공 하지 않는 것 처럼 보입니다 `GetPixels` . 를 사용 `GetPixels` 하면 비트맵 메모리 블록을 가져와 액세스할 수 있습니다. 를 사용 하 여 `SetPixels` 일부 메모리를 할당 하 고 액세스 한 다음이를 비트맵 메모리 블록으로 설정 합니다.

그러나를 사용 하면 `SetPixels` 고유한 구문상 이점이 있습니다. 배열을 사용 하 여 비트맵 픽셀 비트에 액세스할 수 있습니다. 이 기술을 보여 주는의 메서드는 다음과 같습니다 `GradientBitmapPage` . 메서드는 먼저 비트맵 픽셀의 바이트에 해당 하는 다차원 바이트 배열을 정의 합니다. 첫 번째 차원은 행이 고, 두 번째 차원은 열 이며, 세 번째 차원은 각 픽셀의 네 가지 구성 요소로 해당 됩니다.

```csharp
SKBitmap FillBitmapByteBuffer(out string description, out int milliseconds)
{
    description = "SetPixels byte buffer";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    byte[,,] buffer = new byte[256, 256, 4];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col, 0] = (byte)col;   // red
                buffer[row, col, 1] = 0;           // green
                buffer[row, col, 2] = (byte)row;   // blue
                buffer[row, col, 3] = 0xFF;        // alpha
            }

    unsafe
    {
        fixed (byte* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

그런 다음 배열을 픽셀로 채운 후 `unsafe` 블록 및 `fixed` 문이이 배열을 가리키는 바이트 포인터를 가져오는 데 사용 됩니다. 그런 다음 해당 바이트 포인터를로 캐스팅 하 여 `IntPtr` 에 전달할 수 있습니다 `SetPixels` .

사용자가 만드는 배열은 바이트 배열이 아니어도 됩니다. 행과 열에 대해 두 개의 차원만 있는 정수 배열일 수 있습니다.

```csharp
SKBitmap FillBitmapUintBuffer(out string description, out int milliseconds)
{
    description = "SetPixels uint buffer";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    uint[,] buffer = new uint[256, 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col] = MakePixel((byte)col, 0, (byte)row, 0xFF);
            }

    unsafe
    {
        fixed (uint* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

`MakePixel`메서드는 색 구성 요소를 32 비트 픽셀에 결합 하는 데 다시 사용 됩니다.

완전성을 위해 다음과 같은 코드가 있지만 `SKColor` 부호 없는 정수로 캐스팅 된 값이 있습니다.

```csharp
SKBitmap FillBitmapUintBufferColor(out string description, out int milliseconds)
{
    description = "SetPixels SKColor";
    SKBitmap bitmap = new SKBitmap(256, 256);

    stopwatch.Restart();

    uint[,] buffer = new uint[256, 256];

    for (int rep = 0; rep < REPS; rep++)
        for (int row = 0; row < 256; row++)
            for (int col = 0; col < 256; col++)
            {
                buffer[row, col] = (uint)new SKColor((byte)col, 0, (byte)row);
            }

    unsafe
    {
        fixed (uint* ptr = buffer)
        {
            bitmap.SetPixels((IntPtr)ptr);
        }
    }

    milliseconds = (int)stopwatch.ElapsedMilliseconds;
    return bitmap;
}
```

### <a name="comparing-the-techniques"></a>기술 비교

**그라데이션 색** 페이지의 생성자는 위에 표시 된 8 개의 메서드를 모두 호출 하 고 결과를 저장 합니다.

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    string[] descriptions = new string[8];
    SKBitmap[] bitmaps = new SKBitmap[8];
    int[] elapsedTimes = new int[8];

    SKCanvasView canvasView;

    public GradientBitmapPage ()
    {
        Title = "Gradient Bitmap";

        bitmaps[0] = FillBitmapSetPixel(out descriptions[0], out elapsedTimes[0]);
        bitmaps[1] = FillBitmapPixelsProp(out descriptions[1], out elapsedTimes[1]);
        bitmaps[2] = FillBitmapBytePtr(out descriptions[2], out elapsedTimes[2]);
        bitmaps[4] = FillBitmapUintPtr(out descriptions[4], out elapsedTimes[4]);
        bitmaps[6] = FillBitmapUintPtrColor(out descriptions[6], out elapsedTimes[6]);
        bitmaps[3] = FillBitmapByteBuffer(out descriptions[3], out elapsedTimes[3]);
        bitmaps[5] = FillBitmapUintBuffer(out descriptions[5], out elapsedTimes[5]);
        bitmaps[7] = FillBitmapUintBufferColor(out descriptions[7], out elapsedTimes[7]);

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ···
}
```

생성자는 `SKCanvasView` 결과 비트맵을 표시 하기 위해를 만들어 종료 합니다. `PaintSurface`처리기는 해당 표면을 8 개의 사각형으로 나누고 `Display` 를 호출 하 여 각 항목을 표시 합니다.

```csharp
public class GradientBitmapPage : ContentPage
{
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        int width = info.Width;
        int height = info.Height;

        canvas.Clear();

        Display(canvas, 0, new SKRect(0, 0, width / 2, height / 4));
        Display(canvas, 1, new SKRect(width / 2, 0, width, height / 4));
        Display(canvas, 2, new SKRect(0, height / 4, width / 2, 2 * height / 4));
        Display(canvas, 3, new SKRect(width / 2, height / 4, width, 2 * height / 4));
        Display(canvas, 4, new SKRect(0, 2 * height / 4, width / 2, 3 * height / 4));
        Display(canvas, 5, new SKRect(width / 2, 2 * height / 4, width, 3 * height / 4));
        Display(canvas, 6, new SKRect(0, 3 * height / 4, width / 2, height));
        Display(canvas, 7, new SKRect(width / 2, 3 * height / 4, width, height));
    }

    void Display(SKCanvas canvas, int index, SKRect rect)
    {
        string text = String.Format("{0}: {1:F1} msec", descriptions[index],
                                    (double)elapsedTimes[index] / REPS);

        SKRect bounds = new SKRect();

        using (SKPaint textPaint = new SKPaint())
        {
            textPaint.TextSize = (float)(12 * canvasView.CanvasSize.Width / canvasView.Width);
            textPaint.TextAlign = SKTextAlign.Center;
            textPaint.MeasureText("Tly", ref bounds);

            canvas.DrawText(text, new SKPoint(rect.MidX, rect.Bottom - bounds.Bottom), textPaint);
            rect.Bottom -= bounds.Height;
            canvas.DrawBitmap(bitmaps[index], rect, BitmapStretch.Uniform);
        }
    }
}
```

컴파일러가 코드를 최적화 하는 것을 허용 하기 위해이 페이지는 **릴리스** 모드에서 실행 되었습니다. MacBook Pro의 iPhone 8 시뮬레이터에서 실행 되는 페이지는 Nexus 5 Android 휴대폰 및 Windows 10을 실행 하는 Surface Pro 3입니다. 하드웨어 차이로 인해 장치 간의 성능 시간을 비교 하지 말고 각 장치에서 상대적 시간을 확인 하십시오.

[![그라데이션 비트맵](pixel-bits-images/GradientBitmap.png "그라데이션 비트맵")](pixel-bits-images/GradientBitmap-Large.png#lightbox)

실행 시간 (밀리초)을 통합 하는 테이블은 다음과 같습니다.

| API       | 데이터 형식 | iOS  | Android | UWP  |
| --------- | --------- | ----:| -------:| ----:|
| SetPixel  |           | 3.17 |   10.77 | 3.49 |
| 픽셀    |           | 0.32 |    1.23 | 0.07 |
| GetPixels | byte      | 0.09 |    0.24 | 0.10 |
|           | uint      | 0.06 |    0.26 | 0.05 |
|           | 고 색   | 0.29 |    0.99 | 0.07 |
| SetPixels | byte      | 1.33 |    6.78 | 0.11 |
|           | uint      | 0.14 |    0.69 | 0.06 |
|           | 고 색   | 0.35 |    1.93 | 0.10 |

예상 대로 `SetPixel` 65536 시간을 호출 하는 것이 비트맵의 픽셀을 설정 하는 가장 effeicient 방법입니다. 배열을 채우거 `SKColor` 나 속성을 설정 하 `Pixels` 는 것이 훨씬 더 낫지만 잘와 일부 및 기법을 비교 하기도 `GetPixels` `SetPixels` 합니다. 픽셀 값을 사용 하 `uint` 는 것은 일반적으로 별도의 구성 요소를 설정 하는 것 보다 빠르지만 `byte` 값을 `SKColor` 부호 없는 정수로 변환 하면 프로세스에 약간의 오버 헤드가 추가 됩니다.

또한 다양 한 그라데이션을 비교 하는 것이 흥미롭습니다. 각 플랫폼의 위쪽 행은 동일 하 고 의도 한 대로 그라데이션을 표시 합니다. 즉, `SetPixel` 메서드와 `Pixels` 속성은 기본 픽셀 형식에 관계 없이 색에서 픽셀을 올바르게 만듭니다.

IOS 및 Android 스크린샷의 다음 두 행도 동일 합니다 `MakePixel` .이는 작은 메서드가 `Rgba8888` 이러한 플랫폼의 기본 픽셀 형식에 대해 올바르게 정의 되어 있음을 확인 합니다.

IOS 및 Android 스크린샷 맨 아래 행은 역방향 이며,이는 값을 캐스팅 하 여 얻은 부호 없는 정수가 `SKColor` 형식 임을 나타냅니다.

AARRGGBB

바이트의 순서는 다음과 같습니다.

BB GG RR AA

`Bgra8888`순서가 아닌 순서입니다 `Rgba8888` . `Brga8888`형식은 유니버설 Windows 플랫폼에 대 한 기본값입니다 .이 경우 해당 스크린 샷의 마지막 행에 있는 그라데이션은 첫 번째 행과 동일 합니다. 그러나 이러한 비트맵을 만드는 코드에서 순서를 가정 하기 때문에 가운데 두 행은 올바르지 않습니다 `Rgba8888` .

각 플랫폼의 픽셀 비트에 액세스 하는 데 동일한 코드를 사용 하려는 경우 `SKBitmap` 또는 형식을 사용 하 여을 명시적으로 만들 수 있습니다 `Rgba8888` `Bgra8888` . 값을 비트맵 픽셀로 캐스팅 하려면를 `SKColor` 사용 `Bgra8888` 합니다.

## <a name="random-access-of-pixels"></a>픽셀의 임의 액세스

`FillBitmapBytePtr` `FillBitmapUintPtr` **그라데이션 비트맵** 페이지의 및 메서드는 `for` 위쪽 행에서 아래쪽 행으로, 각 행에서 왼쪽에서 오른쪽으로 비트맵을 순차적으로 채우도록 디자인 된 루프의 혜택을 활용할 수 있습니다. 픽셀은 포인터를 증가 시키는 동일한 문으로 설정할 수 있습니다.

경우에 따라 픽셀에 순차적으로 액세스 해야 하는 경우가 있습니다. 이 방법을 사용 하는 경우 `GetPixels` 행과 열을 기준으로 포인터를 계산 해야 합니다. 이는 사인 곡선의 한 주기 형태로 레인을 표시 하는 비트맵을 만드는 **레인 사인** 페이지에서 보여 줍니다.

무지개 색은 HSL (색상, 채도, 광도) 색 모델을 사용 하 여 만드는 것이 가장 쉽습니다. `SKColor.FromHsl`이 메서드는 `SKColor` 0 ~ 360 범위의 색조 값 (원의 각도와 같이 빨간색, 녹색, 파랑, 빨강으로 이동)을 사용 하 여 값을 만듭니다 .이 값은 0 ~ 100 범위의 채도 및 광도 값입니다. 무지개 색의 경우 채도는 최대 100로, 명도는 50의 중간점으로 설정 해야 합니다.

**레인 사인** 은 비트맵의 행을 반복 하 고 360 색상 값을 반복 하 여이 이미지를 만듭니다. 각 색상 값에서 다음과 같이 사인 값을 기반으로 하는 비트맵 열도 계산 합니다.

```csharp
public class RainbowSinePage : ContentPage
{
    SKBitmap bitmap;

    public RainbowSinePage()
    {
        Title = "Rainbow Sine";

        bitmap = new SKBitmap(360 * 3, 1024, SKColorType.Bgra8888, SKAlphaType.Unpremul);

        unsafe
        {
            // Pointer to first pixel of bitmap
            uint* basePtr = (uint*)bitmap.GetPixels().ToPointer();

            // Loop through the rows
            for (int row = 0; row < bitmap.Height; row++)
            {
                // Calculate the sine curve angle and the sine value
                double angle = 2 * Math.PI * row / bitmap.Height;
                double sine = Math.Sin(angle);

                // Loop through the hues
                for (int hue = 0; hue < 360; hue++)
                {
                    // Calculate the column
                    int col = (int)(360 + 360 * sine + hue);

                    // Calculate the address
                    uint* ptr = basePtr + bitmap.Width * row + col;

                    // Store the color value
                    *ptr = (uint)SKColor.FromHsl(hue, 100, 50);
                }
            }
        }

        // Create the SKCanvasView
        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(bitmap, info.Rect);
    }
}
```

생성자는 다음 형식에 따라 비트맵을 만듭니다 `SKColorType.Bgra8888` .

```csharp
bitmap = new SKBitmap(360 * 3, 1024, SKColorType.Bgra8888, SKAlphaType.Unpremul);
```

이를 통해 프로그램은 `SKColor` 걱정 없이 값을 픽셀로 변환 하는 데 사용할 수 있습니다 `uint` . 이 특정 프로그램에서 역할을 수행 하지는 않지만, `SKColor` 픽셀을 설정 하기 위해 변환을 사용할 때마다에서 `SKAlphaType.Unpremul` `SKColor` 알파 값으로 색 구성 요소를 미리 곱하여 하지 않기 때문에를 지정 해야 합니다.

그런 다음 생성자는 메서드를 사용 하 여 `GetPixels` 비트맵의 첫 번째 픽셀에 대 한 포인터를 가져옵니다.

```csharp
uint* basePtr = (uint*)bitmap.GetPixels().ToPointer();
```

특정 행과 열에 대해 오프셋 값을에 추가 해야 합니다 `basePtr` . 이 오프셋은 비트맵 너비의 행 시간과 열을 더한 값입니다.

```csharp
uint* ptr = basePtr + bitmap.Width * row + col;
```

`SKColor`이 값은 다음 포인터를 사용 하 여 메모리에 저장 됩니다.

```csharp
*ptr = (uint)SKColor.FromHsl(hue, 100, 50);
```

`PaintSurface`의 처리기에서 `SKCanvasView` 비트맵은 표시 영역을 채우도록 확장 됩니다.

[![무지개 사인](pixel-bits-images/RainbowSine.png "무지개 사인")](pixel-bits-images/RainbowSine-Large.png#lightbox)

## <a name="from-one-bitmap-to-another"></a>한 비트맵에서 다른 비트맵으로

많은 이미지 처리 작업에는 한 비트맵에서 다른 비트맵으로 전송 되는 픽셀 수정이 포함 됩니다. 이 기법은 **색 조정** 페이지에서 보여 줍니다. 페이지에서 비트맵 리소스 중 하나를 로드 한 다음 세 가지 보기를 사용 하 여 이미지를 수정할 수 있습니다 `Slider` .

[![색 조정](pixel-bits-images/ColorAdjustment.png "색 조정")](pixel-bits-images/ColorAdjustment-Large.png#lightbox)

각 픽셀 색에 대해 첫 번째는 `Slider` 0에서 360 사이의 값을 색에 추가 하지만, 그 다음에는 모듈로 연산자를 사용 하 여 0과 360 사이의 결과를 유지 합니다 .이 경우 UWP 스크린샷에서 보여 주는 것 처럼 스펙트럼을 따라 색이 효과적으로 이동 합니다. 두 번째 `Slider` 방법을 사용 하면 0.5과 2 사이의 곱하기 요소를 선택 하 여 채도에 적용 하 고, 세 번째는 `Slider` Android 스크린 샷에 표시 된 것 처럼 광도에 대해 동일 하 게 수행 됩니다.

프로그램은 라는 원래 원본 비트맵과 이라는 조정 된 대상 비트맵 이라는 두 개의 비트맵을 유지 관리 합니다 `srcBitmap` `dstBitmap` . `Slider`이 이동 될 때마다 프로그램은에서 새 픽셀을 모두 계산 `dstBitmap` 합니다. 물론 사용자는 보기를 매우 빠르게 이동 하 여 시험해 볼 `Slider` 수 있으므로 최상의 성능을 관리할 수 있습니다. 여기에는 `GetPixels` 원본 및 대상 비트맵 모두에 대 한 메서드가 포함 됩니다.

**색 조정** 페이지는 원본 및 대상 비트맵의 색 형식을 제어 하지 않습니다. 대신 및 형식에 대해 약간 다른 논리를 포함 `SKColorType.Rgba8888` `SKColorType.Bgra8888` 합니다. 원본 및 대상의 형식은 서로 다를 수 있으며 프로그램은 계속 작동 합니다.

`TransferPixels`소스를 대상으로 하는 픽셀을 전송 하는 중요 한 메서드를 제외한 프로그램은 다음과 같습니다. 생성자는 `dstBitmap` 와 동일 하 게 설정 `srcBitmap` 됩니다. `PaintSurface`처리기는 다음을 표시 합니다 `dstBitmap` .

```csharp
public partial class ColorAdjustmentPage : ContentPage
{
    SKBitmap srcBitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    SKBitmap dstBitmap;

    public ColorAdjustmentPage()
    {
        InitializeComponent();

        dstBitmap = new SKBitmap(srcBitmap.Width, srcBitmap.Height);
        OnSliderValueChanged(null, null);
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        float hueAdjust = (float)hueSlider.Value;
        hueLabel.Text = $"Hue Adjustment: {hueAdjust:F0}";

        float saturationAdjust = (float)Math.Pow(2, saturationSlider.Value);
        saturationLabel.Text = $"Saturation Adjustment: {saturationAdjust:F2}";

        float luminosityAdjust = (float)Math.Pow(2, luminositySlider.Value);
        luminosityLabel.Text = $"Luminosity Adjustment: {luminosityAdjust:F2}";

        TransferPixels(hueAdjust, saturationAdjust, luminosityAdjust);
        canvasView.InvalidateSurface();
    }
    ···
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(dstBitmap, info.Rect, BitmapStretch.Uniform);
    }
}
```

`ValueChanged`뷰의 처리기는 `Slider` 조정 값 및 호출을 계산 합니다 `TransferPixels` .

전체 `TransferPixels` 메서드는로 표시 됩니다 `unsafe` . 먼저 두 비트맵의 픽셀 비트에 대 한 바이트 포인터를 가져온 다음 모든 행과 열을 반복 합니다. 소스 비트맵에서 메서드는 각 픽셀에 대해 4 바이트를 가져옵니다. 이러한 경우는 또는 순서 중 하나일 수 있습니다 `Rgba8888` `Bgra8888` . 색 형식을 확인 하면 `SKColor` 값을 만들 수 있습니다. 그런 다음 HSL 구성 요소를 추출 하 고 조정 하 여 값을 다시 만드는 데 사용 `SKColor` 합니다. 대상 비트맵이 또는 인지에 따라 `Rgba8888` 바이트는 `Bgra8888` 대상 bitmp에 저장 됩니다.

```csharp
public partial class ColorAdjustmentPage : ContentPage
{
    ···
    unsafe void TransferPixels(float hueAdjust, float saturationAdjust, float luminosityAdjust)
    {
        byte* srcPtr = (byte*)srcBitmap.GetPixels().ToPointer();
        byte* dstPtr = (byte*)dstBitmap.GetPixels().ToPointer();

        int width = srcBitmap.Width;       // same for both bitmaps
        int height = srcBitmap.Height;

        SKColorType typeOrg = srcBitmap.ColorType;
        SKColorType typeAdj = dstBitmap.ColorType;

        for (int row = 0; row < height; row++)
        {
            for (int col = 0; col < width; col++)
            {
                // Get color from original bitmap
                byte byte1 = *srcPtr++;         // red or blue
                byte byte2 = *srcPtr++;         // green
                byte byte3 = *srcPtr++;         // blue or red
                byte byte4 = *srcPtr++;         // alpha

                SKColor color = new SKColor();

                if (typeOrg == SKColorType.Rgba8888)
                {
                    color = new SKColor(byte1, byte2, byte3, byte4);
                }
                else if (typeOrg == SKColorType.Bgra8888)
                {
                    color = new SKColor(byte3, byte2, byte1, byte4);
                }

                // Get HSL components
                color.ToHsl(out float hue, out float saturation, out float luminosity);

                // Adjust HSL components based on adjustments
                hue = (hue + hueAdjust) % 360;
                saturation = Math.Max(0, Math.Min(100, saturationAdjust * saturation));
                luminosity = Math.Max(0, Math.Min(100, luminosityAdjust * luminosity));

                // Recreate color from HSL components
                color = SKColor.FromHsl(hue, saturation, luminosity);

                // Store the bytes in the adjusted bitmap
                if (typeAdj == SKColorType.Rgba8888)
                {
                    *dstPtr++ = color.Red;
                    *dstPtr++ = color.Green;
                    *dstPtr++ = color.Blue;
                    *dstPtr++ = color.Alpha;
                }
                else if (typeAdj == SKColorType.Bgra8888)
                {
                    *dstPtr++ = color.Blue;
                    *dstPtr++ = color.Green;
                    *dstPtr++ = color.Red;
                    *dstPtr++ = color.Alpha;
                }
            }
        }
    }
    ···
}
```

원본 및 대상 비트맵의 다양 한 색 형식 조합에 대해 별도의 메서드를 만들고 모든 픽셀에 대 한 형식을 확인 하는 것을 방지 하 여이 메서드의 성능이 훨씬 향상 될 수 있습니다. 또 다른 옵션은 `for` `col` 색 유형을 기반으로 변수에 대 한 여러 루프를 사용 하는 것입니다.

## <a name="posterization"></a>Posterization

픽셀 비트에 대 한 액세스를 포함 하는 또 다른 일반적인 작업은 _posterization_입니다. 비트맵이 제한 된 색상표를 사용 하 여 손으로 그린 포스터와 비슷한 결과를 얻을 수 있도록 비트맵의 픽셀에서 인코딩된 색이 감소 하는 경우입니다.

**포스터화** 페이지는 원숭이 이미지 중 하나에서이 프로세스를 수행 합니다.

```csharp
public class PosterizePage : ContentPage
{
    SKBitmap bitmap =
        BitmapExtensions.LoadBitmapResource(typeof(FillRectanglePage),
                                            "SkiaSharpFormsDemos.Media.Banana.jpg");
    public PosterizePage()
    {
        Title = "Posterize";

        unsafe
        {
            uint* ptr = (uint*)bitmap.GetPixels().ToPointer();
            int pixelCount = bitmap.Width * bitmap.Height;

            for (int i = 0; i < pixelCount; i++)
            {
                *ptr++ &= 0xE0E0E0FF;
            }
        }

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform;
    }
}
```

생성자의 코드는 각 픽셀에 액세스 하 고 0xE0E0E0FF 값을 사용 하 여 비트 AND 연산을 수행한 다음 결과를 다시 비트맵에 저장 합니다. 0xE0E0E0FF 값은 각 색 구성 요소의 상위 3 비트를 유지 하 고 하위 5 비트를 0으로 설정 합니다. 2<sup>24</sup> 또는 16777216 색 대신 비트맵이 2<sup>9</sup> 또는 512 색으로 줄어듭니다.

[![포스터](pixel-bits-images/Posterize.png "포스터")](pixel-bits-images/Posterize-Large.png#lightbox)

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
