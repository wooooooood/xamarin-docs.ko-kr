---
title: Xamarin의 watchOS 이미지 컨트롤
description: 이 문서에서는 Xamarin으로 빌드된 watchOS 응용 프로그램에서 이미지 컨트롤을 사용 하는 방법을 설명 합니다. WKInterfaceImage 컨트롤, SetImage 메서드, 조사식 확장에 이미지 추가, 애니메이션 등에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: B741C207-3427-46F3-9C90-A52BF8933FA4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 7d24286b5d428a571afc7498afafa1171c075110
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032701"
---
# <a name="watchos-image-controls-in-xamarin"></a>Xamarin의 watchOS 이미지 컨트롤

watchOS는 이미지와 단순 애니메이션을 표시 하는 [`WKInterfaceImage`](xref:WatchKit.WKInterfaceImage) 컨트롤을 제공 합니다. 일부 컨트롤에는 배경 이미지 (예: 단추, 그룹 및 인터페이스 컨트롤러)가 있을 수도 있습니다.

![](image-images/image-walkway.png "그림을 보여 주는 Apple Watch") ![](image-images/image-animation.png "단순 애니메이션으로 Apple Watch")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

Asset catalog 이미지를 사용 하 여 시청 키트 앱에 이미지를 추가 합니다.
모든 시청 장치에 레 티 나가 표시 되기 때문에 **@2x** 버전만 필요 합니다.

![](image-images/asset-universal-sml.png "Only 2x versions are required, since all watch devices have Retina displays")

이미지 자체를 조사식 표시의 올바른 크기로 유지 하는 것이 좋습니다. 잘못 된 크기의 이미지 (특히 큰 이미지) 및 크기 조정을 사용 하 여 조사식에 표시 *하지 않도록* 합니다.

자산 카탈로그 이미지에서 감시 키트 크기 (38mm 및 42mm)를 사용 하 여 각 표시 크기에 대해 서로 다른 이미지를 지정할 수 있습니다.

![](image-images/asset-watch-sml.png "You can use the Watch Kit sizes 38mm and 42mm in an asset catalog image to specify different images for each display size")

## <a name="images-on-the-watch"></a>조사식 이미지

이미지를 표시 하는 가장 효율적인 방법은 *조사식 앱 프로젝트에* 이미지를 포함 하 고 `SetImage(string imageName)` 메서드를 사용 하 여 표시 하는 것입니다.

예를 들어, [WatchKitCatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog/) 샘플에는 watch 앱 프로젝트의 자산 카탈로그에 추가 된 많은 이미지가 있습니다.

![](image-images/asset-whale-sml.png "The WatchKitCatalog sample has a number of images added to an asset catalog in the watch app project")

이러한 작업은 문자열 이름 매개 변수와 함께 `SetImage`를 사용 하 여 효율적으로 로드 되 고 조사식에 표시 될 수 있습니다.

```csharp
myImageControl.SetImage("Whale");
myOtherImageControl.SetImage("Worry");
```

### <a name="background-images"></a>배경 이미지

`Button`, `Group`및 `InterfaceController` 클래스의 `SetBackgroundImage (string imageName)`에도 동일한 논리가 적용 됩니다. 시청 앱 자체에 이미지를 저장 하 여 최상의 성능을 얻을 수 있습니다.

## <a name="images-in-the-watch-extension"></a>조사식 확장의 이미지

Watch 앱 자체에 저장 된 이미지를 로드 하는 것 외에도 확장 번들의 이미지를 시청 앱에 표시 하기 위해 보낼 수 있습니다. 또는 원격 위치에서 이미지를 다운로드 하 여 표시할 수 있습니다.

조사식 확장에서 이미지를 로드 하려면 `UIImage` 인스턴스를 만든 다음 `UIImage` 개체를 사용 하 여 `SetImage`를 호출 합니다.

예를 들어 [WatchKitCatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) 샘플에는 조사식 확장 프로젝트에 **Bumblebee** 라는 이미지가 있습니다.

![](image-images/asset-bumblebee-sml.png "The WatchKitCatalog sample has an image named Bumblebee in the watch extension project")

다음 코드에서는이 발생 합니다.

- 메모리에 로드 되는 이미지
- 조사식에 표시 됩니다.

```csharp
using (var image = UIImage.FromBundle ("Bumblebee")) {
    myImageControl.SetImage (image);
}
```

## <a name="animations"></a>애니메이션

이미지 집합에 애니메이션 효과를 주려면 모두 동일한 접두사로 시작 하 고 숫자 접미사가 있어야 합니다.

[WatchKitCatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) 샘플에는 **버스** 접두사가 있는 watch 앱 프로젝트에 일련 번호의 이미지가 있습니다.

![](image-images/asset-bus-animation-sml.png "The WatchKitCatalog sample has a series of numbered images in the watch app project with the Bus prefix")

이러한 이미지를 애니메이션으로 표시 하려면 먼저 `SetImage`를 사용 하 여 접두사 이름과 함께 이미지를 로드 한 다음 `StartAnimating`를 호출 합니다.

```csharp
animatedImage.SetImage ("Bus");
animatedImage.StartAnimating ();
```

이미지 컨트롤에서 `StopAnimating`를 호출 하 여 애니메이션 반복을 중지 합니다.

```csharp
animatedImage.StopAnimating ();
```

<a name="cache" />

## <a name="appendix-caching-images-watchos-1"></a>부록: 이미지 캐싱 (watchOS 1)

> [!IMPORTANT]
> watchOS 3 개 앱은 장치에서 전적으로 실행 됩니다. 다음 정보는 watchOS 1 앱에만 해당 됩니다.

응용 프로그램에서 확장에 저장 된 이미지를 반복적으로 사용 하는 경우 (또는 다운로드 한 경우), 조사식의 저장소에 이미지를 캐시 하 여 이후 디스플레이의 성능을 향상 시킬 수 있습니다.

`WKInterfaceDevice`s `AddCachedImage` 메서드를 사용 하 여 이미지를 시계로 전송 하 고 이미지 이름 매개 변수와 함께 `SetImage`를 사용 하 여 이미지를 표시 합니다.

```csharp
var device = WKInterfaceDevice.CurrentDevice;
using (var image = UIImage.FromBundle ("Bumblebee")) {
    if (!device.AddCachedImage (image, "Bumblebee")) {
            Console.WriteLine ("Image cache full.");
        } else {
            cachedImage.SetImage ("Bumblebee");
        }
    }
}
```

`WKInterfaceDevice.CurrentDevice.WeakCachedImages`를 사용 하 여 코드에서 이미지 캐시의 내용을 쿼리할 수 있습니다.

### <a name="managing-the-cache"></a>캐시 관리

캐시 크기는 20mb입니다. 앱을 다시 시작 하는 동안 유지 되 고,이를 채울 때 `WKInterfaceDevice.CurrentDevice` 개체의 `RemoveCachedImage` 또는 `RemoveAllCachedImages` 메서드를 사용 하 여 파일을 지우는 것은 사용자의 책임입니다.

## <a name="related-links"></a>관련 링크

- [WatchKitCatalog (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Apple의 이미지 문서](https://developer.apple.com/documentation/watchkit/wkinterfaceimage)
