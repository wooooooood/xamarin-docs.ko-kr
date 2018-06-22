---
title: 바인딩이 통합된 API로 마이그레이션
description: 이 문서에서는 Xamarin.IOS 및 Xamarin.Mac 응용 프로그램에 통합 된 Api를 지원 하기 위해 기존 Xamarin 바인딩 프로젝트를 업데이트 하는 데 필요한 단계에 설명 합니다.
ms.prod: xamarin
ms.assetid: 5E2A3251-D17F-4F9C-9EA0-6321FEBE8577
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 2d57f27bf0d3aaa2a7ba14f23481a8f2bb2d87f2
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33918489"
---
# <a name="migrating-a-binding-to-the-unified-api"></a>바인딩이 통합된 API로 마이그레이션

_이 문서에서는 Xamarin.IOS 및 Xamarin.Mac 응용 프로그램에 통합 된 Api를 지원 하기 위해 기존 Xamarin 바인딩 프로젝트를 업데이트 하는 데 필요한 단계에 설명 합니다._

## <a name="overview"></a>개요

2015 년 2 월 1 일 시작 Apple 필요 iTunes 및 Mac 앱 스토어에 대 한 모든 새 전송은 64 비트 응용 프로그램 이어야 합니다. 결과적으로, 새로운 Xamarin.iOS 또는 Xamarin.Mac 응용 프로그램 수를 사용 하 여 새 통합 API 기존 클래식 MonoTouch 및 MonoMac Api 대신 64 비트 지원 해야 합니다.

또한 Xamarin 바인딩 프로젝트는 64 비트 Xamarin.iOS 또는 Xamarin.Mac 프로젝트에 포함 시킬 새 통합 Api를도 지원 해야 합니다. 이 문서는 통합 API를 사용 하도록 기존 바인딩 프로젝트를 업데이트 하는 데 필요한 단계를 설명 합니다.

## <a name="requirements"></a>요구 사항

이 문서에 제공 된 단계를 완료 하려면 다음이 요구 됩니다.

 -  **Mac 용 visual Studio** -Mac 용 Visual Studio의 최신 버전 설치 하 고 개발 컴퓨터에 구성 합니다.
 -  **Apple Mac** Apple mac은 iOS에 대 한 바인딩 프로젝트를 작성할 필요가 및 Mac.-

바인딩 프로젝트가 Windows 컴퓨터에서 Visual studio에서 지원 되지 않습니다.

## <a name="modify-the-using-statements"></a>사용 하 여 문을 수정 합니다.

통합 Api 쉽게 그 어느 때 보다 Mac 및 iOS로 있도록 동일한 32와 64 비트 응용 프로그램을 지원 하기 위해 이진 사이 코드를 공유할 수 있습니다. 삭제 하 여는 _MonoMac_ 및 _MonoTouch_ 접두사는 네임 스페이스에서 간단한 공유에 걸쳐 균일 Xamarin.Mac Xamarin.iOS 및 응용 프로그램 프로젝트입니다.

결과적으로 우리의 바인딩 계약을 수정 해야 합니다 (및 기타 `.cs` 바인딩 프로젝트에 파일)를 제거 하는 _MonoMac_ 및 _MonoTouch_ 에서 접두사 우리의 `using` 문입니다.

예를 들어 다음 문을 사용 하 여 바인딩 계약에서:

```csharp
using System;
using System.Drawing;
using MonoTouch.Foundation;
using MonoTouch.UIKit;
using MonoTouch.ObjCRuntime;
```

제거 하는 것은 `MonoTouch` 다음 그 결과 접두사:

```csharp
using System;
using System.Drawing;
using Foundation;
using UIKit;
using ObjCRuntime;
```

다시 모든에 대해이 작업을 수행 해야 합니다 `.cs` 바인딩 프로젝트에는 파일입니다. 위치에이 변경으로 다음 단계는 새 네이티브 데이터 형식을 사용 하 여 바인딩 프로젝트를 업데이트할 것입니다.

통합 API에 대 한 자세한 내용은 참조 하십시오는 [통합 API](~/cross-platform/macios/unified/index.md) 설명서입니다. 자세한 배경을 알고 싶으면 32와 64 비트 응용 프로그램 및 프레임 워크에 대 한 정보를 지원에 대 한 참조는 [32, 64 비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64/index.md) 설명서입니다.

## <a name="update-to-native-data-types"></a>네이티브 데이터 형식에 대 한 업데이트

Objective C 매핑하는 `NSInteger` 데이터 형식을 `int32_t` 32 비트 시스템 및 `int64_t` 64 비트 시스템에서 합니다. 이 문제를 찾으려면 새 통합 API는 이전을 사용 하는 대체 `int` (.net에서으로 정의 되 고 항상 `System.Int32`)을 새 데이터 형식: `System.nint`합니다.

새 함께 `nint` 데이터 형식은 통합 API 소개는 `nuint` 및 `nfloat` 형식에 매핑하는 데는 `NSUInteger` 및 `CGFloat` 형식 에서도 합니다.

위의 설명을 고려 하면, 해야 API를 검토 하 고의 모든 인스턴스 않도록 `NSInteger`, `NSUInteger` 및 `CGFloat` 것에 대해 이전에 매핑된 `int`, `uint` 및 `float` 새으로 업데이트 되기를 `nint`, `nuint` 및 `nfloat` 형식입니다.

예를 들어 다음과 같습니다.의 Objective-c 메서드 정의

```csharp
-(NSInteger) add:(NSInteger)operandUn and:(NSInteger) operandDeux;
```

이전 바인딩 계약에는 다음 정의가:

```csharp
[Export("add:and:")]
int Add(int operandUn, int operandDeux);
```

새 바인딩의 업데이트할 것:

```csharp
[Export("add:and:")]
nint Add(nint operandUn, nint operandDeux);
```
무엇에 처음 연결 되는를 보다 최신 버전 3rd 파티 라이브러리에 매핑하는 것을 검토 해야는 `.h` 라이브러리 및 참조 있는 경우 헤더 파일에 대 한 기존, 명시적 호출 `int`, `int32_t`, `unsigned int`, `uint32_t` 또는 `float` 수로 업그레이된는 `NSInteger`, `NSUInteger` 또는 `CGFloat`합니다. 이 경우 수정 하는 `nint`, `nuint` 및 `nfloat` 가 매핑을 변경 해야 하 종류가 필요 합니다.

이러한 데이터 형식 변경 하는 방법에 대 한 자세한 내용은 참조는 [네이티브 형식](~/cross-platform/macios/nativetypes.md) 문서.

## <a name="update-the-coregraphics-types"></a>CoreGraphics 형식 업데이트

함께 사용 되는 포인트, 크기 및 사각형 데이터 형식을 `CoreGraphics` 에서 실행 중인 장치에 따라 32 또는 64 비트를 사용 합니다. Xamarin iOS 및 Mac Api 원래 바인딩된 때 기존 데이터 구조에 속하는 데이터 형식에 맞게 발생 사용한 `System.Drawing` (`RectangleF` 예를 들어).

64 비트 및 새로운 네이티브 데이터 형식을 지원 하기 위해 요구 사항 때문에 다음 조정 작업은 기존 코드를 호출할 때 변경 해야 하 필요 합니다 `CoreGraphic` 메서드:

- **CGRect** -사용 하 여 `CGRect` 대신 `RectangleF` 때 사각형 영역의 소수점 부동 정의 합니다.
- **CGSize** -사용 하 여 `CGSize` 대신 `SizeF` 부동 포인트 크기 (너비와 높이)을 정의 하는 경우.
- **CGPoint** -사용 하 여 `CGPoint` 대신 `PointF` 때 부동 정의 지점 (X 및 Y 좌표) 위치입니다.

API를 검토 하 고의 모든 인스턴스를 확인 하려면 필요 합니다 위의 설명을 고려 하면, `CGRect`, `CGSize` 또는 `CGPoint` 를 이전에 바인딩된 `RectangleF`, `SizeF` 또는 `PointF` 네이티브 형식으로 변경할 수 `CGRect`, `CGSize` 또는 `CGPoint` 직접 합니다.

예를 들어 다음과 같습니다.의 Objective-c 이니셜라이저

```csharp
- (id)initWithFrame:(CGRect)frame;
```

다음 코드를 포함 하는 이전 바인딩에 하는 경우:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (RectangleF frame);

```

해당 코드를 업데이트 합니다.

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);

```

모든 코드 변경 내용을 했으니에서, 우리의 바인딩 프로젝트를 수정 하거나 통합 Api에 대해 바인딩할 파일을 확인 해야 합니다.

## <a name="modify-the-binding-project"></a>바인딩 프로젝트 수정

수정 하거나 해야 통합 Api를 사용 하 여 바인딩 프로젝트를 업데이트 하는 마지막 단계는 `MakeFile` 프로젝트를 빌드하거나 Xamarin 프로젝트 형식 (경우 Mac 용 Visual Studio 내에서 바인딩하는 것) 하 고 지시를 사용 하는 _btouch_  클래식 것 대신 통합 Api에 대해 바인딩할 합니다.


### <a name="updating-a-makefile"></a>메이크파일의 업데이트

메이크파일을 사용 하는 Xamarin에 우리의 바인딩 프로젝트를 빌드할 수는 합니다. DLL의 경우 수집 해야 포함는 `--new-style` 명령줄 옵션과 호출 `btouch-native` 대신 `btouch`합니다.

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

호출에서 전환 해야 `btouch` 를 `btouch-native`이므로 다음과 같은 매크로 정의 조정 합니다.

```csharp
BTOUCH=/Developer/MonoTouch/usr/bin/btouch-native
```

에 대 한 호출 업데이트할 것 `btouch` 추가 `--new-style` 다음과 같은 옵션:

```csharp
XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe --new-style -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a
```

이제 실행 우리의 `MakeFile` API의 새로운 64 비트 버전을 빌드하려면 정상적으로 합니다.

### <a name="updating-a-binding-project-type"></a>바인딩 프로젝트 형식 업데이트

API를 빌드하려면 Mac 바인딩 프로젝트 템플릿에 대 한 Visual Studio를 사용 하는 경우 바인딩 프로젝트 템플릿의 새 통합 API 버전으로 업데이트 해야 합니다. 이 작업을 수행 하는 가장 쉬운 방법은 다시 모든 기존 코드와 설정을 새 통합 API 바인딩 프로젝트 및 복사를 시작 하는 합니다.

다음을 수행합니다.

1. Mac.에 대 한 Visual Studio를 시작 합니다.
2. 선택 **파일** > **새** > **솔루션 중...**
3. 새 솔루션 대화 상자에서 선택 **iOS** > **통합 API** > **iOS 프로젝트 바인딩**: 

    [![](update-binding-images/image01new.png "새 솔루션 대화 상자에서 선택 iOS / 통합 API / iOS 바인딩 프로젝트")](update-binding-images/image01new.png#lightbox)
4. '새 프로젝트 구성' 대화 상자에서 다음을 입력 한 **이름** 클릭 확인 하 고 새 바인딩 프로젝트에 대 한는 **확인** 단추입니다.
5. 64 비트 버전에 대 한 바인딩을 만들 할 Objective C 라이브러리의를 포함 합니다.
6. 기존 32 비트 클래식 API 바인딩 프로젝트의 소스 코드를 통해 복사 (예:는 `ApiDefinition.cs` 및 `StructsAndEnums.cs` 파일).
7. 소스 코드 파일에 위의 표에 나와 변경 내용을 확인 하십시오.

모든 위치에서 이러한 변경 내용을 사용 하 여 32 비트 버전와 마찬가지로 새 64 비트 버전의 API 빌드할 수 있습니다.

## <a name="summary"></a>요약

이 문서에서 새 통합 Api 및 64 비트 장치를 지원 하기 위해 기존 Xamarin 바인딩 프로젝트 변경 해야 하는 변경 사항을 소개 했지만 및 새 64 비트 호환 버전의 API 빌드하는 데 필요한 단계입니다.



## <a name="related-links"></a>관련 링크

- [IOS 및 Mac](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/nativetypes.md)
- [32/64 비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64/index.md)
- [기존 iOS 앱 업그레이드](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [BindingSample](https://developer.xamarin.com/samples/monotouch/BindingSample/)
