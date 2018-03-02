---
title: "네이티브 라이브러리 참조"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1DA80280-E78A-EC4B-8673-C249C8425CF5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/28/2016
ms.openlocfilehash: 9299d2b37825298d3defa18a9f5137e11b29f6ce
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="referencing-native-libraries"></a>네이티브 라이브러리 참조

Xamarin.iOS 네이티브 C 라이브러리와 Objective-c 라이브러리를 모두 사용 하 여 링크를 지원 합니다. 이 문서에서는 네이티브 C 라이브러리 Xamarin.iOS 프로젝트와 연결 하는 방법을 설명 합니다. Objective C 라이브러리에 대해 동일 하 게 이러한 작업에 대 한 정보를 참조 하십시오. 우리의 [바인딩 Objective C 형식](~/ios/platform/binding-objective-c/index.md) 문서.

<a name="building_native" />

## <a name="building-universal-native-libraries-i386-armv7-and-arm64"></a>(I386, ARMv7, 및 ARM64) 유니버설 네이티브 라이브러리 빌드

각 iOS 개발을 위해 지원 되는 플랫폼에 대 한 네이티브 라이브러리 빌드 하는 것이 좋습니다 (장치 자체에 대 한 시뮬레이터 및 ARMv7/ARM64 i386). Xcode 프로젝트 라이브러리에 대 한 맬웨어로 이미,이 실제로 수행 간단 합니다.

네이티브 라이브러리의 i386 버전을 빌드하려면 터미널에서 다음 명령을 실행 합니다.

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphonesimulator -arch i386 -configuration Release clean build
```

네이티브 정적 라이브러리에서 이렇게 하면 `MyProject.xcodeproj/build/Release-iphonesimulator/`합니다. 라이브러리 보관 파일 (libMyLibrary.a) 했지만 나중에 사용할 고유 이름을 지정 안전한 위치에 복사 (또는 이동) (예: **libMyLibrary i386.a**) 하는 동일한 라이브러리의 arm64 및 armv7 버전 충돌 하지 않습니다 다음을 빌드하십시오.

네이티브 라이브러리의 ARM64 버전을 빌드하려면 다음 명령을 실행 합니다.

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch arm64 -configuration Release clean build
```

빌드된 네이티브 라이브러리에 배치 됩니다이 이번 `MyProject.xcodeproj/build/Release-iphoneos/`합니다. 다시 한 번 복사 (또는 이동)와 같은 값으로 이름 바꾸기, 안전한 위치에이 파일 **libMyLibrary arm64.a** 충돌 되지 않도록 합니다.

이제 ARMv7 버전을 라이브러리를 빌드하십시오.

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch armv7 -configuration Release clean build
```

결과 라이브러리 파일을 복사 (또는 이동) 동일한 위치로 이동한 후 2는 다른 버전의 라이브러리에서는 같은 값으로 이름 바꾸기 **libMyLibrary armv7.a**합니다.

Universal 이진, 있습니다 사용할 수는 `lipo` 같이 도구:

```bash
lipo -create -output libMyLibrary.a libMyLibrary-i386.a libMyLibrary-arm64.a libMyLibrary-armv7.a
```

그렇기 때문에 `libMyLibrary.a` 를 적합 한 모든 iOS 개발 대상에 사용할 수 있는 범용 (fat) 라이브러리를 수 있습니다.


### <a name="missing-required-architecture-i386"></a>누락 된 필수 아키텍처 i386

발생 하는 경우는 `does not implement methodSignatureForSelector` 또는 `does not implement doesNotRecognizeSelector` i386 아키텍처에 대 한 런타임 출력 것의 ios 시뮬레이터, 라이브러리는 Objective C 라이브러리를 사용 하려고 할 때에 메시지에서 동작 하지 않습니다 (참조는 [건물 유니버설 네이티브 라이브러리](#building_native) 위의 섹션).

지정한 라이브러리에서 지 원하는 아키텍처를 확인 하려면 터미널에 다음 명령을 사용 합니다.

```bash
lipo -info /full/path/to/libraryname.a
```

여기서 `/full/path/to/` 사용 되는 라이브러리에 전체 경로 및 `libraryname.a` 을 라이브러리의 이름을 확인 하지 못했습니다.

원본 라이브러리에 있으면 iOS 시뮬레이터에서 앱을 테스트 하려는 경우를 컴파일하고 i386 아키텍처에 대 한 번들 해야 합니다.

### <a name="linking-your-library"></a>라이브러리에 연결

사용 하는 모든 타사 라이브러리 응용 프로그램에 정적으로 연결 해야 합니다. 

인터넷 또는 xcode 빌드에서 가져온 "libMyLibrary.a" 라이브러리를 정적으로 연결 하려는 경우 다음을 수행 해야 합니다.

-  프로젝트에 라이브러리 가져오기
-  Xamarin.iOS 라이브러리 연결 하려면 구성
-  라이브러리에서 메서드를 액세스 합니다.


**하 여 프로젝트에 라이브러리를 가져올**, 키를 눌러 확인 하 고 솔루션 탐색기에서 프로젝트를 선택 합니다. **명령 + 옵션 + a**합니다. libMyLibrary.a을 찾아 프로젝트에 추가 합니다. 메시지가 표시 되 면 등에 대 한 Mac 용 Visual Studio 또는 Visual Studio 프로젝트에 복사 합니다. 를 추가한 후 프로젝트의는 libFoo.a 찾을 마우스 오른쪽 단추로 클릭, 설정 된 **빌드 작업** 를 **none**합니다.

**라이브러리 연결 하려면 구성 Xamarin.iOS**에 추가 해야 최종 실행 파일 (라이브러리 자체 하지만 최종 프로그램)에 대 한 프로젝트 옵션에 **iOS 빌드**의 추가 인수 ( 프로젝트 옵션의 일부로)는 "-gcc_flags" 옵션 및 모든 추가 되는 라이브러리에 필요한 프로그램에 대 한 예를 들어 포함 된 따옴표 붙은 문자열:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load ${ProjectDir}/libMyLibrary.a"
```

위의 예제에서는 연결 **libMyLibrary.a**

사용할 수 있습니다 `-gcc_flags` 실행 파일의 마지막 링크를 수행 하는 데 GCC 컴파일러에 전달할 명령줄 인수 집합을 지정할 수 있습니다. 예를 들어이 명령줄도 CFNetwork 프레임 워크를 참조 합니다.

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

네이티브 라이브러리는 c + + 코드를 포함 하는 경우도 전달 해야-cxx 플래그를 "추가 인수"에서 올바른 컴파일러를 사용 하 Xamarin.iOS 알 수 있도록 합니다. C + +에 대 한 이전 옵션은 같습니다.

```bash
-cxx -gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

<a name="Accessing_C_Methods_from_C#" />

## <a name="accessing-c-methods-from-c35"></a>C 메서드 &#35;에서 액세스

두 가지가 네이티브 라이브러리의 사용 가능한 iOS에서:

-  운영 체제에 포함 되어 있는 라이브러리를 공유 합니다.

-  응용 프로그램과 함께 제공 되는 정적 라이브러리입니다.


그 중 하나에 정의 된 메서드를 액세스 하려면 사용 [모노의 P/Invoke 기능](http://www.mono-project.com/Interop_with_Native_Libraries) .NET 되기에 사용할 수 있는 동일한 기술인:

-  C 함수를 호출할지를 결정 합니다.
-  서명 확인
-  에 거주 하는 라이브러리를 확인 합니다.
-  적절 한 P/Invoke 선언 작성


P/Invoke를 사용 하는 경우에 연결 하 고 있는 라이브러리의 경로 지정 해야 합니다. IOS를 사용 하 여 공유 라이브러리, 하드 코드 된 경로 또는에 정의 편의 상수를 사용할 수 있습니다 때 우리의 [상수 클래스](https://developer.xamarin.com/api/type/Constants/), 이러한 상수는 iOS 공유 라이브러리를 포함 해야 합니다.

예를 들어,이 서명이 c:에 표시 되는 Apple의 UIKit 라이브러리에서 UIRectFrameUsingBlendMode 메서드를 호출 하려는 경우

```csharp
void UIRectFrameUsingBlendMode (CGRect rect, CGBlendMode mode);
```

P/Invoke 선언은 다음과 같습니다.

```csharp
[DllImport (Constants.UIKitLibrary,EntryPoint="UIRectFrameUsingBlendMode")]
public extern static void RectFrameUsingBlendMode (RectangleF rect, CGBlendMode blendMode);
```

Constants.UIKitLibrary는 단순히로 정의 된 상수 "/ System/Library/Frameworks/UIKit.framework/UIKit", EntryPoint 수 있도록 설정할 지정 필요에 따라 외부 이름 (UIRectFramUsingBlendMode) 라는 C#에서 짧은 노출 하는 동안 RectFrameUsingBlendMode 합니다.

<a name="Accessing_C_Dylibs" />

### <a name="accessing-c-dylibs"></a>C Dylibs에 액세스

호출 하기 전에 필요한 추가 설치의 비트를 Xamarin.iOS 응용 프로그램에서 C Dylib를 사용 해야 할 경우는 `DllImport` 특성입니다.

예를 들어, 사항이 있는 경우는 `Animal.dylib` 와 `Animal_Version` 클래스를 사용 하는 시도 하기 전에 Xamarin.iOS 라이브러리의 위치에 알려야 할 म म는 응용 프로그램에서 호출 될 메서드입니다.

이 작업을 수행 하려면 편집는 `Main.CS` 파일을 다음과 같이 되도록 합니다.

```csharp
static void Main (string[] args)
{
    // Load Dylib
    MonoTouch.ObjCRuntime.Dlfcn.dlopen ("/full/path/to/Animal.dylib", 0);

    // Start application
    UIApplication.Main (args, null, "AppDelegate");
}
```

여기서 `/full/path/to/` 사용 되 고 Dylib에 전체 경로입니다. 이 코드가에서는 다음에 연결할 수는 `Animal_Version` 메서드를 다음과 같이 합니다.

```csharp
[DllImport("Animal.dylib", EntryPoint="Animal_Version")]
public static extern double AnimalLibraryVersion();
```

<a name="Static_Libraries" />

### <a name="static-libraries"></a>정적 라이브러리

IOS에만 정적 라이브러리를 사용할 수 있습니다, 이므로, 링크할 수 없는 외부 공유 라이브러리 특수 이름을 사용 하 여 path 매개 변수는 DllImport 특성에 필요 하므로 `__Internal` (이중 밑줄 문자는 이름의 시작 부분에 참고)과 반대 경로 이름입니다.

이렇게 하면 공유 라이브러리에서 로드 하는 것이 아니라 현재 프로그램의를 참조 하는 방법의 기호를 조회 하는 DllImport 됩니다.

