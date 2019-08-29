---
title: '연습: iOS Objective-C 라이브러리 바인딩'
description: 이 문서에서는 기존 목표-C 라이브러리, InfColorPicker에 대 한 Xamarin.ios 바인딩을 만드는 실습 연습을 제공 합니다. 정적 목표-C 라이브러리 컴파일, 바인딩 및 Xamarin.ios 응용 프로그램에서 바인딩을 사용 하는 것과 같은 항목을 다룹니다.
ms.prod: xamarin
ms.assetid: D3F6FFA0-3C4B-4969-9B83-B6020B522F57
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/02/2017
ms.openlocfilehash: ffd244a77ae75fefcf42f185bad1e8f7ccdbe560
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70121332"
---
# <a name="walkthrough-binding-an-ios-objective-c-library"></a>연습: iOS Objective-C 라이브러리 바인딩

_이 문서에서는 기존 목표-C 라이브러리, InfColorPicker에 대 한 Xamarin.ios 바인딩을 만드는 실습 연습을 제공 합니다. 정적 목표-C 라이브러리 컴파일, 바인딩 및 Xamarin.ios 응용 프로그램에서 바인딩을 사용 하는 것과 같은 항목을 다룹니다._

IOS에서 작업 하는 경우 타사 목표-C 라이브러리를 사용 하려는 경우가 발생할 수 있습니다. 이러한 경우 xamarin.ios _바인딩 프로젝트_ 를 사용 하 여 xamarin.ios 응용 프로그램에서 라이브러리를 사용할 수 있는 [ C# 바인딩을](~/cross-platform/macios/binding/overview.md) 만들 수 있습니다.

일반적으로 iOS 에코 시스템에서는 다음의 3 가지 버전으로 라이브러리를 찾을 수 있습니다.

- 확장명을 포함 `.a` 하는 미리 컴파일된 정적 라이브러리 파일로 서 헤더 (.h 파일)와 함께 사용 됩니다. 예: [Google의 분석 라이브러리](https://developers.google.com/analytics/devguides/collection/ios/v3/sdk-download?hl=es#download_sdk)
- 미리 컴파일된 프레임 워크로. 이는 정적 라이브러리, 헤더 및 확장을 사용 `.framework` 하는 추가 리소스를 포함 하는 폴더에 불과합니다. 예를 들어 [Google의 AdMob 라이브러리](https://developers.google.com/admob/ios/download)입니다.
- 원본 코드 파일입니다. 예를 들어, `.m` 및 `.h` 목표 C 파일을 포함 하는 라이브러리입니다.

첫 번째 및 두 번째 시나리오에서는 미리 컴파일된 CocoaTouch 정적 라이브러리가 이미 있으므로이 문서에서는 세 번째 시나리오에 중점을 둡니다. 바인딩을 만들기 전에 항상 라이브러리와 함께 제공 되는 라이선스를 확인 하 여 해당 라이선스를 자유롭게 바인딩할 수 있도록 해야 합니다.

이 문서에서는 오픈 소스 [Infcolorpicker](https://github.com/InfinitApps/InfColorPicker) 목표-c 프로젝트를 사용 하 여 바인딩 프로젝트를 만드는 방법에 대 한 단계별 연습을 제공 하지만,이 가이드의 모든 정보는 타사 목표-c 라이브러리와 함께 사용 하도록 조정할 수 있습니다. . InfColorPicker 라이브러리는 다시 사용할 수 있는 뷰 컨트롤러를 제공 합니다 .이를 통해 사용자는이를 통해 HSB 표시를 기준으로 색을 선택 하 여 사용자에 게 더 쉽게 색을 선택할 수 있습니다.

[![](walkthrough-images/run01.png "IOS에서 실행 되는 InfColorPicker 라이브러리의 예")](walkthrough-images/run01.png#lightbox)

Xamarin.ios에서이 특정 목표-C API를 사용 하는 데 필요한 모든 단계를 다룹니다.

- 먼저 Xcode을 사용 하 여 목표-C 정적 라이브러리를 만듭니다.
- 그런 다음이 정적 라이브러리를 Xamarin.ios와 바인딩합니다.
- 다음으로, Xamarin.ios 바인딩에 필요한 API 정의 중 일부 (모두 아님)를 자동으로 생성 하 여 목표 Sharpie에서 작업을 줄일 수 있는 방법을 보여 줍니다.
- 마지막으로 바인딩을 사용 하는 Xamarin.ios 응용 프로그램을 만듭니다.

샘플 응용 프로그램에서는 InfColorPicker API와 C# 코드 간의 통신에 강력한 대리자를 사용 하는 방법을 보여 줍니다. 강력한 대리자를 사용 하는 방법을 살펴본 후에는 약한 대리자를 사용 하 여 동일한 작업을 수행 하는 방법을 설명 합니다.

## <a name="requirements"></a>요구 사항

이 문서에서는 Xcode 및 목표-C 언어에 대해 잘 알고 있는 것으로 가정 하 고, [바인딩 목표-c](~/cross-platform/macios/binding/index.md) 설명서를 읽었습니다. 또한 다음은 제시 된 단계를 완료 하는 데 필요 합니다.

- **Xcode 및 IOS SDK** -Apple의 Xcode와 최신 ios API는 개발자의 컴퓨터에 설치 하 고 구성 해야 합니다.
- **[Xcode 명령줄 도구](#Installing_the_Xcode_Command_Line_Tools)** -현재 설치 된 Xcode 버전에 대해 Xcode 명령줄 도구를 설치 해야 합니다 (설치 정보는 아래 참조).
- **Mac용 Visual Studio 또는 Visual studio** -개발 컴퓨터에서 최신 버전의 Mac용 Visual Studio 또는 visual studio를 설치 하 고 구성 해야 합니다. Xamarin.ios 응용 프로그램을 개발 하려면 Apple Mac이 필요 하 고, Visual Studio를 사용 하는 경우 [xamarin.ios 빌드 호스트](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 에 연결 해야 합니다.
- **최신 버전의 목표 Sharpie** - [여기](~/cross-platform/macios/binding/objective-sharpie/get-started.md)에서 다운로드 한 목표 Sharpie 도구의 현재 복사본입니다. 이미 목표 Sharpie이 설치 되어 있는 경우 다음을 사용 하 여 최신 버전으로 업데이트할 수 있습니다.`sharpie update`

<a name="Installing_the_Xcode_Command_Line_Tools"/>

## <a name="installing-the-xcode-command-line-tools"></a>Xcode 명령줄 도구 설치

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)


위에서 설명한 것 처럼이 연습에서는 Xcode 명령줄 도구 (특히 `make` 및 `lipo`)를 사용 합니다. 명령은 프로그램을 빌드하는 방법을 지정 하는 메이크파일을 사용 하 여 실행 프로그램 및 라이브러리의 컴파일을 자동화 하는 매우 일반적인 Unix 유틸리티입니다. `make` 명령은 다중 아키텍처 파일을 만들기 위한 OS X 명령줄 유틸리티 이며, 여러 `.a` 파일을 모든 하드웨어 아키텍처에서 사용할 수 있는 하나의 파일로 결합 합니다. `lipo`


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


위에서 설명한 것 처럼이 연습에서는 **Mac 빌드 호스트** (특히 `make` 및 `lipo`)에서 Xcode 명령줄 도구를 사용 합니다. 명령은 메이크파일을 사용 하 여 프로그램을 빌드하는 방법을 지정 하는 실행 프로그램 및 라이브러리의 컴파일을 자동화 하는 매우 일반적인 Unix 유틸리티입니다. `make` 명령은 다중 아키텍처 파일을 만들기 위한 OS X 명령줄 유틸리티 이며, 여러 `.a` 파일을 모든 하드웨어 아키텍처에서 사용할 수 있는 하나의 파일로 결합 합니다. `lipo`


-----

Xcode FAQ 설명서를 [사용 하 여 명령줄에서](https://developer.apple.com/library/ios/technotes/tn2339/_index.html) Apple의 빌드에 따라 OS X 10.9 이상에서는 Xcode **기본 설정** 대화 상자의 **다운로드** 창에서 다운로드 하는 명령줄 도구를 더 이상 지원 하지 않습니다.

다음 방법 중 하나를 사용 하 여 도구를 설치 해야 합니다.

- **Xcode 설치** -Xcode을 설치 하면 모든 명령줄 도구와 함께 제공 됩니다. OS X 10.9 shim (에 `/usr/bin`설치 됨)에서는에 `/usr/bin` 포함 된 도구를 Xcode 내의 해당 도구에 매핑할 수 있습니다. 예를 들어 명령줄 `xcrun` 에서 Xcode 내의 모든 도구를 찾거나 실행할 수 있는 명령을 사용할 수 있습니다.
- **터미널 응용 프로그램** -터미널 응용 프로그램에서 `xcode-select --install` 명령을 실행 하 여 명령줄 도구를 설치할 수 있습니다.
  - 터미널 응용 프로그램을 시작 합니다.
  - 을 `xcode-select --install` 입력 하 고 **enter**키를 누릅니다. 예를 들면 다음과 같습니다.

  ```bash
  Europa:~ kmullins$ xcode-select --install
  ```

  - 명령줄 도구를 설치 하 라는 메시지가 표시 되 면 **설치** 단추를 클릭 합니다. [![](walkthrough-images/xcode01.png "명령줄 도구 설치")](walkthrough-images/xcode01.png#lightbox)

  - 이 도구는 Apple의 서버에서 다운로드 되 고 설치 됩니다. [![](walkthrough-images/xcode02.png "도구 다운로드")](walkthrough-images/xcode02.png#lightbox)

- **Apple 개발자를 위한 다운로드** - [Apple 개발자 용 다운로드](https://developer.apple.com/downloads/index.action) 웹 페이지에서 명령줄 도구 패키지를 사용할 수 있습니다. Apple ID로 로그인 한 다음 명령줄 도구를 검색 하 고 다운로드 합니다. [![](walkthrough-images/xcode03.png "명령줄 도구 찾기")](walkthrough-images/xcode03.png#lightbox)

명령줄 도구가 설치 되 면 연습을 진행할 준비가 되었습니다.

## <a name="walkthrough"></a>연습

이 연습에서는 다음 단계를 다룹니다.

- **[정적 라이브러리 만들기](#Creating_A_Static_Library)** -이 단계는 **infcolorpicker** 목표-C 코드의 정적 라이브러리를 만드는 작업을 포함 합니다. 정적 라이브러리 `.a` 는 파일 확장명을 포함 하 고 라이브러리 프로젝트의 .net 어셈블리에 포함 됩니다.
- **[Xamarin.ios 바인딩 프로젝트 만들기](#Create_a_Xamarin.iOS_Binding_Project)** -정적 라이브러리가 있으면이를 사용 하 여 xamarin.ios 바인딩 프로젝트를 만듭니다. 바인딩 프로젝트는 방금 만든 정적 라이브러리로 구성 되며, 목표-C API를 사용 하 C# 는 방법을 설명 하는 코드 형식으로 메타 데이터를 구성 합니다. 이 메타 데이터를 일반적으로 API 정의 라고 합니다. API 정의를 만드는 데 도움이 되는 **[목적 Sharpie](#Using_Objective_Sharpie)** 를 사용 합니다.
- **[API 정의 표준화](#Normalize_the_API_Definitions)** -목표 Sharpie는 유용 하지만 모든 작업을 수행할 수는 없습니다. API 정의를 사용 하기 전에 수행 해야 하는 몇 가지 변경 사항을 설명 합니다.
- **[바인딩 라이브러리 사용](#Using_the_Binding)** -마지막으로, 새로 만든 바인딩 프로젝트를 사용 하는 방법을 보여 주는 xamarin.ios 응용 프로그램을 만듭니다.

이제 관련 된 단계를 이해 했으므로 연습의 나머지 부분으로 이동 하겠습니다.

<a name="Creating_A_Static_Library"/>

## <a name="creating-a-static-library"></a>정적 라이브러리 만들기

Github에서 InfColorPicker에 대 한 코드를 검사 하는 경우:

[![](walkthrough-images/image02.png "Github에서 InfColorPicker에 대 한 코드 검사")](walkthrough-images/image02.png#lightbox)

프로젝트에서 다음 세 가지 디렉터리를 볼 수 있습니다.

- **Infcolorpicker** -이 디렉터리는 프로젝트에 대 한 목표 C 코드를 포함 합니다.
- **PickerSamplePad** -이 디렉터리에는 샘플 iPad 프로젝트가 포함 되어 있습니다.
- **PickerSamplePhone** -이 디렉터리는 샘플 iPhone 프로젝트를 포함 합니다.

[GitHub](https://github.com/InfinitApps/InfColorPicker/archive/master.zip) 에서 InfColorPicker 프로젝트를 다운로드 하 고 선택한 디렉터리에서 압축을 풉니다. 프로젝트에 대 한 `PickerSamplePhone` Xcode 대상을 열면 Xcode 탐색기에 다음과 같은 프로젝트 구조가 표시 됩니다.

[![](walkthrough-images/image03.png "Xcode Navigator의 프로젝트 구조")](walkthrough-images/image03.png#lightbox)

이 프로젝트는 각 샘플 프로젝트에 InfColorPicker 소스 코드 (빨간색 상자)를 직접 추가 하 여 코드를 다시 사용 합니다. 샘플 프로젝트에 대 한 코드는 파란색 상자 안에 있습니다. 이 특정 프로젝트는 정적 라이브러리를 제공 하지 않으므로 Xcode 프로젝트를 만들어 정적 라이브러리를 컴파일해야 합니다.

첫 번째 단계는 InfoColorPicker 소스 코드를 정적 라이브러리에 추가 하는 것입니다. 이렇게 하려면 다음을 수행 합니다.

1. Xcode를 시작합니다.
2. **파일** 메뉴에서 **새** > **프로젝트**...를 선택 합니다.

    [![](walkthrough-images/image04.png "새 프로젝트를 시작 하는 중")](walkthrough-images/image04.png#lightbox)
3. **프레임 워크 & 라이브러리**, **Cocoa Touch 정적 라이브러리** 템플릿을 선택 하 고 **다음** 단추를 클릭 합니다.

    [![](walkthrough-images/image05.png "Cocoa Touch 정적 라이브러리 템플릿을 선택 합니다.")](walkthrough-images/image05.png#lightbox)

4. `InfColorPicker` **프로젝트 이름** 으로를 입력 하 고 **다음** 단추를 클릭 합니다.

    [![](walkthrough-images/image06.png "프로젝트 이름에 대 한 InfColorPicker 입력")](walkthrough-images/image06.png#lightbox)
5. 프로젝트를 저장할 위치를 선택 하 고 **확인** 단추를 클릭 합니다.
6. 이제 InfColorPicker 프로젝트의 소스를 정적 라이브러리 프로젝트에 추가 해야 합니다. **Infcolorpicker .h** 파일은 기본적으로 정적 라이브러리에 이미 있기 때문에 Xcode에서 해당 파일을 덮어쓸 수 없습니다. **Finder**에서 GitHub에서 압축을 푼 원본 프로젝트의 infcolorpicker 소스 코드로 이동 하 고 모든 infcolorpicker 파일을 복사 하 여 새 정적 라이브러리 프로젝트에 붙여넣습니다.

    [![](walkthrough-images/image12.png "모든 InfColorPicker 파일 복사")](walkthrough-images/image12.png#lightbox)

7. Xcode로 돌아가서 **Infcolorpicker** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **"infcolorpicker ..."에 파일 추가를**선택 합니다.

    [![](walkthrough-images/image08.png "파일 추가")](walkthrough-images/image08.png#lightbox)

8. 파일 추가 대화 상자에서 방금 복사한 InfColorPicker 소스 코드 파일로 이동 하 여 모두 선택 하 고 **추가** 단추를 클릭 합니다.

    [![](walkthrough-images/image09.png "모두 선택 하 고 추가 단추를 클릭 합니다.")](walkthrough-images/image09.png#lightbox)

9. 소스 코드는 프로젝트에 복사 됩니다.

    [![](walkthrough-images/image10.png "소스 코드가 프로젝트에 복사 됩니다.")](walkthrough-images/image10.png#lightbox)

10. Xcode 프로젝트 탐색기에서 **Infcolorpicker** 파일을 선택 하 고 마지막 두 줄을 주석으로 처리 합니다 .이 파일은 사용 되지 않습니다.

    [![](walkthrough-images/image14.png "InfColorPicker. m 파일 편집")](walkthrough-images/image14.png#lightbox)

11. 이제 라이브러리에 필요한 프레임 워크가 있는지 확인 해야 합니다. 이 정보는 추가 정보에서 찾거나 제공 된 샘플 프로젝트 중 하나를 열어 볼 수 있습니다. 이 예제에서는 `Foundation.framework`, `UIKit.framework`를 `CoreGraphics.framework` 사용 하므로 추가 해 보겠습니다.

12. **빌드 단계 > Infcolorpicker 대상을** 선택 하 고 **라이브러리를 사용 하 여 이진 파일 연결** 섹션을 확장 합니다.

    [![](walkthrough-images/image16b.png "라이브러리를 사용 하 여 이진 파일 연결 섹션을 확장 합니다.")](walkthrough-images/image16b.png#lightbox)

13. **+** 단추를 사용 하 여 위에 나열 된 필수 프레임 프레임 워크를 추가할 수 있는 대화 상자를 엽니다.

    [![](walkthrough-images/image16c.png "위에 나열 된 필수 프레임 프레임 워크 추가")](walkthrough-images/image16c.png#lightbox)

14. 이제 **라이브러리와 이진 링크** 섹션이 아래 이미지와 같이 표시 됩니다.

    [![](walkthrough-images/image16d.png "라이브러리에 이진 링크 섹션")](walkthrough-images/image16d.png#lightbox)

이 시점에서이 작업을 완료 하는 것은 아닙니다. 정적 라이브러리가 만들어졌지만 iOS 장치 및 iOS 시뮬레이터 모두에 필요한 모든 아키텍처를 포함 하는 Fat 이진 파일을 만들기 위해 빌드해야 합니다.

### <a name="creating-a-fat-binary"></a>Fat 이진 파일 만들기

모든 iOS 장치에는 시간에 따라 개발 된 ARM 아키텍처로 구동 되는 프로세서가 있습니다. 새 아키텍처에는 이전 버전과의 호환성을 유지 하면서 새로운 지침과 기타 기능이 추가 되었습니다. iOS 장치에는 armv6, armv7, armv7s 용 thumb-2, arm64 명령 집합이 있습니다. 하지만 [armv6은 더 이상 사용 되지 않습니다](~/ios/deploy-test/compiling-for-different-devices.md). IOS 시뮬레이터는 ARM에서 구동 되지 않으며 대신 x86 및 x86_64 동력 시뮬레이터입니다. 즉, 각 명령 집합에 대해 라이브러리를 제공 해야 합니다.

Fat 라이브러리는 지원 `.a` 되는 모든 아키텍처를 포함 하는 파일입니다.

Fat 이진 파일을 만드는 과정은 다음 3 단계로 진행 됩니다.

- ARM 7 & ARM64 버전의 정적 라이브러리를 컴파일합니다.
- X86 및 x84_64 버전의 정적 라이브러리를 컴파일합니다.
- `lipo` 명령줄 도구를 사용 하 여 두 정적 라이브러리를 하나로 결합 합니다.

이러한 세 단계는 간단 하지만, 목표-C 라이브러리가 업데이트를 받거나 버그를 수정 해야 하는 경우 나중에이 단계를 반복 해야 할 수도 있습니다. 이러한 단계를 자동화 하기로 결정 한 경우에는 iOS 바인딩 프로젝트의 향후 유지 관리 및 지원이 간소화 됩니다.

이러한 작업을 자동화 하는 데 사용할 수 있는 여러 도구가 있습니다 (셸 스크립트, [rake](http://rake.rubyforge.org/), [xbuild](https://www.mono-project.com/docs/tools+libraries/tools/xbuild/)및 [만들기](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/make.1.html)). Xcode 명령줄 도구가 설치 `make` 되 면도 설치 되므로이 연습에 사용할 빌드 시스템입니다. IOS 장치에서 작동 하는 다중 아키텍처 공유 라이브러리를 만드는 데 사용할 수 있는 **메이크파일** 및 라이브러리에 대 한 시뮬레이터는 다음과 같습니다.

<!--markdownlint-disable MD010 -->
```makefile
XBUILD=/Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild
PROJECT_ROOT=./YOUR-PROJECT-NAME
PROJECT=$(PROJECT_ROOT)/YOUR-PROJECT-NAME.xcodeproj
TARGET=YOUR-PROJECT-NAME

all: lib$(TARGET).a

lib$(TARGET)-i386.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphonesimulator -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphonesimulator/lib$(TARGET).a $@

lib$(TARGET)-armv7.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch armv7 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

lib$(TARGET)-arm64.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch arm64 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

lib$(TARGET).a: lib$(TARGET)-i386.a lib$(TARGET)-armv7.a lib$(TARGET)-arm64.a
    xcrun -sdk iphoneos lipo -create -output $@ $^

clean:
    -rm -f *.a *.dll
```
<!--markdownlint-enable MD010 -->

선택한 일반 텍스트 편집기에서 **메이크파일** 명령을 입력 하 고 프로젝트 이름으로 섹션을 업데이트 합니다. 또한 지침 내에서 탭을 사용 하 여 위의 지침을 정확 하 게 붙여 넣어야 합니다.

이름 **Makefile** 을 포함 하는 파일을 위에서 만든 InfColorPicker Xcode 정적 라이브러리와 동일한 위치에 저장 합니다.

[![](walkthrough-images/lib00.png "이름 메이크파일을 사용 하 여 파일을 저장 합니다.")](walkthrough-images/lib00.png#lightbox)

Mac에서 터미널 응용 프로그램을 열고 메이크파일의 위치로 이동 합니다. 터미널 `make` 에을 입력 하 고 **enter** 키를 누르면 **Makefile** 이 실행 됩니다.

[![](walkthrough-images/lib01.png "샘플 메이크파일 출력")](walkthrough-images/lib01.png#lightbox)

만들기를 실행 하면로 많은 텍스트 스크롤이 표시 됩니다. 모든 것이 제대로 작동 하는 경우 **빌드 성공** 및 `libInfColorPicker-armv7.a`를 `libInfColorPicker-i386.a` 확인 하 고 `libInfColorPickerSDK.a` 파일이 **메이크파일의**동일한 위치에 복사 됩니다.

[![](walkthrough-images/lib02.png "LibInfColorPicker-armv7, libInfColorPicker-i386. a 및 Libinfcolorankersdk. 메이크파일의 생성 파일")](walkthrough-images/lib02.png#lightbox)

다음 명령을 사용 하 여 Fat 이진 내에서 아키텍처를 확인할 수 있습니다.

```bash
xcrun -sdk iphoneos lipo -info libInfColorPicker.a
```

다음이 표시 됩니다.

```bash
Architectures in the fat file: libInfColorPicker.a are: i386 armv7 x86_64 arm64
```

이 시점에서 Xcode 및 Xcode 명령줄 도구 `make` 및 `lipo`를 사용 하 여 정적 라이브러리를 만들어 iOS 바인딩의 첫 번째 단계를 완료 했습니다. 다음 단계로 이동 하 고 **Sharpie** 를 사용 하 여 API 바인딩 만들기를 자동화 해 보겠습니다.

<a name="Create_a_Xamarin.iOS_Binding_Project"/>

## <a name="create-a-xamarinios-binding-project"></a>Xamarin.ios 바인딩 프로젝트 만들기

**Sharpie** 를 사용 하 여 바인딩 프로세스를 자동화 하려면 API 정의를 보관 하는 Xamarin.ios 바인딩 프로젝트를 만들어야 합니다. ( **Sharpie** 를 사용 하 여 빌드하는 데 도움이 됨) microsoft에 대 한 C# 바인딩을 만들 수 있습니다.

다음 작업을 수행해 보겠습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac용 Visual Studio를 시작 합니다.
1. **파일** 메뉴에서 **새** > **솔루션 ...** 을 선택 합니다.

    ![](walkthrough-images/bind01.png "새 솔루션 시작")

1. 새 솔루션 대화 상자에서 **라이브러리** > **iOS 바인딩 프로젝트**를 선택 합니다.

    ![](walkthrough-images/bind02.png "IOS 바인딩 프로젝트 선택")

1. **다음** 단추를 클릭 합니다.

1. **프로젝트 이름** 으로 "Infcolor kerbinding"을 입력 하 고 **만들기** 단추를 클릭 하 여 솔루션을 만듭니다.

    ![](walkthrough-images/bind02a.png "프로젝트 이름으로 Infcolor Kerbinding 입력")

솔루션이 만들어지고 다음과 같은 두 개의 기본 파일이 포함 됩니다.

![](walkthrough-images/bind03.png "솔루션 탐색기의 솔루션 구조")


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


1. Visual Studio를 시작합니다.

1. **파일** 메뉴에서 **새** > **프로젝트**...를 선택 합니다.

    ![새 프로젝트를 시작 하는 중](walkthrough-images/bind01vs.png "새 프로젝트를 시작 하는 중")

1. 새 프로젝트 대화 상자에서 **Visual C# > iPhone & IPad > iOS 바인딩 라이브러리 (Xamarin)** 를 선택 합니다.

    [![IOS 바인딩 라이브러리를 선택 합니다.](walkthrough-images/bind02.w157-sml.png)](walkthrough-images/bind02.w157.png#lightbox)

1. **이름** 으로 "Infcolor kerbinding"을 입력 하 고 **확인** 단추를 클릭 하 여 솔루션을 만듭니다.

솔루션이 만들어지고 다음과 같은 두 개의 기본 파일이 포함 됩니다.

![](walkthrough-images/bind03vs.png "솔루션 탐색기의 솔루션 구조")

-----

- **ApiDefinition.cs** -이 파일은 목표-C API를 래핑하는 방법을 정의 하는 계약을 포함 합니다 C#.
- **Structs.cs** -이 파일에는 인터페이스 및 대리자에 필요한 모든 구조체 또는 열거형 값이 포함 됩니다.

이 연습의 뒷부분에서는 이러한 두 파일을 사용 합니다. 먼저, 바인딩 프로젝트에 InfColorPicker 라이브러리를 추가 해야 합니다.

### <a name="including-the-static-library-in-the-binding-project"></a>바인딩 프로젝트에 정적 라이브러리 포함

이제 기본 바인딩 프로젝트가 준비 되었으므로 **Infcolorpicker** 라이브러리에 대해 위에서 만든 Fat 이진 라이브러리를 추가 해야 합니다.

라이브러리를 추가 하려면 다음 단계를 따르세요.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Solution Pad에서 **네이티브 참조** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **네이티브 참조 추가**를 선택 합니다.

    ![](walkthrough-images/bind04a.png "네이티브 참조 추가")

1. 이전에 만든 Fat 이진 파일 (`libInfColorPickerSDK.a`)로 이동 하 고 **열기** 단추를 누릅니다.

    ![](walkthrough-images/bind05.png "LibInfColorPickerSDK를 선택 합니다. 파일")
1. 이 파일은 프로젝트에 포함 됩니다.

    ![](walkthrough-images/bind04.png "파일 포함")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. `libInfColorPickerSDK.a` **Mac 빌드 호스트** 에서를 복사 하 여 바인딩 프로젝트에 붙여넣습니다.

1. 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **기존 항목 > 추가**...를 선택 합니다.

    ![](walkthrough-images/bind04vs.png "기존 파일 추가")

1. 로 `libInfColorPickerSDK.a` 이동 하 고 **추가** 단추를 누릅니다.

    ![](walkthrough-images/bind05vs.png "Adding libInfColorPickerSDK.a")

1. 파일이 프로젝트에 포함 됩니다.

-----

파일이 프로젝트 에 추가 되 면 xamarin.ios는 파일의 **빌드 작업** 을 자동으로 **ObjcBindingNativeLibrary**로 설정 하 고 라는 `libInfColorPickerSDK.linkwith.cs`특수 파일을 만듭니다.


이 파일에는 `LinkWith` xamarin.ios에서 방금 추가한 정적 라이브러리를 처리 하는 방법을 설명 하는 특성이 포함 되어 있습니다. 이 파일의 내용은 다음 코드 조각에 나와 있습니다.

```csharp
using ObjCRuntime;

[assembly: LinkWith ("libInfColorPickerSDK.a", SmartLink = true, ForceLoad = true)]
```

특성 `LinkWith` 은 프로젝트의 정적 라이브러리 및 몇 가지 중요 한 링커 플래그를 식별 합니다.


다음으로 수행 해야 하는 작업은 InfColorPicker 프로젝트에 대 한 API 정의를 만드는 것입니다. 이 연습에서는 목적 Sharpie을 사용 하 여 **ApiDefinition.cs**파일을 생성 합니다.

<a name="Using_Objective_Sharpie"/>

## <a name="using-objective-sharpie"></a>목표 Sharpie 사용

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)


목표 Sharpie는 타사 목표-C 라이브러리를에 C#바인딩하는 데 필요한 정의를 만드는 데 도움이 될 수 있는 명령줄 도구 (Xamarin에서 제공)입니다. 이 섹션에서는 Sharpie를 사용 하 여 InfColorPicker 프로젝트에 대 한 초기 **ApiDefinition.cs** 를 만듭니다.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


목표 Sharpie는 타사 목표-C 라이브러리를에 C#바인딩하는 데 필요한 정의를 만드는 데 도움이 될 수 있는 명령줄 도구 (Xamarin에서 제공)입니다. 이 섹션에서는 **Mac 빌드 호스트** 의 목적 Sharpie를 사용 하 여 InfColorPicker 프로젝트에 대 한 초기 **ApiDefinition.cs** 를 만듭니다.


-----

시작 하려면이 [가이드](~/cross-platform/macios/binding/objective-sharpie/get-started.md#installing)에 설명 된 대로 목표 Sharpie 설치 관리자 파일을 다운로드 하겠습니다. 설치 관리자를 실행 하 고 설치 마법사의 모든 화면 프롬프트를 따라 개발 컴퓨터에 Sharpie 목표를 설치 합니다.

목표 Sharpie 성공적으로 설치 되 면 터미널 앱을 시작 하 고 다음 명령을 입력 하 여 바인딩을 지원 하기 위해 제공 하는 모든 도구에 대 한 도움말을 봅니다.

```bash
sharpie -help
```

위의 명령을 실행 하면 다음 출력이 생성 됩니다.

```bash
Europa:Resources kmullins$ sharpie -help
usage: sharpie [OPTIONS] TOOL [TOOL_OPTIONS]

Options:
  -h, --helpShow detailed help
  -v, --versionShow version information

Available Tools:

  xcode    Get information about Xcode installations and available SDKs.

  bind     Create a Xamarin C# binding to Objective-C APIs
Europa:Resources kmullins$
```

이 연습에서는 다음과 같은 목적 Sharpie 도구를 사용 합니다.

- **xcode** -이 도구는 현재 xcode 설치 및 설치한 IOS 및 Mac api 버전에 대 한 정보를 제공 합니다. 나중에 바인딩을 생성할 때이 정보를 사용할 예정입니다.
- **바인딩** -이 도구를 사용 하 여 InfColorPicker 프로젝트의 **.h** 파일을 초기 **ApiDefinition.cs** 및 **StructsAndEnums.cs** 파일로 구문 분석 합니다.

특정 목표 Sharpie 도구에 대 한 도움말을 보려면 도구 이름 및 `-help` 옵션을 입력 합니다. 예를 들어 `sharpie xcode -help` 는 다음 출력을 반환 합니다.

```bash
Europa:Resources kmullins$ sharpie xcode -help
usage: sharpie xcode [OPTIONS]+

Options:
  -h, --help                 Show detailed help
  -v, --verbose              Be verbose with output
      --sdks                 List all available Xcode SDKs. Pass -verbose for
                               more details.
Europa:Resources kmullins$
```

바인딩 프로세스를 시작 하기 전에 터미널 `sharpie xcode -sdks`에 다음 명령을 입력 하 여 현재 설치 된 sdk에 대 한 정보를 가져와야 합니다.

```bash
amyb:Desktop amyb$ sharpie xcode -sdks
sdk: appletvos9.2    arch: arm64
sdk: iphoneos9.3     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: watchos2.2      arch: armv7
```

위의 컴퓨터에 `iphoneos9.3` SDK가 설치 되어 있는 것을 볼 수 있습니다. 이 정보를 사용 하 여 infcolorpicker 프로젝트 `.h` 파일을 초기 **ApiDefinition.cs** 및 `StructsAndEnums.cs` infcolorpicker 프로젝트에 대 한 구문 분석할 준비가 되었습니다.

터미널 앱에서 다음 명령을 입력 합니다.

```bash
sharpie bind --output=InfColorPicker --namespace=InfColorPicker --sdk=[iphone-os] [full-path-to-project]/InfColorPicker/InfColorPicker/*.h
```

여기서 `[full-path-to-project]` 는 컴퓨터에 **infcolorpicker** Xcode 프로젝트 파일이 있는 디렉터리의 전체 경로이 고 [iphone-os]는 `sharpie xcode -sdks` 명령에 명시 된 대로 설치한 iOS SDK입니다. 이 예제에서는이 디렉터리의 *모든* 헤더 파일을 포함 하는 매개 변수로  **\*h** 를 전달 했습니다. 일반적으로이 작업을 수행 하면 안 됩니다. 대신 헤더 파일을 자세히 읽어 가장 먼저 확인 해야 합니다 **.** 파일은 다른 모든 관련 파일을 참조 하 고이를 목표 Sharpie 전달 하기만 하면 됩니다.

터미널에서 생성 되는 [출력](walkthrough-images/os05.png) 은 다음과 같습니다.

```bash
Europa:Resources kmullins$ sharpie bind -output InfColorPicker -namespace InfColorPicker -sdk iphoneos8.1 /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h -unified
Compiler configuration:
    -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk -miphoneos-version-min=8.1 -resource-dir /Library/Frameworks/ObjectiveSharpie.framework/Versions/1.1.1/clang-resources -arch armv7 -ObjC

[  0%] parsing /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h
In file included from /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h:60:
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:28:1: warning: no 'assign',
      'retain', or 'copy' attribute is specified - 'assign' is assumed [-Wobjc-property-no-attribute]
@property (nonatomic) UIColor* sourceColor;
^
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:28:1: warning: default property
      attribute 'assign' not appropriate for non-GC object [-Wobjc-property-no-attribute]
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:29:1: warning: no 'assign',
      'retain', or 'copy' attribute is specified - 'assign' is assumed [-Wobjc-property-no-attribute]
@property (nonatomic) UIColor* resultColor;
^
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:29:1: warning: default property
      attribute 'assign' not appropriate for non-GC object [-Wobjc-property-no-attribute]
4 warnings generated.
[100%] parsing complete
[bind] InfColorPicker.cs
Europa:Resources kmullins$
```

**InfColorPicker.enums.cs** 및 **InfColorPicker.cs** 파일은 디렉터리에 생성 됩니다.

[![](walkthrough-images/os06.png "InfColorPicker.enums.cs 및 InfColorPicker.cs 파일")](walkthrough-images/os06.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)


위에서 만든 바인딩 프로젝트에서 이러한 파일을 모두 엽니다. **InfColorPicker.cs** 파일의 내용을 복사 하 고 **ApiDefinition.cs** 파일에 붙여넣어 `namespace ...` 기존 코드 블록을 **InfColorPicker.cs** 파일 `using` 의 내용으로 바꿉니다. 그대로 유지):

![](walkthrough-images/os07.png "InfColorPickerControllerDelegate 파일")


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


위에서 만든 바인딩 프로젝트에서 이러한 파일을 모두 엽니다. **Mac 빌드 호스트**에서 **InfColorPicker.cs** 파일의 내용을 복사 하 고 **ApiDefinition.cs** 파일에 붙여넣어 기존 `namespace ...` 코드 블록을 **InfColorPicker.cs** 파일의 내용으로 바꿉니다 ( `using` 문을 그대로 유지 합니다.)


-----

<a name="Normalize_the_API_Definitions"/>

## <a name="normalize-the-api-definitions"></a>API 정의 정규화

목표 Sharpie 때때로 변환 `Delegates`하는 경우에는 `InfColorPickerControllerDelegate` 인터페이스의 정의를 수정 하 고 `[Protocol, Model]` 줄을 다음으로 바꾸어야 합니다.

```csharp
[BaseType(typeof(NSObject))]
[Model]
```

정의가 다음과 같이 표시 됩니다.

[![](walkthrough-images/os11.png "정의")](walkthrough-images/os11.png#lightbox)

다음으로 `InfColorPicker.enums.cs` 파일의 내용으로 동일한 작업을 수행 하 고 `StructsAndEnums.cs` 파일에 복사 하 여 붙여 넣는 방식으로 `using` 문을 그대로 둡니다.

[![](walkthrough-images/os09.png "StructsAndEnums.cs 파일의 내용입니다.")](walkthrough-images/os09.png#lightbox)

또한 목표 Sharpie 특성을 사용 `[Verify]` 하 여 바인딩에 주석이 추가 되었음을 알 수 있습니다. 이러한 특성은 바인딩을 원래 C/목표값-C 선언과 비교 하 여 (바인딩된 선언 위의 설명에 제공 됨) 목표 Sharpie이 올바른 것을 확인 해야 함을 의미 합니다. 바인딩을 확인 한 후에는 verify 특성을 제거 해야 합니다. 자세한 내용은 [확인](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) 가이드를 참조 하세요.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)


이 시점에서 바인딩 프로젝트를 완료 하 고 빌드할 준비가 완료 되어야 합니다. 바인딩 프로젝트를 빌드하고 오류 없이 종료 되었는지 확인 합니다.

[바인딩 프로젝트를 빌드하고 오류가 없는지 확인 합니다.](walkthrough-images/os12.png)


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


이 시점에서 바인딩 프로젝트를 완료 하 고 빌드할 준비가 완료 되어야 합니다. 바인딩 프로젝트를 빌드하고 오류 없이 종료 되었는지 확인 합니다.


-----

<a name="Using_the_Binding"/>

## <a name="using-the-binding"></a>바인딩 사용

위에서 만든 iOS 바인딩 라이브러리를 사용 하는 샘플 iPhone 응용 프로그램을 만들려면 다음 단계를 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Xamarin.ios 프로젝트 만들기** -다음 스크린샷에 표시 된 것 처럼 솔루션에 **Infcolorpickersample** 이라는 새 xamarin.ios 프로젝트를 추가 합니다.

    ![](walkthrough-images/use01.png "단일 뷰 앱 추가")

    ![](walkthrough-images/use01a.png "식별자 설정")

1. **바인딩 프로젝트에 참조 추가** -Infcolor **kerbinding** 프로젝트에 대 한 참조를 포함 하도록 **infcolorpickersample** 프로젝트를 업데이트 합니다.

    ![](walkthrough-images/use02.png "바인딩 프로젝트에 참조 추가")

1. **IPhone 사용자 인터페이스 만들기** - **Infcolormainstoryboard.storyboard 샘플** 프로젝트에서 **storyboard** 파일을 두 번 클릭 하 여 iOS 디자이너에서 편집 합니다. 다음과 같이 보기에 **단추** 를 추가 하 고 `ChangeColorButton`호출 합니다.

    ![](walkthrough-images/use03.png "뷰에 단추 추가")

1. **Xib** -InfColorPicker 목표-C 라이브러리에 **xib** 파일이 포함 되어 있습니다. Xamarin.ios는 바인딩 프로젝트에이 **xib** 을 포함 하지 않으므로 샘플 응용 프로그램에서 런타임 오류가 발생 합니다. 이에 대 한 해결 방법은 Xamarin.ios 프로젝트에 **xib** 파일을 추가 하는 것입니다. Xamarin.ios 프로젝트를 선택 하 고 마우스 오른쪽 단추를 클릭 한 다음 **추가 > 파일**추가를 선택 하 고 다음 스크린샷에 표시 된 대로 xib 파일을 추가 **합니다** .

    ![](walkthrough-images/use04.png "Infcolorxib를 추가 합니다.")

1. 메시지가 표시 되 면 프로젝트에 **xib** 파일을 복사 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **Xamarin.ios 프로젝트 만들기** - **단일 뷰 응용 프로그램** 템플릿을 사용 하 여 **infcolor kersample** 이라는 새 xamarin.ios 프로젝트를 추가 합니다.

    [![iOS 앱 (Xamarin) 프로젝트](walkthrough-images/use01.w157-sml.png)](walkthrough-images/use01.w157.png#lightbox)

    [![템플릿 선택](walkthrough-images/use01-2.w157-sml.png)](walkthrough-images/use01-2.w157.png#lightbox)

1. **바인딩 프로젝트에 참조 추가** -Infcolor **kerbinding** 프로젝트에 대 한 참조를 포함 하도록 **infcolorpickersample** 프로젝트를 업데이트 합니다.

    ![](walkthrough-images/use02vs.png "바인딩 프로젝트에 참조 추가")

1. **IPhone 사용자 인터페이스 만들기** - **Infcolormainstoryboard.storyboard 샘플** 프로젝트에서 **storyboard** 파일을 두 번 클릭 하 여 iOS 디자이너에서 편집 합니다. 다음과 같이 보기에 **단추** 를 추가 하 고 `ChangeColorButton`호출 합니다.

    ![](walkthrough-images/use03vs.png "IPhone 사용자 인터페이스 만들기")

1. **Xib** -InfColorPicker 목표-C 라이브러리에 **xib** 파일이 포함 되어 있습니다. Xamarin.ios는 바인딩 프로젝트에이 **xib** 을 포함 하지 않으므로 샘플 응용 프로그램에서 런타임 오류가 발생 합니다. 이에 대 한 해결 방법은 **Mac 빌드 호스트**의 xamarin.ios 프로젝트에 **xib** 파일을 추가 하는 것입니다. Xamarin.ios 프로젝트를 선택 하 고 마우스 오른쪽 단추를 클릭 한 다음**기존 항목** **추가** > ...를 선택 하 고 **xib** 파일을 추가 합니다.

-----

다음으로, 목표-C의 프로토콜과 바인딩 및 C# 코드에서이를 처리 하는 방법에 대해 간단히 살펴보겠습니다.

### <a name="protocols-and-xamarinios"></a>프로토콜 및 Xamarin.ios

목표-C에서 프로토콜은 특정 환경에서 사용할 수 있는 메서드 (또는 메시지)를 정의 합니다. 개념적으로는의 C#인터페이스와 매우 비슷합니다. 목적과 인터페이스의 C# 주요 차이점은 프로토콜에는 클래스가 구현 하지 않아도 되는 선택적 메서드 메서드를 사용할 수 있다는 것입니다. 목적-C @optional 는 키워드를 사용 하 여 선택적 메서드를 표시 합니다. 프로토콜에 대 한 자세한 내용은 [이벤트, 프로토콜 및 대리자를](~/ios/app-fundamentals/delegates-protocols-and-events.md)참조 하세요.

**Infcolor의 Kercontroller** 에는 아래 코드 조각에 표시 된 이러한 프로토콜이 하나 있습니다.

```csharp
@protocol InfColorPickerControllerDelegate

@optional

- (void) colorPickerControllerDidFinish: (InfColorPickerController*) controller;
// This is only called when the color picker is presented modally.

- (void) colorPickerControllerDidChangeColor: (InfColorPickerController*) controller;

@end
```

이 프로토콜은 **Infcolor kercontroller** 에서 사용자가 새 색을 선택 하 고 **infcolorpickercontroller** 가 완료 되었음을 클라이언트에 알리는 데 사용 됩니다. 목표 Sharpie 다음 코드 조각과 같이이 프로토콜을 매핑 했습니다.

```csharp
[BaseType(typeof(NSObject))]
[Model]
public partial interface InfColorPickerControllerDelegate {

    [Export ("colorPickerControllerDidFinish:")]
    void ColorPickerControllerDidFinish (InfColorPickerController controller);

    [Export ("colorPickerControllerDidChangeColor:")]
    void ColorPickerControllerDidChangeColor (InfColorPickerController controller);
}

```

바인딩 라이브러리가 컴파일될 때 xamarin.ios는 가상 메서드를 사용 하 여이 인터페이스를 구현 하 `InfColorPickerControllerDelegate`는 라는 추상 기본 클래스를 만듭니다.

Xamarin.ios 응용 프로그램에서이 인터페이스를 구현할 수 있는 방법에는 두 가지가 있습니다.

- **강력한 대리자** -강력한 대리자를 사용 하는 경우 C# 적절 한 메서드 `InfColorPickerControllerDelegate` 를 서브 클래스 하 고 재정의 하는 클래스를 만들어야 합니다. **Infcolorpickercontroller** 는이 클래스의 인스턴스를 사용 하 여 클라이언트와 통신 합니다.
- **약한 대리자** -약한 대리자는 일부 클래스 (예: `InfColorPickerSampleViewController`)에서 공용 메서드를 만든 다음 `Export` 특성을 통해 `InfColorPickerDelegate` 해당 메서드를 프로토콜에 노출 하는 약간 다른 기술입니다.

강력한 대리자는 Intellisense, 형식 안전성 및 더 나은 캡슐화 기능을 제공 합니다. 이러한 이유로 weak 대리자 대신 가능한 경우 강력한 대리자를 사용 해야 합니다.

이 연습에서는 먼저 강력한 대리자를 구현한 다음 weak 대리자를 구현 하는 방법을 설명 합니다.

### <a name="implementing-a-strong-delegate"></a>강력한 대리자 구현

강력한 대리자를 사용 하 여 `colorPickerControllerDidFinish:` 메시지에 응답 하 여 xamarin.ios 응용 프로그램을 완료 합니다.

**하위 클래스 InfColorPickerControllerDelegate** -라는 `ColorSelectedDelegate`프로젝트에 새 클래스를 추가 합니다. 다음 코드를 포함 하도록 클래스를 편집 합니다.

```csharp
using InfColorPickerBinding;
using UIKit;

namespace InfColorPickerSample
{
  public class ColorSelectedDelegate:InfColorPickerControllerDelegate
  {
    readonly UIViewController parent;

    public ColorSelectedDelegate (UIViewController parent)
    {
      this.parent = parent;
    }

    public override void ColorPickerControllerDidFinish (InfColorPickerController controller)
    {
      parent.View.BackgroundColor = controller.ResultColor;
      parent.DismissViewController (false, null);
    }
  }
}
```

Xamarin.ios는 라는 `InfColorPickerControllerDelegate`추상 기본 클래스를 만들어 목표-C 대리자를 바인딩합니다. 이 형식의 서브 클래스를 만들고 `ColorPickerControllerDidFinish` 메서드를 재정의 하 여의 `ResultColor` `InfColorPickerController`속성 값에 액세스 합니다.

**Colorselecteddelegate의 인스턴스를 만듭니다** . 이벤트 처리기에는 이전 단계에서 만든 `ColorSelectedDelegate` 형식의 인스턴스가 필요 합니다. 클래스 `InfColorPickerSampleViewController` 를 편집 하 고 다음 인스턴스 변수를 클래스에 추가 합니다.

```csharp
ColorSelectedDelegate selector;
```

**Colorselecteddelegate 변수를 초기화 합니다** . `selector` 가 유효한 인스턴스인지 확인 하려면 다음 코드 조각과 일치 하도록 `ViewDidLoad` 의 `ViewController` 메서드를 업데이트 합니다.

```csharp
public override void ViewDidLoad ()
{
  base.ViewDidLoad ();
  ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithStrongDelegate;
  selector = new ColorSelectedDelegate (this);
}
```

**HandleTouchUpInsideWithStrongDelegate 메서드를 구현** 합니다. 그러면 사용자가 **colorchangebutton 단추**를 사용할 때에 대 한 이벤트 처리기를 구현 합니다. 를 `ViewController`편집 하 고 다음 메서드를 추가 합니다.

```csharp
using InfColorPicker;
...

private void HandleTouchUpInsideWithStrongDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.Delegate = selector;
    picker.PresentModallyOverViewController (this);
}

```

먼저 정적 메서드를 `InfColorPickerController` 통해 인스턴스를 가져오고 해당 인스턴스가 속성 `InfColorPickerController.Delegate`을 통해 강력한 대리자를 인식 하도록 합니다. 이 속성은 Sharpie 목표에 따라 자동으로 생성 되었습니다. 마지막으로를 `PresentModallyOverViewController` 호출 하 여 사용자 `InfColorPickerSampleViewController.xib` 가 색을 선택할 수 있도록 뷰를 표시 합니다.

**응용 프로그램을 실행** 합니다 .이 시점에서 모든 코드를 사용 하 여 완료 됩니다. 응용 프로그램을 실행 하는 경우 다음 스크린샷에 표시 된 `InfColorColorPickerSampleView` 것 처럼의 배경색을 변경할 수 있습니다.

[![](walkthrough-images/run01.png "응용 프로그램 실행")](walkthrough-images/run01.png#lightbox)

지금까지 이 시점에서 Xamarin.ios 응용 프로그램에서 사용할 목적-C 라이브러리를 성공적으로 만들고 바인딩 했습니다. 다음으로 약한 대리자를 사용 하는 방법을 살펴보겠습니다.

### <a name="implementing-a-weak-delegate"></a>약한 대리자 구현

특정 대리자의 목적-C 프로토콜에 바인딩된 클래스를 서브클래싱 하는 대신 xamarin.ios를 사용 하 여에서 `NSObject`파생 되는 모든 클래스에서 프로토콜 메서드를 구현 하 고 `ExportAttribute`, 메서드를로 데코레이팅하 며, 다음을 수행할 수도 있습니다. 적절 한 선택기를 제공 합니다. 이 방법을 사용 하는 경우 `WeakDelegate` `Delegate` 속성 대신 클래스의 인스턴스를 속성에 할당 합니다. 약한 대리자는 대리자 클래스를 다른 상속 계층 구조에서 사용할 수 있는 유연성을 제공 합니다. Xamarin.ios 응용 프로그램에서 weak 대리자를 구현 하 고 사용 하는 방법을 살펴보겠습니다.

**TouchUpInside에 대 한 이벤트 처리기 만들기** -배경 색 변경 단추의 `TouchUpInside` 이벤트에 대 한 새 이벤트 처리기를 만듭니다. 이 처리기는 이전 섹션에서 만든 `HandleTouchUpInsideWithStrongDelegate` 처리기와 동일한 역할을 채우지만 강력한 대리자 대신 weak 대리자를 사용 합니다. 클래스 `ViewController`를 편집 하 고 다음 메서드를 추가 합니다.

```csharp
private void HandleTouchUpInsideWithWeakDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.WeakDelegate = this;
    picker.SourceColor = this.View.BackgroundColor;
    picker.PresentModallyOverViewController (this);
}
```

**ViewDidLoad 업데이트** -방금 만든 이벤트 `ViewDidLoad` 처리기를 사용 하도록 변경 해야 합니다. 를 `ViewController` 편집 하 `ViewDidLoad` 고 다음 코드 조각과 유사 하 게 변경 합니다.


```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithWeakDelegate;
}

```

**ColorPickerControllerDidFinish를 처리 합니다. 메시지** - `ViewController` 이 완료 되 면 iOS는로 메시지 `colorPickerControllerDidFinish:` 를 `WeakDelegate`보냅니다. 이 메시지를 처리할 수 C# 있는 메서드를 만들어야 합니다. 이렇게 하려면 메서드를 C# 만든 다음를 사용 `ExportAttribute`하 여 장식할 합니다. 를 `ViewController`편집 하 고 다음 메서드를 클래스에 추가 합니다.

```csharp
[Export("colorPickerControllerDidFinish:")]
public void ColorPickerControllerDidFinish (InfColorPickerController controller)
{
    View.BackgroundColor = controller.ResultColor;
    DismissViewController (false, null);
}

```

애플리케이션을 실행합니다. 이제 이전과 정확히 동일 하 게 동작 하지만 강력한 대리자 대신 weak 대리자를 사용 합니다. 이 시점에서이 연습을 성공적으로 완료 했습니다. 이제 Xamarin.ios 바인딩 프로젝트를 만들고 사용 하는 방법을 이해 해야 합니다.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 바인딩 프로젝트를 만들고 사용 하는 과정을 살펴보았습니다. 먼저 기존 목표 C 라이브러리를 정적 라이브러리로 컴파일하는 방법에 대해 설명 했습니다. 그런 다음, Xamarin.ios 바인딩 프로젝트를 만드는 방법 및 목표 Sharpie를 사용 하 여 목표-C 라이브러리에 대 한 API 정의를 생성 하는 방법을 살펴보았습니다. 생성 된 API 정의를 업데이트 하 고 조정 하 여 공용 사용에 적합 하 게 만드는 방법을 설명 했습니다. Xamarin.ios 바인딩 프로젝트가 완료 된 후에는 Xamarin 응용 프로그램에서 해당 바인딩을 사용 하기 위해 이동 했으며, 강력한 대리자 및 약한 대리자 사용에 중점을 두고 있습니다.

## <a name="related-links"></a>관련 링크

- [바인딩 예제 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/infcolorpicker)
- [Objective-C 라이브러리 바인딩](~/cross-platform/macios/binding/objective-c-libraries.md)
- [바인딩 세부 정보](~/cross-platform/macios/binding/overview.md)
- [바인딩 형식 참조 가이드](~/cross-platform/macios/binding/binding-types-reference.md)
- [Objective-C 개발자용 Xamarin](~/ios/get-started/objective-c-developers/index.md)
- [프레임워크 디자인 지침](https://msdn.microsoft.com/library/ms229042.aspx)
