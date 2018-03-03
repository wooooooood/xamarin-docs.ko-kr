---
title: PhotoKit
description: "사진 키트 앱을 시스템 이미지 라이브러리를 쿼리하고 보고 내용을 수정 하는 사용자 지정 UI를 만들 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 6f6858f36c30f45225417c78225926906481eb8d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="photokit"></a>PhotoKit

_사진 키트 앱을 시스템 이미지 라이브러리를 쿼리하고 보고 내용을 수정 하는 사용자 지정 UI를 만들 수 있습니다._

사진 키트는 응용 프로그램 시스템 이미지 라이브러리를 쿼리하고 보고 내용을 수정 하는 사용자 지정 사용자 인터페이스를 만들 수 있도록 하는 새로운 프레임 워크입니다. 다양 한 이미지 및 비디오 자산으로 앨범 및 폴더와 같은 자산 컬렉션을 나타내는 클래스를 포함 합니다.

## <a name="model-objects"></a>모델 개체
사진 키트에서는 모델 개체에서 이러한 자산을 나타냅니다. 사진 및 비디오 자신을 나타내는 모델 개체는 유형 `PHAsset`합니다. A `PHAsset` 자산의 미디어 유형 및 만든 날짜와 같은 메타 데이터를 포함 합니다.
마찬가지로,는 `PHAssetCollection` 및 `PHCollectionList` 클래스 각각 자산 컬렉션 및 컬렉션 목록에 대 한 메타 데이터를 포함 합니다. 자산 컬렉션은 그룹 사진 및 비디오 특정 연도의 등의 자산입니다. 마찬가지로, 컬렉션 목록은 사진과 동영상 연도별로 그룹화 같은 자산 컬렉션의 그룹입니다.

## <a name="querying-model-data"></a>모델 데이터 쿼리
사진 키트 쉽게 쿼리 모델 데이터를 다양 한 인출 메서드를 통해 있습니다. 예를 들어 모든 이미지를 검색 하려면 호출 하는 것 `PFAsset.Fetch`전달 하는 `PHAssetMediaType.Image` 미디어 유형입니다.

    PHFetchResult fetchResults = PHAsset.FetchAssets (PHAssetMediaType.Image, null);

`PHFetchResult` 인스턴스는 모두 포함 한 다음는 `PFAsset` 이미지를 나타내는 인스턴스. 이미지 자체를 얻기 위해 사용 하는 `PHImageManager` (또는 캐싱 버전 `PHCachingImageManager`)를 호출 하 여 이미지에 대 한 요청 `RequestImageForAsset`합니다. 다음 코드에서 각 자산에 대 한 이미지를 검색 하는 예를 들어 한 `PHFetchResult` 컬렉션 뷰 셀에 표시 하려면:


    public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
    {
              var imageCell = (ImageCell)collectionView.DequeueReusableCell (cellId, indexPath);
            imageMgr.RequestImageForAsset ((PHAsset)fetchResults [(uint)indexPath.Item], thumbnailSize,
        PHImageContentMode.AspectFill, new PHImageRequestOptions (), (img, info) => {
            imageCell.ImageView.Image = img;
        });
        return imageCell;
    }

그 결과 표 아래와 같이 이미지의 같습니다.

![](photokit-images/image4.png "표 이미지를 표시 하는 실행 중인 응용 프로그램")
 
## <a name="saving-changes-to-the-photo-library"></a>변경 내용을 저장 하는 사진 라이브러리

쿼리 및 데이터 읽기를 처리 하는 방법입니다. 라이브러리에 변경 내용을 다시 작성할 수도 있습니다. 관심 있는 여러 응용 프로그램 시스템 사진 라이브러리와 상호 작용할 수 있으므로 관찰자는 PhotoLibraryObserver를 사용 하 여 변경에 대 한 알림을 등록할 수 있습니다. 그런 다음 변경 작업은 수행할 응용 프로그램 적절 하 게 업데이트할 수 있습니다. 예를 들어 다음은 위의 컬렉션 뷰를 다시 로드 하는 간단한 구현이입니다.

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
    
응용 프로그램에서 변경 내용을 다시 쓸 실제로, 변경 요청을 만들 수 있습니다. 각 모델 클래스는 연결 된 변경 요청 클래스를 있습니다. 예를 들어 한 PHAsset를 변경 하려면는 PHAssetChangeRequest 만듭니다. 사진 라이브러리에 다시 기록 된 위의 그림과 같이 관찰자에 게 전송 되는 변경 작업을 수행 하는 단계는:

-   편집 작업을 수행 합니다.
-   PHContentEditingOutput 인스턴스를 필터링 된 이미지 데이터를 저장 합니다.
-   변경 요청 양식을 게시 하는 변경 내용이 편집 출력을 확인 합니다.

Core 이미지 noir 필터를 적용 하는 이미지를 다시 변경 내용을 기록 하는 예제는 다음과 같습니다.

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
    
사용자가 단추를 선택, 필터가 적용 됩니다.

![](photokit-images/image5.png "적용 되 고 필터의 예")
 
및는 PHPhotoLibraryChangeObserver 덕분에 변경이 반영 됩니다 컬렉션 뷰에 사용자가 다시 이동:

![](photokit-images/image6.png "사용자가 다시 탐색 모음 보기에는 변경 내용이 반영 됩니다.")
