---
title: "사용자 인터페이스 개체 만들기"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4D6B136C-744A-4936-8655-A77E62BA7A60
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 035956f5c39a77c625a6f4cb92cbfa67a42f2402
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="creating-user-interface-objects"></a>사용자 인터페이스 개체 만들기

Apple 그룹 관련 부분에 "프레임 워크" Xamarin.iOS 네임 스페이스에 있는 기능을 합니다. `UIKit` iOS에 대 한 모든 사용자 인터페이스 컨트롤이 포함 된 네임 스페이스가입니다.

코드에서 사용자 인터페이스 컨트롤, 단추, 레이블 등을 참조 해야 할 때마다이 다음을 포함 해야 문을 사용 하 여:

```csharp
using UIKit;
```


이 장에서 설명한 모든 컨트롤 UIKit 네임 스페이스에 있으며 각각 사용자 컨트롤의 클래스 이름에는 `UI` 접두사입니다.

UI 컨트롤 및 레이아웃에 세 가지 방법으로 편집할 수 있습니다.

-  **[Xamarin iOS 디자이너](~/ios/user-interface/designer/index.md)**  -디자인 화면을 사용 하 여 Xamarin의 기본 레이아웃 디자이너입니다. 스토리 보드 또는 XIB 편집할 파일을 기본 제공 디자이너와 두 번 클릭 합니다.
-  **Xcode 인터페이스 작성기** – 컨트롤 인터페이스 작성기와 함께 화면 레이아웃으로 끌어 옵니다. 파일을 마우스 오른쪽 단추로 클릭 하 여 Xcode에서 스토리 보드 또는 XIB 파일을 열는 **솔루션 패드** 선택한 **연결 프로그램 > Xcode 인터페이스 작성기**합니다.
-  **C#을 사용 하 여** – 컨트롤 프로그래밍 방식으로 코드를 사용 하 여 생성 끌어다 수도 보기 계층에 추가 합니다.

IOS 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 새 스토리 보드 및 XIB 파일을 추가할 수 **추가 > 새 파일...** .

어떤 방법을 사용 하는 속성을 제어 및 이벤트 여전히 C#으로 응용 프로그램 논리에 조작할 수 있습니다.

## <a name="using-xamarin-ios-designer"></a>Xamarin iOS 디자이너를 사용 하 여

IOS 디자이너에서에서 사용자 인터페이스 만들기를 시작 하려면 스토리 보드 파일을 두 번 클릭 합니다. 컨트롤의 디자인 화면으로 끌어 올 수는 **도구 상자** 아래 그림과 같이:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 [ ![](creating-ui-objects-images/image2b.png "도구 상자 채움")](creating-ui-objects-images/image2b.png)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 [ ![](creating-ui-objects-images/image2b-vs.png "도구 상자 패드-Visual Stuio")](creating-ui-objects-images/image2b.png)
 
-----

디자인 화면에 컨트롤을 선택 하면는 **속성 패드** 해당 컨트롤에 대 한 특성이 표시 됩니다. **위젯 > Identity > 이름** 필드 아래 스크린샷에서 채워지는으로 사용 되는 *콘센트* 이름입니다. 다음은 C#에서 컨트롤을 참조 하는 방법입니다.

 [ ![](creating-ui-objects-images/image3b.png "위젯 패드 속성")](creating-ui-objects-images/image3b.png)

IOS 디자이너를 사용 하 여에 대해 자세히 알아보려면에 대 한 참조는 [iOS 디자이너 소개](~/ios/user-interface/designer/introduction.md) 가이드입니다.

## <a name="using-xcode-interface-builder"></a>Xcode 인터페이스 작성기를 사용 하 여

Apple의를 참조 하는 인터페이스 작성기를 사용 하 여 잘 모르는 경우 [인터페이스 작성기](https://developer.apple.com/xcode/interface-builder/) 문서.

Xcode에서 스토리 보드를 열려면 마우스 오른쪽 단추를 스토리 보드 파일에 대 한 상황에 맞는 메뉴에 액세스 하 고으로 열려면 선택은 **Xcode 인터페이스 작성기**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 [ ![](creating-ui-objects-images/imagexcode.png "스토리 보드 상황에 맞는 메뉴-Xcode")](creating-ui-objects-images/imagexcode.png)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](creating-ui-objects-images/imagexcode-vs.png "스토리 보드 상황에 맞는 메뉴-Xcode")](creating-ui-objects-images/imagexcode-vs.png)

-----

컨트롤의 디자인 화면으로 끌어 올 수는 **개체 라이브러리** 아래 그림에 나와 있습니다.

 [ ![](creating-ui-objects-images/image5a.png "Xcode 개체 라이브러리")](creating-ui-objects-images/image5a.png)

만들어야 하는 인터페이스 작성기 UI를 디자인 하는 경우는 **콘센트** C#에서 참조 하고자 하는 각 컨트롤에 대 한 합니다. 설정 하 여 이렇게는 **도우미 편집기** 중심을 사용 하 여 **편집기** Xcode 도구 모음 단추에서 단추:

 [ ![](creating-ui-objects-images/image6a.png "도우미 편집기 단추")](creating-ui-objects-images/image6a.png)

사용자 인터페이스 개체; 클릭 그런 다음 **컨트롤 끌기** .h 파일에 있습니다. 하려면 * * 제어 끌어서 * * 컨트롤이 키를 누른 다음 클릭 한 채로 사용자 인터페이스 개체에 대해 사용자가 만드는 콘센트 (또는 작업)에 대 한 합니다. 헤더 파일에 핸들이 컨트롤 키를 누른 유지 합니다. 아래 끌어 완료는 `@interface` 정의 합니다. 파란색 선 아래 스크린샷에 표시 된 것 처럼 삽입 콘센트 캡션 또는 콘센트 컬렉션으로 표시 됩니다.

클릭을 놓을 때 속성을 만드는 C# 참조 될 수 있는 코드에서 사용 되는 콘센트에 대 한 이름을 제공 하 라는 메시지가 표시 됩니다.

 [ ![](creating-ui-objects-images/image8a.png "콘센트 만들기")](creating-ui-objects-images/image8a.png)

Mac 용 Xcode의 인터페이스 작성기 Visual Studio와 통합 하는 방법에 대 한 자세한 내용은 참조는 [Xib 코드 생성](~/ios/internals/xib-code-generation.md#generated) 문서.

##  <a name="using-c"></a>C#을 사용 하 여

프로그래밍 방식으로 뷰 또는 예를 들어 뷰 컨트롤러) (의 C#을 사용 하 여 사용자 인터페이스 개체를 만드는 경우 다음이 단계를 따르십시오.

-  사용자 인터페이스 개체에 대 한 클래스 수준 필드를 선언 합니다. 자체 컨트롤 만들기에서 한 번, `ViewDidLoad` 예입니다. 개체 수명 주기 메서드 뷰 컨트롤러 (예:의 전체에서 참조할 수 있습니다.
`ViewWillAppear`).
-  만들기는 `CGRect` (화면으로 너비와 높이에 X 및 Y 좌표) 컨트롤의 프레임을 정의 하는 합니다. 있는지 확인 해야 합니다.는 `using CoreGraphics` 지시문이 있습니다.
-  만들고 컨트롤 지정 생성자를 호출 합니다.
-  모든 속성 또는 이벤트 처리기를 설정 합니다.
-  호출 `Add()` 보기 계층에 컨트롤을 추가 하려면.

다음은 만드는 간단한 예제는 `UILabel` 뷰 컨트롤러에서 사용 하 여 C#:

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

## <a name="using-c-and-storyboards"></a>C# 및 스토리 보드를 사용 하 여

컨트롤러 보기는 디자인 화면에 추가 되 면 해당 두 개의 C# 파일 프로젝트에서 만들어집니다. 이 예제에서는 `ControlsViewController.cs` 및 `ControlsViewController.designer.cs` 자동으로 생성 되었습니다.

 [ ![](creating-ui-objects-images/image9b.png "ViewController partial 클래스")](creating-ui-objects-images/image9b.png)

`MainViewController.cs` 파일 위한 *코드*합니다. 여기에는 `View` 와 같은 수명 주기 메서드 `ViewDidLoad` 및 `ViewWillAppear` 구현 된 사용자 고유의 속성, 필드 및 메서드를 추가할 수 있습니다.

`ControlsViewController.designer.cs` partial 클래스 포함 코드를 생성된 합니다. 컨트롤의 디자인에 이름, Mac 용 Visual Studio에서 노출를 하거나 만들 때 콘센트 또는 동작 하는 Xcode, 해당 하는 속성 또는 부분 메서드 (designer.cs) 디자이너 파일에 추가 됩니다. 아래 코드는 단추 중 하나 또한에 있는 두 개의 단추와 텍스트 보기에 대해 생성 된 코드의 예가 나와 `TouchUpInside` 이벤트입니다.

컨트롤을 참조 하 고 디자인 화면에서 선언 된 작업에 응답 하도록 코드를 사용 하도록 설정 하는 partial 클래스의 다음 구성이 요소:

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

`designer.cs` IDE (Mac 또는 Visual Studio에 대 한 Visual Studio)는 스토리 보드와 동기화 지속적으로 업데이트에 대해 책임이 – 파일 수동으로 편집 하지 말아야 합니다.

사용자 인터페이스 개체를 프로그래밍 방식으로 추가 됩니다 한 `View` 또는 `ViewController`, 인스턴스화하고 개체 참조를 직접 관리 되며 따라서 디자이너 파일이 필요 합니다.



## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://developer.xamarin.com/samples/Controls/)
