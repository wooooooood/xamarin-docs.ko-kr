---
title: "Azure 저장소에 데이터 저장 및 액세스"
description: "Azure 저장소에는, 구조화 되지 않은 작업과 구조화 된 데이터를 저장 하는 데 사용할 수 있는 확장 가능한 클라우드 저장소 솔루션입니다. 이 문서에서는 Azure 저장소에 텍스트 및 이진 데이터를 저장 하려면 Xamarin.Forms를 사용 하는 방법과 데이터에 액세스 하는 방법을 보여줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5B10D37B-839B-4CD0-9C65-91014A93F3EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: d2d85840a0c698bfd3aa01dbacb204072ecca119
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="storing-and-accessing-data-in-azure-storage"></a>Azure 저장소에 데이터 저장 및 액세스

_Azure 저장소에는, 구조화 되지 않은 작업과 구조화 된 데이터를 저장 하는 데 사용할 수 있는 확장 가능한 클라우드 저장소 솔루션입니다. 이 문서에서는 Azure 저장소에 텍스트 및 이진 데이터를 저장 하려면 Xamarin.Forms를 사용 하는 방법과 데이터에 액세스 하는 방법을 보여줍니다._

## <a name="overview"></a>개요

Azure 저장소는 네 개의 저장소 서비스를 제공합니다.

- Blob 저장소입니다. Blob는 백업, 가상 컴퓨터, 미디어 파일 또는 문서와 같은 텍스트 또는 이진 데이터를 수 있습니다.
- 테이블 저장소는 NoSQL 키-특성 저장소입니다.
- 큐 저장소는 워크플로 처리 및 클라우드 서비스 간의 통신에 대 한 메시징 서비스입니다.
- 파일 저장소에는 SMB 프로토콜을 사용 하 여 공유 저장소를 제공 합니다.

두 가지 방법으로 저장소 계정:

- 범용 저장소 계정은 단일 계정에서 Azure 저장소 서비스에 대 한 액세스를 제공 합니다.
- Blob 저장소 계정에 blob을 저장 하기 위한 특수 한 저장소 계정이입니다. Blob 데이터를 저장 하려는 경우이 계정 유형은 것이 좋습니다.

이 문서 및 샘플 응용 프로그램을 함께 제공 된 업로드 이미지 및 텍스트 파일을 다운로드 하 고 blob 저장소를 보여 줍니다. 또한 보여 줍니다 blob 저장소에서 파일의 목록을 검색 하 고 파일을 삭제 합니다.

Azure 저장소에 대 한 자세한 내용은 참조 [저장소 소개](https://azure.microsoft.com/documentation/articles/storage-introduction/)합니다.

## <a name="introduction-to-blob-storage"></a>Blob 저장소 소개

Blob 저장소는 다음 다이어그램에 표시 된 대로 세 가지 구성 요소 구성 됩니다.

![](azure-storage-images/blob-storage.png "Blob 저장소 개념")

Azure 저장소에 대 한 모든 액세스는 저장소 계정을 통해 이루어집니다. 저장소 계정에 컨테이너를 개수에 제한 없이 포함 될 수 있습니다 및 컨테이너는 blob 저장소 계정의 용량 제한 개수에 제한 없이 저장할 수 있습니다.

Blob은 모든 형식과 크기의 파일입니다. Azure 저장소에는 세 가지 다른 blob 유형을 지원합니다.

- 블록 blob는 스트리밍 및 클라우드 개체를 저장 하기 위한 최적화 되었으며 백업, 미디어 파일, 문서 등을 저장 하기에 적합 합니다. 블록 blob의 크기가 195 크기가 최대 일 수 있습니다.
- 추가 blob은 블록 blob와 유사 하지만에 맞게 최적화 된 로깅 같은 작업을 추가 합니다. 추가 blob 크기가 195 크기가 최대 일 수 있습니다.
- 페이지 blob는 자주 읽기/쓰기 작업을 위해 최적화 하 고 가상 컴퓨터 및 디스크를 저장 하는 데 일반적으로 사용 됩니다. 페이지 blob의 크기가 1tb까지 될 수 있습니다.

> [!NOTE]
> Blob 저장소 계정에 블록을 지원 및 blob를 사용 하지만 페이지 blob이 아닌 추가 참고 합니다.

Blob은 Azure 저장소로 업로드할 수 있으며 바이트 스트림으로 Azure 저장소에서 다운로드할 합니다. 따라서 파일 업로드 및 다운로드 한 후 원래 표현으로 변환 된 다시 이전 바이트 스트림으로 변환 해야 합니다.

Azure 저장소에 저장 된 모든 개체에 고유한 URL 주소가 있습니다. 저장소 계정 이름을 해당 주소 및 하위 도메인 및 도메인 이름 형식은 조합의의 하위 도메인을 형성 된 *끝점* 저장소 계정에 대 한 합니다. 예를 들어 저장소 계정의 이름이 *mystorageaccount*, 저장소 계정에 대 한 기본 blob 끝점은 `https://mystorageaccount.blob.core.windows.net`합니다.

저장소 계정에는 개체에 액세스 하기 위한 URL 끝점에는 저장소 계정에서 개체의 위치를 추가 하 여 만들어집니다. 예를 들어 blob 주소 형식을 갖습니다 `https://mystorageaccount.blob.core.windows.net/mycontainer/myblob`합니다.

## <a name="setup"></a>설정

Azure 저장소 계정을 Xamarin.Forms 응용 프로그램에 통합 하기 위한 프로세스는 다음과 같습니다.

1. 저장소 계정을 만듭니다. 자세한 내용은 참조 [저장소 계정 만들기](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account)합니다.
1. 추가 [Azure 저장소 클라이언트 라이브러리](https://www.nuget.org/packages/WindowsAzure.Storage/) Xamarin.Forms 응용 프로그램에 있습니다.
1. 저장소 연결 문자열을 구성 합니다. 자세한 내용은 참조 [Azure 저장소에 연결](#connecting)합니다.
1. 추가 `using` 에 대 한 지시문은 `Microsoft.WindowsAzure.Storage` 및 `Microsoft.WindowsAzure.Storage.Blob` Azure 저장소에 액세스 하는 클래스에 네임 스페이스입니다.

> [!NOTE]
> 이 샘플에서는 공유 액세스 프로젝트 Azure 저장소 클라이언트 라이브러리 이제도 지원 클래스 라이브러리 PCL (이식 가능한) 프로젝트에서 사용 되 고 합니다.

<a name="connecting" />

## <a name="connecting-to-azure-storage"></a>Azure 저장소에 연결

저장소 계정 리소스에 대 한 모든 요청은 인증 되어야 합니다. Blob는 익명 인증을 지원 하도록 구성할 수 있지만, 두 가지가 주 응용 프로그램 저장소 계정과 인증에 사용할 수 있습니다.:

- 공유 키입니다. 이 방식은 액세스 저장소 서비스에 Azure 저장소 계정 이름 및 계정 키를 사용합니다. 저장소 계정 만들기 공유 키 인증에 사용할 수 있는 두 개의 개인 키에 할당 됩니다.
- 공유 액세스 서명입니다. 이 저장소 리소스에 대 한 위임 된 액세스를 사용 하도록 설정 하는 URL에 추가 될 수 있는 토큰, 사용 권한으로 지정, 유효 기간에 대 한.

응용 프로그램에서 Azure 저장소 리소스에 액세스 하는 데 필요한 인증 정보를 포함 하는 연결 문자열을 지정할 수 있습니다. 또한 Visual Studio에서 Azure 저장소 에뮬레이터에 연결 하는 연결 문자열을 구성할 수 있습니다.

> [!NOTE]
> Azure 저장소 연결 문자열에 HTTP 및 HTTPS를 지원합니다. 그러나 HTTPS를 사용 하는 것이 좋습니다.

### <a name="connecting-to-the-azure-storage-emulator"></a>Azure 저장소 에뮬레이터에 연결

Azure 저장소 에뮬레이터는 Azure blob, 큐 및 테이블 서비스를 개발 목적으로 에뮬레이트하는 로컬 환경을 제공 합니다.

Azure 저장소 에뮬레이터에 연결 하는 데 사용한 다음 연결 문자열:

```csharp
UseDevelopmentStorage=true
```

Azure 저장소 에뮬레이터에 대 한 자세한 내용은 참조 [개발 및 테스트에 대 한 Azure 저장소 에뮬레이터를 사용 하 여](https://azure.microsoft.com/documentation/articles/storage-use-emulator/)합니다.

### <a name="connecting-to-azure-storage-using-a-shared-key"></a>공유 키를 사용 하 여 Azure 저장소에 연결

공유 키와 Azure 저장소에 연결 하는 다음 연결 문자열 형식을 따라야 합니다.

```csharp
DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey
```

`myAccountName` 저장소 계정의 이름으로 대체 해야 하 고 `myAccountKey` 두 계정 액세스 키 중 하나가 지정 된 대체 해야 합니다.

> [!NOTE]
> 키 인증, 계정 이름 및 계정 키 경우 공유를 사용 하 여 응용 프로그램은 저장소 계정에 대 한 모든 읽기/쓰기 액세스를 제공할 수 있는 사용 하 여 각 사용자에 게 배포 됩니다. 따라서 테스트 목적 으로만, 공유 키 인증을 사용 하 고 다른 사용자에 게 키 배포.

### <a name="connecting-to-azure-storage-using-a-shared-access-signature"></a>공유 액세스 서명을 사용 하 여 Azure 저장소에 연결

SAS는 Azure 저장소에 연결 하는 데 다음 연결 문자열 형식을 따라야 합니다.

`BlobEndpoint=myBlobEndpoint;SharedAccessSignature=mySharedAccessSignature`

`myBlobEndpoint` blob 끝점의 URL로 대체 해야 하 고 `mySharedAccessSignature` 프로그램 SAS로 대체 해야 합니다. SAS는 프로토콜, 서비스 끝점 및 리소스에 액세스 하는 데 사용할 자격 증명을 제공 합니다.

> [!NOTE]
> SAS 인증 프로덕션 응용 프로그램에 대 한 것이 좋습니다. 그러나 프로덕션 응용 프로그램은 SAS 해야 응용 프로그램과 함께 제공 되는 것이 아니라는 백 엔드 서비스 요청에서 검색 됩니다.

공유 액세스 서명에 대 한 자세한 내용은 참조 하십시오. [공유 액세스 서명 (SAS)를 사용 하 여](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)합니다.

## <a name="creating-a-container"></a>컨테이너 만들기

`GetContainer` 메서드를 사용할 수 있는 다음 컨테이너에서 blob를 검색 하거나 blob 컨테이너에 추가 하는 명명 된 컨테이너에 대 한 참조를 사용 합니다. 다음 코드 예제는 `GetContainer` 메서드:

```csharp
static CloudBlobContainer GetContainer(ContainerType containerType)
{
  var account = CloudStorageAccount.Parse(Constants.StorageConnection);
  var client = account.CreateCloudBlobClient();
  return client.GetContainerReference(containerType.ToString().ToLower());
}
```

`CloudStorageAccount.Parse` 메서드는 연결 문자열 구문 분석 하 고 반환는 `CloudStorageAccount` 저장소 계정을 나타내는 인스턴스입니다. A `CloudBlobClient` 컨테이너 및 blob를 검색에 사용 되는 인스턴스를 만든 다음는 `CreateCloudBlobClient` 메서드. `GetContainerReference` 메서드 검색으로 지정된 된 컨테이너는 `CloudBlobContainer` 인스턴스를 호출 하는 메서드로 반환 되기 전에 합니다. 이 예제에서는 컨테이너 이름은 `ContainerType` 열거형 값을 소문자 문자열로 변환 합니다.

> [!NOTE]
> 컨테이너 이름은 소문자, 여야 하며 문자 또는 숫자로 시작 해야 합니다. 또한 문자, 숫자 및 대시 문자만 이루어질 수 있으며 3 자에서 63 자 사이 여야 합니다.

`GetContainer` 메서드가 다음과 같이 호출 됩니다.

```csharp
var container = GetContainer(containerType);
```

`CloudBlobContainer` 인스턴스 컨테이너를 만들려면 존재 하지 않는 경우 사용할 수 있습니다.

```csharp
await container.CreateIfNotExistsAsync();
```

기본적으로 새로 만든된 컨테이너는 전용입니다. 이 컨테이너에서 blob를 검색 하는 저장소 액세스 키를 지정 해야 합니다는 것을 의미 합니다. 공용 컨테이너 내의 blob을 만드는 방법은 참조 [컨테이너 만들기](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#create-a-container)합니다.

## <a name="uploading-data-to-a-container"></a>컨테이너에 데이터 업로드

`UploadFileAsync` 메서드는 blob 저장소에 바이트 데이터 스트림을 업로드 하는 데 사용 되 고 다음 코드 예제에 표시 됩니다.

```csharp
public static async Task<string> UploadFileAsync(ContainerType containerType, Stream stream)
{
  var container = GetContainer(containerType);
  await container.CreateIfNotExistsAsync();

  var name = Guid.NewGuid().ToString();
  var fileBlob = container.GetBlockBlobReference(name);
  await fileBlob.UploadFromStreamAsync(stream);

  return name;
}
```

컨테이너 참조를 검색 한 후 존재 하지 않는 경우 메서드는 컨테이너를 만듭니다. 새 `Guid` 고유 blob 이름으로 역할을 만든 다음 blob 블록 참조로 검색 됩니다는 `CloudBlockBlob` 인스턴스. 데이터 스트림을 사용 하 여 blob에 업로드 한 다음는 `UploadFromStreamAsync` 메서드 파일이 있으면 덮어씁니다 하거나 존재 하지 않는 경우에 blob을 만듭니다.

이 메서드를 사용 하는 저장소 blob에 파일을 업로드할 수 있습니다, 전에 먼저 바이트 스트림으로 변환 합니다. 다음 코드 예제에서이 확인할 수 있습니다.

```csharp
var byteData = Encoding.UTF8.GetBytes(text);
uploadedFilename = await AzureStorage.UploadFileAsync(ContainerType.Text, new MemoryStream(byteData));
```

`text` 데이터 다음에 전달 되는 스트림으로 래핑되고를 바이트 배열로 변환 됩니다는 `UploadFileAsync` 메서드.

## <a name="downloading-data-from-a-container"></a>컨테이너에서 데이터를 다운로드합니다.

`GetFileAsync` 메서드는 Azure 저장소에서 blob 데이터를 다운로드 하는 데 사용 되 고 다음 코드 예제에 표시 됩니다.

```csharp
public static async Task<byte[]> GetFileAsync(ContainerType containerType, string name)
{
  var container = GetContainer(containerType);

  var blob = container.GetBlobReference(name);
  if (await blob.ExistsAsync())
  {
    await blob.FetchAttributesAsync();
    byte[] blobBytes = new byte[blob.Properties.Length];

    await blob.DownloadToByteArrayAsync(blobBytes, 0);
    return blobBytes;
  }
  return null;
}
```

메서드를 컨테이너 참조를 검색 한 후 저장된 된 데이터에 대 한 blob 참조를 검색 합니다. 해당 속성은 검색 blob이 존재 하는 경우는 `FetchAttributesAsync` 메서드. 올바른 크기의 바이트 배열을 만들어지고으로 호출 하는 메서드로 반환 되는 바이트 배열을 blob 다운로드 됩니다.

Blob 바이트 데이터를 다운로드 한 후 원래 표현으로 변환 해야 합니다. 다음 코드 예제에서이 확인할 수 있습니다.

```csharp
var byteData = await AzureStorage.GetFileAsync(ContainerType.Text, uploadedFilename);
string text = Encoding.UTF8.GetString(byteData);
```

바이트의 배열 하 여 Azure 저장소에서 검색 되는 `GetFileAsync` 는 UTF8으로 다시 변환 되기 전에 메서드를 인코딩된 문자열입니다.

## <a name="listing-data-in-a-container"></a>컨테이너에 데이터 표시

`GetFilesListAsync` 메서드를 사용 하는 컨테이너에 저장 된 blob의 목록을 검색 하 고 다음 코드 예제에 표시 됩니다.

```csharp
public static async Task<IList<string>> GetFilesListAsync(ContainerType containerType)
{
  var container = GetContainer(containerType);

  var allBlobsList = new List<string>();
  BlobContinuationToken token = null;

  do
  {
    var result = await container.ListBlobsSegmentedAsync(token);
    if (result.Results.Count() > 0)
    {
      var blobs = result.Results.Cast<CloudBlockBlob>().Select(b => b.Name);
      allBlobsList.AddRange(blobs);
    }
    token = result.ContinuationToken;
  } while (token != null);

  return allBlobsList;
}
```

컨테이너의을 사용 하는 메서드를 컨테이너 참조를 검색 한 후 `ListBlobsSegmentedAsync` 컨테이너 내의 blob에 대 한 참조를 검색 하는 메서드입니다. 반환 된 결과 `ListBlobsSegmentedAsync` 메서드를 열거 하는 동안는 `BlobContinuationToken` 인스턴스가 않습니다 `null`합니다. 각 blob 캐스팅 된 반환 된 `IListBlobItem` 에 `CloudBlockBlob` 순서 액세스에는 `Name` 값 되기 전에 blob의 속성을 추가 `allBlobsList` 컬렉션입니다. 한 번의 `BlobContinuationToken` 인스턴스가 `null`, 반환 된 마지막 blob 이름 및 실행이 루프 종료 합니다.

## <a name="deleting-data-from-a-container"></a>컨테이너에서 데이터 삭제

`DeleteFileAsync` 메서드는 컨테이너에서 blob을 삭제 하는 데 사용 되 고 다음 코드 예제에 표시 됩니다.

```csharp
public static async Task<bool> DeleteFileAsync(ContainerType containerType, string name)
{
  var container = GetContainer(containerType);
  var blob = container.GetBlobReference(name);
  return await blob.DeleteIfExistsAsync();
}
```

메서드를 컨테이너 참조를 검색 한 후 지정된 된 blob에 대 한 blob 참조를 검색 합니다. Blob은와 삭제는 `DeleteIfExistsAsync` 메서드.

## <a name="summary"></a>요약

이 문서에 Xamarin.Forms를 사용 하 여 Azure 저장소에 텍스트 및 이진 데이터를 저장 하는 방법 및 데이터에 액세스 하는 방법을 보여 줍니다. Azure 저장소에는, 구조화 되지 않은 작업과 구조화 된 데이터를 저장 하는 데 사용할 수 있는 확장 가능한 클라우드 저장소 솔루션입니다.


## <a name="related-links"></a>관련 링크

- [Azure 저장소 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureStorage/)
- [저장소 소개](https://azure.microsoft.com/documentation/articles/storage-introduction/)
- [Xamarin에서 Blob 저장소를 사용 하는 방법](https://azure.microsoft.com/documentation/articles/storage-xamarin-blob-storage/)
- [공유 액세스 서명 (SAS)를 사용 하 여](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)
- [Windows Azure 저장소](https://www.nuget.org/packages/WindowsAzure.Storage/)
