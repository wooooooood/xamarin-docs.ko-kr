---
title: "연습: 바인딩 iOS Objective C 라이브러리"
description: "이 문서는 기존의 Objective C 라이브러리 InfColorPicker에 대 한 Xamarin.iOS 바인딩을 만드는 실전 연습을 제공 합니다. Objective C 정적 라이브러리를 컴파일하고, 바인딩, Xamarin.iOS 응용 프로그램에 바인딩 사용 같은 항목을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: D3F6FFA0-3C4B-4969-9B83-B6020B522F57
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: e4619f5b1d3f888b2557cf894aaa83106504766f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="walkthrough-binding-an-ios-objective-c-library"></a>연습: 바인딩 iOS Objective C 라이브러리

_이 문서는 기존의 Objective C 라이브러리 InfColorPicker에 대 한 Xamarin.iOS 바인딩을 만드는 실전 연습을 제공 합니다. Objective C 정적 라이브러리를 컴파일하고, 바인딩, Xamarin.iOS 응용 프로그램에 바인딩 사용 같은 항목을 설명 합니다._

IOS를 사용 하는 경우에 제 3 자 Objective C 라이브러리를 사용 하려는 경우 발생할 수 있습니다. 이러한 경우는 Xamarin.iOS를 사용할 수 있습니다 _바인딩 프로젝트_ 만들려는 [C# 바인딩](~/cross-platform/macios/binding/overview.md) 수 Xamarin.iOS 응용 프로그램에서 라이브러리를 사용할 수 있습니다.

일반적으로 iOS 생태계에서 3 버전의 라이브러리를 찾을 수 있습니다.

* 미리 컴파일된 정적 라이브러리 파일으로 `.a` 해당 헤더 (.h 파일)와 함께 확장 합니다. 예를 들어 [Google Analytics 라이브러리](https://developers.google.com/analytics/devguides/collection/ios/v3/sdk-download?hl=es#download_sdk)
* 미리 컴파일된 프레임 워크입니다. 이 정적 라이브러리, 헤더 및 사용 하 여 경우에 따라 추가 리소스를 포함 하는 폴더만 `.framework` 확장 합니다. 예를 들어 [Google의 AdMob 라이브러리](https://developers.google.com/admob/ios/download)합니다.
* 앞서 소스 코드 파일입니다. 예를 들어만 포함 하는 라이브러리 `.m` 및 `.h` Objective C 파일입니다.

첫 번째 및 두 번째 시나리오에서는 이미 있을 것 미리 컴파일된 CocoaTouch 정적 라이브러리 있으므로이 문서에 대해 살펴볼 것 세 번째 시나리오. 바인딩 만들기에 연결 하려면 무료 인지 확인 하는 라이브러리와 함께 제공 되는 라이선스를 항상 확인을 시작 하기 전에 기억 합니다.

이 문서는 오픈 소스를 사용 하 여 바인딩 프로젝트를 만드는 단계별 연습을 제공 [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) 함께이 가이드의 모든 정보를 사용할 수 있지만 Objective-c 예를 들어, 프로젝트 제 3 자 Objective C 라이브러리입니다. InfColorPicker 라이브러리 색 선택 더 친숙 하을 HSB 표현에 따라 색을 선택할 수 있는 재사용 가능한 뷰 컨트롤러를 제공 합니다.

[![](walkthrough-images/run01.png "IOS에서 실행 중인 InfColorPicker 라이브러리의 예")](walkthrough-images/run01.png#lightbox)

Xamarin.iOS에서이 특정 Objective-c API를 사용 하는 데 필요한 모든 단계를 설명 합니다.

- 첫째, Xcode를 사용 하 여 Objective-c 정적 라이브러리를 만들어 보겠습니다.
- 그런 다음 Xamarin.iOS와이 정적 라이브러리에 바인딩합니다 했습니다.
- 다음으로, 목표 Sharpie 일부 (모두는 아니지만)을 자동으로 생성 하 여 작업 부하를 줄일 수 있습니다 어떻게 표시 Xamarin.iOS 바인딩에서 필요한 필요한 API 정의입니다.
- 마지막으로 바인딩을 사용 하는 Xamarin.iOS 응용 프로그램을 만들어 보겠습니다.

샘플 응용 프로그램에서는 InfColorPicker API와 C# 코드 간의 통신에 대 한 강력한 대리자를 사용 하는 방법을 보여 줍니다. 강력한 대리자를 사용 하는 방법을 살펴본 후 약한 대리자를 사용 하 여 동일한 작업을 수행 하는 방법을 설명 합니다.

## <a name="requirements"></a>요구 사항

이 문서에서는 Xcode 및 Objective C 언어에 조금은 알고 있으며 읽기으로 가정 우리의 [바인딩 Objective-c](~/cross-platform/macios/binding/index.md) 설명서입니다. 또한 다음은 제공 되는 단계를 완료 하려면:

-  **Xcode 및 iOS SDK** -Apple의 Xcode 및 최신 iOS API 설치 하 고 개발자의 컴퓨터에 구성 해야 합니다.
-  **[Xcode 명령줄 도구](#Installing_the_Xcode_Command_Line_Tools)**  -Xcode (설치 세부 정보에 대 한 아래 참조)의 현재 설치 된 버전의 Xcode 명령줄 도구를 설치 해야 합니다.
-  **Mac 또는 Visual Studio 용 visual Studio** -최신 버전의 Visual Studio를 Mac 또는 Visual Studio를 설치 하 고 개발 컴퓨터에 구성 해야 합니다. Apple Mac Xamarin.iOS 응용 프로그램을 개발 하기 위한 필수 항목이 며 연결에 해야 Visual Studio를 사용 하는 경우 [는 Xamarin.iOS 빌드 호스트](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
-  **최신 버전의 목표 Sharpie** -목표 Sharpie 도구의 현재 복사본에서 다운로드 한 [여기](~/cross-platform/macios/binding/objective-sharpie/get-started.md)합니다. 설치 하는 목표 Sharpie 이미 있는 경우 업데이트할 수 있습니다 최신 버전을 사용 하 여는 `sharpie update`

<a name="Installing_the_Xcode_Command_Line_Tools"/>

## <a name="installing-the-xcode-command-line-tools"></a>Xcode 명령 명령줄 도구를 설치합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


Xcode 명령줄 도구를 사용해 보겠습니다 위에서 설명 했 듯이 (특히 `make` 및 `lipo`)이이 연습에서 합니다. `make` 명령은 매우 일반적인 Unix 유틸리티를 사용 하 여 실행 프로그램 및 라이브러리의 컴파일 자동화할는 _메이크파일_ 프로그램을 작성 해야 하는 방법을 지정 하는 합니다. `lipo` 명령은 OS X 명령줄 유틸리티를 사용 하 여 다중 아키텍처 파일 만들기에 이지만 여러를 결합 합니다. `.a` 모든 하드웨어 아키텍처에서 사용할 수 있는 하나의 파일에는 파일입니다.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


위에서 설명 했 듯이 사용할 예정 Xcode 명령줄 도구에는 **Mac 빌드 호스트** (특히 `make` 및 `lipo`)이이 연습에서 합니다. `make` 명령은 매우 일반적인 Unix 유틸리티를 사용 하 여 실행 프로그램 및 라이브러리의 컴파일 자동화할는 _메이크파일_ 를 프로그램을 빌드하는 방법을 지정 합니다. `lipo` 명령은 OS X 명령줄 유틸리티를 사용 하 여 다중 아키텍처 파일 만들기에 이지만 여러를 결합 합니다. `.a` 모든 하드웨어 아키텍처에서 사용할 수 있는 하나의 파일에는 파일입니다.


-----

Apple에 따라 [Xcode FAQ를 사용 하 여 명령줄에서 빌드](https://developer.apple.com/library/ios/technotes/tn2339/_index.html) 설명서, OS X 10.9 이상, **다운로드** Xcode의 창 **기본 설정** 대화 하지 더 이상 다운로드 명령줄 도구를 지원합니다.

다음 방법 중 하나를 사용 하 여 도구를 설치 해야 합니다.

- **Xcode를 설치** -Xcode를 설치 하는 경우 모든 명령줄 도구 함께 번들 형태로 제공 합니다. OS X 10.9에서 shim (에 설치 된 `/usr/bin`)에 포함 된 모든 도구를 매핑할 수 `/usr/bin` Xcode 내 해당 도구입니다. 예를 들어는 `xcrun` 명령을 찾을 수 없거나 명령줄에서 Xcode 내 모든 도구를 실행할 수 있습니다.
- **터미널 응용 프로그램** -터미널 응용 프로그램에서 명령줄 도구를 실행 하 여 설치할 수 있습니다는 `xcode-select --install` 명령:
    - 터미널 응용 프로그램을 시작 합니다.
    - 형식 `xcode-select --install` 누릅니다 **Enter**, 예:

    ```bash
    Europa:~ kmullins$ xcode-select --install
    ```

    - 명령줄 도구 설치를 클릭 합니다. 라는 메시지가 표시 됩니다는 **설치** 단추: [ ![ ] (walkthrough-images/xcode01.png "명령줄 도구를 설치 합니다.")](walkthrough-images/xcode01.png#lightbox)

    - 도구 다운로드 되 고 Apple 서버에서 설치: [ ![ ] (walkthrough-images/xcode02.png "도구 다운로드")](walkthrough-images/xcode02.png#lightbox)

- **Apple 개발자를 위한 다운로드** -명령줄 도구 패키지를 사용할 수는 [Apple 개발자를 위한 다운로드]() 웹 페이지입니다. Apple ID를 사용 하 여 로그인 한 다음 검색 하 고, 명령줄 도구를 다운로드: [ ![ ] (walkthrough-images/xcode03.png "명령줄 도구 찾기")](walkthrough-images/xcode03.png#lightbox)

명령줄 도구가 설치 된 준비가 연습에서 계속 합니다.

## <a name="walkthrough"></a>연습

이 연습에서는 다음 단계를 설명 합니다.

- **[정적 라이브러리 만들기](#Creating_A_Static_Library)**  -이 단계에서는 정적 라이브러리를 만드는 **InfColorPicker** Objective C 코드입니다. 정적 라이브러리 갖습니다는 `.a` 파일 확장명이 및 라이브러리 프로젝트의.NET 어셈블리에 포함 됩니다.
- **[Xamarin.iOS 바인딩 프로젝트 만들기](#Create_a_Xamarin.iOS_Binding_Project)**  -정적 라이브러리, 있으면 Xamarin.iOS 바인딩 프로젝트를 만들려면 사용 됩니다. 방금 만든 정적 라이브러리 및 Objective-c API 사용할 수 있는 방법을 설명 하는 C# 코드의 형태로 메타 데이터의 바인딩 프로젝트 구성 됩니다. 이 메타 데이터를 일반적으로 API 정의 라고 합니다. 에서는  **[목표 Sharpie](#Using_Objective_Sharpie)**  와 API 정의 만드는 데 도움이 되도록 합니다.
- **[API 정의 표준화](#Normalize_the_API_Definitions)**  -목표 Sharpie 않습니다 하는 데 도와, 계획을 유지 하지만 모든 작업을 수행할 수 없습니다. 사용 하려면 먼저 API 정의를 확인 해야 하는 일부 변경 내용에 설명 합니다.
- **[바인딩 라이브러리를 사용 하 여](#Using_the_Binding)**  -마지막으로,이 새로 생성된 된 바인딩의 프로젝트를 사용 하는 방법을 보여 주는 Xamarin.iOS 응용 프로그램을 만들겠습니다.

관련 된 단계를 이해 했으므로 살펴보겠습니다 연습의 나머지 부분에 있습니다.

<a name="Creating_A_Static_Library"/>

## <a name="creating-a-static-library"></a>정적 라이브러리 만들기

경우 म InfColorPicker Github에서 코드 있는지 검사 합니다.

[![](walkthrough-images/image02.png "Github에서 InfColorPicker에 대 한 코드 검사")](walkthrough-images/image02.png#lightbox)

다음 세 가지 디렉터리 프로젝트에서 볼 수 있습니다.

- **InfColorPicker** -이 디렉터리는 프로젝트에 대 한 Objective C 코드를 포함 합니다.
- **PickerSamplePad** -이 디렉터리는 샘플 iPad 프로젝트를 포함 합니다.
- **PickerSamplePhone** -이 디렉터리는 샘플 iPhone 프로젝트를 포함 합니다.

InfColorPicker 프로젝트를 다운로드 해 보겠습니다 [GitHub](https://github.com/InfinitApps/InfColorPicker/archive/master.zip) 고 우리의 선택의 디렉터리에 압축을 풉니다. 에 대 한 Xcode 대상을 열면 `PickerSamplePhone` 프로젝트를 Xcode 탐색 창에서 다음 프로젝트 구조를 보면:

[![](walkthrough-images/image03.png "Xcode 탐색기의 프로젝트 구조")](walkthrough-images/image03.png#lightbox)

이 프로젝트 직접 InfColorPicker 소스 코드는 빨간색 상자에서 각 샘플 프로젝트에 추가 하 여 코드 재사용을 달성 합니다. 샘플 프로젝트에 대 한 코드는 파란색 상자 내부. 해야 하는이 특정 프로젝트에서 제공 하지 않으므로 우리는 정적 라이브러리와, 정적 라이브러리를 컴파일하는 데 Xcode 프로젝트를 만들 수 있습니다.

정적 라이브러리에 InfoColorPicker 소스 코드를 추가 하기 위해 첫 번째 단계가입니다. 이 작업을 수행할 보겠습니다에서 다음을 수행 합니다.

1. Xcode를 시작합니다.
2. **파일** 메뉴 선택 **새로** > **프로젝트...** :

    [![](walkthrough-images/image04.png "새 프로젝트를 시작합니다.")](walkthrough-images/image04.png#lightbox)
3. 선택 **프레임 워크 및 라이브러리**, **Cocoa 터치 정적 라이브러리** 템플릿과 클릭은 **다음** 단추:

    [![](walkthrough-images/image05.png "정적 라이브러리 Cocoa 터치 템플릿 선택")](walkthrough-images/image05.png#lightbox)
4. 입력 `InfColorPicker` 에 대 한는 **프로젝트 이름** 클릭는 **다음** 단추:

    [![](walkthrough-images/image06.png "프로젝트 이름에 대 한 InfColorPicker 입력")](walkthrough-images/image06.png#lightbox)
5. 프로젝트를 저장 하 고 클릭 위치를 선택는 **확인** 단추입니다.
6. 이제 당사의 정적 라이브러리 프로젝트에 InfColorPicker 프로젝트에서 소스를 추가 해야 합니다. 때문에 **InfColorPicker.h** 파일이 이미이 정적 라이브러리에 기본적으로, Xcode 덮어쓰시겠습니까를 허용 하지 것입니다. **Finder**, GitHub에서 압축을 푼 우리는 원래 프로젝트에서 InfColorPicker 소스 코드로 이동, 모든 InfColorPicker 파일 복사 및 우리의 새 정적 라이브러리 프로젝트에 붙여 넣습니다.

    [![](walkthrough-images/image12.png "모든 InfColorPicker 파일 복사")](walkthrough-images/image12.png#lightbox)

7. Xcode에 반환, 마우스 오른쪽 단추로 클릭는 **InfColorPicker** 폴더와 선택 **"InfColorPicker..."에 파일 추가**:

    [![](walkthrough-images/image08.png "파일 추가")](walkthrough-images/image08.png#lightbox)

8. 파일 추가 대화 상자에서 방금 복사한 InfColorPicker 소스 코드 파일을 이동, 모든 컨트롤을 선택 하 고 클릭는 **추가** 단추:

    [![](walkthrough-images/image09.png "모두 선택 하 고 추가 단추를 클릭 합니다.")](walkthrough-images/image09.png#lightbox)

9. 소스 코드의 프로젝트에 복사 됩니다.

    [![](walkthrough-images/image10.png "소스 코드 프로젝트에 복사 되지 것입니다.")](walkthrough-images/image10.png#lightbox)

10. Xcode 프로젝트 탐색기에서 선택 된 **InfColorPicker.m** 파일 (이 라이브러리 작성 된이 파일이 사용 되지 않는 방식) 때문에 마지막 두 줄을 주석 및:

    [![](walkthrough-images/image14.png "InfColorPicker.m 파일 편집")](walkthrough-images/image14.png#lightbox)

11. 이제는 라이브러리에 필요한 프레임 워크는 확인 해야 합니다. 추가 정보 파일, 또는 제공 된 샘플 프로젝트 중 하나를 열면이 정보를 찾을 수 있습니다. 이 예에서는 `Foundation.framework`, `UIKit.framework`, 및 `CoreGraphics.framework` 추가 하겠습니다.

12. 선택 된 **InfColorPicker 대상 > 빌드 단계** 확장는 **링크 이진 파일과 라이브러리** 섹션:

    [![](walkthrough-images/image16b.png "링크 이진 파일과 라이브러리 섹션을 확장")](walkthrough-images/image16b.png#lightbox)

13. 사용 하 여는  **+**  단추 위에 나열 된 필수 프레임 프레임 워크를 추가할 수 있도록 대화 상자를 엽니다.

    [![](walkthrough-images/image16c.png "위에 나열 된 필수 프레임 프레임 워크 추가")](walkthrough-images/image16c.png#lightbox)

14. **링크 이진 파일과 라이브러리** 섹션 아래 이미지와 같이 이제 표시:

    [![](walkthrough-images/image16d.png "링크 이진 파일과 라이브러리 섹션")](walkthrough-images/image16d.png#lightbox)

이 시점에서는 시가, 종가 प ण त ु 완료 정확 하지 않습니다. 정적 라이브러리, 만들어졌지만 Fat iOS 장치 및 iOS 시뮬레이터를 모두에 대해 필요한 아키텍처를 모두 포함 하는 이진 만들려는 빌드에 필요 합니다.

### <a name="creating-a-fat-binary"></a>Fat 이진 파일을 만들

모든 iOS 장치 시간이 지남에 따라 개발 ARM 아키텍처에서 제공 하는 프로세서를 포함 합니다. 각 새로운 아키텍처 이전 버전과 호환성 유지 하면서 새로운 명령 및 기타 개선 기능을 추가 했습니다. IOS 장치에 있는 armv6, armv7, armv7s, arm64 명령 집합 – 있지만 [armv6 사용 하 여 더 이상](~/ios/deploy-test/compiling-for-different-devices.md)합니다. IOS 시뮬레이터는 ARM을 통해 수행 되지 않습니다 되며 istead는 x86 및 전원이 x86_64 시뮬레이터 있습니다. 라이브러리 각 명령에 제공 해야 하는 것을 의미 하 설정 합니다.

Fat 라이브러리는 `.a` 지원 되는 아키텍처를 모두 포함 된 파일입니다.

fat를 이진으로 만드는 작업은 다음 3 단계 프로세스:

- ARM 7 & ARM64 버전을의 정적 라이브러리를 컴파일하십시오.
- x86 및 x84_64 버전을의 정적 라이브러리를 컴파일하십시오.
- 사용 하 여 `lipo` 명령줄 도구를 두 개의 정적 라이브러리를 하나로 결합 합니다.

이 세 단계는 보다 간단 하 고 Objective C 라이브러리 업데이트를 받을 때 또는 버그 수정 해야 하는 경우 나중에 반복 하는 일을 할 수 있습니다. 이러한 단계를 자동화 하려는 경우 향후 유지 관리 및 iOS 바인딩 프로젝트의 지원을 간소화 됩니다.

많은 도구 등의 작업-는 셸 스크립트 자동화를 사용할 수 있는 [rake](http://rake.rubyforge.org/), [xbuild](http://www.mono-project.com/Microsoft.Build), 및 [확인](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/make.1.html)합니다. Xcode 명령줄 도구를 설치할 것도 작성,이 연습에 사용 될 빌드 시스템 하도록 설치. 다음은 한 **메이크파일** iOS 장치 및 모든 라이브러리에 대 한 시뮬레이터에서 사용할 수 있는 다중 아키텍처 공유 라이브러리를 만드는 데 사용할 수 있는:

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

입력는 **메이크파일** 명령에서 일반 텍스트 편집기를 선택 하 고 있는 섹션이 업데이트 **귀하가 프로젝트 이름** 프로젝트의 이름으로 합니다. 것도 충족 되도록 중요 지침 내에서 탭 보존 된, 위의 지침에 붙여 넣을 것입니다.

파일 이름으로 저장 **메이크파일** 위에서 만든 InfColorPicker Xcode 정적 라이브러리와 동일한 위치에:

[![](walkthrough-images/lib00.png "메이크파일 이름의 파일을 저장 합니다.")](walkthrough-images/lib00.png#lightbox)

Mac에서 터미널 응용 프로그램을 열고 메이크파일의 위치로 이동 합니다. 형식 `make` 를 터미널을 눌러 **Enter** 및 **메이크파일** 실행 됩니다.

[![](walkthrough-images/lib01.png "샘플 메이크파일 출력")](walkthrough-images/lib01.png#lightbox)

확인을 실행 하면 여 스크롤 텍스트가 표시 됩니다. 올바르게 작동 했던 단어가 나타납니다 **빌드에 성공** 및 `libInfColorPicker-armv7.a`, `libInfColorPicker-i386.a` 및 `libInfColorPickerSDK.a` 와 동일한 위치에 파일을 복사할는 **메이크파일**:

[![](walkthrough-images/lib02.png "메이크파일의에서 생성 한 libInfColorPicker armv7.a, libInfColorPicker i386.a 및 libInfColorPickerSDK.a 파일")](walkthrough-images/lib02.png#lightbox)

Fat 이진 파일 내에서 다음 명령을 사용 하 여 아키텍처를 확인할 수 있습니다.

```bash
xcrun -sdk iphoneos lipo -info libInfColorPicker.a
```

다음 표시 되어야 합니다.

```bash
Architectures in the fat file: libInfColorPicker.a are: i386 armv7 x86_64 arm64
```

이 시점에서는 Xcode 및 Xcode 명령줄 도구를 사용 하 여 정적 라이브러리를 만들어 iOS 바인딩에의 첫 번째 단계를 완료 한 `make` 및 `lipo`합니다. 다음 단계로 이동 하 고 사용 하 여 하겠습니다 **목표 Sharpie** us에 대 한 API 바인딩의 만들기를 자동화 합니다.

<a name="Create_a_Xamarin.iOS_Binding_Project"/>

## <a name="create-a-xamarinios-binding-project"></a>프로젝트를 바인딩 Xamarin.iOS 만들기

사용할 수 있습니다 **목표 Sharpie** API 정의 수용 하도록 Xamarin.iOS 바인딩 프로젝트를 만들 필요는 바인딩 프로세스를 자동화 하려면 (사용할 예정입니다 **목표 Sharpie** 하기 위한 빌드) us에 대 한 C# 바인딩을 만듭니다.

다음을 수행 하겠습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac.에 대 한 Visual Studio를 시작 합니다.
1. **파일** 메뉴 선택 **새로** > **솔루션 중...** :

    ![](walkthrough-images/bind01.png "새 솔루션 시작")

1. 새 솔루션 대화 상자에서 선택 **라이브러리** > **iOS 프로젝트 바인딩**:

    ![](walkthrough-images/bind02.png "IOS 프로젝트 바인딩 선택")

1. 클릭는 **다음** 단추입니다.

1. "InfColorPickerBinding"으로 입력 된 **프로젝트 이름** 클릭는 **만들기** 솔루션을 만드는 단추:

    ![](walkthrough-images/bind02a.png "프로젝트 이름으로 InfColorPickerBinding를 입력 합니다.")

솔루션 생성 되 고 두 개의 기본 파일이 포함 됩니다.

![](walkthrough-images/bind03.png "솔루션 탐색기에서 솔루션 구조")


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. Visual Studio를 시작합니다.

1. **파일** 메뉴 선택 **새로** > **프로젝트...** :

    ![](walkthrough-images/bind01vs.png "새 프로젝트를 시작합니다.")

1. 새 프로젝트 대화 상자에서 선택 **iOS** > **바인딩 라이브러리**:

    ![](walkthrough-images/bind02vs.png "IOS 바인딩 라이브러리를 선택 합니다.")

1. "InfColorPickerBinding"으로 입력은 **이름** 클릭는 **확인** 솔루션을 만드는 단추입니다.

솔루션 생성 되 고 두 개의 기본 파일이 포함 됩니다.

![](walkthrough-images/bind03vs.png "솔루션 탐색기에서 솔루션 구조")

-----



- **ApiDefinition.cs** -이 파일은 Objective-c API의 C#에서 래핑 방법을 정의 하는 계약을 포함 합니다.
- **Structs.cs** -이 파일을 모든 구조를 보유할 또는 열거형 값을 하는 인터페이스 및 대리자에 필요한 합니다.

이 연습의 뒷부분에서는 이러한 두 개의 파일이 작업 합니다. 먼저, 바인딩 프로젝트 InfColorPicker 라이브러리를 추가 해야 합니다.

### <a name="including-the-static-library-in-the-binding-project"></a>바인딩 프로젝트에 정적 라이브러리를 포함합니다.

에 대 한 위에서 만든 Fat 이진 라이브러리를 추가 해야 한다고 기본 바인딩 프로젝트 진행 준비 했으므로, 이제는 **InfColorPicker** 라이브러리입니다.

라이브러리를 추가 하려면 다음이 단계를 수행 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 마우스 오른쪽 단추로 클릭는 **네이티브 참조** 솔루션 패드 고에서 폴더 **네이티브 참조 추가**:

    ![](walkthrough-images/bind04a.png "네이티브 참조를 추가 합니다.")

1. Fat 이진 이전에 만든로 이동 (`libInfColorPickerSDK.a`)를 누르고는 **열려** 단추:

    ![](walkthrough-images/bind05.png "LibInfColorPickerSDK.a 파일 선택")
1. 파일이는 프로젝트에 포함 됩니다.

    ![](walkthrough-images/bind04.png "파일을 포함 하 여")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 복사는 `libInfColorPickerSDK.a` 에서 프로그램 **Mac 빌드 호스트** 바인딩 프로젝트에 붙여 넣습니다.

1. 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 기존 항목...** :

    ![](walkthrough-images/bind04vs.png "기존 파일 추가")

1. 로 이동 된 `libInfColorPickerSDK.a` 누릅니다는 **추가** 단추:

    ![](walkthrough-images/bind05vs.png "Adding libInfColorPickerSDK.a")

1. 파일을 프로젝트에 포함 됩니다.

-----


Xamarin.iOS은 자동으로 설정.a 파일이 프로젝트에 추가 되는 경우는 **빌드 작업** 파일의 **ObjcBindingNativeLibrary**, 라는 특수 파일을 만들고 `libInfColorPickerSDK.linkwith.cs`합니다.


이 파일에 포함 된 `LinkWith` 핸들 실제로 정적 라이브러리 추가 하는 방법과 Xamarin.iOS 알리는 특성이 필요 합니다. 이 파일의 내용은 다음 코드 조각에 나와 있습니다.

```csharp
using ObjCRuntime;

[assembly: LinkWith ("libInfColorPickerSDK.a", SmartLink = true, ForceLoad = true)]
```

`LinkWith` 특성 프로젝트 및 몇 가지 중요 한 링커 플래그에 대 한 정적 라이브러리를 식별 합니다.


해야 할 다음으로 InfColorPicker 프로젝트에 대 한 API 정의 만드는 것입니다. 이 연습에서는 파일을 생성 하도록 목표 Sharpie 사용 합니다 **ApiDefinition.cs**합니다.

<a name="Using_Objective_Sharpie"/>

## <a name="using-objective-sharpie"></a>목표 Sharpie를 사용 하 여

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


목표 Sharpie은 명령줄을 사용 하는 데 도움이 되는 C# 3rd 파티 Objective C 라이브러리를 바인딩하는 데 필요한 정의 만드는 도구 (Xamarin에서 제공). 이 섹션에서는 목표 Sharpie 초기 만들려는 **ApiDefinition.cs** InfColorPicker 프로젝트에 대 한 합니다.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


목표 Sharpie은 명령줄을 사용 하는 데 도움이 되는 C# 3rd 파티 Objective C 라이브러리를 바인딩하는 데 필요한 정의 만드는 도구 (Xamarin에서 제공). 이 섹션에서는 목표 Sharpie에 우리의 **Mac 빌드 호스트** 초기 만들려는 **ApiDefinition.cs** InfColorPicker 프로젝트에 대 한 합니다.


-----

시작 하려면이 항목에 설명 된 대로 목표 Sharpie 설치 관리자 파일을 다운로드 해 보겠습니다 [가이드](~/cross-platform/macios/binding/objective-sharpie/get-started.md#installing)합니다. 설치 프로그램을 실행 하 고 목표 Sharpie 우리의 개발 컴퓨터에 설치 하려면 설치 마법사에서 모든 화면의 지시를 따르십시오.

목표 Sharpie를 성공적으로 가져온 다음 설치 하겠습니다 터미널 앱을 시작 하 고 모든 바인딩을 지원 하기 위해 제공 하는 도구에 도움말을 보려면 다음 명령을 입력:

```bash
sharpie -help
```

위의 명령을 실행 하는 경우 다음과 같은 출력이 생성 됩니다.

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

- **xcode** -이 도구 목록에 현재 Xcode 설치 및 버전의 iOS 및 Mac Api를 설치한 것에 대 한 정보입니다. 이 바인딩을 생성 하는 경우 나중에이 정보를 사용 합니다.
- **바인딩** -구문 분석 하려면이 도구에서는 **.h** 초기에 InfColorPicker 프로젝트의 파일 **ApiDefinition.cs** 및 **StructsAndEnums.cs** 파일입니다.

특정 목표 Sharpie 도구 도움말을 보려면 도구의 이름을 입력 및 `-help` 옵션입니다. 예를 들어 `sharpie xcode -help` 다음과 같은 출력을 반환 합니다.

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

바인딩 프로세스를 시작할 수 있습니다, 전에 터미널에 다음 명령을 입력 하 여이 현재 설치 된 Sdk에 대 한 정보를 가져올 해야 `sharpie xcode -sdks`:

```bash
amyb:Desktop amyb$ sharpie xcode -sdks
sdk: appletvos9.2    arch: arm64
sdk: iphoneos9.3     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: watchos2.2      arch: armv7
```

있는지 확인할 수 있습니다 위의 `iphoneos8.1` SDK 컴퓨터에 설치 합니다. 위치에이 정보를 사용 준비가 InfColorPicker 프로젝트를 구문 분석 `.h` 초기로 파일 **ApiDefinition.cs** 및 `StructsAndEnums.cs` InfColorPicker 프로젝트에 대 한 합니다.

다음 명령을 입력 하 고 터미널 앱:

```bash
sharpie bind --output=InfColorPicker --namespace=InfColorPicker --sdk=[iphone-os] [full-path-to-project]/InfColorPicker/InfColorPicker/*.h
```

여기서 `[full-path-to-project]` 디렉터리의 전체 경로는 **InfColorPicker** Xcode 프로젝트 파일을이 컴퓨터에 있고 [iphone-os] 하 여 설명한 것 처럼 iOS SDK를 설치한 우리는 `sharpie xcode -sdks` 명령입니다. 이 예제에서는 한 전달  **\*.h** 포함 하는 매개 변수로 *모든* -이 디렉터리에 있는 헤더 파일 일반적으로 해야 하지이 작업을 수행 해도 대신 자세히 읽어는 헤더 파일을 최상위 **.h** 방금 목표 Sharpie로 전달 하도록를 다른 모든 관련 파일을 참조 하는 파일입니다.

다음 [출력](walkthrough-images/os05.png) 터미널에 생성 됩니다.

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

및 **InfColorPicker.enums.cs** 및 **InfColorPicker.cs** 파일의 디렉터리에 생성 됩니다.

[![](walkthrough-images/os06.png "InfColorPicker.enums.cs 및 InfColorPicker.cs 파일")](walkthrough-images/os06.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


위에서 만든 바인딩 프로젝트에서 이러한 파일을 모두를 엽니다. 내용을 복사 하는 **InfColorPicker.cs** 파일을 붙여넣습니다는 **ApiDefinition.cs** 교체한 파일 `namespace ...` 코드 블록의 내용으로는  **InfColorPicker.cs** 파일 (종료에서 `using` 그대로 문):

![](walkthrough-images/os07.png "InfColorPickerControllerDelegate 파일")


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


위에서 만든 바인딩 프로젝트에서 이러한 파일을 모두를 엽니다. 내용을 복사 하는 **InfColorPicker.cs** 파일 (에서 **Mac 빌드 호스트**)에 붙여 넣을 **ApiDefinition.cs** 교체한 파일 `namespace ...` 코드 블록의 내용으로는 **InfColorPicker.cs** 파일 (종료는 `using` 그대로 문).


-----

<a name="Normalize_the_API_Definitions"/>

## <a name="normalize-the-api-definitions"></a>API 정의 표준화

목표 Sharpie는 문제를 해석 하는 경우가 `Delegates`이므로의 정의 수정 해야 합니다는 `InfColorPickerControllerDelegate` 인터페이스 및 바꾸기는 `[Protocol, Model]` 다음 준수:

```csharp
[BaseType(typeof(NSObject))]
[Model]
```
정의 다음과 같습니다.

[![](walkthrough-images/os11.png "정의")](walkthrough-images/os11.png#lightbox)

에서는의 내용으로 동일한 작업을 수행 하는 다음으로 `InfColorPicker.enums.cs` 파일을 복사 및 붙여넣기에 `StructsAndEnums.cs` 그대로 두고 파일은 `using` 그대로 문:

[![](walkthrough-images/os09.png "콘텐츠는 StructsAndEnums.cs 파일 ")](walkthrough-images/os09.png#lightbox)

살펴보면 목표 Sharpie가 사용 하 여 바인딩을 주석이 추가 된 `[Verify]` 특성입니다. 이러한 특성 나타냅니다 목표 Sharpie 원래 C/Objective-c 선언 (바인딩된 선언 위에 주석에서 제공 됩니다)를 사용 하 여 바인딩을 비교 하 여에서는 올바른 동작인이 있는지 확인 해야 합니다. 바인딩을 확인 한 후 확인 특성을 제거 합니다. 자세한 내용은 참조는 [확인](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) 가이드입니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


이 시점에서 완전 하 고 빌드할 준비가 바인딩 프로젝트 진행 해야 합니다. 우리의 바인딩 프로젝트를 빌드하고 우리는 결국 오류 없이 있는지 확인 보겠습니다.

[바인딩 프로젝트를 빌드하고 오류가 없는지 확인](walkthrough-images/os12.png)


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


이 시점에서 완전 하 고 빌드할 준비가 바인딩 프로젝트 진행 해야 합니다. 우리의 바인딩 프로젝트를 빌드하고 우리는 결국 오류 없이 있는지 확인 하겠습니다.


-----

<a name="Using_the_Binding"/>

## <a name="using-the-binding"></a>바인딩 사용

위에서 만든 바인딩 라이브러리의 iOS를 사용 하는 샘플 iPhone 응용 프로그램을 만들려면 다음이 단계를 수행 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **Xamarin.iOS 프로젝트 만들기** -라는 새 Xamarin.iOS 프로젝트를 추가 **InfColorPickerSample** 다음 스크린샷에서 같이 솔루션에:

    ![](walkthrough-images/use01.png "단일 보기 응용 프로그램 추가")

    ![](walkthrough-images/use01a.png "식별자를 설정합니다.")

1. **바인딩 프로젝트 참조를 추가할** -업데이트에서 **InfColorPickerSample** 에 대 한 참조를 프로젝트는 **InfColorPickerBinding** 프로젝트:

    ![](walkthrough-images/use02.png "바인딩 프로젝트에 참조 추가")

1. **IPhone 사용자 인터페이스 만들기** -두 번 클릭 하는 **MainStoryboard.storyboard** 파일에 **InfColorPickerSample** iOS 디자이너에서에서 편집 하는 프로젝트입니다. 추가 **단추** 보기로 버전을 호출 `ChangeColorButton`다음에 나온 것 처럼:

    ![](walkthrough-images/use03.png "보기에 단추 추가")
1. **추가 InfColorPickerView.xib** -The InfColorPicker Objective-c 라이브러리에 포함 되어는 **.xib** 파일입니다. Xamarin.iOS이 포함 되지 것입니다 **.xib** 바인딩 프로젝트에 샘플 응용 프로그램에서 런타임 오류가 발생 합니다. 이 대 한 해결 방법은 추가 하는 것은 **.xib** 우리의 Xamarin.iOS 프로젝트에 파일입니다. Xamarin.iOS 프로젝트, 마우스 오른쪽 단추 클릭 및 선택 선택 **추가 > 파일 추가**, 추가 하 고는 **.xib** 다음 스크린샷에 표시 된 대로 파일:

    ![](walkthrough-images/use04.png "InfColorPickerView.xib 추가")

1. 을 묻는 메시지가 나타나면 복사는 **.xib** 프로젝트에는 파일입니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. **Xamarin.iOS 프로젝트 만들기** -라는 새 Xamarin.iOS 프로젝트를 추가 **InfColorPickerSample** 다음 스크린샷에 표시 된 대로 솔루션에:

    ![](walkthrough-images/use01vs.png "Xamarin.iOS 프로젝트 만들기")

1. **바인딩 프로젝트 참조를 추가할** -업데이트에서 **InfColorPickerSample** 에 대 한 참조를 프로젝트는 **InfColorPickerBinding** 프로젝트:

    ![](walkthrough-images/use02vs.png "바인딩 프로젝트 참조 추가")

1. **IPhone 사용자 인터페이스 만들기** -두 번 클릭 하는 **MainStoryboard.storyboard** 파일에 **InfColorPickerSample** iOS 디자이너에서에서 편집 하는 프로젝트입니다. 추가 **단추** 보기로 버전을 호출 `ChangeColorButton`다음에 나온 것 처럼:

    ![](walkthrough-images/use03vs.png "IPhone 사용자 인터페이스 만들기")

1. **추가 InfColorPickerView.xib** -The InfColorPicker Objective-c 라이브러리에 포함 되어는 **.xib** 파일입니다. Xamarin.iOS이 포함 되지 것입니다 **.xib** 바인딩 프로젝트에 샘플 응용 프로그램에서 런타임 오류가 발생 합니다. 이 대 한 해결 방법은 추가 하는 것은 **.xib** 에서 우리의 Xamarin.iOS 프로젝트에 파일 우리의 **Mac 빌드 호스트**합니다. Xamarin.iOS 프로젝트, 마우스 오른쪽 단추 클릭 및 선택 선택 **추가** > **기존 항목...** 를 추가 하 고는 **.xib** 파일입니다.


-----



다음에 살펴보겠습니다 프로토콜 Objective-c 및 처리 방식으로 바인딩 및 C# 코드를 빠르게 확인 합니다.

### <a name="protocols-and-xamarinios"></a>Xamarin.iOS 및 프로토콜

Objective C 프로토콜 정의 메서드 (또는 메시지)는 특정 환경에서는 사용할 수 있는 합니다. 이론적으로 C#에서 인터페이스와 매우 비슷합니다. Objective C 프로토콜 및 C# 인터페이스가 차이점은 프로토콜 선택적 메서드-없는 클래스를 구현 하는 메서드를 사용할 수 있도록 합니다. Objective C를 사용 하는 @optional 키워드는 메서드는 선택 사항 나타내는 데 사용 됩니다. 프로토콜에 대 한 자세한 내용은 참조 [이벤트, 프로토콜 및 대리자](~/ios/app-fundamentals/delegates-protocols-and-events.md)합니다.

**InfColorPickerController** 아래 코드 조각에 표시 한 하나의 프로토콜에:

```csharp
@protocol InfColorPickerControllerDelegate

@optional

- (void) colorPickerControllerDidFinish: (InfColorPickerController*) controller;
// This is only called when the color picker is presented modally.

- (void) colorPickerControllerDidChangeColor: (InfColorPickerController*) controller;

@end
```

이 프로토콜을 사용 하 여 **InfColorPickerController** 새로운 색 하 고 있는 사용자 선택 않은 클라이언트에 게 알리기 위해는 **InfColorPickerController** 를 완료 합니다. 다음 코드 조각에 표시 된 대로 목표 Sharpie이이 프로토콜을 매핑됩니다.

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

Xamarin.iOS 라고 하는 추상 기본 클래스 만들어집니다 바인딩 라이브러리를 컴파일할 때 `InfColorPickerControllerDelegate`, 가상 메서드에도이 인터페이스를 구현 하는 합니다.

두 가지 방법으로 Xamarin.iOS 응용 프로그램에서이 인터페이스를 구현할 수 있습니다.

- **강력한 위임** -강력한 대리자를 사용 하 여 해당 서브 클래스는 C# 클래스를 만들 포함 `InfColorPickerControllerDelegate` 적절 한 메서드를 재정의 합니다. **InfColorPickerController** 해당 클라이언트와 통신 하도록이 클래스의 인스턴스를 사용 합니다.
- **약한 대리자** -약한 대리자는 일부 클래스에는 공용 메서드를 만들어야 하는 약간 다른 기술 (같은 `InfColorPickerSampleViewController`) 해당 메서드를 노출는 `InfColorPickerDelegate` 프로토콜을 통해는 `Export` 특성입니다.

Intellisense, 형식 안전성 및 더 나은 캡슐화 강력한 대리자를 제공합니다. 이러한 이유로, 강력한 대리자 할 수 있는, 약한 대리자 대신 사용 해야 있습니다.

이 연습에서는 두 가지 방법을 모두 설명 합니다: 먼저 강력한 대리자를 구현 하 고 다음 약한 대리자를 구현 하는 방법에 설명 합니다.

### <a name="implementing-a-strong-delegate"></a>강력한 대리자 구현

Xamarin.iOS 응용 프로그램에 응답 하는 강력한 대리자를 사용 하 여 완료 된 `colorPickerControllerDidFinish:` 메시지:

**하위 클래스 InfColorPickerControllerDelegate** -라는 프로젝트에 새 클래스를 추가 `ColorSelectedDelegate`합니다. 다음 코드를 클래스를 편집 합니다.

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

Xamarin.iOS Objective-c 대리자 호출 하는 추상 기본 클래스를 만들어 바인딩합니다 `InfColorPickerControllerDelegate`합니다. 하위 클래스 유형 및 재정의 `ColorPickerControllerDidFinish` 의 값에 액세스 하는 메서드는 `ResultColor` 속성 `InfColorPickerController`합니다.

**ColorSelectedDelegate의 인스턴스를 만들고** -이벤트 처리기의 인스턴스를 해야 합니다는 `ColorSelectedDelegate` 이전 단계에서 만든 형식입니다. 마우스 오른쪽 단추로 클래스 `InfColorPickerSampleViewController` 클래스에 다음 인스턴스 변수를 추가 합니다.

```csharp
ColorSelectedDelegate selector;
```

**ColorSelectedDelegate 변수 초기화** -되도록 `selector` 은 유효한 인스턴스를 방법을 업데이트 `ViewDidLoad` 에 `ViewController` 다음 코드 조각에 맞게:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithStrongDelegate;
    selector = new ColorSelectedDelegate (this);
}
```
**HandleTouchUpInsideWithStrongDelegate 메서드를 구현** -그런 다음 사용자와 연결 하는 경우에 대 한 이벤트 처리기를 구현 **ColorChangeButton**합니다. 편집 `ViewController`, 다음 메서드를 추가 합니다.

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

인스턴스를 먼저 가져올 우리 `InfColorPickerController` 정적 메서드 및 속성을 통해 강력한 우리의 대리자의 인식 인스턴스 만들기를 통해 `InfColorPickerController.Delegate`합니다. 이 속성 목표 Sharpie 여 우리 자동으로 생성 되었습니다. 마지막으로 이라고 `PresentModallyOverViewController` 보기를 표시 하려면 `InfColorPickerSampleViewController.xib` 색을 선택할 수 있도록 합니다.

**응용 프로그램을 실행** -코드의 모든 완료 된이 위치에 있습니다. 배경색을 변경할 수 있어야 응용 프로그램을 실행 하는 경우는 `InfColorColorPickerSampleView` 다음 스크린샷에 표시 된 것 처럼:

[![](walkthrough-images/run01.png "응용 프로그램 실행")](walkthrough-images/run01.png#lightbox)

지금까지 이 시점에서 성공적으로 생성 되었으며 바인딩된 Xamarin.iOS 응용 프로그램에 사용 된 Objective C 라이브러리입니다. 다음으로 약한 대리자를 사용 하 여에 대해 알아보겠습니다.

### <a name="implementing-a-weak-delegate"></a>약한 대리자 구현

특정 대리자의 Objective-c 프로토콜에 바인딩된 클래스 서브클래싱, 대신 Xamarin.iOS 수도 있습니다 클래스에서 파생 되는 프로토콜 메서드를 구현 `NSObject`와 메서드를 데코레이팅하는 `ExportAttribute`, 한 다음 적절 한 선택기를 제공 합니다. 클래스의 인스턴스를 할당이 방법을 사용 하는 경우는 `WeakDelegate` 대신 속성의 `Delegate` 속성입니다. 약한 대리자는 다른 상속 계층 구조 아래로 대리자 클래스를 사용할 수 있는 유연성을 제공 합니다. 구현 및이 Xamarin.iOS 응용 프로그램에서 약한 대리자를 사용 하는 방법을 살펴보겠습니다.

**TouchUpInside에 대 한 이벤트 처리기를 만들고** -에 대 한 새 이벤트 처리기를 만들어 보겠습니다는 `TouchUpInside` 단추의 배경색 변경 이벤트. 이 처리기와 동일한 역할을 채울는 `HandleTouchUpInsideWithStrongDelegate` 처리기에서는 이전 섹션에서 만든 하지만 강력한 대리자 대신 약한 대리자를 사용 합니다. 마우스 오른쪽 단추로 클래스 `ViewController`, 다음 메서드를 추가 합니다.

```csharp
private void HandleTouchUpInsideWithWeakDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.WeakDelegate = this;
    picker.SourceColor = this.View.BackgroundColor;
    picker.PresentModallyOverViewController (this);
}
```
**ViewDidLoad 업데이트** -변경 해야 `ViewDidLoad` 를 방금 만든는 이벤트 처리기를 사용 하도록 합니다. 편집 `ViewController` 변경 `ViewDidLoad` 다음 코드 조각과 유사 하 게 합니다.


```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithWeakDelegate;
}

```

**처리는 colorPickerControllerDidFinish: 메시지** -는 `ViewController` 은 작업이 끝난 후에 iOS에서 메시지를 보내지 `colorPickerControllerDidFinish:` 에 `WeakDelegate`합니다. 이 메시지를 처리할 수 있는 C# 메서드를 만드는 해야 합니다. 이 위해 우리는 C# 메서드 만들고 사용 하 여를 장식할는 `ExportAttribute`합니다. 편집 `ViewController`, 클래스에 다음 메서드를 추가 합니다.

```csharp
[Export("colorPickerControllerDidFinish:")]
public void ColorPickerControllerDidFinish (InfColorPickerController controller)
{
    View.BackgroundColor = controller.ResultColor;
    DismissViewController (false, null);
}

```

응용 프로그램을 실행합니다. 이제 이전과 하지만 강력한 대리자 대신 약한 대리자를 사용 하는 것으로 정확 하 게 동작 합니다. 이 시점에서이 연습을 성공적으로 완료 했습니다. 이제는 만들고 Xamarin.iOS 바인딩 프로젝트를 사용 하는 방법 이해가 있어야 합니다.

## <a name="summary"></a>요약

이 문서를 만들고 Xamarin.iOS 바인딩 프로젝트를 사용 하는 프로세스를 살펴보았습니다. 먼저 기존 Objective C 라이브러리를 정적 라이브러리로 컴파일하는 방법에 설명 했습니다. 다음에서는 Xamarin.iOS 바인딩 프로젝트를 만드는 방법 및 Objective-c 라이브러리에 대 한 API 정의 생성 하 목표 Sharpie를 사용 하는 방법을 설명 합니다. 업데이트 하 고 공용으로 사용 하기 적합 하면 생성 된 API 정의 조정 하는 방법에 설명 했습니다. Xamarin.iOS 바인딩 프로젝트가 완료 된 후 이동 강력한 대리자 및 약한 대리자를 사용 하 여에 중점을 둔 Xamarin.iOS 응용 프로그램에서 해당 바인딩을 사용 하는 데 있습니다.

## <a name="related-links"></a>관련 링크

- [바인딩 예제 (샘플)](https://developer.xamarin.com/samples/monotouch/InfColorPicker/)
- [Objective-C 라이브러리 바인딩](~/cross-platform/macios/binding/objective-c-libraries.md)
- [바인딩 세부 정보](~/cross-platform/macios/binding/overview.md)
- [바인딩 형식에 대 한 가이드](~/cross-platform/macios/binding/binding-types-reference.md)
- [Objective-C 개발자용 Xamarin](~/ios/get-started/objective-c-developers/index.md)
- [프레임워크 디자인 지침](http://msdn.microsoft.com/en-us/library/ms229042.aspx)
