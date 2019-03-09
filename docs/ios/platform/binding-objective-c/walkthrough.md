---
title: '연습: IOS Objective-c 라이브러리 바인딩'
description: 이 문서는 기존 Objective-c 라이브러리 InfColorPicker에 대 한 Xamarin.iOS 바인딩을 만드는 실습 연습을 제공 합니다. 정적 Objective-c 라이브러리 바인딩, 컴파일하고 Xamarin.iOS 응용 프로그램에 바인딩 사용 같은 주제를 다룹니다.
ms.prod: xamarin
ms.assetid: D3F6FFA0-3C4B-4969-9B83-B6020B522F57
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/02/2017
ms.openlocfilehash: fcf4e6d9b281eaac4be888c499e537f7397528a0
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669273"
---
# <a name="walkthrough-binding-an-ios-objective-c-library"></a>연습: IOS Objective-c 라이브러리 바인딩

_이 문서는 기존 Objective-c 라이브러리 InfColorPicker에 대 한 Xamarin.iOS 바인딩을 만드는 실습 연습을 제공 합니다. 정적 Objective-c 라이브러리 바인딩, 컴파일하고 Xamarin.iOS 응용 프로그램에 바인딩 사용 같은 주제를 다룹니다._

IOS에서 작동 하는 경우 타사 Objective-c 라이브러리를 사용 하려는 경우 발생할 수 있습니다. 이러한 상황에서는 Xamarin.iOS를 사용할 수 있습니다 _바인딩 프로젝트가_ 만들려는 [ C# 바인딩](~/cross-platform/macios/binding/overview.md) Xamarin.iOS 응용 프로그램에서 라이브러리를 사용할 수 있도록 합니다.

일반적으로 iOS 에코 시스템에서의 3 가지 라이브러리를 찾을 수 있습니다.

* 사용 하 여 미리 컴파일된 정적 라이브러리 파일로 `.a` 해당 헤더 (.h 파일)와 함께 확장 합니다. 예를 들어 [Google Analytics 라이브러리](https://developers.google.com/analytics/devguides/collection/ios/v3/sdk-download?hl=es#download_sdk)
* 미리 컴파일된 프레임 워크입니다. 이 정적 라이브러리, 헤더 및 사용 하 여 경우에 따라 추가 리소스를 포함 하는 폴더만 `.framework` 확장 합니다. 예를 들어 [Google AdMob 라이브러리](https://developers.google.com/admob/ios/download)합니다.
* 간단히 소스 코드 파일입니다. 예를 들어,만 포함 하는 라이브러리 `.m` 고 `.h` Objective C 파일.

첫 번째와 두 번째 시나리오에서는 이미 있습니다 미리 컴파일된 CocoaTouch 정적 라이브러리가 문서의 세 번째 시나리오에 중점. 바인딩을 만들 항상 바인딩할 수 있는지 확인 하는 라이브러리를 사용 하 여 제공 된 라이선스 확인을 시작 하기 전에 해야 합니다.

이 문서에서는 오픈 소스를 사용 하 여 바인딩 프로젝트 만들기의 단계별 연습은 [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) Objective-c로 사용 하 여이 가이드의 모든 정보는 사용 하기 위해 조정할 수 있지만 예를 들어, 프로젝트 타사 Objective-c 라이브러리입니다. InfColorPicker 라이브러리 사용자 친화적인 색 선택 영역을 만드는, HSB 표현에 따라 색을 선택할 수 있는 재사용 가능한 뷰 컨트롤러를 제공 합니다.

[![](walkthrough-images/run01.png "IOS에서 실행 중인 InfColorPicker 라이브러리의 예")](walkthrough-images/run01.png#lightbox)

Xamarin.iOS에서이 특정 Objective C API를 사용 하는 데 필요한 모든 단계를 다룹니다.

- 첫째, Xcode를 사용 하 여 Objective-c로 정적 라이브러리를 만듭니다.
- 그런 다음 Xamarin.iOS 사용 하 여 정적 라이브러리가 바인딩하기만 됩니다.
- 다음으로, 목표 Sharpie 일부 (전부가 아님)를 자동으로 생성 하 여 워크 로드를 줄일 수 있는 방법을 보여 줍니다 Xamarin.iOS 바인딩에서 필요한 필요한 API 정의 합니다.
- 마지막으로 바인딩을 사용 하는 Xamarin.iOS 응용 프로그램을 만듭니다.

샘플 응용 프로그램에서는 InfColorPicker API 간의 통신을 위한 강력한 대리자를 사용 하는 방법을 보여 줍니다 및 C# 코드입니다. 강력한 대리자를 사용 하는 방법을 살펴본 후 약한 대리자를 사용 하 여 동일한 작업을 수행 하는 방법을 설명 합니다.

## <a name="requirements"></a>요구 사항

이 문서에서는 Xcode 및 Objective-c 언어 사용 경험이 있고 읽었다고 가정 우리의 [Objective-c 바인딩](~/cross-platform/macios/binding/index.md) 설명서. 또한 다음은 제시 된 단계를 완료 하려면:

-  **Xcode 및 iOS SDK** -Apple의 Xcode 및 최신 iOS API 설치 하 고 개발자의 컴퓨터에서 구성 해야 합니다.
-  **[Xcode 명령줄 도구](#Installing_the_Xcode_Command_Line_Tools)**  -Xcode (설치 세부 정보에 대 한 아래 참조)의 현재 설치 된 버전의 Xcode 명령줄 도구를 설치 해야 합니다.
-  **Visual Studio 또는 Mac 용 visual Studio** -최신 버전의 Visual Studio 또는 Mac 용 Visual Studio를 설치 하 고 개발 컴퓨터에 구성 해야 합니다. Apple Mac는 Xamarin.iOS 응용 프로그램을 개발 하기 위한 필수 항목이 며 연결을 해야 Visual Studio를 사용 하는 경우 [Xamarin.iOS 빌드 호스트](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
-  **최신 버전의 목적은 Sharpie** -에서 다운로드 한 목표 Sharpie 도구의 현재 복사본 [여기](~/cross-platform/macios/binding/objective-sharpie/get-started.md)합니다. 설치 하는 목표 Sharpie에 이미 있는 경우 업데이트할 수 있습니다 최신 버전을 사용 하 여는 `sharpie update`

<a name="Installing_the_Xcode_Command_Line_Tools"/>

## <a name="installing-the-xcode-command-line-tools"></a>Xcode 명령 줄 도구 설치

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)


위에서 설명한 대로 사용 Xcode 명령줄 도구 (특히 `make` 고 `lipo`)이 연습의 합니다. `make` 명령은 실행 프로그램 및 라이브러리의 컴파일을 사용 하 여 자동화할 수 있는 매우 일반적인 Unix 유틸리티는 _메이크파일_ 프로그램을 작성 해야 하는 방법을 지정 하는 합니다. 합니다 `lipo` 명령 OS X 명령줄 유틸리티를 사용 하 여 다중 아키텍처 파일 만들기에 여러 결합이 `.a` 모든 하드웨어 아키텍처에서 사용할 수 있는 하나의 파일로 파일입니다.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


위에서 설명한 대로 사용 Xcode 명령줄 도구에는 **Mac 빌드 호스트** (특히 `make` 및 `lipo`)이 연습의 합니다. 합니다 `make` 명령은 실행 프로그램 및 라이브러리의 컴파일을 사용 하 여 자동화할 수 있는 매우 일반적인 Unix 유틸리티는 _메이크파일_ 프로그램을 빌드하는 방법을 지정 하 합니다. 합니다 `lipo` 명령 OS X 명령줄 유틸리티를 사용 하 여 다중 아키텍처 파일 만들기에 여러 결합이 `.a` 모든 하드웨어 아키텍처에서 사용할 수 있는 하나의 파일로 파일입니다.


-----

Apple에 따라 [Xcode FAQ를 사용 하 여 명령줄에서 빌드](https://developer.apple.com/library/ios/technotes/tn2339/_index.html) 설명서, OS X 10.9 이상, **다운로드** Xcode의 창 **기본** 대화 없습니다 더 이상 다운로드 명령줄 도구를 지원 합니다.

도구를 설치 하려면 다음 방법 중 하나를 사용 해야 합니다.

- **Xcode 설치** -Xcode를 설치 하는 경우 모든 명령줄 도구를 사용 하 여 번들로 제공 됩니다. OS X 10.9에서 shim (설치 `/usr/bin`)에 포함 된 모든 도구를 매핑할 수 `/usr/bin` Xcode 내에서 해당 도구. 예를 들어를 `xcrun` 명령을 찾을 수 없거나 명령줄에서 Xcode 내에서 모든 도구를 실행할 수 있습니다.
- **터미널 응용 프로그램** -터미널 응용 프로그램에서 실행 하 여 명령줄 도구를 설치할 수는 `xcode-select --install` 명령:
    - 터미널 응용 프로그램을 시작 합니다.
    - 형식 `xcode-select --install` 키를 누릅니다 **Enter**예를 들어:

    ```bash
    Europa:~ kmullins$ xcode-select --install
    ```

    - 메시지가 표시 됩니다 명령줄 도구를 설치 하려면를 클릭 합니다 **설치** 단추:   [![](walkthrough-images/xcode01.png "명령줄 도구를 설치합니다.")](walkthrough-images/xcode01.png#lightbox)

    - 도구를 다운로드 하 고 Apple 서버에서 설치 됩니다.   [![](walkthrough-images/xcode02.png "도구 다운로드")](walkthrough-images/xcode02.png#lightbox)

- **Apple 개발자를 위한 다운로드** -명령줄 도구 패키지를 사용할 수는 [Apple 개발자를 위한 다운로드](https://developer.apple.com/downloads/index.action) 웹 페이지입니다. Apple ID를 사용 하 여 로그인 하 고 검색할 명령줄 도구를 다운로드 합니다. [![](walkthrough-images/xcode03.png "명령줄 도구 찾기")](walkthrough-images/xcode03.png#lightbox)

명령줄 도구를 설치 하는 것에서 연습을 진행할 준비가 된 것입니다.

## <a name="walkthrough"></a>연습

이 연습에서는 다음 단계를 다룹니다.

- **[정적 라이브러리를 만드는](#Creating_A_Static_Library)**  -이 단계에서는 정적 라이브러리를 작성 합니다 **InfColorPicker** Objective-c 코드입니다. 정적 라이브러리 갖습니다는 `.a` 파일 확장명 및 라이브러리 프로젝트의.NET 어셈블리에 포함 됩니다.
- **[Xamarin.iOS 바인딩 프로젝트를 만듭니다](#Create_a_Xamarin.iOS_Binding_Project)**  -정적 라이브러리에 있으면 Xamarin.iOS 바인딩 프로젝트를 만들려면 사용할 것입니다. 바인딩 프로젝트를 구성 하 고 방금 만든 정적 라이브러리 형태로 메타 데이터의 C# Objective C API를 사용할 수 있는 방법을 설명 하는 코드입니다. 이 메타 데이터를 일반적으로 API 정의 라고 합니다. 사용 하 여 **[목표 Sharpie](#Using_Objective_Sharpie)** 는 데 사용 하 여 API 정의 만듭니다.
- **[API 정의 정규화](#Normalize_the_API_Definitions)**  -목표 Sharpie 도움을 잘 않습니다 하지만 모든 작업을 수행할 수 없습니다. 사용할 수 있으려면 먼저 API 정의를 변경 해야 하는 몇 가지 변경 내용을 설명 하겠습니다.
- **[바인딩 라이브러리를 사용 하 여](#Using_the_Binding)**  -마지막으로 새로 만든된 바인딩 프로젝트를 사용 하는 방법을 보여 주는 Xamarin.iOS 응용 프로그램을 만듭니다.

관련 된 단계를 이해 했으므로에 대해 알아 보며 연습의 나머지 부분입니다.

<a name="Creating_A_Static_Library"/>

## <a name="creating-a-static-library"></a>정적 라이브러리 만들기

Github에서 InfColorPicker에 대 한 코드 검사 하면:

[![](walkthrough-images/image02.png "Github에서 InfColorPicker에 대 한 코드를 검사 합니다.")](walkthrough-images/image02.png#lightbox)

다음 세 가지 디렉터리 프로젝트에서 볼 수 있습니다.

- **InfColorPicker** -이 디렉터리를 프로젝트에 대 한 Objective-c 코드를 포함 합니다.
- **PickerSamplePad** -이 디렉터리에 샘플 iPad 프로젝트가 포함 되어 있습니다.
- **PickerSamplePhone** -이 디렉터리에 샘플 iPhone 프로젝트가 포함 되어 있습니다.

InfColorPicker 프로젝트를 다운로드 합시다 [GitHub](https://github.com/InfinitApps/InfColorPicker/archive/master.zip) 및 바꿔도의 디렉터리에 압축을 풉니다. 에 대 한 Xcode 대상 열어 `PickerSamplePhone` 프로젝트를 Xcode 탐색 창에서 다음과 같은 프로젝트 구조 표시:

[![](walkthrough-images/image03.png "Xcode 탐색기의 프로젝트 구조")](walkthrough-images/image03.png#lightbox)

이 프로젝트는 직접 각 샘플 프로젝트로 (빨간색 상자)에서 InfColorPicker 소스 코드를 추가 하 여 코드 재사용을 달성 합니다. 샘플 프로젝트에 대 한 코드 파랑 상자 안에 있는 경우 에 필요한 것이 특정 프로젝트 정적 라이브러리를 사용 하 여 의견을 제공 하지 않으므로, 정적 라이브러리를 컴파일합니다 Xcode 프로젝트 만들기.

첫 번째 단계는 정적 라이브러리로 InfoColorPicker 소스 코드를 추가할입니다. 이제 이렇게 하려면 다음을 수행 합니다.

1. Xcode를 시작합니다.
2. **파일** 메뉴 선택 **새로 만들기** > **프로젝트...** :

    [![](walkthrough-images/image04.png "새 프로젝트를 시작합니다.")](walkthrough-images/image04.png#lightbox)
3. 선택 **프레임 워크 및 라이브러리**서 **Cocoa Touch 정적 라이브러리** 서식 파일을 클릭 합니다 **다음** 단추:

    [![](walkthrough-images/image05.png "정적 라이브러리 Cocoa Touch 템플릿 선택")](walkthrough-images/image05.png#lightbox)

4. 입력 `InfColorPicker` 에 대 한 합니다 **프로젝트 이름** 을 클릭 합니다 **다음** 단추:

    [![](walkthrough-images/image06.png "InfColorPicker 프로젝트 이름 입력")](walkthrough-images/image06.png#lightbox)
5. 프로젝트를 저장 하 고 클릭 위치를 선택 합니다 **확인** 단추입니다.
6. 이제 정적 라이브러리 프로젝트에 InfColorPicker 프로젝트에서 원본을 추가 해야 합니다. 때문에 합니다 **InfColorPicker.h** 파일에에서 이미 정적 라이브러리 (기본적으로), Xcode를 덮어 쓸까요 허용 하지 않습니다. **Finder**, GitHub에서 압축을 푼에서는 원래 프로젝트에서 InfColorPicker 소스 코드로 이동, 모든 InfColorPicker 파일 복사 및 새 정적 라이브러리 프로젝트에 붙여넣습니다.

    [![](walkthrough-images/image12.png "모든 InfColorPicker 파일 복사")](walkthrough-images/image12.png#lightbox)

7. Xcode 돌아갑니다, 마우스 오른쪽 단추로 클릭 합니다 **InfColorPicker** 선택한 폴더 **"InfColorPicker..."에 파일을 추가할**:

    [![](walkthrough-images/image08.png "파일 추가")](walkthrough-images/image08.png#lightbox)

8. 파일 추가 대화 상자에서 방금 복사한 InfColorPicker 소스 코드 파일을 이동, 모든 모두 선택 하 고 클릭 합니다 **추가** 단추:

    [![](walkthrough-images/image09.png "모두 선택 하 고 추가 단추를 클릭 합니다.")](walkthrough-images/image09.png#lightbox)

9. 소스 코드 프로젝트에 복사 됩니다.

    [![](walkthrough-images/image10.png "소스 코드 프로젝트에 복사 됩니다.")](walkthrough-images/image10.png#lightbox)

10. Xcode 프로젝트 탐색기에서 선택 합니다 **InfColorPicker.m** 파일과 마지막 두 줄을 주석 처리 (때문에이 라이브러리 쓰여진 방법과,이 파일이 사용 되지 않습니다).

    [![](walkthrough-images/image14.png "InfColorPicker.m 파일 편집")](walkthrough-images/image14.png#lightbox)

11. 이제 라이브러리에 필요한 모든 프레임 워크 있는지 확인 해야 합니다. 추가 정보, 또는 제공 된 샘플 프로젝트 중 하나를 열면이 정보를 찾을 수 있습니다. 이 예제에서는 `Foundation.framework`, `UIKit.framework`, 및 `CoreGraphics.framework` 추가 해 보겠습니다.

12. 선택 된 **InfColorPicker 대상 > 빌드 단계** 확장을 **이진 파일과 라이브러리 연결** 섹션:

    [![](walkthrough-images/image16b.png "이진 파일과 라이브러리 연결 섹션을 확장")](walkthrough-images/image16b.png#lightbox)

13. 사용 된 **+** 단추 위에 나열 된 필수 프레임 프레임 워크를 추가할 수 있도록 대화 상자를 엽니다.

    [![](walkthrough-images/image16c.png "위에 나열 된 필수 프레임 프레임 워크 추가")](walkthrough-images/image16c.png#lightbox)

14. 합니다 **이진 파일과 라이브러리 연결** 섹션에 이제 다음 이미지 처럼 보여야 합니다.

    [![](walkthrough-images/image16d.png "이진 파일과 라이브러리 연결 섹션")](walkthrough-images/image16d.png#lightbox)

이 시점에서는 닫기 하지만 않음 완료. 정적 라이브러리에 생성 되었지만 Fat 모든 iOS 장치 및 iOS 시뮬레이터에 대 한 필요한 아키텍처를 포함 하는 이진 만들려면 작성 해야 합니다.

### <a name="creating-a-fat-binary"></a>Fat 이진 파일 만들기

모든 iOS 장치에 시간이 지남에 따라 개발 ARM 아키텍처에서 제공 하는 프로세서 있습니다. 각 새 아키텍처 이전 버전과 호환성 유지 하면서 새 지침 및 기타 향상 된 기능을 추가 합니다. IOS 장치에서 있다고 armv6 armv7, armv7s, arm64 명령 집합 – 있지만 [armv6 더 이상 사용](~/ios/deploy-test/compiling-for-different-devices.md)합니다. IOS 시뮬레이터는 ARM에서 전원이 이며 istead는 x86 및 전원이 x86_64 시뮬레이터입니다. 각 명령에 라이브러리를 제공 해야 합니다는 것을 의미 하는 설정입니다.

Fat 라이브러리를 `.a` 지원 되는 모든 아키텍처를 포함 하는 파일입니다.

fat 이진 만들기는 3 단계 프로세스입니다.

- 정적 라이브러리의 7 ARM 및 ARM64 버전을 컴파일하십시오.
- 정적 라이브러리는 x86 및 x84_64 버전을 컴파일하십시오.
- 사용 된 `lipo` 명령줄 도구를 두 정적 라이브러리를 하나로 결합 합니다.

이 세 단계는 매우 간단 하 고 나중에 업데이트를 수신 하는 Objective-c 라이브러리 또는 버그 수정 해야 하는 경우를 반복 하는 일을 할 수 있습니다. 이러한 단계를 자동화 하려는 경우 향후 유지 관리 및 지원 iOS 바인딩 프로젝트의이 간소화 됩니다.

셸 스크립트를 이러한 작업을 자동화 하는 사용 가능한 도구가 많이 [rake](http://rake.rubyforge.org/)를 [xbuild](https://www.mono-project.com/docs/tools+libraries/tools/xbuild/), 및 [확인](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/make.1.html)합니다. Xcode 명령줄 도구를 설치할 때이 연습에 사용할 빌드 시스템에서 정말 make를 설치도 했습니다. 다음은 **메이크파일** iOS 장치 및 모든 라이브러리에 대 한 시뮬레이터에서 작동 하는 다중 아키텍처 공유 라이브러리를 만드는 데 사용할 수 있는:

```bash
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

입력 합니다 **메이크파일** , 일반 텍스트 편집기에서 명령을 사용 하 여 섹션을 업데이트 하 고 **YOUR 프로젝트 이름** 프로젝트의 이름입니다. 것도 되도록 중요 지침 내에서 탭을 그대로 남아 있을 위의 지침을 붙여 넣을 것입니다.

파일 이름으로 저장 **메이크파일** 위에서 만든 InfColorPicker Xcode 정적 라이브러리와 동일한 위치에 있습니다.

[![](walkthrough-images/lib00.png "메이크파일 이름의 파일을 저장 합니다.")](walkthrough-images/lib00.png#lightbox)

Mac에서 터미널 응용 프로그램을 열고 프로그램 메이크파일의 위치로 이동 합니다. 형식 `make` 키를 눌러 터미널에 **Enter** 하며 **메이크파일** 실행 됩니다.

[![](walkthrough-images/lib01.png "샘플 메이크파일 출력")](walkthrough-images/lib01.png#lightbox)

확인을 실행 하면 스크롤 하는 텍스트가 표시 됩니다. 모든 항목이 제대로 작동 하는 경우에 단어가 표시 됩니다 **빌드에 성공** 하며 `libInfColorPicker-armv7.a`를 `libInfColorPicker-i386.a` 및 `libInfColorPickerSDK.a` 와 동일한 위치에 파일을 복사할를 **메이크파일**:

[![](walkthrough-images/lib02.png "메이크파일에서 생성 한 libInfColorPicker armv7.a, libInfColorPicker i386.a 및 libInfColorPickerSDK.a 파일")](walkthrough-images/lib02.png#lightbox)

Fat 이진 파일에 내에서 다음 명령을 사용 하 여 아키텍처를 확인할 수 있습니다.

```bash
xcrun -sdk iphoneos lipo -info libInfColorPicker.a
```

다음 표시 되어야 합니다.

```bash
Architectures in the fat file: libInfColorPicker.a are: i386 armv7 x86_64 arm64
```

Xcode 및 Xcode 명령줄 도구를 사용 하 여 정적 라이브러리를 만들어 iOS 바인딩에의 첫 번째 단계를 완료 했으므로 시점 `make` 고 `lipo`입니다. 이제 다음 단계로 이동 하 고 사용 **목표 Sharpie** 미국에 대 한 API 바인딩의 만들기를 자동화 합니다.

<a name="Create_a_Xamarin.iOS_Binding_Project"/>

## <a name="create-a-xamarinios-binding-project"></a>Xamarin.iOS 프로젝트 바인딩 만들기

사용 하기 전에 **목표 Sharpie** 바인딩 프로세스를 자동화 하려면 API 정의 저장할 Xamarin.iOS 바인딩 프로젝트를 생성 해야 (사용할 예정입니다 **목표 Sharpie** 도움이 빌드)를 만들어는 C# 미국에 대 한 바인딩.

다음을 수행 하겠습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac 용 Visual Studio를 시작 합니다.
1. **파일** 메뉴에서 **새로 만들기** > **솔루션...** :

    ![](walkthrough-images/bind01.png "새 솔루션 시작")

1. 새 솔루션 대화 상자에서 선택 **라이브러리** > **iOS 프로젝트 바인딩**:

    ![](walkthrough-images/bind02.png "IOS 바인딩 프로젝트를 선택 합니다.")

1. 클릭 합니다 **다음** 단추입니다.

1. "InfColorPickerBinding"으로 입력 합니다 **프로젝트 이름** 클릭 합니다 **만들기** 솔루션을 만드는 단추:

    ![](walkthrough-images/bind02a.png "프로젝트 이름으로 InfColorPickerBinding 입력")

솔루션이 만들어지고 두 개의 기본 파일이 포함 됩니다.

![](walkthrough-images/bind03.png "솔루션 탐색기에서 솔루션 구조")


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


1. Visual Studio를 시작합니다.

1. **파일** 메뉴에서 **새로 만들기** > **프로젝트...** :

    ![새 프로젝트를 시작](walkthrough-images/bind01vs.png "새 프로젝트를 시작 합니다.")

1. 새 프로젝트 대화 상자에서 선택 **시각적 C# > iPhone 및 iPad > iOS 바인딩 라이브러리 (Xamarin)**:

    [![IOS 바인딩 라이브러리를 선택 합니다.](walkthrough-images/bind02.w157-sml.png)](walkthrough-images/bind02.w157.png#lightbox)

1. "InfColorPickerBinding"으로 입력 합니다 **이름** 클릭 합니다 **확인** 솔루션을 만드는 단추.

솔루션이 만들어지고 두 개의 기본 파일이 포함 됩니다.

![](walkthrough-images/bind03vs.png "솔루션 탐색기에서 솔루션 구조")

-----

- **ApiDefinition.cs** -이 파일의 Objective C API 래핑될 하는 방법을 정의 하는 계약에 포함 됩니다 C#합니다.
- **Structs.cs** -이 파일에는 구조가 보유할 또는 열거형 값의 인터페이스 및 대리자에 필요 합니다.

이 연습의 뒷부분에서이 두 파일을 사용 하 여 작업할 것입니다. 먼저, InfColorPicker 라이브러리 바인딩 프로젝트를 추가 해야 합니다.

### <a name="including-the-static-library-in-the-binding-project"></a>바인딩 프로젝트에 정적 라이브러리를 포함합니다.

준비 기본 바인딩 프로젝트에 표시 되는 Fat 이진에 대해 위에서 만든 라이브러리를 추가 해야 합니다 **InfColorPicker** 라이브러리입니다.

라이브러리를 추가 하려면 다음이 단계를 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 마우스 오른쪽 단추로 클릭 합니다 **네이티브 참조** 선택한 Solution Pad에서 폴더 **네이티브 참조 추가**:

    ![](walkthrough-images/bind04a.png "네이티브 참조를 추가 합니다.")

1. Fat 이진 이전에 만든로 이동 (`libInfColorPickerSDK.a`) 키를 누릅니다 합니다 **오픈** 단추:

    ![](walkthrough-images/bind05.png "LibInfColorPickerSDK.a 파일 선택")
1. 파일을 프로젝트에 포함 됩니다.

    ![](walkthrough-images/bind04.png "파일 포함")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 복사 합니다 `libInfColorPickerSDK.a` 에서 프로그램 **Mac 빌드 호스트** 바인딩 프로젝트에 붙여 넣습니다.

1. 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 기존 항목...** :

    ![](walkthrough-images/bind04vs.png "기존 파일 추가")

1. 로 이동 합니다 `libInfColorPickerSDK.a` 키를 누릅니다 합니다 **추가** 단추:

    ![](walkthrough-images/bind05vs.png "Adding libInfColorPickerSDK.a")

1. 파일을 프로젝트에 포함 됩니다.

-----

경우는 **.a** Xamarin.iOS에서 자동으로 설정 파일을 프로젝트에 추가 됩니다 합니다 **빌드 작업** 파일의 **ObjcBindingNativeLibrary**, 특수 파일을 만듭니다 호출 `libInfColorPickerSDK.linkwith.cs`합니다.


이 파일에 포함 된 `LinkWith` 핸들 방금는 정적 라이브러리를 추가 하는 방법을 Xamarin.iOS 알리는 특성이 필요 합니다. 이 파일의 내용은 다음 코드 조각에 나와 있습니다.

```csharp
using ObjCRuntime;

[assembly: LinkWith ("libInfColorPickerSDK.a", SmartLink = true, ForceLoad = true)]
```

`LinkWith` 특성 프로젝트 및 몇 가지 중요 한 링커 플래그에 대 한 정적 라이브러리를 식별 합니다.


다음으로 할 InfColorPicker 프로젝트에 대 한 API 정의 만드는 것입니다. 이 연습을 위해 사용 하 여 목표 Sharpie 파일을 생성 **ApiDefinition.cs**합니다.

<a name="Using_Objective_Sharpie"/>

## <a name="using-objective-sharpie"></a>목표 Sharpie를 사용 하 여

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)


목표 Sharpie는 명령줄 도구 (Xamarin에서 제공)는 타사 Objective-c 라이브러리를 바인딩하는 데 필요한 정의 만드는 데 도움이 되는 C#입니다. 이 섹션에서는를 사용 하 여 목표 Sharpie 만들 초기 **ApiDefinition.cs** InfColorPicker 프로젝트에 대 한 합니다.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


목표 Sharpie는 명령줄 도구 (Xamarin에서 제공)는 타사 Objective-c 라이브러리를 바인딩하는 데 필요한 정의 만드는 데 도움이 되는 C#입니다. 이 섹션에서는 목표 Sharpie에서 우리의 **Mac 빌드 호스트** 초기에 만들려는 **ApiDefinition.cs** InfColorPicker 프로젝트에 대 한 합니다.


-----

시작 하려면이 항목에 설명 된 대로 목표 Sharpie 설치 관리자 파일을 다운로드 해 보겠습니다 [가이드](~/cross-platform/macios/binding/objective-sharpie/get-started.md#installing)합니다. 설치 관리자를 실행 하 고 목표 Sharpie이 개발 컴퓨터에 설치 하려면 설치 마법사에서 모든 화면의 지시를 따릅니다.

목표 Sharpie 성공적으로 가져온 다음 설치 하겠습니다 터미널 앱을 시작 하 고 모든 바인딩을 지원 하기 위해 제공 하는 도구에서 도움말을 보려면 다음 명령을 입력:

```bash
sharpie -help
```

위의 명령은 실행 하는 경우 다음 출력이 생성 됩니다.

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

이 연습에서는 다음 목표 Sharpie 도구를 사용 합니다.

- **xcode** -이 도구는 현재 Xcode 설치 및 iOS 및 Mac Api에서는 설치의 버전에 대 한 정보 제공 합니다. 이 바인딩을 생성 하는 경우 나중에이 정보를 사용 합니다.
- **바인딩할** -이 도구 사용 하 여 구문 분석 합니다 **.h** 초기에 InfColorPicker 프로젝트의 파일 **ApiDefinition.cs** 및 **StructsAndEnums.cs** 파일입니다.

특정 목표 Sharpie 도구 도움말을 보려면 도구의 이름을 입력 하며 `-help` 옵션입니다. 예를 들어 `sharpie xcode -help` 다음 출력을 반환 합니다.

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

터미널에 다음 명령을 입력 하 여 현재 설치 된 Sdk에 대 한 정보를 얻을 수 해야 바인딩 프로세스를 시작할 수 있습니다, 전에 `sharpie xcode -sdks`:

```bash
amyb:Desktop amyb$ sharpie xcode -sdks
sdk: appletvos9.2    arch: arm64
sdk: iphoneos9.3     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: watchos2.2      arch: armv7
```

위의 보면 있다는 것을 `iphoneos9.3` SDK는 컴퓨터에 설치 합니다. 이 정보를 사용 하 여 준비가 InfColorPicker 프로젝트를 구문 분석할 `.h` 초기에 파일 **ApiDefinition.cs** 및 `StructsAndEnums.cs` InfColorPicker 프로젝트에 대 한 합니다.

터미널 앱에서 다음 명령을 입력 합니다.

```bash
sharpie bind --output=InfColorPicker --namespace=InfColorPicker --sdk=[iphone-os] [full-path-to-project]/InfColorPicker/InfColorPicker/*.h
```

여기서 `[full-path-to-project]` 디렉터리의 전체 경로 위치를 **InfColorPicker** 이 컴퓨터에 있는 Xcode 프로젝트 파일 및 [iphone os]에서 설명한 것 처럼 iOS SDK 설치에서는는 `sharpie xcode -sdks` 명령입니다. 이 예제에서는 한 전달  **\*.h** 포함 된 매개 변수로 *모든* 이 디렉터리에 있는 헤더 파일 일반적으로 이렇게 하지 해도 대신 신중 하 게 읽고 합니다 헤더 파일을 최상위 **.h** 목표 Sharpie는 전달 하기만 다른 모든 관련 파일을 참조 하는 파일입니다.

다음 [출력](walkthrough-images/os05.png) 터미널에서 생성 됩니다.

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

및 **InfColorPicker.enums.cs** 하 고 **InfColorPicker.cs** 파일이 디렉터리에 만들어집니다.

[![](walkthrough-images/os06.png "InfColorPicker.enums.cs 파일과 InfColorPicker.cs")](walkthrough-images/os06.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)


위에서 만든 바인딩 프로젝트에서 두이 파일을 엽니다. 내용을 복사 합니다 **InfColorPicker.cs** 파일에 붙여 넣습니다를 **ApiDefinition.cs** 파일을 기존 `namespace ...` 코드 블록의 내용으로는  **InfColorPicker.cs** 파일 (종료 된 `using` 문을 그대로):

![](walkthrough-images/os07.png "InfColorPickerControllerDelegate 파일")


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


위에서 만든 바인딩 프로젝트에서 두이 파일을 엽니다. 내용을 복사 합니다 **InfColorPicker.cs** 파일 (에서 **Mac 빌드 호스트**)에 붙여 넣습니다 합니다 **ApiDefinition.cs** 파일을 기존 `namespace ...` 내용으로 코드 블록을 **InfColorPicker.cs** 파일 (종료를 `using` 그대로 문).


-----

<a name="Normalize_the_API_Definitions"/>

## <a name="normalize-the-api-definitions"></a>API 정의 정규화 합니다.

목표 Sharpie는 문제를 변환 하는 경우가 있습니다 `Delegates`이므로 정의 수정 해야 합니다 `InfColorPickerControllerDelegate` 바꾸고 인터페이스는 `[Protocol, Model]` 다음 줄:

```csharp
[BaseType(typeof(NSObject))]
[Model]
```
정의 다음과 같습니다.

[![](walkthrough-images/os11.png "정의")](walkthrough-images/os11.png#lightbox)

것의 내용으로 동일한 작업을 수행 하는 다음으로 `InfColorPicker.enums.cs` 파일을 복사 및 붙여넣기에 `StructsAndEnums.cs` 유지 하는 파일은 `using` 그대로 문:

[![](walkthrough-images/os09.png "콘텐츠는 StructsAndEnums.cs 파일 ")](walkthrough-images/os09.png#lightbox)

목표 Sharpie에 바인딩을 사용 하 여 주석이 추가 된 찾을 수도 있습니다 `[Verify]` 특성입니다. 이러한 특성은 확인 하는 바인딩 (바인딩된 선언 위에 주석에서 제공 됩니다)는 원래 C/Objective-c 선언과 비교 하 여 바람직한 목표 Sharpie에가 하 나타냅니다. 바인딩을 확인 한 후 확인 특성을 제거 해야 합니다. 자세한 내용은 참조는 [확인](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) 가이드입니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)


이 시점에서 바인딩 프로젝트 완전 하 고 만들 준비 해야 합니다. 바인딩 프로젝트를 빌드하고에서는 최종적으로 오류가 있는지 확인 하겠습니다.

[바인딩 프로젝트를 빌드하고 오류가 없는지 확인](walkthrough-images/os12.png)


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)


이 시점에서 바인딩 프로젝트 완전 하 고 만들 준비 해야 합니다. 바인딩 프로젝트를 빌드하고에서는 최종적으로 오류가 있는지 확인 하겠습니다.


-----

<a name="Using_the_Binding"/>

## <a name="using-the-binding"></a>바인딩 사용

IOS 바인딩 라이브러리 위에서 만든를 사용 하는 샘플 iPhone 응용 프로그램을 만들려면 다음이 단계를 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Xamarin.iOS 프로젝트를 만듭니다** -라는 새 Xamarin.iOS 프로젝트를 추가 **InfColorPickerSample** 를 솔루션에는 다음 스크린샷과 같이:

    ![](walkthrough-images/use01.png "단일 뷰 앱 추가")

    ![](walkthrough-images/use01a.png "식별자를 설정합니다.")

1. **바인딩 프로젝트에 대 한 참조를 추가** -업데이트 된 **InfColorPickerSample** 에 대 한 참조를 프로젝트의 **InfColorPickerBinding** 프로젝트:

    ![](walkthrough-images/use02.png "바인딩 프로젝트에 대 한 참조를 추가합니다.")

1. **IPhone 사용자 인터페이스를 만듭니다** -두 번 클릭 합니다 **MainStoryboard.storyboard** 파일를 **InfColorPickerSample** iOS 디자이너에서에서 편집 하려는 프로젝트. 추가 된 **단추** 보기로 호출 `ChangeColorButton`다음에 표시 된 것 처럼:

    ![](walkthrough-images/use03.png "보기에 단추 추가")

1. **InfColorPickerView.xib 추가** -The InfColorPicker Objective-c 라이브러리 포함을 **.xib** 파일입니다. Xamarin.iOS이 포함 되지 것입니다 **.xib** 바인딩 프로젝트에서 샘플 응용 프로그램에서 런타임 오류가 발생 합니다. 추가 하는 것이 문제를 해결 합니다 **.xib** Xamarin.iOS 프로젝트 파일입니다. Xamarin.iOS 프로젝트를 마우스 오른쪽 단추로 선택 **추가 > 파일 추가**를 추가 합니다 **.xib** 다음 스크린샷에 표시 된 것과 같이 파일:

    ![](walkthrough-images/use04.png "InfColorPickerView.xib 추가")

1. 을 묻는 메시지가 나타나면 복사 합니다 **.xib** 프로젝트 파일입니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **Xamarin.iOS 프로젝트를 만듭니다** -라는 새 Xamarin.iOS 프로젝트에 추가 **InfColorPickerSample** 사용 하는 **단일 뷰 앱** 템플릿:

    [![iOS 앱 (Xamarin) 프로젝트](walkthrough-images/use01.w157-sml.png)](walkthrough-images/use01.w157.png#lightbox)

    [![템플릿 선택](walkthrough-images/use01-2.w157-sml.png)](walkthrough-images/use01-2.w157.png#lightbox)

1. **바인딩 프로젝트에 대 한 참조를 추가** -업데이트 된 **InfColorPickerSample** 에 대 한 참조를 프로젝트의 **InfColorPickerBinding** 프로젝트:

    ![](walkthrough-images/use02vs.png "바인딩 프로젝트에 대 한 참조를 추가 합니다.")

1. **IPhone 사용자 인터페이스를 만듭니다** -두 번 클릭 합니다 **MainStoryboard.storyboard** 파일를 **InfColorPickerSample** iOS 디자이너에서에서 편집 하려는 프로젝트. 추가 된 **단추** 보기로 호출 `ChangeColorButton`다음에 표시 된 것 처럼:

    ![](walkthrough-images/use03vs.png "IPhone 사용자 인터페이스 만들기")

1. **InfColorPickerView.xib 추가** -The InfColorPicker Objective-c 라이브러리 포함을 **.xib** 파일입니다. Xamarin.iOS이 포함 되지 것입니다 **.xib** 바인딩 프로젝트에서 샘플 응용 프로그램에서 런타임 오류가 발생 합니다. 추가 하는 것이 문제를 해결 합니다 **.xib** 에서 Xamarin.iOS 프로젝트 파일 우리의 **Mac 빌드 호스트**합니다. Xamarin.iOS 프로젝트를 마우스 오른쪽 단추로 선택 **추가** > **기존 항목...** 를 추가 합니다 **.xib** 파일입니다.

-----

다음으로, 살펴보겠습니다를 빠른 Objective-c 및 처리 하 고 바인딩에 대 한 프로토콜 및 C# 코드입니다.

### <a name="protocols-and-xamarinios"></a>Xamarin.iOS 및 프로토콜

Objective-c 프로토콜 정의 메서드 (또는 메시지) 특정 상황에서 사용할 수 있습니다. 인터페이스와 매우 비슷합니다는 개념적으로 C#입니다. 주요 차이점 중 하나는 Objective-c 프로토콜 및 C# 프로토콜 선택적 메서드-구현 되지 않은 클래스 메서드는 인터페이스입니다. Objective-c로 사용 하는 @optional 키워드는 메서드는 선택 사항 나타내는 데 사용 됩니다. 프로토콜에 대 한 자세한 내용은 참조 하세요 [이벤트, 프로토콜 및 대리자](~/ios/app-fundamentals/delegates-protocols-and-events.md)합니다.

**InfColorPickerController** 에 아래 코드 조각 에서처럼 한 하나의 프로토콜이:

```csharp
@protocol InfColorPickerControllerDelegate

@optional

- (void) colorPickerControllerDidFinish: (InfColorPickerController*) controller;
// This is only called when the color picker is presented modally.

- (void) colorPickerControllerDidChangeColor: (InfColorPickerController*) controller;

@end
```

이 프로토콜을 사용 하 여 **InfColorPickerController** 새 색을 지정 하 고는 사용자가 선택 클라이언트에 알리는 데 합니다 **InfColorPickerController** 가 완료 합니다. 다음 코드 조각에 표시 된 대로 목표 Sharpie이이 프로토콜을 매핑됩니다.

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

바인딩 라이브러리를 컴파일할 때 Xamarin.iOS 라는 추상 기본 클래스를 만듭니다 `InfColorPickerControllerDelegate`, 가상 메서드를 사용 하 여이 인터페이스를 구현 하는 합니다.

두 가지 방법으로 Xamarin.iOS 응용 프로그램에서이 인터페이스를 구현할 수 있습니다.

- **강력한 위임** -강력한 대리자를 사용 하 여 만들어야를 C# 는 클래스 `InfColorPickerControllerDelegate` 및 적절 한 메서드를 재정의 합니다. **InfColorPickerController** 해당 클라이언트와 통신 하려면이 클래스의 인스턴스를 사용 합니다.
- **약한 대리자** -약한 대리자는 일부 클래스의 public 메서드를 만들어야 하는 약간 다른 기술 (같은 `InfColorPickerSampleViewController`) 해당 메서드를 노출 합니다 `InfColorPickerDelegate` 프로토콜을 통해는 `Export` 특성입니다.

강력한 대리자에는 Intellisense, 형식 안전성 및 캡슐화를 효율적으로 제공합니다. 이러한 이유로, 강력한 대리자 수 있는, 약한 대리자 대신 사용 해야 있습니다.

이 연습에서는 두 가지 방법을 모두 설명 하면: 먼저 강력한 대리자를 구현 하 고 다음 약한 대리자를 구현 하는 방법을 설명 합니다.

### <a name="implementing-a-strong-delegate"></a>강력한 대리자를 구현합니다.

Xamarin.iOS 응용 프로그램에 응답 하는 강력한 대리자를 사용 하 여 완료 된 `colorPickerControllerDidFinish:` 메시지:

**서브 클래스 InfColorPickerControllerDelegate** -라는 프로젝트에 새 클래스를 추가 `ColorSelectedDelegate`합니다. 다음 코드를 갖도록 클래스를 편집 합니다.

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

Xamarin.iOS는 Objective-c 대리자 라고 하는 추상 기본 클래스를 만들어 바인딩합니다 `InfColorPickerControllerDelegate`합니다. 하위 클래스 입니다가 형식과 재정의 `ColorPickerControllerDidFinish` 의 값에 액세스 하는 방법 합니다 `ResultColor` 의 속성 `InfColorPickerController`합니다.

**ColorSelectedDelegate의 인스턴스를 만듭니다** -이벤트 처리기의 인스턴스를 해야 합니다 `ColorSelectedDelegate` 이전 단계에서 만든 형식입니다. 클래스 편집 `InfColorPickerSampleViewController` 클래스에 다음 인스턴스 변수를 추가 합니다.

```csharp
ColorSelectedDelegate selector;
```

**ColorSelectedDelegate 변수 초기화** -되도록 `selector` 유효한 인스턴스 메서드를 업데이트 합니다 `ViewDidLoad` 에서 `ViewController` 다음 코드 조각에 맞게:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithStrongDelegate;
    selector = new ColorSelectedDelegate (this);
}
```
**HandleTouchUpInsideWithStrongDelegate 메서드를 구현** -사용자가 경우에 대 한 이벤트 처리기를 다음 구현 **ColorChangeButton**합니다. 편집 `ViewController`, 다음 메서드를 추가 합니다.

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

인스턴스를 먼저 구한 `InfColorPickerController` 정적 메서드 및 속성을 통해이 강력한 대리자의 인식 인스턴스 확인을 통해 `InfColorPickerController.Delegate`합니다. 이 속성 목표 Sharpie에서 우리 회사에 자동으로 생성 되었습니다. 마지막 호출 `PresentModallyOverViewController` 보기를 표시 하려면 `InfColorPickerSampleViewController.xib` 색을 선택할 수 있도록 합니다.

**응용 프로그램을 실행** -이 시점에서는 모든 코드를 사용 하 여 완료 합니다. 배경색을 변경할 수 없게 해야 응용 프로그램을 실행 하는 경우는 `InfColorColorPickerSampleView` 다음 스크린샷과에서 같이:

[![](walkthrough-images/run01.png "응용 프로그램 실행")](walkthrough-images/run01.png#lightbox)

지금까지 이 시점에서 성공적으로 만든를 Xamarin.iOS 응용 프로그램에서 사용 하 여에 대 한 Objective-c 라이브러리 바인딩 했으므로 있습니다. 다음으로 약한 대리자 사용에 대해 알아보겠습니다.

### <a name="implementing-a-weak-delegate"></a>약한 대리자를 구현합니다.

특정 대리자의 Objective-c 프로토콜에 바인딩된 클래스를 하위 클래스 지정 대신 Xamarin.iOS 있습니다 클래스에서 파생 되는 프로토콜 메서드를 구현 `NSObject`,으로 메서드를 데코레이팅하는 `ExportAttribute`, 고 적절 한 선택기를 제공 합니다. 클래스를의 인스턴스를 할당 하는 경우이 방법을 사용 하는 `WeakDelegate` 속성 대신는 `Delegate` 속성입니다. 약한 대리자 다른 상속 계층을 다운 대리자 클래스를 사용할 수 있는 유연성을 제공 합니다. 구현 및 Xamarin.iOS 응용 프로그램에서 약한 대리자를 사용 하는 방법을 살펴보겠습니다.

**TouchUpInside에 대 한 이벤트 처리기를 만듭니다** -에 대 한 새 이벤트 처리기를 만들어 보겠습니다는 `TouchUpInside` 배경색 변경 단추의 이벤트입니다. 이 처리기와 동일한 역할을 채울는 `HandleTouchUpInsideWithStrongDelegate` 처리기에서는 이전 섹션에서 만든 하지만 강력한 대리자 대신 약한 대리자를 사용 합니다. 클래스 편집 `ViewController`, 다음 메서드를 추가 합니다.

```csharp
private void HandleTouchUpInsideWithWeakDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.WeakDelegate = this;
    picker.SourceColor = this.View.BackgroundColor;
    picker.PresentModallyOverViewController (this);
}
```
**업데이트 ViewDidLoad** -변경 해야 `ViewDidLoad` 우리가 방금 만들었던 이벤트 처리기를 사용할 수 있도록 합니다. 편집할 `ViewController` 변경 `ViewDidLoad` 다음 코드 조각은 유사 하 게 합니다.


```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithWeakDelegate;
}

```

**colorPickerControllerDidFinish 핸들: 메시지** -합니다 `ViewController` 는 완료 되 면 iOS에서 메시지를 보내지 `colorPickerControllerDidFinish:` 에 `WeakDelegate`합니다. 생성 해야는 C# 이 메시지를 처리할 수 있는 메서드. 이 작업을 수행 하려면 만듭니다는 C# 메서드 다음 표시 하 고는 `ExportAttribute`합니다. 편집 `ViewController`, 클래스에 다음 메서드를 추가 합니다.

```csharp
[Export("colorPickerControllerDidFinish:")]
public void ColorPickerControllerDidFinish (InfColorPickerController controller)
{
    View.BackgroundColor = controller.ResultColor;
    DismissViewController (false, null);
}

```

애플리케이션을 실행합니다. 이제 이전에 수행한 것 하지만 강력한 대리자 대신 약한 대리자를 사용 하는 것으로 정확 하 게 동작 합니다. 이 시점에서이 연습을 완료 했습니다. 이제 만들고 Xamarin.iOS 바인딩 프로젝트를 사용 하는 방법 이해 하 고 있어야 합니다.

## <a name="summary"></a>요약

이 문서를 만들고 Xamarin.iOS 바인딩 프로젝트를 사용 하는 과정 설명 했습니다. 먼저 기존 Objective-c 라이브러리를 정적 라이브러리로 컴파일하는 방법에 설명 했습니다. 다음에서는 Xamarin.iOS 바인딩 프로젝트를 만드는 방법 및 목표 Sharpie를 사용 하 여 Objective-c 라이브러리에 대 한 API 정의 생성 하는 방법을 설명 합니다. 업데이트 및 공개에 대 한 적합 한 있도록 생성 된 API 정의 조정 하는 방법에 설명 했습니다. Xamarin.iOS 바인딩 프로젝트가 완료 된 후에서는 이동 강력한 대리자 및 약한 대리자 사용에 중점을 둔 Xamarin.iOS 응용 프로그램에서 해당 바인딩을 사용 합니다.

## <a name="related-links"></a>관련 링크

- [바인딩 예제 (샘플)](https://developer.xamarin.com/samples/monotouch/InfColorPicker/)
- [Objective-C 라이브러리 바인딩](~/cross-platform/macios/binding/objective-c-libraries.md)
- [바인딩 세부 정보](~/cross-platform/macios/binding/overview.md)
- [바인딩 유형 참조 가이드](~/cross-platform/macios/binding/binding-types-reference.md)
- [Objective-C 개발자용 Xamarin](~/ios/get-started/objective-c-developers/index.md)
- [프레임워크 디자인 지침](https://msdn.microsoft.com/library/ms229042.aspx)
- [Xamarin University 과정: Objective-c 바인딩 라이브러리를 빌드](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University 과정: 목표 Sharpie로는 Objective-c 바인딩 라이브러리 빌드](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
