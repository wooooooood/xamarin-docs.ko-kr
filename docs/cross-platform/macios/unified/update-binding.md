---
title: 바인딩을 Unified API로 마이그레이션
description: 이 문서에서는 Xamarin.ios 및 Xamarin.ios 응용 프로그램에 대 한 통합 Api를 지원 하도록 기존 Xamarin 바인딩 프로젝트를 업데이트 하는 데 필요한 단계를 설명 합니다.
ms.prod: xamarin
ms.assetid: 5E2A3251-D17F-4F9C-9EA0-6321FEBE8577
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: c8f55dd2d300da80a57c06f15cf185558cfc5e41
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73015040"
---
# <a name="migrating-a-binding-to-the-unified-api"></a>바인딩을 Unified API로 마이그레이션

_이 문서에서는 Xamarin.ios 및 Xamarin.ios 응용 프로그램에 대 한 통합 Api를 지원 하도록 기존 Xamarin 바인딩 프로젝트를 업데이트 하는 데 필요한 단계를 설명 합니다._

## <a name="overview"></a>개요

2 월 1 일부 터 2015를 시작 하려면 iTunes 및 Mac App Store에 대 한 모든 새 제출이 64 비트 응용 프로그램 이어야 합니다. 따라서 새 Xamarin.ios 또는 Xamarin.ios 응용 프로그램은 64 비트를 지원 하기 위해 기존 클래식 Monotouch.dialog 및 MonoMac Api 대신 새 Unified API을 사용 해야 합니다.

또한 모든 Xamarin 바인딩 프로젝트는 64 비트 Xamarin.ios 또는 Xamarin.ios 프로젝트에 포함 될 새 통합 Api를 지원 해야 합니다. 이 문서에서는 Unified API 사용 하도록 기존 바인딩 프로젝트를 업데이트 하는 데 필요한 단계에 대해 설명 합니다.

## <a name="requirements"></a>요구 사항

이 문서에 제공 된 단계를 완료 하려면 다음이 필요 합니다.

- **Mac용 Visual Studio** -개발 컴퓨터에 설치 및 구성 된 최신 버전의 Mac용 Visual Studio입니다.
- **Apple mac** -IOS 및 Mac에 대 한 바인딩 프로젝트를 빌드하려면 apple mac이 필요 합니다.

Windows 컴퓨터의 Visual studio에서는 바인딩 프로젝트가 지원 되지 않습니다.

## <a name="modify-the-using-statements"></a>Using 문 수정

통합 Api를 사용 하면 Mac과 iOS 간에 코드를 보다 쉽게 공유할 수 있을 뿐만 아니라 동일한 이진 파일을 사용 하 여 32 및 64 비트 응용 프로그램을 지원할 수 있습니다. 네임 스페이스에서 _MonoMac_ 및 _monotouch.dialog_ 접두사를 삭제 하면 xamarin.ios 및 xamarin.ios 응용 프로그램 프로젝트에서 더 간단한 공유를 얻을 수 있습니다.

따라서 `using` 문에서 _MonoMac_ 및 _monotouch.dialog_ 접두사를 제거 하려면 바인딩 계약과 바인딩 프로젝트의 기타 `.cs` 파일을 수정 해야 합니다.

예를 들어, 바인딩 계약에서 다음 using 문을 지정 합니다.

```csharp
using System;
using System.Drawing;
using MonoTouch.Foundation;
using MonoTouch.UIKit;
using MonoTouch.ObjCRuntime;
```

`MonoTouch` 접두사를 제거 하면 다음과 같은 결과가 발생 합니다.

```csharp
using System;
using System.Drawing;
using Foundation;
using UIKit;
using ObjCRuntime;
```

다시, 바인딩 프로젝트의 모든 `.cs` 파일에 대해이 작업을 수행 해야 합니다. 이 변경 내용이 적용 되 면 다음 단계는 새 네이티브 데이터 형식을 사용 하도록 바인딩 프로젝트를 업데이트 하는 것입니다.

Unified API에 대 한 자세한 내용은 [Unified API](~/cross-platform/macios/unified/index.md) 설명서를 참조 하세요. 32 및 64 비트 응용 프로그램 지원에 대 한 배경 및 프레임 워크에 대 한 자세한 내용은 [32 및 64 비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64/index.md) 설명서를 참조 하세요.

## <a name="update-to-native-data-types"></a>네이티브 데이터 형식으로 업데이트

목적-C는 32 비트 시스템의 `int32_t`에 `NSInteger` 데이터 형식을 매핑하고 64 비트 시스템에서 `int64_t` 합니다. 이 동작을 일치 시키기 위해 새로운 Unified API `int` (.NET에서는 항상 `System.Int32`로 정의 됨)를 새 데이터 형식으로 대체 합니다. `System.nint`.

새 `nint` 데이터 형식과 함께 Unified API는 `NSUInteger` 및 `CGFloat` 형식에 매핑하기 위한 `nuint` 및 `nfloat` 형식을 소개 합니다.

위의 내용을 고려 하 여 API를 검토 하 고 이전에 `int`에 매핑한 `NSInteger`, `NSUInteger` 및 `CGFloat` 인스턴스를 새 `uint`으로 업데이트 해야 `float` , `nuint` 및 `nfloat` 형식.

예를 들어 다음과 같은 목표-C 메서드 정의를 가정 합니다.

```csharp
-(NSInteger) add:(NSInteger)operandUn and:(NSInteger) operandDeux;
```

이전 바인딩 계약의 정의가 다음과 같은 경우:

```csharp
[Export("add:and:")]
int Add(int operandUn, int operandDeux);
```

새 바인딩을 다음과 같이 업데이트 합니다.

```csharp
[Export("add:and:")]
nint Add(nint operandUn, nint operandDeux);
```

이전에 연결한 것 보다 최신 버전의 타사 라이브러리에 매핑하는 경우 라이브러리에 대 한 `.h` 헤더 파일을 검토 하 고 `int`, `int32_t`에 대 한 명시적 호출이 있는지 확인 해야 `unsigned int`, `uint32_t` 또는 `float` `NSInteger`, `NSUInteger` 또는 `CGFloat`로 업그레이드 되었습니다. 그렇다면 `nint`, `nuint` 및 `nfloat` 형식도 동일한 수정 작업을 수행 해야 합니다.

이러한 데이터 형식 변경에 대해 자세히 알아보려면 [네이티브 형식](~/cross-platform/macios/nativetypes.md) 문서를 참조 하세요.

## <a name="update-the-coregraphics-types"></a>CoreGraphics 형식 업데이트

`CoreGraphics` 사용 되는 점, 크기 및 사각형 데이터 형식은 실행 중인 장치에 따라 32 또는 64 비트를 사용 합니다. Xamarin에서 원래 iOS 및 Mac Api를 바인딩한 경우 `System.Drawing`의 데이터 형식과 일치 하는 기존 데이터 구조를 사용 했습니다 (예:`RectangleF`).

64 비트와 새로운 네이티브 데이터 형식을 지원 하기 위한 요구 사항으로 인해 `CoreGraphic` 메서드를 호출할 때 기존 코드를 다음과 같이 조정 해야 합니다.

- **CGRect** -부동 소수점 사각형 영역을 정의할 때 `RectangleF` 대신 `CGRect`를 사용 합니다.
- **CGSize** -부동 소수점 크기 (너비 및 높이)를 정의할 때 `SizeF` 대신 `CGSize`를 사용 합니다.
- **CGPoint** -부동 소수점 위치 (X 및 Y 좌표)를 정의할 때 `PointF` 대신 `CGPoint`를 사용 합니다.

위의 내용을 고려 하 여 API를 검토 하 고 이전에 `RectangleF`에 바인딩된 `CGRect`, `CGSize` 또는 `CGPoint` 인스턴스를 네이티브 형식으로 변경 해야 `SizeF` , `CGSize` 또는 `CGPoint` 직접.

예를 들어의 목적-C 이니셜라이저가 제공 됩니다.

```csharp
- (id)initWithFrame:(CGRect)frame;
```

이전 바인딩에는 다음 코드가 포함 되어 있습니다.

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (RectangleF frame);

```

해당 코드를 다음으로 업데이트 합니다.

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);

```

이제 모든 코드를 변경 하 여 바인딩 프로젝트를 수정 하거나 통합 된 Api에 바인딩하기 위해 파일을 만들어야 합니다.

## <a name="modify-the-binding-project"></a>바인딩 프로젝트 수정

통합 Api를 사용 하도록 바인딩 프로젝트를 업데이트 하는 마지막 단계로, 프로젝트를 빌드하는 데 사용 하는 `MakeFile`를 수정 하거나 (Mac용 Visual Studio 내에서 바인딩하는 경우), 클래식 Api 대신 통합 된 Api입니다.

### <a name="updating-a-makefile"></a>메이크파일 업데이트

메이크파일을 사용 하 여 Xamarin에 바인딩 프로젝트를 빌드하는 경우 DLL에서는 `--new-style` 명령줄 옵션을 포함 하 고 `btouch`대신 `btouch-native`를 호출 해야 합니다.

따라서 다음 `MakeFile`제공 됩니다.

<!--markdownlint-disable MD010 -->
```makefile
BINDDIR=/src/binding
XBUILD=/Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild
PROJECT_ROOT=XMBindingLibrarySample
PROJECT=$(PROJECT_ROOT)/XMBindingLibrarySample.xcodeproj
TARGET=XMBindingLibrarySample
BTOUCH=/Developer/MonoTouch/usr/bin/btouch

all: XMBindingLibrary.dll

libXMBindingLibrarySample-i386.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphonesimulator -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphonesimulator/lib$(TARGET).a $@

libXMBindingLibrarySample-arm64.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch arm64 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySample-armv7.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch armv7 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySampleUniversal.a: libXMBindingLibrarySample-armv7.a libXMBindingLibrarySample-i386.a libXMBindingLibrarySample-arm64.a
    lipo -create -output $@ $^

XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a

clean:
    -rm -f *.a *.dll
```
<!--markdownlint-enable MD010 -->

`btouch` 호출에서 `btouch-native`로 전환 해야 하므로 다음과 같이 매크로 정의를 조정 합니다.

```makefile
BTOUCH=/Developer/MonoTouch/usr/bin/btouch-native
```

`btouch`에 대 한 호출을 업데이트 하 고 `--new-style` 옵션을 다음과 같이 추가 합니다.

<!--markdownlint-disable MD010 -->
```makefile
XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe --new-style -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a
```
<!--markdownlint-enable MD010 -->

이제 `MakeFile`를 정상적으로 실행 하 여 API의 새 64 비트 버전을 빌드할 수 있습니다.

### <a name="updating-a-binding-project-type"></a>바인딩 프로젝트 형식 업데이트

Mac용 Visual Studio 바인딩 프로젝트 템플릿을 사용 하 여 API를 빌드하는 경우 새 Unified API 버전의 바인딩 프로젝트 템플릿으로 업데이트 해야 합니다. 이 작업을 수행 하는 가장 쉬운 방법은 새 Unified API 바인딩 프로젝트를 시작 하 고 모든 기존 코드 및 설정을 복사 하는 것입니다.

다음을 수행합니다.

1. Mac용 Visual Studio를 시작합니다.
2. **파일** > **새** > **솔루션** ...을 선택 합니다.
3. 새 솔루션 대화 상자에서 **ios > ** **Unified API** > **ios 바인딩 프로젝트**를 선택 합니다. 

    [![](update-binding-images/image01new.png "In the New Solution Dialog Box, select iOS / Unified API / iOS Binding Project")](update-binding-images/image01new.png#lightbox)
4. ' 새 프로젝트 구성 ' 대화 상자에서 새 바인딩 프로젝트의 **이름을** 입력 하 고 **확인** 단추를 클릭 합니다.
5. 바인딩을 만들려는 64 비트 버전의 목표-C 라이브러리를 포함 합니다.
6. `ApiDefinition.cs` 및 `StructsAndEnums.cs` 파일과 같은 기존 32 비트 Classic API 바인딩 프로젝트에서 소스 코드를 복사 합니다.
7. 소스 코드 파일에 대해 위에서 언급 한 변경을 수행 합니다.

이러한 모든 변경 내용을 적용 하 여 32 비트 버전과 마찬가지로 새 64 비트 버전의 API를 빌드할 수 있습니다.

## <a name="summary"></a>요약

이 문서에서는 새로운 통합 Api 및 64 비트 장치를 지원 하기 위해 기존 Xamarin 바인딩 프로젝트에 적용 해야 하는 변경 내용과 새 64 비트 호환 버전의 API를 빌드하는 데 필요한 단계를 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [Mac 및 iOS](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/nativetypes.md)
- [32/64 비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64/index.md)
- [기존 iOS 앱 업그레이드](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [BindingSample](https://docs.microsoft.com/samples/xamarin/ios-samples/bindingsample/)
