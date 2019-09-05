---
title: Xamarin.ios 아키텍처
description: 이 가이드에서는 Xamarin.ios 및 목표-C와의 관계를 낮은 수준에서 살펴봅니다. 컴파일, 선택기, 등록 기관, 앱 시작 및 생성기와 같은 개념을 설명 합니다.
ms.prod: xamarin
ms.assetid: 74D1FF57-4F2A-4646-8669-003DE99671D4
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 04/12/2017
ms.openlocfilehash: 2c9bbd663257e937e35e062f03b4aa84813edb27
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70287780"
---
# <a name="xamarinmac-architecture"></a>Xamarin.ios 아키텍처

_이 가이드에서는 Xamarin.ios 및 목표-C와의 관계를 낮은 수준에서 살펴봅니다. 컴파일, 선택기, 등록 기관, 앱 시작 및 생성기와 같은 개념을 설명 합니다._

## <a name="overview"></a>개요

Xamarin.ios 응용 프로그램은 Mono 실행 환경 내에서 실행 되 고, Xamarin의 컴파일러를 사용 하 여 IL (중간 언어)로 컴파일 (JIT (Just-in-time)) 하 고 런타임에 네이티브 코드로 컴파일됩니다. 이는 목표-C 런타임과 side-by-side 방식으로 실행 됩니다. 두 런타임 환경은 모두 UNIX와 유사한 커널, 특히 XNU에서 실행 되며 개발자가 기본 네이티브 또는 관리 되는 시스템에 액세스할 수 있도록 하는 다양 한 Api를 사용자 코드에 노출 합니다.

아래 다이어그램은이 아키텍처의 기본 개요를 보여 줍니다.

[![아키텍처의 기본 개요를 보여 주는 다이어그램](architecture-images/mac-arch.png "아키텍처의 기본 개요를 보여 주는 다이어그램")](architecture-images/mac-arch-large.png#lightbox)

### <a name="native-and-managed-code"></a>네이티브 및 관리 코드

Xamarin 용으로 개발 하는 경우 *네이티브* 코드 및 *관리* 코드 라는 용어를 사용 하는 경우가 많습니다. 관리 코드는 .NET Framework 공용 언어 런타임에서 실행 되는 코드 또는 Xamarin의 경우 Mono 런타임입니다.

네이티브 코드는 특정 플랫폼에서 기본적으로 실행 되는 코드입니다 (예: AOT 또는 ARM 칩에서 컴파일된 코드). 이 가이드에서는 관리 코드를 네이티브 코드로 컴파일하는 방법에 대해 설명 하 고, Xamarin.ios 응용 프로그램이 작동 하는 방식에 대해 설명 하 고 바인딩을 사용 하 여 Apple의 Mac Api를 완전히 사용 하 고에도 액세스할 수 있도록 합니다. 순의 BCL 및와 C#같은 고급 언어입니다.

## <a name="requirements"></a>요구 사항

Xamarin.Mac으로 macOS 애플리케이션을 개발하려면 다음이 필요합니다.

- MacOS Sierra (10.12) 이상을 실행 하는 Mac입니다.
- 최신 버전의 Xcode ( [앱 스토어](https://itunes.apple.com/us/app/xcode/id497799835?mt=12)에서 설치 됨)
- 최신 버전의 Xamarin.ios 및 Mac용 Visual Studio

Xamarin.Mac으로 만든 Mac 애플리케이션을 실행하려면 다음과 같은 시스템 요구 사항이 필요합니다.

- 10.7 이상을 실행 하는 Mac Mac OS X.

## <a name="compilation"></a>컴파일

Xamarin platform C# 응용 프로그램을 컴파일할 때 Mono (또는 F#) 컴파일러는를 실행 하 고 C# 및 F# 코드를 MSIL 또는 IL (Microsoft 중간 언어)로 컴파일합니다. 그런 다음 xamarin.ios는 런타임에 *JIT (just-in-time)* 컴파일러를 사용 하 여 네이티브 코드를 컴파일하고 필요에 따라 올바른 아키텍처에서 실행할 수 있도록 합니다.

AOT 컴파일을 사용 하는 Xamarin.ios와는 대조적입니다. AOT 컴파일러를 사용 하는 경우 모든 어셈블리와 그 안의 모든 메서드는 빌드 시에 컴파일됩니다. JIT를 사용 하면 실행 되는 메서드에만 요청 시 컴파일이 수행 됩니다.

Xamarin.ios 응용 프로그램을 사용 하는 경우 Mono는 일반적으로 앱 번들에 포함 되 고 **포함 된 mono**라고 합니다. 클래식 Xamarin.ios API를 사용 하는 경우 응용 프로그램에서 **시스템 Mono**를 대신 사용할 수 있습니다. 그러나이는 Unified API에서 지원 되지 않습니다. 시스템 Mono는 운영 체제에 설치 된 Mono를 나타냅니다. 응용 프로그램 시작 시 Xamarin.ios 앱은이를 사용 합니다.

## <a name="selectors"></a>선택기

Xamarin을 사용 하는 경우 두 개의 개별 에코 시스템 .NET 및 Apple이 있습니다 .이를 통해 최대한 간소화 된 것으로 표시 하 여 최종 목표가 원활한 사용자 환경을 보장 해야 합니다. 위의 섹션에는 두 런타임이 통신 하는 방법에 대 한 내용이 나와 있으며, Xamarin에서 네이티브 Mac Api를 사용할 수 있도록 하는 용어 ' 바인딩 '을 매우 잘 들었습니다. 바인딩은 [목표-C 바인딩 설명서](~/mac/platform/binding.md)에 자세히 설명 되어 있으므로 이제 xamarin.ios에서 작동 하는 방식을 살펴보겠습니다.

첫째, 선택기를 통해 수행 되는 목표-C를에 C#노출 하는 방법이 있어야 합니다. 선택기는 개체 또는 클래스로 전송 되는 메시지입니다. [Objc_msgSend](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/index.html) 함수를 통해이 작업을 수행할 수 있습니다. 선택기 사용에 대 한 자세한 내용은 iOS [목표-C 선택기](~/ios/internals/objective-c-selectors.md) 가이드를 참조 하세요. 또한 관리 코드를 목표 c에 노출 하는 방법이 있어야 합니다 .이는 목표-C가 관리 코드에 대 한 정보를 알 수 없기 때문에 더 복잡 합니다. 이 문제를 해결 하기 위해 [등록자](~/mac/internals/registrar.md)를 사용 합니다. 이에 대해서는 다음 섹션에서 자세히 설명 합니다.

## <a name="registrar"></a>등록자

위에서 언급 했 듯이 등록자는 관리 코드를 목표 C에 노출 하는 코드입니다. NSObject에서 파생 되는 모든 관리 되는 클래스 목록을 만들어이 작업을 수행 합니다.

- 기존 목표-c 클래스를 래핑하는 모든 클래스의 경우,이 클래스는 `[Export]` 특성이 있는 모든 관리 되는 멤버를 포함 하는 객관적인 c 멤버를 사용 하 여 새 목표 c 클래스를 만듭니다.
- 각 목표 – C 멤버에 대 한 구현에서는 미러된 관리 되는 멤버를 호출 하기 위해 코드가 자동으로 추가 됩니다.

아래 의사 코드는 이러한 작업을 수행 하는 방법의 예를 보여 줍니다.

**C#(관리 코드):**

```csharp
class MyViewController : UIViewController{
    [Export ("myFunc")]
    public void MyFunc ()
    {
    }
 }
 ```

**목표-C (네이티브 코드):**

```objc
@interface MyViewController : UIViewController
 - (void)myFunc;
@end 

@implementation MyViewController
- (void)myFunc {
    // Code to call the managed C# MyFunc method in MyViewController
}
@end
```

관리 코드에는를 사용 하 여 `[Register]` 개체 `[Export]`를 목표에 노출 해야 한다는 것을 알 수 있도록 등록 기관에서 사용 하는 특성 및이 포함 될 수 있습니다. [Register] 특성은 생성 된 기본 이름이 적절 하지 않은 경우 생성 된 목표-C 클래스의 이름을 지정 하는 데 사용 됩니다. NSObject에서 파생 된 모든 클래스는 목표-C에 자동으로 등록 됩니다. 필수 [Export] 특성에는 생성 된 목표-C 클래스에서 사용 되는 선택기 인 문자열이 포함 되어 있습니다.

Xamarin.ios에서 사용 되는 등록 기관에는 동적 및 정적 이라는 두 가지 유형이 있습니다.

- Dynamic 등록 기관 – 모든 Xamarin.ios 빌드에 대 한 기본 등록자입니다. 동적 등록자는 런타임에 어셈블리의 모든 형식에 대해 등록을 수행 합니다. 이는 목표-C의 런타임 API에서 제공 하는 함수를 사용 하 여 수행 합니다. 따라서 동적 등록자는 시작 속도가 느리고 빌드 시간을 단축 합니다. Trampolines 라고 하는 네이티브 함수 (일반적으로 C)는 동적 등록 기관를 사용할 때 메서드 구현으로 사용 됩니다. 아키텍처는 서로 다릅니다.
- 정적 등록 기관 – 정적 등록자는 빌드 중에 목표 C 코드를 생성 합니다 .이 코드는 정적 라이브러리로 컴파일되고 실행 파일에 연결 됩니다. 이렇게 하면 더 빨리 시작할 수 있지만 빌드 시간 동안 시간이 오래 걸립니다.

## <a name="application-launch"></a>응용 프로그램 시작

Xamarin.ios 시작 논리는 embedded 또는 시스템 Mono를 사용 하는지에 따라 달라 집니다. Xamarin.ios 응용 프로그램 시작에 대 한 코드와 단계를 보려면 xamarin-macios 공용 리포지토리에서 [시작 헤더](https://github.com/xamarin/xamarin-macios/blob/master/runtime/xamarin/launch.h) 파일을 참조 하세요.

## <a name="generator"></a>Generator

Xamarin.ios에는 모든 Mac API에 대 한 정의가 포함 되어 있습니다. [Macios github](https://github.com/xamarin/xamarin-macios/tree/master/src)리포지토리에서 이러한 중 하나를 탐색할 수 있습니다. 이러한 정의에는 특성이 포함 된 인터페이스 뿐만 아니라 필요한 메서드 및 속성도 포함 됩니다. 예를 들어 다음 코드는 [Appkit 네임 스페이스](https://github.com/xamarin/xamarin-macios/blob/master/src/appkit.cs#L1465-L1526)에서 nsbox를 정의 하는 데 사용 됩니다. 이 인터페이스는 여러 메서드 및 속성을 포함 하는 인터페이스입니다.

```csharp
[BaseType (typeof (NSView))]
public interface NSBox {

    …

    [Export ("borderRect")]
    CGRect BorderRect { get; }

    [Export ("titleRect")]
    CGRect TitleRect { get; }

    [Export ("titleCell")]
    NSObject TitleCell { get; }

    [Export ("sizeToFit")]
    void SizeToFit ();

    [Export ("contentViewMargins")]
    CGSize ContentViewMargins { get; set; }

    [Export ("setFrameFromContentFrame:")]
    void SetFrameFromContentFrame (CGRect contentFrame);

    …

}
```

Xamarin.ios에서 호출 `bmac` 되는 생성기는 이러한 정의 파일을 사용 하 고 .net 도구를 사용 하 여 임시 어셈블리로 컴파일합니다. 그러나이 임시 어셈블리는 목표-C 코드를 호출할 수 없습니다. 그러면 생성기에서 임시 어셈블리를 읽고 런타임에 사용할 C# 수 있는 코드를 생성 합니다. 예를 들어, 사용자가 정의 .cs 파일에 임의 특성을 추가 하면 출력 코드에 표시 되지 않습니다. 생성기는이 `bmac` 에 대해 알지 못하는 것 이므로 임시 어셈블리에서이를 검색 하 여 출력 하는 것을 알 수 없습니다.

Xamarin.ios를 만든 `mmp`후에는 패키지를 통해 모든 구성 요소를 번들로 묶습니다.

개략적인 수준에서 다음 작업을 실행 하 여이 작업을 수행 합니다.

- 앱 번들 구조를 만듭니다.
- 관리 되는 어셈블리를 복사 합니다.
- 링크를 사용 하는 경우 관리 되는 링커를 실행 하 여 사용 하지 않는 부분을 제거 하 여 어셈블리를 최적화 합니다.
- 정적 모드의 경우 등록자 코드와 함께 표시 되는 시작 관리자 코드에서 연결 하는 시작 관리자 응용 프로그램을 만듭니다.

그런 다음 사용자 코드를 파일의 xamarin.ios를 참조 하는 어셈블리로 컴파일하고를 실행 `mmp` 하 여 패키지로 만드는 사용자 빌드 프로세스의 일부로 실행 됩니다.

링커에 대 한 자세한 내용 및 사용 방법에 대 한 자세한 내용은 iOS [링커](~/ios/deploy-test/linker.md) 가이드를 참조 하세요.

## <a name="summary"></a>요약

이 가이드에서는 Xamarin.ios 앱을 컴파일한 다음, Xamarin.ios 및 목표에 대 한 관계를 탐색 하는 방법을 살펴보았습니다.
