---
title: 멀티태스킹 Xamarin.iOS 방향의 iPad 용
description: iOS 9에 동시, 통해 슬라이드를 사용 하 여 또는 분할 된 뷰를 실행 하는 두 개의 앱을 지원 합니다. 또한 비디오 재생 그림에서 그림을 지원 합니다.
ms.prod: xamarin
ms.assetid: 0F2266D7-21FF-404D-A148-0CFDE76B12AA
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 7eacd9ece067d2ddf6363c0551055daa3df4433a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787957"
---
# <a name="multitasking-for-ipad-in-xamarinios"></a>멀티태스킹 Xamarin.iOS 방향의 iPad 용

_iOS 9에 동시, 통해 슬라이드를 사용 하 여 또는 분할 된 뷰를 실행 하는 두 개의 앱을 지원 합니다. 또한 비디오 재생 그림에서 그림을 지원 합니다._

![](multitasking-images/about02-sml.png "화면 예제 분할") ![ ] (multitasking-images/about03-sml.png "그림에서 그림 예제")

iOS 9 iPad 특정 하드웨어에 동시에 두 개의 응용 프로그램을 실행 하기 위한 멀티태스킹 지원을 추가 합니다. 멀티태스킹 iPad에 대 한 다음과 같은 기능을 통해 사용할 수 있습니다.

- [**통해 슬라이드** ](#Slide-Over) -일시적으로 현재 실행 중인 기본 응용 프로그램의 25%에 약 적용 (언어 방향에 따라 화면 오른쪽 이나 왼쪽에 하나) 패널 아웃 슬라이드에서 두 번째는 iOS 앱을 실행 하려면 사용자 수 있습니다. 통해 이동은 iPad Pro, 공기 iPad, iPad 공기 2, iPad 미니 2, 미니 3, iPad 또는 iPad를 미니 4 에서만 사용할 수 있습니다.
- [**분할 된 뷰** ](#Split-View) -지원 되는 iPad 하드웨어 (iPad iPad를 미니 4 및 iPad Pro만 공기 2)에서 사용자는 두 번째 응용 프로그램을 선택 하 고 실행할 수 나란히 분할 화면 모드에서 현재 실행 중인 앱과 함께 합니다. 사용자는 각 응용 프로그램에서 차지 하는 기본 화면의 백분율을 제어할 수 있습니다.
- [**그림에서 그림** ](#Picture-in-Picture) -비디오 콘텐츠 재생, 이제 비디오 수 있는 iOS 장치에서 현재 실행 중인 다른 응용 프로그램을 통해 배치 되는 이동 가능한 및 크기 조정 가능한 창에서 재생할 수 있는 응용 프로그램에 있습니다. 사용자에 게이 창의 위치와 크기에 대 한 모든 권한을 합니다. 그림에는 그림은 iPad Pro, 공기 iPad, iPad 공기 2, iPad 미니 2, 미니 3, iPad 또는 iPad를 미니 4 에서만 사용할 수 있습니다.

시 고려 사항 사항이 [멀티태스킹 응용 프로그램에서 지 원하는](#Supporting-Multitasking-in-your-App)를 포함 하 여:

- [화면 크기와 방향](#Screen-Size-Considerations)
- [사용자 지정 하드웨어 바로 가기 키](#Custom-Hardware-Keyboard-Shortcuts)
- [리소스 관리](#Resource-Management-Considerations)

응용 프로그램 개발자는 수도 있습니다 [멀티태스킹 옵트아웃](#Opting-Out-of-Multitasking)를 포함 하 여 [PIP 비디오 재생을 사용 하지 않도록 설정](#Disabling-PIP-Video-Playback)합니다.

이 문서에서는 멀티태스킹 환경에서 올바르게 실행 되는 Xamarin.iOS 앱 또는 앱에 적합 한 것이 좋은 경우 멀티태스킹 옵트아웃 하는 방법을 확인 하는 데 필요한 단계를 설명 합니다.

> [!VIDEO https://youtube.com/embed/GctYAozoLr8]

**IPad의 경우에 대 한 멀티태스킹 여 [Xamarin 대학](https://university.xamarin.com)**


<a name="Multitasking-QuickStart" />

## <a name="multitasking-quickstart"></a>멀티태스킹 빠른 시작

지원 하기 위해 **슬라이드를 통해** 또는 **분할 보기** 앱에서 다음을 수행 해야 합니다.

 - IOS 9 (또는 그 이상)에 대해 빌드할 수 있습니다.
 - 스토리 보드를 사용 하 여 시작 화면에 대 한 (및 자산 하지 이미지).
 - UI에 자동 레이아웃 및 크기 클래스를 사용한 스토리 보드를 사용 합니다.
 - 모든 4 iOS 장치 방향 (세로, 거꾸로 세로, 가로 왼쪽 및 오른쪽 가로)를 지원 합니다.

<a name="Multitasking" />

## <a name="about-multitasking-for-ipad"></a>IPad 용 다중 작업에 대 한

iOS 9 iPad의 도입으로에서 새로운 멀티태스킹 능력 제공 _슬라이드를 통해_, _분할 보기_ (iPad iPad를 미니 4 및 iPad Pro 공기 2) 및 _그림에는 그림_합니다. 다음 섹션의 좀 더 자세히 살펴보고 이러한 기능 이동 합니다.

<a name="Slide-Over" />

### <a name="slide-over"></a>슬라이드를 통해

기능을 통해 이동 하는 두 번째 응용 프로그램을 선택 하 고 빠른 상호 작용을 제공 하는 작은 슬라이딩 패널에 표시할 수 있습니다. 패널을 통해 슬라이드는 일시적 이며 사용자가 다시 기본 응용 프로그램을 다시 작업할 때 닫힙니다.

[![](multitasking-images/about01.png "통해 슬라이드 패널")](multitasking-images/about01.png#lightbox)

기억해 야 할 주요 사항은 사용자가 어떤 두 응용 프로그램 개발자가 제어할 수 없거나이 프로세스와 나란히 실행 될 결정입니다. 결과적으로, Xamarin.iOS 앱을 통해 슬라이드 패널에서 제대로 실행 되도록 하기 위해 수행 해야 하는 몇 가지 있습니다.

- **자동 레이아웃 및 크기 클래스를 사용 하 여** -Xamarin.iOS 앱 슬라이드 아웃 왼쪽 패널에서 실행할 수 없습니다, 때문에 더 이상 사용할 수는 장치, 화면 크기, 레이아웃의 방향 UI입니다. 응용 프로그램의 인터페이스를 올바로 조정 되도록 자동 레이아웃 및 크기 클래스를 사용 해야 합니다. 자세한 내용은 참조 우리의 [스토리 보드 통합 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 설명서입니다.
- **리소스가 효율적으로 사용 하 여** -앱 수 이제 공유 시스템 실행 중인 다른 응용 프로그램으로, 되므로 중요 한 응용 프로그램 시스템 리소스를 효율적으로 사용 합니다. 메모리 스파스 되 면 시스템은 자동으로 가장 많은 메모리를 소비 하는 앱을 끝납니다. Apple의 참조 [iOS 앱에 대 한 에너지 효율성 가이드](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) 자세한 세부 정보에 대 한 합니다.

통해 이동은 iPad Pro, 공기 iPad, iPad 공기 2, iPad 미니 2, 미니 3, iPad 또는 iPad를 미니 4 에서만 사용할 수 있습니다. 통해 슬라이드에 대 한 앱을 준비 하는 방법에 대 한 자세한 내용은 Apple의를 참조 하십시오 [iPad에서 멀티태스킹 향상 기능을 채택](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145) 설명서입니다.

<a name="Split-View" />

### <a name="split-view"></a>분할된 뷰

지원 되는 iPad 하드웨어 (iPad iPad를 미니 4 및 iPad Pro만 공기 2)에서 사용자는 두 번째 응용 프로그램을 선택 하 고 분할 화면 모드에서 현재 실행 중인 응용 프로그램을 사용 하 여-병렬 실행 수입니다. 사용자의 기본 화면으로 끌어 각 앱 차지 하는 백분율을 제어할 수 있는 화면 구분선입니다.

[![](multitasking-images/about02.png "분할 뷰")](multitasking-images/about02.png#lightbox)

슬라이드 통해와 마찬가지로 사용자가 어떤 두 응용 프로그램-병렬 실행을 결정 하 고 다시 개발자는이 프로세스를 통해 제어 하지 않습니다. 결과적으로, 분할 된 뷰 Xamarin.iOS 앱에서 이와 비슷한 요구 사항을 넣습니다.

- **자동 레이아웃 및 크기 클래스를 사용 하 여** -Xamarin.iOS 앱 수 이제 사용자의 지정 된 크기에서 분할 화면 모드로 실행할 수 없습니다, 때문에 더 이상 사용할 수는 장치, 화면 크기, 레이아웃의 방향 UI입니다. 응용 프로그램의 인터페이스를 올바로 조정 되도록 자동 레이아웃 및 크기 클래스를 사용 해야 합니다. 자세한 내용은 참조 우리의 [스토리 보드 통합 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 설명서입니다.
- **리소스가 효율적으로 사용 하 여** -앱 수 이제 공유 시스템 실행 중인 다른 응용 프로그램으로, 되므로 중요 한 응용 프로그램 시스템 리소스를 효율적으로 사용 합니다. 메모리 스파스 되 면 시스템은 자동으로 가장 많은 메모리를 소비 하는 앱을 끝납니다. Apple의 참조 [iOS 앱에 대 한 에너지 효율성 가이드](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) 자세한 세부 정보에 대 한 합니다.

분할 뷰에 대 한 앱을 준비 하는 방법에 대 한 자세한 내용은 Apple의를 참조 하십시오 [iPad에서 멀티태스킹 향상 기능을 채택](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145) 설명서입니다.

<a name="Picture-in-Picture" />

### <a name="picture-in-picture"></a>그림의 그림

그림 기능에서 새 그림 (라고도 _PIP_) 사용자는 위에서 실행 중인 다른 응용 프로그램 화면에서 아무 곳 이나 배치할 수 있는 작은, 부동 창에서 비디오를 볼 수 있습니다.

[![](multitasking-images/about03.png "예가 그림 부동 창에는 그림")](multitasking-images/about03.png#lightbox)

슬라이드를 통해 및 분할 뷰를 사용 하 여 하는 사용자 그림 모드로 그림에는 비디오를 시청에 대해 모든 권한을 갖습니다. 응용 프로그램의 main 함수를 비디오를 시청 하려면 PIP 모드에서 제대로 동작 하려면이 필요 합니다. 그렇지 않으면 변경 내용이 없습니다 PIP를 지원 해야 합니다.

사용자의 요청에 PIP 비디오를 표시 하는 응용 프로그램을 하나 사용 해야 합니다 _AVKit_ 또는 _AV Foundation Api_합니다. 미디어 플레이어 프레임 워크는 iOS 9에에서 위해서 되었습니다 및 PIP를 지원 하지 않습니다.

그림에는 그림은 iPad Pro, 공기 iPad, iPad 공기 2, iPad 미니 2, 미니 3, iPad 또는 iPad를 미니 4 에서만 사용할 수 있습니다. 자세한 내용은 참조 하십시오 우리의 [PictureInPicture 샘플 응용 프로그램](https://developer.xamarin.com/samples/ios/iOS9/) 과 Apple [그림 빠른 시작에서 그림](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/QuickStartForPictureInPicture.html#//apple_ref/doc/uid/TP40015145-CH14) 설명서입니다.

<a name="Supporting-Multitasking-in-your-App" />

## <a name="supporting-multitasking-in-your-app"></a>멀티태스킹 응용 프로그램에서 지원

모든 기존 Xamarin.iOS 앱에 대 한 지원 멀티태스킹을 투명 하 게 작업으로 응용 프로그램 이미 Apple의 설계 가이드 및 iOS 8에 대 한 모범 사례를 따릅니다. 즉,는 응용 프로그램에 따라 사용 해야 스토리 보드 슬라이드 레이아웃 및 크기 클래스와 해당 사용자 인터페이스 레이아웃 (참조 우리의 [스토리 보드 통합 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 자세한 정보에 대 한).

이러한 앱에 대해 거의 또는 전혀 변경이 필요한 멀티태스킹 지원 하 고에 속함을 동작 하도록 합니다. 응용 프로그램의 UI를 만든 경우 직접 배치 및 C# 코드 또는 특정 장치 화면 크기 또는 방향에 의존 하는 경우의 UI 요소 크기 조정 등의 다른 방법을 사용 하 여 iOS 9 멀티태스킹을 올바르게 지원 하도록 많은 부분을 수정이 필요 합니다.

IOS 9 멀티태스킹 새 Xamarin.iOS 앱을 지원 하려면 자동 레이아웃 및 크기 클래스를 사용한 스토리 보드를 사용 하 여 모든 응용 프로그램의 사용자 인터페이스 레이아웃에 대 한 다시 고 다음 섹션의 지침을 구현 합니다.

<a name="Screen-Size-Considerations" />

### <a name="screen-size-and-orientation-considerations"></a>화면 크기와 방향 고려 사항

IOS 9 하기 전에 특정 장치 화면 크기와 방향이 설정에 대 한 앱을 디자인할 수 있습니다. 앱 슬라이드 아웃 패널 또는 분할 보기 모드에서 실행할 수 있습니다, 때문에 실행 compact 또는 일반 가로 크기 클래스 장치의 실제 방향 또는 화면 크기에 관계 없이 iPad에서 자체를 찾을 수 있습니다.

[![](multitasking-images/sizeclasses01.png "화면 크기와 방향 고려 사항")](multitasking-images/sizeclasses01.png#lightbox)

IPad에서 전체 화면 앱에 일반 가로 및 세로 크기가 클래스가 있습니다. 모든 Iphone 있지만 iPhone 6 Plus 및 iPhone 6s 방향에서 모두에 클래스를 압축 크기 또한 합니다. IPhone 6 Plus 및 iPhone 6s 가로 모드에서 일반 가로 크기 클래스 및 흡사 iPad 미니 세로 크기 클래스를 압축 해야 합니다.

위로 이동 및 분할 된 뷰를 지 원하는 Ipad를에서 다음과 같은 조합을 결국 수 있습니다.:

| **방향** | **기본 응용 프로그램** | **보조 응용 프로그램** |
|--- |--- |--- |
| **세로** |화면의 75%<br />Compact 가로<br />일반 세로|화면의 25%<br />Compact 가로<br />일반 세로|
| **가로** |화면의 75%<br />일반 가로<br />일반 세로|화면의 25%<br />Compact 가로<br />일반 세로|
| **가로** |화면 50%<br />Compact 가로<br />일반 세로|화면 50%<br />Compact 가로<br />일반 세로|

예제에서 [MuliTask](https://developer.xamarin.com/samples/monotouch/ios9/MultiTask/) 목록과 동시에 세부 정보 보기 모두 제공 합니다 앱을 전체 화면 가로 모드로 iPad에서 실행 되는 경우:

[![](multitasking-images/sizeclasses03.png "동시에 표시 되는 세부 정보 보기 및 목록")](multitasking-images/sizeclasses03.png#lightbox)

동일한 응용 프로그램을 통해 슬라이드 패널에서를 실행 하는 경우 Compact 가로 크기 클래스로으로 배치 하 고 목록만 표시:

[![](multitasking-images/sizeclasses04.png "장치가 가로 때 제공 된 목록에만")](multitasking-images/sizeclasses04.png#lightbox)

앱이에서 올바르게 작동을 보장 하려면 특성 컬렉션 크기 클래스와 함께 채택 하 고 준수 해야는 `IUIContentContainer` 및 `IUITraitEnvironment` 인터페이스입니다. Apple의를 참조 하십시오. [UITraitCollection 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITraitCollection_ClassReference/index.html#//apple_ref/doc/uid/TP40014202) 및 [스토리 보드 통합 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 대 한 자세한 내용은 가이드입니다.

또한 더 이상 응용 프로그램의 표시 영역을 정의 하는 장치 화면 경계에 의존할 수 없습니다, 응용 프로그램의 창 범위를 대신 사용 해야 합니다. 창 범위는 사용자의 제어를 받으며 완벽 하 게 되므로 프로그래밍 방식으로 조정할 수 없거나 사용자가이 경계를 변경 하지 못하도록 합니다.

응용 프로그램의 집합을 사용 하지 않고 시작 화면을 표시 하도록 스토리 보드 파일을 사용 해야 마지막으로, **.png** 이미지 파일 및 모든 4 개의 인터페이스 방향 (세로, 거꾸로 세로, 가로 왼쪽 및 오른쪽 가로)를 지원 합니다. 패널을 통해 이동 또는 분할 보기 모드에서 실행 하는 데 고려해 야 합니다.

<a name="Custom-Hardware-Keyboard-Shortcuts" />

### <a name="custom-hardware-keyboard-shortcuts"></a>사용자 지정 하드웨어 바로 가기 키

iOS 9 iPad에서 실행, Apple에 하드웨어 키보드에 대 한 추가 지원 합니다. Ipad에는 항상 Bluetooth 및 하드 연결 iOS 전용 키를 포함 하는 일부 키보드 만든 제조업체 키보드를 통해 기본 외부 키보드 지원을 포함 됩니다.

이제 iOS 9, 앱 사용자 지정 바로 가기 키가 자신의 만들 수 있습니다. 또한, 일부 기본 바로 가기 키와 같은 사용할 수 있습니다 **명령 C** (복사), **명령 X** (잘라내기), **명령-V** (붙여넣기) 및 **명령-Shift-H**  (홈)에 특별히 작성 된 응답 되는 응용 프로그램 없이 합니다.

**명령 탭** Mac OS 매우 유사 하 게 키보드에서 응용 프로그램 사이 빠르게 전환할 수 있도록 하는 응용 프로그램 전환기를 표시 합니다.

[![](multitasking-images/keyboard01.png "응용 프로그램 전환기")](multitasking-images/keyboard01.png#lightbox)

IOS 9 앱에서 바로 가기 키가 포함 된 경우 사용자 수 키를 누른 채는 **명령**, **옵션** 또는 **제어** 팝업에 표시 하는 키:

[![](multitasking-images/keyboard02.png "바로 가기 키 팝업")](multitasking-images/keyboard02.png#lightbox)

#### <a name="defining-custom-keyboard-shortcuts"></a>사용자 지정 바로 가기 키를 정의합니다.

추가할 경우 다음 코드 보기 또는 뷰-컨트롤러 앱에서 해당 뷰 또는 컨트롤러 표시 되 면, 사용자 지정 바로 가기 키를 사용할 수 있습니다.

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

첫째, 재정의 `CanBecomeFirstResponder` 속성 및 반환 `true` 보기 또는 뷰-컨트롤러 키보드 입력을 받을 수 있도록 합니다. 

다음으로 재정의 `KeyCommands` 속성을 새로 만듭니다 `UIKeyCommand` 에 대 한는 **명령 N** 키 입력 합니다. 호출에서 키 입력 활성화 되 면는 `NewEntry` 메서드 (사용 하 여 iOS 9를 노출 하는 `Export` 명령) 요청한 작업을 수행할 합니다.

하드웨어 키보드를 연결 및 사용자가 iPad에서이 응용 프로그램을 실행 하는 경우 **명령 N**, 새 항목이 목록에 추가 됩니다. 사용자는에 유지 하는 경우는 **명령** 키, 바로 가기 목록이 표시 됩니다.

[![](multitasking-images/keyboard03.png "바로 가기 키 팝업")](multitasking-images/keyboard03.png#lightbox)

이 샘플을 참조 하십시오 [MultiTask 앱](http://developer.xamarin.com/samples/monotouch/ios9/MultiTask/) 예제 구현에 대 한 합니다.

<a name="Resource-Management-Considerations" />

### <a name="resource-management-considerations"></a>리소스 관리 고려 사항

IOS 8의 디자인 지침 및 모범 사례를 이미 사용 중인 응용 프로그램에도 리소스를 효과적으로 관리 문제를 여전히 수 있습니다. IOS 9에에서 앱 없다면 메모리, CPU 또는 기타 시스템 리소스를 단독으로 사용 합니다.

결과적으로, 시스템 리소스를 효과적으로 사용 하도록 Xamarin.iOS 앱을 미세 조정 해야 하거나 메모리가 부족 한 경우 아래에서 종료를 연결 합니다. 다중 작업을 취소 하는 응용 프로그램에 동일 하 게 해당 됩니다, 그리고 두 번째 이후 응용 프로그램 계속 실행 될 수 슬라이드를 통해 패널 또는 초당 60 프레임에 추가 리소스를 필요로 하거나 새로 고침 빈도 일으키는 그림 창에 그림에서 합니다.

다음과 같은 사용자 작업 및 해당 의미를 고려 합니다.

- **패널을 통해 슬라이드에 텍스트를 입력** -앱에 없는 텍스트 입력, 경우에 시스템 키보드 표시할 수 있습니다 UI를 통해 합니다. 결과적으로, 응용 프로그램 키보드 표시 알림 (예:을 표시 하거나 숨김으로써 키보드)에 응답 해야 합니다.
- **패널을 통해 슬라이드에 두 번째 응용 프로그램을 실행** -새 앱은 이제 포그라운드에서 실행 중 이며 경쟁 하 고 메모리 및 CPU 주기 같은 시스템 리소스에 대 한 기존 응용 프로그램입니다.
- **PIP 창에서 비디오 재생** -하지 비디오를 시작 하는 앱 여전히 백그라운드에서 실행 하 고 CPU 및 메모리 리소스를 소비 되지만이 창은 응용 프로그램의 인터페이스의 일부를 포함할 수 있습니다.

을 보장 하기 위해 앱 리소스를 효율적으로 사용 되는 다음을 수행 해야 합니다.

- **장비 응용 프로그램 프로 파일링** -성격의 CPU 사용량 및 영역에 응용 프로그램 차단할 수도 있습니다는 주 스레드에 메모리 누수를 확인 합니다.
- **상태 전환 메서드에 응답** -프로그램 **AppDelegate.cs** 파일 재정 및 상태에 대 한 응답을 입력 하거나 백그라운드에서 반환 하는 앱과 같은 메서드를 변경 합니다. 이미지, 데이터 또는 뷰 뷰-컨트롤러 등 불필요 모든 자산을 해제 합니다.
- **나란히 메모리 집약적인 앱과 테스트** -위성 보기 모드에서 집약적인 앱 맵과 같은 메모리 사용 하 여 아웃 슬라이드와 물리적 Io 하드웨어에서 분할 된 뷰를 사용 하 여 앱을 실행 하 고 테스트 두 앱 응답성 및 충돌 하지 않습니다.

Apple의 참조 [iOS 앱에 대 한 에너지 효율성 가이드](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) 리소스 관리에 대 한 자세한 내용은 합니다.

<a name="Opting-Out-of-Multitasking" />

## <a name="opting-out-of-multitasking"></a>멀티태스킹 옵트아웃

Apple 모든 iOS 9 앱 멀티태스킹 지원 하는지 알 수, 사항이 있을 수 있습니다는 응용 프로그램에 대 한 매우 구체적인 이유가 너무, 게임 또는 제대로 작동 하려면 전체 화면을 필요로 하는 카메라 앱과 같이 합니다.

Xamarin.iOS 앱 중 하나는 아웃 슬라이드 패널 또는 분할 보기 모드에서 실행 되 고 컬렉션에서 프로젝트의 편집 **Info.plist** 파일을 확인 **전체 화면 필요**:

[![](multitasking-images/fullscreen01.png "멀티태스킹 옵트아웃")](multitasking-images/fullscreen01.png#lightbox)

> [!IMPORTANT]
> 슬라이드 아웃 또는 분할 뷰에서 실행 되지 않도록 앱을 방지 멀티태스킹을 옵트아웃을 하는 동안 앱과 함께 표시 안 함 슬라이드 아웃 또는 그림 비디오의 그림에서 실행할 수 있는 다른 앱이 중단 되지는 않습니다.

<a name="Disabling-PIP-Video-Playback" />

### <a name="disabling-pip-video-playback"></a>PIP 비디오 재생을 사용 하지 않도록 설정

대부분의 경우에서 응용 프로그램 사용자가 표시 되는 그림 부동 창에 그림 비디오 콘텐츠를 재생 하도록 허용 해야 합니다. 그러나 여기서이 수도 적합 하지 게임 컷된 장면 비디오와 같은 상황 있을 수 있습니다.

PIP 비디오 재생을 취소 하기 위해 앱에서 다음을 수행 합니다.

 - 사용 하는 경우는 `AVPlayerViewController` 설정 비디오를 표시 하는 `AllowsPictureInPicturePlayback` 속성을 `false`합니다.
 - 사용 하는 경우는 `AVPlayerLayer` 비디오를 표시 하지 않는 인스턴스화할는 `AVPictureInPictureController`합니다.
 - 사용 하는 경우는 `WKWebView` 설정 비디오를 표시 하는 `AllowsPictureInPictureMediaPlayback` 속성을 `false`합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서는 Xamarin.iOS 앱 실행 되 고 Ipad의 iOS 9의 새 멀티태스킹 능력에서 올바르게 작동 확인 하는 데 필요한 단계 검사가 수행 됩니다. 또한 멀티태스킹 수 없는 가장 잘 맞는 응용 프로그램의 품질을 높이면 아웃을 다룹니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [멀티태스킹이 (샘플)](https://developer.xamarin.com/samples/monotouch/ios9/MultiTask/)
- [통합 된 스토리 보드에는 소개](~/ios/user-interface/storyboards/unified-storyboards.md)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [IPad에서 멀티태스킹 향상 된 기능을 채택합니다.](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145)
- [블로그 게시물](https://blog.xamarin.com/using-auto-layouts-for-ios-9-splitview/)
