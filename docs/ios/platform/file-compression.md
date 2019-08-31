---
title: Xamarin.ios의 파일 압축
description: 이 문서에서는 Xamarin.ios에서, Xamarin.ios 압축 API를 사용 하는 방법을 설명 합니다. Deflating, 않아서 및 지원 되는 다양 한 알고리즘에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 94D05DAB-01E8-4C62-9CEF-9D6417EEA8EB
ms.technology: xamarin-ios
author: mandel-macaque
ms.author: mandel
ms.date: 03/04/2019
ms.openlocfilehash: bcc63aa4e1926f5502d571bf47c83b0c8ea7e429
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2019
ms.locfileid: "70199528"
---
# <a name="file-compression-in-xamarinios"></a>Xamarin.ios의 파일 압축

IOS 9.0 또는 macOS 10.11를 대상으로 하는 Xamarin 앱은 _압축 프레임 워크_ 를 사용 하 여 데이터를 압축 (인코딩) 및 압축 풀기 (디코드) 할 수 있습니다. Xamarin.ios는 Stream API를 따라이 프레임 워크를 제공 합니다. 개발자는 압축 프레임 워크를 사용 하 여 콜백이 나 대리자를 사용할 필요 없이 일반적인 스트림 처럼 압축 및 압축 해제 된 데이터와 상호 작용할 수 있습니다.

압축 프레임 워크는 다음과 같은 알고리즘을 지원 합니다.

* LZ4
* LZ4 Raw
* Lzfse
* Lzma
* Zlib

개발자는 압축 프레임 워크를 사용 하 여 타사 라이브러리 또는 Nuget 없이 압축 작업을 수행할 수 있습니다. 이렇게 하면 외부 종속성이 줄어들고 모든 플랫폼에서 압축 작업이 지원 됩니다 (최소 OS 요구 사항을 충족 하는 경우).

## <a name="general-file-decompression"></a>일반 파일 압축 풀기

압축 프레임 워크는 Xamarin.ios 및 Xamarin.ios에서 stream API를 사용 합니다. 이 API는 데이터를 압축 하기 위해 개발자가 .NET 내의 다른 IO Api에 사용 되는 일반 패턴을 사용할 수 있음을 의미 합니다. 다음 샘플에서는 `System.IO.Compression.DeflateStream` api에 있는 api와 유사한 압축 프레임 워크를 사용 하 여 데이터를 압축 해제 하는 방법을 보여 줍니다.

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

는 `CompressionStream` `IDisposable` 다른 와`System.IO.Streams`같이 인터페이스를 구현 하므로 개발자는 리소스가 더 이상 필요 하지 않은 경우 해제 되도록 해야 합니다.

## <a name="general-file-compression"></a>일반 파일 압축

또한 개발자는 압축 API를 사용 하 여 동일한 API를 따라 데이터를 압축할 수 있습니다. `CompressionAlgorithm` 열거자에 명시 된 제공 된 알고리즘 중 하나를 사용 하 여 데이터를 압축할 수 있습니다.

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

는에서 지원 `System.IO.DeflateStream`되는 모든 비동기 작업을 지원합니다.즉,개발자는async키워드를사용하여UI스레드를차단하지않고압축/압축풀기작업을수행할수있습니다.`CompressionStream`
