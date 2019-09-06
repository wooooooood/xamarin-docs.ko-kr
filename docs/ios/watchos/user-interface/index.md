---
title: Xamarin의 watchOS 사용자 인터페이스 컨트롤
description: 이 문서에서는 watchOS 사용자 인터페이스에서 사용할 수 있는 다양 한 컨트롤에 대해 설명 합니다. 레이블, 단추, 스위치, 슬라이더, 이미지, 구분 기호, 지도 등에 대 한 설명을 제공 합니다.
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/19/2016
ms.openlocfilehash: cebacf6d845affe017e289fe4522a3b7c76d2942
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291787"
---
# <a name="watchos-user-interface-controls-in-xamarin"></a>Xamarin의 watchOS 사용자 인터페이스 컨트롤

[**WatchKitCatalog**](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) 샘플은 다양 한 watchOS 컨트롤을 보여 줍니다. 앱의 스토리 보드가 여기에 표시 됩니다 (확대/축소 하려면 클릭).

[![](images/storyboard-sml.png "샘플 watchOS 레이아웃")](images/storyboard.png#lightbox)

모든 컨트롤의 프로그래밍 이름 앞에는 접두사가 붙습니다 `WKInterface` (예: `WKInterfaceLabel`, `WKInterfaceButton`).

|Control|설명|스크린 샷|
|---|---|---|
|레이블|및 `SetText` 기타 속성을 사용 하 여 레이블 컨트롤의 텍스트 모양을 제어 합니다. `NSAttributedString`도 지원 됩니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs)|![](Images/label.png)|
|단추|스토리 보드에서 속성을 만들고 설정 합니다. 를 클릭 하 여를 클릭 `Action` 하면 처리기를 구현할를 추가 하려면 Ctrl + 끌어서를 추가 합니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs)|![](Images/button.png)|
|전환|스위치 `SetOn` 상태를 제어 하는 데 사용 합니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs)|![](Images/switch.png)|
|슬라이더|여러 가지 스타일을 사용할 수 있습니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs)|![](Images/slider.png)|
|이미지|를 `myImage.SetImage("MyWatchImage")` 사용 하 여 조사식에 이미지를 로드 `WKInterfaceDevice.CurrentDevice.AddCachedImage` 하거나 조사식에서 반복적으로 사용할 수 있도록 캐시 합니다.<br />[이미지 컨트롤 설명서](~/ios/watchos/user-interface/image.md)<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs)|![](Images/image.png)|
|구분 기호|구분 기호를 사용 하 여 멋진 조사식 Ui를 만들 수 있습니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs)|![](Images/separator.png)| 
|맵|지도 이미지가 조사식에 정적으로 표시 되지만 핀 추가를 포함 하 여 모양의 여러 측면을 제어할 수 있습니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs)|![](Images/map.png)|
|영화 & InlineMove|동영상은 자체적으로 또는 인라인으로 열 수 있습니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs)|![](Images/movie.png)|
|그룹화|그룹을 사용 하 여 멋진 조사식 Ui를 만들 수 있습니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs)|![](Images/group.png)|
|표|IOS에 있는 테이블의 간소화 된 버전입니다. 사용자 `DidSelectRow` 선택에 응답 하거나 segue를 사용 하 여를 구현 합니다.<br />[테이블 컨트롤 설명서](~/ios/watchos/user-interface/table.md)<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/Table%20Detail%20Controller/TableDetailController.cs)|![](Images/table.png)|
|디바이스|`WKInterfaceDevice.CurrentDevice`, 및 `ScreenBounds` `ScreenScale` 와같은속성`PreferredContentSizeCategory`을 포함 합니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs)|![](Images/device.png)|
|[메뉴](~/ios/watchos/user-interface/menu.md)|스토리 보드에서 강제 누르기 메뉴를 정의 하 고 코드의 각 단추에 대 한 동작을 구현 합니다.<br />[메뉴 컨트롤 (Force Touch) 설명서](~/ios/watchos/user-interface/menu.md)<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs)|![](Images/controller.png)|
|텍스트 입력|및 열거형을 사용 `PresentTextInputController`합니다 `WKTextInputMode` .<br />[텍스트 입력 설명서](~/ios/watchos/user-interface/text-input.md)<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputController.cs)|![](Images/textinput.png)|
|Digital Crown|Digital Crown를 사용 하 여 선택기를 구동 하거나 코드에서 회전을 추적할 수 있습니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs)|![](Images/digital-crown.png)|
|제스처|장면에 추가할 수 있는 제스처 인식에는 다음 네 가지 유형이 있습니다. 탭, 살짝 밀기, 이동 및 고를 누릅니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs)|![](Images/gestures.png)|


## <a name="related-links"></a>관련 링크

- [WatchKitCatalog (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [감시 키트 API 참조](xref:WatchKit)
