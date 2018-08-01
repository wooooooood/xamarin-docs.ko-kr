---
title: Xamarin.Mac 아키텍처
description: 이 가이드를 낮은 수준에서 Objective-c Xamarin.Mac 활동 및 활동과 관계를 탐색합니다. 컴파일, 선택기, 등록, 앱 시작 및 생성기 같은 개념을 설명합니다.
ms.prod: xamarin
ms.assetid: 74D1FF57-4F2A-4646-8669-003DE99671D4
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/12/2017
ms.openlocfilehash: d6d7557fed5ea0ca0719dcbddbda316340645320
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30785558"
---
# <a name="xamarinmac-architecture"></a>Xamarin.Mac 아키텍처

_이 가이드를 낮은 수준에서 Objective-c Xamarin.Mac 활동 및 활동과 관계를 탐색합니다. 컴파일, 선택기, 등록, 앱 시작 및 생성기 같은 개념을 설명합니다._

## <a name="overview"></a>개요

Xamarin.Mac 응용 프로그램 모노 실행 환경 내에서 실행 및 Xamarin 컴파일러 변수인 Just 시간 JIT ()에서 런타임에 네이티브 코드로 컴파일된 다음 아래쪽으로 중간 언어 (IL)를 컴파일하는 데 사용 합니다. 병렬 실행이 Objective C 런타임을 사용 합니다. 런타임 환경 모두 UNIX 방식 커널, 특히 XNU 위에서 실행 및 때문에 개발자가 기본 네이티브 또는 관리 되는 시스템에 액세스 하려면 사용자 코드에 다양 한 Api를 노출 합니다.

다음 다이어그램에서는이 아키텍처의 기본적인 개요를 보여 줍니다.

[![아키텍처의 기본적인 개요를 보여 주는 다이어그램](architecture-images/mac-arch.png "아키텍처의 기본적인 개요를 보여 주는 다이어그램")](architecture-images/mac-arch-large.png#lightbox)

### <a name="native-and-managed-code"></a>네이티브 모듈과 관리 코드

Xamarin에 대 한 개발 하는 경우 약관 *네이티브* 및 *관리* 코드는 종종 사용 됩니다. 관리 되는 코드는 포함 된.NET Framework 공용 언어 런타임, 또는 Xamarin의 경우 관리 하는 실행 코드를: 모노 런타임.

네이티브 코드는 코드입니다 (예를 들어 Objective-c 또는 ARM 칩의 AOT 컴파일된 코드)는 특정 플랫폼에서 기본적으로 실행 됩니다. 이 가이드에 대 한 액세스도 않고 바인딩 사용 하 여 Apple Mac api 모두 사용할 어떻게 관리 코드를 네이티브 코드로 컴파일되고 Xamarin.Mac 응용 프로그램의 작동 방법에 대해 설명 탐색 합니다. NET의 BCL 및 C#과 같은 복잡 한 언어입니다.

## <a name="requirements"></a>요구 사항

Xamarin.Mac으로 macOS 응용 프로그램을 개발하려면 다음이 필요합니다.

- Mac 실행 중인 macOS 시에라 (10.12) 이상.
- 최신 버전의 Xcode (에서 설치는 [앱 스토어](https://itunes.apple.com/us/app/xcode/id497799835?mt=12))
- Xamarin.Mac 및 Mac 용 Visual Studio의 최신 버전

Xamarin.Mac으로 만든 Mac 응용 프로그램을 실행하려면 다음과 같은 시스템 요구 사항이 필요합니다.

- Mac OS X 10.7 이상 실행 하는 Mac.

## <a name="compilation"></a>컴파일

Xamarin 플랫폼 응용 프로그램을 컴파일할 때 모노 C# (또는 F #) 컴파일러 실행 되 고 C# 및 F # 코드 Microsoft Intermediate Language (MSIL 또는 IL)로 컴파일됩니다. Xamarin.Mac 다음 사용 하 여 한 *마법사에 JIT (Time)* 필요에 따라 올바른 아키텍처에는 실행을 허용 하는 네이티브 코드를 컴파일하는 런타임 시 컴파일러.

즉 AOT 컴파일을 사용 하 여 Xamarin.iOS입니다. AOT 컴파일러를 사용 하는 경우 모든 메서드와 그 안에 포함 되는 모든 어셈블리 빌드 시에 컴파일됩니다. 컴파일 동작 JIT를 사용 하 여 실행 되는 방법에 대 한 주문형 합니다.

Xamarin.Mac 응용 프로그램과 함께 모노 앱 번들에 포함 일반적으로 됩니다 (라고 하 고 **포함 된 모노**). 그러나 클래식 Xamarin.Mac API를 사용 하는 경우 응용 프로그램 대신 사용할 수 **시스템 모노**,이에서는 지원 되지 않으며 통합 API입니다. 운영 체제에 설치 된 모노 시스템 모노 참조 합니다. 응용 프로그램 시작 시 Xamarin.Mac 앱이를 사용 합니다.

## <a name="selectors"></a>선택기

두 개의 별도 에코 시스템,.NET 및 Apple 상태로 전환 해야 하는 xamarin을 하기 위해 함께 최종 목표 원활한 사용자 환경을 지 확인 하려면 가능한 같이 간소화 된 것 같습니다. 두 개의 런타임 통신 하는 방법 위의 섹션에서 살펴본 것 및 상태일 때 잘 들어보았을 것 용어의 '바인딩은' 네이티브 Mac Api Xamarin에서 사용할 수 있습니다. 바인딩은에서 자세히 설명 되어는 [Objective-c 바인딩 설명서](~/mac/platform/binding.md)지금은 하므로 Xamarin.Mac 내부에서 작동 하는 방법을 살펴보겠습니다.

첫째, Objective-c 선택기를 통해 수행 된 C#을 노출 하는 방법에 있습니다. 선택 기가 개체 또는 클래스에 전송 된 메시지입니다. Objective C 사용을 통해 수행 됩니다이 [objc_msgSend](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/index.html) 함수입니다. 선택기를 사용 하 여에 대 한 자세한 내용은 iOS 참조 [Objective-c 선택기](~/ios/internals/objective-c-selectors.md) 가이드입니다. 발생에 때문을 관리 코드에 대해서 Objective-c 알지 못하기 더 복잡 한 변수인 Objective C로 관리 되는 코드를 노출 하는 방법에 있습니다. 이 해결 하려면 사용을 [등록자](~/mac/internals/registrar.md)합니다. 이 다음 섹션에서 자세히 설명 되어 있습니다.

## <a name="registrar"></a>등록자

등록자는 Objective c 노출 관리 되는 코드를 코드는 위에서 설명한 대로 NSObject에서 파생 되는 모든 관리 되는 클래스의 목록을 생성 하 여 수행 합니다.

- 기존 Objective-c 클래스를 래핑하는 모든 클래스에 대 한 있는 모든 관리 되는 멤버 미러링 Objective-c 멤버가 포함 된 새 Objective-c 클래스 만듭니다는 `[Export]` 특성입니다.
- 각 목표-C 멤버에 대 한 구현에서는 코드가 미러된 관리 되는 멤버를 호출 하려면 자동으로 추가 됩니다.

아래의 의사 코드에서는이 수행 되는 방법의 예를 보여 줍니다.

**C# (관리 코드):**

```csharp
class MyViewController : UIViewController{
    [Export ("myFunc")]
    public void MyFunc ()
    {
    }
 }
 ```

**Objective C (네이티브 코드):**

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

관리 코드의 특성을 포함할 수 `[Register]` 및 `[Export]`, 등록자를 사용 하 여 개체가 없습니다. 목표에 노출 되도록 해야 함을 알고 [등록] 특성은 기본 생성 된 이름이 적합 한 경우에 생성된 된 Objective-c 클래스의 이름을 지정 하는 데 사용 됩니다. NSObject에서 파생 된 모든 클래스가 자동으로 등록 된 목표 없습니다. 필수 [내보내기] 특성 선택기 생성된 Objective-c 클래스에 사용 되는 문자열을 포함 합니다.

등록 기관 Xamarin.Mac – 동적 및 정적에 사용 된 유형은

- 동적 등록 기관 – 모든 Xamarin.Mac 빌드에 대 한 기본 등록 기관입니다. 동적 등록 기관에서 런타임에 어셈블리에 있는 모든 유형의 등록을 수행합니다. Objective-c의 런타임 API에서 제공 되는 함수를 사용 하 여 수행 합니다. 따라서 동적 등록 기관 느린 시작 모드를, 더 빠르거나 있지만 빌드 시간을 포함 됩니다. 동적 등록을 사용 하는 경우 trampolines, 호출 된 (일반적으로 C)에서 네이티브 함수 메서드 구현으로 사용 됩니다. 여러 아키텍처 마다 다릅니다.
- 정적 등록 기관-정적 등록자를 정적 라이브러리로 취합 되어 실행 파일에 연결 하는 빌드 중 Objective C 코드를 생성 합니다. 이 빠른 시작을 위한 허용 하지만 빌드 시간 동안 오래 걸리는 합니다.

## <a name="application-launch"></a>응용 프로그램 시작

포함 Xamarin.Mac 시작 논리에 따라 달라 집니다 또는 모노 시스템을 사용 합니다. 코드 및 Xamarin.Mac 응용 프로그램을 시작 하기 위한 단계를 보려면 참조는 [시작 헤더](https://github.com/xamarin/xamarin-macios/blob/master/runtime/xamarin/launch.h) xamarin macios 공용 리포지토리에 있는 파일입니다.

## <a name="generator"></a>Generator

Xamarin.Mac 모든 Mac API에 대 한 정의 포함합니다. 탐색할 수 있습니다 이러한 항목 중 하나에 [MaciOS github 리포지토리](https://github.com/xamarin/xamarin-macios/tree/master/src)합니다. 이러한 정의 특성을 가진 인터페이스 뿐만 아니라 필요한 메서드 및 속성을 포함 합니다. 예를 들어 다음 코드는에서 NSBox 정의 하는 데 사용 되는 [AppKit 네임 스페이스](https://github.com/xamarin/xamarin-macios/blob/master/src/appkit.cs#L1465-L1526)합니다. 다양 한 메서드 및 속성을 사용 하 여 인터페이스 인지 확인 합니다.

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

호출할 생성자 `bmac` Xamarin.Mac에서 이러한 정의 파일을 사용 하 고.NET 도구를 사용 하 여 임시 어셈블리로 컴파일할 수 있습니다. 그러나이 임시 어셈블리 파일이 Objective C 코드를 호출할 사용할 수 없습니다. 생성기에서 다음 임시 어셈블리 읽고 런타임에 사용할 수 있는 C# 코드를 생성 합니다. 이유, 예를 들어 정의.cs 파일에 임의의 특성을 추가 하는 경우 그에 나타나지 않습니다 출력된 코드입니다. 생성기 알지 못하기, 일부 이므로 `bmac` 를 출력 하는 것은 임시 어셈블리에 찾을 하는지 알 수 없습니다.

Xamarin.Mac.dll 만든 후에는 packager `mmp`, 모든 구성 요소를 하나로 묶습니다.

상위 수준에서 달성이 다음 작업을 실행 하 여:

- 앱 번들 구조를 만듭니다.
- 관리 되는 어셈블리에 복사 합니다.
- 연결을 사용 하는 경우 사용 하지 않는 부분을 제거 하 여 어셈블리를 최적화 하기 위해 관리 되는 링커를 실행 합니다.
- 정적 모드에 있는 경우 registar 코드와 함께 이야기 실행 프로그램 코드에 연결 하는 시작 관리자 응용 프로그램을 만듭니다.

이 참조 하는 Xamarin.Mac.dll 및 실행 하는 사용자의 일부로 실행 사용자 코드를 어셈블리로 컴파일되지 않는 프로세스를 빌드하여 `mmp` 패키지를 확인 하려면

IOS를 참조 하는 링커 및 사용 방법에 대 한 정보를 자세한 [링커](~/ios/deploy-test/linker.md) 가이드입니다.

## <a name="summary"></a>요약

이 가이드의 컴파일 Xamarin.Mac 앱 및 탐색된 Xamarin.Mac 및 Objective c의 관계에 검토
