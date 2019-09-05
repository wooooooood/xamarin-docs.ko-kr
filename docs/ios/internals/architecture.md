---
title: iOS 앱 아키텍처
description: 이 문서는 낮은 수준의 Xamarin.ios에 대해 설명 하 고, 네이티브 코드와 관리 코드가 상호 작용 하는 방법, AOT 컴파일, 선택기, 등록 기관, 응용 프로그램 시작 및 생성기를 설명 합니다.
ms.prod: xamarin
ms.assetid: F40F2275-17DA-4B4D-9678-618FF25C6803
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 7d59a295961c25ecfcc99bb54fdc188c957cf3ee
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291947"
---
# <a name="ios-app-architecture"></a>iOS 앱 아키텍처

Xamarin.ios 응용 프로그램은 Mono 실행 환경에서 실행 되며 AOT (전체 시간) 컴파일을 사용 하 여 코드를 ARM 어셈블리 C# 언어로 컴파일합니다. 이는 [목표-C 런타임과](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/)side-by-side 방식으로 실행 됩니다. 두 런타임 환경은 모두 UNIX와 유사한 커널, 특히 [Xnu](https://en.wikipedia.org/wiki/XNU)에서 실행 되며 개발자가 기본 네이티브 또는 관리 되는 시스템에 액세스할 수 있도록 하는 다양 한 api를 사용자 코드에 노출 합니다.

아래 다이어그램은이 아키텍처의 기본 개요를 보여 줍니다.

[![](architecture-images/ios-arch-small.png "이 다이어그램에서는 AOT (사전) 컴파일 아키텍처의 기본 개요를 보여 줍니다.")](architecture-images/ios-arch.png#lightbox)

## <a name="native-and-managed-code-an-explanation"></a>네이티브 및 관리 코드: 설명

Xamarin 용으로 개발 하는 경우 *네이티브 코드와 관리* 코드를 사용 하는 경우가 많습니다. [관리 코드](https://blogs.msdn.microsoft.com/brada/2004/01/09/what-is-managed-code/) 는 [.NET Framework 공용 언어 런타임에서](https://msdn.microsoft.com/library/8bs2ecf4(v=vs.110).aspx)실행 되는 코드 또는 Xamarin의 경우 Mono 런타임입니다. 중간 언어 라고 하는 것입니다.

네이티브 코드는 특정 플랫폼에서 기본적으로 실행 되는 코드입니다 (예: AOT 또는 ARM 칩에서 컴파일된 코드). 이 가이드에서는 AOT이 관리 코드를 네이티브 코드로 컴파일하는 방법에 대해 설명 하 고, Xamarin.ios 응용 프로그램이 작동 하는 방식에 대해 설명 하 고 바인딩을 사용 하 여 Apple iOS Api를 완전히 활용 하는 동시에에도 액세스할 수 있도록 합니다. 순의 BCL 및와 C#같은 고급 언어입니다.

## <a name="aot"></a>AOT

Xamarin 플랫폼 C# 응용 프로그램을 컴파일할 때 Mono (또는 F#) 컴파일러는를 실행 하 고 C# 및 F# 코드를 MSIL (Microsoft 중간 언어)로 컴파일합니다. Xamarin Android, Xamarin.ios 응용 프로그램 또는 시뮬레이터에서 Xamarin.ios 응용 프로그램을 실행 하는 경우 [.NET CLR (공용 언어 런타임)](https://msdn.microsoft.com/library/8bs2ecf4(v=vs.110).aspx) 은 JIT (just-in-time) 컴파일러를 사용 하 여 MSIL을 컴파일합니다. 런타임에는 응용 프로그램에 대 한 올바른 아키텍처에서 실행 될 수 있는 네이티브 코드로 컴파일됩니다.

그러나 Apple에 의해 설정 된 iOS에는 장치에서 동적으로 생성 된 코드를 실행할 수 없도록 하는 보안 제한 사항이 있습니다.
이러한 안전 프로토콜을 준수 하도록 하기 위해 Xamarin.ios는 대신 AOT (사전) 컴파일러를 사용 하 여 관리 코드를 컴파일합니다. 이렇게 하면 Apple의 ARM 기반 프로세서에 배포할 수 있는 기본 iOS 이진 파일이 생성 되 고, 선택적으로 LLVM을 사용 하 여 장치에 최적화 됩니다. 이에 대 한 대략적인 다이어그램은 아래에 나와 있습니다.

[![](architecture-images/aot.png "이러한 방식이 함께 작동 하는 방식을 대략적으로 보여 주는 다이어그램")](architecture-images/aot-large.png#lightbox)

AOT를 사용 하는 [경우 제한 사항 가이드에](~/ios/internals/limitations.md) 자세히 설명 된 여러 가지 제한 사항이 있습니다. 또한 시작 시간을 줄이고 다양 한 성능 최적화를 통해 JIT에 비해 많은 향상 된 기능을 제공 합니다.

이제 소스 코드에서 네이티브 코드로 코드를 컴파일하는 방법에 대해 살펴보았습니다. 이제 Xamarin.ios를 사용 하 여 완전 한 네이티브 iOS 응용 프로그램을 작성 하는 방법을 살펴보겠습니다.

## <a name="selectors"></a>선택기

Xamarin을 사용 하는 경우 두 개의 개별 에코 시스템 .NET 및 Apple이 있습니다 .이를 통해 최대한 간소화 된 것으로 표시 하 여 최종 목표가 원활한 사용자 환경을 보장 해야 합니다. 위의 섹션에는 두 런타임이 통신 하는 방법에 대 한 내용이 나와 있으며, Xamarin에서 네이티브 iOS Api를 사용할 수 있도록 하는 용어 ' 바인딩 '을 매우 잘 들었습니다. 바인딩은 [목표-C 바인딩](~/cross-platform/macios/binding/overview.md) 설명서에 자세히 설명 되어 있으므로 이제 iOS가 내부적으로 작동 하는 방식을 살펴보겠습니다.

첫째, 선택기를 통해 수행 되는 목표-C를에 C#노출 하는 방법이 있어야 합니다. 선택기는 개체 또는 클래스로 전송 되는 메시지입니다. [Objc_msgSend](~/cross-platform/macios/binding/overview.md) 함수를 통해이 작업을 수행할 수 있습니다.
선택기 사용에 대 한 자세한 내용은 [목표-C 선택기](~/ios/internals/objective-c-selectors.md) 가이드를 참조 하세요. 또한 관리 코드를 목표 c에 노출 하는 방법이 있어야 합니다 .이는 목표-C가 관리 코드에 대 한 정보를 알 수 없기 때문에 더 복잡 합니다. 이 문제를 해결 하려면 *등록 기관*를 사용 합니다. 이러한 내용은 다음 섹션에서 자세히 설명 합니다.

## <a name="registrars"></a>등록 기관

위에서 언급 했 듯이 등록자는 관리 코드를 목표 C에 노출 하는 코드입니다. NSObject에서 파생 되는 모든 관리 되는 클래스 목록을 만들어이 작업을 수행 합니다.

- 기존 목표-c 클래스를 래핑하는 모든 클래스의 경우에는 [`Export`] 특성을 포함 하는 모든 관리 되는 멤버를 대상으로 하는 모든 관리 되는 멤버를 미러링 하는 새 목표-c 클래스를 만듭니다.

- 각 목표 – C 멤버에 대 한 구현에서는 미러된 관리 되는 멤버를 호출 하기 위해 코드가 자동으로 추가 됩니다.

아래 의사 코드는 이러한 작업을 수행 하는 방법의 예를 보여 줍니다.

**C#(관리 코드)**

```csharp
 class MyViewController : UIViewController{
     [Export ("myFunc")]
     public void MyFunc ()
     {
     }
 }
```

**Objective-C:**

```objectivec
@interface MyViewController : UIViewController { }

    -(void)myFunc;
@end

@implementation MyViewController {}

    -(void) myFunc
    {
        /* code to call the managed MyViewController.MyFunc method */
    }
@end

```

관리 코드에는를 사용 하 여 `[Register]` 개체 `[Export]`를 목표에 노출 해야 한다는 것을 알 수 있도록 등록 기관에서 사용 하는 특성 및이 포함 될 수 있습니다.
`[Register]` 특성은 생성 된 기본 이름이 적절 하지 않은 경우 생성 된 목표-C 클래스의 이름을 지정 하는 데 사용 됩니다. NSObject에서 파생 된 모든 클래스는 목표-C에 자동으로 등록 됩니다.
Required `[Export]` 특성에는 생성 된 목표-C 클래스에서 사용 되는 선택기 인 문자열이 포함 됩니다.

Xamarin.ios에서 사용 되는 등록 기관에는 동적 및 정적 이라는 두 가지 유형이 있습니다.


- **동적 등록 기관** – 동적 등록자는 런타임에 어셈블리의 모든 형식에 대해 등록을 수행 합니다. 이는 [목표-C의 런타임 API](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/)에서 제공 하는 함수를 사용 하 여 수행 합니다. 따라서 동적 등록자는 시작 속도가 느리고 빌드 시간을 단축 합니다. 이는 iOS 시뮬레이터에 대 한 기본값입니다. Trampolines 라고 하는 네이티브 함수 (일반적으로 C)는 동적 등록 기관를 사용할 때 메서드 구현으로 사용 됩니다. 아키텍처는 서로 다릅니다.

- **정적 등록 기관** – 정적 등록자는 빌드 중에 목표 C 코드를 생성 합니다 .이 코드는 정적 라이브러리로 컴파일되고 실행 파일에 연결 됩니다. 이렇게 하면 더 빨리 시작할 수 있지만 빌드 시간 동안 시간이 오래 걸립니다. 이는 기본적으로 장치 빌드에 사용 됩니다. 정적 등록자는 다음과 같이 프로젝트의 빌드 옵션에서 `--registrar:static` `mtouch` 특성으로 전달 하 여 iOS 시뮬레이터에서 사용할 수도 있습니다.

    [![](architecture-images/image1.png "추가 mtouch 인수 설정")](architecture-images/image1.png#lightbox)

Xamarin.ios에서 사용 하는 iOS 유형 등록 시스템의 세부 정보에 대 한 자세한 내용은 [등록자 유형](~/ios/internals/registrar.md) 가이드를 참조 하세요.

## <a name="application-launch"></a>응용 프로그램 시작

모든 xamarin.ios 실행 파일의 진입점은 mono를 초기화 하는 라는 `xamarin_main`함수에 의해 제공 됩니다.

프로젝트 형식에 따라 다음이 수행 됩니다.

- 일반 iOS 및 tvOS 응용 프로그램의 경우 Xamarin 앱에서 제공 하는 관리 되는 Main 메서드를 호출 합니다. 이 관리 되는 Main 메서드 `UIApplication.Main`는 목표-C의 진입점인를 호출 합니다. Uiapplication. Main은 목표-C의 `UIApplicationMain` 메서드에 대 한 바인딩입니다.
- 확장의 경우 Apple 라이브러리에서 제공 `NSExtensionMain` 하는`NSExtensionmain` 네이티브 함수 (WatchOS 확장의 경우)가 호출 됩니다. 이러한 프로젝트는 실행 프로젝트가 아닌 클래스 라이브러리 이기 때문에 실행할 관리 되는 기본 메서드가 없습니다.

이 시작 시퀀스는 모두 정적 라이브러리로 컴파일되며 최종 실행 파일에 연결 됩니다. 그러면 앱에서 그라운드를 끄는 방법을 알 수 있습니다.

앱이 시작 되 고 Mono가 실행 되 고 있으며,이 시점에서 관리 코드를 실행 하 고 네이티브 코드를 호출 하 고 다시 호출 하는 방법을 알고 있습니다. 다음으로 수행 해야 하는 작업은 실제로 컨트롤 추가를 시작 하 고 앱을 대화형으로 만드는 것입니다.


## <a name="generator"></a>Generator

Xamarin.ios에는 모든 단일 iOS API에 대 한 정의가 포함 되어 있습니다. [Macios github](https://github.com/xamarin/xamarin-macios/tree/master/src)리포지토리에서 이러한 중 하나를 탐색할 수 있습니다. 이러한 정의에는 특성이 포함 된 인터페이스 뿐만 아니라 필요한 메서드 및 속성도 포함 됩니다. 예를 들어 다음 코드는 UIKit [네임 스페이스](https://github.com/xamarin/xamarin-macios/blob/master/src/uikit.cs#L11277-L11327)에서 UIToolbar를 정의 하는 데 사용 됩니다. 이 인터페이스는 여러 메서드 및 속성을 포함 하는 인터페이스입니다.

```csharp
[BaseType (typeof (UIView))]
public interface UIToolbar : UIBarPositioning {
    [Export ("initWithFrame:")]
    IntPtr Constructor (CGRect frame);

    [Export ("barStyle")]
    UIBarStyle BarStyle { get; set; }

    [Export ("items", ArgumentSemantic.Copy)][NullAllowed]
    UIBarButtonItem [] Items { get; set; }

    [Export ("translucent", ArgumentSemantic.Assign)]
    bool Translucent { [Bind ("isTranslucent")] get; set; }

    // done manually so we can keep this "in sync" with 'Items' property
    //[Export ("setItems:animated:")][PostGet ("Items")]
    //void SetItems (UIBarButtonItem [] items, bool animated);

    [Since (5,0)]
    [Export ("setBackgroundImage:forToolbarPosition:barMetrics:")]
    [Appearance]
    void SetBackgroundImage ([NullAllowed] UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);

    [Since (5,0)]
    [Export ("backgroundImageForToolbarPosition:barMetrics:")]
    [Appearance]
    UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics);

    ...
}
```

Xamarin.ios에서 호출 [`btouch`](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs) 되는 생성기는 이러한 정의 파일을 사용 하 고 .net 도구를 사용 하 여 [임시 어셈블리로 컴파일합니다](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs#L318). 그러나이 임시 어셈블리는 목표-C 코드를 호출할 수 없습니다. 그러면 생성기에서 임시 어셈블리를 읽고 런타임에 사용할 C# 수 있는 코드를 생성 합니다.
예를 들어, 사용자가 정의 .cs 파일에 임의 특성을 추가 하면 출력 코드에 표시 되지 않습니다. 생성기는이 `btouch` 에 대해 알지 않으므로 임시 어셈블리에서이를 검색 하 여 출력을 찾을 수 없습니다.

Xamarin.ios를 만든 후에는 mtouch에서 모든 구성 요소를 함께 번들로 만듭니다.

개략적인 수준에서 다음 작업을 실행 하 여이 작업을 수행 합니다.

- 앱 번들 구조를 만듭니다.
- 관리 되는 어셈블리를 복사 합니다.
- 링크를 사용 하는 경우 관리 되는 링커를 실행 하 여 사용 되지 않은 부분을 복사 하 여 어셈블리를 최적화 합니다.
- AOT 컴파일
- 네이티브 실행 파일에 연결 된 일련의 정적 라이브러리 (각 어셈블리에 대해 하나씩)를 출력 하는 네이티브 실행 파일을 만듭니다. 기본 실행 파일은 시작 관리자 코드, 등록자 코드 (정적) 및 AOT의 모든 출력으로 구성 됩니다. 컴파일러나


링커에 대 한 자세한 내용 및 사용 방법에 대 한 자세한 내용은 [링커](~/ios/deploy-test/linker.md) 가이드를 참조 하세요.

## <a name="summary"></a>요약

이 가이드에서는 Xamarin.ios 앱을 AOT 하 고 Xamarin.ios와 목적과 관련 된 관계를 자세히 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [제한 사항](~/ios/internals/limitations.md)
- [Objective-C 바인딩](~/cross-platform/macios/binding/overview.md)
- [목표-C 선택기](~/ios/internals/objective-c-selectors.md)
- [등록자 형식](~/ios/internals/registrar.md)
- [링커](~/ios/deploy-test/linker.md)
