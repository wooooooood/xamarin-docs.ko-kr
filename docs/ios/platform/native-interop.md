---
title: Xamarin.iOS에서 네이티브 라이브러리 참조
description: 이 문서에서는 Xamarin.iOS 응용 프로그램에 네이티브 C 라이브러리를 연결 하는 방법을 설명 합니다. 빌드 유니버설 네이티브 라이브러리 및 C 메서드에서 액세스 하는 방법을 설명 C#입니다.
ms.prod: xamarin
ms.assetid: 1DA80280-E78A-EC4B-8673-C249C8425CF5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/28/2016
ms.openlocfilehash: a94c70bb7068847ed1b410dd7eddc70921fdf307
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106041"
---
# <a name="referencing-native-libraries-in-xamarinios"></a>Xamarin.iOS에서 네이티브 라이브러리 참조

Xamarin.iOS는 네이티브 C 라이브러리와 Objective-c 라이브러리를 사용 하 여 연결을 지원 합니다. 이 문서에서는 Xamarin.iOS 프로젝트를 사용 하 여 네이티브 C 라이브러리를 연결 하는 방법을 설명 합니다. Objective-c 라이브러리에 대 한 동일한 내용은 참조 하세요. 당사의 [Objective-c 바인딩 형식](~/ios/platform/binding-objective-c/index.md) 문서.

<a name="building_native" />

## <a name="building-universal-native-libraries-i386-armv7-and-arm64"></a>(I386, ARMv7, 및 ARM64) 유니버설 네이티브 라이브러리 빌드

IOS 개발을 위해 지원 되는 플랫폼 각각에 대해 네이티브 라이브러리를 구축할 하는 것이 좋습니다 (장치 자체에 대 한 시뮬레이터 및 ARMv7/ARM64 i386). 라이브러리에 대 한 Xcode 프로젝트를 이미 보면이 매우 간단 수행 합니다.

네이티브 라이브러리의 i386 버전을 빌드하려면 터미널에서 다음 명령을 실행 합니다.

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphonesimulator -arch i386 -configuration Release clean build
```

그러면 아래에 있는 네이티브 정적 라이브러리 `MyProject.xcodeproj/build/Release-iphonesimulator/`합니다. 나중에 사용할 고유 이름을 지정 하 고 안전한 위치에 라이브러리 보관 파일 (libMyLibrary.a)을 복사 (또는 이동) (같은 **libMyLibrary i386.a**) 해야 하는 라이브러리의 arm64 및 armv7 버전과 충돌 하지 않도록 다음으로 빌드 되었습니다.

네이티브 라이브러리의 ARM64 버전을 빌드하려면 다음 명령을 실행 합니다.

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch arm64 -configuration Release clean build
```

작성된 된 네이티브 라이브러리에는이 이번 `MyProject.xcodeproj/build/Release-iphoneos/`합니다. 다시 한 번 복사 (또는 이동)로 같은 이름을 바꾸면를 안전한 위치에이 파일 **libMyLibrary arm64.a** 충돌 하지 않도록 합니다.

이제 ARMv7 버전을 라이브러리를 빌드하십시오.

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch armv7 -configuration Release clean build
```

결과 라이브러리 파일을 복사 (또는 이동) 같은 위치로 이동 라이브러리의 다른 2 버전 같이 이름을 바꾸면 **libMyLibrary armv7.a**합니다.

사용할 수 있도록 universal 이진에 `lipo` 같이 도구:

```bash
lipo -create -output libMyLibrary.a libMyLibrary-i386.a libMyLibrary-arm64.a libMyLibrary-armv7.a
```

이렇게 `libMyLibrary.a` 되 고 모든 iOS 개발 대상에 대 한 사용 하기에 적합 한 될 유니버설 (fat) 라이브러리.


### <a name="missing-required-architecture-i386"></a>누락 된 필수 아키텍처 i386

발생 하는 경우는 `does not implement methodSignatureForSelector` 또는 `does not implement doesNotRecognizeSelector` i386 아키텍처에 대 한 런타임 출력 아마도 iOS 시뮬레이터를 라이브러리에에서 Objective-c 라이브러리를 사용 하려고 할 때에 메시지를 컴파일하지 않았습니다 (참조는 [빌드 유니버설 네이티브 라이브러리](#building_native) 위의 섹션).

지정 된 라이브러리에서 지 원하는 아키텍처를 확인 하려면 터미널에서 다음 명령을 사용 합니다.

```bash
lipo -info /full/path/to/libraryname.a
```

여기서 `/full/path/to/` 사용 중인 라이브러리의 전체 경로 및 `libraryname.a` 은 라이브러리의 이름에 해당 합니다.

원본 라이브러리에 있는 경우 해야 컴파일도 i386 아키텍처용 번들 및 iOS 시뮬레이터에서 앱을 테스트 하려는 경우.

### <a name="linking-your-library"></a>라이브러리에 연결

사용 하는 모든 타사 라이브러리는 응용 프로그램을 사용 하 여 정적으로 링크 해야 합니다. 

Xcode 사용 하 여 빌드를 인터넷에서 가져온 "libMyLibrary.a" 라이브러리를 정적으로 연결 하려는 경우 다음을 수행 해야 합니다.

-  프로젝트에 라이브러리 가져오기
-  Xamarin.iOS 라이브러리 연결 구성
-  라이브러리에서 메서드를 액세스 합니다.


하 **프로젝트에 라이브러리를 가져오는**, 누릅니다 솔루션 탐색기에서 프로젝트를 선택 **명령 + 옵션 + a**합니다. libMyLibrary.a을 찾아 프로젝트에 추가 합니다. 메시지가 표시 되 면 Mac 용 Visual Studio 또는 Visual Studio 프로젝트에 복사할 지시 합니다. 를 추가한 후 프로젝트에는 libFoo.a 찾을, 마우스 오른쪽 단추로 클릭를 설정 합니다 **빌드 작업** 하 **none**합니다.

하 **라이브러리를 연결 하려면 구성 Xamarin.iOS**, 최종 실행 파일 (자체 라이브러리가 아니라 하지만 최종 프로그램)에 대 한 프로젝트 옵션에 추가 해야 **iOS 빌드**의 (이 경우 추가 인수 프로젝트 옵션의 일부)는 "-gcc_flags" 뒤에 따옴표 붙은 문자열이 필요 없는 프로그램에 대 한 예를 들어 모든 추가 라이브러리를 포함 하는 옵션:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load ${ProjectDir}/libMyLibrary.a"
```

위의 예제에서는 연결 **libMyLibrary.a**

사용할 수 있습니다 `-gcc_flags` 집합이 실행 파일의 마지막 링크를 수행 하는 데 GCC 컴파일러에 전달할 명령줄 인수를 지정 합니다. 예를 들어이 명령줄도 CFNetwork 프레임 워크를 참조 합니다.

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

네이티브 라이브러리는 c + + 코드를 포함 하는 경우 전달 해야-cxx 플래그를 "추가 인수"에 올바른 컴파일러를 사용 하려면 Xamarin.iOS가 알 수 있도록 합니다. C + +에 대 한 이전 옵션과 같습니다.

```bash
-cxx -gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

<a name="Accessing_C_Methods_from_C#" />

## <a name="accessing-c-methods-from-c35"></a>C에서 액세스 C 메서드&#35;

두 가지 종류 네이티브 라이브러리의 사용 가능한 iOS에서:

-  운영 체제의 일부인 라이브러리를 공유 합니다.

-  응용 프로그램과 함께 제공 되는 정적 라이브러리입니다.


그 중 하나에 정의 된 메서드에 액세스 하려면 사용 [Mono의 P/Invoke 기능](http://www.mono-project.com/docs/advanced/pinvoke/) 약는.net에서 사용할 수 있는 동일한 기술인:

-  호출 하려는 C 함수를 결정 합니다.
-  해당 서명을 확인 합니다.
-  상주 하는 라이브러리를 확인 합니다.
-  적절 한 P/Invoke 선언을 작성합니다


P/Invoke를 사용 하는 경우와 연결 되어 있는 라이브러리의 경로 지정 해야 합니다. IOS를 사용 하 여 공유 라이브러리, 하드 코드 된 경로 또는에서 정의한 편의 상수를 사용할 수 있습니다 하는 경우 우리의 [상수 클래스](https://developer.xamarin.com/api/type/Constants/), 이러한 상수는 iOS 공유 라이브러리를 포함 해야 합니다.

예를 들어,이 서명이 c: \에서는 Apple의 UIKit 라이브러리에서 UIRectFrameUsingBlendMode 메서드를 호출 하려는 경우

```csharp
void UIRectFrameUsingBlendMode (CGRect rect, CGBlendMode mode);
```

P/Invoke 선언에는 다음과 같습니다.

```csharp
[DllImport (Constants.UIKitLibrary,EntryPoint="UIRectFrameUsingBlendMode")]
public extern static void RectFrameUsingBlendMode (RectangleF rect, CGBlendMode blendMode);
```

Constants.UIKitLibrary는 단순히로 정의 된 상수 "/ System/Library/Frameworks/UIKit.framework/UIKit", EntryPoint를 통해 지정 필요에 따라 외부 이름 (UIRectFramUsingBlendMode)에서 다른 이름을 제공 하는 동안 C#, 짧은 RectFrameUsingBlendMode 합니다.

<a name="Accessing_C_Dylibs" />

### <a name="accessing-c-dylibs"></a>C Dylibs 액세스

Xamarin.iOS 응용 프로그램에서 C Dylib를 사용할 필요가 있다면는 호출 하기 전에 필요한 추가 설치는 `DllImport` 특성입니다.

예를 들어 있는 경우는 `Animal.dylib` 사용 하 여는 `Animal_Version` 에서는 호출 하는 응용 프로그램에는 메서드를 사용 하기 전에 Xamarin.iOS 라이브러리의 위치를 알리기 위해 필요 합니다.

이 작업을 수행 하려면 편집을 `Main.CS` 파일을 다음과 같이 표시 되도록 합니다.

```csharp
static void Main (string[] args)
{
    // Load Dylib
    MonoTouch.ObjCRuntime.Dlfcn.dlopen ("/full/path/to/Animal.dylib", 0);

    // Start application
    UIApplication.Main (args, null, "AppDelegate");
}
```

여기서 `/full/path/to/` 소모 Dylib의 전체 경로입니다. 이 코드를 사용 하 여 것을 연결할 수는 `Animal_Version` 같이 메서드:

```csharp
[DllImport("Animal.dylib", EntryPoint="Animal_Version")]
public static extern double AnimalLibraryVersion();
```

<a name="Static_Libraries" />

### <a name="static-libraries"></a>정적 라이브러리

없기 때문에 정적 라이브러리를 사용 하 여 iOS에만를 연결할 외부 공유 라이브러리가 없습니다 DllImport 특성 경로 매개 변수는 특수 한 이름이 사용 해야 하므로 `__Internal` (참고를 이름의 시작 부분에 이중 밑줄)와 반대로 경로 이름입니다.

이렇게 하면 공유 라이브러리에서 로드 하는 것이 아니라 현재 프로그램에서 참조 하는 메서드의 기호를 조회 하는 DllImport 됩니다.

