---
title: Xamarin.Mac architecture
description: 이 가이드에서는 objective-c 낮은 수준에서 Xamarin.Mac 및 해당 관계를 살펴봅니다. 컴파일, 선택기, 기관, 앱 시작 및 생성자와 같은 개념을 설명합니다.
ms.prod: xamarin
ms.assetid: 74D1FF57-4F2A-4646-8669-003DE99671D4
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 04/12/2017
ms.openlocfilehash: 1ea38b527acaa89b9f25690de4e55664a7afd9e8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61034152"
---
# <a name="xamarinmac-architecture"></a>Xamarin.Mac architecture

_이 가이드에서는 objective-c 낮은 수준에서 Xamarin.Mac 및 해당 관계를 살펴봅니다. 컴파일, 선택기, 기관, 앱 시작 및 생성자와 같은 개념을 설명합니다._

## <a name="overview"></a>개요

Xamarin.Mac 응용 프로그램 Mono 실행 환경 내에서 실행 하 고는 Just 시간 (JIT)-런타임에 네이티브 코드로 컴파일된 다음 아래로 IL (Intermediate Language)을 컴파일하는 데 Xamarin의 컴파일러를 사용 합니다. Side-by-side-실행 Objective C 런타임을 사용 합니다. 두 런타임 환경 UNIX 유사 커널을, 특히 XNU 위에서 실행 되며 기본 네이티브 또는 관리 되는 시스템에 액세스 하려면 개발자가 사용자 코드에 다양 한 Api를 노출 합니다.

아래 다이어그램은이 아키텍처의 기본적인 개요를 보여 줍니다.

[![아키텍처의 기본적인 개요를 보여 주는 다이어그램](architecture-images/mac-arch.png "아키텍처의 기본적인 개요를 보여 주는 다이어그램")](architecture-images/mac-arch-large.png#lightbox)

### <a name="native-and-managed-code"></a>네이티브 및 관리 코드

Xamarin을 개발 하는 경우 용어 *네이티브* 하 고 *관리* 코드는 종종 사용 됩니다. 관리 코드는.NET Framework 공용 언어 런타임, 또는 Xamarin의 경우 관리 되는 실행 된 코드: Mono 런타임입니다.

네이티브 코드는 고유 하 게 (예를 들어, Objective-c 또는 ARM 칩에도 AOT 컴파일되어야 코드)는 특정 플랫폼에서 실행 되는 코드입니다. 이 가이드에서는 관리 코드를 네이티브 코드로 컴파일됩니다 및 Xamarin.Mac 응용 프로그램의 작동 하는 방법에 대해 설명 하는 방법 또한 액세스 하는 동안 바인딩 사용 하 여 Apple Mac Api의 전체 사용 합니다. 와 같은 복잡 한 언어 및 NET의 BCL C#입니다.

## <a name="requirements"></a>요구 사항

Xamarin.Mac으로 macOS 애플리케이션을 개발하려면 다음이 필요합니다.

- Mac 실행 중인 macOS Sierra 10.12 이상.
- 최신 버전의 Xcode (에서 설치 합니다 [앱 스토어](https://itunes.apple.com/us/app/xcode/id497799835?mt=12))
- 최신 버전의 Xamarin.Mac 및 Mac 용 Visual Studio

Xamarin.Mac으로 만든 Mac 애플리케이션을 실행하려면 다음과 같은 시스템 요구 사항이 필요합니다.

- Mac OS X 10.7 이상을 실행 하는 Mac.

## <a name="compilation"></a>컴파일

모든 Xamarin 플랫폼 응용 프로그램의 Mono를 컴파일할 때 C# (또는 F#) 컴파일러를 실행 하 고 컴파일되지에 C# 및 F# 코드를 Microsoft Intermediate Language (MSIL 또는 IL). Xamarin.Mac을 사용 하 여는 *시간 JIT (in) 하면* 필요에 따라 올바른 아키텍처에 대 한 실행을 허용 하는 네이티브 코드를 컴파일하는 런타임 시 컴파일러.

이 aot를 사용 하는 Xamarin.iOS 대조적입니다. AOT 컴파일러를 사용 하는 경우 모든 메서드와 그 안에 포함 되는 모든 어셈블리가 빌드 시에 컴파일됩니다. JIT를 사용 하 여 컴파일 시 실행 되는 방법에 대해서만 발생 합니다.

Xamarin.Mac 응용 프로그램을 사용 하 여 Mono는 일반적으로 포함 된 앱 번들으로 (이라고 **포함 된 Mono**). 그러나 클래식 Xamarin.Mac API를 사용 하는 경우 응용 프로그램 대신 사용할 수 **시스템 Mono**,이 지원 되지 않는 통합 api에서. 시스템 Mono 운영 체제에 설치 된 Mono를 가리킵니다. Xamarin.Mac 앱은 응용 프로그램 시작 시이 사용 합니다.

## <a name="selectors"></a>선택기

Xamarin을 사용 하 여 두 개의 별도 에코 시스템,.NET 및 Apple 상태로 전환 해야 하는 것으로 원활한 사용자 환경을 최종 목표는 되도록 가능한 한 간소화 된 것을 함께 합니다. 런타임과 통신 하는 방법을 위의 섹션에서 살펴본 및 매우 잘 들어보았을 기본 Mac Api Xamarin에서 사용할 수 있는 용어 '바인딩'. 바인딩에서 자세히 설명 합니다 [Objective-c 바인딩 설명서](~/mac/platform/binding.md)이므로 지금은 내부적 Xamarin.Mac 작동 원리를 살펴보겠습니다.

Objective-c를 노출 하는 방법에 먼저 C#에 있는 선택기를 통해 수행 됩니다. 선택기에는 개체 또는 클래스에 전송 된 메시지입니다. Objective C를 사용 하 여 이렇게 통해 합니다 [objc_msgSend](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/index.html) 함수입니다. 선택기를 사용 하 여에 대 한 자세한 내용은 iOS 참조 [Objective-c 선택기](~/ios/internals/objective-c-selectors.md) 가이드입니다. 또한 더 때문에 Objective-c로 관리 코드에 대해서 알지 못하기 더 복잡 한는 Objective-c로 관리 되는 코드를 노출 하는 방법입니다. 이 문제를 해결 하려면 사용 된 [등록자](~/mac/internals/registrar.md)합니다. 이 다음 섹션에서 자세히 설명 되어 있습니다.

## <a name="registrar"></a>등록자

등록 기관은 목표 C에 노출 관리 되는 코드를 코드는 위에서 설명한 대로 NSObject에서 파생 되는 모든 관리 되는 클래스의 목록을 만들어 수행 합니다.

- 기존 Objective-c 클래스를 래핑하는 모든 클래스에 있는 모든 관리 되는 멤버를 미러링 하는 Objective-c로 멤버를 사용 하 여 새 Objective C 클래스 만들고는 `[Export]` 특성입니다.
- 각 – Objective-c 멤버에 대 한 구현에서 코드는 미러된 관리 되는 멤버를 호출에 자동으로 추가 됩니다.

아래의 의사 (pseudo) 코드는이 수행 되는 방법의 예를 보여줍니다.

**C#(관리 코드):**

```csharp
class MyViewController : UIViewController{
    [Export ("myFunc")]
    public void MyFunc ()
    {
    }
 }
 ```

**Objective C (네이티브 코드용):**

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

관리 코드에 특성을 포함할 수 있습니다 `[Register]` 고 `[Export]`, 등록자를 사용 하 여 개체를 보여주기 위한 것입니다.에 노출 해야 한다고 알고 [등록] 특성을 생성 하는 기본 이름은 적합 한 경우 생성된 된 Objective-c 클래스의 이름을 지정 됩니다. NSObject에서 파생 된 모든 클래스가 자동으로 등록 된 보여주기 위한 것입니다. 필수 [내보내기] 특성 선택기 Objective-c로 생성 된 클래스에서 사용 되는 문자열을 포함 합니다.

Xamarin.Mac – 동적 및 정적에 사용 되는 등록자의는 다음과 같은 두 종류가 있습니다.

- 동적 등록 기관 – 모든 Xamarin.Mac 빌드에 대 한 기본 등록 기관입니다. 동적 등록 기관은 런타임에 어셈블리에서 모든 종류의 등록을 수행합니다. Objective-c의 런타임 API에서 제공 하는 함수를 사용 하 여 수행 합니다. 따라서 동적 등록 기관은 느린 신생 기업, 빠르고 있지만 빌드 시간을 포함 됩니다. 동적 등록 기관을 사용 하는 경우 trampolines, 호출 (C)에서 일반적으로 네이티브 함수 메서드 구현으로 사용 됩니다. 이러한 다양 한 아키텍처 마다 다릅니다.
- 정적 등록 기관 – 정적 등록 기관에는 정적 라이브러리에 컴파일되고 실행 파일에 연결 하는 빌드 중 Objective-c 코드를 생성 합니다. 이 빠른 시작을 허용 하지만 빌드 시 길어집니다.

## <a name="application-launch"></a>응용 프로그램 시작

포함 여부에 따라 다른 Xamarin.Mac 시작 논리 또는 Mono 시스템을 사용 합니다. 코드 및 Xamarin.Mac 응용 프로그램 시작에 대 한 단계를 보려면를 참조 합니다 [시작 헤더](https://github.com/xamarin/xamarin-macios/blob/master/runtime/xamarin/launch.h) xamarin macios 공용 리포지토리에 있는 파일입니다.

## <a name="generator"></a>Generator

Xamarin.Mac 모든 Mac API에 대 한 정의 포함합니다. 탐색할 수 있습니다 이러한 합니다 [MaciOS github 리포지토리](https://github.com/xamarin/xamarin-macios/tree/master/src)합니다. 이러한 정의 특성을 가진 인터페이스 뿐만 아니라 모든 필요한 메서드 및 속성을 포함 합니다. 다음 코드는 NSBox에서 정의 하는 예를 들어 합니다 [AppKit 네임 스페이스](https://github.com/xamarin/xamarin-macios/blob/master/src/appkit.cs#L1465-L1526)합니다. 다양 한 메서드 및 속성을 사용 하 여 인터페이스 인지 확인 합니다.

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

생성자 호출 `bmac` Xamarin.Mac는 이러한 정의 파일 및 임시 어셈블리로 컴파일하기 위해.NET 도구를 사용 합니다. 그러나이 임시 어셈블리 Objective-c 코드를 호출 하는 사용 가능한 아닙니다. 생성기 다음 임시 어셈블리를 읽고 생성 C# 런타임에 사용할 수 있는 코드입니다. 따라서 예를 들어, 임의 특성을 정의.cs 파일에 추가 하는 경우이 표시 되지 않습니다 출력 코드에서입니다. 생성기를 알지 있어 `bmac` 출력 임시 어셈블리를 찾는 위치를 알 수 없습니다.

Xamarin.Mac.dll를 만든 후, packager를 `mmp`, 모든 구성 요소를 함께 번들 됩니다.

높은 수준에서이 작업을 위해 다음 작업을 실행 하 여:

- 앱 번들 구조를 만듭니다.
- 관리 되는 어셈블리에서 복사 합니다.
- 연결을 사용 하는 경우 사용 하지 않는 부분을 제거 하 여 어셈블리를 최적화 하기 위해 관리 되는 링커를 실행 합니다.
- 시작 관리자 응용 프로그램을 고정 모드에서 작업 하는 경우 등록 기관 코드와 함께 이야기 launcher 코드에서 연결을 만듭니다.

이 사용자의 일부로 실행 빌드 프로세스에서 사용자 코드를 어셈블리로 컴파일합니다를 참조 하는 Xamarin.Mac.dll 및 실행 한 다음 `mmp` 패키지를 확인 하려면

링커 및 사용 방법에 대 한 정보를 자세한 참조 iOS [링커](~/ios/deploy-test/linker.md) 가이드입니다.

## <a name="summary"></a>요약

이 가이드의 Xamarin.Mac 앱 및 탐색된 Xamarin.Mac 및 해당 관계를 보여주기 위한 것입니다. 컴파일 살펴보고
