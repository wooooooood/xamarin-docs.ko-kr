---
title: Xamarin.ios의 사진 키트
description: 이 문서에서는 사진 키트에 대해 설명 하 고, 모델 개체에 대해 설명 하 고, 모델 데이터를 쿼리하고, 변경 내용을 사진 라이브러리에 저장 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 7FDEE394-3787-40FA-8372-76A05BF184B3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: 5e5cc20e9fbeaf2b00e022ccdbf67286aed6d5ef
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528815"
---
# <a name="photokit-in-xamarinios"></a>Xamarin.ios의 사진 키트

사진 키트는 응용 프로그램이 시스템 이미지 라이브러리를 쿼리하고 해당 내용을 보고 수정할 수 있는 사용자 지정 사용자 인터페이스를 만들 수 있도록 하는 새로운 프레임 워크입니다. 여기에는 이미지 및 비디오 자산을 나타내는 여러 클래스 뿐만 아니라 앨범 및 폴더와 같은 자산 컬렉션도 포함 됩니다.

## <a name="model-objects"></a>모델 개체

사진 키트는 모델 개체를 호출 하는 항목에서 이러한 자산을 나타냅니다. 사진과 비디오 자체를 나타내는 모델 개체는 형식 `PHAsset`입니다. 에 `PHAsset` 는 자산의 미디어 유형 및 만든 날짜와 같은 메타 데이터가 포함 됩니다.
마찬가지로 및 `PHAssetCollection` `PHCollectionList` 클래스에는 각각 자산 컬렉션과 컬렉션 목록에 대 한 메타 데이터가 포함 됩니다. 자산 컬렉션은 지정 된 연도의 모든 사진과 비디오와 같은 자산 그룹입니다. 마찬가지로 컬렉션 목록은 연도별로 그룹화 된 사진 및 비디오와 같은 자산 컬렉션 그룹입니다.

## <a name="querying-model-data"></a>모델 데이터 쿼리

사진 키트를 사용 하면 다양 한 fetch 메서드를 통해 모델 데이터를 쉽게 쿼리할 수 있습니다. 예를 들어 모든 이미지를 검색 하려면를 호출 `PFAsset.Fetch`하 여 `PHAssetMediaType.Image` 미디어 형식을 전달 합니다.

```csharp
PHFetchResult fetchResults = PHAsset.FetchAssets (PHAssetMediaType.Image, null);
```

그러면 `PHFetchResult` 인스턴스는 이미지를 나타내는 `PFAsset` 모든 인스턴스를 포함 합니다. 이미지 자체를 가져오려면 `PHImageManager` (또는 캐싱 `PHCachingImageManager`버전)를 사용 하 여를 호출 `RequestImageForAsset`하 여 이미지에 대 한 요청을 수행 합니다. 예를 들어 다음 코드는 컬렉션 뷰 셀에 표시할의 `PHFetchResult` 각 자산에 대 한 이미지를 검색 합니다.

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

![](photokit-images/image4.png "이미지 그리드를 표시 하는 실행 중인 앱")

## <a name="saving-changes-to-the-photo-library"></a>사진 라이브러리의 변경 내용 저장

데이터 쿼리 및 읽기를 처리 하는 방법입니다. 또한 변경 내용을 라이브러리에 다시 쓸 수 있습니다. 많은 관심 응용 프로그램이 시스템 사진 라이브러리와 상호 작용할 수 있으므로 PhotoLibraryObserver를 사용 하 여 변경 내용에 대 한 알림이 표시 되도록 관찰자를 등록할 수 있습니다. 그런 다음 변경 내용이 들어오면 응용 프로그램이 그에 따라 업데이트 될 수 있습니다. 예를 들어 다음은 위의 컬렉션 뷰를 다시 로드 하는 간단한 구현입니다.

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

실제로 응용 프로그램에서 변경 내용을 다시 작성 하려면 변경 요청을 만듭니다. 각 모델 클래스에는 연결 된 변경 요청 클래스가 있습니다. 예를 들어 PHAsset를 변경 하려면 PHAssetChangeRequest를 만듭니다. 사진 라이브러리에 다시 기록 되 고 위와 같은 관찰자에 게 전송 되는 변경 내용을 수행 하는 단계는 다음과 같습니다.

- 편집 작업을 수행 합니다.
- PHContentEditingOutput 인스턴스에 필터링 된 이미지 데이터를 저장 합니다.
- 편집 출력에 변경 내용을 게시 하는 변경 요청을 만듭니다.

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

![](photokit-images/image5.png "적용 되는 필터의 예")

PHPhotoLibraryChangeObserver 덕분에 사용자가 뒤로 이동할 때 변경 내용이 컬렉션 뷰에 반영 됩니다.

![](photokit-images/image6.png "사용자가 다시 탐색할 때 컬렉션 뷰에 변경 내용이 반영 됩니다.")
