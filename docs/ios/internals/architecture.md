---
title: iOS 앱 아키텍처
description: 이 문서에서는 AOT 컴파일, 선택기, 기관, 응용 프로그램 시작 및 생성기는 낮은 수준의 설명 어떻게 네이티브와 관리 코드 상호 작용에서 Xamarin.iOS를 설명 합니다.
ms.prod: xamarin
ms.assetid: F40F2275-17DA-4B4D-9678-618FF25C6803
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 426c5ef5cc32877546ebb88cb485a81723816e6e
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67675061"
---
# <a name="ios-app-architecture"></a>iOS 앱 아키텍처

Xamarin.iOS 응용 프로그램 Mono 실행 환경 내에서 실행 하 고 전체 계속 해 서의 시간 () aot 컴파일하는 데 사용할 C# ARM 어셈블리 언어로 코드입니다. Side-by-side-실행이 사용 하 여 합니다 [Objective-c 런타임에서](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/)합니다. 두 런타임 환경 특히 UNIX 유사 커널을 기반으로 실행 [XNU](https://en.wikipedia.org/wiki/XNU), 기본 네이티브 또는 관리 되는 시스템에 액세스 하려면 개발자가 사용자 코드에 다양 한 Api를 노출 합니다.

아래 다이어그램은이 아키텍처의 기본적인 개요를 보여 줍니다.

[![](architecture-images/ios-arch-small.png "이 다이어그램은 계속 해 서의 시간 (AOT) 컴파일 아키텍처의 기본적인 개요를 보여 줍니다.")](architecture-images/ios-arch.png#lightbox)

## <a name="native-and-managed-code-an-explanation"></a>네이티브 및 관리 되는 코드의 경우: 자세한 내용

Xamarin 용 개발 하는 경우 용어 *네이티브 및 관리* 코드는 종종 사용 됩니다. [관리 코드](https://blogs.msdn.microsoft.com/brada/2004/01/09/what-is-managed-code/) 에서 관리 하는 실행 된 코드를 [.NET Framework 공용 언어 런타임](https://msdn.microsoft.com/library/8bs2ecf4(v=vs.110).aspx), 또는 Xamarin의 경우: Mono 런타임입니다. 중간 언어 라는 것입니다.

네이티브 코드는 고유 하 게 (예를 들어, Objective-c 또는 ARM 칩에도 AOT 컴파일되어야 코드)는 특정 플랫폼에서 실행 되는 코드입니다. 이 가이드 AOT 네이티브 코드로 관리 되는 코드를 컴파일하는 방법을 설명 하 고 전체 활용 바인딩 사용 하 여 Apple iOS Api 원하신다면에 대 한 액세스는 Xamarin.iOS 응용 프로그램 작동 하는 방법 설명 합니다. 와 같은 복잡 한 언어 및 NET의 BCL C#입니다.

## <a name="aot"></a>AOT

모든 Xamarin 플랫폼 응용 프로그램의 Mono를 컴파일할 때 C# (또는 F#) 컴파일러를 실행 하 고 컴파일되지에 C# 및 F# 코드를 중간 언어 (MSIL (Microsoft). 시뮬레이터에서 Xamarin.Android, Xamarin.Mac 응용 프로그램 또는 Xamarin.iOS 응용 프로그램 에서도 실행 하는 경우는 [.NET 공용 언어 런타임 (CLR)](https://msdn.microsoft.com/library/8bs2ecf4(v=vs.110).aspx) JIT (Time) 컴파일러에서는 Just를 사용 하 여 MSIL을 컴파일합니다. 네이티브 코드로 컴파일된이 런타임 시 응용 프로그램에 대 한 올바른 아키텍처에서 실행할 수입니다.

그러나 ios에서 Apple 장치에서 동적으로 생성 된 코드의 실행을 허용 하지 않는 설정한 보안 제한이 있습니다.
이러한 보안 프로토콜 준수를 위해, Xamarin.iOS 관리 되는 코드를 컴파일하려면 앞의 시간 (AOT) 컴파일러를 대신 사용 합니다. 필요에 따라 액세스에 최적화 된 LLVM을 사용 하 여 Apple의 ARM 기반 프로세서에서 배포할 수 있는 장치에 대 한 네이티브 iOS 이진을 생성 합니다. 아래 함께 맞추는 방법의 대략적인 다이어그램을 보여 줍니다.

[![](architecture-images/aot.png "함께 맞추는 방법의 대략적인 다이어그램")](architecture-images/aot-large.png#lightbox)

자세히 설명 하는 제한 개수가 AOT를 사용 하는 [제한 사항](~/ios/internals/limitations.md) 가이드입니다. 또한 수많은 개선 사항이 JIT 조치를 통해 제공 시작 시간 및 다양 한 성능 최적화의 감소

네이티브 코드에 코드를 소스에서 컴파일된 방법을 탐색 했습니다 했으므로 살펴보겠습니다 Xamarin.iOS 완전 한 네이티브 iOS 응용 프로그램을 작성 하기을 허용 하는 방법을 보려면 살펴보기

## <a name="selectors"></a>선택기

Xamarin을 사용 하 여 두 개의 별도 에코 시스템,.NET 및 Apple 상태로 전환 해야 하는 것으로 원활한 사용자 환경을 최종 목표는 되도록 가능한 한 간소화 된 것을 함께 합니다. 런타임과 통신 하는 방법을 위의 섹션에서 살펴본 및 매우 잘 들어보았을 네이티브 iOS Api Xamarin에서 사용할 수 있는 용어 '바인딩'. 바인딩에서 자세히 설명 되어 우리의 [Objective-c 바인딩](~/cross-platform/macios/binding/overview.md) 설명서 있으므로 지금은 살펴보겠습니다 iOS 내부에서 작동 하는 방식입니다.

Objective-c를 노출 하는 방법에 먼저 C#에 있는 선택기를 통해 수행 됩니다. 선택기에는 개체 또는 클래스에 전송 된 메시지입니다. Objective C를 사용 하 여 이렇게 통해 합니다 [objc_msgSend](~/cross-platform/macios/binding/overview.md) 함수입니다.
선택기를 사용 하 여에 대 한 자세한 내용은 참조는 [Objective-c 선택기](~/ios/internals/objective-c-selectors.md) 가이드입니다. 또한 더 때문에 Objective-c로 관리 코드에 대해서 알지 못하기 더 복잡 한는 Objective-c로 관리 되는 코드를 노출 하는 방법입니다. 이 문제를 해결 하려면 사용 *등록자*합니다. 이러한 작업은 다음 섹션에서 자세히 설명 되어 있습니다.

## <a name="registrars"></a>등록 기관

등록 기관은 목표 C에 노출 관리 되는 코드를 코드는 위에서 설명한 대로 NSObject에서 파생 되는 모든 관리 되는 클래스의 목록을 만들어 수행 합니다.

- 기존 Objective-c 클래스를 래핑하는 모든 클래스에 있는 모든 관리 되는 멤버를 미러링 하는 Objective-c로 멤버를 사용 하 여 새 Objective C 클래스 만들고는 [`Export`] 특성입니다.

- 각 – Objective-c 멤버에 대 한 구현에서 코드는 미러된 관리 되는 멤버를 호출에 자동으로 추가 됩니다.

아래의 의사 (pseudo) 코드는이 수행 되는 방법의 예를 보여줍니다.

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

관리 코드에 특성을 포함할 수 있습니다 `[Register]` 고 `[Export]`, 등록자를 사용 하 여 개체를 보여주기 위한 것입니다.에 노출 해야 한다고 알고
`[Register]` 특성 생성 하는 기본 이름은 적합 한 경우 생성된 된 Objective-c 클래스의 이름을 지정 하는 데 사용 됩니다. NSObject에서 파생 된 모든 클래스가 자동으로 등록 된 보여주기 위한 것입니다.
필수 `[Export]` 선택기 Objective-c로 생성 된 클래스에서 사용 되는 문자열을 포함 하는 특성입니다.

Xamarin.iOS – 동적 및 정적에 사용 되는 등록자의는 다음과 같은 두 종류가 있습니다.


- **동적 등록 기관** – 동적 등록 기관에서는 런타임에 어셈블리에 있는 모든 유형의 등록 합니다. 제공 하는 함수를 사용 하 여 이렇게 [Objective-c의 런타임 API](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/)합니다. 따라서 동적 등록 기관은 느린 신생 기업, 빠르고 있지만 빌드 시간을 포함 됩니다. IOS 시뮬레이터에 대 한 기본값입니다. 동적 등록 기관을 사용 하는 경우 trampolines, 호출 (C)에서 일반적으로 네이티브 함수 메서드 구현으로 사용 됩니다. 이러한 다양 한 아키텍처 마다 다릅니다.

- **정적 등록 기관** – 정적 등록 기관에는 정적 라이브러리에 컴파일되고 실행 파일에 연결 하는 빌드 중 Objective-c 코드를 생성 합니다. 이 빠른 시작을 허용 하지만 빌드 시 길어집니다. 장치 빌드에 대 한 기본적으로 사용 됩니다. 정적 등록 기관에는 iOS 시뮬레이터를 사용 하 여 전달 하 여 `--registrar:static` 으로 `mtouch` 아래와 같이 프로젝트의 빌드 옵션에서 특성:

    [![](architecture-images/image1.png "추가 mtouch 인수를 설정합니다.")](architecture-images/image1.png#lightbox)

IOS Xamarin.iOS에서 사용 하는 형식 등록 시스템의 세부 사항에 자세한 내용은 참조는 [형식 등록자](~/ios/internals/registrar.md) 가이드입니다.

## <a name="application-launch"></a>응용 프로그램 시작

호출 된 함수에서 제공 하는 모든 Xamarin.iOS 실행 파일의 진입점 `xamarin_main`는 mono를 초기화 합니다.

프로젝트 형식에 따라 다음 작업 수행 됩니다.

- 일반 iOS 및 tvOS 응용 프로그램에 대 한 Xamarin 앱에서 제공한 관리 되는 Main 메서드에서 호출 됩니다. Main 메서드에서 관리 되는이 호출 `UIApplication.Main`, 목표 C에 대 한 진입점에는 UIApplication.Main는 Objective-c의에 대 한 바인딩을 `UIApplicationMain` 메서드.
- 확장의 경우 네이티브 함수 – `NSExtensionMain` 또는 (`NSExtensionmain` WatchOS 확장에 대 한) – 라이브러리 라고 Apple에서 제공 합니다. 이러한 프로젝트에 클래스 라이브러리와 프로젝트를 실행 하지 되므로 실행 하려면 관리 되는 Main 메서드가 없습니다 있습니다.

이 시작 시퀀스의 모든 앱에서 시작 하는 방법을 알 수 있도록 다음 최종 실행 파일에 연결 되는 정적 라이브러리로 컴파일됩니다.

이 시점에서 앱에 시작, Mono에서 실행 되 고, 관리 코드에서 및에서는 네이티브 코드를 호출 하 고 다시 호출할 방법을 알 수 있습니다. 다음으로 할 실제로 컨트롤을 추가 하 고 앱을 대화형 확인 하는 것입니다.


## <a name="generator"></a>Generator

Xamarin.iOS는 모든 단일 iOS API에 대 한 정의 포함합니다. 탐색할 수 있습니다 이러한 합니다 [MaciOS github 리포지토리](https://github.com/xamarin/xamarin-macios/tree/master/src)합니다. 이러한 정의 특성을 가진 인터페이스 뿐만 아니라 모든 필요한 메서드 및 속성을 포함 합니다. 다음 코드는 UIKit의는 UIToolbar를 정의 하는 예를 들어 [네임 스페이스](https://github.com/xamarin/xamarin-macios/blob/master/src/uikit.cs#L11277-L11327)합니다. 다양 한 메서드 및 속성을 사용 하 여 인터페이스 인지 확인 합니다.

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

호출 된 생성기 [ `btouch` ](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs) Xamarin.iOS에는 이러한 정의 파일 및.NET 도구를 사용 하 여 [임시 어셈블리로 컴파일할 수](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs#L318)합니다. 그러나이 임시 어셈블리 Objective-c 코드를 호출 하는 사용 가능한 아닙니다. 생성기 다음 임시 어셈블리를 읽고 생성 C# 런타임에 사용할 수 있는 코드입니다.
따라서 예를 들어, 임의 특성을 정의.cs 파일에 추가 하는 경우이 표시 되지 않습니다 출력 코드에서입니다. 생성기가 알지 있어 `btouch` 출력 임시 어셈블리를 찾는 위치를 알 수 없습니다.

Xamarin.iOS.dll 만들어지면 mtouch는 모든 구성 요소를 함께 번들 됩니다.

높은 수준에서이 작업을 위해 다음 작업을 실행 하 여:

-   앱 번들 구조를 만듭니다.
-   관리 되는 어셈블리에서 복사 합니다.
-   연결을 사용 하는 경우 초과 사용 하지 않는 부분을 복사 하 여 어셈블리를 최적화 하기 위해 관리 되는 링커를 실행 합니다.
-   AOT 컴파일입니다.
-   네이티브 실행 파일 실행 프로그램 코드, 등록자 코드 (정적) 경우 및는 AOT의 모든 출력을 구성 하는 일련의 네이티브 실행 파일에 연결 되어 있는 (각 어셈블리에 대해 하나) 정적 라이브러리를 출력 하는 네이티브 실행 파일 만들기 컴파일러


자세한 링커 및 사용 방법에 대 한 정보를 참조 합니다 [링커](~/ios/deploy-test/linker.md) 가이드입니다.

## <a name="summary"></a>요약

이 가이드는 Xamarin.iOS 앱 탐색된 Xamarin.iOS 및 Objective-c 방어에서 해당 관계의 AOT 컴파일 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [제한 사항](~/ios/internals/limitations.md)
- [Objective-C 바인딩](~/cross-platform/macios/binding/overview.md)
- [Objective-c 선택기](~/ios/internals/objective-c-selectors.md)
- [형식 등록](~/ios/internals/registrar.md)
- [링커](~/ios/deploy-test/linker.md)
