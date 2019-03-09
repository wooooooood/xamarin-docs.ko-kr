---
title: Xamarin.iOS 방향의 iPad 용 멀티태스킹
description: iOS 9 동시, 슬라이드를 사용 하 여을 통해 또는 분할 된 뷰를 실행 하는 두 개의 앱을 지원 합니다. 또한 그림에서 그림을 재생 하는 비디오를 지원 합니다.
ms.prod: xamarin
ms.assetid: 0F2266D7-21FF-404D-A148-0CFDE76B12AA
ms.technology: xamarin-ios
ms.custom: xamu-video
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 68c2ae6eace2669d2ea6c77d72f4476d767c0a7d
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57672445"
---
# <a name="multitasking-for-ipad-in-xamarinios"></a>Xamarin.iOS 방향의 iPad 용 멀티태스킹

_iOS 9 동시, 슬라이드를 사용 하 여을 통해 또는 분할 된 뷰를 실행 하는 두 개의 앱을 지원 합니다. 또한 그림에서 그림을 재생 하는 비디오를 지원 합니다._

![](multitasking-images/about02-sml.png "화면 예제 분할") ![](multitasking-images/about03-sml.png "그림에서 그림 예제")

iOS 9 iPad 특정 하드웨어에서 동시에 두 개의 앱을 실행 하기 위한 멀티태스킹 지원을 추가 합니다. IPad 용 멀티태스킹 다음 기능을 통해 사용할 수 있습니다.

- [**통해 슬라이드** ](#Slide-Over) -일시적으로 슬라이드 아웃 약 25%를 현재 실행 중인 기본 앱을 포함 하는 (언어 방향에 따라 화면 오른쪽 이나 왼쪽에 하나) 패널에에서는 두 번째 iOS 앱을 실행할 수 있습니다. 이동 되는 iPad Pro, iPad 항공, iPad 공기 2, iPad 미니 2, 미니 3, iPad 또는 iPad 미니 4 에서만 사용할 수 있습니다.
- [**분할 된 뷰** ](#Split-View) -지원 되는 iPad 하드웨어 (iPad 공기 2, iPad 미니 4 및 iPad Pro만 해당)에서 사용자는 두 번째 앱을 선택 하 고 실행할 수 side-by-side-분할 화면 모드에서 현재 실행 중인 앱을 사용 하 여 합니다. 사용자는 각 앱 차지 하는 주 화면 비율을 제어할 수 있습니다.
- [**그림에는 그림** ](#Picture-in-Picture) -비디오 콘텐츠가 재생, 비디오 수 이제 iOS 장치에서 현재 실행 중인 다른 앱을 통해 배치 되는 이동 가능한 및 크기 조정 가능한 창에서 재생할 수 있는 앱입니다. 사용자에는 크기와이 창의 위치를 통해 모든 권한을 가집니다. 그림에는 그림은 iPad Pro, iPad 항공, iPad 공기 2, iPad 미니 2, iPad 미니 3 또는 iPad 미니 4 에서만 사용할 수 있습니다.

시 고려 사항 많습니다 [멀티태스킹 앱에서 지 원하는](#Supporting-Multitasking-in-your-App)등.

- [화면 크기 및 방향](#Screen-Size-Considerations)
- [사용자 지정 하드웨어 바로 가기 키](#Custom-Hardware-Keyboard-Shortcuts)
- [리소스 관리](#Resource-Management-Considerations)

앱 개발자 할 수도 있습니다 [멀티태스킹 옵트아웃](#Opting-Out-of-Multitasking)등 [PIP 비디오 재생을 사용 하지 않도록 설정](#Disabling-PIP-Video-Playback)합니다.

이 문서에서는 멀티태스킹 환경에서 올바르게 실행 되는 Xamarin.iOS 앱에 앱에 대 한 적합 한 없으면 멀티태스킹 옵트아웃 하는 방법을 확인 하는 데 필요한 단계를 설명 합니다.

> [!VIDEO https://youtube.com/embed/GctYAozoLr8]

**IPad 용 멀티태스킹 여 [Xamarin University](https://university.xamarin.com)**


<a name="Multitasking-QuickStart" />

## <a name="multitasking-quickstart"></a>멀티태스킹 빠른 시작

지원 하기 위해 **슬라이드를 통해** 또는 **분할 보기** 앱에는 다음을 수행 해야 합니다.

 - IOS 9 이상에 대해 빌드됩니다.
 - 스토리 보드를 사용 하 여 시작 화면에 대 한 (및 이미지 자산 없습니다).
 - 자동 레이아웃 및 Size 클래스를 사용 하 여 해당 UI에 대 한 스토리 보드를 사용 합니다.
 - 모든 4 개의 iOS 장치 방향 (세로, 거꾸로 세로, 가로 왼쪽 및 오른쪽 가로 방향)을 지원 합니다.

<a name="Multitasking" />

## <a name="about-multitasking-for-ipad"></a>IPad 용 멀티태스킹에 대 한

iOS 9 제공 되면서 iPad에서 새로운 멀티태스킹 능력 _슬라이드를 통해_, _분할 보기_ (iPad 공기 2, iPad 미니 4 및 iPad Pro) 및 _그림에는 그림_합니다. 다음 섹션에서 이러한 기능에 대해 좀 더 자세히 알아보겠습니다.

<a name="Slide-Over" />

### <a name="slide-over"></a>슬라이드를 통해

슬라이드를 통해 기능을 사용 하면 두 번째 앱을 선택 하 고 빠른 상호 작용을 제공 하는 작은 상대 (sliding) 패널에 표시를 합니다. 패널을 통해 슬라이드는 일시적 이며 사용자가 기본 앱을 다시 사용 하는 경우 닫힙니다.

[![](multitasking-images/about01.png "슬라이드를 통해 패널")](multitasking-images/about01.png#lightbox)

기억할 중요 한 점은 사용자가 side-by-side-및 개발자가 제어할 수 없거나이 프로세스는 두 개의 앱 실행을 결정 하는 합니다. 결과적으로, 가지 Xamarin.iOS 앱을 통해 슬라이드 패널에서 제대로 실행 되도록 하기 위해 수행 해야 하는 몇 가지 사항이 있습니다.

- **자동 레이아웃 및 Size 클래스를 사용 하 여** -이제 슬라이드 아웃 측면 패널에서 Xamarin.iOS 앱을 실행할 수 없습니다, 되므로 더 이상 사용 장치, 화면 크기 또는 해당 방향으로 레이아웃 UI입니다. 앱의 인터페이스를 올바르게 조정 되도록 자동 레이아웃 및 크기 클래스를 사용 해야 합니다. 자세한 내용은 참조 하세요. 당사의 [통합 스토리 보드 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 설명서.
- **리소스 효율적으로 사용 하 여** -앱 수 이제 공유 시스템 다른 실행 중인 앱을 사용 하 여, 이므로 중요 한 응용 프로그램 시스템 리소스를 효율적으로 사용 합니다. 메모리 스파스 되 면 시스템 가장 많은 메모리를 사용 하는 앱에 자동으로 종료 됩니다. Apple의를 참조 하세요 [iOS 앱에 대 한 에너지 효율성 가이드](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) 대 한 자세한 내용은 합니다.

이동 되는 iPad Pro, iPad 항공, iPad 공기 2, iPad 미니 2, 미니 3, iPad 또는 iPad 미니 4 에서만 사용할 수 있습니다. 통해 슬라이드에 대 한 앱을 준비 하는 방법에 대 한 자세한 내용은 Apple의를 참조 하세요 [iPad에 채택 멀티태스킹 향상](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145) 설명서.

<a name="Split-View" />

### <a name="split-view"></a>분할된 뷰

지원 되는 iPad 하드웨어 (iPad 공기 2, iPad 미니 4 및 iPad Pro만 해당)에서 사용자는 두 번째 앱을 선택 하 고 분할 화면 모드에서 현재 실행 중인 앱을 사용 하 여 side-by-side-실행 수 있습니다. 사용자의 주 화면으로 끌어 각 앱 차지 하는 백분율을 제어할 수는 화면의 구분선입니다.

[![](multitasking-images/about02.png "분할 뷰")](multitasking-images/about02.png#lightbox)

슬라이드 조치와 마찬가지로 사용자가 있는 두 개의 앱-병렬 실행을 결정 하 고 마찬가지로 개발자는이 프로세스를 제어할 수 없습니다. 결과적으로, 분할 보기 Xamarin.iOS 앱에 유사한 요구 사항에 넣습니다.

- **자동 레이아웃 및 Size 클래스를 사용 하 여** -이제 사용자의 지정 된 크기로 분할 화면 모드에서 Xamarin.iOS 앱을 실행할 수 없습니다, 되므로 더 이상 사용할 수 장치, 화면 크기 또는 레이아웃으로 해당 방향 UI입니다. 앱의 인터페이스를 올바르게 조정 되도록 자동 레이아웃 및 크기 클래스를 사용 해야 합니다. 자세한 내용은 참조 하세요. 당사의 [통합 스토리 보드 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 설명서.
- **리소스 효율적으로 사용 하 여** -앱 수 이제 공유 시스템 다른 실행 중인 앱을 사용 하 여, 이므로 중요 한 응용 프로그램 시스템 리소스를 효율적으로 사용 합니다. 메모리 스파스 되 면 시스템 가장 많은 메모리를 사용 하는 앱에 자동으로 종료 됩니다. Apple의를 참조 하세요 [iOS 앱에 대 한 에너지 효율성 가이드](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) 대 한 자세한 내용은 합니다.

분할 뷰에 대 한 앱을 준비 하는 방법에 대 한 자세한 내용은 Apple의를 참조 하세요 [iPad에 채택 멀티태스킹 향상](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145) 설명서.

<a name="Picture-in-Picture" />

### <a name="picture-in-picture"></a>그림에는 그림

그림 기능에서 새 그림 (라고도 _PIP_) 사용자 위의 다른 실행 중인 앱 화면에서 아무 곳 이나 배치할 수 있는 소규모의 부동 창에서 비디오를 볼 수 있습니다.

[![](multitasking-images/about03.png "사진 떠 창에는 그림 예제")](multitasking-images/about03.png#lightbox)

처럼 슬라이드 및 분할 뷰를 사용 하 여 사용자가 전체 그림 그림 모드에서 비디오를 시청 합니다. 비디오를 시청 하려면 main 함수 앱의 경우 PIP 모드에서 제대로 동작을 약간 수정을 해야 합니다. 이 고, 그렇지 내용이 PIP를 지원 해야 합니다.

사용자의 요청에 PIP 비디오를 표시 하 여 앱에 대 한 중 하나 사용 해야 합니다 _AVKit_ 또는 _AV Foundation Api_합니다. 미디어 플레이어 프레임 워크는 iOS 9에서에서 상각액이 되었습니다 및 PIP를 지원 하지 않습니다.

그림에는 그림은 iPad Pro, iPad 항공, iPad 공기 2, iPad 미니 2, iPad 미니 3 또는 iPad 미니 4 에서만 사용할 수 있습니다. 자세한 내용은 참조 하십시오 우리의 [PictureInPicture 샘플 앱](https://developer.xamarin.com/samples/ios/iOS9/) 과 Apple [그림 빠른 시작에서 그림](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/QuickStartForPictureInPicture.html#//apple_ref/doc/uid/TP40015145-CH14) 설명서.

<a name="Supporting-Multitasking-in-your-App" />

## <a name="supporting-multitasking-in-your-app"></a>앱에서 멀티태스킹 지원

모든 기존 Xamarin.iOS 앱에 대해 멀티태스킹을 지 원하는 투명 하 게 작업 한 앱 이미 Apple 디자인 가이드 및 iOS 8에 대 한 모범 사례를 따릅니다. 이 앱에 따라 사용 해야 스토리 보드 자동 레이아웃 및 Size 클래스를 사용 하 여 해당 사용자 인터페이스 레이아웃을 의미 (참조 우리의 [통합 스토리 보드 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 자세한).

이러한 앱에 대 한 거의 또는 전혀 변경이 필요한 멀티태스킹 지원 하 고 그 잘 작동 합니다. 앱의 UI에 직접 배치에서 UI 요소 크기 조정 등 다른 방법을 사용 하 여 생성 된 C# 코드 또는 특정 장치 화면 크기 또는 방향에 의존 하는 경우 iOS 9 멀티태스킹을 제대로 지원 하기 위해 많은 부분을 수정 해야 합니다.

를 지원 하기 위해 iOS 9 멀티태스킹 새 Xamarin.iOS 앱에 자동 레이아웃 및 Size 클래스를 사용 하 여 스토리 보드를 사용 하 여 모든 앱의 사용자 인터페이스 레이아웃에 대 한 다시 다음 섹션의 지침을 구현 합니다.

<a name="Screen-Size-Considerations" />

### <a name="screen-size-and-orientation-considerations"></a>화면 크기 및 방향 고려 사항

IOS 9 하기 전에 특정 장치 화면 크기 및 방향에 대해 앱을 디자인할 수 있습니다. 분할 보기 모드 또는 슬라이드 아웃 패널에서 앱을 실행할 수 있습니다, 되므로 실행에서 압축 또는 일반 가로 크기 클래스 iPad 장치의 실제 방향 또는 화면 크기에 관계 없이 자체를 찾을 수 있습니다.

[![](multitasking-images/sizeclasses01.png "화면 크기 및 방향 고려 사항")](multitasking-images/sizeclasses01.png#lightbox)

IPad에서 전체 화면 앱을 일반 가로 및 세로 크기 클래스에 있습니다. 모든 Iphone 있지만 iPhone 6 Plus 및 iPhone 6s 방향에서 양방향으로는 압축 크기 클래스 또한 합니다. IPhone 6 Plus 및 iPhone 6s Plus 가로 모드에 있고 일반 가로 크기 클래스 (미니 iPad와 비슷하게) Compact 세로 크기 클래스입니다.

슬라이드 및 분할 뷰를 지 원하는 iPads에 다음과 같은 조합을로 끝날 수 있습니다.:

| **방향** | **기본 앱** | **보조 앱** |
|--- |--- |--- |
| **세로** |화면의 75%<br />Compact 가로<br />일반 세로|화면의 25%<br />Compact 가로<br />일반 세로|
| **Landscape** |화면의 75%<br />일반 가로<br />일반 세로|화면의 25%<br />Compact 가로<br />일반 세로|
| **Landscape** |화면 50%<br />Compact 가로<br />일반 세로|화면 50%<br />Compact 가로<br />일반 세로|

예에서 [MuliTask](https://developer.xamarin.com/samples/monotouch/ios9/MultiTask/) 앱을 전체 화면을 iPad 가로 모드에서 실행 되는 경우, 목록 및 세부 정보 뷰에서 동시에 모두 표시 됩니다 것:

[![](multitasking-images/sizeclasses03.png "목록과 동시에 표시 되는 세부 정보 보기")](multitasking-images/sizeclasses03.png#lightbox)

동일한 앱을 통해 슬라이드 패널에 실행 된 경우 Compact 가로 크기 클래스로 배치 하 고 목록만 표시:

[![](multitasking-images/sizeclasses04.png "장치가 가로 때 제공 된 목록만")](multitasking-images/sizeclasses04.png#lightbox)

앱 이러한 상황에서는 올바르게 동작 하도록 Size 클래스와 함께 특성 (trait) 컬렉션을 채택 및 준수 해야 합니다 `IUIContentContainer` 고 `IUITraitEnvironment` 인터페이스입니다. Apple의를 참조 하세요 [UITraitCollection 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITraitCollection_ClassReference/index.html#//apple_ref/doc/uid/TP40014202) 및 [통합 스토리 보드 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 자세한 가이드입니다.

또한 앱의 표시 영역을 정의 하는 장치 화면 범위를 더 이상 사용할 수 없습니다, 그리고 앱의 창 경계를 대신 사용 해야 합니다. 사용자의 제어 창 범위를 완벽 하 게 되므로 프로그래밍 방식으로 조정 없거나 사용자가 이러한 범위를 변경할 수 없습니다.

앱 집합을 사용 하는 대신 해당 시작 화면을 표시 하는 스토리 보드 파일을 사용 해야 합니다 마지막으로, **.png** 이미지 파일 및 모든 네 가지 인터페이스 방향 (세로, 거꾸로 세로, 가로 왼쪽 및 오른쪽 가로)를 지원 합니다. 슬라이드를 통해 패널 또는 분할 보기 모드에서 실행 하는 데 고려해 야 합니다.

<a name="Custom-Hardware-Keyboard-Shortcuts" />

### <a name="custom-hardware-keyboard-shortcuts"></a>사용자 지정 하드웨어 바로 가기 키

iOS 9 iPad에서 실행 되는 Apple 하드웨어 키보드 확장된 지원 합니다. Ipad 항상 Bluetooth 및 유선 iOS 전용 키를 포함 하는 일부 키보드 만든 제조업체 키보드를 통해 외부 기본적인 키보드 지원을 포함 합니다.

이제 iOS 9, 앱 자체 사용자 지정 키보드 바로 가기를 만들 수 있습니다. 또한 일부 기본 바로 가기 키 수와 같은 **명령 + C** (복사), **명령 X** (잘라내기), **명령-V** (붙여넣기) 및 **명령-Shift-H**  (홈)에 앱에 특별히 작성 된 응답을 하지 않고 있습니다.

**명령 탭** Mac OS와 마찬가지로 키보드에서 앱을 신속 하 게 전환할 수 있도록 하는 응용 프로그램 전환기를 표시 합니다.

[![](multitasking-images/keyboard01.png "응용 프로그램 전환기")](multitasking-images/keyboard01.png#lightbox)

IOS 9 앱 바로 가기 키를 포함 하는 경우를 누르고 있을 수 합니다 **명령**를 **옵션** 또는 **컨트롤** 팝업에 표시할 키:

[![](multitasking-images/keyboard02.png "바로 가기 키 팝업")](multitasking-images/keyboard02.png#lightbox)

#### <a name="defining-custom-keyboard-shortcuts"></a>사용자 지정 바로 가기 키를 정의합니다.

추가할 경우 다음 코드 보기 또는 뷰 컨트롤러를 앱에서 해당 뷰 또는 컨트롤러에 표시 되는 경우, 사용자 지정 바로 가기 키를 사용할 수 있습니다.

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

재정의 먼저 합니다 `CanBecomeFirstResponder` 속성과 반환 `true` 보기 또는 뷰 컨트롤러 키보드 입력을 받을 수 있도록 합니다. 

재정의 다음으로 `KeyCommands` 속성을 새로 만듭니다 `UIKeyCommand` 에 대 한 합니다 **명령-N** 키 입력 합니다. 호출 했으면 활성화 되 면 합니다 `NewEntry` 메서드 (사용 하 여 iOS 9를 노출 하는 `Export` 명령) 요청 된 작업을 수행 하 합니다.

사용자 형식과 연결 된 하드웨어 키보드를 사용 하 여이 앱 iPad에서 실행 하는 경우 **명령 N**에 새 항목이 목록에 추가 됩니다. 를 누르고 있는 경우는 **명령** 키, 바로 가기 목록이 표시 됩니다.

[![](multitasking-images/keyboard03.png "바로 가기 키 팝업")](multitasking-images/keyboard03.png#lightbox)

샘플을 참조 하세요 [MultiTask 앱](https://developer.xamarin.com/samples/monotouch/ios9/MultiTask/) 구현 예제입니다.

<a name="Resource-Management-Considerations" />

### <a name="resource-management-considerations"></a>리소스 관리 고려 사항

IOS 8의 디자인 지침 및 모범 사례를 이미 사용 중인 앱에 대해서도 리소스를 효과적으로 관리에 문제가 될 수 있습니다. 앱 더 이상 ios 9에서 메모리, CPU 또는 기타 시스템 리소스를 단독으로 사용 합니다.

결과적으로, 시스템 리소스를 효과적으로 사용 하 여 Xamarin.iOS 앱을 미세 조정 해야 합니다 또는 메모리 부족 상황에서 종료 직면 합니다. 멀티태스킹 옵트아웃 하는 앱도 마찬가지, 잠시 후 앱 계속 실행 될 수 슬라이드를 통해 패널 또는 초당 60 프레임 이하로 추가 리소스가 필요한 또는 새로 고침 빈도 일으키는 그림 창에서 그림에서.

다음과 같은 사용자 작업 및 해당 구현에 대해 고려해 야 합니다.

- **패널을 통해 슬라이드에 텍스트를 입력** -앱에 없는 텍스트 입력 하는 경우에 시스템 키보드를 이제 표시할 UI를 통해. 결과적으로, 앱 키보드 표시 알림 (예: 표시 및 숨기기 키보드)에 응답 해야 합니다.
- **슬라이드를 통해 패널에서 두 번째 앱을 실행** -새 앱을 이제 포그라운드에서 실행 하 고 메모리 및 CPU 주기 같은 시스템 리소스에 대 한 기존 앱을 사용 하 여 경쟁 합니다.
- **PIP 창에서 비디오 재생** -하지 비디오를 시작 하는 앱을 계속 백그라운드에서 실행 하 고 CPU 및 메모리 리소스를 소비 되지만이 창은 응용 프로그램의 인터페이스를의 부분을 덮을 수 있습니다.

을 보장 하기 위해 앱 리소스를 효율적으로 사용 되는 다음을 수행 해야 합니다.

- **계측을 사용 하 여 응용 프로그램 프로 파일링** -담아야 CPU 사용량과 있는 앱 수 수 주 스레드를 차단 하는 영역 메모리 누수를 확인 하십시오.
- **상태 전환 메서드 응답할** -프로그램 **AppDelegate.cs** 파일 재정 및 상태에 대 한 응답 입력 또는 백그라운드에서 반환 하는 앱과 같은 메서드를 변경 합니다. 이미지, 데이터 또는 뷰 및 뷰 컨트롤러와 같은 모든 필요 없는 자산을 해제 합니다.
- **나란히 메모리 집약적인 앱을 사용 하 여 테스트** -슬라이드 아웃와 실제 iOS 하드웨어에서 분할 뷰를 사용 하 여 메모리 집약적에서 앱을 맵과 같은 (위성 보기 모드)를 사용 하 여 앱을 실행 하 고 테스트 모두 앱 응답성을 유지 하 고 충돌 하지 않습니다.

Apple의를 참조 하세요 [iOS 앱에 대 한 에너지 효율성 가이드](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) 리소스 관리에 대 한 자세한 내용은 합니다.

<a name="Opting-Out-of-Multitasking" />

## <a name="opting-out-of-multitasking"></a>멀티태스킹 옵트아웃

모든 iOS 9 앱 멀티태스킹을 지 Apple에서 제안 하는 동안 있더라도 앱에 대 한 구체적인 이유가 하지도 제대로 작동 하려면 전체 화면을 필요로 하는 카메라 앱 또는 게임 등.

프로젝트의 편집 하거나 슬라이드 아웃 패널에서 또는 분할 보기 모드에서 실행 되 고 옵트아웃 하려면 Xamarin.iOS 앱에 대해 **Info.plist** 파일을 체크 **전체 화면 필요**:

[![](multitasking-images/fullscreen01.png "멀티태스킹 옵트아웃")](multitasking-images/fullscreen01.png#lightbox)

> [!IMPORTANT]
> 앱 슬라이드 아웃 또는 분할 뷰에서 실행 되지 않도록 방지 멀티태스킹 옵트아웃을 하는 동안 해당 해도 다른 앱에서 앱과 함께 표시에서 슬라이드 아웃 또는 비디오 그림과에서 그림에서 실행 되 고 있습니다.

<a name="Disabling-PIP-Video-Playback" />

### <a name="disabling-pip-video-playback"></a>PIP 비디오 재생을 사용 하지 않도록 설정

대부분의 경우 앱 사용자가 사진 떠 창에 있는 그림에 표시 될 비디오 콘텐츠를 재생 하도록 허용 해야 합니다. 그러나 여기서이 될 수 있습니다 수 적절 하지 않으면 게임 잘라내기 장면 비디오와 같은 경우가 있을 수 있습니다.

PIP 비디오 재생을 옵트아웃 하려면 앱에서 다음을 수행 합니다.

 - 사용 중인 경우는 `AVPlayerViewController` 비디오를 표시 하려면 설정 합니다 `AllowsPictureInPicturePlayback` 속성을 `false`입니다.
 - 사용 중인 경우는 `AVPlayerLayer` 비디오를 표시 하려면 인스턴스화하지 마세요는 `AVPictureInPictureController`합니다.
 - 사용 중인 경우는 `WKWebView` 비디오를 표시 하려면 설정 합니다 `AllowsPictureInPictureMediaPlayback` 속성을 `false`입니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.iOS 앱을 실행 되 고 Ipad 용 iOS 9의 새로운 멀티태스킹 기능에서 올바르게 작동 하는 데 필요한 단계를 설명 했습니다. 또한 없는 적합 하는 앱 용 멀티태스킹의 항목을 선택 하면 스케일 아웃을 설명 합니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [멀티태스킹을 (샘플)](https://developer.xamarin.com/samples/monotouch/ios9/MultiTask/)
- [통합된 Storyboards 소개](~/ios/user-interface/storyboards/unified-storyboards.md)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [IPad에서 멀티태스킹 향상 된 기능을 채택](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145)
- [블로그 게시물](https://blog.xamarin.com/using-auto-layouts-for-ios-9-splitview/)
