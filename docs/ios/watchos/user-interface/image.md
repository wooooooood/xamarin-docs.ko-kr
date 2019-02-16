---
title: watchOS에서 Xamarin 이미지 컨트롤
description: 이 문서에는 Xamarin에 내장 된 watchOS 응용 프로그램에서 이미지 컨트롤을 사용 하는 방법을 설명 합니다. WKInterfaceImage 컨트롤 이미지 조사식 확장, 애니메이션 등 추가 SetImage 메서드를 설명 합니다.
ms.prod: xamarin
ms.assetid: B741C207-3427-46F3-9C90-A52BF8933FA4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 6a2b8c99156963ae167aecd29a618d0feeffbdc7
ms.sourcegitcommit: 2713f2c1d74e3582704c3d0ca65b6651119ed489
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56321131"
---
# <a name="watchos-image-controls-in-xamarin"></a>watchOS에서 Xamarin 이미지 컨트롤

watchOS 제공 된 [`WKInterfaceImage` ](xref:WatchKit.WKInterfaceImage) 간단한 애니메이션 및 이미지를 표시 하도록 컨트롤입니다. 일부 컨트롤 (예: 단추, 그룹 및 인터페이스 컨트롤러) 배경 이미지를 포함할 수도 있습니다.

![](image-images/image-walkway.png "Apple Watch 보여 주는 그림") ![](image-images/image-animation.png "간단한 애니메이션을 사용 하 여 Apple Watch")
 <!-- watch image courtesy of http://infinitapps.com/bezel/ -->

자산 카탈로그 이미지를 사용 하 여 조사 키트 앱에 이미지를 추가 합니다.
만 **@2x** 레 티 나 디스플레이 장치를 시청 하는 모든 이후 버전은 필수입니다.

![](image-images/asset-universal-sml.png "2 x 버전은 전체 보기 레 티 나 디스플레이 장치 때문에 필요")

이미지 자체는 조사식에 대 한 올바른 크기를 확인 하는 것이 좋습니다. *방지* 크기가 잘못 된 이미지 (특히 대규모)를 사용 하 여 및 시계에 모두 표시 하려면 확장 합니다.

각 디스플레이 크기에 대 한 다양 한 이미지를 지정 하려면 자산 카탈로그 이미지에서 조사식 키트 크기 ((38mm 및 42 mm)를 사용할 수 있습니다.

![](image-images/asset-watch-sml.png "자산 카탈로그 이미지 조사식 키트 크기 (38mm 및 42 mm를 사용 하 여 각 디스플레이 크기에 대 한 다른 이미지를 지정 하는 수 있습니다.")


## <a name="images-on-the-watch"></a>시계의 이미지

이미지를 표시 하는 가장 효율적인 방법은 하는 것 *watch 앱 프로젝트에 포함할* 하 고 표시를 사용 하 여는 `SetImage(string imageName)` 메서드.

예를 들어 합니다 [WatchKitCatalog](https://developer.xamarin.com/samples/WatchKitCatalog/) 샘플 개수가 watch 앱 프로젝트에서 자산 카탈로그를 추가할 이미지:

![](image-images/asset-whale-sml.png "Watch 앱 프로젝트에서 자산 카탈로그에 추가 하는 이미지의 번호가 WatchKitCatalog 샘플")

이러한 수 효율적으로 로드 하 고 사용 하 여 조사식 표시 `SetImage` 문자열 name 매개 변수를 사용 하 여:

```csharp
myImageControl.SetImage("Whale");
myOtherImageControl.SetImage("Worry");
```

### <a name="background-images"></a>배경 이미지

동일한 논리가 적용 됩니다는 `SetBackgroundImage (string imageName)` 에 `Button`를 `Group`, 및 `InterfaceController` 클래스입니다. 최상의 성능은 watch 앱 자체에서 이미지를 저장 하 여 이루어집니다.


## <a name="images-in-the-watch-extension"></a>이미지 조사식 확장에서

Watch 앱 자체에 저장 된 이미지를 로드 하는 것 외에도 표시 하기 위해 watch 앱을 확장 번들에서 이미지를 보낼 수 있습니다 (또는 원격 위치에서 이미지를 다운로드 하 고 해당 표시).

이미지 조사식 확장에서 로드를 만들려면 `UIImage` 인스턴스 및 호출 `SetImage` 사용 하 여는 `UIImage` 개체입니다.

예를 들어 합니다 [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) 샘플에는 명명 된 이미지로 **Bumblebee** 조사식 확장 프로젝트에서:

![](image-images/asset-bumblebee-sml.png "WatchKitCatalog이 샘플에는 이미지 조사식 확장 프로젝트에서 Bumblebee 라는")

다음 코드에서 발생 합니다.

- 메모리에 로드 중인 이미지 및
- 시계에 표시 됩니다.

```csharp
using (var image = UIImage.FromBundle ("Bumblebee")) {
    myImageControl.SetImage (image);
}
```


## <a name="animations"></a>애니메이션

이미지 집합, 애니메이션에 모두 동일한 접두사로 시작 하 고 숫자 접미사 해야 합니다.

합니다 [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) 샘플 watch 앱 프로젝트에 대 한 일련의 번호가 매겨진된 이미지는 **버스** 접두사:

![](image-images/asset-bus-animation-sml.png "WatchKitCatalog 샘플 Bus 접두사를 사용 하 여 watch 앱 프로젝트에 대 한 일련의 번호가 매겨진된 이미지")

이러한 이미지를 애니메이션으로 표시 하려면 먼저 로드 하 여 이미지 `SetImage` 접두사 이름 및 호출을 사용 하 여 `StartAnimating`:

```csharp
animatedImage.SetImage ("Bus");
animatedImage.StartAnimating ();
```

호출 `StopAnimating` 애니메이션 반복을 중지 하려면 이미지 컨트롤에서:

```csharp
animatedImage.StopAnimating ();
```


<a name="cache" />

## <a name="appendix-caching-images-watchos-1"></a>부록: 캐싱 이미지 (watchOS 1)

> [!IMPORTANT]
> watchOS 3 앱 장치에서 완전히 실행 합니다. 다음 정보는 watchOS 1 앱에 해당 됩니다.

응용 프로그램 확장에 저장 됩니다 (또는 다운로드 한) 이미지를 반복적으로 사용 하는 경우 후속 디스플레이 대 한 성능을 향상 시키기 위해 시계의 저장소에서 이미지를 캐시 하 불가능 합니다.

사용 된 `WKInterfaceDevice`s `AddCachedImage` watch에서 이미지를 전송 하 고 사용 하 여 방법 `SetImage` 표시 문자열로 이미지 name 매개 변수를 사용 하 여:

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

코드를 사용 하 여 이미지 캐시의 콘텐츠를 쿼리할 수 있습니다 `WKInterfaceDevice.CurrentDevice.WeakCachedImages`합니다.


### <a name="managing-the-cache"></a>캐시 관리

캐시 크기가 약 20MB입니다. 앱 다시 시작에 대해 유지 되 고 사용자의 책임을 사용 하 여 파일을 지울 것이 가득 차면 `RemoveCachedImage` 나 `RemoveAllCachedImages` 메서드를 `WKInterfaceDevice.CurrentDevice` 개체입니다.



## <a name="related-links"></a>관련 링크

- [WatchKitCatalog (샘플)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple의 이미지 문서](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Images.html)
