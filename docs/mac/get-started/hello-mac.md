---
title: Hello, Mac
description: 이 지침에서는 첫 번째 Xamarin.Mac 앱을 만드는 단계를 안내하고, 그 과정에서 Mac용 Visual Studio, Xcode 및 Interface Builder를 포함한 개발 도구 체인을 소개합니다. 또한 코드에 UI 컨트롤을 표시하는 출선 및 작업을 소개하고, 마지막으로 Xamarin.Mac 응용 프로그램을 빌드, 실행 및 테스트하는 방법을 보여줍니다.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 37D0E9E6-979B-7069-B3BE-C5F0AF99BA72
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/23/2017
ms.openlocfilehash: e5d87d42765480c97da392cf07b6599108895321
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="hello-mac"></a>Hello, Mac

Xamarin.Mac을 사용하면 *Objective-C* 및 *Xcode*에서 개발할 때 사용되는 동일한 OS X 라이브러리 및 인터페이스 컨트롤을 사용하여 C# 및 .NET에서 완전한 네이티브 Mac 앱을 개발할 수 있습니다. Xamarin.Mac이 Xcode와 직접 통합되므로 개발자는 Xcode의 _Interface Builder_를 사용하여 앱의 사용자 인터페이스를 만들 수 있습니다(또는 필요에 따라 C# 코드에서 바로 작성).

또한 Xamarin.Mac 응용 프로그램은 C# 및 .NET으로 작성되므로 각 플랫폼에 기본 경험을 제공하면서도 공통적인 백 엔드 코드를 Xamarin.iOS 및 Xamarin.Android 모바일 앱과 공유할 수 있습니다.

이 문서에서는 단추 클릭 횟수를 계산하는 간단한 **Hello, Mac** 앱을 빌드하는 프로세스를 살펴보면서 Xamarin.Mac, Mac용 Visual Studio 및 Xcode의 Interface Builder를 사용하여 Mac 앱을 만드는 데 필요한 핵심 개념을 소개합니다.

[![](hello-mac-images/run02.png "실행 중인 Hello, Mac 앱의 예")](hello-mac-images/run02.png#lightbox)

다음 개념을 다룹니다.

-  **Mac용 Visual Studio** – Mac용 Visual Studio를 소개하고 Mac용 Visual Studio를 사용하여 Xamarin.Mac 응용 프로그램을 만드는 방법을 설명합니다.
-  **Xamarin.Mac 응용 프로그램 분석** – Xamarin.Mac 응용 프로그램의 구성 요소를 살펴봅니다.
-  **Xcode의 Interface Builder** - Xcode의 Interface Builder를 사용하여 앱의 사용자 인터페이스를 정의하는 방법을 알아봅니다.
-  **출선 및 작업** - 출선 및 작업을 사용하여 사용자 인터페이스에서 컨트롤을 연결하는 방법을 알아봅니다.
-  **배포/테스트** – Xamarin.Mac 앱을 실행하고 테스트하는 방법을 알아봅니다.


<a name="Requirements" />

## <a name="requirements"></a>요구 사항

Xamarin.Mac으로 macOS 응용 프로그램을 개발하려면 다음이 필요합니다.

- macOS Yosemite(10.10) 이상을 실행하는 Mac 컴퓨터.
- Xcode 7 이상([앱 스토어](https://itunes.apple.com/us/app/xcode/id497799835?mt=12)에서 안정적인 최신 버전을 설치할 것을 권장).
- 최신 버전의 Xamarin.Mac 및 Mac용 Visual Studio.

Xamarin.Mac으로 만든 Mac 응용 프로그램을 실행하려면 다음과 같은 시스템 요구 사항이 필요합니다.

- Mac OS X 10.7 이상을 실행하는 Mac 컴퓨터.

<a name="Starting_a_new_Xamarin.Mac_App_in_Xamarin_Studio" />

## <a name="starting-a-new-xamarinmac-app-in-visual-studio-for-mac"></a>Mac용 Visual Studio에서 새 Xamarin.Mac 앱 시작

위에서 언급했듯이, 이 가이드에서는 주 창에 단일 단추 및 레이블을 추가하는 `Hello_Mac`이라는 Mac 앱을 만드는 단계를 설명합니다. 단추를 클릭하면 레이블에 단추 클릭 횟수가 표시됩니다.

시작하려면 다음을 수행합니다.

1. Mac용 Visual Studio 시작:

    [![](hello-mac-images/setup01.png "Mac용 Visual Studio 기본 인터페이스")](hello-mac-images/setup01.png#lightbox)

2. 화면의 왼쪽 위 모서리에서 **새 솔루션...** 링크를 클릭하여 **새 프로젝트** 대화 상자를 엽니다.

    [![](hello-mac-images/setup03.png "Mac용 Visual Studio에서 새 솔루션 만들기")](hello-mac-images/setup02.png#lightbox)

3. **Mac** > **앱** > **Cocoa 앱**을 선택하고 **다음** 단추를 클릭합니다.

    [![](hello-mac-images/setup03.png "Cocoa 앱 선택")](hello-mac-images/setup03.png#lightbox)

4. **앱 이름**으로 `Hello_Mac`을 입력하고, 나머지는 기본값을 유지합니다. **다음**을 클릭합니다.

    [![](hello-mac-images/setup05.png "앱 이름 설정")](hello-mac-images/setup05.png#lightbox)

4. 여러 다양한 프로젝트를 저장하는 솔루션을 만들 때 개발자는 다른 **솔루션 이름**을 설정하고 싶겠지만, 여기서는 이 예제의 목적 달성을 위해 기본값인 **Project Name**으로 둡니다.

    [![](hello-mac-images/setup04.png "새 솔루션 세부 정보 확인")](hello-mac-images/setup04.png#lightbox)

5. **만들기** 단추를 클릭합니다.

Mac용 visual Studio가 새 Xamarin.Mac 앱을 만들고 앱의 솔루션에 추가되는 기본 파일을 표시합니다.

 [![](hello-mac-images/project01.png "새 솔루션 기본 보기")](hello-mac-images/project01.png#lightbox)

Mac용 Visual Studio는 **솔루션** 및 **프로젝트**를 Visual Studio와 똑같은 방식으로 사용합니다. 솔루션은 하나 이상의 프로젝트를 보관할 수 있는 컨테이너이고, 프로젝트는 응용 프로그램, 지원 라이브러리, 테스트 응용 프로그램 등을 포함할 수 있습니다. 여기서는 Mac용 Visual Studio가 솔루션과 응용 프로그램 프로젝트를 자동으로 만들었습니다.

원하는 경우 개발자는 일반적인 공유 코드를 포함하는 하나 이상의 코드 라이브러리 프로젝트를 만들 수 있습니다. 이러한 라이브러리 프로젝트는 표준.NET 응용 프로그램과 동일한 방식으로 앱의 프로젝트에서 사용하거나 다른 Xamarin.Mac 앱 프로젝트(또는 코드 형식에 따라 Xamarin.iOS 및 Xamarin.Android)와 공유할 수 있습니다.

<a name="The_Project" />

## <a name="anatomy-of-a-xamarinmac-application"></a>Xamarin.Mac 응용 프로그램 분석

iOS 프로그래밍에 익숙한 경우 수많은 유사성을 발견할 수 있습니다. 사실, iOS는 Mac에서 사용하는 Cocoa를 경량화한 버전인 CocoaTouch 프레임워크를 사용하므로 수많은 개념이 서로 겹칩니다.

프로젝트의 파일을 살펴보세요.

-   `Main.cs` - 앱의 진입점을 포함합니다. 앱이 실행되면 첫 번째 클래스와 실행되는 메서드를 포함합니다.
-   `AppDelegate.cs` – 이 파일은 운영 체제의 이벤트를 수신하는 일을 담당하는 기본 앱 클래스를 포함합니다.
-   `Info.plist` – 이 파일은 응용 프로그램 이름, 아이콘 등의 앱 속성을 포함합니다.
-   `Entitlements.plist` - 이 파일은 앱에 대한 권리를 포함하며 샌드박싱 및 iCloud 지원 같은 것들에 대한 액세스를 허용합니다.
-  `Main.storyboard` – 앱의 사용자 인터페이스(Windows 및 Menus)를 정의하고 Segues를 통해 Windows 간 상호 연결을 배치합니다. 스토리보드는 보기의 정의(사용자 인터페이스 요소)를 포함하는 XML 파일입니다. 이 파일은 Xcode의 Interface Builder 내에서 만들고 유지 관리할 수 있습니다.
-   `ViewController.cs` – 주 창에 대한 컨트롤러입니다. 컨트롤러에 대한 자세한 내용은 다른 문서에서 다루기로 하고, 지금은 특정 보기의 주 엔진이라고 생각하시면 됩니다.
-   `ViewController.designer.cs` – 이 파일은 기본 화면의 사용자 인터페이스와 통합하도록 도와주는 배관 코드를 포함합니다.

다음 섹션에서는 이러한 파일 중 일부를 간략하게 살펴보겠습니다. 자세한 내용은 나중에 살펴보기로 하고, 여기서는 기본 사항을 알아보겠습니다.

<a name="Main_cs" />

### <a name="maincs"></a>Main.cs

**Main.cs** 파일은 매우 간단합니다. 새 Xamarin.Mac 앱 인스턴스를 만들고 OS 이벤트를 처리할 클래스(여기서는 `AppDelegate` 클래스) 이름을 전달하는 정적 `Main` 메서드를 포함합니다.

```csharp
using System;
using System.Drawing;
using Foundation;
using AppKit;
using ObjCRuntime;

namespace Hello_Mac
{
        class MainClass
        {
                static void Main (string[] args)
                {
                        NSApplication.Init ();
                        NSApplication.Main (args);
                }
        }
}
```

<a name="AppDelegate_cs" />

### <a name="appdelegatecs"></a>AppDelegate.cs

`AppDelegate.cs` 파일은 창을 만들고 OS 이벤트를 수신하는 역할을 담당하는 `AppDelegate` 클래스를 포함합니다.

```csharp
using AppKit;
using Foundation;

namespace Hello_Mac
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        public AppDelegate ()
        {
        }

        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
    }
}
```

이전에 iOS 앱을 만들어 본 경험이 없는 개발자들에게는 이 코드가 낯설겠지만, 매우 간단한 코드입니다.

`DidFinishLaunching` 메서드는 앱이 인스턴스화된 후 실행되며, 실제로 앱의 창을 만들고 그 안에 보기를 표시하는 프로세스를 시작하는 역할을 담당합니다.

사용자 또는 시스템에서 앱의 종료를 인스턴스화할 때 `WillTerminate` 메서드가 호출됩니다. 개발자는 앱이 종료되기 전에 이 메서드를 사용하여 앱을 완료해야 합니다(예: 사용자 기본 설정 또는 창 크기와 위치를 저장).

<a name="ViewController_cs" />

### <a name="viewcontrollercs"></a>ViewController.cs

Cocoa(및 Cocoa에서 파생된 CocoaTouch)는 *MVC(모델 보기 컨트롤러)* 패턴을 사용합니다. `ViewController` 선언은 실제 앱 창의 개체 컨트롤을 나타냅니다. 일반적으로 생성되는 모든 창(그리고 창 내부의 여러 것들)에는 창을 표시하고, 창에 새 보기(컨트롤)를 추가하는 등 창 수명 주기를 담당하는 컨트롤러가 있습니다.

`ViewController` 클래스는 주 창의 컨트롤러입니다. 즉, 주 창의 수명 주기를 관리합니다. 자세한 내용은 나중에 알아보기로 하고, 지금은 간략하게 살펴보겠습니다.

```csharp
using System;

using AppKit;
using Foundation;

namespace Hello_Mac
{
    public partial class ViewController : NSViewController
    {
        public ViewController (IntPtr handle) : base (handle)
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any additional setup after loading the view.
        }

        public override NSObject RepresentedObject {
            get {
                return base.RepresentedObject;
            }
            set {
                base.RepresentedObject = value;
                // Update the view, if already loaded.
            }
        }
    }
}
```

<a name="ViewController_Designer_cs" />

### <a name="viewcontrollerdesignercs"></a>ViewController.Designer.cs

주 창 클래스의 디자이너 파일이 지금은 비어 있지만, Xcode 내부에서 Interface Builder를 사용하여 사용자 인터페이스를 만들면 Mac용 Visual Studio가 자동으로 디자이너 파일을 채웁니다.

```csharp
// WARNING
//
// This file has been generated automatically by Visual Studio for Mac to store outlets and
// actions made in the UI designer. If it is removed, they will be lost.
// Manual changes to this file may not be handled correctly.
//
using Foundation;

namespace Hello_Mac
{
    [Register ("ViewController")]
    partial class ViewController
    {
        void ReleaseDesignerOutlets ()
        {
        }
    }
}
```

Mac용 Visual Studio가 자동으로 디자이너 파일을 관리하고 앱의 창 또는 보기에 추가된 컨트롤에 대한 액세스를 허용하는 필수 배관 코드를 제공하므로, 개발자는 일반적으로 디자이너 파일에 대해 신경을 쓰지 않아도 됩니다.

Xamarin.Mac 앱 프로젝트를 만들고 구성 요소에 대한 기본적인 내용을 알아보았으니, Xcode로 넘어가서 Interface Builder를 사용하여 사용자 인터페이스를 만들겠습니다.

<a name="Info_plist" />

### <a name="infoplist"></a>Info.plist

`Info.plist` 파일은 **이름**, **번들 식별자** 등 Xamarin.Mac 앱에 대한 정보를 포함합니다.

[![](hello-mac-images/infoplist01.png "Mac용 Visual Studio plist 편집기")](hello-mac-images/infoplist01.png#lightbox)

또한 **주 인터페이스** 드롭다운 아래에 Xamarin.Mac 앱의 사용자 인터페이스를 표시하는 데 사용되는 _스토리보드_를 정의합니다. 위의 예에서 드롭다운의 `Main`은 **솔루션 탐색기**의 프로젝트 소스 트리에 있는 `Main.storyboard`와 관련되어 있습니다. 또한 아이콘(여기서는 AppIcons)을 포함하는 *자산 카탈로그*를 지정하여 앱의 아이콘을 정의합니다.

### <a name="entitlementsplist"></a>Entitlements.plist

앱의 `Entitlements.plist` 파일은 Xamarin.Mac 앱이 보유한 **샌드박싱** 및 **iCloud** 같은 자격을 제어합니다.

[![](hello-mac-images/entitlements01.png "Mac용 Visual Studio 자격 편집기")](hello-mac-images/entitlements01.png#lightbox)

Hello World 예제에서는 자격이 필요 없습니다. 다음 섹션에서는 Xcode의 Interface Builder를 사용하여 `Main.storyboard` 파일을 편집하고 Xamarin.Mac 앱의 UI를 정의하는 방법을 보여줍니다.

<a name="Introduction_to_Xcode_and_Interface_Builder" />

## <a name="introduction-to-xcode-and-interface-builder"></a>Xcode 및 Interface Builder 소개

Apple에서는 Xcode의 일부로 개발자가 디자이너에서 시각적으로 사용자 인터페이스를 만들 수 있는 Interface Builder라는 도구를 만들었습니다. Xamarin.Mac은 Interface Builder와 자연스럽게 통합되어 Objective-C 사용자와 동일한 도구를 사용하여 UI를 만들 수 있게 해줍니다.

시작하려면 Xcode 및 Interface Builder에서 파일을 편집할 수 있도록 **솔루션 탐색기**에서 `Main.storyboard` 파일을 두 번 클릭하여 엽니다.

[![](hello-mac-images/xcode01.png "솔루션 탐색기의 Main.storyboard 파일")](hello-mac-images/xcode01.png#lightbox)

그러면 Xcode가 실행되고 다음과 같이 표시됩니다.

[![](hello-mac-images/xcode02.png "기본 Xcode Interface Builder 보기")](hello-mac-images/xcode02.png#lightbox)

인터페이스 디자인을 시작하기 전에, Xcode를 간략하게 살펴보면서 우리가 사용할 주요 기능을 알아보겠습니다.

> [!NOTE]
> 개발자는 Xamarin.Mac 앱에 대한 사용자 인터페이스를 만들기 위해 반드시 Xcode와 Interface Builder를 사용해야 하는 것은 아니고, C# 코드에서 직접 UI를 만들 수도 있습니다. 그러나 그 방법은 본 문서의 범위를 벗어납니다. 간단한 설명을 위해 이 자습서의 나머지 부분에서는 Interface Builder를 사용하여 사용자 인터페이스를 만들겠습니다.


<a name="Components_of_Xcode" />

### <a name="components-of-xcode"></a>Xcode 구성 요소

Mac용 Visual Studio에서 Xcode로 `.storyboard` 파일을 열면 왼쪽에는 **프로젝트 탐색기**, 가운데에는 **인터페이스 계층 구조** 및 **인터페이스 편집기**, 오른쪽에는 **속성 및 유틸리티** 섹션이 표시됩니다.

[![](hello-mac-images/xcode03.png "Xcode에서 Interface Builder의 다양한 섹션")](hello-mac-images/xcode03.png#lightbox)

다음 섹션에서는 각 Xcode 기능이 하는 일 및 각 기능을 사용하여 Xamarin.Mac 앱의 인터페이스를 만드는 방법을 살펴보겠습니다.

<a name="Project_Navigation" />

### <a name="project-navigation"></a>프로젝트 탐색

Xcode에서 편집하기 위해 `.storyboard` 파일을 열면 Mac용 Visual Studio는 Xcode와 변경 내용을 전달하기 위해 백그라운드에서 *Xcode 프로젝트 파일*을 만듭니다. 나중에 개발자가 Xcode에서 Mac용 Visual Studio로 전환하면 이 프로젝트에서 변경된 내용이 Mac용 Visual Studio에 의해 Xamarin.Mac 프로젝트와 동기화됩니다.

**프로젝트 탐색** 섹션에서 개발자는 이 _shim_ Xcode 프로젝트를 구성하는 모든 파일을 탐색할 수 있습니다. 일반적으로 개발자들은 `Main.storyboard`처럼 이 목록의 `.storyboard` 파일에만 관심이 있습니다.

<a name="Interface_Hierarchy" />

### <a name="interface-hierarchy"></a>인터페이스 계층 구조

**인터페이스 계층 구조** 섹션에서 개발자는 **자리 표시자** 및 주 **창** 같은 사용자 인터페이스의 여러 가지 주요 속성에 쉽게 액세스할 수 합니다. 이 섹션에서는 사용자 인터페이스를 구성하는 개별 요소(보기)에 액세스하고 계층 구조 내에서 요소를 끌어서 중첩하는 방법을 조정할 수 있습니다.

<a name="Interface_Editor" />

### <a name="interface-editor"></a>인터페이스 편집기

**인터페이스 편집기** 섹션에서는 사용자 인터페이스를 그래픽으로 배치하는 화면을 제공합니다. **속성 및 유틸리티** 섹션의 **라이브러리** 섹션에서 요소를 끌어 설계를 작성합니다. 사용자 인터페이스 요소(보기)가 디자인 화면에 추가되면 **인터페이스 편집기**에 나타나는 순서대로 **인터페이스 계층 구조** 섹션에 추가됩니다.

<a name="Properties_Utilities" />

### <a name="properties--utilities"></a>속성 및 유틸리티

**속성 및 유틸리티** 섹션은 크게 **속성**(검사기라고도 함) 및 **라이브러리**의 두 섹션으로 나뉩니다.

[![](hello-mac-images/xcode04.png "속성 검사기")](hello-mac-images/xcode04.png#lightbox)

처음에는 이 섹션이 거의 비어 있지만, 개발자가 **인터페이스 편집기** 또는 **인터페이스 계층 구조**에서 요소를 선택하면 **속성** 섹션은 조정 가능한 특정 요소 및 속성에 대한 정보로 채워집니다.

**속성** 섹션 내에는 다음 그림처럼 8개의 *검사기 탭*이 있습니다.

[![](hello-mac-images/xcode05.png "모든 검사기 개요")](hello-mac-images/xcode05.png#lightbox)

<a name="Properties_Utility_Types" />

### <a name="properties--utility-types"></a>속성 및 유틸리티 유형

왼쪽부터 시작해서 오른쪽으로 가면서 다음과 같은 탭이 있습니다.

-   **파일 검사기** – 파일 검사기는 파일 이름, 편집 중인 Xib 파일의 위치와 같은 파일 정보를 보여줍니다.
-   **빠른 도움말** – 빠른 도움말 탭에서는 현재 Xcode에서 선택한 항목에 따라 상황에 맞는 도움말을 제공합니다.
-   **ID 검사기** – ID 검사기는 선택한 컨트롤/보기에 대한 정보를 제공합니다.
-   **특성 검사기** – 개발자는 특성 검사기를 사용하여 선택한 컨트롤/보기의 다양한 특성을 사용자 지정할 수 있습니다.
-   **크기 검사기** – 개발자는 크기 검사기를 사용하여 선택한 컨트롤/보기의 크기 및 크기 조정 동작을 제어할 수 있습니다.
-   **연결 검사기** – 연결 검사기는 선택한 컨트롤의 **출선** 및 **작업** 연결을 보여줍니다. 출선 및 작업은 아래에서 자세히 설명하겠습니다.
-   **바인딩 검사기** – 개발자는 바인딩 검사기를 사용하여 컨트롤 값이 자동으로 데이터 모델에 바인딩되도록 컨트롤을 구성할 수 있습니다.
-   **보기 효과 검사기** – 개발자는 보기 효과 검사기를 사용하여 컨트롤에 대해 애니메이션 같은 효과를 지정할 수 있습니다.

**라이브러리** 섹션을 사용하여 디자이너에 배치할 컨트롤 및 개체를 찾아 그래픽으로 사용자 인터페이스를 빌드할 수 있습니다.

[![](hello-mac-images/xcode06.png "Xcode 라이브러리 검사기")](hello-mac-images/xcode06.png#lightbox)

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>인터페이스 만들기

Xcode IDE 및 Interface Builder의 기본 사항에 대해 배웠으니, 개발자는 주 보기에 대한 사용자 인터페이스를 만들 수 있습니다.

다음을 수행합니다.

1. Xcode의 **라이브러리 섹션**에서 **누름 단추**를 끕니다.

    [![](hello-mac-images/xcode07.png "라이브러리 검사기에서 NSButton 선택")](hello-mac-images/xcode07.png#lightbox)

2. **인터페이스 편집기**에서 **보기**(**창 컨트롤러** 아래)에 단추를 놓습니다.

    [![](hello-mac-images/xcode08.png "인터페이스 디자인에 단추 추가")](hello-mac-images/xcode08.png#lightbox)

3. **특성 검사기**에서 **제목** 속성을 클릭하고 단추 제목을 `Click Me`로 변경합니다.

    [![](hello-mac-images/xcode09.png "단추의 속성 설정")](hello-mac-images/xcode09.png#lightbox)

4. **라이브러리 섹션**에서 **레이블**을 끕니다.

    [![](hello-mac-images/xcode10.png "라이브러리 검사기에서 레이블 선택")](hello-mac-images/xcode10.png#lightbox)

5. **인터페이스 편집기**에서 단추 옆에 있는 **창**에 레이블을 놓습니다.

    [![](hello-mac-images/xcode11.png "인터페이스 디자인에 레이블 추가")](hello-mac-images/xcode11.png#lightbox)

6. 레이블의 오른쪽 핸들을 잡고 창의 가장자리 근처까지 끕니다.

    [![](hello-mac-images/xcode12.png "레이블 크기 조정")](hello-mac-images/xcode12.png#lightbox)

7. **인터페이스 편집기**에서 방금 추가한 단추를 선택하고 창의 아래쪽에서 **제약 조건 편집기** 아이콘을 클릭합니다.

    [![](hello-mac-images/xcode13.png "단추에 제약 조건 추가")](hello-mac-images/xcode13.png#lightbox)

8. 편집기 상단에서 위쪽과 왼쪽의 **빨간색 I-빔**을 클릭합니다. 이렇게 하면 창 크기를 조정할 때 단추가 화면의 왼쪽 모서리에서 같은 위치를 유지합니다.

9. 다음으로 **높이** 및 **너비** 상자를 선택하고 기본 크기를 사용합니다. 이렇게 하면 창 크기를 조정할 때 단추가 같은 크기로 유지됩니다.

10. **4개 제약 조건 추가** 단추를 클릭하여 제약 조건을 추가하고 편집기를 닫습니다.

11. 레이블을 선택하고 **제약 조건 편집기** 아이콘을 다시 클릭합니다.

    [![](hello-mac-images/xcode14.png "레이블에 제약 조건 추가")](hello-mac-images/xcode14.png#lightbox)

12. **제약 조건 편집기**의 위쪽, 오른쪽 및 왼쪽에서 **빨간색 I-빔**을 클릭하면 실행 중인 응용 프로그램에서 창 크기를 조정할 때 레이블의 위치는 지정된 X 및 Y로 고정되고 크기는 확장 및 축소됩니다.

13. 다시 한 번 **높이** 상자를 선택하고 기본 크기를 사용한 다음, **4개 제약 조건 추가** 단추를 클릭하여 제약 조건을 추가하고 편집기를 닫습니다.

14. 변경 내용을 사용자 인터페이스에 저장합니다.

컨트롤 크기를 조정하고 이동할 때 Interface Builder는 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)에 따라 유용한 스냅 힌트를 제공합니다. 이러한 지침은 Mac 사용자에게 친숙한 모양과 느낌을 주는 고품질 앱을 만들려는 개발자에게 도움이 됩니다.

**인터페이스 계층 구조** 섹션에서 사용자 인터페이스를 구성하는 요소의 레이아웃과 계층 구조가 어떤 모양인지 살펴볼 수 있습니다.

[![](hello-mac-images/xcode15.png "인터페이스 계층 구조에서 요소 선택")](hello-mac-images/xcode15.png#lightbox)

여기서 개발자는 편집하거나 끌 항목을 선택하여 필요한 경우 UI 요소를 다시 정렬할 수 있습니다. 예를 들어 UI 요소를 다른 요소가 가리는 경우 목록 맨 아래로 끌어서 창의 최상위 항목으로 만들 수 있습니다.

사용자 인터페이스가 생성되면 개발자는 Xamarin.Mac이 C# 코드로 UI 항목에 액세스하고 상호 작용할 수 있도록 UI 항목을 노출해야 합니다. 그 방법은 다음 섹션인 **출선 및 작업**에서 설명하겠습니다.

<a name="Outlets_and_Actions" />

### <a name="outlets-and-actions"></a>출선 및 작업

**출선** 및 **작업**이란? 전통적인 .NET 사용자 인터페이스 프로그래밍에서, 사용자 인터페이스의 컨트롤은 추가될 때 자동으로 속성으로 노출됩니다. Mac에서는 동작이 약간 다릅니다. 보기에 컨트롤을 추가하기만 해서는 코드에 액세스할 수 없습니다. 개발자가 UI 요소를 코드에 명시적으로 노출해야 합니다. 이 작업을 위해 Apple에서 두 가지 옵션을 제공합니다.

-   **출선** – 출선은 속성과 비슷합니다. 개발자가 컨트롤을 출선에 연결하면 출선은 속성을 통해 코드에 노출되고, 따라서 이벤트 처리기를 연결하고 메서드를 호출하는 등의 작업을 할 수 있습니다.
-   **작업** – 작업은 WPF의 명령 패턴과 비슷합니다. 예를 들어 컨트롤에서 단추 클릭 작업을 수행하면 컨트롤이 코드에서 메서드를 자동으로 호출합니다. 작업은 개발자가 여러 컨트롤을 같은 작업에 연결할 수 있기 때문에 강력하고 편리합니다.

Xcode에서 **출선** 및 **작업**은 *컨트롤 끌기*를 통해 코드에서 직접 추가됩니다. 보다 구체적으로 말해서, 개발자는 **출선** 또는 **작업**을 만들기 위해 개발자는 **출선** 또는 **작업**을 추가할 컨트롤 요소를 선택하고, 키보드에서 **Control** 키를 누른 상태로 해당 컨트롤을 코드로 직접 끕니다.

Xamarin.Mac 개발자의 경우 **출선** 또는 **작업**을 만들려는 C# 파일에 해당하는 Objective-C 스텁 파일로 끕니다. Mac용 Visual Studio는 Interface Builder를 사용하기 위해 생성한 shim Xcode 프로젝트의 일부로 `ViewController.h`라는 파일을 만들었습니다.

[![](hello-mac-images/xcode16.png "Xcode에서 원본 보기")](hello-mac-images/xcode16.png#lightbox)

이 스텁 `.h` 파일은 새 `NSWindow`가 생성될 때 Xamarin.Mac 프로젝트에 자동으로 추가되는 `ViewController.designer.cs`를 미러링합니다. 이 파일은 Interface Builder에서 변경한 내용을 동기화하는 데 사용되며, UI 요소가 C# 코드에 노출되도록 **출선** 및 **작업**이 생성되는 위치입니다.

<a name="Adding_an_Outlet" />

#### <a name="adding-an-outlet"></a>출선 추가

**출선** 및 **작업**에 대한 기본적인 내용을 이해했으니, 앞에서 만든 레이블을 C# 코드에 노출하는 **출선**을 만들겠습니다.

다음을 수행합니다.

1. 화면의 오른쪽 맨 위 모서리에 있는 Xcode에서 **이중 원** 단추를 클릭하여 **도우미 편집기**를 엽니다.

    [![](hello-mac-images/outlet01.png "도우미 편집기 표시")](hello-mac-images/outlet01.png#lightbox)

2. Xcode가 분할 보기 모드로 전환되어 한 쪽에는 **인터페이스 편집기**가, 다른 쪽에는 **코드 편집기** 표시됩니다.

3. Xcode가 **코드 편집기**에서 잘못된 **ViewController.m** 파일을 자동으로 선택했습니다. 위에서 **출선** 및 **작업**에 대해 살펴볼 때, 개발자는 **ViewController.h**를 선택해야 합니다.

4. **코드 편집기** 위쪽에서 **자동 링크**를 클릭하고 `ViewController.h` 파일을 선택합니다.

    [![](hello-mac-images/outlet02.png "올바른 파일 선택")](hello-mac-images/outlet02.png#lightbox)

5. Xcode가 이제 올바른 파일을 선택했습니다.

    [![](hello-mac-images/outlet03.png "ViewController.h 파일 보기")](hello-mac-images/outlet03.png#lightbox)

6. **마지막 단계는 매우 중요합니다!** 개발자가 올바른 파일을 선택하지 않으면 **출선** 및 **작업**을 만들 수 없거나 C#에서 잘못된 클래스에 노출됩니다.

7. **인터페이스 편집기**에서 키보드의 **Control** 키를 누른 채로 위에서 만든 레이블을 클릭하여 `@interface ViewController : NSViewController {}` 코드 바로 아래에 있는 코드 편집기 위로 끕니다.

    [![](hello-mac-images/outlet04.png "끌어서 아웃렛 만들기")](hello-mac-images/outlet04.png#lightbox)

8. 대화 상자가 표시됩니다. **연결**은 **출선**으로 두고 **이름**으로 `ClickedLabel`을 입력합니다.

    [![](hello-mac-images/outlet05.png "아웃렛 정의")](hello-mac-images/outlet05.png#lightbox)

9. **연결** 단추를 클릭하여 **출선**을 만듭니다.

    [![](hello-mac-images/outlet06.png "최종 아웃렛 보기")](hello-mac-images/outlet06.png#lightbox)

10. 파일의 변경 내용을 저장합니다.

<a name="Adding_an_Action" />

#### <a name="adding-an-action"></a>작업 추가

다음으로, 단추를 C# 코드에 노출합니다. 위의 레이블과 마찬가지로, 개발자는 단추를 **출선**에 연결할 수 있습니다. 우리는 클릭되는 단추에만 응답할 것이므로 **작업**을 대신 사용합니다.

다음을 수행합니다.

1. Xcode가 여전히 **도우미 편집기**에 있고 **코드 편집기**에서 **ViewController.h** 파일이 보이는지 확인합니다.
2. **인터페이스 편집기**에서 키보드의 **Control** 키를 누른 채로 위에서 만든 단추를 클릭하여 `@property (assign) IBOutlet NSTextField *ClickedLabel;` 코드 바로 아래에 있는 코드 편집기 위로 끕니다.

    [![](hello-mac-images/action01.png "끌어서 작업 만들기")](hello-mac-images/action01.png#lightbox)

3. **연결** 형식을 **작업**으로 변경합니다.

    [![](hello-mac-images/action02.png "작업 정의")](hello-mac-images/action02.png#lightbox)

4. **이름**으로 `ClickedButton`을 입력합니다.

    [![](hello-mac-images/action03.png "새 작업의 이름 지정")](hello-mac-images/action03.png#lightbox)

5. **연결** 단추를 클릭하여 **작업**을 만듭니다.

    [![](hello-mac-images/action04.png "최종 작업 보기")](hello-mac-images/action04.png#lightbox)

6. 파일의 변경 내용을 저장합니다.

사용자 인터페이스를 연결하고 C# 코드에 노출했으니, Mac용 Visual Studio로 돌아가서 Xcode 및 Interface Builder에서 변경된 내용이 동기화되기를 기다립니다.

> [!NOTE]
> 첫 번째 앱에서는 사용자 인터페이스와 **출선** 및 **작업**을 만드는 데 시간이 오래 걸렸을 것이며 할 일이 많은 것처럼 보일 것입니다. 하지만 여러 가지 새로운 개념이 도입되었고 새로운 분야를 개척하기 위해 많은 시간이 소요되었습니다. Interface Builder를 조금만 연습하면 이 인터페이스와 모든 **출선** 및 **작업**을 1~2분이면 만들 수 있습니다.

<a name="Synchronizing_Changes_with_Xcode" />

### <a name="synchronizing-changes-with-xcode"></a>Xcode와 변경 내용 동기화

개발자가 Xcode에서 Mac용 Visual Studio로 전환하면 Xcode에서 변경된 내용이 자동으로 Xamarin.Mac 프로젝트와 동기화됩니다.

**솔루션 탐색기**에서 **ViewController.designer.cs**를 선택하여 C# 코드에서 **출선** 및 **작업**이 연결되는 방식을 살펴봅니다.

[![](hello-mac-images/sync01.png "Xcode와 변경 내용 동기화")](hello-mac-images/sync01.png#lightbox)

**ViewController.designer.cs** 파일의 두 정의를 살펴봅니다.

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

Xcode에서 `ViewController.h` 파일의 정의와 일치시킵니다.

```csharp
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

Mac용 Visual Studio는 **.h** 파일의 변경 내용을 수신 대기하면서 각 **.designer.cs** 파일의 변경 내용을 자동으로 동기화하여 앱에 노출합니다. **ViewController.designer.cs**는 partial 클래스이며, 따라서 Mac용 Visual Studio는 개발자가 클래스에서 변경한 모든 변경 내용을 덮어쓰는 **ViewController.cs**를 수정할 필요가 없습니다.

일반적으로 개발자는 **ViewController.designer.cs** 파일을 열어볼 일이 없으며, 여기서는 교육을 목적으로 보여드린 것입니다.

> [!NOTE]
> 대부분의 경우 Mac용 Visual Studio는 Xcode에서 변경된 내용을 자동으로 확인하여 Xamarin.Mac 프로젝트와 동기화합니다. 동기화가 자동으로 수행되지 않는 경우 Xcode로 돌아가서 다시 Mac용 Visual Studio로 돌아갑니다. 대부분 이렇게 하면 동기화 주기가 시작됩니다.

<a name="Writing_the_Code" />

## <a name="writing-the-code"></a>코드 작성

**출선** 및 **작업**을 통해 사용자 인터페이스를 만들고 UI 요소를 코드에 노출했으니, 마지막으로 프로그램을 실행할 코드를 작성해야 합니다.

이 샘플 앱에서 첫 번째 단추를 클릭할 때마다 단추 클릭 횟수를 표시하도록 레이블이 업데이트됩니다. 이 작업을 수행하려면 `ViewController.cs` 파일을 편집할 수 있도록 **솔루션 탐색기**에서 이 파일을 두 번 클릭하여 엽니다.

[![](hello-mac-images/code01.png "Mac용 Visual Studio에서 ViewController.cs 파일 보기")](hello-mac-images/code01.png#lightbox)

먼저 `ViewController` 클래스에서 발생한 클릭 횟수를 추적하는 클래스 수준 변수를 만듭니다. 클래스 정의를 편집하고 다음과 비슷하게 만듭니다.

```csharp
namespace Hello_Mac
{
    public partial class ViewController : NSViewController
    {
        private int numberOfTimesClicked = 0;
        ...
```

다음으로, 동일한 클래스(`ViewController`)에서 `ViewDidLoad` 메서드를 재정의하고 레이블의 초기 메시지를 설정하는 일부 코드를 추가합니다.

```csharp
public override void ViewDidLoad ()
{
    base.AwakeFromNib ();

    // Set the initial value for the label
    ClickedLabel.StringValue = "Button has not been clicked yet.";
}
```

`Initialize` 같은 다른 메서드 대신 `ViewDidLoad`를 사용합니다. 왜냐하면 `ViewDidLoad`는 OS가 **.storyboard** 파일에서 사용자 인터페이스를 로드하여 인스턴스화한 *후* 호출되기 때문입니다. **.storyboard** 파일이 완전히 로드되어 인스턴스화되기 전에 개발자가 레이블 컨트롤에 액세스하려고 시도하면 레이블 컨트롤이 아직 존재하지 않아 `NullReferenceException` 오류가 발생합니다.

다음으로 단추를 클릭하는 사용자에게 응답하는 코드를 추가합니다. 다음 partial 메서드를 `ViewController` 클래스에 추가합니다.

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

이 코드는 Xcode 및 Interface Builder에서 만든 **작업**에 연결되며 사용자가 단추를 클릭할 때마다 호출됩니다.

<a name="Testing_the_Application" />

## <a name="testing-the-application"></a>응용 프로그램 테스트

앱을 빌드하고 실행하여 앱이 예상대로 실행되는지 확인할 시간입니다. 개발자는 한 단계서 앱을 빌드하고 실행할 수도 있고, 앱을 빌드하지만 실행하지는 않을 수 있습니다.

앱을 빌드할 때마다 개발자는 원하는 빌드 종류를 선택할 수 있습니다.

-   **디버그** - 디버그 빌드는 **.app**(응용 프로그램) 파일로 컴파일되고 개발자가 앱이 실행되는 동안 발생하는 문제를 디버그할 수 있는 여러 가지 추가 메타데이터를 제공합니다.
-   **릴리스** – 릴리스 빌드도 **.app** 파일을 만들지만 디버그 정보를 포함하지 않으므로 크기가 작고 실행 속도가 빠릅니다.

개발자는 Mac용 Visual Studio 화면의 왼쪽 위 모서리에 있는 **구성 선택기**에서 빌드 유형을 선택할 수 있습니다.

[![](hello-mac-images/run01.png "디버그 빌드 선택")](hello-mac-images/run01.png#lightbox)

<a name="Building_the_Application" />

## <a name="building-the-application"></a>응용 프로그램 빌드

이 예제의 경우 디버그 빌드를 사용할 예정이므로 **디버그**를 선택합니다. **⌘B**를 누르거나 **빌드** 메뉴에서 **모두 빌드**를 선택하여 우선 앱을 빌드합니다.

오류가 없으면  Mac용 Visual Studio의 상태 표시줄에 **빌드 성공** 메시지가 표시됩니다. 오류가 있으면 프로젝트를 검토하여 위의 단계를 올바르게 수행되었는지 확인합니다. 먼저 코드(Xcode의 코드와 Mac용 Visual Studio의 코드 모두)가 자습서의 코드와 일치하는지 확인합니다.

<a name="Running_the_Application" />

## <a name="running-the-application"></a>응용 프로그램 실행

앱을 실행하는 세 가지 방법이 있습니다.

-  **⌘ + Enter**를 누릅니다.
-  **실행** 메뉴에서 **디버그**를 선택합니다.
-  Mac용 Visual Studio 도구 모음에서 **재생** 단추를 클릭합니다(**솔루션 탐색기** 바로 위에 있음).

앱이 빌드되고(아직 빌드되지 않은 경우), 디버그 모드에서 시작되고, 주 인터페이스 창을 표시합니다.

[![](hello-mac-images/run02.png "응용 프로그램 실행")](hello-mac-images/run02.png#lightbox)

단추를 몇 번 클릭하면 그 횟수에 따라 레이블이 업데이트됩니다.

[![](hello-mac-images/run03.png "단추 클릭 결과 표시")](hello-mac-images/run03.png#lightbox)

<a name="Where_to_Next" />

## <a name="where-to-next"></a>다음 위치

Xamarin.Mac 응용 프로그램을 작업하기 위한 기본 개념을 살펴보았으니, 다음 문서를 통해 자세한 내용을 알아보겠습니다.

- [스토리보드 소개](~/mac/platform/storyboards/index.md) - 이 문서에서는 Xamarin.Mac 앱에서 스토리보드를 작업하는 방법을 소개합니다. 스토리보드와 Xcode의 Interface Builder를 사용하여 앱의 UI를 만들고 유지 관리하는 내용을 다룹니다.
- [창](~/mac/user-interface/window.md) - 이 문서에서는 Xamarin.Mac 응용 프로그램에서 창 및 패널을 작업하는 내용을 다룹니다. Xcode 및 Interface Builder에서 창과 패널을 생성 및 유지 관리하고, .xib 파일에서 창과 패널을 로드하고, C# 코드에서 창을 사용하고 창에 응답하는 내용을 다룹니다.
- [대화 상자](~/mac/user-interface/dialog.md) - 이 문서에서는 Xamarin.Mac 응용 프로그램에서 대화 상자 및 모달 창을 작업하는 내용을 다룹니다. Xcode 및 Interface Builder에서 모달 창을 생성 및 유지 관리하고, 표준 대화 상자를 작업하고, C# 코드에서 창을 표시하고 응답하는 내용을 다룹니다.
- [경고](~/mac/user-interface/alert.md) - 이 문서에서는 Xamarin.Mac 응용 프로그램에서 경고를 작업하는 내용을 다룹니다. C# 코드에서 경고를 생성 및 표시하고 경고에 응답하는 내용을 다룹니다.
- [메뉴](~/mac/user-interface/menu.md) - 메뉴는 화면 위쪽의 주 메뉴부터 창의 아무 곳에나 나타날 수 있는 팝업 및 상황에 맞는 메뉴까지, Mac 응용 프로그램 사용자 인터페이스의 여러 부분에 사용됩니다. 메뉴는 Mac 응용 프로그램의 사용자 경험에서 필수 요소입니다. 이 문서에서는 Xamarin.Mac 응용 프로그램에서 Cocoa 메뉴를 작업하는 내용을 다룹니다.
- [도구 모음](~/mac/user-interface/toolbar.md) - 이 문서에서는 Xamarin.Mac 응용 프로그램에서 도구 모음을 작업하는 내용을 다룹니다. Xcode 및 Interface Builder에서 도구 모음을 생성 및 유지 관리하고, 출선 및 작업을 사용하여 도구 모음 항목을 코드에 노출하고, 도구 모음을 설정 및 해제하고, 마지막으로 C# 코드에서 도구 모음 항목에 응답하는 내용을 다룹니다.
- [테이블 보기](~/mac/user-interface/table-view.md) - 이 문서에서는 Xamarin.Mac 응용 프로그램에서 테이블 보기를 작업하는 내용을 다룹니다. Xcode 및 Interface Builder에서 테이블 보기를 생성 및 유지 관리하고, 출선 및 작업을 사용하여 테이블 보기 항목을 코드에 노출하고, 테이블 항목을 채우고, 마지막으로 C# 코드에서 테이블 보기 항목에 응답하는 내용을 다룹니다.
- [출선 보기](~/mac/user-interface/outline-view.md) - 이 문서에서는 Xamarin.Mac 응용 프로그램에서 출선 보기를 작업하는 내용을 다룹니다. Xcode 및 Interface Builder에서 출선 보기를 생성 및 유지 관리하고, 출선 및 작업을 사용하여 출선 보기 항목을 코드에 노출하고, 출선 항목을 채우고, 마지막으로 C# 코드에서 출선 보기 항목에 응답하는 내용을 다룹니다.
- [원본 목록](~/mac/user-interface/source-list.md) - 이 문서에서는 Xamarin.Mac 응용 프로그램에서 원본 목록을 작업하는 내용을 다룹니다. Xcode 및 Interface Builder에서 원본 목록을 생성 및 유지 관리하고, 출선 및 작업을 사용하여 원본 목록 항목을 코드에 노출하고, 원본 목록 항목을 채우고, 마지막으로 C# 코드에서 원본 목록 항목에 응답하는 내용을 다룹니다.
- [컬렉션 보기](~/mac/user-interface/collection-view.md) - 이 문서에서는 Xamarin.Mac 응용 프로그램에서 컬렉션 보기를 작업하는 내용을 다룹니다. Xcode 및 Interface Builder에서 컬렉션 보기를 생성 및 유지 관리하고, 출선 및 작업을 사용하여 컬렉션 보기 요소를 코드에 노출하고, 컬렉션 보기를 채우고, 마지막으로 C# 코드에서 컬렉션 보기에 응답하는 내용을 다룹니다.
- [이미지 작업](~/mac/app-fundamentals/image.md) - 이 문서에서는 Xamarin.Mac 응용 프로그램에서 이미지 및 아이콘을 작업하는 내용을 다룹니다. 앱 아이콘을 만드는 데 필요한 이미지를 생성 및 유지 관리하고 C# 코드와 Xcode의 Interface Builder에서 이미지를 사용하는 내용을 다룹니다.

[Mac 샘플 갤러리](https://developer.xamarin.com/samples/mac/all/)도 살펴보시기를 권장합니다. 개발자가 Xamarin.Mac 프로젝트를 신속하게 시작할 수 있도록 즉시 사용 가능한 여러 코드가 제공됩니다.

사용자가 일반적인 Mac 응용 프로그램에서 예상하는 여러 기능을 포함하고 있는 완전한 Xamarin.Mac 앱의 예제는 [SourceWriter 샘플 앱](https://developer.xamarin.com/samples/mac/SourceWriter/)을 참조하세요. SourceWriter는 코드 완성 및 간단한 구문 강조 기능을 제공하는 간단한 소스 코드 편집기입니다.

SourceWriter 코드는 완벽하게 주석 처리되어 있으며, 가능한 경우 Xamarin.Mac 지침 설명서에 핵심 기술 또는 메서드부터 관련 정보까지 다양한 링크가 제공됩니다.

## <a name="summary"></a>요약

이 문서에서는 표준 Xamarin.Mac 앱의 기본 사항을 다루었습니다. Mac용 Visual Studio에서 새 앱을 만들고, Xcode 및 Interface Builder에서 사용자 인터페이스를 디자인하고, **출선** 및 **작업**을 사용하여 UI 요소를 C# 코드에 노출하고, UI 요소를 작업하는 코드를 추가하고, 마지막으로 Xamarin.Mac 앱을 빌드하고 테스트했습니다.

## <a name="related-links"></a>관련 링크

- [Hello, Mac(샘플)](https://developer.xamarin.com/samples/mac/Hello_Mac/)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
