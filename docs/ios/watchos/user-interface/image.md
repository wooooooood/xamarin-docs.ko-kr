---
title: watchOS Xamarin에서 이미지 컨트롤
description: 이 문서에서는 Xamarin을 사용 하 여 빌드한 watchOS 응용 프로그램에서 이미지 컨트롤을 사용 하는 방법을 설명 합니다. 이미지 조사식 확장, 애니메이션 및에 추가 SetImage 메서드 WKInterfaceImage 컨트롤에 설명 합니다.
ms.prod: xamarin
ms.assetid: B741C207-3427-46F3-9C90-A52BF8933FA4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: eb58c587f737a5991a21f0efe9964353a8ab0399
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791253"
---
# <a name="watchos-image-controls-in-xamarin"></a>watchOS Xamarin에서 이미지 컨트롤

watchOS 제공는 [ `WKInterfaceImage` ](https://developer.xamarin.com/api/type/WatchKit.WKInterfaceImage/) 이미지 및 간단한 애니메이션 표시 하는 컨트롤입니다. 일부 컨트롤 (예: 단추, 그룹 및 인터페이스 컨트롤러)의 배경 이미지를 포함할 수도 있습니다.

![](image-images/image-walkway.png "Apple Watch 보여 주는 그림") ![ ] (image-images/image-animation.png "간단한 애니메이션으로 Apple Watch")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

자산 카탈로그 이미지를 사용 하 여 조사식 키트 앱에 이미지를 추가 합니다.
만 **@2x** 장치 레 티 나 디스플레이 감시 하는 모든 이후 버전은 필요 합니다.

![](image-images/asset-universal-sml.png "2 x 버전 되므로 필요한 경우 모든 시청 레 티 나 디스플레이 장치")

이미지 자체는 조사식에 대 한 올바른 크기를 확인 하는 것이 좋습니다. *방지* 크기가 잘못 된 이미지 (특히 대규모 조직)을 사용 하 여 및 시계에 표시 하려면 확장 합니다.

각 디스플레이 크기에 대해 서로 다른 이미지를 지정 하려면 자산 카탈로그 이미지에서 조사식 키트 크기 (38 mm 및 42 mm)을 사용할 수 있습니다.

![](image-images/asset-watch-sml.png "자산 카탈로그 이미지 38 mm 및 42 mm 조사식 키트 크기를 사용 하 여 각 디스플레이 크기에 대해 서로 다른 이미지를 지정 하는 수 있습니다.")


## <a name="images-on-the-watch"></a>시계의 이미지

에 이미지를 표시 하는 가장 효율적인 방법은 *조사식 응용 프로그램 프로젝트에 포함* 표시를 사용 하 여는 `SetImage(string imageName)` 메서드.

예를 들어는 [WatchKitCatalog](https://developer.xamarin.com/samples/WatchKitCatalog/) 샘플에는 여러 조사식 응용 프로그램 프로젝트에는 자산 카탈로그에 추가 된 이미지:

![](image-images/asset-whale-sml.png "WatchKitCatalog 샘플에는 여러 조사식 응용 프로그램 프로젝트에는 자산 카탈로그에 추가 된 이미지")

이러한 효율적으로 로드 및 표시 될 수 사용 하 여 조사식 `SetImage` 문자열 이름 매개 변수를 사용 합니다.

```csharp
myImageControl.SetImage("Whale");
myOtherImageControl.SetImage("Worry");
```

### <a name="background-images"></a>배경 이미지

에 대 한 적용 되는 동일한 논리는 `SetBackgroundImage (string imageName)` 에 `Button`, `Group`, 및 `InterfaceController` 클래스. 최상의 성능은 watch 앱 자체에 이미지를 저장 하 여 이루어집니다.


## <a name="images-in-the-watch-extension"></a>이미지 조사식 확장에서

조사식 앱 자체에 저장 된 이미지를 로드 하는 것 외에도 표시 하기 위해 watch 앱을 확장 번들에서 이미지를 보낼 수 있습니다 (또는 원격 위치에서 이미지를 다운로드할을 하는 것을 표시할 수 있습니다).

조사식 확장에서 이미지를 로드 하려면 만듭니다 `UIImage` 인스턴스 한 다음 호출 `SetImage` 와 `UIImage` 개체입니다.

예를 들어는 [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) 샘플에 명명 된 이미지 **Bumblebee** 조사식 확장 프로젝트에서:

![](image-images/asset-bumblebee-sml.png "WatchKitCatalog 샘플에는 이미지 조사식 확장 프로젝트에서 Bumblebee 라는")

다음 코드에서 발생 합니다.

- 메모리에 로드 되 고 이미지 및
- 시계에 표시 합니다.

```csharp
using (var image = UIImage.FromBundle ("Bumblebee")) {
    myImageControl.SetImage (image);
}
```


## <a name="animations"></a>애니메이션

일련의 이미지, 애니메이션 효과를 줄은 모두 동일한 접두사로 시작 하 고 숫자 접미사 해야 합니다.

[WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) 샘플은 watch 앱 프로젝트에서 일련의 번호가 매겨진된 이미지에는 **버스** 접두사:

![](image-images/asset-bus-animation-sml.png "WatchKitCatalog 샘플 버스 접두사로 조사식 응용 프로그램 프로젝트에 대 한 일련의 번호가 매겨진된 이미지")

이러한 이미지는 애니메이션을 표시 하려면 먼저 로드를 사용 하 여 이미지 `SetImage` 접두사 이름 및 호출 `StartAnimating`:

```csharp
animatedImage.SetImage ("Bus");
animatedImage.StartAnimating ();
```

호출 `StopAnimating` 애니메이션 반복을 중지할 이미지 컨트롤에서:

```csharp
animatedImage.StopAnimating ();
```


<a name="cache" />

## <a name="appendix-caching-images-watchos-1"></a>부록: 캐싱 이미지 (watchOS 1)

> [!IMPORTANT]
> watchOS 3 앱은 장치에서 완전히 실행 합니다. 다음 정보는 watchOS 1 앱만 해당 합니다.

응용 프로그램 확장에 저장 됩니다 (또는 다운로드 한) 되는 이미지를 반복적으로 사용 하는 경우 후속 디스플레이 대 한 성능 향상을 위해 시계의 저장소에서 이미지를 캐시 하도록 같습니다.

사용 하 여는 `WKInterfaceDevice`s `AddCachedImage` 메서드는 이미지 조사식, 전송을 사용 하 여 `SetImage` 표시 하는 문자열로 이미지 이름 매개 변수를 사용 합니다.

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

이미지 캐시에서 사용 하 여 코드의 내용을 쿼리할 수 `WKInterfaceDevice.CurrentDevice.WeakCachedImages`합니다.


### <a name="managing-the-cache"></a>캐시 관리

캐시 크기가 약 20MB입니다. 응용 프로그램을 다시 시작에서 유지 하 고 사용 하 여 파일을 제거 하기 위해 작업은 가득 차면 `RemoveCachedImage` 또는 `RemoveAllCachedImages` 에 대 한 메서드는 `WKInterfaceDevice.CurrentDevice` 개체입니다.



## <a name="related-links"></a>관련 링크

- [WatchKitCatalog (샘플)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple의 이미지 문서](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Images.html)
