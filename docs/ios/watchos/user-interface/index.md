---
title: "watchOS 사용자 인터페이스"
ms.topic: article
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/19/2016
ms.openlocfilehash: 0717b8484c6094bb1d9589c44df37745d9e21900
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="watchos-user-interface"></a>watchOS 사용자 인터페이스

[ **WatchKitCatalog** ](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) 샘플에서는 다양 한 watchOS 컨트롤을 보여 줍니다. 응용 프로그램의 스토리 보드는 다음과 같습니다 (확대/축소 하려면 클릭).

[![](images/storyboard-sml.png "샘플 watchOS 레이아웃")](images/storyboard.png#lightbox)

모든 컨트롤의 프로그래밍 방식 이름 접두사로 `WKInterface` 않음 (예: `WKInterfaceLabel`, `WKInterfaceButton`).


<table align="center" border="1" cellpadding="1" cellspacing="1">
  <thead>
      <th>
        <strong>컨트롤</strong>
      </th>
      <th>
        <strong>설명</strong>
      </th>
      <th>
        <strong>스크린 샷</strong>
      </th>
    </thead>
    <tbody>
    <tr>
      <td valign="top">
레이블 </td>
      <td valign="top">
사용 하 여 <code>SetText</code> 및 기타 속성을 label 컨트롤에서 텍스트의 모양을 제어 합니다. <code>NSAttributedString</code> 에서도 지원 됩니다.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs">카탈로그 코드</a>
      </td>
      <td>
        <img src="Images/label.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
단추 </td>
      <td valign="top">
만들기 및 스토리 보드에서 속성을 설정 합니다. <kbd>Ctrl + 드래그</kbd> 추가 하는 <code>Action</code> 클릭 하면에 대 한 처리기를 구현 하 합니다.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs">카탈로그 코드</a>
      </td>
      <td>
        <img src="Images/button.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
전환 </td>
      <td valign="top">
사용 하 여 <code>SetOn</code> 스위치 상태를 제어할 수 있습니다.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs">카탈로그 코드</a>
      </td>
      <td>
        <img src="Images/switch.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
슬라이더 </td>
      <td valign="top">
지정할 수 있습니다.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs">카탈로그 코드</a>
      </td>
      <td>
        <img src="Images/slider.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
이미지 </td>
      <td valign="top">
사용 하 여 <code>myImage.SetImage("MyWatchImage")</code> watch에 이미지를 로드 또는 <code>WKInterfaceDevice.CurrentDevice.AddCachedImage</code> 시계에 반복된 사용을 위해 캐시 하 합니다.
        <br />
        <a href="~/ios/watchos/user-interface/image.md">이미지 컨트롤 설명서</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs">카탈로그 코드</a>
      </td>
      <td>
        <img src="Images/image.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
구분 기호 </td>
      <td valign="top">
Ui 매력적 조사식을 만드는 데 구분 기호를 사용 합니다.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs">카탈로그 코드</a>
      </td>
      <td>
        <img src="Images/separator.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
맵 </td>
      <td valign="top">
지도 이미지 시계에 정적으로 표시 되 있지만 모양이 핀을 추가 하는 등의 여러 측면을 제어할 수 있습니다.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs">카탈로그 코드</a>
      </td>
      <td>
        <img src="Images/map.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
동영상 및 InlineMove </td>
      <td valign="top">
영화 유지할 수 자체적으로 open 또는 인라인 <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs">카탈로그 코드</a>
      </td>
      <td>
        <img src="Images/movie.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
그룹화 </td>
      <td valign="top">
매력적인 조사식 Ui를 만들 수 있도록 그룹을 사용 합니다.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs">카탈로그 코드</a>
      </td>
      <td>
        <img src="Images/group.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
표 </td>
      <td valign="top">
IOS에서 테이블의 단순화 된 버전입니다.
구현 <code>DidSelectRow</code> 사용자 선택에 응답 (또는 segue 사용).
        <br />
        <a href="~/ios/watchos/user-interface/table.md">테이블 컨트롤 설명서</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TableDetailController.cs">카탈로그 코드</a>
      </td>
      <td>
        <img src="Images/table.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
장치 </td>
      <td valign="top">
        <code>WKInterfaceDevice.CurrentDevice</code> 와 같은 속성에 포함 <code>ScreenBounds</code>, <code>ScreenScale</code>, 및 <code>PreferredContentSizeCategory</code>합니다.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs">카탈로그 코드</a>
      </td>
      <td>
        <img src="Images/device.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="~/ios/watchos/user-interface/menu.md">메뉴</a>
      </td>
      <td valign="top">
스토리 보드에서 강제 키를 눌러 메뉴를 정의 하 고 코드에서 각 단추에 대 한 작업을 구현 합니다.
        <br />
        <a href="~/ios/watchos/user-interface/menu.md">메뉴 컨트롤 (강제 터치) 설명서</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs">카탈로그 코드</a>
      </td>
      <td>
        <img src="Images/controller.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
텍스트 입력 </td>
      <td valign="top">
사용 하 여 <code>PresentTextInputController</code> 및 <code>WKTextInputMode</code> 열거형입니다.
        <br />
        <a href="~/ios/watchos/user-interface/text-input.md">텍스트 입력 설명서</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputDetailController.cs">카탈로그 코드</a>
      </td>
      <td>
        <img src="Images/textinput.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
디지털 왕관 </td>
      <td valign="top">
디지털 왕관 선택기를 진행 하는 데 사용할 수 또는 회전의 코드에서 추적할 수 있습니다.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs">카탈로그 코드</a>
      </td>
      <td>
        <img src="Images/digital-crown.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
제스처 </td>
      <td valign="top">
화면에 추가할 수 있는 제스처 인식의 네 가지가: Tap, 통과, 팬 및 LongPress 합니다.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs">카탈로그 코드</a>
      </td>
      <td>
        <img src="Images/gestures.png" class="tableimg">
      </td>
    </tr>
    </tbody>
</table>



## <a name="related-links"></a>관련 링크

- [WatchKitCatalog (샘플)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [조사식 키트 API 참조](https://developer.xamarin.com/api/namespace/WatchKit/)
