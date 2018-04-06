---
title: 'Hello, iOS: 자세히 알아보기'
description: 두 부분으로 구성된 이 가이드에서는 Mac용 Visual Studio 또는 Visual Studio를 사용하여 기본 Xamarin.iOS 응용 프로그램을 빌드하고, Xamarin을 사용하여 iOS 응용 프로그램 개발에 대한 기본 사항을 이해하는 방법을 설명합니다. Xamarin.iOS 응용 프로그램을 빌드 및 배포하는 데 필요한 도구, 개념 및 단계를 소개합니다.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 61ba3a7e-fe11-4439-8bc8-9809512b8eff
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 5ca2918a0348254407fcbfff030def6c36af4988
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="hello-ios-deep-dive"></a>Hello, iOS 자세히 알아보기

빠른 시작 연습에서는 기본 Xamarin.iOS 응용 프로그램의 빌드 및 실행을 소개하였습니다. 이제, 더 복잡한 프로그램을 빌드할 수 있도록 iOS 응용 프로그램의 작동 방식을 심층적으로 알아볼 시간입니다. 이 가이드에서는 iOS 응용 프로그램 개발의 기본 개념을 이해할 수 있도록 Hello, iOS 연습에 있는 단계를 검토합니다.

이 문서에서는 다음과 같은 주제를 살펴봅니다.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- **Mac용 Visual Studio 소개** – Mac용 Visual Studio 및 새로운 응용 프로그램 만들기를 소개합니다.
- **Xamarin.iOS 응용 프로그램 분석** - Xamarin.iOS 응용 프로그램의 핵심 부분을 안내합니다.
- **아키텍처 및 앱의 기본** – iOS 응용 프로그램의 파트들을 검토하고 파트 간 관계를 알아봅니다.
- **UI(사용자 인터페이스)** – iOS 디자이너를 통해 사용자 인터페이스를 만듭니다.
- **뷰 컨트롤러 및 뷰 수명 주기** – 뷰 수명 주기 및 뷰 컨트롤러로 콘텐츠 뷰 계층 구조 관리에 대해 소개합니다.
- **테스트, 배포 및 마무리** - 테스트, 배포, 아트워크 생성 등에 관한 정보를 활용하여 응용 프로그램을 완성합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- **Visual Studio 소개** – Visual Studio 및 새로운 응용 프로그램 만들기를 소개합니다.
- **Xamarin.iOS 응용 프로그램 분석** - Xamarin.iOS 응용 프로그램의 핵심 부분을 안내합니다.
- **아키텍처 및 앱의 기본** – iOS 응용 프로그램의 파트들을 검토하고 파트 간 관계를 알아봅니다.
- **UI(사용자 인터페이스)** – iOS 디자이너를 통해 사용자 인터페이스를 만듭니다.
- **뷰 컨트롤러 및 뷰 수명 주기** – 뷰 수명 주기 및 뷰 컨트롤러로 콘텐츠 뷰 계층 구조 관리에 대해 소개합니다.
- **테스트, 배포 및 마무리** - 테스트, 배포, 아트워크 생성 등에 관한 정보를 활용하여 응용 프로그램을 완성합니다.

-----

이 가이드는 단일 화면 iOS 응용 프로그램을 빌드하는 데 필요한 기술 및 정보를 개발하는 데 유용합니다. 이를 통해 Xamarin.iOS 응용 프로그램의 서로 다른 파트들을 이해하고 어떻게 서로 맞도록 조정하는지에 대해 습득합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Mac용 Visual Studio 소개

Mac용 Visual Studio는 Visual Studio와 XCode의 기능을 결합하는 무료 오픈 소스 IDE입니다. 완전히 통합된 비주얼 디자이너, 리팩터리 도구가 함께 포함된 텍스트 편집기, 어셈블리 브라우저, 소스 코드 통합 등을 제공합니다. 이 가이드는 몇 가지 기본 Mac용 Visual Studio 기능을 소개하지만, Mac용 Visual Studio를 처음 접하는 경우 [Mac용 Visual Studio](https://docs.microsoft.com/visualstudio/mac/) 설명서를 확인하세요.

Mac용 Visual Studio는 코드를 *솔루션* 및 *프로젝트*로 구성하는 Visual Studio 연습을 따릅니다. 솔루션은 하나 이상의 프로젝트를 포함할 수 있는 컨테이너입니다. 프로젝트는 응용 프로그램(예: iOS 또는 Android), 지원 라이브러리, 테스트 응용 프로그램 등이 될 수 있습니다. **단일 뷰 응용 프로그램** 템플릿을 사용하여 Phoneword 앱에서 새 iPhone 프로젝트가 추가됩니다. 초기 솔루션은 다음과 같았습니다.

![](hello-ios-deepdive-images/image30.png "초기 솔루션의 스크린샷")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Visual Studio 소개

Visual Studio는 Microsoft의 강력한 IDE입니다. 완전히 통합된 비주얼 디자이너, 리팩터리 도구가 함께 포함된 텍스트 편집기, 어셈블리 브라우저, 소스 코드 통합 등을 제공합니다. 이 가이드에서는 Xamarin 플러그 인을 사용한 몇 가지 기본적인 Visual Studio 기능을 소개합니다.

Visual Studio는 코드를 _솔루션_ 및 *프로젝트*로 구성합니다. 솔루션은 하나 이상의 프로젝트를 포함할 수 있는 컨테이너입니다. 프로젝트는 응용 프로그램(예: iOS 또는 Android), 지원 라이브러리, 테스트 응용 프로그램 등이 될 수 있습니다. **단일 뷰 응용 프로그램** 템플릿을 사용하여 Phoneword 앱에서 새 iPhone 프로젝트가 추가됩니다. 초기 솔루션은 다음과 같았습니다.

![](hello-ios-deepdive-images/vs-image30.png "초기 솔루션의 스크린샷")

-----

<a name="anatomy" />

## <a name="anatomy-of-a-xamarinios-application"></a>Xamarin.iOS 응용 프로그램 분석

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

왼쪽은 *솔루션 패드*로, 솔루션과 연결된 디렉터리 구조와 모든 파일이 포함됩니다.

![](hello-ios-deepdive-images/image31.png "솔루션과 연결된 디렉터리 구조와 모든 파일이 포함된 솔루션 패드")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

오른쪽은 *솔루션 창*으로, 솔루션과 연결된 디렉터리 구조와 모든 파일이 포함됩니다.

![](hello-ios-deepdive-images/vs-image31.png "솔루션과 연결된 디렉터리 구조와 모든 파일이 포함된 솔루션 창")

-----

[Hello, iOS](~/ios/get-started/hello-ios/hello-ios-quickstart.md) 연습에서는 **Phoneword**라는 솔루션을 만들고 내부에 iOS 프로젝트인 **Phoneword_iOS**를 배치했습니다. 프로젝트 내에 있는 항목은 다음과 같습니다.

-  **References** - 응용 프로그램을 빌드하고 실행하는 데 필요한 어셈블리가 포함됩니다. 디렉터리를 확장하면 Xamarin의 Xamarin.iOS 어셈블리에 대한 참조와 함께 [System](http://msdn.microsoft.com/en-us/library/system%28v=vs.110%29.aspx), System.Core 및 [System.Xml](http://msdn.microsoft.com/en-us/library/system.xml%28v=vs.110%29.aspx)과 같은 .NET 어셈블리에 대한 참조를 확인할 수 있습니다.
-  **Packages** - Packages 디렉터리에는 미리 만들어진 NuGet 패키지가 포함됩니다.
-  **Resources** - Resources 폴더는 기타 미디어를 저장합니다.
-  **Main.cs** – 여기에는 응용 프로그램의 주요 진입점이 포함됩니다. 응용 프로그램을 시작하려면 주 응용 프로그램 클래스의 이름인 `AppDelegate`가 전달됩니다.
-  **AppDelegate.cs** – 이 파일은 주 응용 프로그램 클래스를 포함하며 창을 만들고, 사용자 인터페이스를 빌드하고, 운영 체제에서 이벤트를 수신 대기하는 역할을 담당합니다.
-  **Main.storyboard** - 스토리보드는 응용 프로그램의 사용자 인터페이스의 시각적 디자인을 포함합니다. 스토리보드 파일은 iOS 디자이너라고 하는 그래픽 편집기에서 엽니다.
-  **ViewController.cs** – 뷰 컨트롤러는 사용자가 보고 터치하는 화면(뷰)을 제공합니다. 뷰 컨트롤러는 사용자와 뷰 간 상호 작용을 처리하는 일을 담당합니다.
-  **ViewController.designer.cs** – `designer.cs`는 뷰의 컨트롤과 뷰 컨트롤러에서 해당 코드 표현 간의 연결점으로 사용되는 자동으로 생성되는 파일입니다. 내부 배관 파일이기 때문에 IDE는 수동 변경을 덮어쓰게 되며 대부분의 경우 이 파일은 무시할 수 있습니다. 비주얼 디자이너와 백업 코드 사이의 관계에 대한 자세한 내용은 [iOS 디자이너 소개](~/ios/user-interface/designer/introduction.md) 가이드를 참조하세요.
-  **Info.plist** – `Info.plist`에서는 응용 프로그램 이름, 아이콘, 시작 이미지 등과 같은 응용 프로그램 속성이 설정됩니다. 강력한 파일로, 전체 소개는 [속성 목록 작업](~/ios/app-fundamentals/property-lists.md) 가이드에서 확인할 수 있습니다.
-  **Entitlements.plist** - 자격 속성 목록을 통해 iCloud, PassKit 등의 *기능*(앱 스토어 기술이라고도 함)을 지정할 수 있습니다. `Entitlements.plist`에 대한 자세한 내용은 [속성 목록 작업](~/ios/app-fundamentals/property-lists.md) 가이드에서 확인할 수 있습니다. 자격에 대한 일반적인 소개의 경우 [장치 프로비전](~/ios/get-started/installation/device-provisioning/index.md) 가이드를 참조하세요.

## <a name="architecture-and-app-fundamentals"></a>아키텍처 및 앱 기본 사항

iOS 응용 프로그램이 사용자 인터페이스를 로드하려면 두 가지 사항이 준비되어야 합니다. 첫 번째, 응용 프로그램은 응용 프로그램의 프로세스가 메모리에 로드될 때 실행하는 첫 번째 코드인 *진입점*을 정의해야 합니다. 두 번째, 응용 프로그램 수준의 이벤트를 처리하고 운영 체제와 상호 작용하는 클래스를 정의해야 합니다.

이 섹션에서는 다음 다이어그램에 표시된 관계를 연구합니다.

[![](hello-ios-deepdive-images/image32.png "아키텍처 및 앱의 기본 관계가 이 다이어그램에 설명되었습니다.")](hello-ios-deepdive-images/image32.png#lightbox)

처음부터 시작하여 응용 프로그램 시작 시 어떻게 되는지 알아봅니다.

### <a name="main"></a>Main

iOS 응용 프로그램의 주 진입점은 `Main.cs` 파일입니다. `Main.cs`에는 새 Xamarin.iOS 응용 프로그램 인스턴스를 만들어 OS 이벤트를 처리하는 *응용 프로그램 대리자* 클래스의 이름을 전달하는 정적 Main 메서드가 포함됩니다. 정적 `Main` 메서드에 대한 템플릿 코드가 아래에 나와 있습니다.

```csharp
using System;
using UIKit;

namespace Phoneword_iOS
{
    public class Application
    {
        static void Main (string[] args)
        {
            UIApplication.Main (args, null, "AppDelegate");
        }
    }
}
```

### <a name="application-delegate"></a>응용 프로그램 대리자

iOS에서 *응용 프로그램 대리자* 클래스는 시스템 이벤트를 처리합니다. 이 클래스는 `AppDelegate.cs` 안에 있습니다. `AppDelegate` 클래스는 응용 프로그램 *창*을 관리합니다. 창은 사용자 인터페이스에 대한 컨테이너 역할을 하는 `UIWindow` 클래스의 단일 인스턴스입니다. 기본적으로 응용 프로그램은 콘텐츠를 로드할 하나의 창만 가져오고, 창은 실제 장치 화면의 크기와 일치하는 경계 사각형을 제공하는 *화면*(단일 `UIScreen` 인스턴스)에 연결됩니다.

또한 *AppDelegate*는 앱 시작이 종료될 때 또는 메모리가 부족할 때와 중요한 응용 프로그램 이벤트에 대한 시스템 업데이트를 구독하는 역할을 합니다.

AppDelegate에 대한 템플릿 코드는 아래와 같습니다.

```csharp
using System;
using Foundation;
using UIKit;

namespace Phoneword_iOS
{

    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        public override UIWindow Window {
            get;
            set;
        }

        ...
    }
}
```

응용 프로그램이 해당 창을 정의하면 사용자 인터페이스 로드를 시작할 수 있습니다. 다음 섹션에서는 UI 만들기를 탐색합니다.

## <a name="user-interface"></a>사용자 인터페이스

iOS 앱의 사용자 인터페이스는 storefront와 비슷합니다. 응용 프로그램은 일반적으로 하나의 창을 가져오지만 필요한 많은 개체로 채울 수 있으며 개체 및 배열은 앱이 표시하려고 항목에 따라 변경할 수 있습니다. 사용자에게 표시되는 항목인 이 시나리오의 개체를 뷰라고 합니다. 응용 프로그램에서 단일 화면을 빌드하려면 뷰가 *콘텐츠 뷰 계층 구조*에 쌓이고 계층 구조는 단일 뷰 컨트롤러에서 관리됩니다. 여러 화면이 있는 응용 프로그램은 각각 고유한 뷰 컨트롤러가 있는 여러 콘텐츠 뷰 계층 구조가 있으며, 응용 프로그램은 사용자가 보는 화면에 따라 다른 콘텐츠 뷰 계층 구조를 만들도록 창에 뷰를 배치합니다.

이 섹션에서는 뷰, 콘텐츠 뷰 계층 구조 및 iOS 디자이너를 설명하면서 사용자 인터페이스에 대해 살펴봅니다.

### <a name="ios-designer-and-storyboards"></a>iOS 디자이너 및 스토리보드

iOS 디자이너는 Xamarin에서 사용자 인터페이스를 빌드하는 비주얼 도구입니다. 다음 스크린샷과 유사한 뷰가 열리는 스토리보드(.storyboard) 파일을 두 번 클릭하여 디자이너를 시작할 수 있습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image33.png "iOS 디자이너 인터페이스")

*스토리보드*는 화면 간 전환 및 관계와 함께 응용 프로그램 화면의 시각적 디자인을 포함하는 파일입니다. 스토리보드에서 응용 프로그램의 화면 표현을 _장면_이라고 합니다. 각 장면은 뷰 컨트롤러와 관리하는 뷰의 스택을 나타냅니다(콘텐츠 뷰 계층 구조). 새 **단일 뷰 응용 프로그램** 프로젝트를 템플릿에서 만들면 Mac용 Visual Studio는 `Main.storyboard`라는 스토리보드 파일을 자동으로 생성하고 아래 스크린샷에 나온 것처럼 단일 장면으로 채웁니다.

![](hello-ios-deepdive-images/image34.png "Mac용 Visual Studio가 자동으로 Main.storyboard라는 스토리보드 파일을 생성하고 단일 장면에 채웁니다.")

장면에 대한 뷰 컨트롤러를 선택하려면 스토리보드 화면 아래쪽의 검은색 표시줄을 선택합니다. 뷰 컨트롤러는 콘텐츠 뷰 계층 구조에 대한 백업 코드를 포함하는 `UIViewController` 클래스의 인스턴스입니다. 이 뷰 컨트롤러의 속성은 아래 스크린샷에 나온 것처럼 **속성 패드**에서 확인하고 설정할 수 있습니다.

![](hello-ios-deepdive-images/image35.png "속성 창")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image33.png "iOS 디자이너 인터페이스")

*스토리보드*는 화면 간 전환 및 관계와 함께 응용 프로그램 화면의 시각적 디자인을 포함하는 파일입니다. 스토리보드에서 응용 프로그램의 화면 표현을 _장면_이라고 합니다. 각 장면은 뷰 컨트롤러와 관리하는 뷰의 스택을 나타냅니다(콘텐츠 뷰 계층 구조). 새 **단일 뷰 응용 프로그램** 프로젝트를 템플릿에서 만들면 Visual Studio는 `Main.storyboard`라는 스토리보드 파일을 자동으로 생성하고 아래 스크린샷에 나온 것처럼 단일 장면으로 채웁니다.

![](hello-ios-deepdive-images/vs-image34.png "Visual Studio가 자동으로 Main.storyboard라는 스토리보드 파일을 생성하고 단일 장면에 채웁니다.")

장면에 대한 뷰 컨트롤러를 선택하려면 스토리보드 화면 아래쪽의 표시줄을 선택합니다. 뷰 컨트롤러는 콘텐츠 뷰 계층 구조에 대한 백업 코드를 포함하는 `UIViewController` 클래스의 인스턴스입니다. 이 뷰 컨트롤러의 속성은 아래 스크린샷에 나온 것처럼 **속성 창**에서 확인하고 설정할 수 있습니다.

![](hello-ios-deepdive-images/vs-image35.png "속성 창")

-----

_뷰_는 장면의 흰색 부분을 클릭하여 선택할 수 있습니다. 뷰는 화면의 영역을 정의하고 해당 영역에서 콘텐츠를 사용하기 위한 인터페이스를 제공하는 `UIView` 클래스의 인스턴스입니다. 기본 뷰는 전체 장치 화면을 채우는 단일 *루트 뷰*입니다.

아래 스크린샷에서 나온 것처럼 장면의 왼쪽에 플래그 아이콘이 포함된 회색 화살표가 있습니다.

 [![](hello-ios-deepdive-images/image37.png "플래그 아이콘이 포함된 회색 화살표")](hello-ios-deepdive-images/image37.png#lightbox)

회색 화살표는 *Segue*(“세그웨이”라고 발음)라고 하는 스토리보드 전환을 나타냅니다. 이 Segue에는 원본이 없으므로 *원본 없는 Segue*라고 합니다. 원본 없는 Segue는 뷰가 시작 시 응용 프로그램의 창으로 로드되는 첫 번째 장면을 가리킵니다. 그 안에 포함된 장면 및 뷰는 앱이 로드 될 때 사용자에게 가장 먼저 표시됩니다.

사용자 인터페이스를 빌드할 때 아래 스크린샷에서 나온 것처럼 추가 뷰를 **도구 상자**에서 디자인 화면의 주 뷰로 끌 수 있습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image38.png "도구 상자에서 디자인 화면에 있는 주 뷰로 끌 수 있는 추가 뷰")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image38.png "도구 상자에서 디자인 화면에 있는 주 뷰로 끌 수 있는 추가 뷰")

-----

이러한 추가 뷰를 *하위 뷰*라고 합니다. 루트 뷰와 하위 뷰는 `ViewController`에서 관리하는 모두 *콘텐츠 뷰 계층 구조*의 일부입니다. **문서 개요** 패드에서 검사를 수행하면 장면에 있는 모든 요소의 개요를 볼 수 있습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image39.png "문서 개요 패드")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image39.png "문서 개요 패드")

-----

하위 뷰는 아래 다이어그램에 강조 표시되어 있습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image40.png "다이어그램에서 강조 표시되어 있는 하위 뷰")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image40.png "다이어그램에서 강조 표시되어 있는 하위 뷰")

-----

다음 섹션에서는 이 장면으로 표시되는 콘텐츠 뷰 계층 구조를 분할합니다.

## <a name="content-view-hierarchy"></a>콘텐츠 뷰 계층 구조

_콘텐츠 뷰 계층 구조_는 아래 다이어그램에 나온 것처럼 단일 뷰 컨트롤러에서 관리하는 뷰 및 하위 뷰의 스택입니다.

 [![](hello-ios-deepdive-images/image41.png "콘텐츠 보기 계층 구조")](hello-ios-deepdive-images/image41.png#lightbox)

`ViewController`의 콘텐츠 뷰 계층 구조를 더 쉽게 확인할 수 있도록 **속성 패드**의 뷰 섹션에서 루트 뷰의 배경 색상을 아래 스크린샷에 나온 것처럼 일시적으로 노란색으로 변경할 수 있습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image42.png "속성 패드의 뷰 섹션에서 루트 뷰의 배경 색상을 노란색으로 변경")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image42.png "속성 패드의 뷰 섹션에서 루트 뷰의 배경 색상을 노란색으로 변경")

-----

다음 다이어그램에는 장치 화면에 사용자 인터페이스를 가져오는 창, 뷰, 하위 뷰 및 뷰 컨트롤러 간의 관계가 나와 있습니다.

 [![](hello-ios-deepdive-images/image43.png "창, 보기, 하위 보기 및 보기 컨트롤러 간의 관계")](hello-ios-deepdive-images/image43.png#lightbox)

다음에서 섹션에서는 코드에서 뷰를 사용하고 뷰 컨트롤러 및 뷰 수명 주기를 사용하여 사용자 상호 작용에 대한 프로그래밍하는 방법에 대해 알아봅니다.

## <a name="view-controllers-and-the-view-lifecycle"></a>뷰 컨트롤러 및 뷰 수명 주기

모든 콘텐츠 뷰 계층 구조에는 사용자 상호 작용을 구동하는 해당 뷰 컨트롤러가 있습니다. 뷰 컨트롤러의 역할은 콘텐츠 뷰 계층 구조에서 뷰를 관리하는 것입니다. 뷰 컨트롤러는 콘텐츠 뷰 계층 구조의 일부가 아니며 인터페이스에는 요소가 없습니다. 대신, 화면에서 개체와 사용자의 상호 작용을 구동하는 코드를 제공합니다.

### <a name="view-controllers-and-storyboards"></a>뷰 컨트롤러 및 스토리보드

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

뷰 컨트롤러는 장면의 아래쪽에 표시줄로 스토리보드에 표시됩니다. 뷰 컨트롤러를 선택하면 해당 속성이 **속성 패드**에 나타납니다.

![](hello-ios-deepdive-images/image44.png "뷰 컨트롤러를 선택하면 해당 속성이 속성 창에 나타남")

이 장면으로 표시되는 콘텐츠 뷰 계층 구조에 대한 사용자 지정 뷰 컨트롤러 클래스는 **속성 패드**에 있는 **ID** 섹션의 **클래스** 속성을 편집하여 설정할 수 있습니다. 예를 들어 **Phoneword** 응용 프로그램은 아래 스크린샷에 나온 것처럼 `ViewController`를 첫 번째 화면에 대한 뷰 컨트롤러로 설정합니다.

![](hello-ios-deepdive-images/image45new.png "Phoneword 응용 프로그램이 ViewController를 뷰 컨트롤러로 설정")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

뷰 컨트롤러는 장면의 아래쪽에 표시줄로 스토리보드에 표시됩니다. 뷰 컨트롤러를 선택하면 해당 속성이 **속성 창**에 나타납니다.

![](hello-ios-deepdive-images/vs-image44.png "뷰 컨트롤러를 선택하면 해당 속성이 속성 창에 나타남")

이 장면으로 표시되는 콘텐츠 뷰 계층 구조에 대한 사용자 지정 뷰 컨트롤러 클래스는 **속성 창**에 있는 **ID** 섹션의 **클래스** 속성을 편집하여 설정할 수 있습니다. 예를 들어 **Phoneword** 응용 프로그램은 아래 스크린샷에 나온 것처럼 `ViewController`를 첫 번째 화면에 대한 뷰 컨트롤러로 설정합니다.

![](hello-ios-deepdive-images/vs-image45.png "Phoneword 응용 프로그램이 ViewController를 뷰 컨트롤러로 설정")

-----

그러면 뷰 컨트롤러의 스토리보드 표시가 `ViewController` C# 클래스로 연결됩니다. `ViewController.cs` 파일을 열고, 아래 코드에 나온 것처럼 뷰 컨트롤러가 `UIViewController`의 *하위 클래스*임을 확인합니다.

```csharp
public partial class ViewController : UIViewController
{
    public ViewController (IntPtr handle) : base (handle)
    {

    }
}
```

이제 `ViewController`가 스토리보드에서 이 뷰 컨트롤러와 연결된 콘텐츠 뷰 계층 구조의 상호 작용을 수행합니다. 다음으로는 뷰 수명 주기라고 하는 프로세스를 소개함으로써 뷰를 관리할 때 뷰 컨트롤러의 역할에 대해 알아봅니다.

> [!NOTE]
> 사용자 상호 작용이 필요 없는 시각적 개체 전용 화면의 경우 **Properties Pad**에서 **Class** 속성을 비워 둘 수 있습니다. 이는 뷰 컨트롤러의 지원 클래스를 `UIViewController`의 기본 구현으로 설정하며, 이는 추가 사용자 지정 코드에서 계획하지 않은 경우 적절합니다.

### <a name="view-lifecycle"></a>뷰 수명 주기

뷰 컨트롤러는 창에서 콘텐츠 뷰 계층 구조를 로드하거나 언로드합니다. 콘텐츠 뷰 계층 구조에서 뷰에서 중요한 문제가 발생한 경우 운영 체제는 뷰 수명 주기의 이벤트를 통해 뷰 컨트롤러에 알립니다. 뷰 수명 주기에서 메서드를 재정의하면 화면에서 개체와 상호 작용할 수 있으며 동적이고 응답성이 뛰어난 사용자 인터페이스를 만들 수 있습니다.

다음은 기본 수명 주기 메서드 및 해당 기능입니다.

-  **ViewDidLoad** - 처음에 *한 번* 호출되면 뷰 컨트롤러가 해당 콘텐츠 뷰 계층 구조를 메모리에 로드합니다. 코드에서 하위 뷰를 먼저 사용할 수 있게 되므로 초기 설치 프로그램을 수행하는 것이 좋습니다.
-  **ViewWillAppear** - 호출될 때마다 뷰 컨트롤러가 콘텐츠 뷰 계층 구조에 추가되고 화면에 나타납니다.
-  **ViewWillDisappear** - 호출될 때마다 뷰 컨트롤러의 뷰가 콘텐츠 뷰 계층 구조에서 제거되고 화면에서 사라집니다. 이 수명 주기 이벤트는 상태를 정리 및 저장하는 데 사용됩니다.
-  **ViewDidAppear** 및 **ViewDidDisappear** - 각각 뷰가 콘텐츠 뷰 계층 구조에서 추가되거나 제거되는 경우 호출됩니다.


사용자 지정 코드가 수명 주기의 단계에서 추가되는 경우 해당 수명 주기 메서드의 *기본 구현*은 *재정의되어야* 합니다. 이는 일부 코드가 이미 연결되어 있는 기존 수명 주기 메서드를 이용하고 추가 코드로 확장하여 달성됩니다. 원본 코드가 새 코드보다 먼저 실행되도록 메서드 내에서 기본 구현을 호출합니다. 다음 섹션에서 이러한 예제를 설명합니다.

뷰 컨트롤러 작업에 대한 자세한 내용은 Apple의 [iOS에 대한 뷰 컨트롤러 프로그래밍 가이드](https://developer.apple.com/library/ios/featuredarticles/ViewControllerPGforiPhoneOS/ViewLoadingandUnloading/ViewLoadingandUnloading.html) 및 [UIViewController 참조](https://developer.apple.com/library/ios/documentation/uikit/reference/UIViewController_Class/Reference/Reference.html)를 확인하세요.

### <a name="responding-to-user-interaction"></a>사용자 상호 작용에 응답

뷰 컨트롤러의 가장 중요한 역할은 단추 누르기, 탐색 등과 같은 사용자 상호 작용에 응답하는 것입니다. 사용자 상호 작용을 처리하는 가장 간단한 방법은 사용자 입력을 수신 대기하도록 컨트롤을 연결하고 입력에 응답하도록 이벤트 처리기를 연결하는 것입니다. 예를 들어 Phoneword 앱에 설명되어 있듯이 터치 이벤트에 응답하도록 단추를 연결할 수 있습니다.

뷰 및 뷰 컨트롤러에 대해 자세히 알아보았습니다. 이제 작동 원리를 살펴보겠습니다.
`Phoneword_iOS` 프로젝트에서 콘텐츠 뷰 계층 구조에 `TranslateButton`이라고 하는 단추가 추가되었습니다.

 [![](hello-ios-deepdive-images/image1.png "콘텐츠 보기 계층 구조에 TranslateButton이라는 단추가 추가되었습니다.")](hello-ios-deepdive-images/image1.png#lightbox)

**이름**이 **속성 패드**의 **단추** 컨트롤에 할당되는 경우 iOS 디자이너는 `ViewController` 클래스 내에서 `TranslateButton`을 사용할 수 있도록 자동으로  **ViewController.designer.cs**의 컨트롤에 매핑합니다. 컨트롤은 먼저 뷰 수명 주기의 `ViewDidLoad` 단계에서 사용할 수 있게 되므로 이 수명 주기 메서드는 사용자의 터치에 응답하는 데 사용됩니다.

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // wire up TranslateButton here
}
```

Phoneword 앱은 `TouchUpInside`라는 터치 이벤트를 사용하여 사용자의 터치를 수신 대기합니다. `TouchUpInside`는 컨트롤의 범위 내 터치 다운(화면을 터치하는 손가락)에 따르는 터치 업 이벤트(화면에서 들어올리는 손가락)를 수신 대기합니다. `TouchUpInside`의 반대는 사용자가 컨트롤을 누를 때 발생하는 `TouchDown` 이벤트입니다. `TouchDown` 이벤트는 많은 노이즈를 캡처하며, 사용자가 컨트롤에서 벗어나 손가락을 밀어서 터치를 취소할 수 있는 옵션은 없습니다. `TouchUpInside`는 **단추** 터치에 응답하는 가장 일반적인 방법이며 사용자가 단추를 누를 때 기대하는 경험을 만듭니다. 이에 대한 자세한 내용은 Apple의 [iOS 휴먼 인터페이스 지침](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/MobileHIG/index.html)에서 확인할 수 있습니다.

앱은 람다가 있는 `TouchUpInside` 이벤트를 처리하지만 대리자 또는 명명된 이벤트 처리기가 사용될 수도 있습니다. 최종 단추 코드는 다음과 유사합니다.

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
      translatedNumber = Core.PhonewordTranslator.ToNumber(PhoneNumberText.Text);
      PhoneNumberText.ResignFirstResponder ();

      if (translatedNumber == "") {
        CallButton.SetTitle ("Call", UIControlState.Normal);
        CallButton.Enabled = false;
      } else {
        CallButton.SetTitle ("Call " + translatedNumber, UIControlState.Normal);
        CallButton.Enabled = true;
      }
  };
}
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Phoneword에 도입된 추가 개념

Phoneword 응용 프로그램에는 이 가이드에서 다루지 않은 몇 가지 개념이 도입되었습니다. 이러한 개념은 다음과 같습니다.

- **단추 텍스트 변경** – Phoneword 앱은 **Button**에서 `SetTitle`을 호출하여 새 텍스트 및 **단추**의 _컨트롤 상태_에 전달하여 **단추**의 텍스트를 변경하는 방법을 보여 줍니다. 예를 들어 다음 코드는 CallButton의 텍스트를 “Call”로 변경합니다.

    ```csharp
    CallButton.SetTitle ("Call", UIControlState.Normal);
    ```
- **설정/해제 단추** – **단추**는 `Enabled` 또는 `Disabled` 상태일 수 있습니다. 사용하지 않도록 설정된 **단추**는 사용자 입력에 응답하지 않습니다. 예를 들어 CallButton.Enabled = false 코드는 `CallButton`을 사용하지 않도록 설정합니다. 단추에 대한 자세한 내용은 [단추](~/ios/user-interface/controls/buttons.md) 가이드를 참조하세요.
- **키보드 해제** – 사용자가 텍스트 필드를 탭하면 iOS는 사용자가 입력할 수 있도록 키보드를 표시합니다. 그러나 키보드를 해제할 수 있는 기본 제공 기능은 없습니다. 사용자가 `TranslateButton`를 누를 때 키보드를 해제하도록 PhoneNumberText.ResignFirstResponder () 코드가 `TranslateButton`에 추가됩니다. 키보드 해제의 또 다른 예제는 [키보드 해제](https://developer.xamarin.com/recipes/ios/input/keyboards/dismiss_the_keyboard) 조리법을 참조하세요.
- **URL로 전화 걸기** – Phoneword 앱에서는 시스템 전화 앱을 실행하는 데 Apple URL 구성표가 사용됩니다. 사용자 지정 URL 구성표는 아래 코드에 표시된 것처럼 “tel:” 접두사와 번역된 전화 번호로 구성됩니다.

    ```csharp
    var url = new NSUrl ("tel:" + translatedNumber);
    if (!UIApplication.SharedApplication.OpenUrl (url))
    {
        // show alert Controller
    }
    ```
- **경고 표시** – 사용자가 통화를 지원하지 않는 장치(예: 시뮬레이터 또는 iPod Touch)에서 전화 걸기를 시도하는 경우 사용자에게 전화를 걸 수 없음을 알리는 경고 대화 상자가 표시됩니다. 아래 코드는 경고 컨트롤러를 만들고 채웁니다.

    ```csharp
    if (!UIApplication.SharedApplication.OpenUrl (url)) {
                    var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                    alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                    PresentViewController (alert, true, null);
                }
    ```

iOS 경고 보기에 대한 자세한 내용은 [경고 컨트롤러 레시피](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/)를 참조하세요.

## <a name="testing-deployment-and-finishing-touches"></a>터치 테스트, 배포 및 마무리

Mac용 Visual Studio와 Visual Studio 모두 응용 프로그램을 테스트하고 배포하기 위한 다양한 옵션을 제공합니다. 이 섹션에서는 디버깅 옵션에 대해 다루고, 장치에서 응용 프로그램 테스트하기에 대해 설명하며, 사용자 지정 앱 아이콘 및 시작 이미지를 만들기 위한 도구를 소개합니다.

### <a name="debugging-tools"></a>디버깅 도구

경우에 따라 응용 프로그램 코드의 문제는 진단하기가 어렵습니다. 복잡한 코드 문제를 진단하려면 [중단점을 설정](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/)하거나, [코드를 단계별 실행](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/step_through_code/)하거나, [로그 창에 정보를 출력](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/output_information_to_log_window/)합니다.

### <a name="deploy-to-a-device"></a>장치에 배포

iOS 시뮬레이터는 응용 프로그램을 테스트하기 위한 빠른 방법입니다. 시뮬레이터에는 모의 위치, [이동 시뮬레이션](https://developer.xamarin.com/recipes/ios/multitasking/test_location_changes_in_simulator/) 등을 포함한 테스트에 유용한 최적화가 많이 있습니다. 그러나 사용자는 시뮬레이터에서 최종 앱을 사용하지 않습니다. 모든 응용 프로그램은 조기에 그리고 자주 실제 장치에서 테스트해야 합니다.

장치를 프로비전하는 데는 시간이 걸리며 Apple 개발자 계정이 있어야 합니다. [장치 프로비전](~/ios/get-started/installation/device-provisioning/index.md) 가이드에 개발을 위한 장치 준비에 대한 철저한 지침이 나와 있습니다.

> [!NOTE]
> 현재 Apple의 요구 사항에 따라 장치 또는 시뮬레이터용 코드를 빌드하려면 개발 인증서 또는 _서명 ID_가 필요합니다. 이를 설정하려면 [장치 프로비전 가이드](~/ios/get-started/installation/device-provisioning/manual-provisioning.md)의 단계를 수행합니다.

장치가 프로비전되고 나면, 다음 스크린샷에 표시된 것처럼 플러그 인하고 빌드 도구 모음에 있는 대상을 iOS 장치로 변경한 다음, **Start**(**Play**) 키를 눌러 배포할 수 있습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image46new.png "Start/Play 키 누르기")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image46.png "Start/Play 키 누르기")

-----

앱이 iOS 장치에 배포합니다.

[![](hello-ios-deepdive-images/image1.png "앱이 iOS 장치에 배포되고 실행됩니다.")](hello-ios-deepdive-images/image1.png#lightbox)

### <a name="generate-custom-icons-and-launch-images"></a>사용자 지정 아이콘 및 시작 이미지 생성

모든 사람이 앱을 차별화하는 데 필요한 사용자 지정 아이콘 및 시작 이미지를 만들 수 있는 디자이너를 가지고 있지는 않습니다. 다음은 사용자 앱 아트워크를 생성하는 몇 가지 대안 방법입니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- [**Sketch**](https://www.sketchapp.com") – Sketch는 사용자 인터페이스, 아이콘 등을 디자인하기 위한 Mac 앱입니다. 이는 Xamarin 앱 아이콘 및 시작 이미지 집합을 디자인하는 데 사용되었던 앱입니다. Sketch 3은 App Store에서 사용할 수 있습니다. 무료 [Sketch Tool](http://bohemiancoding.com/sketch/tool/)도 사용해 볼 수 있습니다.
- [**Pixelmator**](http://www.pixelmator.com/) – Mac용 다양한 이미지 편집 앱입니다(약 $30).
- [**Glyphish**](http://www.glyphish.com/) – 무료 다운로드 및 구매가 가능한 고품질의 미리 빌드된 아이콘 집합입니다.
- [**Fiverr**](http://www.fiverr.com/) – 다양한 디자이너를 선택하여 자신에 게 맞는 아이콘 집합을 만들 수 있습니다(가격: $5부터).  성공할 수도 있고 아니면 실패할 수도 있지만, 즉시 디자인된 아이콘이 필요한 경우에 좋은 리소스입니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

* Visual Studio - IDE에서 직접 앱에 대한 간단한 아이콘 집합을 만드는 데 사용할 수 있습니다.
- [**Glyphish**](http://www.glyphish.com/) – 무료 다운로드 및 구매가 가능한 고품질의 미리 빌드된 아이콘 집합입니다.
- [**Fiverr**](http://www.fiverr.com/) – 다양한 디자이너를 선택하여 자신에 게 맞는 아이콘 집합을 만들 수 있습니다(가격: $5부터).  성공할 수도 있고 아니면 실패할 수도 있지만, 즉시 디자인된 아이콘이 필요한 경우에 좋은 리소스입니다.

-----

아이콘 및 시작 이미지 크기 및 요구 사항에 대한 자세한 내용은 [이미지 가이드 작업](~/ios/app-fundamentals/images-icons/index.md)을 참조하세요.

## <a name="summary"></a>요약

지금까지 이제 Xamarin.iOS 응용 프로그램의 구성 요소와 이를 만드는 데 사용되는 도구에 대해 확실하게 이해했습니다.
[시작하기 시리즈의 다음 자습서](~/ios/get-started/hello-ios-multiscreen/index.md)에서는 여러 화면을 처리하도록 응용 프로그램을 확장합니다. 그 과정에서 여러 화면을 처리하도록 응용 프로그램을 확장하면서 탐색 컨트롤러를 구현하고, 스토리보드 Segues에 대해 알아보며, MVC(모델, 뷰, 컨트롤러) 패턴을 소개합니다.


## <a name="related-links"></a>관련 링크

- [Hello, iOS(샘플)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS 휴먼 인터페이스 지침](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS 프로비전 포털](https://developer.apple.com/ios/manage/overview/index.action)
