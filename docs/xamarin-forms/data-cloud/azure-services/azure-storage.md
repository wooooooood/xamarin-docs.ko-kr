---
title: Azure Storage의 데이터 저장 및 액세스Xamarin.Forms
description: Azure Storage는 구조화 되지 않은 구조화 된 데이터와 구조화 된 데이터를 저장 하는 데 사용할 수 있는 확장 가능한 클라우드 저장소 솔루션입니다. 이 문서에서는를 사용 하 여 Xamarin.Forms Azure Storage에서 텍스트 및 이진 데이터를 저장 하는 방법과 데이터에 액세스 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 5B10D37B-839B-4CD0-9C65-91014A93F3EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/28/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a69edd3bf014809cc479dcb7cba0e430dcefbe5b
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84564682"
---
# <a name="store-and-access-data-in-azure-storage-from-xamarinforms"></a>Azure Storage의 데이터 저장 및 액세스Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azurestorage)

_Azure Storage는 구조화 되지 않은 구조화 된 데이터와 구조화 된 데이터를 저장 하는 데 사용할 수 있는 확장 가능한 클라우드 저장소 솔루션입니다. 이 문서에서는를 사용 하 여 Xamarin.Forms Azure Storage에서 텍스트 및 이진 데이터를 저장 하는 방법과 데이터에 액세스 하는 방법을 보여 줍니다._

Azure Storage는 다음과 같은 4 개의 저장소 서비스를 제공 합니다.

- Blob Storage. Blob은 백업, 가상 컴퓨터, 미디어 파일 또는 문서와 같은 텍스트 또는 이진 데이터 일 수 있습니다.
- Table Storage는 NoSQL 키-특성 저장소입니다.
- Queue Storage는 클라우드 서비스 간의 워크플로 처리 및 통신을 위한 메시징 서비스입니다.
- File Storage은 SMB 프로토콜을 사용 하 여 공유 저장소를 제공 합니다.

스토리지 계정에는 다음과 같은 두 종류가 있습니다.

- 범용 저장소 계정은 단일 계정에서 Azure Storage 서비스에 대 한 액세스를 제공 합니다.
- Blob 저장소 계정은 blob을 저장 하기 위한 특수 저장소 계정입니다. 이 계정 유형은 blob 데이터만 저장 해야 하는 경우에 권장 됩니다.

이 문서와 함께 제공 되는 샘플 응용 프로그램은 이미지와 텍스트 파일을 blob 저장소에 업로드 하 고 다운로드 하는 방법을 보여 줍니다. 또한 blob 저장소에서 파일 목록을 검색 하 고 파일을 삭제 하는 방법도 보여 줍니다.

Azure Storage에 대 한 자세한 내용은 [저장소 소개](https://azure.microsoft.com/documentation/articles/storage-introduction/)를 참조 하세요.

> [!NOTE]
> [Azure 구독](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)이 아직 없는 경우 시작하기 전에 [체험 계정](https://aka.ms/azfree-docs-mobileapps)을 만듭니다.

## <a name="introduction-to-blob-storage"></a>Blob Storage 소개

Blob storage는 다음 다이어그램에 표시 된 세 가지 구성 요소로 구성 됩니다.

![](azure-storage-images/blob-storage.png "Blob Storage Concepts")

Azure Storage에 대 한 모든 액세스는 저장소 계정을 통해 사용 됩니다. 저장소 계정에는 개수에 제한 없이 컨테이너를 포함할 수 있으며, 컨테이너는 저장소 계정의 최대 용량 한도까지 blob을 무제한으로 저장할 수 있습니다.

Blob은 모든 형식 및 크기의 파일입니다. Azure Storage는 다음과 같은 세 가지 blob 유형을 지원 합니다.

- 블록 blob은 클라우드 개체를 스트리밍하 고 저장 하기 위해 최적화 되었으며 백업, 미디어 파일, 문서 등을 저장 하는 데 적합 합니다. 블록 blob의 크기는 최대 195Gb 일 수 있습니다.
- 추가 blob은 블록 blob과 유사 하지만 로깅 등의 추가 작업에 최적화 되어 있습니다. 추가 blob의 크기는 최대 195Gb 일 수 있습니다.
- 페이지 blob은 빈번한 읽기/쓰기 작업에 최적화 되어 있으며 일반적으로 가상 컴퓨터 및 해당 디스크를 저장 하는 데 사용 됩니다. 페이지 blob의 크기는 최대 1Tb 일 수 있습니다.

> [!NOTE]
> Blob storage 계정은 블록 및 추가 blob을 지원 하지만 페이지 blob은 지원 하지 않습니다.

Blob은 Azure Storage로 업로드 되 고 Azure Storage에서 바이트 스트림으로 다운로드 됩니다. 따라서 파일을 업로드 하기 전에 바이트 스트림으로 변환 하 고 다운로드 후 원래 표현으로 다시 변환 해야 합니다.

Azure Storage에 저장 된 모든 개체에는 고유한 URL 주소가 있습니다. 저장소 계정 이름은 해당 주소의 하위 도메인을 구성 하 고 하위 도메인 및 도메인 이름 조합은 저장소 계정에 대 한 *끝점* 을 형성 합니다. 예를 들어 저장소 계정의 이름이 *mystorageaccount*인 경우 저장소 계정의 기본 blob 끝점은 `https://mystorageaccount.blob.core.windows.net` 입니다.

스토리지 계정의 개체에 액세스하기 위한 URL은 스토리지 계정의 개체 위치를 엔드포인트에 추가하여 작성됩니다. 예를 들어, blob 주소는 형식을 갖습니다 `https://mystorageaccount.blob.core.windows.net/mycontainer/myblob` .

## <a name="setup"></a>설치 프로그램

Azure Storage 계정을 응용 프로그램에 통합 하는 프로세스는 다음과 같습니다 Xamarin.Forms .

1. 스토리지 계정을 만듭니다. 자세한 내용은 [저장소 계정 만들기](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account)를 참조 하세요.
1. 응용 프로그램에 [Azure Storage 클라이언트 라이브러리](https://www.nuget.org/packages/WindowsAzure.Storage/) 를 추가 Xamarin.Forms 합니다.
1. 저장소 연결 문자열을 구성 합니다. 자세한 내용은 [Azure Storage에 연결](#connecting-to-azure-storage)을 참조 하세요.
1. `using` `Microsoft.WindowsAzure.Storage` `Microsoft.WindowsAzure.Storage.Blob` Azure Storage에 액세스 하는 클래스에 및 네임 스페이스에 대 한 지시문을 추가 합니다.

## <a name="connecting-to-azure-storage"></a>Azure Storage에 연결

저장소 계정 리소스에 대 한 모든 요청을 인증 해야 합니다. 익명 인증을 지원 하도록 blob을 구성할 수 있지만 응용 프로그램에서 저장소 계정으로 인증 하는 데 사용할 수 있는 두 가지 주요 방법은 다음과 같습니다.

- 공유 키. 이 방법은 Azure Storage 계정 이름과 계정 키를 사용 하 여 저장소 서비스에 액세스 합니다. 저장소 계정에는 만들 때 공유 키 인증에 사용할 수 있는 두 개의 개인 키가 할당 됩니다.
- 공유 액세스 서명입니다. URL에 추가할 수 있는 토큰으로, 지정 된 사용 권한을 사용 하 여 저장소 리소스에 대 한 위임 된 액세스를 사용 하 여 유효한 기간을 지정 합니다.

응용 프로그램에서 Azure Storage 리소스에 액세스 하는 데 필요한 인증 정보를 포함 하는 연결 문자열을 지정할 수 있습니다. 또한 Visual Studio에서 Azure Storage 에뮬레이터에 연결 하도록 연결 문자열을 구성할 수 있습니다.

> [!NOTE]
> Azure Storage는 연결 문자열에서 HTTP 및 HTTPS를 지원 합니다. 그러나 HTTPS를 사용 하는 것이 좋습니다.

### <a name="connecting-to-the-azure-storage-emulator"></a>Azure Storage 에뮬레이터에 연결

Azure Storage 에뮬레이터는 개발 목적으로 Azure blob, 큐 및 table service를 에뮬레이트하는 로컬 환경을 제공 합니다.

Azure Storage 에뮬레이터에 연결 하려면 다음 연결 문자열을 사용 해야 합니다.

```csharp
UseDevelopmentStorage=true
```

Azure Storage 에뮬레이터에 대 한 자세한 내용은 [개발 및 테스트에 Azure Storage 에뮬레이터 사용](https://azure.microsoft.com/documentation/articles/storage-use-emulator/)을 참조 하세요.

### <a name="connecting-to-azure-storage-using-a-shared-key"></a>공유 키를 사용 하 여 Azure Storage에 연결

다음 연결 문자열 형식은 공유 키를 사용 하 여 Azure Storage에 연결 하는 데 사용 해야 합니다.

```csharp
DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey
```

`myAccountName`는 저장소 계정 이름으로 바꾸어야 하며, `myAccountKey` 두 계정 액세스 키 중 하나로 바꾸어야 합니다.

> [!NOTE]
> 공유 키 인증을 사용 하는 경우 계정 이름 및 계정 키가 응용 프로그램을 사용 하는 각 사용자에 게 배포 되며,이는 저장소 계정에 대 한 전체 읽기/쓰기 액세스를 제공 합니다. 따라서 테스트 목적 으로만 공유 키 인증을 사용 하 고 다른 사용자에 게 키를 배포 하지 마십시오.

### <a name="connecting-to-azure-storage-using-a-shared-access-signature"></a>공유 액세스 서명을 사용 하 여 Azure Storage에 연결

다음 연결 문자열 형식은 SAS를 사용 하 여 Azure Storage에 연결 하는 데 사용 해야 합니다.

`BlobEndpoint=myBlobEndpoint;SharedAccessSignature=mySharedAccessSignature`

`myBlobEndpoint`는 blob 끝점의 URL로 바꾸어야 하며 `mySharedAccessSignature` SAS로 바꾸어야 합니다. SAS는 프로토콜, 서비스 끝점 및 리소스에 액세스 하기 위한 자격 증명을 제공 합니다.

> [!NOTE]
> SAS 인증은 프로덕션 응용 프로그램에 권장 됩니다. 그러나 프로덕션 응용 프로그램에서 SAS는 응용 프로그램과 함께 제공 되는 것이 아니라 요청 시 백 엔드 서비스에서 검색 되어야 합니다.

공유 액세스 서명에 대 한 자세한 내용은 [SAS (공유 액세스 서명) 사용](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)을 참조 하세요.

## <a name="creating-a-container"></a>컨테이너 만들기

`GetContainer`메서드는 명명 된 컨테이너에 대 한 참조를 검색 하는 데 사용 되며,이를 사용 하 여 컨테이너에서 blob을 검색 하거나 컨테이너에 blob을 추가할 수 있습니다. 다음 코드 예제는 `GetContainer` 메서드를 보여줍니다.

```csharp
static CloudBlobContainer GetContainer(ContainerType containerType)
{
  var account = CloudStorageAccount.Parse(Constants.StorageConnection);
  var client = account.CreateCloudBlobClient();
  return client.GetContainerReference(containerType.ToString().ToLower());
}
```

`CloudStorageAccount.Parse`메서드는 연결 문자열을 구문 분석 하 고 `CloudStorageAccount` 저장소 계정을 나타내는 인스턴스를 반환 합니다. `CloudBlobClient`컨테이너 및 blob을 검색 하는 데 사용 되는 인스턴스는 메서드에 의해 생성 됩니다 `CreateCloudBlobClient` . `GetContainerReference`메서드는 `CloudBlobContainer` 호출 메서드로 반환 되기 전에 지정 된 컨테이너를 인스턴스로 검색 합니다. 이 예에서 컨테이너 이름은 `ContainerType` 소문자 문자열로 변환 된 열거 값입니다.

> [!NOTE]
> 컨테이너 이름은 소문자 여야 하 고 문자나 숫자로 시작 해야 합니다. 또한 문자, 숫자 및 대시 문자만 포함할 수 있으며, 길이가 3 ~ 007e; 63 자 사이 여야 합니다.

메서드는 다음과 `GetContainer` 같이 호출 됩니다.

```csharp
var container = GetContainer(containerType);
```

`CloudBlobContainer`그런 다음 인스턴스는 아직 없는 경우 컨테이너를 만드는 데 사용할 수 있습니다.

```csharp
await container.CreateIfNotExistsAsync();
```

기본적으로 새로 만든 컨테이너는 전용 컨테이너입니다. 이는 컨테이너에서 blob을 검색 하기 위해 저장소 액세스 키를 지정 해야 함을 의미 합니다. 컨테이너 내에서 blob을 공용으로 만드는 방법에 대 한 자세한 내용은 [컨테이너 만들기](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#create-a-container)를 참조 하세요.

## <a name="uploading-data-to-a-container"></a>컨테이너에 데이터 업로드

`UploadFileAsync`메서드는 바이트 데이터 스트림을 blob 저장소에 업로드 하는 데 사용 되며, 다음 코드 예제에서 볼 수 있습니다.

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

컨테이너 참조를 검색 한 후에는 메서드가 아직 없는 경우 컨테이너를 만듭니다. `Guid`그러면 고유한 blob 이름으로 사용할 새이 만들어지고 blob 블록 참조가 인스턴스로 검색 됩니다 `CloudBlockBlob` . 그런 다음 blob을 만드는 메서드를 사용 하 여 blob에 데이터 스트림을 업로드 합니다 .이는 blob이 없는 경우에는 `UploadFromStreamAsync` blob을 만들고, 그렇지 않으면 덮어씁니다.

이 메서드를 사용 하 여 blob 저장소에 파일을 업로드 하려면 먼저 바이트 스트림으로 변환 해야 합니다. 이는 다음 코드 예제에서 보여 줍니다.

```csharp
var byteData = Encoding.UTF8.GetBytes(text);
uploadedFilename = await AzureStorage.UploadFileAsync(ContainerType.Text, new MemoryStream(byteData));
```

`text`데이터는 바이트 배열로 변환 된 다음 메서드에 전달 되는 스트림으로 래핑됩니다 `UploadFileAsync` .

## <a name="downloading-data-from-a-container"></a>컨테이너에서 데이터 다운로드

`GetFileAsync`메서드는 Azure Storage에서 blob 데이터를 다운로드 하는 데 사용 되며, 다음 코드 예제에서 볼 수 있습니다.

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

컨테이너 참조를 검색 한 후 메서드는 저장 된 데이터에 대 한 blob 참조를 검색 합니다. Blob이 있는 경우 해당 속성은 메서드에 의해 검색 됩니다 `FetchAttributesAsync` . 올바른 크기의 바이트 배열이 만들어지고 blob는 호출 메서드에 반환 되는 바이트 배열로 다운로드 됩니다.

Blob 바이트 데이터를 다운로드 한 후 원래 표현으로 변환 해야 합니다. 이는 다음 코드 예제에서 보여 줍니다.

```csharp
var byteData = await AzureStorage.GetFileAsync(ContainerType.Text, uploadedFilename);
string text = Encoding.UTF8.GetString(byteData);
```

바이트 배열은 메서드에 의해 Azure Storage에서 검색 된 `GetFileAsync` 후 UTF8로 인코딩된 문자열로 다시 변환 되기 전에 검색 됩니다.

## <a name="listing-data-in-a-container"></a>컨테이너에 데이터 나열

`GetFilesListAsync`메서드는 컨테이너에 저장 된 blob 목록을 검색 하는 데 사용 되며, 다음 코드 예제에서 볼 수 있습니다.

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

컨테이너 참조를 검색 한 후 메서드는 컨테이너의 메서드를 사용 하 여 `ListBlobsSegmentedAsync` 컨테이너 내의 blob에 대 한 참조를 검색 합니다. 메서드가 반환 하는 결과는 `ListBlobsSegmentedAsync` `BlobContinuationToken` 인스턴스가이 아닌 동안 열거 됩니다 `null` . 각 blob은 `IListBlobItem` `CloudBlockBlob` `Name` 값이 컬렉션에 추가 되기 전에 blob의 속성에 액세스 하기 위해 반환 된에서로 캐스팅 됩니다 `allBlobsList` . `BlobContinuationToken`인스턴스가 이면 `null` 마지막 blob 이름이 반환 되 고 실행이 루프를 종료 합니다.

## <a name="deleting-data-from-a-container"></a>컨테이너에서 데이터 삭제

`DeleteFileAsync`메서드는 컨테이너에서 blob을 삭제 하는 데 사용 되며, 다음 코드 예제에서 볼 수 있습니다.

```csharp
public static async Task<bool> DeleteFileAsync(ContainerType containerType, string name)
{
  var container = GetContainer(containerType);
  var blob = container.GetBlobReference(name);
  return await blob.DeleteIfExistsAsync();
}
```

컨테이너 참조를 검색 한 후 메서드는 지정 된 blob에 대 한 blob 참조를 검색 합니다. 그런 다음 메서드를 사용 하 여 blob를 삭제 합니다 `DeleteIfExistsAsync` .

## <a name="related-links"></a>관련 링크

- [Azure Storage (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azurestorage)
- [저장소 소개](https://azure.microsoft.com/documentation/articles/storage-introduction/)
- [Xamarin에서 Blob Storage를 사용하는 방법](https://azure.microsoft.com/documentation/articles/storage-xamarin-blob-storage/)
- [SAS(공유 액세스 서명) 사용](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)
- [Windows Azure Storage (NuGet)](https://www.nuget.org/packages/WindowsAzure.Storage/)
