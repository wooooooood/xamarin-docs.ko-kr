---
title: "iOS 아키텍처"
description: "Xamarin.iOS 낮은 수준에서 탐색"
ms.topic: article
ms.prod: xamarin
ms.assetid: F40F2275-17DA-4B4D-9678-618FF25C6803
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: bf9d292acf43bbbe3e4ba76b5a264a11288b7225
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="ios-architecture"></a>iOS 아키텍처

Xamarin.iOS 응용 프로그램 모노 실행 환경 내에서 실행 하 고 C# 코드를 ARM 어셈블리 언어를 컴파일하는 데 전체 앞의 시간 (AOT) 컴파일을 사용. 그러면-병렬 실행으로 [Objective C 런타임](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/)합니다. 특히 두 런타임 환경 UNIX 방식 커널 위에서 실행 [XNU](https://en.wikipedia.org/wiki/XNU), 때문에 개발자가 기본 네이티브 또는 관리 되는 시스템에 액세스 하려면 사용자 코드에 다양 한 Api를 노출 합니다.

다음 다이어그램에서는이 아키텍처의 기본적인 개요를 보여 줍니다.

[ ![](architecture-images/ios-arch-small.png "이 다이어그램은 앞의 시간 (AOT) 컴파일 아키텍처의 기본적인 개요를 보여 줍니다.")](architecture-images/ios-arch.png)

## <a name="native-and-managed-code-an-explanation"></a>네이티브 및 관리 코드: An 설명

Xamarin에 대 한 개발 하는 경우 약관 *네이티브 및 관리* 코드는 종종 사용 됩니다. [관리 코드](https://blogs.msdn.microsoft.com/brada/2004/01/09/what-is-managed-code/) 는에서는에서 관리 하는 실행 하는 코드는 [.NET Framework 공용 언어 런타임](https://msdn.microsoft.com/en-us/library/8bs2ecf4(v=vs.110).aspx), 또는 Xamarin의 경우: 모노 런타임. 중간 언어 주기입니다.

네이티브 코드는 코드입니다 (예를 들어 Objective-c 또는 ARM 칩의 AOT 컴파일된 코드)는 특정 플랫폼에서 기본적으로 실행 됩니다. 이 가이드 AOT 네이티브 코드로 관리 되는 코드를 컴파일하는 방법을 설명 하 고 사용 하는 것 바인딩 사용 하 여 Apple iOS Api도 동시에 대 한 액세스 Xamarin.iOS 응용 프로그램의 작동 방법을 설명 합니다. NET의 BCL 및 C#과 같은 복잡 한 언어입니다.


## <a name="aot"></a>AOT

Xamarin 플랫폼 응용 프로그램을 컴파일할 때 모노 C# (또는 F #) 컴파일러 실행 되 고 C# 및 F # 코드에 언어 MSIL (Microsoft Intermediate) 컴파일할 됩니다. 시뮬레이터에서 Xamarin.Android는 Xamarin.Mac 응용 프로그램 또는 Xamarin.iOS 응용 프로그램 에서도 실행 하는 경우는 [.NET 공용 언어 런타임 (CLR)](https://msdn.microsoft.com/en-us/library/8bs2ecf4(v=vs.110).aspx) JIT (Time) 컴파일러에는 마법사를 사용 하 여 MSIL을 컴파일합니다. 런타임에이 네이티브 코드로 컴파일된 응용 프로그램에 대 한 올바른 아키텍처에 실행할 수 있는 합니다.

그러나 ios에서 Apple 장치에서 동적으로 생성 된 코드의 실행을 허용 하지 않는 설정한 보안 제한이 있습니다.
이러한 보안 프로토콜을 준수 것을 보장 하려면 Xamarin.iOS 관리 되는 코드를 컴파일하려면 앞의 시간 (AOT) 컴파일러 대신 사용 합니다. Apple의 ARM 기반 프로세서에 배포할 수 있는 장치에 대 한 원하는 LLVM로 최적화 필요에 따라 기본 iOS 이진을 생성 합니다. 아래에 함께 맞추는 방법 대 한 대략적인 다이어그램 보여 줍니다.

[ ![](architecture-images/aot.png "이 적용 되는 방식 함께의 대략적인 다이어그램")](architecture-images/aot-large.png)

에 자세히 설명 되어 있는 제한의 개수가 AOT를 사용 하는 [제한](~/ios/internals/limitations.md) 가이드입니다. 또한 다양 한 기능이 향상 수 있게 JIT 시작 시간 및 다양 한 성능 최적화의 감소를 통해

네이티브 코드에 코드 원본의 컴파일 방식을 이해할 म 했으므로 살펴보도록 하죠 Xamarin.iOS 허용 하 완전 한 네이티브 iOS 응용 프로그램을 작성 하는 방법을 볼 수는 내부적

## <a name="selectors"></a>선택기

두 개의 별도 에코 시스템,.NET 및 Apple 상태로 전환 해야 하는 xamarin을 하기 위해 함께 최종 목표 원활한 사용자 환경을 지 확인 하려면 가능한 같이 간소화 된 것 같습니다. 두 개의 런타임 통신 하는 방법 위의 섹션에서 살펴본 것 및 상태일 때 잘 들어보았을 것에서 네이티브 iOS Api Xamarin에서 사용할 수 있는 기간 '바인딩은'. 바인딩은에서 자세히 설명 되어 우리의 [Objective-c 바인딩](~/cross-platform/macios/binding/overview.md) 설명서, 있으므로 지금은 살펴보겠습니다 iOS 내부에서 작동 하는 방식입니다.

첫째, Objective-c 선택기를 통해 수행 된 C#을 노출 하는 방법에 있습니다. 선택 기가 개체 또는 클래스에 전송 된 메시지입니다. Objective C 사용을 통해 수행 됩니다이 [objc_msgSend](~/cross-platform/macios/binding/overview.md) 함수입니다.
선택기를 사용 하 여에 대 한 자세한 내용은 참조는 [Objective-c 선택기](~/ios/internals/objective-c-selectors.md) 가이드입니다. 발생에 때문을 관리 코드에 대해서 Objective-c 알지 못하기 더 복잡 한 변수인 Objective C로 관리 되는 코드를 노출 하는 방법에 있습니다. 이 해결 하려면 사용 *등록자*합니다. 이러한 작업은 다음 섹션에서 자세히 설명 되어 있습니다.

## <a name="registrars"></a>등록 기관

등록자는 Objective c 노출 관리 되는 코드를 코드는 위에서 설명한 대로 NSObject에서 파생 되는 모든 관리 되는 클래스의 목록을 생성 하 여 수행 합니다.

- 기존 Objective-c 클래스를 래핑하는 모든 클래스에 대 한 미러링 있는 모든 관리 되는 멤버 Objective-c 구성원과 새 Objective C 클래스 만듭니다는 [`Export`] 특성입니다.

- 각 목표-C 멤버에 대 한 구현에서는 코드가 미러된 관리 되는 멤버를 호출 하려면 자동으로 추가 됩니다.

아래의 의사 코드에서는이 수행 되는 방법의 예를 보여 줍니다.

**C# (관리 코드)**

```

 class MyViewController : UIViewController{
    [Export ("myFunc")]
    public void MyFunc ()
    {
    }
 }


```

**Objective-C:**

```csharp
@interface MyViewController : UIViewController { }

    -(void)myFunc;
@end @implementation

    MyViewController {}
    -(void) myFunc
    {
    /* code to call the managed MyViewController.MyFunc method */
    }
@end

```

관리 코드의 특성을 포함할 수 `[Register]` 및 `[Export]`, 등록자를 사용 하 여 개체가 없습니다. 목표에 노출 되도록 해야 함을 알고
`[Register]` 특성 기본 생성 된 이름이 적합 한 경우에 생성된 된 Objective-c 클래스의 이름을 지정 하는 데 사용 됩니다. NSObject에서 파생 된 모든 클래스가 자동으로 등록 된 목표 없습니다.
필요한 `[Export]` 특성 선택기 생성된 Objective-c 클래스에 사용 되는 문자열을 포함 합니다.

등록 기관 Xamarin.iOS – 동적 및 정적에 사용 된 유형은


- **동적 등록 기관** – 동적 등록자는 런타임 시 사용자 어셈블리에 있는 모든 형식의 등록 합니다. 제공 하는 함수를 사용 하 여 수행 [Objective-c의 런타임 API](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/)합니다. 따라서 동적 등록 기관 느린 시작 모드를, 더 빠르거나 있지만 빌드 시간을 포함 됩니다. IOS 시뮬레이터에 대 한 기본값입니다. 동적 등록을 사용 하는 경우 trampolines, 호출 된 (일반적으로 C)에서 네이티브 함수 메서드 구현으로 사용 됩니다. 여러 아키텍처 마다 다릅니다.

- **정적 등록 기관** -정적 등록자를 정적 라이브러리로 취합 되어 실행 파일에 연결 하는 빌드 중 Objective C 코드를 생성 합니다. 이 빠른 시작을 위한 허용 하지만 빌드 시간 동안 오래 걸리는 합니다. 이 장치 빌드에 대해 기본적으로 사용 됩니다. 정적 등록자는 iOS 시뮬레이터와 전달 하 여 `--registrar:static` 로 `mtouch` 아래와 같이 프로젝트의 빌드 옵션 특성:

    [ ![](architecture-images/image1.png "추가 mtouch 인수 설정")](architecture-images/image1.png)

IOS Xamarin.iOS에서 사용 하는 형식 등록 시스템의 세부 사항에 자세한 내용은 참조는 [형식 등록자](~/ios/internals/registrar.md) 가이드입니다.

## <a name="application-launch"></a>응용 프로그램 시작

모든 Xamarin.iOS 실행 파일의 진입점 함수를 호출 하 여 제공 됩니다 `xamarin_main`, 모노 초기화입니다.

프로젝트 형식에 따라 다음 작업 수행 됩니다.

- 일반 iOS 및 tvOS 응용 프로그램에 대 한 Xamarin 앱에서 제공 되는 관리 되는 Main 메서드에서 호출 됩니다. Main 메서드에서 관리 되는이 호출 다음 `UIApplication.Main`, 없습니다. 목표에 대 한 진입점을 변수인 UIApplication.Main는 Objective-c의에 대 한 바인딩을 `UIApplicationMain` 메서드.
- 확장의 경우 네이티브 함수 – `NSExtensionMain` 또는 (`NSExtensionmain` WatchOS 확장에 대 한) – 제공 라이브러리 Apple에 의해 호출 됩니다. 이러한 프로젝트 클래스 라이브러리 및 하지 실행 파일 프로젝트의 되므로를 실행할 관리 되는 Main 메서드가 없습니다.

이 시작 시퀀스의 모든 앱에서 지 면에서 가져오는 방법을 알 수 있도록 다음 최종 실행 파일에 연결 되는 정적 라이브러리에 컴파일됩니다.

앱이 시점에서 시작 되, 모노 실행 되 고, 관리 코드에서 및 네이티브 코드를 호출 하 고 다시 호출 하는 방법이 있습니다. 작업을 수행 하는 다음 작업 실제로 컨트롤을 추가 하기 시작 하 고 대화형 응용 프로그램을 만드는 것입니다.


## <a name="generator"></a>Generator

Xamarin.iOS 모든 단일 iOS API에 대 한 정의 포함합니다. 탐색할 수 있습니다 이러한 항목 중 하나에 [MaciOS github 리포지토리](https://github.com/xamarin/xamarin-macios/tree/master/src)합니다. 이러한 정의 특성을 가진 인터페이스 뿐만 아니라 필요한 메서드 및 속성을 포함 합니다. 예를 들어 다음 코드는을 사용 하는 UIKit에는 UIToolbar 정의 [네임 스페이스](https://github.com/xamarin/xamarin-macios/blob/master/src/uikit.cs#L11277-L11327)합니다. 다양 한 메서드 및 속성을 사용 하 여 인터페이스 인지 확인 합니다.

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

호출할 생성자 [ `btouch` ](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs) Xamarin.iOS에서 이러한 정의 파일을 사용 하 고.NET 도구를 사용 하 여 [임시 어셈블리에 컴파일할](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs#L318)합니다. 그러나이 임시 어셈블리 파일이 Objective C 코드를 호출할 사용할 수 없습니다. 생성기에서 다음 임시 어셈블리 읽고 런타임에 사용할 수 있는 C# 코드를 생성 합니다.
이유, 예를 들어 정의.cs 파일에 임의의 특성을 추가 하는 경우 그에 나타나지 않습니다 출력된 코드입니다. 생성기 알지 못하기 스레드와 `btouch` 를 출력 하는 것은 임시 어셈블리에 찾을 하는지 알 수 없습니다.

Xamarin.iOS.dll를 만든 후 mtouch 모든 구성 요소를 함께 묶이 됩니다.

상위 수준에서 달성이 다음 작업을 실행 하 여:

-   앱 번들 구조를 만듭니다.
-   관리 되는 어셈블리에 복사 합니다.
-   연결을 사용 하는 경우 아웃 사용 하지 않는 부분을 복사 하 여 어셈블리를 최적화 하기 위해 관리 되는 링커를 실행 합니다.
-   AOT 컴파일입니다.
-   네이티브 실행 파일 시작 관리자 코드, 등록자 코드 (정적) 경우 및는 AOT의 모든 출력으로 구성 하는 일련의 기본 실행 파일에 연결 된 (각 어셈블리에 대해 하나씩) 정적 라이브러리를 출력 하는 기본 실행 파일을 만듭니다 컴파일러


링커 및 사용 방법에 대 한 정보를 자세한 참조는 [링커](~/ios/deploy-test/linker.md) 가이드입니다.

## <a name="summary"></a>요약

이 가이드는 AOT 컴파일 Xamarin.iOS 앱 및 탐색된 Xamarin.iOS 및 깊이에서 관계 Objective-c를 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [제한 사항](~/ios/internals/limitations.md)
- [Objective C 바인딩](~/cross-platform/macios/binding/overview.md)
- [Objective C 선택기](~/ios/internals/objective-c-selectors.md)
- [유형 등록 기관](~/ios/internals/registrar.md)
- [Linker](~/ios/deploy-test/linker.md)
