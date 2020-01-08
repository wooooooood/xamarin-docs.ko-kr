---
title: Xamarin.ios의 사진 키트
description: 이 문서에서는 사진 키트에 대해 설명 하 고, 모델 개체에 대해 설명 하 고, 모델 데이터를 쿼리하고, 변경 내용을 사진 라이브러리에 저장 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 7FDEE394-3787-40FA-8372-76A05BF184B3
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: def34efd1fd48cc0e7dd802a6d3e843be1e156a4
ms.sourcegitcommit: 5ddb107b0a56bef8a16fce5bc6846f9673b3b22e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75558809"
---
# <a name="photokit-in-xamarinios"></a>Xamarin.ios의 사진 키트

[샘플 다운로드 ![코드 샘플 다운로드](~/media/shared/download.png)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-samplephotoapp/)

사진 키트는 응용 프로그램이 시스템 이미지 라이브러리를 쿼리하고 해당 내용을 보고 수정할 수 있는 사용자 지정 사용자 인터페이스를 만들 수 있도록 하는 프레임 워크입니다. 여기에는 이미지 및 비디오 자산을 나타내는 여러 클래스 뿐만 아니라 앨범 및 폴더와 같은 자산 컬렉션도 포함 됩니다.

## <a name="permissions"></a>권한

앱이 사진 라이브러리에 액세스 하기 전에 사용자에 게 사용 권한 대화 상자가 표시 됩니다. **Info.plist** 파일에 설명 텍스트를 제공 하 여 앱에서 사진 라이브러리를 사용 하는 방법을 설명 해야 합니다. 예를 들면 다음과 같습니다.

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Applies filters to photos and updates the original image</string>
```

## <a name="model-objects"></a>모델 개체

사진 키트는 모델 개체를 호출 하는 항목에서 이러한 자산을 나타냅니다. 사진과 비디오 자체를 나타내는 모델 개체는 `PHAsset`유형입니다. `PHAsset`에는 자산의 미디어 유형 및 만든 날짜와 같은 메타 데이터가 포함 됩니다.
마찬가지로 `PHAssetCollection` 및 `PHCollectionList` 클래스에는 각각 자산 컬렉션 및 컬렉션 목록에 대 한 메타 데이터가 포함 됩니다. 자산 컬렉션은 지정 된 연도의 모든 사진과 비디오와 같은 자산 그룹입니다. 마찬가지로 컬렉션 목록은 연도별로 그룹화 된 사진 및 비디오와 같은 자산 컬렉션 그룹입니다.

## <a name="querying-model-data"></a>모델 데이터 쿼리

사진 키트를 사용 하면 다양 한 fetch 메서드를 통해 모델 데이터를 쉽게 쿼리할 수 있습니다. 예를 들어 모든 이미지를 검색 하려면 `PHAsset.Fetch`를 호출 하 여 `PHAssetMediaType.Image` 미디어 형식을 전달 합니다.

```csharp
PHFetchResult fetchResults = PHAsset.FetchAssets (PHAssetMediaType.Image, null);
```

그러면 `PHFetchResult` 인스턴스에 이미지를 나타내는 모든 `PHAsset` 인스턴스가 포함 됩니다. 이미지 자체를 가져오려면 `PHImageManager` (또는 캐싱 버전 `PHCachingImageManager`)를 사용 하 여 `RequestImageForAsset`를 호출 하 여 이미지에 대 한 요청을 수행 합니다. 예를 들어 다음 코드는 컬렉션 뷰 셀에 표시할 `PHFetchResult`의 각 자산에 대 한 이미지를 검색 합니다.

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    var imageCell = (ImageCell)collectionView.DequeueReusableCell (cellId, indexPath);
    imageMgr.RequestImageForAsset (
        (PHAsset)fetchResults [(uint)indexPath.Item],
        thumbnailSize,
        PHImageContentMode.AspectFill, new PHImageRequestOptions (),
        (img, info) => {
            imageCell.ImageView.Image = img;
        }
    );
    return imageCell;
}
```

이로 인해 아래와 같이 이미지가 표로 표시 됩니다.

![](photokit-images/image4.png "The running app displaying a grid of images")

## <a name="saving-changes-to-the-photo-library"></a>사진 라이브러리의 변경 내용 저장

데이터 쿼리 및 읽기를 처리 하는 방법입니다. 또한 변경 내용을 라이브러리에 다시 쓸 수 있습니다. 많은 관심 응용 프로그램이 시스템 사진 라이브러리와 상호 작용할 수 있기 때문에 `PhotoLibraryObserver`를 사용 하 여 변경 내용을 알리도록 관찰자를 등록할 수 있습니다. 그런 다음 변경 내용이 들어오면 응용 프로그램이 그에 따라 업데이트 될 수 있습니다. 예를 들어 다음은 위의 컬렉션 뷰를 다시 로드 하는 간단한 구현입니다.

```csharp
class PhotoLibraryObserver : PHPhotoLibraryChangeObserver
{
    readonly PhotosViewController controller;
    public PhotoLibraryObserver (PhotosViewController controller)

    {
        this.controller = controller;
    }

    public override void PhotoLibraryDidChange (PHChange changeInstance)
    {
        DispatchQueue.MainQueue.DispatchAsync (() => {
            var changes = changeInstance.GetFetchResultChangeDetails (controller.fetchResults);
            controller.fetchResults = changes.FetchResultAfterChanges;
            controller.CollectionView.ReloadData ();
        });
    }
}
```

실제로 응용 프로그램에서 변경 내용을 다시 작성 하려면 변경 요청을 만듭니다. 각 모델 클래스에는 연결 된 변경 요청 클래스가 있습니다. 예를 들어 `PHAsset`를 변경 하려면 `PHAssetChangeRequest`를 만듭니다. 사진 라이브러리에 다시 기록 되 고 위와 같은 관찰자에 게 전송 되는 변경 내용을 수행 하는 단계는 다음과 같습니다.

1. 편집 작업을 수행 합니다.
2. 필터링 된 이미지 데이터를 `PHContentEditingOutput` 인스턴스에 저장 합니다.
3. 변경 내용을 편집 출력에서 게시 하도록 변경 요청을 만듭니다.

다음은 Core 이미지 noir 필터를 적용 하는 이미지에 대 한 변경 내용을 다시 기록 하는 예제입니다.

```csharp
void ApplyNoirFilter (object sender, EventArgs e)
{
    Asset.RequestContentEditingInput (new PHContentEditingInputRequestOptions (), (input, options) => {

        // perform the editing operation, which applies a noir filter in this case
        var image = CIImage.FromUrl (input.FullSizeImageUrl);
        image = image.CreateWithOrientation((CIImageOrientation)input.FullSizeImageOrientation);
        var noir = new CIPhotoEffectNoir {
            Image = image
        };
        var ciContext = CIContext.FromOptions (null);
        var output = noir.OutputImage;
        var uiImage = UIImage.FromImage (ciContext.CreateCGImage (output, output.Extent));
        imageView.Image = uiImage;
        //
        // save the filtered image data to a PHContentEditingOutput instance
        var editingOutput = new PHContentEditingOutput(input);
        var adjustmentData = new PHAdjustmentData();
        var data = uiImage.AsJPEG();
        NSError error;
        data.Save(editingOutput.RenderedContentUrl, false, out error);
        editingOutput.AdjustmentData = adjustmentData;
        //
        // make a change request to publish the changes form the editing output
        PHPhotoLibrary.GetSharedPhotoLibrary.PerformChanges (() => {
            PHAssetChangeRequest request = PHAssetChangeRequest.ChangeRequest(Asset);
            request.ContentEditingOutput = editingOutput;
        },
        (ok, err) => Console.WriteLine ("photo updated successfully: {0}", ok));
    });
}
```

사용자가 단추를 선택 하면 필터가 적용 됩니다.

![필터를 적용 하기 전후에 사진을 보여 주는 두 가지 예제](photokit-images/image5.png)

`PHPhotoLibraryChangeObserver`덕분에이 변경 내용은 사용자가 뒤로 이동할 때 컬렉션 뷰에 반영 됩니다.

![수정 된 사진을 보여 주는 사진 컬렉션 뷰](photokit-images/image6.png)
