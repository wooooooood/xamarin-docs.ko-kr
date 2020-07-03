---
title: '연습: iOS Swift 라이브러리 바인딩'
description: 이 문서에서는 기존 Swift framework, Gigya에 대 한 Xamarin.ios 바인딩을 만드는 실습 연습을 제공 합니다. Swift 프레임 워크를 컴파일하고, 바인딩하고, Xamarin.ios 응용 프로그램에서 바인딩을 사용 하는 등의 항목을 다룹니다.
ms.prod: xamarin
ms.assetid: B563ADE9-D0E3-4BC3-A288-66D2296BE706
ms.technology: xamarin-ios
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: 3c63b1a4ed58b0efcc510085934a5380e6049ae7
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853156"
---
# <a name="walkthrough-bind-an-ios-swift-library"></a>연습: iOS Swift 라이브러리 바인딩

> [!IMPORTANT]
> 현재 Xamarin 플랫폼에서 사용자 지정 바인딩 사용을 조사 하 고 있습니다. 향후 개발 노력을 알리기 위해 [**이 설문 조사**](https://www.surveymonkey.com/r/KKBHNLT) 를 수행 하세요.

Xamarin을 사용 하면 모바일 개발자가 Visual Studio 및 c #을 사용 하 여 플랫폼 간 네이티브 모바일 환경을 만들 수 있습니다. IOS platform SDK 구성 요소를 즉시 사용할 수 있습니다. 그러나 대부분의 경우에는 해당 플랫폼용으로 개발 된 타사 Sdk를 사용 하 여 바인딩을 통해 수행할 수 있도록 하는 것이 좋습니다. 타사 목표-C 프레임 워크를 Xamarin.ios 응용 프로그램에 통합 하려면 응용 프로그램에서 사용할 수 있으려면 먼저 해당 응용 프로그램에 대 한 Xamarin.ios 바인딩을 만들어야 합니다.

IOS 플랫폼은 해당 네이티브 언어 및 도구와 함께 지속적으로 진화 하 고 있으며 Swift는 현재 iOS 개발 분야에서 가장 많은 동적 영역 중 하나입니다. 여러 타사 Sdk가 이미 목표-C에서 Swift로 마이그레이션 되었으며 새로운 과제가 제공 됩니다. Swift 바인딩 프로세스는 객관적인 C와 유사 하지만 AppStore에 허용 되는 Xamarin.ios 응용 프로그램을 성공적으로 빌드하고 실행 하려면 추가 단계와 구성 설정이 필요 합니다.

이 문서의 목표는이 시나리오를 해결 하기 위한 개략적인 접근 방식을 간략하게 설명 하 고 간단한 예제를 사용 하 여 자세한 단계별 가이드를 제공 하는 것입니다.

## <a name="background"></a>배경

Swift는 처음에는 2014에 Apple에서 도입 되었으며, 현재는 타사 프레임 워크의 채택을 빠르게 성장 하는 버전 5.1에 있습니다. Swift 프레임 워크 바인딩에 대 한 몇 가지 옵션을 사용할 수 있으며이 문서는 목표-C 생성 된 인터페이스 헤더를 사용 하는 방법을 간략하게 설명 합니다. 헤더는 프레임 워크가 생성 될 때 Xcode 도구에 의해 자동으로 생성 되며, 관리 되는 지역에서 Swift 세계로 통신 하는 방법으로 사용 됩니다.

## <a name="prerequisites"></a>사전 요구 사항

이 연습을 완료 하려면 다음이 필요 합니다.

- [Xcode](https://apps.apple.com/us/app/xcode/id497799835)
- [Mac용 Visual Studio](https://visualstudio.microsoft.com/downloads)
- [Objective Sharpie](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/get-started#installing-objective-sharpie)
- [Appcenter CLI](https://docs.microsoft.com/appcenter/test-cloud/) (선택 사항)

## <a name="build-a-native-library"></a>네이티브 라이브러리 빌드

첫 번째 단계는 목표-C 헤더를 사용 하는 네이티브 Swift 프레임 워크를 빌드하는 것입니다. 프레임 워크는 일반적으로 타사 개발자가 제공 하며 다음 디렉터리의 패키지에 포함 된 헤더를 포함 ** \<FrameworkName> 합니다. 프레임 워크/헤더/ \<FrameworkName> -Swift**.

이 헤더는 Xamarin.ios 바인딩 메타 데이터를 생성 하 고 Swift framework 멤버를 노출 하는 c # 클래스를 생성 하는 데 사용 되는 공용 인터페이스를 노출 합니다. 헤더가 존재 하지 않거나 불완전 한 공용 인터페이스를 포함 하는 경우 (예: 클래스/멤버가 표시 되지 않는 경우) 두 가지 옵션이 있습니다.

- Swift 소스 코드를 업데이트 하 여 헤더를 생성 하 고 필요한 멤버를 특성으로 표시 합니다. `@objc`
- 공용 인터페이스를 제어 하 고 기본 프레임 워크에 대 한 모든 호출을 프록시 하는 프록시 프레임 워크 빌드

이 자습서에서는 항상 사용할 수 있는 타사 소스 코드에 대 한 종속성이 적기 때문에 두 번째 방법을 설명 합니다. 첫 번째 방법을 사용 하지 않는 또 다른 이유는 이후 프레임 워크 변경을 지 원하는 데 필요한 추가 노력입니다. 타사 소스 코드에 변경 내용을 추가 하는 것을 시작한 후에는 이러한 변경 내용을 지원 하 고 이후의 모든 업데이트를 사용 하 여 잠재적으로 병합할 책임이 있습니다.

예를 들어이 자습서에서는 [Gigya SWIFT SDK](https://developers.gigya.com/display/GD/Swift+SDK) 에 대 한 바인딩을 만듭니다.

1. Xcode를 열고 Xamarin.ios 코드와 타사 Swift 프레임 워크 간의 프록시가 될 새 Swift 프레임 워크를 만듭니다. **파일 > 새 > 프로젝트** 를 클릭 하 고 마법사의 단계를 따릅니다.

    ![xcode create framework 프로젝트](walkthrough-images/xcode-create-framework-project.png)

    ![xcode name framework 프로젝트](walkthrough-images/xcode-name-framework-project.png)

1. 개발자 웹 사이트에서 [Gigya framework](https://developers.gigya.com/display/GD/Swift+SDK) 를 다운로드 하 고 압축을 풉니다. 작성 당시 최신 버전은 [Gigya SWIFT SDK 1.0.9](https://downloads.gigya.com/predownload?fileName=Swift-Core-framework-1.0.9.zip) 입니다.

1. 프로젝트 파일 탐색기에서 **SwiftFrameworkProxy** 를 선택 하 고 일반 탭을 선택 합니다.

1. **Gigya** 패키지를 일반 탭의 Xcode 프레임 워크 및 라이브러리 목록으로 끌어서 놓습니다. 프레임 워크를 추가 하는 동안 **필요한 경우 항목 복사** 옵션을 선택 합니다.

    ![xcode 복사 프레임 워크](walkthrough-images/xcode-copy-framework.png)

    Swift framework가 프로젝트에 추가 되었는지 확인 합니다. 그렇지 않으면 다음 옵션을 사용할 수 없습니다.

1. **포함 안 함** 옵션이 선택 되어 있는지 확인 합니다 .이 옵션은 나중에 수동으로 제어할 수 있습니다.

    [![xcode donotembed 옵션](walkthrough-images/xcode-donotembed-option.png)](walkthrough-images/xcode-donotembed-option.png#lightbox)

1. 빌드 설정 옵션은 프레임 워크가 No로 설정 된 Swift 라이브러리를 포함 하는 **Swift 표준 라이브러리를 항상 포함**해야 합니다. Swift dylibs가 최종 패키지에 포함 되는 경우 나중에 수동으로 제어 됩니다.

    [![xcode 항상 false 옵션 포함](walkthrough-images/xcode-alwaysembedfalse-option.png)](walkthrough-images/xcode-alwaysembedfalse-option.png#lightbox)

1. **Bitcode 사용** 옵션이 **아니요**로 설정 되어 있는지 확인 합니다. 현재 Xamarin.ios에서 Xamarin.ios는 Bitcode를 포함 하지 않지만 Apple에서는 동일한 아키텍처를 지원 하기 위해 모든 라이브러리가 필요 합니다.

    [![xcode enable bitcode false 옵션](walkthrough-images/xcode-enablebitcodefalse-option.png)](walkthrough-images/xcode-enablebitcodefalse-option.png#lightbox)

    프레임 워크에 대해 다음 터미널 명령을 실행 하 여 결과 프레임 워크에 Bitcode 옵션이 사용 하지 않도록 설정 되어 있는지 확인할 수 있습니다.

    ```bash
    otool -l SwiftFrameworkProxy.framework/SwiftFrameworkProxy | grep __LLVM
    ```

    출력은 비어 있어야 합니다. 그렇지 않으면 특정 구성에 대 한 프로젝트 설정을 검토 합니다.

1. **목표-C 생성 된 인터페이스 헤더 이름** 옵션이 설정 되어 있는지 확인 하 고 헤더 이름을 지정 합니다. 기본 이름은 ** \<FrameworkName> -Swift입니다.**

    [![xcode objectice-c 헤더 사용 옵션](walkthrough-images/xcode-objcheaderenabled-option.png)](walkthrough-images/xcode-objcheaderenabled-option.png#lightbox)

1. 원하는 메서드를 노출 하 고 특성을 사용 하 여 표시 `@objc` 하 고 아래 정의 된 추가 규칙을 적용 합니다. 이 단계를 거치지 않고 프레임 워크를 빌드하면 생성 된 목표-C 헤더가 비어 있고 Xamarin.ios가 Swift framework 멤버에 액세스할 수 없습니다. 새 Swift 파일 **SwiftFrameworkProxy** 를 만들고 다음 코드를 정의 하 여 기본 GIGYA Swift SDK에 대 한 초기화 논리를 노출 합니다.

    ```swift
    import Foundation
    import UIKit
    import Gigya

    @objc(SwiftFrameworkProxy)
    public class SwiftFrameworkProxy : NSObject {

        @objc
        public func initFor(apiKey: String) -> String {
            Gigya.sharedInstance().initFor(apiKey: apiKey)
            let gigyaDomain = Gigya.sharedInstance().config.apiDomain
            let result = "Gigya initialized with domain: \(gigyaDomain)"
            return result
        }
    }
    ```

    위의 코드에 대 한 몇 가지 중요 한 참고 사항:

    - 원본 타사 Gigya SDK에서 Gigya 모듈을 가져와 이제 프레임 워크의 모든 멤버에 액세스할 수 있습니다.
    - 이름을 지정 하는 특성을 사용 하 여 SwiftFrameworkProxy 클래스 `@objc` 를 표시 합니다. 그렇지 않으면와 같이 읽을 수 없는 고유 이름이 생성 됩니다 `_TtC19SwiftFrameworkProxy19SwiftFrameworkProxy` . 형식 이름은 나중에 이름으로 사용 되므로 명확 하 게 정의 해야 합니다.
    - 에서 프록시 클래스를 상속 합니다 `NSObject` . 그렇지 않은 경우에는 목표-C 헤더 파일에 생성 되지 않습니다.
    - 모든 멤버를로 표시 되도록 표시 `public` 합니다.

1. 스키마 빌드 구성을 **디버그** 에서 **릴리스**로 변경 합니다. 이렇게 하려면 **Xcode > 대상 > 구성표 편집** 대화 상자를 연 다음 **빌드 구성** 옵션을 **릴리스**로 설정 합니다.

    ![xcode 편집 체계](walkthrough-images/xcode-edit-scheme.png)

    ![xcode 편집 체계 릴리스](walkthrough-images/xcode-edit-scheme-release.png)

1. 이 시점에서 프레임 워크를 만들 준비가 되었습니다. 시뮬레이터 및 장치 아키텍처의 프레임 워크를 빌드한 다음, 출력을 단일 프레임 워크 패키지로 결합 합니다. **Xcodebuild** 도구를 사용 하 여 소스 코드를 빌드하기 위해 설치 된 SDK 버전을 확인 합니다.

    ```bash
    xcodebuild -showsdks
    ```

    다음과 유사하게 출력됩니다.

    ```bash
    iOS SDKs:
            iOS 13.2                        -sdk iphoneos13.2
    iOS Simulator SDKs:
            Simulator - iOS 13.2            -sdk iphonesimulator13.2
    macOS SDKs:
            DriverKit 19.0                  -sdk driverkit.macosx19.0
            macOS 10.15                     -sdk macosx10.15
    tvOS SDKs:
            tvOS 13.2                       -sdk appletvos13.2
    tvOS Simulator SDKs:
            Simulator - tvOS 13.2           -sdk appletvsimulator13.2
    watchOS SDKs:
            watchOS 6.1                     -sdk watchos6.1
    watchOS Simulator SDKs:
            Simulator - watchOS 6.1         -sdk watchsimulator6.1
    ```

    원하는 iOS SDK 및 iOS 시뮬레이터 SDK 버전 (이 경우 버전 13.2)을 선택 하 고 다음 명령을 사용 하 여 빌드를 실행 합니다.

    ```bash
    xcodebuild -sdk iphonesimulator13.2 -project "Swift/SwiftFrameworkProxy/SwiftFrameworkProxy.xcodeproj" -configuration Release
    xcodebuild -sdk iphoneos13.2 -project "Swift/SwiftFrameworkProxy/SwiftFrameworkProxy.xcodeproj" -configuration Release
    ```

    > [!TIP]
    > Project 대신 작업 영역이 있는 경우 작업 영역을 빌드하고 대상을 필수 매개 변수로 지정 합니다. 또한 작업 영역에 대 한 출력 디렉터리를 지정 하려고 합니다 .이 디렉터리는 프로젝트 빌드와는 다릅니다.

    > [!TIP]
    > [도우미 스크립트](https://github.com/alexeystrakh/xamarin-binding-swift-framework/blob/master/Swift/Scripts/build.fat.sh#L3-L14) 를 사용 하 여 적용 가능한 모든 아키텍처의 프레임 워크를 빌드하거나 대상 선택기의 Xcode 전환 시뮬레이터 및 장치 에서만 빌드할 수 있습니다.

1. 각 플랫폼에 대해 두 개의 Swift 프레임 워크가 있으며 나중에 Xamarin.ios 바인딩 프로젝트에 포함 될 단일 패키지로 결합 합니다. 두 아키텍처를 결합 하는 fat 프레임 워크를 만들려면 다음 단계를 수행 해야 합니다. 프레임 워크 패키지는 파일 추가, 제거 및 교체와 같은 모든 유형의 작업을 수행할 수 있는 폴더 일 뿐입니다.

    - **Iphoneos** 및 **드롭다운에서 iphonesimulator 대상을** 하위 폴더를 사용 하 여 빌드 출력 폴더로 이동한 후 프레임 워크 중 하나를 최종 출력 (fat 프레임 워크)의 초기 버전으로 복사 합니다.

        ```bash
        cp -R "Release-iphoneos" "Release-fat"
        ```

    - 다른 빌드에서 모듈을 fat 프레임 워크 모듈과 결합

        ```bash
        cp -R "Release-iphonesimulator/SwiftFrameworkProxy.framework/Modules/SwiftFrameworkProxy.swiftmodule/" "Release-fat/SwiftFrameworkProxy.framework/Modules/SwiftFrameworkProxy.swiftmodule/"
        ```

    - Iphoneos + 드롭다운에서 iphonesimulator 대상을 구성을 fat 프레임 워크로 결합

        ```bash
        lipo -create -output "Release-fat/SwiftFrameworkProxy.framework/SwiftFrameworkProxy" "Release-iphoneos/SwiftFrameworkProxy.framework/SwiftFrameworkProxy" "Release-iphonesimulator/SwiftFrameworkProxy.framework/SwiftFrameworkProxy"
        ```

    - 결과 확인

        ```bash
        lipo -info "Release-fat/SwiftFrameworkProxy.framework/SwiftFrameworkProxy"
        ```

        출력은 프레임 워크와 포함 된 아키텍처의 이름을 반영 하 여 다음을 렌더링 해야 합니다.

        ```bash
        Architectures in the fat file: Release-fat/SwiftFrameworkProxy.framework/SwiftFrameworkProxy are: x86_64 arm64
        ```

    > [!TIP]
    > 단일 플랫폼만 지원 하려는 경우 (예: 장치 에서만 실행 될 수 있는 앱을 빌드하는 경우), fat 라이브러리를 만드는 단계를 건너뛰고 이전 장치 빌드에서 출력 프레임 워크를 사용할 수 있습니다.

    > [!TIP]
    > [도우미 스크립트](https://github.com/alexeystrakh/xamarin-binding-swift-framework/blob/master/Swift/Scripts/build.fat.sh#L16-L24) 를 사용 하 여 위의 모든 단계를 자동화 하는 fat 프레임 워크를 만들 수도 있습니다.

## <a name="prepare-metadata"></a>메타 데이터 준비

이번에는 Xamarin.ios 바인딩에서 사용할 준비가 된 목표로 생성 된 인터페이스 헤더가 있는 프레임 워크를 사용 해야 합니다.  다음 단계는 c # 클래스를 생성 하기 위해 바인딩 프로젝트에서 사용 되는 API 정의 인터페이스를 준비 하는 것입니다. 이러한 정의는 [목표 Sharpie](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/) 도구 및 생성 된 헤더 파일에서 수동으로 또는 자동으로 만들 수 있습니다. Sharpie를 사용 하 여 메타 데이터를 생성 합니다.

1. 공식 다운로드 웹 사이트에서 최신 [목표 Sharpie](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/) 도구를 다운로드 하 고 마법사에 따라 설치 합니다. 설치가 완료 되 면 sharpie 명령을 실행 하 여 확인할 수 있습니다.

    ```bash
    sharpie -v
    ```

1. Sharpie 및 자동 생성 된 목표-C 헤더 파일을 사용 하 여 메타 데이터를 생성 합니다.

    ```bash
    sharpie bind --sdk=iphoneos13.2 --output="XamarinApiDef" --namespace="Binding" --scope="Release-fat/SwiftFrameworkProxy.framework/Headers/" "Release-fat/SwiftFrameworkProxy.framework/Headers/SwiftFrameworkProxy-Swift.h"
    ```

    출력은 생성 되는 메타 데이터 파일 ( **ApiDefinitions.cs** 및 **StructsAndEnums.cs**)을 반영 합니다. 다음 단계를 위해 이러한 파일을 저장 하 여 네이티브 참조와 함께 Xamarin.ios 바인딩 프로젝트에 포함 합니다.

    ```bash
    Parsing 1 header files...
    Binding...
        [write] ApiDefinitions.cs
        [write] StructsAndEnums.cs
    ```

    이 도구는 다음 코드와 유사 하 게 표시 되는 각각의 노출 된 목표-C 멤버에 대해 c # 메타 데이터를 생성 합니다. 볼 수 있듯이 사용자가 읽을 수 있는 형식 및 간단한 멤버 매핑이 있으므로 수동으로 정의할 수 있습니다.

    ```csharp
    [Export ("initForApiKey:")]
    string InitForApiKey (string apiKey);
    ```

    > [!TIP]
    > 헤더 이름에 대 한 기본 Xcode 설정을 변경한 경우 헤더 파일 이름이 다를 수 있습니다. 기본적으로는 **-Swift** 접미사를 사용 하는 프로젝트의 이름을 갖습니다. 프레임 워크 패키지의 헤더 폴더로 이동 하 여 언제 든 지 파일 및 해당 이름을 확인할 수 있습니다.

    > [!TIP]
    > 자동화 프로세스의 일부로, fat 프레임 워크를 만든 후 [도우미 스크립트](https://github.com/alexeystrakh/xamarin-binding-swift-framework/blob/master/Swift/Scripts/build.fat.sh#L35) 를 사용 하 여 메타 데이터를 자동으로 생성할 수 있습니다.

## <a name="build-a-binding-library"></a>바인딩 라이브러리 빌드

다음 단계는 Visual Studio 바인딩 템플릿을 사용 하 여 Xamarin.ios 바인딩 프로젝트를 만들고, 필요한 메타 데이터와 네이티브 참조를 추가한 다음, 프로젝트를 빌드하여 사용할 수 있는 라이브러리를 생성 하는 것입니다.

1. Mac용 Visual Studio를 열고 새 Xamarin.ios 바인딩 라이브러리 프로젝트를 만들고 이름을 지정 합니다 .이 경우 SwiftFrameworkProxy를 입력 하 고 마법사를 완료 합니다. Xamarin.ios 바인딩 템플릿은 다음 경로에서 찾을 수 있습니다. **ios > library > 바인딩 라이브러리**:

    ![visual studio 바인딩 라이브러리 만들기](walkthrough-images/visualstudio-create-binding-library.png)

1. 기존 메타 데이터 파일 **ApiDefinition.cs** 및 **Structs.cs** 는 목표 Sharpie 도구에 의해 생성 된 메타 데이터로 완전히 대체 되므로 삭제 합니다.
1. 이전 단계 중 하나에서 Sharpie에 의해 생성 된 메타 데이터를 복사 하 고, 속성 창에서 **ApiDefinitions.cs** 파일에 대 한 **Objbindingapidefinition** 및 **StructsAndEnums.cs** 파일에 대 한 **objbindingapidefinition** 빌드 작업을 선택 합니다.

    ![visual studio 프로젝트 구조 메타 데이터](walkthrough-images/visualstudio-project-structure-metadata.png)

    메타 데이터 자체는 c # 언어를 사용 하 여 노출 된 각 목표 C 클래스 및 멤버를 설명 합니다. C # 선언과 함께 원래 목표-C 헤더 정의를 볼 수 있습니다.

    ```csharp
    // @interface SwiftFrameworkProxy : NSObject
    [BaseType (typeof(NSObject))]
    interface SwiftFrameworkProxy
    {
        // -(NSString * _Nonnull)initForApiKey:(NSString * _Nonnull)apiKey __attribute__((objc_method_family("none"))) __attribute__((warn_unused_result));
        [Export ("initForApiKey:")]
        string InitForApiKey (string apiKey);
    }
    ```

    이는 유효한 c # 코드 이더라도 그대로 사용 되지 않고, Xamarin.ios 도구에서이 메타 데이터 정의에 따라 c # 클래스를 생성 하는 데 사용 됩니다. 결과적으로, 인터페이스 대신 SwiftFrameworkProxy 코드에서 인스턴스화할 수 있는 동일한 이름의 c # 클래스를 가져옵니다. 이 클래스는 메타 데이터에 의해 정의 된 메서드, 속성 및 기타 멤버를 가져옵니다 .이 멤버는 c # 방식으로 호출 됩니다.

1. 생성 된 이전 fat 프레임 워크 및 해당 프레임 워크의 각 종속성에 네이티브 참조를 추가 합니다. 이 경우 SwiftFrameworkProxy 및 Gigya framework 네이티브 참조를 모두 바인딩 프로젝트에 추가 합니다.

    - 네이티브 프레임 워크 참조를 추가 하려면 finder를 열고 프레임 워크를 사용 하 여 폴더로 이동 합니다. 솔루션 탐색기의 네이티브 참조 위치에 프레임 워크를 끌어서 놓습니다. 또는 네이티브 참조 폴더에서 상황에 맞는 메뉴 옵션을 사용 하 고 **네이티브 참조 추가** 를 클릭 하 여 프레임 워크를 조회 하 고 추가할 수 있습니다.

    ![visual studio 프로젝트 구조 네이티브 참조](walkthrough-images/visualstudio-project-structure-nativerefs.png)

    - 모든 네이티브 참조의 속성을 업데이트 하 고 세 가지 중요 한 옵션을 확인 합니다.

        - 스마트 링크 설정 = true
        - Force Load = false 설정
        - 원래 프레임 워크를 만드는 데 사용 되는 프레임 워크 목록을 설정 합니다. 이 경우 각 프레임 워크에는 두 개의 종속성 인 Foundation 및 UIKit만 있습니다. 프레임 워크 필드로 설정 합니다.

        ![visual studio nativeref 프록시 옵션](walkthrough-images/visualstudio-nativeref-proxy-options.png)

        지정할 추가 링커 플래그가 있는 경우 링커 플래그 필드에서 설정 합니다. 이 경우에는 비워 둡니다.

    - 필요한 경우 추가 링커 플래그를 지정 합니다. 바인딩하는 라이브러리가 목표-C Api만 제공 하지만 내부적으로 Swift을 사용 하는 경우 다음과 같은 문제가 발생할 수 있습니다.

        ```Console
        error MT5209 : Native linking error : warning: Auto-Linking library not found for -lswiftCore
        error MT5209 : Native linking error : warning: Auto-Linking library not found for -lswiftQuartzCore
        error MT5209 : Native linking error : warning: Auto-Linking library not found for -lswiftCoreImage
        ```

        네이티브 라이브러리에 대 한 바인딩 프로젝트의 속성에서 링커 플래그에 다음 값을 추가 해야 합니다.

        ```Linker
        L/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/swift/iphonesimulator/ -L/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/swift/iphoneos -Wl,-rpath -Wl,@executable_path/Frameworks
        ```

        처음 두 가지 옵션 (이 옵션  `-L ...`   )은 네이티브 컴파일러에 swift 라이브러리를 찾을 위치를 알려 줍니다. 네이티브 컴파일러는 올바른 아키텍처가 없는 라이브러리를 무시 합니다. 즉, 시뮬레이터 라이브러리와 장치 라이브러리의 위치를 동시에 전달 하 여 시뮬레이터 및 장치 빌드 모두에 대해 작동 하도록 할 수 있습니다. 이러한 경로는 iOS에만 정확 하 고 tvOS 및 watchOS는 업데이트 해야 합니다. 한 가지 단점은이 접근 방식을 사용 하는 경우에는 Xcode의 올바른 작업이 필요 합니다. 바인딩 라이브러리의 소비자가 다른 위치에 Xcode 경우에는 작동 하지 않습니다. 대체 솔루션은 실행 프로젝트의 iOS 빌드 옵션 ()에서 추가 mtouch 인수에 이러한 옵션을 추가 하는 것입니다 `--gcc_flags -L... -L...` . 세 번째 옵션은 OS가 찾을 수 있도록 네이티브 링커가 실행 파일에 swift 라이브러리의 위치를 저장 하도록 합니다.

1. 최종 작업은 라이브러리를 빌드하고 컴파일 오류가 없는지 확인 하는 것입니다. 일반적으로 목표 Sharpie에서 생성 된 바인딩 메타 데이터는 특성으로 주석이 추가 됩니다  `[Verify]`   . 이러한 특성은 바인딩을 원래 목표-C 선언 (바인딩된 선언 위의 설명에 제공 됨)과 비교 하 여 목표 Sharpie이 올바른 사물 인지 확인 해야 함을 의미 합니다. [다음 링크](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/platform/verify)를 사용 하 여 특성으로 표시 된 멤버에 대해 자세히 알아볼 수 있습니다. 프로젝트가 빌드되면 Xamarin.ios 응용 프로그램에서 사용 될 수 있습니다.

## <a name="consume-the-binding-library"></a>바인딩 라이브러리 사용

마지막 단계는 Xamarin.ios 응용 프로그램에서 Xamarin.ios 바인딩 라이브러리를 사용 하는 것입니다. 새 Xamarin.ios 프로젝트를 만들고, 바인딩 라이브러리에 참조를 추가 하 고, Gigya Swift SDK를 활성화 합니다.

1. Xamarin.ios 프로젝트를 만듭니다. **IOS > 앱 > 단일 뷰 앱** 을 시작 지점으로 사용할 수 있습니다.

    ![visual studio 앱 새로 만들기](walkthrough-images/visualstudio-app-new.png)

1. 이전에 만든 대상 프로젝트 또는 .dll에 바인딩 프로젝트 참조를 추가 합니다. 바인딩 라이브러리를 일반 Xamarin.ios 라이브러리로 처리 합니다.

    ![visual studio 앱 참조](walkthrough-images/visualstudio-app-refs.png)

1. 앱의 소스 코드를 업데이트 하 고 Gigya SDK를 활성화 하는 기본 ViewController에 초기화 논리를 추가 합니다.

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();
        var proxy = new SwiftFrameworkProxy();
        var result = proxy.InitForApiKey("APIKey");
        System.Diagnostics.Debug.WriteLine(result);
    }
    ```

1. **BtnLogin** 이름으로 단추를 만들고 다음 단추 클릭 처리기를 추가 하 여 인증 흐름을 활성화 합니다.

    ```csharp
    private void btnLogin_Tap(object sender, EventArgs e)
    {
        _proxy.LoginWithProvider(GigyaSocialProvidersProxy.Instagram, this, (result, error) =>
        {
            // process your login result here
        });
    }
    ```

1. 앱을 실행 하 고, 디버그 출력에서 다음 줄이 표시 되어야 합니다 `Gigya initialized with domain: us1.gigya.com` . 단추를 클릭 하 여 인증 흐름을 활성화 합니다.

    [![swift 프록시 결과](walkthrough-images/swiftproxy-result.png)](walkthrough-images/swiftproxy-result.png#lightbox)

지금까지 Swift 프레임 워크를 사용 하는 Xamarin.ios 앱 및 바인딩 라이브러리를 성공적으로 만들었습니다. 위의 응용 프로그램은 iOS 12.2 이상에서 시작 되므로 iOS 12.2 이상에서 실행 됩니다 .이 iOS 버전에서 시작 하는 ABI에는 [ABI 안정성](https://swift.org/blog/swift-5-1-released/) 및 모든 IOS 시작 Swift 런타임 라이브러리가 포함 되어 Swift 5.1 +로 컴파일된 응용 프로그램을 실행 하는 데 사용할 수 있습니다. 이전 iOS 버전에 대 한 지원을 추가 해야 하는 경우 몇 가지 단계를 더 수행 해야 합니다.

1. IOS 12.1 및 이전 버전에 대 한 지원을 추가 하려면 프레임 워크를 컴파일하는 데 사용 되는 특정 Swift dylibs를 제공 하려고 합니다. [SwiftRuntimeSupport](https://www.nuget.org/packages/Xamarin.iOS.SwiftRuntimeSupport/) NuGet 패키지를 사용 하 여 필요한 라이브러리를 IPA로 처리 하 고 복사 합니다. 대상 프로젝트에 NuGet 참조를 추가 하 고 응용 프로그램을 다시 빌드합니다. 추가 단계가 필요 하지 않습니다. NuGet 패키지는 빌드 프로세스를 사용 하 여 실행 되는 특정 작업을 설치 하 고, 필수 Swift dylibs를 식별 하 고, 최종 IPA를 사용 하 여 패키지 합니다.

1. 앱을 앱 스토어에 제출 하기 위해 Xcode 및 배포 옵션을 사용 하려는 경우 AppStore에서 허용 되도록 IPA 파일 및 **SwiftSupport** 폴더 dylibs를 업데이트 합니다.

    ○ 앱을 보관 합니다. Mac용 Visual Studio 메뉴에서 **게시를 위해 빌드 > 보관을**선택 합니다.

    ![게시를 위한 visual studio 보관 파일](walkthrough-images/visualstudio-archiveforpublishing.png)

    이 작업을 수행 하면 프로젝트가 빌드되고 Xcode에서 배포용으로 액세스할 수 있는 구성 도우미에 게 달성 됩니다.

    ○는 Xcode를 통해 배포 합니다. Xcode를 열고 **창 > 구성 도우미** 메뉴 옵션으로 이동 합니다.

    ![visual studio 보관](walkthrough-images/visualstudio-archives.png)

    이전 단계에서 만든 아카이브를 선택 하 고 앱 배포 단추를 클릭 합니다. 마법사에 따라 응용 프로그램을 AppStore에 업로드 합니다.

1. 이 단계는 선택 사항 이지만 앱이 iOS 12.1 및 이전 버전 및 12.2에서 실행 될 수 있는지 확인 하는 것이 중요 합니다. Test Cloud 및 UITest framework를 통해이 작업을 수행할 수 있습니다. UITest 프로젝트 및 앱을 실행 하는 기본 UI 테스트를 만듭니다.

    - UITest 프로젝트를 만들고 Xamarin.ios 응용 프로그램에 대해 구성 합니다.

        ![visual studio uitest 새로 만들기](walkthrough-images/visualstudio-uitest-new.png)

        > [!TIP]
        > UITest 프로젝트를 만들고 [다음 링크로](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)앱에 대해 구성 하는 방법에 대 한 자세한 내용을 확인할 수 있습니다.

    - 앱을 실행 하 고 Swift SDK 기능 중 일부를 사용 하는 기본 테스트를 만듭니다. 이 테스트는 앱을 활성화 하 고 로그인 한 다음 취소 단추를 누릅니다.

        ```csharp
        [Test]
        public void HappyPath()
        {
            app.WaitForElement(StatusLabel);
            app.WaitForElement(LoginButton);
            app.Screenshot("App loaded.");
            Assert.AreEqual(app.Query(StatusLabel).FirstOrDefault().Text, "Gigya initialized with domain: us1.gigya.com");

            app.Tap(LoginButton);
            app.WaitForElement(GigyaWebView);
            app.Screenshot("Login activated.");

            app.Tap(CancelButton);
            app.WaitForElement(LoginButton);
            app.Screenshot("Login cancelled.");
        }
        ```

        > [!TIP]
        > Uitest framework 및 UI 자동화에 대 한 자세한 내용은 [다음 링크를](https://docs.microsoft.com/appcenter/test-cloud/uitest/)참조 하세요.

    - App center에서 iOS 앱을 만들고, 테스트를 실행 하기 위해 새 장치 집합을 사용 하 여 새 테스트 실행을 만듭니다.

        ![visual studio app center 새로 만들기](walkthrough-images/visualstudio-appcenter-new.png)

        > [!TIP]
        > [다음 링크를](https://docs.microsoft.com/appcenter/test-cloud/)통해 appcenter Test Cloud에 대해 자세히 알아보세요.

    - Appcenter CLI 설치

        ```bash
        npm install -g appcenter-cli
        ```

        > [!IMPORTANT]
        > 노드 v 6.3 이상을 설치 했는지 확인 합니다.

    - 다음 명령을 사용 하 여 테스트를 실행 합니다. 또한 appcenter 명령줄이 현재 로그인 되어 있는지 확인 합니다.

        ```bash
        appcenter test run uitest --app "Mobile-Customer-Advisory-Team/SwiftBinding.iOS" --devices a7e7cb50 --app-path "Xamarin.SingleView.ipa" --test-series "master" --locale "en_US" --build-dir "Xamarin/Xamarin.SingleView.UITests/bin/Debug/"
        ```

    - 결과를 확인 합니다. AppCenter 포털에서 테스트 **> 테스트 > 테스트 실행**으로 이동 합니다.

        ![visual studio appcenter uitest 결과](walkthrough-images/visualstudio-appcenter-uitest-result.png)

        그런 다음 원하는 테스트 실행을 선택 하 고 결과를 확인 합니다.

        ![visual studio appcenter uitest 실행](walkthrough-images/visualstudio-appcenter-uitest-runs.png)

Xamarin.ios 바인딩 라이브러리를 통해 네이티브 Swift 프레임 워크를 사용 하는 기본 Xamarin.ios 응용 프로그램을 개발 했습니다. 이 예제에서는 선택한 프레임 워크를 사용 하는 간단한 방법을 제공 하 고 실제 응용 프로그램에서는 이러한 Api에 대 한 메타 데이터를 준비 하 고 더 많은 Api를 노출 해야 합니다. 메타 데이터를 생성 하는 스크립트는 나중에 프레임 워크 Api를 변경 하는 것을 단순화 합니다.

## <a name="related-links"></a>관련 링크

- [Xcode](https://apps.apple.com/us/app/xcode/id497799835)
- [Mac용 Visual Studio](https://visualstudio.microsoft.com/downloads)
- [Objective Sharpie](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/)
- [Sharpie 메타 데이터 확인](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/platform/verify)
- [바인딩 목표-C 프레임 워크](https://docs.microsoft.com/xamarin/ios/platform/binding-objective-c/walkthrough)
- [Gigya iOS SDK (Swift framework)](https://developers.gigya.com/display/GD/Swift+SDK)
- [Swift 5.1 ABI 안정성](https://swift.org/blog/swift-5-1-released/)
- [SwiftRuntimeSupport NuGet](https://www.nuget.org/packages/Xamarin.iOS.SwiftRuntimeSupport/)
- [Xamarin UITest automation](https://docs.microsoft.com/appcenter/test-cloud/uitest/)
- [Xamarin.ios UITest 구성](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)
- [AppCenter Test Cloud](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)
- [샘플 프로젝트 리포지토리](https://github.com/alexeystrakh/xamarin-binding-swift-framework)
