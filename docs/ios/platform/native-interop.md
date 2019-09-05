---
title: Xamarin.ios에서 네이티브 라이브러리 참조
description: 이 문서에서는 네이티브 C 라이브러리를 Xamarin.ios 응용 프로그램에 연결 하는 방법을 설명 합니다. 유니버설 네이티브 라이브러리를 빌드하고에서 C#C 메서드에 액세스 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 1DA80280-E78A-EC4B-8673-C249C8425CF5
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/28/2016
ms.openlocfilehash: 75180152c3ed7056102038b9019f8017183c17ee
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70279942"
---
# <a name="referencing-native-libraries-in-xamarinios"></a>Xamarin.ios에서 네이티브 라이브러리 참조

Xamarin.ios는 네이티브 C 라이브러리와 목적-C 라이브러리를 모두 사용 하 여 연결을 지원 합니다. 이 문서에서는 네이티브 C 라이브러리를 Xamarin.ios 프로젝트와 연결 하는 방법을 설명 합니다. 목적-C 라이브러리에 대해 동일한 작업을 수행 하는 방법에 대 한 자세한 내용은 [바인딩 목표-c 형식](~/ios/platform/binding-objective-c/index.md) 문서를 참조 하세요.

<a name="building_native" />

## <a name="building-universal-native-libraries-i386-armv7-and-arm64"></a>유니버설 네이티브 라이브러리 빌드 (i386, ARMv7 및 ARM64)

IOS 개발을 위해 지원 되는 각 플랫폼에 대 한 네이티브 라이브러리를 빌드하는 것이 좋습니다 (시뮬레이터의 경우 i386 및 장치 자체의 경우 ARMv7/ARM64). 라이브러리에 대 한 Xcode 프로젝트를 이미 가져온 경우이 작업은 정말 간단 합니다.

네이티브 라이브러리의 i386 버전을 빌드하려면 터미널에서 다음 명령을 실행 합니다.

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphonesimulator -arch i386 -configuration Release clean build
```

그러면에서 `MyProject.xcodeproj/build/Release-iphonesimulator/`네이티브 정적 라이브러리가 생성 됩니다. 라이브러리 보관 파일 (arm64)을 나중에 사용 하기 위해 안전한 위치에 복사 (또는 이동) 하 여 다음에 빌드할 동일한 라이브러리의 및 armv7 버전과 충돌 하지 않도록 해당 이름을 해당 이름 (예: **)에 제공 합니다.**

ARM64 버전의 네이티브 라이브러리를 빌드하려면 다음 명령을 실행 합니다.

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch arm64 -configuration Release clean build
```

이번에는 빌드된 네이티브 라이브러리가에 `MyProject.xcodeproj/build/Release-iphoneos/`배치 됩니다. 다시 한 번이 파일을 안전한 위치로 복사 (또는 이동) 하 고 **arm64** 와 같은 이름으로 변경 하 여 충돌 하지 않도록 합니다.

이제 라이브러리의 ARMv7 버전을 빌드합니다.

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch armv7 -configuration Release clean build
```

라이브러리의 다른 두 버전을 이동한 동일한 위치에 결과 라이브러리 파일을 복사 하거나 이동 하 여 라이브러리의 이름을 **armv7**같은 이름으로 바꿉니다.

유니버설 이진을 만들려면 다음과 같은 `lipo` 도구를 사용할 수 있습니다.

```bash
lipo -create -output libMyLibrary.a libMyLibrary-i386.a libMyLibrary-arm64.a libMyLibrary-armv7.a
```

이렇게 하면 `libMyLibrary.a` 모든 iOS 개발 대상에 사용 하기에 적합 한 유니버설 (fat) 라이브러리가 생성 됩니다.


### <a name="missing-required-architecture-i386"></a>필수 아키텍처 i386 누락

IOS 시뮬레이터에서 목표- `does not implement methodSignatureForSelector` C `does not implement doesNotRecognizeSelector` 라이브러리를 사용 하려고 할 때 런타임 출력에 또는 메시지가 표시 되는 경우 라이브러리는 i386 아키텍처에 대해 컴파일되지 않았을 수 있습니다 ( [유니버설 네이티브 빌드 참조). 라이브러리](#building_native) 섹션).

지정 된 라이브러리에서 지 원하는 아키텍처를 확인 하려면 터미널에서 다음 명령을 사용 합니다.

```bash
lipo -info /full/path/to/libraryname.a
```

여기서 `/full/path/to/` 는 사용 되는 라이브러리의 전체 경로이 고 `libraryname.a` 은 해당 라이브러리의 이름입니다.

라이브러리에 대 한 소스가 있는 경우 iOS 시뮬레이터에서 앱을 테스트 하려는 경우 i386 아키텍처에 대해서도이를 컴파일 및 번들 해야 합니다.

### <a name="linking-your-library"></a>라이브러리 연결

사용 하는 타사 라이브러리는 응용 프로그램에 정적으로 연결 해야 합니다. 

인터넷에서 가져오거나 Xcode를 사용 하 여 빌드한 라이브러리를 정적으로 연결 하려는 경우 다음 몇 가지 작업을 수행 해야 합니다.

- 라이브러리를 프로젝트로 가져오기
- 라이브러리를 연결 하는 Xamarin.ios 구성
- 라이브러리에서 메서드에 액세스 합니다.


**라이브러리를 프로젝트로 가져오려면**솔루션 탐색기에서 프로젝트를 선택 하 고 **Command + Option + a**를 누릅니다. 라이브러리를 찾아 프로젝트에 추가 합니다. 메시지가 표시 되 면 Mac용 Visual Studio 또는 Visual Studio에 프로젝트에 복사 하도록 지시 합니다. 프로젝트를 추가한 후 프로젝트에서 파일을 찾아 마우스 오른쪽 단추로 클릭 하 고 **빌드 작업** 을 **없음**으로 설정 합니다.

**라이브러리를 연결 하기 위해 xamarin.ios를 구성**하려면 **ios 빌드의**추가 인수 (프로젝트 옵션의 일부)에 추가 해야 하는 최종 실행 파일 (라이브러리 자체는 아님)에 대 한 프로젝트 옵션에 "-gcc_flags"를 추가 합니다. 다음 예제와 같이 프로그램에 필요한 모든 추가 라이브러리를 포함 하는 따옴표 붙은 문자열을 선택 합니다.

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load ${ProjectDir}/libMyLibrary.a"
```

위의 예제에서는 **라이브러리를 연결 합니다.**

를 사용 `-gcc_flags` 하 여 실행 파일의 최종 링크를 수행 하는 데 사용 되는 GCC 컴파일러에 전달할 명령줄 인수 집합을 지정할 수 있습니다. 예를 들어이 명령줄은 CFNetwork 프레임 워크도 참조 합니다.

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

네이티브 라이브러리가 코드를 포함 C++ 하는 경우 xamarin.ios가 올바른 컴파일러를 사용 하는 것을 알 수 있도록 "추가 인수"에-.cxx 플래그를 전달 해야 합니다. 위의 C++ 옵션은 다음과 같습니다.

```bash
-cxx -gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

<a name="Accessing_C_Methods_from_C#" />

## <a name="accessing-c-methods-from-c35"></a>C에서 C 메서드 액세스&#35;

IOS에서 사용할 수 있는 기본 라이브러리에는 두 가지 종류가 있습니다.

- 운영 체제에 포함 된 공유 라이브러리

- 응용 프로그램과 함께 제공 되는 정적 라이브러리입니다.


이러한 방법 중 하나에서 정의 된 메서드에 액세스 하려면 .NET에서 사용 하는 것과 동일한 기술인 [Mono의 P/Invoke 기능](https://www.mono-project.com/docs/advanced/pinvoke/) 을 사용 합니다 .이는 대략적으로 다음과 같이 사용 됩니다.

- 호출 하려는 C 함수 결정
- 서명 확인
- 상주 하는 라이브러리 확인
- 적절 한 P/Invoke 선언을 작성 합니다.

P/Invoke를 사용 하는 경우 연결 하는 라이브러리의 경로를 지정 해야 합니다. Ios 공유 라이브러리를 사용 하는 경우 경로를 하드 코딩 하거나에서 `Constants`정의한 편리한 상수를 사용할 수 있습니다. 이러한 상수는 ios 공유 라이브러리를 포함 해야 합니다.

예를 들어 C:에이 서명이 있는 Apple의 UIKit 라이브러리에서 UIRectFrameUsingBlendMode 메서드를 호출 하려는 경우

```csharp
void UIRectFrameUsingBlendMode (CGRect rect, CGBlendMode mode);
```

P/Invoke 선언은 다음과 같습니다.

```csharp
[DllImport (Constants.UIKitLibrary,EntryPoint="UIRectFrameUsingBlendMode")]
public extern static void RectFrameUsingBlendMode (RectangleF rect, CGBlendMode blendMode);
```

UIKitLibrary는 "/System/Library/Frameworks/UIKit.framework/UIKit"로 정의 된 상수 이며, EntryPoint를 사용 하 여 선택적으로 외부 이름 (UIRectFramUsingBlendMode)을 지정 하는 동시에 C#다른 이름을 노출할 수 있습니다. RectFrameUsingBlendMode 짧습니다.

<a name="Accessing_C_Dylibs" />

### <a name="accessing-c-dylibs"></a>C Dylibs 액세스

Xamarin.ios 응용 프로그램에서 C Dylib를 사용 해야 하는 경우 특성을 `DllImport` 호출 하기 전에 필요한 약간의 추가 설정이 있습니다.

예를 들어 응용 프로그램에서 호출할 `Animal.dylib` `Animal_Version` 메서드가 포함 된가 있는 경우이를 사용 하기 전에 라이브러리의 위치에 대해 xamarin.ios에 알려야 합니다.

이렇게 하려면 `Main.CS` 파일을 편집 하 여 다음과 같이 만듭니다.

```csharp
static void Main (string[] args)
{
    // Load Dylib
    MonoTouch.ObjCRuntime.Dlfcn.dlopen ("/full/path/to/Animal.dylib", 0);

    // Start application
    UIApplication.Main (args, null, "AppDelegate");
}
```

여기서 `/full/path/to/` 는 사용 되는 Dylib의 전체 경로입니다. 이 코드가 준비 되 면 다음과 같이 `Animal_Version` 메서드에 연결할 수 있습니다.

```csharp
[DllImport("Animal.dylib", EntryPoint="Animal_Version")]
public static extern double AnimalLibraryVersion();
```

<a name="Static_Libraries" />

### <a name="static-libraries"></a>정적 라이브러리

IOS에서 정적 라이브러리를 사용할 수 있기 때문에와 연결할 외부 공유 라이브러리가 없으므로 DllImport 특성의 path 매개 변수는 특수 이름을 `__Internal` 사용 해야 합니다 (이름의 시작 부분에 있는 이중 밑줄 문자를 참조). 경로 이름입니다.

이렇게 하면 DllImport는 공유 라이브러리에서 로드 하는 대신 현재 프로그램에서 참조 하는 메서드의 기호를 조회 합니다.

