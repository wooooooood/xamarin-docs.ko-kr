---
title: Xamarin.iOS에서 사용자 인터페이스 개체 만들기
description: 이 문서에서는 Xamarin.iOS에서 사용자 인터페이스를 만드는 방법의 개요를 제공 합니다. Xcode Interface Builder iOS 디자이너에 설명 C#, 및 스토리 보드입니다.
ms.prod: xamarin
ms.assetid: 4D6B136C-744A-4936-8655-A77E62BA7A60
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: a4cf63b84d0686bc28b02b18a6266908251bdf6f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61154077"
---
# <a name="creating-user-interface-objects-in-xamarinios"></a>Xamarin.iOS에서 사용자 인터페이스 개체 만들기

Apple 그룹 관련된 부분 "프레임 워크" Xamarin.iOS 네임 스페이스에 상응 하는 기능입니다. `UIKit` iOS에 대 한 모든 사용자 인터페이스 컨트롤이 포함 된 네임 스페이스가입니다.

다음을 포함 해야 코드에서 레이블 또는 단추와 같은 사용자 인터페이스 컨트롤을 참조 해야 할 때마다 문을 사용 하 여:

```csharp
using UIKit;
```

이 챕터에 설명 된 모든 컨트롤이 UIKit 네임 스페이스에 있으며 각 사용자 컨트롤 클래스 이름에는 `UI` 접두사입니다.

UI 컨트롤 및 세 가지 방법으로 레이아웃을 편집할 수 있습니다.

-  **[Xamarin iOS 디자이너](~/ios/user-interface/designer/index.md)**  – 사용 하 여 Xamarin의 기본 제공 레이아웃 디자이너 화면을 디자인 합니다. 스토리 보드 또는 기본 제공 디자이너를 사용 하 여 편집 XIB 파일을 두 번 클릭 합니다.
-  **Xcode Interface Builder** – 컨트롤 Interface Builder를 사용 하 여 화면 레이아웃으로 끌어 옵니다. Xcode에서에서 파일을 마우스 오른쪽 단추로 클릭 하 여 스토리 보드 또는 XIB 파일을 엽니다는 **Solution Pad** 선택 하 고 **연결 > Xcode Interface Builder**합니다.
-  **사용 하 여 C#**  – 컨트롤 프로그래밍 방식으로 코드를 사용 하 여 생성 및 수도 뷰 계층 구조에 추가 합니다.

IOS 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 새 스토리 보드 및 XIB 파일을 추가할 수 있습니다 **추가 > 새 파일...** .

어떤 메서드를 사용 하면 컨트롤 속성 및 이벤트 여전히을 조작할 수 있습니다 C# 응용 프로그램 논리에 있습니다.

## <a name="using-xamarin-ios-designer"></a>Xamarin iOS 디자이너를 사용 하 여

IOS 디자이너에서에서 사용자 인터페이스를 만들기를 시작 하려면 스토리 보드 파일을 두 번 클릭 합니다. 컨트롤의 디자인 화면으로 끌어 올 수는 **도구 상자** 아래 그림과 같이:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

 [![](creating-ui-objects-images/image2b.png "도구 상자 패드")](creating-ui-objects-images/image2b.png#lightbox)
 
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

 [![](creating-ui-objects-images/image2b-vs.png "도구 상자 패드-Visual Stuio")](creating-ui-objects-images/image2b.png#lightbox)
 
-----

디자인 화면에서 컨트롤이 선택 되는 경우는 **Properties Pad** 해당 컨트롤에 대 한 특성이 표시 됩니다. **위젯 > Identity > 이름** 아래 스크린샷에 채워지는 필드를 사용 합니다 *출 선* 이름. 이 컨트롤을 참조 하는 방법 C#:

 [![](creating-ui-objects-images/image3b.png "위젯 속성 패드")](creating-ui-objects-images/image3b.png#lightbox)

IOS 디자이너를 사용 하 여 심층 분석에 참조를 [iOS 디자이너 소개](~/ios/user-interface/designer/introduction.md) 가이드입니다.

## <a name="using-xcode-interface-builder"></a>Xcode Interface Builder를 사용 하 여

Interface Builder를 사용 하 여 잘 모르는 경우 Apple 가리킵니다 [Interface Builder](https://developer.apple.com/xcode/interface-builder/) 문서.

Xcode에서 스토리 보드를 열려면 마우스 오른쪽 단추로 클릭 하 여 선택한 스토리 보드 파일에 대 한 상황에 맞는 메뉴에 액세스 합니다 **Xcode Interface Builder**:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

 [![](creating-ui-objects-images/imagexcode.png "Xcode storyboard 상황에 맞는 메뉴")](creating-ui-objects-images/imagexcode.png#lightbox)
 
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](creating-ui-objects-images/imagexcode-vs.png "Xcode storyboard 상황에 맞는 메뉴")](creating-ui-objects-images/imagexcode-vs.png#lightbox)

-----

컨트롤의 디자인 화면으로 끌어 올 수는 **개체 라이브러리** 아래 그림과 같이:

 [![](creating-ui-objects-images/image5a.png "Xcode 개체 라이브러리")](creating-ui-objects-images/image5a.png#lightbox)

만들어야 하는 Interface Builder를 사용 하 여 UI를 디자인 하는 경우는 **출 선** 에서 참조 하려는 각 컨트롤에 대 한 C#합니다. 설정 하 여 이렇게 합니다 **도우미 편집기** 센터를 사용 하 **편집기** Xcode 도구 모음 단추는 단추:

 [![](creating-ui-objects-images/image6a.png "단말기 편집기 단추")](creating-ui-objects-images/image6a.png#lightbox)

사용자 인터페이스 개체;를 클릭합니다 그런 다음 **컨트롤 끌기** .h 파일에 있습니다. * * 컨트롤 끌기 * * 컨트롤 키를 클릭 하 고 사용자 인터페이스 개체 위로 가져가면 만들려는 출 선 (또는 작업)에 대 한 합니다. 헤더 파일을 끌어다 있지만 컨트롤 키를 누른 유지 합니다. 아래 끌어 마침은 `@interface` 정의 합니다. 아래 스크린샷에 표시 된 것과 같이 캡션 삽입 출 선 또는 출 선 컬렉션을 사용 하 여 파란색 선이 표시 됩니다.

만들기에 사용 되는 콘센트에 대 한 이름을 제공 하기 위해 묻는 클릭 놓으면는 C# 코드에서 참조할 수 있는 속성:

 [![](creating-ui-objects-images/image8a.png "아웃렛 만들기")](creating-ui-objects-images/image8a.png#lightbox)

Mac 용 Xcode의 Interface Builder Visual Studio와 통합 하는 방법에 대 한 자세한 내용은 참조는 [Xib 코드 생성](~/ios/internals/xib-code-generation.md#generated) 문서.

##  <a name="using-c"></a>사용 하 여C#

프로그래밍 방식으로 사용 하 여 사용자 인터페이스 개체를 만들기로 결정 하는 경우 C# (뷰 또는 예를 들어 뷰 컨트롤러)에서 다음이 단계를 수행 합니다.

-  사용자 인터페이스 개체에 대 한 클래스 수준 필드를 선언 합니다. 만들 컨트롤 자체에서 한 번 `ViewDidLoad` 예를 들어 합니다. 개체 (예: 뷰 컨트롤러의 수명 주기 메서드 전체에서 참조할 수 있습니다.
`ViewWillAppear`).
-  만들기는 `CGRect` (화면 뿐만 아니라 해당 너비와 높이에서 X 및 Y 좌표) 컨트롤의 프레임을 정의 하는 합니다. 했는지 확인 해야는 `using CoreGraphics` 이 대 한 지시문입니다.
-  컨트롤을 만들고 생성자를 호출 합니다.
-  속성 또는 이벤트 처리기를 설정 합니다.
-  호출 `Add()` 뷰 계층 구조에 컨트롤을 추가 합니다.

다음은 만드는 간단한 예제는 `UILabel` 사용 하 여 뷰 컨트롤러 C#:

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

## <a name="using-c-and-storyboards"></a>사용 하 여 C# 및 스토리 보드

뷰 컨트롤러에 해당 하는 두 개의 디자인 화면에 추가 될 때 C# 파일이 프로젝트에 만들어집니다. 이 예에서 `ControlsViewController.cs` 및 `ControlsViewController.designer.cs` 자동으로 생성 되었습니다.

 [![](creating-ui-objects-images/image9b.png "ViewController partial 클래스")](creating-ui-objects-images/image9b.png#lightbox)

합니다 `MainViewController.cs` 파일에 대 한 것 *코드*합니다. 이 경우는 `View` 와 같은 수명 주기 메서드 `ViewDidLoad` 및 `ViewWillAppear` 구현 됩니다 고유한 속성, 필드 및 메서드를 추가할 수 있습니다.

`ControlsViewController.designer.cs` 생성 된 코드는 partial 클래스 포함입니다. 디자인에서 컨트롤 이름을 Mac 용 Visual Studio에서 화면에서 만들거나 때 출 선 또는 동작 하는 Xcode, 해당 속성 또는 부분 메서드를 디자이너 (designer.cs) 파일에 추가 됩니다. 아래 코드는 단추 중 하나 수도 있는 두 개의 단추와 텍스트 뷰를 생성 된 코드의 예가 나와 `TouchUpInside` 이벤트입니다.

컨트롤을 참조 하 고 디자인 화면에서 선언 되는 작업에 응답 하도록 코드를 사용 하도록 설정 하는 partial 클래스의 이러한 구성 요소:

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

`designer.cs` 파일 수동으로 편집 하지 말아야 – IDE (Visual Studio for Mac 또는 Visual Studio)는 스토리 보드를 사용 하 여 동기화 유지 합니다.

사용자 인터페이스 개체를 프로그래밍 방식으로 추가 될 때를 `View` 또는 `ViewController`를 인스턴스화하고 개체 참조를 직접 관리 이므로 디자이너 파일이 필요 합니다.



## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://developer.xamarin.com/samples/Controls/)
