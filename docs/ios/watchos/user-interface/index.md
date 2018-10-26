---
title: watchOS에서 Xamarin 사용자 인터페이스 컨트롤
description: 이 문서에서는 watchOS 사용자 인터페이스에서 사용할 수 있는 다양 한 컨트롤을 설명 합니다. 레이블, 단추, 스위치, 슬라이더, 이미지, 구분 기호, 맵 등의 설명을 제공합니다.
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/19/2016
ms.openlocfilehash: a7be193cee60b40f70b3dd4a840e0a26ccb8c3b2
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109005"
---
# <a name="watchos-user-interface-controls-in-xamarin"></a>watchOS에서 Xamarin 사용자 인터페이스 컨트롤

합니다 [ **WatchKitCatalog** ](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) 샘플에서는 다양 한 watchOS 컨트롤을 보여 줍니다. 앱의 스토리 보드는 다음과 같습니다 (확대/축소 하려면 클릭)

[![](images/storyboard-sml.png "WatchOS 레이아웃 샘플")](images/storyboard.png#lightbox)

모든 컨트롤의 프로그래밍 방식 이름 접두사로 `WKInterface` (예: `WKInterfaceLabel`, `WKInterfaceButton`).

|Control|설명|스크린 샷|
|---|---|---|
|레이블|사용 하 여 `SetText` 및 기타 속성을 레이블 컨트롤의 텍스트의 모양을 제어 합니다. `NSAttributedString` 도 지원 됩니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs)|![](Images/label.png)|
|단추|만들기 및 스토리 보드에서 속성을 설정 합니다. Ctrl + 드래그 추가할는 `Action` 클릭 하면에 대 한 처리기를 구현 합니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs)|![](Images/button.png)|
|전환|사용 하 여 `SetOn` 스위치 상태를 제어 하 합니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs)|![](Images/switch.png)|
|슬라이더|많은 다양 한 스타일 있을 수 있습니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs)|![](Images/slider.png)|
|이미지|사용 하 여 `myImage.SetImage("MyWatchImage")` watch, 이미지를 로드 또는 `WKInterfaceDevice.CurrentDevice.AddCachedImage` 시계에서 반복된 사용을 위해 캐시 하 합니다.<br />[이미지 컨트롤 설명서](~/ios/watchos/user-interface/image.md)<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs)|![](Images/image.png)|
|구분 기호|매력적인 조사식 Ui를 만들기 위해 구분 기호를 사용 합니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs)|![](Images/separator.png)| 
|맵|지도 이미지 시계에 정적으로 표시 됩니다 있지만 모양 핀을 추가 하는 등의 다양 한 측면을 제어할 수 있습니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs)|![](Images/map.png)|
|동영상 및 InlineMove|영화 수 자체적으로 open 또는 인라인<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs)|![](Images/movie.png)|
|그룹화|매력적인 조사식 Ui를 만들기 위해 그룹을 사용 합니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs)|![](Images/group.png)|
|표|IOS에서 테이블의 간소화 된 버전입니다. 구현 `DidSelectRow` 사용자 선택에 응답 (또는 segue를 사용 하 여).<br />[Table 컨트롤 설명서](~/ios/watchos/user-interface/table.md)<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/Table%20Detail%20Controller/TableDetailController.cs)|![](Images/table.png)|
|장치|`WKInterfaceDevice.CurrentDevice` 와 같은 속성에 포함 `ScreenBounds`, `ScreenScale`, 및 `PreferredContentSizeCategory`합니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs)|![](Images/device.png)|
|[메뉴](~/ios/watchos/user-interface/menu.md)|스토리 보드에서 force-키를 눌러 메뉴를 정의 하 고 코드에 각 단추에 대 한 작업을 구현 합니다.<br />[메뉴 (Force Touch) 제어 문서](~/ios/watchos/user-interface/menu.md)<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs)|![](Images/controller.png)|
|텍스트 입력|사용 하 여 `PresentTextInputController` 하며 `WKTextInputMode` 열거형입니다.<br />[텍스트 입력 설명서](~/ios/watchos/user-interface/text-input.md)<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputController.cs)|![](Images/textinput.png)|
|디지털 Crown|코드에서의 회전을 추적할 수 있습니다 또는 디지털 Crown 선택기를 드라이브를 사용할 수 있습니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs)|![](Images/digital-crown.png)|
|제스처|장면에 추가할 수 있는 제스처 인식의 네 가지가: 탭, 살짝 밀기, 팬 및 LongPress 합니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs)|![](Images/gestures.png)|


## <a name="related-links"></a>관련 링크

- [WatchKitCatalog (샘플)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [조사식 키트 API 참조](https://developer.xamarin.com/api/namespace/WatchKit/)
