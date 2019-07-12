---
title: 바인딩을 Unified API로 마이그레이션
description: 이 문서에서는 Xamarin.IOS 및 Xamarin.Mac 응용 프로그램에 대 한 통합 Api를 지원 하도록 기존 Xamarin 바인딩 프로젝트를 업데이트 하는 데 필요한 단계를 설명 합니다.
ms.prod: xamarin
ms.assetid: 5E2A3251-D17F-4F9C-9EA0-6321FEBE8577
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: f081ccda507fe3fe65af0e2fb50841aecd7b3c23
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830460"
---
# <a name="migrating-a-binding-to-the-unified-api"></a>바인딩을 Unified API로 마이그레이션

_이 문서에서는 Xamarin.IOS 및 Xamarin.Mac 응용 프로그램에 대 한 통합 Api를 지원 하도록 기존 Xamarin 바인딩 프로젝트를 업데이트 하는 데 필요한 단계를 설명 합니다._

## <a name="overview"></a>개요

2015 년 2 월 1 일부 터 시작 Apple iTunes 및 Mac 앱 스토어에 대 한 모든 새 전송 해야 합니다 64 비트 응용 프로그램을 되어 있어야 합니다. 결과적으로, 새 Xamarin.iOS 또는 Xamarin.Mac 응용 프로그램은 64 비트를 지원 하기 위해 기존 클래식 MonoTouch 및 MonoMac Api 대신 새 통합 API를 사용 해야 합니다.

또한 모든 Xamarin 바인딩 프로젝트는 64 비트 Xamarin.iOS 또는 Xamarin.Mac 프로젝트에 포함할 새 통합 Api를도 지원 해야 합니다. 이 문서에서는 통합 API를 사용 하는 기존 바인딩 프로젝트를 업데이트 하는 데 필요한 단계를 설명 합니다.

## <a name="requirements"></a>요구 사항

이 문서에 제공 된 단계를 완료 하려면 다음이 요구 됩니다.

- **Mac 용 visual Studio** -Mac 용 Visual Studio의 최신 버전 설치 및 개발 컴퓨터에서 구성 합니다.
- **Apple Mac** -Apple iOS에 대 한 바인딩 프로젝트를 빌드하려면 mac이 필요 하 고 Mac.

바인딩 프로젝트는 Windows 컴퓨터에서 Visual studio에서 지원 되지 않습니다.

## <a name="modify-the-using-statements"></a>사용 하 여 문을 수정 합니다.

Unified Api를 쉽게 그 어느 때 보다 이진 같은 32 비트 및 64 비트 응용 프로그램을 지원할 수 있도록 뿐만 아니라 Mac 및 iOS 간에 코드 공유. 삭제 하 여 합니다 _MonoMac_ 하 고 _MonoTouch_ 접두사 네임 스페이스에서 간단한 공유 걸쳐 균일 하 게 Xamarin.iOS 및 Xamarin.Mac 응용 프로그램 프로젝트입니다.

결과적으로 바인딩 계약 중 하나를 수정 해야 합니다 (및 기타 `.cs` 바인딩 프로젝트의 파일)를 제거 하는 _MonoMac_ 및 _MonoTouch_ 에서 접두사는 `using` 문입니다.

예를 들어, 다음 지정 된 바인딩 계약의 문을 사용 하 여:

```csharp
using System;
using System.Drawing;
using MonoTouch.Foundation;
using MonoTouch.UIKit;
using MonoTouch.ObjCRuntime;
```

제거 하는 것은 `MonoTouch` 접두사는 다음과 같은 결과:

```csharp
using System;
using System.Drawing;
using Foundation;
using UIKit;
using ObjCRuntime;
```

에 대 한이 작업을 수행 해야 다시 `.cs` 바인딩 프로젝트의 파일입니다. 이 변경 된 다음 단계 새로운 네이티브 데이터 형식을 사용 하 여 바인딩 프로젝트를 업데이트 합니다.

Unified API에 대 한 자세한 내용은 참조 하십시오 합니다 [Unified API](~/cross-platform/macios/unified/index.md) 설명서. 32 비트 및 64 비트 응용 프로그램 및 프레임 워크에 대 한 정보를 지원 하기 위해 자세한 배경 정보에 대 한 참조를 [32 및 64 비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64/index.md) 설명서.

## <a name="update-to-native-data-types"></a>네이티브 데이터 형식 업데이트

Objective-c로 매핑하는 `NSInteger` 데이터 형식을 `int32_t` 32 비트 시스템 및 `int64_t` 64 비트 시스템에서. 이 동작은 일치를 새 통합 API는 이전을 사용 하는 대체 `int` (항상으로 정의 되는.NET `System.Int32`)을 새 데이터 형식: `System.nint`합니다.

새 함께 `nint` 데이터 형식은 통합 API 소개를 `nuint` 및 `nfloat` 형식에 매핑할 수는 `NSUInteger` 및 `CGFloat` 형식도 합니다.

API를 검토 하 고는의 모든 인스턴스를 확인 하려면 먼저 위의 들어 `NSInteger`, `NSUInteger` 하 고 `CGFloat` 것에 대해 이전에 매핑된 `int`, `uint` 및 `float` 새 업데이트 `nint`, `nuint` 고 `nfloat` 형식입니다.

예를 들어 다음과 같습니다.는 Objective-c 메서드 정의

```csharp
-(NSInteger) add:(NSInteger)operandUn and:(NSInteger) operandDeux;
```

다음 정의 이전 바인딩 계약 있으면:

```csharp
[Export("add:and:")]
int Add(int operandUn, int operandDeux);
```

새 바인딩의 업데이트할 것:

```csharp
[Export("add:and:")]
nint Add(nint operandUn, nint operandDeux);
```
검토 해야 최신 버전 타사 라이브러리를 새로운 것가 처음에 연결 하는 보다에 매핑하는 것을 하는 경우는 `.h` 라이브러리에 있는 경우 참조 헤더 파일에 대 한 기존, 명시적 호출 `int`, `int32_t`, `unsigned int`, `uint32_t` 또는 `float` 되도록 업그레이된는 `NSInteger`를 `NSUInteger` 또는 `CGFloat`합니다. 그렇다면 수정 사항은 합니다 `nint`, `nuint` 및 `nfloat` 형식 매핑과 수 해야 합니다.

이러한 데이터 형식 변경에 대 한 자세한 내용은 참조는 [네이티브 형식을](~/cross-platform/macios/nativetypes.md) 문서.

## <a name="update-the-coregraphics-types"></a>CoreGraphics 형식 업데이트

사용 되는 지점, 크기 및 사각형 데이터 형식 `CoreGraphics` 실행 중인 장치에 따라 32 또는 64 비트를 사용 합니다. Xamarin iOS 및 Mac Api 원래 바인딩될 때 발생 한 데이터 형식과 일치 하는 기존 데이터 구조를 사용 했습니다 `System.Drawing` (`RectangleF` 예를 들어).

64 비트 및 새로운 네이티브 데이터 형식을 지원 하기 위해 요구 사항으로 인해를 호출할 때 기존 코드에 대해 다음과 같이 조정 해야 `CoreGraphic` 메서드:

- **Cgrect는 크기** -사용 `CGRect` 대신 `RectangleF` 경우 사각형 영역을 가리키고 부동 정의 합니다.
- **CGSize** -사용 `CGSize` 대신 `SizeF` 부동 포인트 크기 (너비 및 높이)을 정의 하는 경우.
- **CGPoint** -사용 `CGPoint` 대신 `PointF` 경우 부동 정의 지점 (X 및 Y 좌표) 위치입니다.

위의 들어 해야 API를 검토 하 고 모든 인스턴스의 했는지 `CGRect`, `CGSize` 또는 `CGPoint` 는 이전에 바인딩된 `RectangleF`에 `SizeF` 또는 `PointF` 네이티브 형식으로 변경할 수 `CGRect`, `CGSize` 또는 `CGPoint` 직접.

예를 들어 다음과 같습니다.의 Objective-c 이니셜라이저

```csharp
- (id)initWithFrame:(CGRect)frame;
```

다음 코드를 포함 하는 이전 바인딩의:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (RectangleF frame);

```

해당 코드를 업데이트 했습니다.

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);

```

모든 했으니 이제에서 코드 변경 내용을 사용 하 여 바인딩 프로젝트를 수정 하거나 통합 Api에 대해 바인딩할 파일을 확인 해야 합니다.

## <a name="modify-the-binding-project"></a>바인딩 프로젝트 수정

Unified Api를 사용 하 여 바인딩 프로젝트를 업데이트 하는 중에 마지막 단계로 하거나 수정 해야 합니다 `MakeFile` 프로젝트를 빌드하거나 Xamarin 프로젝트 형식 (Mac 용 Visual Studio 내에서 바인딩하는 것) 하는 경우 및 지시 하는 데 사용할 _u c h_  클래식 것 대신 통합 Api에 대해 바인딩할 합니다.


### <a name="updating-a-makefile"></a>메이크파일의 업데이트 하는 중

메이크파일을 사용 하는 Xamarin에 바인딩 프로젝트를 빌드할 수입니다. DLL은 포함 해야 합니다 `--new-style` 명령줄 옵션과 호출 `btouch-native` 대신 `btouch`합니다.

따라서 다음 지정 된 `MakeFile`:

```csharp
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

호출에서 전환 하려면 먼저 `btouch` 에 `btouch-native`이므로에서는 매크로 정의 다음과 같이 조정 됩니다.

```csharp
BTOUCH=/Developer/MonoTouch/usr/bin/btouch-native
```

호출을 업데이트할 것 `btouch` 추가 된 `--new-style` 다음과 같은 옵션:

```csharp
XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe --new-style -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a
```

이제 실행할 수 있습니다이 `MakeFile` API의 새로운 64 비트 버전을 빌드하려면 정상적으로 합니다.

### <a name="updating-a-binding-project-type"></a>바인딩 프로젝트 유형을 업데이트

API를 빌드하려면 Mac 바인딩 프로젝트 템플릿에 대 한 Visual Studio를 사용 하는 경우 바인딩 프로젝트 템플릿의 새 통합 API 버전으로 업데이트 해야 합니다. 이 작업을 수행 하는 가장 쉬운 방법은 모든 기존 코드와 설정을 통해 새 통합 API 바인딩 프로젝트 및 복사를 시작 하는 경우

다음을 수행합니다.

1. Mac 용 Visual Studio를 시작 합니다.
2. 선택 **파일** > **새** > **솔루션...**
3. 새 솔루션 대화 상자에서 선택 **iOS** > **Unified API** > **iOS 프로젝트 바인딩**: 

    [![](update-binding-images/image01new.png "새 솔루션 대화 상자에서 iOS를 선택 / 통합 API / iOS 바인딩 프로젝트")](update-binding-images/image01new.png#lightbox)
4. '새 프로젝트 구성' 대화 상자에서 입력을 **이름** 새 바인딩 프로젝트를 클릭 합니다 **확인** 단추.
5. 에 대 한 바인딩을 만들 하려고 하는 Objective C 라이브러리의 64 비트 버전을 포함 합니다.
6. 기존 32 비트 클래식 API 바인딩 프로젝트에서 소스 코드를 복사 (같은 합니다 `ApiDefinition.cs` 고 `StructsAndEnums.cs` 파일).
7. 소스 코드 파일에 기록 된 변경을 확인 합니다.

모든 위치에서 이러한 변경 내용을 사용 하 여 32 비트 버전과 마찬가지로 새 64 비트 버전의 API 빌드할 수 있습니다.

## <a name="summary"></a>요약

이 문서의 새 통합 Api 및 64 비트 장치를 지원 하도록 기존 Xamarin 바인딩 프로젝트에 대 한 필요한 변경 내용을 설명 했습니다. 새 64 비트 호환 되는 버전의 API 빌드하는 데 필요한 단계를



## <a name="related-links"></a>관련 링크

- [Mac 및 iOS](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/nativetypes.md)
- [32/64 비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64/index.md)
- [기존 iOS 앱 업그레이드](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [BindingSample](https://developer.xamarin.com/samples/monotouch/BindingSample/)
