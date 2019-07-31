---
title: Xamarin.ios의 iPad 용 멀티태스킹
description: iOS 9는 슬라이드를 반복 하거나 분할 보기를 사용 하 여 동시에 실행 되는 두 개의 앱을 지원 합니다. 또한 비디오 재생 그림을 지원 합니다.
ms.prod: xamarin
ms.assetid: 0F2266D7-21FF-404D-A148-0CFDE76B12AA
ms.technology: xamarin-ios
ms.custom: xamu-video
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 98b68423012fad479f3949452d53c2a49e2a677e
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68654307"
---
# <a name="multitasking-for-ipad-in-xamarinios"></a>Xamarin.ios의 iPad 용 멀티태스킹

_iOS 9는 슬라이드를 반복 하거나 분할 보기를 사용 하 여 동시에 실행 되는 두 개의 앱을 지원 합니다. 또한 비디오 재생 그림을 지원 합니다._

![](multitasking-images/about02-sml.png "화면 예제 분할") ![](multitasking-images/about03-sml.png "그림에서 그림 예제")

iOS 9는 특정 iPad 하드웨어에서 두 앱을 동시에 실행 하기 위한 멀티태스킹 지원을 추가 합니다. IPad 용 멀티태스킹은 다음 기능을 통해 지원 됩니다.

- [**밀기**](#Slide-Over) -사용자는 현재 실행 중인 주 앱의 약 25%에 해당 하는 슬라이드 아웃 패널 (언어 방향에 따라 화면의 오른쪽 또는 왼쪽)에서 두 번째 iOS 앱을 일시적으로 실행할 수 있습니다. 밀기는 iPad Pro, iPad Air, iPad Air 2, iPad 미니 2, iPad 미니 3 또는 iPad 미니 4 에서만 사용할 수 있습니다.
- [**분할 보기**](#Split-View) -지원 되는 iPad 하드웨어 (ipad Air 2, ipad 미니 4 및 iPad Pro만 해당)에서 사용자는 두 번째 앱을 선택 하 고 분할 화면 모드에서 현재 실행 중인 앱과 나란히 실행할 수 있습니다. 사용자는 각 앱이 차지 하는 주 화면의 백분율을 제어할 수 있습니다.
- [**그림의 그림**](#Picture-in-Picture) -비디오 콘텐츠를 재생 하는 앱의 경우 이제 iOS 장치에서 현재 실행 되 고 있는 다른 앱 위에 배치 되는 크기 조정 가능한 창에서 비디오를 재생할 수 있습니다. 사용자는이 창의 크기와 위치에 대 한 모든 권한을 가집니다. 그림의 그림은 iPad Pro, iPad Air, iPad Air 2, iPad 미니 2, iPad 미니 3 또는 iPad 미니 4 에서만 사용할 수 있습니다.

[앱에서 멀티태스킹을 지원할](#Supporting-Multitasking-in-your-App)때 다음과 같은 여러 가지 사항을 고려해 야 합니다.

- [화면 크기 및 방향](#Screen-Size-Considerations)
- [사용자 지정 하드웨어 바로 가기 키](#Custom-Hardware-Keyboard-Shortcuts)
- [리소스 관리](#Resource-Management-Considerations)

앱 개발자로 [PIP 비디오 재생을 사용 하지 않도록 설정](#Disabling-PIP-Video-Playback)하는 등 [멀티태스킹을 옵트아웃 (opt out](#Opting-Out-of-Multitasking)) 할 수도 있습니다.

이 문서에서는 Xamarin.ios 앱이 멀티태스킹 환경에서 올바르게 실행 되도록 하는 데 필요한 단계 또는 앱에 적합 하지 않은 경우 멀티태스킹 옵트아웃 (opt out)을 수행 하는 데 필요한 단계를 다룹니다.

> [!VIDEO https://youtube.com/embed/GctYAozoLr8]

**IPad 용 멀티태스킹 비디오**


<a name="Multitasking-QuickStart" />

## <a name="multitasking-quickstart"></a>멀티태스킹 빠른 시작

**슬라이드 위** 또는 **분할 보기** 를 지원 하려면 앱에서 다음을 수행 해야 합니다.

- IOS 9 이상에 대해 작성 해야 합니다.
- 이미지 자산이 아닌 시작 화면에 Storyboard를 사용 합니다.
- UI에 대해 자동 레이아웃 및 크기 클래스와 함께 Storyboard를 사용 합니다.
- 4 개 iOS 장치 방향 (세로, 세로 방향, 세로, 가로 왼쪽 & 가로)을 모두 지원 합니다.

<a name="Multitasking" />

## <a name="about-multitasking-for-ipad"></a>IPad 용 멀티태스킹 정보

iOS 9는 _슬라이드 반복_, _분할 보기_ (ipad Air 2, iPad 미니 4 및 ipad Pro만 해당) 및 _그림 그림_의 도입으로 iPad에서 새로운 멀티태스킹 기능을 제공 합니다. 다음 섹션에서는 이러한 기능에 대해 자세히 살펴보겠습니다.

<a name="Slide-Over" />

### <a name="slide-over"></a>위로 밀기

반복 기능을 사용 하면 사용자가 두 번째 앱을 선택 하 고 작은 슬라이딩 패널에 표시 하 여 빠른 상호 작용을 제공할 수 있습니다. 패널 위로 이동은 일시적 이며 사용자가 다시 주 앱을 사용 하 여 작업 하는 경우 닫힙니다.

[![](multitasking-images/about01.png "패널 위로 이동")](multitasking-images/about01.png#lightbox)

기억해 야 할 주요 사항은 사용자가 동시에 실행 되는 두 개의 앱을 결정 하 고 개발자가이 프로세스를 제어 하지 않는다는 것입니다. 결과적으로, Xamarin.ios 앱이 패널을 통해 슬라이드에서 올바르게 실행 되도록 하기 위해 몇 가지 작업을 수행 해야 합니다.

- **자동 레이아웃 및 크기 클래스 사용** -이제 xamarin.ios 앱을 슬라이드 외부 측면 패널에서 실행할 수 있으므로 더 이상 장치, 해당 화면 크기 또는 UI를 레이아웃 하는 방향에 의존할 수 없습니다. 앱에서 인터페이스의 크기를 올바르게 조정 하려면 자동 레이아웃 및 크기 클래스를 사용 해야 합니다. 자세한 내용은 [통합 Storyboard 소개 설명서를](~/ios/user-interface/storyboards/unified-storyboards.md) 참조 하세요.
- **효율적으로 리소스 사용** -앱이 실행 중인 다른 앱과 시스템을 공유할 수 있으므로 앱에서 시스템 리소스를 효율적으로 사용 하는 것이 중요 합니다. 메모리가 스파스가 되 면 시스템은 메모리를 가장 많이 사용 하는 앱을 자동으로 종료 합니다. 자세한 내용은 Apple의 [IOS 앱에 대 한 에너지 효율성 가이드](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) 를 참조 하세요.

밀기는 iPad Pro, iPad Air, iPad Air 2, iPad 미니 2, iPad 미니 3 또는 iPad 미니 4 에서만 사용할 수 있습니다. 슬라이드로 앱을 준비 하는 방법에 대 한 자세한 내용은 iPad 설명서에서 Apple의 [멀티태스킹 기능 강화](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145) 를 참조 하세요.

<a name="Split-View" />

### <a name="split-view"></a>분할된 뷰

지원 되는 iPad 하드웨어 (iPad Air 2, iPad 미니 4 및 iPad Pro만 해당)에서 사용자는 두 번째 앱을 선택 하 고 분할 화면 모드에서 현재 실행 중인 앱과 나란히 실행할 수 있습니다. 사용자는 화면 경계를 끌어 각 앱이 차지 하는 주 화면의 백분율을 제어할 수 있습니다.

[![](multitasking-images/about02.png "분할 뷰")](multitasking-images/about02.png#lightbox)

사용자는 동시에 실행 되는 두 개의 앱을 결정 하 고, 개발자가이 프로세스를 제어 하지 않습니다. 결과적으로, 분할 보기는 Xamarin.ios 앱에 다음과 같은 요구 사항을 적용 합니다.

- **자동 레이아웃 및 크기 클래스 사용** -이제 xamarin.ios 앱을 사용자의 지정 된 크기에서 분할 화면 모드로 실행할 수 있으므로 더 이상 장치, 해당 화면 크기 또는 UI를 레이아웃 하는 방향에 의존할 수 없습니다. 앱에서 인터페이스의 크기를 올바르게 조정 하려면 자동 레이아웃 및 크기 클래스를 사용 해야 합니다. 자세한 내용은 [통합 Storyboard 소개 설명서를](~/ios/user-interface/storyboards/unified-storyboards.md) 참조 하세요.
- **효율적으로 리소스 사용** -앱이 실행 중인 다른 앱과 시스템을 공유할 수 있으므로 앱에서 시스템 리소스를 효율적으로 사용 하는 것이 중요 합니다. 메모리가 스파스가 되 면 시스템은 메모리를 가장 많이 사용 하는 앱을 자동으로 종료 합니다. 자세한 내용은 Apple의 [IOS 앱에 대 한 에너지 효율성 가이드](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) 를 참조 하세요.

분할 보기를 위해 앱을 준비 하는 방법에 대 한 자세한 내용은 [iPad 설명서의 Apple의 멀티태스킹 기능 강화](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145) 를 참조 하세요.

<a name="Picture-in-Picture" />

### <a name="picture-in-picture"></a>그림의 그림

그림의 새 그림 기능 ( _PIP_라고도 함)을 사용 하면 사용자가 실행 중인 다른 앱 위의 화면에서 어디에 나 배치할 수 있는 작은 부동 창의 비디오를 볼 수 있습니다.

[![](multitasking-images/about03.png "그림 부동 창의 예제 그림")](multitasking-images/about03.png#lightbox)

슬라이드 오버 및 분할 보기와 마찬가지로 사용자는 그림 모드에서 그림의 비디오를 시청 하는 모든 권한을 가집니다. 앱의 주요 기능이 비디오를 시청 하는 경우 PIP 모드에서 올바르게 동작 하려면 약간의 수정이 필요 합니다. 그렇지 않으면 PIP를 지원 하기 위해 변경할 필요가 없습니다.

앱이 사용자의 요청에 PIP 비디오를 표시 하려면 _Avkit_ 또는 _AV 기반 api_를 사용 해야 합니다. Media Player 프레임 워크는 iOS 9에서 사용 PIP를 지원 하지 않습니다.

그림의 그림은 iPad Pro, iPad Air, iPad Air 2, iPad 미니 2, iPad 미니 3 또는 iPad 미니 4 에서만 사용할 수 있습니다. 자세한 내용은 [그림 빠른 시작 설명서의 그림](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/QuickStartForPictureInPicture.html#//apple_ref/doc/uid/TP40015145-CH14) [샘플 앱](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9) 및 Apple의 그림을 참조 하세요.

<a name="Supporting-Multitasking-in-your-App" />

## <a name="supporting-multitasking-in-your-app"></a>앱에서 멀티태스킹 지원

기존 Xamarin.ios 앱의 경우 앱이 이미 Apple의 디자인 가이드와 iOS 8의 모범 사례를 준수 하는 한, 멀티태스킹 지원은 투명 한 작업입니다. 즉, 앱은 사용자 인터페이스 레이아웃에 대해 자동 레이아웃 및 크기 클래스와 함께 storyboard를 사용 해야 합니다 (자세한 내용은 [통합 스토리 보드 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 참조).

이러한 앱에 대해 멀티태스킹을 지원 하 고 it에서 효과적으로 작동 하기 위해 거의 또는 전혀 변경 하지 않아도 됩니다. 코드에서 C# ui 요소를 직접 배치 하 고 크기를 조정 하는 등의 다른 방법을 사용 하 여 앱의 ui가 생성 된 경우 또는 특정 장치 화면 크기 또는 방향에 의존 하는 경우 iOS 9 멀티태스킹을 올바르게 지원 하기 위해 상당한 수정이 필요 합니다.

모든 새 Xamarin.ios 앱에서 iOS 9 멀티태스킹을 지원 하려면 모든 앱의 사용자 인터페이스 레이아웃에 대해 자동 레이아웃 및 크기 클래스를 사용 하 여 스토리 보드를 다시 사용 하 고 다음 섹션의 지침을 구현 합니다.

<a name="Screen-Size-Considerations" />

### <a name="screen-size-and-orientation-considerations"></a>화면 크기 및 방향 고려 사항

IOS 9 이전에는 특정 장치 화면 크기 및 방향에 대해 앱을 디자인할 수 있습니다. 이제 앱을 슬라이드 아웃 패널 또는 분할 보기 모드로 실행할 수 있으므로 장치의 실제 방향 또는 화면 크기에 관계 없이 iPad의 compact 또는 regular 가로 크기 클래스에서 실행 되는 것을 찾을 수 있습니다.

[![](multitasking-images/sizeclasses01.png "화면 크기 및 방향 고려 사항")](multitasking-images/sizeclasses01.png#lightbox)

IPad에서 전체 화면 앱에는 일반 가로 및 세로 크기 클래스가 있습니다. 모든 Iphone iPhone 6 Plus 및 iPhone 6s Plus에는 모든 방향으로 양방향으로 압축 된 크기의 클래스가 있습니다. IPhone 6 Plus 및 iPhone 6s Plus 가로 모드에는 일반 가로 크기 클래스와 컴팩트 세로 크기 클래스 (iPad 미니와 매우 유사)가 있습니다.

슬라이드 오버 및 분할 보기를 지 원하는 Ipad에서 다음과 같은 조합을 사용할 수 있습니다.

| **방향** | **기본 앱** | **보조 앱** |
|--- |--- |--- |
| **세로** |화면 75%<br />수평 압축<br />일반 세로|화면 25%<br />수평 압축<br />일반 세로|
| **바꾸십시오** |화면 75%<br />일반 가로<br />일반 세로|화면 25%<br />수평 압축<br />일반 세로|
| **바꾸십시오** |화면 50%<br />수평 압축<br />일반 세로|화면 50%<br />수평 압축<br />일반 세로|

예제 [MuliTask](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-multitask) 앱에서 가로 모드의 iPad에서 전체 화면을 실행 하는 경우 목록 및 세부 정보 보기를 동시에 표시 합니다.

[![](multitasking-images/sizeclasses03.png "목록 및 세부 정보 뷰가 동시에 표시 됩니다.")](multitasking-images/sizeclasses03.png#lightbox)

패널을 통해 동일한 앱이 실행 되는 경우 해당 앱은 간결한 가로 크기 클래스로 배치 되며 목록만 표시 됩니다.

[![](multitasking-images/sizeclasses04.png "장치가 가로 인 경우에만 표시 되는 목록")](multitasking-images/sizeclasses04.png#lightbox)

이러한 상황에서 앱이 올바르게 동작 하도록 하려면 크기 클래스와 함께 특성 컬렉션을 채택 하 고 `IUIContentContainer` 및 `IUITraitEnvironment` 인터페이스를 준수 해야 합니다. 자세한 내용은 Apple의 [Uitraitcollection 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITraitCollection_ClassReference/index.html#//apple_ref/doc/uid/TP40014202) 및 [통합 된 스토리 보드 소개 가이드를](~/ios/user-interface/storyboards/unified-storyboards.md) 참조 하세요.

또한 더 이상 장치 화면 경계를 사용 하 여 앱의 표시 영역을 정의할 수 없습니다. 대신 앱의 창 경계를 사용 해야 합니다. 창 경계는 전적으로 사용자의 제어를 받고 있으므로 프로그래밍 방식으로 조정 하거나 사용자가 이러한 범위를 변경 하지 못하도록 방지할 수 없습니다.

마지막으로, 응용 프로그램은 .png 이미지 파일 집합을 사용 하는 것과는 반대로 storyboard 파일을 사용 하 여 시작 화면을 표시 하 고 네 가지 인터페이스 방향 (세로, 세로 방향 세로, 가로 왼쪽 및 가로 오른쪽)을 모두 지원 합니다 **.** 패널을 통해 슬라이드나 분할 뷰 모드에서 실행 하는 경우

<a name="Custom-Hardware-Keyboard-Shortcuts" />

### <a name="custom-hardware-keyboard-shortcuts"></a>사용자 지정 하드웨어 바로 가기 키

IPad에서 실행 되는 iOS 9에서는 Apple에서 하드웨어 키보드에 대 한 지원이 확장 되었습니다. Ipad는 항상 Bluetooth를 통한 기본적인 외부 키보드 지원 및 하드 유선 iOS 관련 키를 포함 하는 키보드를 만든 키보드 제조업체를 포함 합니다.

이제 iOS 9를 사용 하 여 앱에서 고유한 사용자 지정 바로 가기 키를 만들 수 있습니다. 또한 특수 하 게 작성 된 앱이 없으면 **명령 C** (복사), **명령 X** (잘라내기), **명령 V** (붙여넣기) 및 **명령-H** (홈)와 같은 몇 가지 기본 바로 가기 키를 사용할 수 있습니다.

**명령 탭** 은 사용자가 키보드에서 앱 간을 빠르게 전환할 수 있도록 하는 앱 전환기를 표시 합니다. Mac OS와 매우 비슷합니다.

[![](multitasking-images/keyboard01.png "앱 전환기")](multitasking-images/keyboard01.png#lightbox)

IOS 9 앱에 바로 가기 키가 포함 되어 있는 경우 사용자는 **명령**, **옵션** **또는 키** 를 사용 하 여 팝업에 표시할 수 있습니다.

[![](multitasking-images/keyboard02.png "바로 가기 키 팝업")](multitasking-images/keyboard02.png#lightbox)

#### <a name="defining-custom-keyboard-shortcuts"></a>사용자 지정 바로 가기 키 정의

응용 프로그램의 보기 또는 보기 컨트롤러에 다음 코드를 추가 하는 경우 해당 뷰나 컨트롤러가 표시 되 면 사용자 지정 바로 가기 키를 사용할 수 있습니다.

```csharp
#region Custom Keyboard Shortcut
public override bool CanBecomeFirstResponder {
    get { return true; }
}

public override UIKeyCommand[] KeyCommands {
    get {

        var keyCommand = UIKeyCommand.Create (new NSString("n"), UIKeyModifierFlags.Command, new Selector ("NewEntry"), new NSString("New Entry"));
        return new UIKeyCommand[]{ keyCommand };
    }
}

[Export("NewEntry")]
public void NewEntry() {

    // Add a new entry
    ...

}
#endregion
```

먼저 뷰 또는 뷰 컨트롤러가 `CanBecomeFirstResponder` 키보드 입력을 `true` 받을 수 있도록 속성을 재정의 하 고를 반환 합니다. 

그런 다음, `KeyCommands` 속성을 재정의 하 고 **명령 N** 키 입력에 대 한 새 `UIKeyCommand` 를 만듭니다. 키 입력이 활성화 되 면 `NewEntry` `Export` 명령을 사용 하 여 iOS 9에 노출 하는 메서드를 호출 하 여 요청 된 작업을 수행 합니다.

하드웨어 키보드가 연결 된 iPad에서이 앱을 실행 하 고 사용자가 **명령-N**을 입력 하면 목록에 새 항목이 추가 됩니다. 사용자가 **명령** 키를 누르고 있는 경우 바로 가기 목록이 표시 됩니다.

[![](multitasking-images/keyboard03.png "바로 가기 키 팝업")](multitasking-images/keyboard03.png#lightbox)

예제 구현은 샘플 [멀티태스킹을 앱](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-multitask) 을 참조 하세요.

<a name="Resource-Management-Considerations" />

### <a name="resource-management-considerations"></a>리소스 관리 고려 사항

이미 iOS 8의 디자인 가이드 및 모범 사례를 사용 하는 앱의 경우에도 효율적인 리소스 관리가 문제가 될 수 있습니다. IOS 9에서 앱은 더 이상 메모리, CPU 또는 기타 시스템 리소스를 단독으로 사용 하지 않습니다.

따라서 시스템 리소스를 효과적으로 사용 하거나 메모리 부족 상황에서 종료 되도록 Xamarin.ios 앱을 미세 조정 해야 합니다. 이는 두 번째 앱이 계속 해 서 슬라이드에서 실행 될 수도 있고, 추가 리소스가 필요 하거나 새로 고침 빈도가 초당 60 프레임 미만인 경우에도 계속 해 서 실행 되 고 있는 경우에도 마찬가지입니다.

다음과 같은 사용자 작업 및 그 영향을 고려 합니다.

- **패널을 통해 슬라이드에 텍스트 입력** -앱에 텍스트 입력이 없는 경우에도 이제 UI를 통해 시스템 키보드를 표시할 수 있습니다. 따라서 앱이 키보드 표시 및 숨기기와 같이 키보드 표시 알림에 응답 해야 할 수 있습니다.
- **패널을 통해 슬라이드에서 두 번째 앱 실행** -새 앱은 이제 포그라운드에서 실행 되며 메모리 및 CPU 주기와 같은 시스템 리소스에 대해 기존 앱과 경쟁 합니다.
- **PIP 창에서 비디오 재생** -이 창은 앱 인터페이스의 일부를 처리할 수 있을 뿐 아니라 비디오를 시작한 앱은 백그라운드에서 계속 실행 되며 CPU 및 메모리 리소스를 사용 합니다.

앱이 리소스를 효율적으로 사용 하 고 있는지 확인 하려면 다음을 수행 해야 합니다.

- **계측을 사용 하 여 앱 프로 파일링** -메모리 누수를 확인 합니다. overt CPU 사용량과 앱이 주 스레드를 차단할 수 있는 영역을 확인 합니다.
- **상태 전환 방법에 응답** - **AppDelegate.cs** 파일 재정의 및 백그라운드에서 입력 하거나 반환 하는 앱과 같은 상태 변경 방법에 대 한 응답입니다. 이미지, 데이터, 뷰 및 뷰 컨트롤러와 같은 불필요 한 자산을 해제 합니다.
- **메모리를 많이 사용 하는 앱과 함께 테스트** -메모리 집약적 앱 (위성 보기 모드)을 사용 하 여 실제 iOS 하드웨어에서 슬라이드 아웃 및 분할 보기 모두를 사용 하 여 앱을 실행 하 고 두 앱이 모두 응답 하 고 충돌 하지 않도록 테스트 합니다.

리소스 관리에 대 한 자세한 내용은 Apple의 [IOS 앱에 대 한 에너지 효율성 가이드](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) 를 참조 하세요.

<a name="Opting-Out-of-Multitasking" />

## <a name="opting-out-of-multitasking"></a>멀티태스킹 옵트아웃

Apple은 모든 iOS 9 앱이 멀티태스킹 기능을 지원 한다는 것을 제안 하지만 전체 화면이 제대로 작동 해야 하는 게임 또는 카메라 앱과 같이 앱에 대 한 특별 한 이유가 있을 수 있습니다.

Xamarin.ios 앱이 슬라이드 아웃 패널 또는 분할 보기 모드에서 실행 되는 것을 옵트아웃 하려면 프로젝트의 **info.plist** 파일을 편집 하 고 **전체 화면**을 확인 해야 합니다.

[![](multitasking-images/fullscreen01.png "멀티태스킹 옵트아웃")](multitasking-images/fullscreen01.png#lightbox)

> [!IMPORTANT]
> 멀티태스킹을 옵트아웃 하면 앱이 슬라이드 아웃 또는 분할 보기에서 실행 되지 않지만, 다른 앱이 실행 되는 것을 방지 하지는 않지만, 그림 비디오의 그림은 앱과 함께 표시 됩니다.

<a name="Disabling-PIP-Video-Playback" />

### <a name="disabling-pip-video-playback"></a>PIP 비디오 재생 사용 안 함

대부분의 경우 앱은 사용자가 그림 부동 창의 그림에 표시 되는 비디오 콘텐츠를 재생할 수 있도록 해야 합니다. 그러나 게임 장면 비디오와 같이 이것이 바람직하지 않을 수 있는 경우가 있을 수 있습니다.

PIP 비디오 재생을 옵트아웃 하려면 앱에서 다음을 수행 합니다.

- 을 `AVPlayerViewController` 사용 하 여 비디오를 표시 하는 경우 `AllowsPictureInPicturePlayback` 속성을로 `false`설정 합니다.
- 을 사용 하 `AVPlayerLayer` 여 비디오를 표시 하는 경우를 `AVPictureInPictureController`인스턴스화하지 마세요.
- 을 `WKWebView` 사용 하 여 비디오를 표시 하는 경우 `AllowsPictureInPictureMediaPlayback` 속성을로 `false`설정 합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Ipad에 대 한 iOS 9의 새로운 멀티태스킹 기능에서 Xamarin.ios 앱이 실행 되 고 제대로 작동 하는지 확인 하는 데 필요한 단계에 대해 설명 했습니다. 또한 잘 맞지 않는 앱에 대 한 멀티태스킹의 옵트아웃에 대해 설명 했습니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [멀티태스킹을 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-multitask)
- [통합 Storyboard 소개](~/ios/user-interface/storyboards/unified-storyboards.md)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [IPad에서 멀티태스킹 기능 강화](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145)
- [블로그 게시물](https://blog.xamarin.com/using-auto-layouts-for-ios-9-splitview/)
