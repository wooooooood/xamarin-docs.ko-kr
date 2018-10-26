---
title: Xamarin.iOS에서 PhotoKit
description: 이 문서에서는 PhotoKit, 해당 모델 개체를 설명 설명 모델 데이터를 쿼리하려면 및 사진 라이브러리에 대 한 변경 내용을 저장 하는 방법입니다.
ms.prod: xamarin
ms.assetid: 7FDEE394-3787-40FA-8372-76A05BF184B3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: d59cac9403a244ce553d84e0590b8a9c3d4d2f30
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102713"
---
# <a name="photokit-in-xamarinios"></a>Xamarin.iOS에서 PhotoKit

PhotoKit는 새 응용 프로그램을 확인 하 고 해당 내용을 수정할 사용자 지정 사용자 인터페이스를 만들고 시스템 이미지 라이브러리를 쿼리 합니다. 많은 앨범 및 폴더와 같은 자산 컬렉션 뿐만 아니라 이미지 및 비디오 자산을 나타내는 클래스가 포함 됩니다.

## <a name="model-objects"></a>모델 개체

PhotoKit 이러한 자산을에서는 모델 개체를 나타냅니다. 사진 및 비디오 자신을 나타내는 모델 개체는 형식의 `PHAsset`합니다. `PHAsset` 자산의 미디어 유형 및 만든 날짜와 같은 메타 데이터를 포함 합니다.
마찬가지로, 합니다 `PHAssetCollection` 고 `PHCollectionList` 클래스 각각 자산 컬렉션 및 컬렉션 목록에 대 한 메타 데이터를 포함 합니다. 자산 컬렉션은 그룹 사진 및 지정 된 연도 대 한 비디오와 같은 자산입니다. 마찬가지로, 목록 컬렉션은 그룹 사진 및 연도별로 그룹화 하는 비디오와 같은 자산 컬렉션입니다.

## <a name="querying-model-data"></a>모델 데이터 쿼리

PhotoKit 쉽게 모델 데이터를 쿼리 하는 다양 한 인출 메서드를 통해. 예를 들어, 모든 이미지를 검색 하는 호출 `PFAsset.Fetch`전달 된 `PHAssetMediaType.Image` 미디어 유형입니다.

    PHFetchResult fetchResults = PHAsset.FetchAssets (PHAssetMediaType.Image, null);

합니다 `PHFetchResult` 인스턴스에 다음 포함 모든는 `PFAsset` 이미지를 나타내는 인스턴스. 사용할 자체 이미지를 가져오려면 합니다 `PHImageManager` (또는 캐싱 버전인 `PHCachingImageManager`) 호출 하 여 이미지에 대 한 요청을 `RequestImageForAsset`합니다. 다음 코드에서 각 자산에 대 한 이미지를 검색 하는 예를 들어, 한 `PHFetchResult` 컬렉션 뷰 셀을 표시 하려면:


    public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
    {
              var imageCell = (ImageCell)collectionView.DequeueReusableCell (cellId, indexPath);
            imageMgr.RequestImageForAsset ((PHAsset)fetchResults [(uint)indexPath.Item], thumbnailSize,
        PHImageContentMode.AspectFill, new PHImageRequestOptions (), (img, info) => {
            imageCell.ImageView.Image = img;
        });
        return imageCell;
    }

이 인해 이미지 아래와 같이 포함 된 표:

![](photokit-images/image4.png "표 이미지를 표시 하는 실행 중인 앱")
 
## <a name="saving-changes-to-the-photo-library"></a>사진 라이브러리에 대 한 변경 내용을 저장

데이터를 읽고 쿼리를 처리 하는 방법입니다. 또한 변경 내용을 라이브러리로 다시 작성할 수 있습니다. 여러 관심 있는 응용 프로그램 시스템 사진 라이브러리와 상호 작용할 수 되므로 PhotoLibraryObserver를 사용 하 여 변경 내용을 알릴 관찰자를 등록할 수 있습니다. 그런 다음 변경 내용을 완료 되 면 응용 프로그램이 적절 하 게 업데이트할 수 있습니다. 예를 들어, 다음은 위의 컬렉션 뷰를 다시 로드 하기 위한 간단한 구현:

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
    
쓸 실제로 변경 내용을 다시 응용 프로그램에서 변경 요청을 만들 수 있습니다. 각 모델 클래스는 연결 된 변경 요청 클래스를 있습니다. 예를 들어를 PHAsset를 변경 하려면 PHAssetChangeRequest를 만듭니다. 사진 라이브러리에 다시 작성 되 고 위의 그림과 같이 관찰자에 게 전송 하는 변경을 수행 하는 단계는.

-   편집 작업을 수행 합니다.
-   PHContentEditingOutput 인스턴스로 필터링 된 이미지 데이터를 저장 합니다.
-   변경 양식을 편집 출력을 게시 하는 변경 요청을 확인 합니다.

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
    
사용자가 단추를 선택 하면 필터 적용 됩니다.

![](photokit-images/image5.png "적용 되는 필터의 예제")
 
있으며 PHPhotoLibraryChangeObserver, 덕분 변경에에서 반영 됩니다 컬렉션 뷰에서 사용자가 다시 탐색 합니다.

![](photokit-images/image6.png "사용자가 다시 탐색 컬렉션 보기에는 변경 내용이 반영 됩니다.")
