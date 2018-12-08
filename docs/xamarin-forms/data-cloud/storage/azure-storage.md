---
title: Azure Storage에 데이터 저장 및 액세스
description: Azure Storage는 비 구조화 및 구조화 된 데이터를 저장 하는 데 사용할 수 있는 확장 가능한 클라우드 저장소 솔루션입니다. 이 문서에서는 Xamarin.Forms를 사용 하 여 Azure Storage에 텍스트 및 이진 데이터를 저장 하는 방법을 데이터에 액세스 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 5B10D37B-839B-4CD0-9C65-91014A93F3EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 1f920eb36eab3e451b20aa91734f00cee5ba6485
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53059222"
---
# <a name="storing-and-accessing-data-in-azure-storage"></a>Azure Storage에 데이터 저장 및 액세스

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureStorage/)

_Azure Storage는 비 구조화 및 구조화 된 데이터를 저장 하는 데 사용할 수 있는 확장 가능한 클라우드 저장소 솔루션입니다. 이 문서에서는 Xamarin.Forms를 사용 하 여 Azure Storage에 텍스트 및 이진 데이터를 저장 하는 방법을 데이터에 액세스 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

Azure Storage는 4 개의 저장소 서비스를 제공합니다.

- Blob 저장소입니다. Blob에 백업, virtual machines, 미디어 파일 또는 문서와 같은 텍스트 또는 이진 데이터를 수 있습니다.
- 테이블 저장소는 NoSQL 키-특성 저장소입니다.
- 큐 저장소는 워크플로 처리 및 클라우드 서비스 간의 통신에 대 한 메시징 서비스입니다.
- File Storage는 SMB 프로토콜을 사용 하 여 공유 저장소를 제공 합니다.

저장소 계정의 다음과 같은 두 종류가 있습니다.

- 범용 저장소 계정을 단일 계정에서 Azure Storage 서비스에 액세스를 제공 합니다.
- Blob storage 계정에 blob 저장에 대 한 특수 저장소 계정이입니다. Blob 데이터를 저장 해야 하는 경우이 계정 유형은 것이 좋습니다.

이 문서 및 샘플 응용 프로그램을 함께 제공 되는 blob storage 및 다운로드에 이미지 및 텍스트 파일을 업로드를 보여 줍니다. 또한 보여 줍니다 blob storage에서 파일의 목록을 검색 하 고 파일을 삭제 합니다.

Azure Storage에 대 한 자세한 내용은 참조 하세요. [Storage 소개](https://azure.microsoft.com/documentation/articles/storage-introduction/)합니다.

## <a name="introduction-to-blob-storage"></a>Blob Storage 소개

Blob 저장소는 다음 다이어그램에 나와 있는 세 가지 구성 요소가 구성 됩니다.

![](azure-storage-images/blob-storage.png "Blob 저장소 개념")

Azure Storage에 대 한 모든 액세스는 저장소 계정을 통해 이루어집니다. 저장소 계정 컨테이너를 개수에 제한 없이 포함 될 수 있습니다 및 컨테이너에 blob 저장소 계정의 용량 제한까지 개수에 제한 없이 저장할 수 있습니다.

Blob에는 모든 형식과 크기의 파일입니다. Azure Storage에는 세 가지 다른 blob 유형을 지원합니다.

- 블록 blob는 클라우드 개체를 저장 및 스트리밍에 대 한 액세스에 최적화 된 및 백업, 미디어 파일, 문서 등을 저장 하는 데 적합 합니다. 블록 blob은 최대 195gb 크기에서 될 수 있습니다.
- 추가 blob은 블록 blob와 유사 하지만에 최적화 된 로깅 등의 작업을 추가 합니다. 추가 blob는 최대 195gb 크기에서 일 수 있습니다.
- 페이지 blob는 빈번한 읽기/쓰기 작업에 대 한 액세스에 최적화 된 및 가상 머신과 해당 디스크를 저장 하는 데 일반적으로 사용 됩니다. 페이지 blob은 최대 1tb 크기 수 있습니다.

> [!NOTE]
> blob storage 계정은 블록 및 추가 지원 blob을 사용 하지만 페이지 blob이 아닌 참고 합니다.

Blob은 Azure Storage에 업로드 및 바이트 스트림으로 Azure Storage에서 다운로드 합니다. 따라서 파일 업로드 및 다운로드 한 후 원래 해당 표현 다시 변환된 하기 전에 바이트 스트림을 변환할 수 있어야 합니다.

Azure Storage에 저장 된 모든 개체에 고유한 URL 주소가 있습니다. 해당 주소 및 하위 도메인 및 도메인 이름 형식의 조합을의 하위 도메인을 구성 하는 저장소 계정 이름은 *끝점* 저장소 계정에 대 한 합니다. 예를 들어 저장소 계정 이름은 *mystorageaccount*, 저장소 계정에 대 한 기본 blob 끝점은 `https://mystorageaccount.blob.core.windows.net`합니다.

저장소 계정의 개체에 액세스 하기 위한 URL 끝점에 저장소 계정에서 개체의 위치를 추가 하 여 빌드됩니다. 예를 들어 blob 주소는 형식이 `https://mystorageaccount.blob.core.windows.net/mycontainer/myblob`합니다.

## <a name="setup"></a>설정

Xamarin.Forms 응용 프로그램을 Azure Storage 계정 통합에 대 한 프로세스는 다음과 같습니다.

1. 저장소 계정을 만듭니다. 자세한 내용은 [저장소 계정 만들기](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account)합니다.
1. 추가 된 [Azure Storage Client Library](https://www.nuget.org/packages/WindowsAzure.Storage/) Xamarin.Forms 응용 프로그램입니다.
1. 저장소 연결 문자열을 구성 합니다. 자세한 내용은 [Azure Storage에 연결할](#connecting)합니다.
1. 추가 `using` 지시문에 대 한 합니다 `Microsoft.WindowsAzure.Storage` 및 `Microsoft.WindowsAzure.Storage.Blob` 네임 스페이스는 Azure Storage에 액세스 하는 클래스입니다.

> [!NOTE]
> 이 샘플은 공유 액세스 프로젝트를 사용 하는 동안 Azure Storage 클라이언트 라이브러리도 이제 이식 가능한 클래스 라이브러리 (PCL) 프로젝트에서 사용 중인 합니다.

<a name="connecting" />

## <a name="connecting-to-azure-storage"></a>Azure Storage에 연결

저장소 계정 리소스에 대 한 모든 요청은 인증 되어야 합니다. Blob을 익명 인증을 지원 하도록 구성할 수 있습니다. 두 가지 주 저장소 계정으로 인증을 사용 하 여:

- 공유 키입니다. 이 방법은 저장소 서비스에 액세스 하려면 Azure Storage 계정 이름과 계정 키를 사용합니다. 저장소 계정은 공유 키 인증에 사용할 수 있는 생성 시 두 개인 키에 할당 됩니다.
- 공유 액세스 서명입니다. 이것은 저장소 리소스에 대 한 위임 된 액세스를 사용 하도록 설정 하는 URL에 추가할 수 있는 토큰 권한으로를 지정 합니다, 유효 기간.

응용 프로그램에서 Azure Storage 리소스에 액세스 하는 데 필요한 인증 정보를 포함 하는 연결 문자열을 지정할 수 있습니다. 또한 Visual Studio에서 Azure Storage 에뮬레이터에 연결할 연결 문자열을 구성할 수 있습니다.

> [!NOTE]
> Azure Storage 연결 문자열에서 HTTP 및 HTTPS를 지원합니다. 그러나 HTTPS를 사용 하는 것이 좋습니다.

### <a name="connecting-to-the-azure-storage-emulator"></a>Azure Storage 에뮬레이터에 연결

Azure Storage 에뮬레이터는 Azure blob, 큐 및 table services를 개발 목적으로 에뮬레이트하는 로컬 환경을 제공 합니다.

다음 연결 문자열을 사용 하 여 Azure Storage 에뮬레이터에 연결할 수 해야:

```csharp
UseDevelopmentStorage=true
```

Azure Storage 에뮬레이터에 대 한 자세한 내용은 참조 하세요. [개발 및 테스트에 대 한 Azure Storage 에뮬레이터를 사용 하 여](https://azure.microsoft.com/documentation/articles/storage-use-emulator/)입니다.

### <a name="connecting-to-azure-storage-using-a-shared-key"></a>공유 키를 사용 하 여 Azure Storage에 연결

공유 키를 사용 하 여 Azure Storage에 연결 하는 다음 연결 문자열 형식을 따라야 합니다.

```csharp
DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey
```

`myAccountName` 저장소 계정의 이름을 바꿔야 하 고 `myAccountKey` 두 계정 액세스 키 중 하나를 사용 하 여 교체 해야 합니다.

> [!NOTE]
> 키 인증, 계정 이름 및 계정 키 경우 공유를 사용 하 여 저장소 계정에 대 한 전체 읽기/쓰기 액세스를 제공 하는 프로그램을 사용 하는 각 사람에 게 배포 됩니다. 따라서 테스트 목적 으로만 공유 키 인증을 사용 하 고 다른 사용자에 게 키를 배포 하지 마십시오.

### <a name="connecting-to-azure-storage-using-a-shared-access-signature"></a>공유 액세스 서명을 사용 하 여 Azure Storage에 연결

SAS 사용 하 여 Azure Storage에 연결 하려면 다음 연결 문자열 형식을 따라야 합니다.

`BlobEndpoint=myBlobEndpoint;SharedAccessSignature=mySharedAccessSignature`

`myBlobEndpoint` blob 끝점의 URL로 바꿔야 하 고 `mySharedAccessSignature` SAS로 바꿔야 합니다. SAS는 프로토콜, 서비스 끝점 및 리소스에 액세스할 때 자격 증명을 제공 합니다.

> [!NOTE]
> 프로덕션 응용 프로그램에 대 한 SAS 인증 것이 좋습니다. 그러나 프로덕션 응용 프로그램의 SAS는 응용 프로그램과 함께 번들로 제공 되는 것이 아니라는 백 엔드 서비스에 요청에서 검색 됩니다.

공유 액세스 서명에 대 한 자세한 내용은 참조 하세요. [공유 액세스 서명 (SAS)를 사용 하 여](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)입니다.

## <a name="creating-a-container"></a>컨테이너 만들기

`GetContainer` 메서드 검색 컨테이너에서 blob를 검색 하거나 blob 컨테이너에 추가할 다음 사용할 수 있는 명명 된 컨테이너에 대 한 참조를 사용 합니다. 다음 코드 예제는 `GetContainer` 메서드:

```csharp
static CloudBlobContainer GetContainer(ContainerType containerType)
{
  var account = CloudStorageAccount.Parse(Constants.StorageConnection);
  var client = account.CreateCloudBlobClient();
  return client.GetContainerReference(containerType.ToString().ToLower());
}
```

합니다 `CloudStorageAccount.Parse` 연결 문자열을 구문 분석 하 고 반환 하는 메서드를 `CloudStorageAccount` 저장소 계정을 나타내는 인스턴스. A `CloudBlobClient` 하 여 컨테이너 및 blob을 검색 하는 경우 다음 만들어집니다는 `CreateCloudBlobClient` 메서드. 합니다 `GetContainerReference` 으로 지정된 된 컨테이너를 검색 하는 메서드를 `CloudBlobContainer` 인스턴스를 호출 하는 메서드로 반환 되기 전에 합니다. 이 예제의 컨테이너 이름은 `ContainerType` 열거형 값을 소문자 문자열로 변환 합니다.

> [!NOTE]
> 컨테이너 이름은 소문자 여야 하며 문자 또는 숫자로 시작 해야 합니다. 또한 문자, 숫자 및 대시 문자를 포함할 수 있습니다 않으며 3 ~ 63 자 사이 여야 합니다.

`GetContainer` 메서드는 다음과 같이 호출 됩니다.

```csharp
var container = GetContainer(containerType);
```

`CloudBlobContainer` 인스턴스 컨테이너를 만들려면 이미 존재 하지 않는 경우 사용할 수 있습니다.

```csharp
await container.CreateIfNotExistsAsync();
```

기본적으로 새로 만든된 컨테이너는 전용입니다. 이 컨테이너에서 blob을 검색할 저장소 액세스 키를 지정 해야 합니다는 것을 의미 합니다. 공용 컨테이너 내의 blob에 대 한 내용은 [컨테이너를 만드는](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#create-a-container)합니다.

## <a name="uploading-data-to-a-container"></a>컨테이너에 데이터 업로드

`UploadFileAsync` 메서드 바이트 데이터를 blob storage에 스트림을 업로드 하는 데 사용 되 고 다음 코드 예제에 표시 됩니다.

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

컨테이너 참조를 검색 한 후 메서드가 존재 하지 않는 경우 컨테이너를 만듭니다. 새 `Guid` 고유한 blob 이름으로 역할을 만든는 블록 blob 참조로 검색 됩니다는 `CloudBlockBlob` 인스턴스. 데이터 스트림을 사용 하 여 blob에 업로드 됩니다는 `UploadFromStreamAsync` 메서드인 파일이 있으면 덮어씁니다 되거나 존재 하지 않는 경우 blob을 만듭니다.

이 메서드를 사용 하 여 storage blob에 파일을 업로드할 수 있습니다, 전에 먼저 바이트 스트림으로 변환 합니다. 다음 코드 예제에서이 확인할 수 있습니다.

```csharp
var byteData = Encoding.UTF8.GetBytes(text);
uploadedFilename = await AzureStorage.UploadFileAsync(ContainerType.Text, new MemoryStream(byteData));
```

`text` 에 전달 되는 스트림으로 다음 래핑됩니다를 바이트 배열로 변환할 데이터를 `UploadFileAsync` 메서드.

## <a name="downloading-data-from-a-container"></a>컨테이너에서 데이터를 다운로드합니다.

`GetFileAsync` 메서드는 Azure Storage에서 blob 데이터를 다운로드 하 고 다음 코드 예제에 표시 됩니다.

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

메서드를 컨테이너 참조를 검색 한 후 저장된 된 데이터에 대 한 blob 참조를 검색 합니다. 해당 속성에서 검색 됩니다 blob이 존재 하는 경우는 `FetchAttributesAsync` 메서드. 올바른 크기의 바이트 배열 만들어지고 호출 메서드로 반환 되는 바이트 배열 blob 다운로드 됩니다.

Blob 바이트 데이터를 다운로드 한 후 원래 표현으로 변환 해야 합니다. 다음 코드 예제에서이 확인할 수 있습니다.

```csharp
var byteData = await AzureStorage.GetFileAsync(ContainerType.Text, uploadedFilename);
string text = Encoding.UTF8.GetString(byteData);
```

바이트의 배열 하 여 Azure Storage에서 검색 되는 `GetFileAsync` 로 인코딩된 문자열을 UTF8으로 변환 되기 전에 메서드.

## <a name="listing-data-in-a-container"></a>컨테이너에 있는 데이터를 나열합니다.

`GetFilesListAsync` 메서드 컨테이너에 저장 된 blob 목록을 검색 하는 데 사용 되 고 다음 코드 예제에 표시 됩니다.

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

컨테이너 참조를 검색 한 후 메서드를 사용 하 여 컨테이너의 `ListBlobsSegmentedAsync` 컨테이너 내의 blob에 대 한 참조를 검색 하는 방법입니다. 반환한 결과 `ListBlobsSegmentedAsync` 메서드를 열거 하는 동안 합니다 `BlobContinuationToken` 인스턴스가 `null`. 각 blob은 캐스팅에서 반환 된 `IListBlobItem` 에 `CloudBlockBlob` 순서 액세스에서를 `Name` 값인 전에 blob의 속성을 추가 `allBlobsList` 컬렉션. 한 번 합니다 `BlobContinuationToken` 인스턴스가 `null`, 반환 된 마지막으로 blob 이름 및 실행 루프를 종료 합니다.

## <a name="deleting-data-from-a-container"></a>컨테이너에서 데이터 삭제

`DeleteFileAsync` 메서드 컨테이너에서 blob을 삭제 하는 데 사용 되 고 다음 코드 예제에 표시 됩니다.

```csharp
public static async Task<bool> DeleteFileAsync(ContainerType containerType, string name)
{
  var container = GetContainer(containerType);
  var blob = container.GetBlobReference(name);
  return await blob.DeleteIfExistsAsync();
}
```

컨테이너 참조를 검색 한 후 메서드는 지정 된 blob에 대 한 blob 참조를 검색 합니다. Blob은 사용 하 여 삭제를 `DeleteIfExistsAsync` 메서드.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Forms를 사용 하 여 Azure Storage에 텍스트 및 이진 데이터를 저장 하는 방법 및 데이터에 액세스 하는 방법을 보여 줍니다. Azure Storage는 비 구조화 및 구조화 된 데이터를 저장 하는 데 사용할 수 있는 확장 가능한 클라우드 저장소 솔루션입니다.


## <a name="related-links"></a>관련 링크

- [Azure Storage (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureStorage/)
- [Storage 소개](https://azure.microsoft.com/documentation/articles/storage-introduction/)
- [Xamarin에서 Blob Storage를 사용 하는 방법](https://azure.microsoft.com/documentation/articles/storage-xamarin-blob-storage/)
- [공유 액세스 서명 (SAS)을 사용 하 여](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)
- [Windows Azure 저장소](https://www.nuget.org/packages/WindowsAzure.Storage/)
