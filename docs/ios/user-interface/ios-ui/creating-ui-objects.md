---
title: Xamarin.ios에서 사용자 인터페이스 개체 만들기
description: 이 문서에서는 Xamarin.ios에서 사용자 인터페이스를 만드는 방법에 대 한 개요를 제공 합니다. IOS Designer, Xcode Interface Builder, C#및 storyboard에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 4D6B136C-744A-4936-8655-A77E62BA7A60
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 204bf087a51132fdd204990c3b92453ecce96a53
ms.sourcegitcommit: 20c645f41620d5124da75943de1b690261d00660
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72426584"
---
# <a name="creating-user-interface-objects-in-xamarinios"></a>Xamarin.ios에서 사용자 인터페이스 개체 만들기

Apple에서는 관련 기능을 Xamarin.ios 네임 스페이스와 관련 된 "프레임 워크"로 그룹화 합니다. `UIKit`은 iOS의 모든 사용자 인터페이스 컨트롤을 포함 하는 네임 스페이스입니다.

코드에서 레이블이나 단추와 같은 사용자 인터페이스 컨트롤을 참조 해야 할 때마다 다음 using 문을 포함 해야 합니다.

```csharp
using UIKit;
```

이 장에서 설명 하는 모든 컨트롤은 UIKit 네임 스페이스에 있고 각 사용자 컨트롤 클래스 이름에는 `UI` 접두사가 있습니다.

다음 세 가지 방법으로 UI 컨트롤 및 레이아웃을 편집할 수 있습니다.

- **[Xamarin IOS Designer](~/ios/user-interface/designer/index.md)** – xamarin의 기본 제공 레이아웃 디자이너를 사용 하 여 화면을 디자인 합니다. 스토리 보드 또는 XIB 파일을 두 번 클릭 하 여 기본 제공 디자이너를 사용 하 여 편집할 수 있습니다.
- **Xcode Interface Builder** – Interface Builder를 사용 하 여 화면 레이아웃에 컨트롤을 끌어 놓습니다. **Solution Pad** 에서 파일을 마우스 오른쪽 단추로 클릭 하 고 **> Xcode Interface Builder를 사용**하 여 열기를 선택 하 여 XCODE에서 storyboard 또는 XIB 파일을 엽니다.
- **사용 C#**  – 컨트롤을 프로그래밍 방식으로 코드를 사용 하 여 생성 하 고 뷰 계층 구조에 추가할 수도 있습니다.

IOS 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **새 파일 > 추가**...를 선택 하 여 새 스토리 보드 및 XIB 파일을 추가할 수 있습니다.

어떤 방법을 사용 하 든 컨트롤 속성 및 이벤트는 응용 프로그램 논리에서 C# 를 사용 하 여 계속 조작할 수 있습니다.

## <a name="using-xamarin-ios-designer"></a>Xamarin iOS Designer 사용

IOS 디자이너에서 사용자 인터페이스 만들기를 시작 하려면 스토리 보드 파일을 두 번 클릭 합니다. 아래 그림과 같이 **도구 상자** 에서 컨트롤을 디자인 화면으로 끌어 놓을 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

 [![](creating-ui-objects-images/image2b.png "Toolbox Pad")](creating-ui-objects-images/image2b.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

 [![](creating-ui-objects-images/image2b-vs.png "Toolbox Pad - Visual Stuio")](creating-ui-objects-images/image2b.png#lightbox)

-----

디자인 화면에서 컨트롤을 선택 하면 **Properties Pad** 해당 컨트롤에 대 한 특성이 표시 됩니다. 아래 스크린샷에서 채워지는 **위젯 > id > 이름** 필드가 *콘센트* 이름으로 사용 됩니다. 에서 C#컨트롤을 참조 하는 방법은 다음과 같습니다.

 [![](creating-ui-objects-images/image3b.png "Properties Widget Pad")](creating-ui-objects-images/image3b.png#lightbox)

IOS designer를 사용 하는 방법에 대 한 자세한 내용은 [Ios 디자이너 소개](~/ios/user-interface/designer/introduction.md) 가이드를 참조 하세요.

## <a name="using-xcode-interface-builder"></a>Xcode Interface Builder 사용

Interface Builder 사용에 익숙하지 않은 경우 Apple의 [Interface Builder](https://developer.apple.com/xcode/interface-builder/) 문서를 참조 하세요.

Xcode에서 Storyboard를 열려면 마우스 오른쪽 단추를 클릭 하 여 스토리 보드 파일의 상황에 맞는 메뉴에 액세스 하 고 **Xcode Interface Builder**를 사용 하 여 열도록 선택 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

 [![](creating-ui-objects-images/imagexcode.png "Storyboard context menu - Xcode")](creating-ui-objects-images/imagexcode.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](creating-ui-objects-images/imagexcode-vs.png "Storyboard context menu - Xcode")](creating-ui-objects-images/imagexcode-vs.png#lightbox)

-----

컨트롤은 아래에 설명 된 **개체 라이브러리** 의 Design Surface 끌어 올 수 있습니다.

 [![](creating-ui-objects-images/image5a.png "Xcode Object Library")](creating-ui-objects-images/image5a.png#lightbox)

Interface Builder를 사용 하 여 UI를 디자인 하는 경우 에서 C#참조 하려는 각 컨트롤에 대 한 콘센트를 만들어야 합니다. Xcode 도구 모음 단추의 가운데 **편집기** 단추를 사용 하 여 **길잡이 편집기** 를 켜면이 작업을 수행할 수 있습니다.

 [![](creating-ui-objects-images/image6a.png "Assistant Editor button")](creating-ui-objects-images/image6a.png#lightbox)

사용자 인터페이스 개체를 클릭 합니다. 그런 다음 컨트롤을 .h 파일로 **끌어 놓습니다** . **끌기를 제어**하려면 컨트롤 키를 누른 채에 대 한 콘센트가 나 동작을 만들 사용자 인터페이스 개체를 클릭 합니다. 컨트롤 키를 계속 누르고 있으면 헤더 파일로 끌어 놓습니다. @No__t-0 정의 아래로 끌기를 마칩니다. 아래 스크린샷에 표시 된 것 처럼 캡션이 삽입 유출 또는 유출 컬렉션으로 표시 됩니다.

클릭 하면 코드에서 참조 될 수 있는 C# 속성을 만드는 데 사용할 수 있는 콘센트의 이름을 입력 하 라는 메시지가 표시 됩니다.

 [![](creating-ui-objects-images/image8a.png "Creating an outlet")](creating-ui-objects-images/image8a.png#lightbox)

Xcode의 Interface Builder Mac용 Visual Studio 통합 하는 방법에 대 한 자세한 내용은 [Xib 코드 생성](~/ios/internals/xib-code-generation.md#generated) 문서를 참조 하세요.

## <a name="using-c"></a>Using C @ no__t-0

보기 또는 보기 컨트롤러에서를 사용 하 여 C# 프로그래밍 방식으로 사용자 인터페이스 개체를 만들려면 다음 단계를 수행 합니다.

- 사용자 인터페이스 개체에 대 한 클래스 수준 필드를 선언 합니다. 예를 들어 `ViewDidLoad`에서 컨트롤 자체를 한 번 만듭니다. 그런 다음 뷰 컨트롤러의 수명 주기 메서드 (예:
`ViewWillAppear`).
- 컨트롤의 프레임 (너비와 높이는 물론 화면에서 X 및 Y 좌표)을 정의 하는 `CGRect`을 만듭니다. 이에 대 한 `using CoreGraphics` 지시문이 있는지 확인 해야 합니다.
- 생성자를 호출 하 여 컨트롤을 만들고 할당 합니다.
- 속성 또는 이벤트 처리기를 설정 합니다.
- @No__t-0을 호출 하 여 뷰 계층 구조에 컨트롤을 추가 합니다.

다음은를 사용 하 여 C#뷰 컨트롤러에서 `UILabel`을 만드는 간단한 예입니다.

```csharp
UILabel label1;
public override void ViewDidLoad () {
    base.ViewDidLoad ();
    var frame = new CGRect(10, 10, 300, 30);
    label1 = new UILabel(frame);
    label1.Text = "New Label";
    View.Add (label1);
}
```

<a name="partial_classes" />

## <a name="using-c-and-storyboards"></a>및 C# storyboard 사용

Design Surface에 뷰 컨트롤러를 추가 하면 프로젝트에 해당 C# 하는 두 개의 파일이 만들어집니다. 이 예제에서는 `ControlsViewController.cs` 및 `ControlsViewController.designer.cs`이 자동으로 생성 되었습니다.

 [![](creating-ui-objects-images/image9b.png "ViewController partial class")](creating-ui-objects-images/image9b.png#lightbox)

@No__t-0 파일은 *코드*를 위한 것입니다. 여기에는 `ViewDidLoad`과 같은 `View` 수명 주기 메서드 및 `ViewWillAppear`가 구현 되며 사용자 고유의 속성, 필드 및 메서드를 추가할 수 있습니다.

@No__t-0은 partial 클래스를 포함 하는 생성 된 코드입니다. Mac용 Visual Studio의 디자인 화면에서 컨트롤의 이름을 설정 하거나 Xcode에서 콘센트가 나 작업을 만들면 designer.cs (디자이너) 파일에 해당 속성 또는 부분 메서드가 추가 됩니다. 아래 코드에서는 두 개의 단추와 텍스트 뷰에 대해 생성 된 코드의 예제를 보여 줍니다. 여기에는 단추 중 하나에 `TouchUpInside` 이벤트가 포함 됩니다.

Partial 클래스의 이러한 요소를 사용 하면 코드에서 컨트롤을 참조 하 고 디자인 화면에 선언 된 작업에 응답할 수 있습니다.

```csharp
[Register ("ControlsViewController")]
    partial class ControlsViewController
    {
        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UIButton Button1 { get; set; }

        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UIButton Button2 { get; set; }

        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UITextField textfield1 { get; set; }

        [Action ("button2_TouchUpInside:")]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        partial void button2_TouchUpInside (UIButton sender);

        void ReleaseDesignerOutlets ()
        {
            if (Button1 != null) {
                Button1.Dispose ();
                Button1 = null;
            }
            if (Button2 != null) {
                Button2.Dispose ();
                Button2 = null;
            }
            if (textfield1 != null) {
                textfield1.Dispose ();
                textfield1 = null;
            }
        }
    }
}
```

@No__t-0 파일은 수동으로 편집 하면 안 됩니다. IDE (Mac용 Visual Studio 또는 Visual Studio)는 스토리 보드와 동기화 된 상태로 유지 해야 합니다.

사용자 인터페이스 개체가 `View` 또는 `ViewController`에 프로그래밍 방식으로 추가 되는 경우 개체 참조를 직접 인스턴스화하고 관리 하므로 디자이너 파일이 필요 하지 않습니다.

## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
