---
title: Xamarin.iOS에서 파일 압축
description: 이 문서에서는 Xamarin.iOS에서 libcompression API 사용 하는 방법을 설명 합니다. Deflating, 값, 설명 및 다양 한 알고리즘을 지원 합니다.
ms.prod: xamarin
ms.assetid: 94D05DAB-01E8-4C62-9CEF-9D6417EEA8EB
ms.technology: xamarin-ios
author: mandel-macaque
ms.author: mandel
ms.date: 03/04/2019
ms.openlocfilehash: f7a1df65047fd8040dd40e9f7f057d6bfe6dea61
ms.sourcegitcommit: 4c97f5d73be7eb2da153a85183be4258b6b11ca6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58290932"
---
# <a name="file-compression-in-xamarinios"></a>Xamarin.iOS에서 파일 압축

9.0 iOS 또는 macOS 10.11 (및 이상)를 대상으로 하는 Xamarin 앱에서 사용할 수는 _압축 Framework_ 압축 (인코딩) 및 압축 풀기 (디코딩) 데이터. Xamarin.iOS는 Stream API를 다음이 프레임 워크를 제공 합니다. 압축 프레임 워크는 개발자가 압축을 사용 하 여 상호 작용할 수 있습니다 및 콜백 또는 대리자를 사용할 필요 없이 일반 스트림을 것 처럼 데이터를 압축 합니다.

압축 프레임 워크에는 다음 알고리즘에 대 한 지원을 제공합니다.

* LZ4
* 원시 LZ4
* Lzfse
* Lzma
* Zlib

압축 프레임 워크를 사용 하 여 개발자가 타사 라이브러리 또는 Nuget 없이 압축 작업을 수행할 수 있습니다. 이 외부 종속성 감소 및 압축 작업 (으로 충족 하는 최소 OS 요구 사항) 모든 플랫폼에서 지원 됩니다.

## <a name="general-file-decompression"></a>일반 파일의 압축 풀기

압축 프레임 워크는 Xamarin.iOS 및 Xamarin.Mac 스트림 API를 사용합니다. 이 API는 데이터를 압축 하려면 개발자 패턴을 사용 하는 일반.NET 내에서 다른 IO Api에 사용 되는 것을 의미 합니다. 다음 샘플에 있는 API와 유사한는 압축 프레임 워크를 사용 하 여 데이터를 압축 하는 방법을 보여 줍니다는 `System.IO.Compression.DeflateStream` API:

```csharp
// sample zlib data
static byte [] compressed_data = { 0xf3, 0x48, 0xcd, 0xc9, 0xc9, 0xe7, 0x02, 0x00 };

using (var backing = new MemoryStream (compressed_data)) // compress data to read
using (var decompressing = new CompressionStream (backing, CompressionMode.Decompress, CompressionAlgorithm.Zlib)) // create decompression stream with the correct algorithm
using (var reader = new StreamReader (decompressing))
{
    // perform the required stream operations
    Console.WriteLine (reader.ReadLine ());
}
```

합니다 `CompressionStream` 구현 합니다 `IDisposable` 같은 다른 인터페이스 `System.IO.Streams`이므로 개발자는 더 이상 필요 없는 되 면 리소스가 해제 되는 확인 해야 합니다.

## <a name="general-file-compression"></a>일반 파일 압축

압축 API는 동일한 API를 다음 데이터를 압축 하는 개발자를 수도 있습니다. 제공 된 알고리즘에 나와 있는 것 중 하나를 사용 하 여 데이터를 압축할 수 있습니다는 `CompressionAlgorithm` 열거자입니다.

```csharp
// sample method that copies the data from the source stream to the destination stream
static void CopyStream (Stream src, Stream dest)
{
    byte[] array = new byte[1024];
    int bytes_read;
    bytes_read = src.Read (array, 0, 1024);
    while (bytes_read != 0) {
        dest.Write (array, 0, bytes_read);
        bytes_read = src.Read (array, 0, 1024);
    }
}

static void CompressExample ()
{
    // create some sample data to compress
    byte [] data = new byte[100000];
    for (int i = 0; i < 100000; i++) {
        data[i] = (byte) i;
    }

    using (var dataStream = new MemoryStream (data)) // stream that contains the data to compress
    using (var backing = new MemoryStream ()) // stream in which the compress data will be written
    using (var compressing = new CompressionStream (backing, CompressionMode.Compress, CompressionAlgorithm.Zlib, true))
    {
        // copy the data to the compressing stream
        CopyStream (dataStream, compressing);
        // at this point, the CompressionStream contains all the data from the dataStream but compressed
        // using the zlib algorithm
    }
```

## <a name="async-support"></a>비동기 지원

합니다 `CompressionStream` 에서 지원 되는 모든 비동기 작업을 지원 합니다 `System.IO.DeflateStream`, 즉, 개발자가 UI 스레드를 차단 하지 않고 압축/압축 풀기 작업을 수행 하는 async 키워드를 사용할 수 있습니다.