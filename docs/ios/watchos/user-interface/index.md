---
title: watchOS 사용자 인터페이스
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/19/2016
ms.openlocfilehash: 73099768d876cad08571c3d0bf8340535eb1307b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="watchos-user-interface"></a>watchOS 사용자 인터페이스

[ **WatchKitCatalog** ](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) 샘플에서는 다양 한 watchOS 컨트롤을 보여 줍니다. 응용 프로그램의 스토리 보드는 다음과 같습니다 (확대/축소 하려면 클릭).

[![](images/storyboard-sml.png "샘플 watchOS 레이아웃")](images/storyboard.png#lightbox)

모든 컨트롤의 프로그래밍 방식 이름 접두사로 `WKInterface` 않음 (예: `WKInterfaceLabel`, `WKInterfaceButton`).

|Control|설명|스크린 샷|
|---|---|---|
|레이블|사용 하 여 `SetText` 및 기타 속성을 label 컨트롤에서 텍스트의 모양을 제어 합니다. `NSAttributedString` 에서도 지원 됩니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs)|![](Images/label.png)|
|단추|만들기 및 스토리 보드에서 속성을 설정 합니다. Ctrl + 드래그 추가 하는 `Action` 클릭 하면에 대 한 처리기를 구현 합니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs)|![](Images/button.png)|
|전환|사용 하 여 `SetOn` 스위치 상태를 제어할 수 있습니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs)|![](Images/switch.png)|
|슬라이더|지정할 수 있습니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs)|![](Images/slider.png)|
|이미지|사용 하 여 `myImage.SetImage("MyWatchImage")` watch에 이미지를 로드 또는 `WKInterfaceDevice.CurrentDevice.AddCachedImage` 시계에 반복된 사용을 위해 캐시 하 합니다.<br />[이미지 컨트롤 설명서](~/ios/watchos/user-interface/image.md)<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs)|![](Images/image.png)|
|구분 기호|Ui 매력적 조사식을 만드는 데 구분 기호를 사용 합니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs)|![](Images/separator.png)| 
|맵|지도 이미지 시계에 정적으로 표시 되 있지만 모양이 핀을 추가 하는 등의 여러 측면을 제어할 수 있습니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs)|![](Images/map.png)|
|동영상 및 InlineMove|영화 유지할 수 자체적으로 open 또는 인라인<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs)|![](Images/movie.png)|
|그룹화|매력적인 조사식 Ui를 만들 수 있도록 그룹을 사용 합니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs)|![](Images/group.png)|
|표|IOS에서 테이블의 단순화 된 버전입니다. 구현 `DidSelectRow` 사용자 선택에 응답 (또는 segue 사용).<br />[테이블 컨트롤 설명서](~/ios/watchos/user-interface/table.md)<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/Table%20Detail%20Controller/TableDetailController.cs)|![](Images/table.png)|
|장치|`WKInterfaceDevice.CurrentDevice` 와 같은 속성에 포함 `ScreenBounds`, `ScreenScale`, 및 `PreferredContentSizeCategory`합니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs)|![](Images/device.png)|
|[메뉴](~/ios/watchos/user-interface/menu.md)|스토리 보드에서 강제 키를 눌러 메뉴를 정의 하 고 코드에서 각 단추에 대 한 작업을 구현 합니다.<br />[메뉴 컨트롤 (강제 터치) 설명서](~/ios/watchos/user-interface/menu.md)<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs)|![](Images/controller.png)|
|텍스트 입력|사용 하 여 `PresentTextInputController` 및 `WKTextInputMode` 열거형입니다.<br />[텍스트 입력 설명서](~/ios/watchos/user-interface/text-input.md)<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputController.cs)|![](Images/textinput.png)|
|디지털 왕관|디지털 왕관 선택기를 진행 하는 데 사용할 수 또는 회전의 코드에서 추적할 수 있습니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs)|![](Images/digital-crown.png)|
|제스처|화면에 추가할 수 있는 제스처 인식의 네 가지가: Tap, 통과, 팬 및 LongPress 합니다.<br />[카탈로그 코드](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs)|![](Images/gestures.png)|


## <a name="related-links"></a>관련 링크

- [WatchKitCatalog (샘플)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [조사식 키트 API 참조](https://developer.xamarin.com/api/namespace/WatchKit/)
