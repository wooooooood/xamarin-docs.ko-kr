---
title: SkiaSharp 픽셀 비트에 액세스
description: 액세스 하 고 SkiaSharp 비트맵의 픽셀 비트를 수정 하는 다양 한 기술을 검색 합니다.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DBB58522-F816-4A8C-96A5-E0236F16A5C6
author: charlespetzold
ms.author: chape
ms.date: 07/11/2018
ms.openlocfilehash: 51252a1c18602c704b87f7b01efe7cc5a7549ac1
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39131478"
---
# <a name="accessing-skiasharp-pixel-bits"></a>SkiaSharp 픽셀 비트에 액세스

이 문서에서 볼 수 있듯이 [ **파일에 저장 SkiaSharp 비트맵**](saving.md), 비트맵은 일반적으로 압축 된 형식으로 JPEG 또는 PNG와 같은 파일에 저장 됩니다. 이와 달리 SkiaSharp 비트맵 메모리에 저장 된 압축 되지 않습니다. 픽셀의 순차적으로 저장 됩니다. 압축 되지 않은 형식으로 표시 화면에 비트맵의 전송을 지원합니다.

SkiaSharp 비트맵을 차지 하는 메모리 블록은 아주 간단 하 게 구성 됩니다: 왼쪽에서 오른쪽으로 까지의 픽셀의 첫 번째 행을 시작 하 고 두 번째 행을 사용 하 여 계속 합니다. 전체 색 비트맵에 대 한 각 픽셀 이루어져 있습니다 4 바이트 비트맵에 필요한 총 메모리 공간 4 번의 너비와 높이의 제품 임을 의미입니다.

이 문서에서는 응용 프로그램에서 비트맵의 픽셀 메모리 블록에 액세스 하 여 간접적으로 이러한 픽셀에 대 한 액세스를 가져올 수는 방법에 대해 설명 합니다. 또는 간접적으로 합니다. 일부 경우에 프로그램 하려는 이미지의 픽셀을 분석 하 고 일종의 히스토그램을 생성 합니다. 일반적으로 응용 프로그램은 비트맵을 구성 하는 픽셀 알고리즘 방식으로 만들어 고유 이미지를 생성할 수 있습니다.

![픽셀 비트 샘플](pixel-bits-images/PixelBitsSample.png "픽셀 비트 샘플")

## <a name="the-techniques"></a>기술

SkiaSharp는 비트맵의 픽셀 비트에 액세스 하기 위한 몇 가지 방법을 제공 합니다. 어떤 것을 선택은 일반적으로 코딩 (관련이 유지 관리 및 디버깅 편의성)는 편 및 성능 절충 합니다. 대부분에서의 속성과 다음 방법 중 하나를 사용 하겠습니다 `SKBitmap` 비트맵의 픽셀에 액세스 합니다.

- 합니다 `GetPixel` 고 `SetPixel` 메서드 얻거나 단일 픽셀의 색을 설정할 수 있습니다.
- `Pixels` 속성은 전체 비트맵의 픽셀 색의 배열을 가져오는 또는 색의 배열을 설정 합니다.
- `GetPixels` 비트맵을 사용 하는 픽셀 메모리의 주소를 반환 합니다.
- `SetPixels` 비트맵을 사용 하는 픽셀 메모리의 주소를 바꿉니다.

"상위 수준"으로 처음 두 기술 및 두 "낮은 수준입니다."로 생각할 수 있습니다. 일부 다른 메서드 및 속성을 사용할 수 있는 있지만 이들은 가장 유용 합니다.

이러한 기술 간의 성능 차이 볼 수 있도록 하는 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) 라는 페이지를 포함 하는 응용 프로그램 **그라데이션 비트맵** 는 그라데이션을 만드는 데 빨간색, 파란색 음영을 결합 하는 픽셀을 사용 하 여 비트맵을 만듭니다. 프로그램 8 개의 다른 복사본을 만들어이 비트맵의 모든 기술을 사용 하 여 다른 비트맵 픽셀을 설정 합니다. 이러한 8 비트맵의 각 기술의 대 한 간략 한 텍스트 설명을 설정 하 고 모든 픽셀을 설정 하는 데 필요한 시간을 계산 하는 별도 메서드에서 만들어집니다. 각 메서드 반복 픽셀 설정 논리를 100 번 하 게 더 나은 성능 예측을 합니다. 

### <a name="the-setpixel-method"></a>SetPixel 메서드

해야 할 경우만 여러 개별 픽셀을 가져오거나 설정 합니다 [ `SetPixel` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.SetPixel/p/System.Int32/System.Int32/SkiaSharp.SKColor/) 및 [ `GetPixel` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.GetPixel/p/System.Int32/System.Int32/) 방법이 적합 합니다. 이러한 두 가지 방법 중 각각에 대 한 정수 열 및 행을 지정합니다. 픽셀 형식에 관계 없이 이러한 두 가지 방법 있습니다 얻거나 설정으로 픽셀을 `SKColor` 값:

```csharp
bitmap.SetPixel(col, row, color);

SKColor color = bitmap.GetPixel(col, row);
```

`col` 인수 해야 범위는 0에서 1 보다 작은 `Width` 비트맵의 속성 및 `row` 하나에 대 한 범위는 0 보다 작은 `Height` 속성입니다.

여기에 메서드가 **그라데이션 비트맵** 사용 하 여 비트맵 콘텐츠를 설정 하는 `SetPixel` 메서드. 비트맵은 256 x 256 픽셀 및 `for` 루프 값의 범위를 사용 하 여 하드 코드 됩니다.

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

각 픽셀에 대해 설정 된 색상에는 빨강 구성 요소와 같은 비트맵 열 및 행을 파랑 구성 요소와 같은 있습니다. 결과 비트맵은 왼쪽 위, 오른쪽 위, 아래 왼쪽 및 오른쪽 아래에 그라데이션으로 다른 곳에서 자홍 파란색에서 빨간색 검정입니다.

`SetPixel` 메서드를 호출한 65,536 시간에 관계 없이 효율성이 메서드가 될 수 있습니다, 아니기 일반적으로 대 안으로 사용할 수 있는 경우 많은 API 호출을 수행 하는 것이 좋습니다. 다행 스럽게도 몇 가지 대안이 있습니다.

### <a name="the-pixels-property"></a>픽셀 속성

`SKBitmap` 정의 된 [ `Pixels` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Pixels/) 배열을 반환 하는 속성 `SKColor` 전체 비트맵에 대 한 값입니다. 사용할 수도 있습니다 `Pixels` 비트맵의 색 값의 배열을 설정 합니다.

```csharp
SKColor[] pixels = bitmap.Pixels;

bitmap.Pixels = pixels;
```

다음 두 번째 행, 왼쪽에서 오른쪽, 첫 번째 행을 사용 하 여 시작 하 여 배열에서 정렬 되는 픽셀과 등입니다. 총 수가 배열의 색 비트맵 너비와 높이의 곱 하는 것과 같습니다.

이 속성 효율적으로 나타나지만, 염두에 비트맵을 다시 배열 및 배열에 비트맵에서 픽셀을 복사 하는 하에서 픽셀을 변환할지 `SKColor` 값입니다.

여기에 메서드가 합니다 `GradientBitmapPage` 사용 하 여 비트맵을 설정 하는 클래스는 `Pixels` 속성입니다. 할당 메서드는 `SKColor` 하지만 필요한 크기의 배열을 사용할 수 있습니다는 `Pixels` 해당 배열을 만들려면 속성:

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

인덱스를 `pixels` 배열에서 계산 해야 합니다 `row` 및 `col` 변수입니다. 행은 픽셀 (이 경우 256), 각 행의 수를 곱한 및 열이 추가 됩니다.

`SKBitmap` 유사한 정의 `Bytes` 속성은 전체 비트맵에 대 한 바이트 배열을 반환 하는 이지만 전체 색 비트맵에 대 한 더 불편 합니다.

### <a name="the-getpixels-pointer"></a>GetPixels 포인터

비트맵 픽셀 액세스에 대 한 가장 강력한 방법은 잠재적 [ `GetPixels` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.GetPixels()/), 혼동 해서는 합니다 `GetPixel` 메서드 또는 `Pixels` 속성. 사용 하 여 차이점을 즉시 확인할 수 있습니다 `GetPixels` 는 반환 되는 C# 프로그래밍에서 매우 흔하게 없습니다.

```csharp
IntPtr pixelsAddr = bitmap.GetPixels();
```

.NET [ `IntPtr` ](xref:System.IntPtr) 형식 포인터를 나타냅니다. 라고 `IntPtr` 프로그램이 실행 되는, 일반적으로 32 비트 또는 64 비트 길이에서 컴퓨터의 기본 프로세서의 정수 길이 때문입니다. 합니다 `IntPtr` 는 `GetPixels` 는 실제 비트맵 개체는 해당 픽셀을 저장 하는 데 사용 하는 메모리 블록의 주소를 반환 합니다. 

변환할 수는 `IntPtr` C# 포인터 형식을 사용 하 여 합니다 [ `ToPointer` ](xref:System.IntPtr.ToPointer) 메서드. C# 포인터 구문은 C 및 c + +와 동일 합니다.

```csharp
byte* ptr = (byte*)pixelsAddr.ToPointer();
```

합니다 `ptr` 형식의 변수가 _바이트 포인터_합니다. 이 `ptr` 변수를 사용 하면 비트맵의 픽셀을 저장 하는 데 사용 되는 메모리의 개별 바이트를 액세스할 수 있습니다. 이 메모리에서 바이트를 읽거나 메모리 바이트를 쓸 다음과 같은 코드를 사용 합니다.

```sharp
byte pixelComponent = *ptr;

*ptr = pixelComponent;
```

이 컨텍스트에서 별표는 C# _간접 참조 연산자_ 가리키는 메모리의 내용을 참조 하는 데 사용 되 고 `ptr`입니다. 처음에 `ptr` 비트맵의 첫째 행의 첫 번째 픽셀의 첫 번째 바이트를 가리키는에서 산술 연산을 수행할 수는 `ptr` 비트맵 내의 다른 위치로 이동 하는 변수입니다.

한 가지 단점은이 사용할 수 있는 `ptr` 변수는 코드 블록 에서만에서 표시 된 `unsafe` 키워드. 또한 어셈블리를 안전 하지 않은 블록을 허용 하도록 플래그가 지정 해야 합니다. 프로젝트의 속성에서 수행 됩니다.

C#에서 포인터를 사용 하는 것은 매우 강력 하지만 매우 위험 합니다. 참조 해야 하는 포인터가 넘는 메모리에 액세스 하지는 않도록 주의 해야 합니다. 이것이 포인터 사용은 "안전 하지 않은." 라는 단어를 사용 하 여 연결 하는 이유

여기에 메서드가 합니다 `GradientBitmapPage` 사용 하는 클래스는 `GetPixels` 메서드. 통지를 `unsafe` 바이트 포인터를 사용 하는 모든 코드를 포함 하는 블록:

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

경우는 `ptr` 변수에서 처음 가져온는 `ToPointer` 메서드를 가리키는 비트맵의 첫째 행의 가장 왼쪽 픽셀의 첫 번째 바이트. `for` 루프에 대 한 `row` 및 `col` 설정 되어 있도록 `ptr` 사용 하 여 증가 될 수는 `++` 연산자 각 픽셀의 각 바이트가 설정 된 후 합니다. 픽셀을 통해 다른 99 루프는 `ptr` 비트맵의 시작 부분으로 다시 설정 해야 합니다.

각 픽셀의 메모리, 4 바이트 이므로 각 바이트를 개별적으로 설정 해야 합니다. 여기에서 코드 있다고 가정 바이트가 순서 빨간색, 녹색, 파랑 알파를 일치 하는 `SKColorType.Rgba8888` 형식 색입니다. 이 유니버설 Windows 플랫폼 아니라 iOS 및 Android에 대 한 기본 색 형식 인지를 회수할 수 있습니다. UWP를 기본적으로 사용 하 여 비트맵을 만듭니다는 `SKColorType.Bgra8888` 형식 색입니다. 이러한 이유로 해당 플랫폼의 일부 다른 결과가 표시 될!

반환 된 값으로 캐스팅할 수 있기 `ToPointer` 에 `uint` 포인터 대신 `byte` 포인터입니다. 이렇게 하면 하나의 문에서 액세스할 수는 전체 픽셀 수 있습니다. 적용 된 `++` 연산자는 포인터에 대 한 다음 픽셀을 가리키도록 4 바이트씩 증가 합니다.

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

픽셀 사용 하도록 설정 됩니다는 `MakePixel` 빨간색, 녹색, 파랑 및 알파 구성 요소에서 정수 픽셀을 생성 하는 메서드. 에 유의 합니다 `SKColorType.Rgba8888` 형식에는 다음과 같은 순서 픽셀 바이트:

RR GG, BB AA

하지만 해당 바이트에 해당 하는 정수는:

AABBGGRR

정수 최하위 바이트는 little endian 아키텍처에 따라 먼저 저장 됩니다. 이 `MakePixel` 사용 하 여 비트맵에 대 한 메서드를 제대로 작동 하지 것입니다는 `Bgra8888` 형식 색입니다.

`MakePixel` 메서드 플래그가 합니다 [ `MethodImplOptions.AggressiveInlining` ](xref:System.Runtime.CompilerServices.MethodImplOptions) 되지 않도록 하기 위해이 별도 메서드를 대신 코드를 컴파일하려면 메서드를 호출 하는 컴파일러를 유도 하는 옵션. 이렇게 하면 성능이 향상 됩니다.

흥미롭게도 합니다 `SKColor` 구조에서 명시적 변환을 정의 `SKColor` 는 부호 없는 정수입니다. 즉는 `SKColor` 값을 만들 수 및 변환할 `uint` 대신 사용할 수 있습니다 `MakePixel`:

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

만 질문은: 정수 형식에 `SKColor` 순서로 값를 `SKColorType.Rgba8888` 형식 색 또는 `SKColorType.Bgra8888` 색 형식 또는 다른 완전히? 해당 질문에 대답 곧 공개 됩니다.

### <a name="the-setpixels-method"></a>SetPixels 메서드

`SKBitmap` 또한 라는 메서드를 정의 [ `SetPixels` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.SetPixels/p/System.IntPtr/)를 다음과 같이 호출 하는:

```csharp
bitmap.SetPixels(intPtr);
```

이전에 설명한 대로 `GetPixels` 가져옵니다는 `IntPtr` 비트맵의 픽셀을 저장 하기 위해 사용 된 메모리 블록을 참조 합니다. `SetPixels` 호출 _대체_ 에서 참조 하는 메모리 블록을 사용 하 여 해당 메모리 블록을 `IntPtr` 로 지정 된는 `SetPixels` 인수입니다. 비트맵 다음 이전에 사용한 메모리 블록이 해제 됩니다. 다음 번 `GetPixels` 는 호출을 가져와서 사용 하 여 설정 된 메모리 블록 `SetPixels`합니다.

처음에 것 같습니다 처럼 `SetPixels` 더 더욱 강력 하 고 보다 성능을 제공 `GetPixels` 동시에 보다 편리 합니다. 사용 하 여 `GetPixels` 비트맵 메모리 블록을 가져오고 액세스 합니다. 사용 하 여 `SetPixels` 할당 하 고 일부 메모리에 액세스 하 고 비트맵 메모리 블록으로 설정 하는 합니다.

하지만 사용 하 여 `SetPixels` 고유 구문 이점이: 비트맵 픽셀 비트 배열을 사용 하 여 액세스할 수 있습니다. 여기에 메서드가 `GradientBitmapPage` 이 기법을 보여 주는 합니다. 메서드는 먼저 비트맵의 픽셀의 바이트를 해당 다차원 바이트 배열을 정의 합니다. 첫 번째 차원 행을 두 번째 차원의 열 이며 각 픽셀의 네 가지 구성 요소를 세 번째 차원의 합니다.

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

그런 다음 픽셀을 사용 하 여 배열에 채워진 후는 `unsafe` 블록 및 `fixed` 문을이 배열을 가리키는 바이트 포인터를 가져오는 데 사용 됩니다. 해당 바이트 포인터에 캐스팅 될 수는 `IntPtr` 전달할 `SetPixels`합니다.

사용자가 만든 배열의 바이트 배열 필요가 없습니다. 행 및 열에 대 한 두 개의 차원 가진 정수 배열을 수 있습니다.

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

`MakePixel` 메서드는 다시 32 비트 픽셀 색 구성 요소를 결합 하는 데 사용 됩니다.

방금 완결성 같습니다 동일한 코드를 했지만 `SKColor` 값을 부호 없는 정수로 캐스팅 합니다.

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

생성자는 **그라데이션 색** 페이지 8 모든 위에 표시 된 메서드를 호출 하 고 결과 저장 합니다.

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

생성자를 만들어 완료는 `SKCanvasView` 결과 비트맵을 표시 합니다. 합니다 `PaintSurface` 처리기 나눕니다 화면 8 사각형과 호출 `Display` 각각을 표시 하려면:

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

컴파일러는 코드를 최적화할 수 있도록이 페이지에서 실행 되었습니다 **릴리스** 모드입니다. MacBook Pro, Nexus 5 Android 휴대폰 및 Windows 10을 실행 하는 Surface Pro 3에는 8 iPhone 시뮬레이터에서 실행 되 고 해당 페이지는 다음과 같습니다. 하드웨어 차이로 인해 비교와 장치 간의 성능 시간을 방지 하지만 대신 각 장치에서 상대 시간을 확인 합니다.

[![그라데이션 비트맵](pixel-bits-images/GradientBitmap.png "그라데이션 비트맵")](pixel-bits-images/GradientBitmap-Large.png#lightbox)

다음은 실행 시간 (밀리초)에 통합 하는 테이블이입니다.

| API       | 데이터 형식 | iOS  | Android | UWP  |
| --------- | --------- | ----:| -------:| ----:|
| SetPixel  |           | 3.17 |   10.77 | 3.49 |
| 픽셀    |           | 0.32 |    1.23 | 0.07 |
| GetPixels | byte      | 0.09 |    0.24 | 0.10 |
|           | uint      | 0.06 |    0.26 | 0.05 |
|           | SKColor   | 0.29 |    0.99 | 0.07 |
| SetPixels | byte      | 1.33 |    6.78 | 0.11 |
|           | uint      | 0.14 |    0.69 | 0.06 |
|           | SKColor   | 0.35 |    1.93 | 0.10 |

예상 대로 호출 `SetPixel` 65,536 번 하는 방법은 최소 effeicient 비트맵의 픽셀을 설정 합니다. 채우기는 `SKColor` 배열 및 설정 합니다 `Pixels` 속성 훨씬 더 좋고도 중 일부를 사용 하 여 알맞게 비교를 `GetPixels` 및 `SetPixels` 기술. 작업할 `uint` 픽셀 값은 일반적으로 별도 설정 보다 더 빠릅니다 `byte` 구성 요소 및 변환의 `SKColor` 프로세스에 약간의 오버 헤드를 추가 하는 부호 없는 정수 값입니다.

다양 한 그라데이션 비교할 사실도: 상위 행 세 플랫폼 모두의 동일 및 의도 대로 그라데이션을 보여 줍니다. 즉 합니다 `SetPixel` 메서드 및 `Pixels` 속성 기본 픽셀 형식에 관계 없이 색에서 픽셀을 올바르게 만듭니다.

IOS 및 Android 스크린샷 다음 두 행은 동일 수도 있는지 확인 하는 작은 `MakePixel` 메서드가 기본 올바르게 정의 되어 `Rgba8888` 이러한 플랫폼에 대 한 픽셀 형식입니다.

IOS 및 Android 스크린샷 맨 아래쪽 행입니다. 이전 버전과 호환성을 부호 없는 정수로 캐스팅 하 여 가져올 있음을 나타냅니다는 `SKColor` 형식에서 값은:

AARRGGBB

바이트 순서로 표시 됩니다.

BB GG RR AA

이 `Bgra8888` 순서 대신 `Rgba8888` 순서입니다. `Brga8888` 형식은 그라데이션을 스크린샷에의 마지막 행에서 첫 번째 행 동일는 유니버설 Windows 플랫폼의 경우 기본값입니다. 이러한 비트맵을 생성 하는 코드 가정 하기 때문에 중간 두 행은 올바르지 않습니다 하지만 `Rgba8888` 순서입니다.

명시적으로 만들면 세 플랫폼 모두에서 픽셀 비트에 액세스 하기 위한 동일한 코드를 사용 하려는 경우는 `SKBitmap` 하나를 사용 하는 `Rgba8888` 또는 `Bgra8888` 형식입니다. 캐스팅 하려는 경우 `SKColor` 사용 하는 비트맵 픽셀 값 `Bgra8888`합니다.

## <a name="random-access-of-pixels"></a>픽셀의 임의 액세스

`FillBitmapBytePtr` 하 고 `FillBitmapUintPtr` 의 메서드는 **그라데이션 비트맵** 페이지에서 활용할 수 있습니다 `for` 루프 맨 아래 행을 왼쪽에서 오른쪽으로 각 행의 맨 위 행에서 비트맵을 순차적으로 충족 하도록 설계 되었습니다. 포인터 증가 하는 동일한 문을 사용 하 여 픽셀을 설정할 수 있습니다. 

경우에 따라 순차적으로 대신 임의로 픽셀에 액세스 해야 합니다. 사용 중인 경우는 `GetPixels` 포인터를 행 및 열에 따라 계산 해야 접근 방식입니다. 에 설명 되어이 **레인 보우 사인** 페이지는 rainbow 사인 곡선의 한 주기의 형태로 보여 주는 비트맵을 만듭니다. 

무지개 색깔이 HSL (색상, 채도 명도) 색 모델을 사용 하 여 만드는 가장 쉬운 방법은입니다. 합니다 `SKColor.FromHsl` 메서드를 만듭니다는 `SKColor` 범위는 0에서 360 (예:의 각도 원 있지만 빨간색, 녹색 및 파란색 및 빨간색으로), 색상 값을 사용 하 여 값 및 0에서 100 까지의 채도 명도 값입니다. 전설의 무지개 요정을 색에 대 한 최대 100 및 50의 중간점에 명도를 채도 설정 해야 합니다.

**Rainbow 사인** 비트맵의 행을 반복 하 고 다음 360 색상 값을 반복 하 여이 이미지를 만듭니다. 각 색상 값에서 비트맵 열도를의 사인 값에 따라 계산 됩니다.

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

생성자 기반 비트맵은 만듭니다는 `SKColorType.Bgra8888` 형식:

```csharp
bitmap = new SKBitmap(360 * 3, 1024, SKColorType.Bgra8888, SKAlphaType.Unpremul);
```

이렇게 하면 변환을 사용 하 여 프로그램 `SKColor` 값을 `uint` 걱정 없이 픽셀입니다. 역할 재생 되지 않을 것이 특정 프로그램에 있지만 사용할 때마다 합니다 `SKColor` 도 지정 해야 픽셀을 설정 하는 변환 `SKAlphaType.Unpremul` 때문에 `SKColor` 색 구성 요소를 프리 멀티 플라이 알파 값을 기준으로 하지 않습니다.

생성자를 사용 하 여는 `GetPixels` 비트맵의 첫째 픽셀에 대 한 포인터를 가져오는 방법.

```csharp
uint* basePtr = (uint*)bitmap.GetPixels().ToPointer();
```

모든 특정 행과 열에 대 한 오프셋된 값에 추가 해야 `basePtr`합니다. 이 오프셋은 행을 곱한 비트맵 너비와 열:

```csharp
uint* ptr = basePtr + bitmap.Width * row + col;
```

`SKColor` 값이이 포인터를 사용 하 여 메모리에 저장 됩니다.

```csharp
*ptr = (uint)SKColor.FromHsl(hue, 100, 50);
```

에 `PaintSurface` 처리기는 `SKCanvasView`, 비트맵은 표시 영역을 채우도록 확장:

[![Rainbow 사인](pixel-bits-images/RainbowSine.png "레인 보우 사인")](pixel-bits-images/RainbowSine-Large.png#lightbox)

## <a name="from-one-bitmap-to-another"></a>다른 하나의 비트맵에서

매우 많은 이미지 처리 작업에서 다른 하나의 비트맵에서 전송 될 때 픽셀을 수정 포함 됩니다. 이 기술은에 설명 되어는 **색 조정** 페이지입니다. 페이지 비트맵 리소스 중 하나를 로드 하 고 다음 세 가지를 사용 하 여 이미지를 수정할 수 있습니다 `Slider` 뷰:

[![색 조정](pixel-bits-images/ColorAdjustment.png "색 조정")](pixel-bits-images/ColorAdjustment-Large.png#lightbox)

각 픽셀 색에 대 한 첫 번째 `Slider` 값을 0에서 360의 색을 추가 하지만 사용 하 여를 모듈로 0부터 360 사이 결과 유지 하려면 연산자를 효과적으로 시프트 색 스펙트럼을 따라 (에서처럼 UWP 스크린샷). 두 번째 `Slider` 0.5, 채도 및 세 번째에 적용할 수는 2 사이의 곱하기 요소를 선택할 수 있습니다 `Slider` Android 스크린샷에 표시 된 것과 같이 광도에 대 한 동일한 작업을 수행 합니다.

프로그램 유지 라는 원래 소스 비트맵 두 비트맵 `srcBitmap` 라는 조정 된 대상 비트맵 및 `dstBitmap`합니다. 각 시간을 `Slider` 이동 되 면 프로그램의 모든 새 픽셀 계산 `dstBitmap`합니다. 사용자가 이동 하 여 실험은 물론,는 `Slider` 뷰 매우 신속 하 게 있으므로 최상의 성능을 관리할 수 있습니다. 여기에 `GetPixels` 원본 및 대상 비트맵에 대 한 메서드. 

합니다 **색 조정** 페이지 원본 및 대상 비트맵의 색 형식을 제어 하지 않습니다. 대신 약간 다른 논리를 포함 `SKColorType.Rgba8888` 고 `SKColorType.Bgra8888` 형식입니다. 원본 및 대상에는 서로 다른 형식으로 될 수 있습니다 및 프로그램이 계속 작동 합니다.

여기는 것이 중요 제외 하 고 프로그램이 `TransferPixels` 대상에 소스를 형성 하는 픽셀을 전송 하는 메서드. 생성자 `dstBitmap` 같음 `srcBitmap`합니다. 합니다 `PaintSurface` 처리기 표시 `dstBitmap`:

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

합니다 `ValueChanged` 에 대 한 처리기는 `Slider` 조정 값과 호출을 계산 하는 뷰 `TransferPixels`합니다.

전체 `TransferPixels` 메서드가로 표시 되어 `unsafe`입니다. 두 비트맵의 픽셀 비트에 대 한 바이트 포인터를 가져와서 시작 하 고 모든 행과 열을 반복 합니다. 소스 비트맵에서 메서드는 각 픽셀에 대해 4 바이트를 가져옵니다. 중 하나에 있을 수 있습니다 이러한 합니다 `Rgba8888` 또는 `Bgra8888` 순서입니다. 색 형식에서 허용에 대 한 검사는 `SKColor` 만들 값입니다. HSL 구성 요소는 다음 추출, 조정 및 다시 만드는 데는 `SKColor` 값입니다. 대상 비트맵 인지에 따라 `Rgba8888` 또는 `Bgra8888`, 바이트 대상 bitmp에 저장 됩니다.

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

이 방법의 성능은 다양 한 원본 및 대상 비트맵의 색 형식 조합에 대 한 별도 메서드를 만들어 훨씬 더 향상 될 수 없습니다 하 고 모든 픽셀에 대 한 형식 검사 하지 않고 가능성이 있습니다. 또 다른 옵션은 여러 개의 `for` 에 대 한 루프를 `col` 변수 형식을 기반으로 색입니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
