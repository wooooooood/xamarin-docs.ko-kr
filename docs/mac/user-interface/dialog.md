---
title: Xamarin.ios의 대화 상자
description: 이 문서에서는 Xamarin.ios 응용 프로그램에서 대화 상자 및 모달 창을 사용 하는 방법을 설명 합니다. Xcode 및 Interface builder에서 모달 창을 만들고, 표준 대화 상자를 사용 하 고, 코드에서 C# 이러한 컨트롤과 상호 작용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 55451990-B77B-4D44-B8BB-F874EC503B0C
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 03fad33d49f1454700c118ad44c8582453a75eee
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68645661"
---
# <a name="dialogs-in-xamarinmac"></a>Xamarin.ios의 대화 상자

Xamarin.ios 응용 프로그램 C# 에서 및 .net을 사용 하는 경우 *목표-C* 및 *Xcode* 에서 작업 하는 개발자가 동일한 대화 상자 및 모달 창에 액세스할 수 있습니다. Xamarin.ios는 Xcode와 직접 통합 되므로 Xcode의 _Interface Builder_ 를 사용 하 여 모달 창을 만들고 유지 관리 하거나 (필요에 따라 코드에서 C# 직접 만들 수 있습니다.)

사용자 동작에 대 한 응답으로 대화 상자가 나타나고 일반적으로 사용자가 작업을 완료할 수 있는 방법이 제공 됩니다. 대화 상자를 닫기 전에 사용자의 응답이 필요 합니다.

Windows는 모덜리스 상태 (예: 한 번에 여러 문서를 열 수 있는 텍스트 편집기) 또는 모달 (예: 응용 프로그램을 계속 하기 전에 해제 해야 하는 내보내기 대화 상자)에서 사용할 수 있습니다.

[![](dialog-images/dialog03.png "열기 대화 상자")](dialog-images/dialog03.png#lightbox)

이 문서에서는 Xamarin.ios 응용 프로그램에서 대화 상자 및 모달 창 작업의 기본 사항을 다룹니다. [Hello, Mac](~/mac/get-started/hello-mac.md) 문서를 먼저 사용 하는 것이 가장 좋습니다. 특히 [Xcode 및 Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 및 [콘센트 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션을 소개 하 고,에서 사용할 주요 개념 및 기술을 설명 하 고 있습니다. 이 문서를 참조 하세요.

[Xamarin.ios 내부](~/mac/internals/how-it-works.md) 문서의 [클래스/메서드를 대상으로 노출 C# -C](~/mac/internals/how-it-works.md) 섹션을 살펴볼 수 있습니다. C# 클래스를 연결 하는 데 사용 되는 `Register` 및 `Export` 명령을 설명 합니다. 목표-C 개체 및 UI 요소입니다.

<a name="Introduction_to_Dialogs" />

## <a name="introduction-to-dialogs"></a>대화 상자 소개

사용자 작업 (예: 파일 저장)에 대 한 응답으로 대화 상자가 나타나고 사용자가 해당 작업을 완료 하는 방법을 제공 합니다. 대화 상자를 닫기 전에 사용자의 응답이 필요 합니다.

Apple에 따르면 대화 상자를 표시 하는 세 가지 방법이 있습니다.

- **문서 모달** -문서 모달 대화 상자는 사용자가 지정 된 문서 내에서 해제할 때까지 다른 작업을 수행할 수 없도록 합니다.
- **앱 모달** -앱 모달 대화 상자를 사용 하면 사용자가 응용 프로그램을 닫을 때까지 응용 프로그램을 조작할 수 없습니다.
- **모덜리스** 모덜리스 대화 상자를 사용 하면 사용자가 계속 해 서 문서 창과 상호 작용 하는 동안 대화 상자에서 설정을 변경할 수 있습니다.

### <a name="modal-window"></a>모달 창

모든 표준은 `NSWindow` 모달을 표시 하 여 사용자 지정 된 대화 상자로 사용할 수 있습니다.

[![](dialog-images/modal01.png "예제 모달 창")](dialog-images/modal01.png#lightbox)

### <a name="document-modal-dialog-sheets"></a>문서 모달 대화 상자 시트

_시트_ 는 지정 된 문서 창에 연결 되어 사용자가 대화 상자를 해제할 때까지 창과 상호 작용할 수 없도록 하는 모달 대화 상자입니다. 시트가 나타나는 창에 연결 되어 있고 한 번에 하나의 시트만 창에 대해 열 수 있습니다.

[![](dialog-images/sheet08.png "예제 모달 시트")](dialog-images/sheet08.png#lightbox)

### <a name="preferences-windows"></a>기본 설정 창

기본 설정 창은 사용자가 자주 변경 하지 않는 응용 프로그램의 설정을 포함 하는 모덜리스 대화 상자입니다. 기본 설정 창에는 사용자가 여러 설정 그룹 간에 전환할 수 있는 도구 모음이 포함 되는 경우가 많습니다.

[![](dialog-images/dialog02.png "예제 기본 설정 창")](dialog-images/dialog02.png#lightbox)

### <a name="open-dialog"></a>대화 상자 열기

열기 대화 상자를 사용 하면 응용 프로그램에서 항목을 찾고 열 수 있는 일관 된 방법을 사용자에 게 제공 합니다.

[![](dialog-images/dialog03.png "열기 대화 상자")](dialog-images/dialog03.png#lightbox)


### <a name="print-and-page-setup-dialogs"></a>인쇄 및 페이지 설정 대화 상자

macOS는 사용자가 사용 하는 모든 응용 프로그램에서 일관 된 인쇄 환경을 제공할 수 있도록 응용 프로그램에서 표시할 수 있는 표준 인쇄 및 페이지 설정 대화 상자를 제공 합니다.

인쇄 대화 상자는 사용 가능한 부동 대화 상자로 모두 표시 될 수 있습니다.

[![](dialog-images/print01.png "인쇄 대화 상자")](dialog-images/print01.png#lightbox)

또는 다음과 같이 시트로 표시 될 수 있습니다.

[![](dialog-images/print02.png "인쇄 시트")](dialog-images/print02.png#lightbox)

페이지 설정 대화 상자는 사용 가능한 부동 대화 상자로 표시 될 수 있습니다.

[![](dialog-images/print03.png "페이지 설정 대화 상자")](dialog-images/print03.png#lightbox)

또는 다음과 같이 시트로 표시 될 수 있습니다.

[![](dialog-images/print04.png "페이지 설정 시트")](dialog-images/print04.png#lightbox)

### <a name="save-dialogs"></a>대화 상자 저장

저장 대화 상자는 사용자에 게 응용 프로그램에 항목을 저장 하는 일관 된 방법을 제공 합니다. 저장 대화 상자에는 두 가지 상태가 있습니다. **최소** (축소 라고도 함):

[![](dialog-images/save01.png "저장 대화 상자")](dialog-images/save01.png#lightbox)

**확장** 된 상태:

[![](dialog-images/save02.png "확장 된 저장 대화 상자")](dialog-images/save02.png#lightbox)

**최소** 저장 대화 상자는 시트로 표시 될 수도 있습니다.

[![](dialog-images/save03.png "최소 저장 시트")](dialog-images/save03.png#lightbox)

**확장** 된 저장 대화 상자에서 다음을 수행할 수 있습니다.

[![](dialog-images/save04.png "확장 된 저장 시트")](dialog-images/save04.png#lightbox)

자세한 내용은 Apple [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) 의 [대화 상자](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowDialogs.html#//apple_ref/doc/uid/20000957-CH43-SW1) 섹션을 참조 하세요.

<a name="Adding_a_Modal_Window_to_a_Project" />

## <a name="adding-a-modal-window-to-a-project"></a>프로젝트에 모달 창 추가

기본 문서 창 외에도 Xamarin.ios 응용 프로그램은 기본 설정 또는 검사기 패널과 같은 다른 형식의 창을 사용자에 게 표시 해야 할 수 있습니다.

새 창을 추가 하려면 다음을 수행 합니다.

1. **솔루션 탐색기**에서 Xcode의 Interface Builder에서 `Main.storyboard` 편집할 파일을 엽니다.
2. 새 **뷰 컨트롤러** 를 Design Surface 끌어 옵니다.

    [![](dialog-images/new01.png "라이브러리에서 보기 컨트롤러 선택")](dialog-images/new01.png#lightbox)
3. **Identity Inspector**에서 **클래스 이름**으로 `CustomDialogController` 를 입력 합니다. 

    [![](dialog-images/new02.png "클래스 이름 설정")](dialog-images/new02.png#lightbox)
4. Mac용 Visual Studio으로 다시 전환 하 여 Xcode와 동기화 하 고 `CustomDialogController.h` 파일을 만듭니다.
5. Xcode로 돌아가서 인터페이스를 디자인 합니다. 

    [![](dialog-images/new03.png "Xcode에서 UI 디자인")](dialog-images/new03.png#lightbox)
6. 대화 상자를 대화 상자 창으로 여는 UI 요소에서 컨트롤을 끌어 새 뷰 컨트롤러에 대 한 응용 프로그램의 주 창에서 **모달 Segue** 을 만듭니다. 식별자`ModalSegue`를 할당 합니다. 

    [![](dialog-images/new06.png "모달 segue")](dialog-images/new06.png#lightbox)
7. **작업** 및 **콘센트**를 연결 합니다. 

    [![](dialog-images/new04.png "작업 구성")](dialog-images/new04.png#lightbox)
8. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

파일이 다음과 `CustomDialogController.cs` 같이 표시 되도록 합니다.

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class CustomDialogController : NSViewController
    {
        #region Private Variables
        private string _dialogTitle = "Title";
        private string _dialogDescription = "Description";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string DialogTitle {
            get { return _dialogTitle; }
            set { _dialogTitle = value; }
        }

        public string DialogDescription {
            get { return _dialogDescription; }
            set { _dialogDescription = value; }
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public CustomDialogController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial title and description
            Title.StringValue = DialogTitle;
            Description.StringValue = DialogDescription;
        }
        #endregion

        #region Private Methods
        private void CloseDialog() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptDialog (Foundation.NSObject sender) {
            RaiseDialogAccepted();
            CloseDialog();
        }

        partial void CancelDialog (Foundation.NSObject sender) {
            RaiseDialogCanceled();
            CloseDialog();
        }
        #endregion

        #region Events
        public EventHandler DialogAccepted;

        internal void RaiseDialogAccepted() {
            if (this.DialogAccepted != null)
                this.DialogAccepted (this, EventArgs.Empty);
        }

        public EventHandler DialogCanceled;

        internal void RaiseDialogCanceled() {
            if (this.DialogCanceled != null)
                this.DialogCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

이 코드는 대화에 대 한 제목 및 설명을 설정 하는 몇 가지 속성을 노출 하 고, 대화 상자를 취소 하거나 수락 하는 데 반응할 몇 가지 이벤트를 표시 합니다.

그런 다음 `ViewController.cs` 파일을 편집 하 고 `PrepareForSegue` 메서드를 재정의 하 여 다음과 같이 만듭니다.

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    }
}
```

이 코드는 Xcode의 Interface Builder에 정의 된 segue를 대화 상자에 초기화 하 고 제목 및 설명을 설정 합니다. 또한 사용자가 대화 상자에서 수행 하는 선택 항목을 처리 합니다.

응용 프로그램을 실행 하 고 사용자 지정 대화 상자를 표시할 수 있습니다.

[![](dialog-images/new05.png "예제 대화 상자")](dialog-images/new05.png#lightbox)

Xamarin.ios 응용 프로그램에서 windows를 사용 하는 방법에 대 한 자세한 내용은 [windows의 작업](~/mac/user-interface/window.md) 설명서를 참조 하세요.

<a name="Creating_a_Custom_Sheet" />

## <a name="creating-a-custom-sheet"></a>사용자 지정 시트 만들기

_시트_ 는 지정 된 문서 창에 연결 되어 사용자가 대화 상자를 해제할 때까지 창과 상호 작용할 수 없도록 하는 모달 대화 상자입니다. 시트가 나타나는 창에 연결 되어 있고 한 번에 하나의 시트만 창에 대해 열 수 있습니다. 

Xamarin.ios에서 사용자 지정 시트를 만들려면 다음을 수행 합니다.

1. **솔루션 탐색기**에서 Xcode의 Interface Builder에서 `Main.storyboard` 편집할 파일을 엽니다.
2. 새 **뷰 컨트롤러** 를 Design Surface 끌어 옵니다.

    [![](dialog-images/new01.png "라이브러리에서 보기 컨트롤러 선택")](dialog-images/new01.png#lightbox)
3. 사용자 인터페이스 디자인:

    [![](dialog-images/sheet01.png "UI 디자인")](dialog-images/sheet01.png#lightbox)
4. 주 창에서 새 뷰 컨트롤러로 **시트 Segue** 를 만듭니다. 

    [![](dialog-images/sheet02.png "Sheet segue 유형 선택")](dialog-images/sheet02.png#lightbox)
5. **Identity Inspector**에서 뷰 컨트롤러의 **클래스** `SheetViewController`이름을 다음과 같이 표시 합니다. 

    [![](dialog-images/sheet03.png "클래스 이름 설정")](dialog-images/sheet03.png#lightbox)
6. 필요한 모든 **콘센트** 및 **작업**을 정의 합니다. 

    [![](dialog-images/sheet04.png "필요한 콘센트 및 작업 정의")](dialog-images/sheet04.png#lightbox)
7. 변경 내용을 저장 하 고 동기화 할 Mac용 Visual Studio로 돌아갑니다.

그런 다음 `SheetViewController.cs` 파일을 편집 하 여 다음과 같이 만듭니다.

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class SheetViewController : NSViewController
    {
        #region Private Variables
        private string _userName = "";
        private string _password = "";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string UserName {
            get { return _userName; }
            set { _userName = value; }
        }

        public string Password {
            get { return _password;}
            set { _password = value;}
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public SheetViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial values
            NameField.StringValue = UserName;
            PasswordField.StringValue = Password;

            // Wireup events
            NameField.Changed += (sender, e) => {
                UserName = NameField.StringValue;
            };
            PasswordField.Changed += (sender, e) => {
                Password = PasswordField.StringValue;
            };
        }
        #endregion

        #region Private Methods
        private void CloseSheet() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptSheet (Foundation.NSObject sender) {
            RaiseSheetAccepted();
            CloseSheet();
        }

        partial void CancelSheet (Foundation.NSObject sender) {
            RaiseSheetCanceled();
            CloseSheet();
        }
        #endregion

        #region Events
        public EventHandler SheetAccepted;

        internal void RaiseSheetAccepted() {
            if (this.SheetAccepted != null)
                this.SheetAccepted (this, EventArgs.Empty);
        }

        public EventHandler SheetCanceled;

        internal void RaiseSheetCanceled() {
            if (this.SheetCanceled != null)
                this.SheetCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

그런 다음 `ViewController.cs` 파일을 편집 하 고 `PrepareForSegue` 메서드를 편집 하 여 다음과 같이 만듭니다.

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    case "SheetSegue":
        var sheet = segue.DestinationController as SheetViewController;
        sheet.SheetAccepted += (s, e) => {
            Console.WriteLine ("User Name: {0} Password: {1}", sheet.UserName, sheet.Password);
        };
        sheet.Presentor = this;
        break;
    }
}
```

응용 프로그램을 실행 하 고 시트를 열면 창에 연결 됩니다.

[![](dialog-images/sheet08.png "예제 시트")](dialog-images/sheet08.png#lightbox)

<a name="Creating_a_Preferences_Dialog" />

## <a name="creating-a-preferences-dialog"></a>기본 설정 대화 상자 만들기

Interface Builder에서 기본 설정 보기를 시작 하기 전에 기본 설정 전환을 처리 하는 사용자 지정 segue 형식을 추가 해야 합니다. 프로젝트에 새 클래스를 추가 하 고 호출 `ReplaceViewSeque`합니다. 클래스를 편집 하 고 다음과 같이 만듭니다.

```csharp
using System;
using AppKit;
using Foundation;

namespace MacWindows
{
    [Register("ReplaceViewSeque")]
    public class ReplaceViewSeque : NSStoryboardSegue
    {
        #region Constructors
        public ReplaceViewSeque() {

        }

        public ReplaceViewSeque (string identifier, NSObject sourceController, NSObject destinationController) : base(identifier,sourceController,destinationController) {

        }

        public ReplaceViewSeque (IntPtr handle) : base(handle) {
        }

        public ReplaceViewSeque (NSObjectFlag x) : base(x) {
        }
        #endregion

        #region Override Methods
        public override void Perform ()
        {
            // Cast the source and destination controllers
            var source = SourceController as NSViewController;
            var destination = DestinationController as NSViewController;

            // Is there a source?
            if (source == null) {
                // No, get the current key window
                var window = NSApplication.SharedApplication.KeyWindow;

                // Swap the controllers
                window.ContentViewController = destination;

                // Release memory
                window.ContentViewController?.RemoveFromParentViewController ();
            } else {
                // Swap the controllers
                source.View.Window.ContentViewController = destination;

                // Release memory
                source.RemoveFromParentViewController ();
            }
        
        }
        #endregion

    }

}
```

사용자 지정 segue를 만들면 Xcode의 Interface Builder에 새 창을 추가 하 여 기본 설정을 처리할 수 있습니다.

새 창을 추가 하려면 다음을 수행 합니다.

1. **솔루션 탐색기**에서 Xcode의 Interface Builder에서 `Main.storyboard` 편집할 파일을 엽니다.
2. 새 **창 컨트롤러** 를 Design Surface 끌어 옵니다.

    [![](dialog-images/pref01.png "라이브러리에서 창 컨트롤러 선택")](dialog-images/pref01.png#lightbox)
3. **메뉴 모음** 디자이너 근처에 창을 정렬 합니다.

    [![](dialog-images/pref02.png "새 창 추가")](dialog-images/pref02.png#lightbox)
4. 기본 설정 보기에 탭이 있으므로 연결 된 뷰 컨트롤러의 복사본을 만듭니다.

    [![](dialog-images/pref03.png "필요한 뷰 컨트롤러 추가")](dialog-images/pref03.png#lightbox)
5. **라이브러리**에서 새 **도구 모음 컨트롤러** 를 끌어 옵니다.

    [![](dialog-images/pref04.png "라이브러리에서 도구 모음 컨트롤러 선택")](dialog-images/pref04.png#lightbox)
6. Design Surface 창에 놓습니다.

    [![](dialog-images/pref05.png "새 도구 모음 컨트롤러 추가")](dialog-images/pref05.png#lightbox)
7. 도구 모음의 디자인 레이아웃:

    [![](dialog-images/pref06.png "도구 모음 레이아웃")](dialog-images/pref06.png#lightbox)
8. 컨트롤을 클릭 하 고 각 **도구 모음 단추** 를 위에서 만든 뷰로 끕니다. **사용자 지정** segue 형식 선택:

    [![](dialog-images/pref07.png "Segue 형식 설정")](dialog-images/pref07.png#lightbox)
9. 새 Segue를 선택 하 고 **클래스** 를로 `ReplaceViewSegue`설정 합니다.

    [![](dialog-images/pref08.png "Segue 클래스 설정")](dialog-images/pref08.png#lightbox)
10. Design Surface에서 메뉴 **모음 디자이너** 의 응용 프로그램 메뉴에서 **기본 설정 ...** 을 선택 하 고, 컨트롤을 클릭 한 다음 기본 설정 창으로 끌어서 **Show** segue을 만듭니다.

    [![](dialog-images/pref09.png "Segue 형식 설정")](dialog-images/pref09.png#lightbox)
11. 변경 내용을 저장 하 고 동기화 할 Mac용 Visual Studio로 돌아갑니다.

**응용 프로그램 메뉴**에서 코드를 실행 하 고 **기본 설정 ...** 을 선택 하는 경우 창이 표시 됩니다.

[![](dialog-images/pref10.png "예제 기본 설정 창")](dialog-images/pref10.png#lightbox)

Windows 및 도구 모음 사용에 대 한 자세한 내용은 [windows](~/mac/user-interface/window.md) 및 [도구 모음](~/mac/user-interface/toolbar.md) 설명서를 참조 하세요.

<a name="Saving-and-Loading-Preferences" />

### <a name="saving-and-loading-preferences"></a>기본 설정 저장 및 로드

일반적인 macOS 앱에서 사용자가 앱의 사용자 기본 설정을 변경 하면 해당 변경 내용이 자동으로 저장 됩니다. Xamarin.ios 앱에서이를 처리 하는 가장 쉬운 방법은 모든 사용자의 기본 설정을 관리 하 고 시스템 전체에서 공유 하는 단일 클래스를 만드는 것입니다.

먼저 새 `AppPreferences` 클래스를 프로젝트에 추가 하 고에서 `NSObject`상속 합니다. 기본 설정은 기본 설정 양식을 만들고 유지 관리 하는 프로세스를 수행 하는 [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md) 을 사용 하도록 디자인 됩니다. 기본 설정에 적은 양의 단순 데이터 형식이 구성 되므로 기본 제공 `NSUserDefaults` 를 사용 하 여 값을 저장 하 고 검색 합니다.

`AppPreferences.cs` 파일을 편집 하 여 다음과 같이 만듭니다.

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    [Register("AppPreferences")]
    public class AppPreferences : NSObject
    {
        #region Computed Properties
        [Export("DefaultLanguage")]
        public int DefaultLanguage {
            get { 
                var value = LoadInt ("DefaultLanguage", 0);
                return value; 
            }
            set {
                WillChangeValue ("DefaultLanguage");
                SaveInt ("DefaultLanguage", value, true);
                DidChangeValue ("DefaultLanguage");
            }
        }

        [Export("SmartLinks")]
        public bool SmartLinks {
            get { return LoadBool ("SmartLinks", true); }
            set {
                WillChangeValue ("SmartLinks");
                SaveBool ("SmartLinks", value, true);
                DidChangeValue ("SmartLinks");
            }
        }

        // Define any other required user preferences in the same fashion
        ...

        [Export("EditorBackgroundColor")]
        public NSColor EditorBackgroundColor {
            get { return LoadColor("EditorBackgroundColor", NSColor.White); }
            set {
                WillChangeValue ("EditorBackgroundColor");
                SaveColor ("EditorBackgroundColor", value, true);
                DidChangeValue ("EditorBackgroundColor");
            }
        }
        #endregion

        #region Constructors
        public AppPreferences ()
        {
        }
        #endregion

        #region Public Methods
        public int LoadInt(string key, int defaultValue) {
            // Attempt to read int
            var number = NSUserDefaults.StandardUserDefaults.IntForKey(key);

            // Take action based on value
            if (number == null) {
                return defaultValue;
            } else {
                return (int)number;
            }
        }
            
        public void SaveInt(string key, int value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetInt(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public bool LoadBool(string key, bool defaultValue) {
            // Attempt to read int
            var value = NSUserDefaults.StandardUserDefaults.BoolForKey(key);

            // Take action based on value
            if (value == null) {
                return defaultValue;
            } else {
                return value;
            }
        }

        public void SaveBool(string key, bool value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetBool(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public string NSColorToHexString(NSColor color, bool withAlpha) {
            //Break color into pieces
            nfloat red=0, green=0, blue=0, alpha=0;
            color.GetRgba (out red, out green, out blue, out alpha);

            // Adjust to byte
            alpha *= 255;
            red *= 255;
            green *= 255;
            blue *= 255;

            //With the alpha value?
            if (withAlpha) {
                return String.Format ("#{0:X2}{1:X2}{2:X2}{3:X2}", (int)alpha, (int)red, (int)green, (int)blue);
            } else {
                return String.Format ("#{0:X2}{1:X2}{2:X2}", (int)red, (int)green, (int)blue);
            }
        }

        public NSColor NSColorFromHexString (string hexValue)
        {
            var colorString = hexValue.Replace ("#", "");
            float red, green, blue, alpha;

            // Convert color based on length
            switch (colorString.Length) {
            case 3 : // #RGB
                red = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(0, 1)), 16) / 255f;
                green = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(1, 1)), 16) / 255f;
                blue = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(2, 1)), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 6 : // #RRGGBB
                red = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 8 : // #AARRGGBB
                alpha = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                red = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(6, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, alpha);
            default :
                throw new ArgumentOutOfRangeException(string.Format("Invalid color value '{0}'. It should be a hex value of the form #RBG, #RRGGBB or #AARRGGBB", hexValue));
            }
        }

        public NSColor LoadColor(string key, NSColor defaultValue) {

            // Attempt to read color
            var hex = NSUserDefaults.StandardUserDefaults.StringForKey(key);

            // Take action based on value
            if (hex == null) {
                return defaultValue;
            } else {
                return NSColorFromHexString (hex);
            }
        }

        public void SaveColor(string key, NSColor color, bool sync) {
            // Save to default
            NSUserDefaults.StandardUserDefaults.SetString(NSColorToHexString(color,true), key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }
        #endregion
    }
}
```

이 클래스에는 `SaveInt` `SaveColor` `LoadColor` `LoadInt` 더`NSUserDefaults` 쉽게 작업할 수 있도록,,, 등의 몇 가지 도우미 루틴이 포함 되어 있습니다. `NSColorFromHexString` `NSColorToHexString` `#RRGGBBAA` `AA` 또한에는를 처리 `NSColors`하는 기본 제공 방법이 없으므로 및 메서드를 사용 하 여 색을 웹 기반 16 진수 문자열 (여기서는 알파 투명도)로 변환 하 여 `NSUserDefaults` 쉽게 저장 하 고 검색 합니다.

파일에서 앱 전체에 사용 되는 **apppreferences 설정** 개체의 인스턴스를 만듭니다. `AppDelegate.cs`

```csharp
using AppKit;
using Foundation;
using System.IO;
using System;

namespace SourceWriter
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewWindowNumber { get; set;} = -1;

        public AppPreferences Preferences { get; set; } = new AppPreferences();
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion
        
        ...
```

<a name="Wiring-Preferences-to-Preference-Views" />

### <a name="wiring-preferences-to-preference-views"></a>기본 설정 뷰에 대 한 연결 기본 설정

그런 다음 기본 설정 창 및 위에서 만든 보기의 UI 요소에 기본 설정 클래스를 연결 합니다. Interface Builder에서 기본 설정 보기 컨트롤러를 선택 하 고 **Id 검사자**로 전환 하 여 컨트롤러에 대 한 사용자 지정 클래스를 만듭니다. 

[![](dialog-images/prefs12.png "Identity Inspector")](dialog-images/prefs12.png#lightbox)

Mac용 Visual Studio으로 다시 전환 하 여 변경 내용을 동기화 하 고 새로 만든 클래스를 편집용으로 엽니다. 클래스가 다음과 같이 표시 되도록 합니다.

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class EditorPrefsController : NSViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        [Export("Preferences")]
        public AppPreferences Preferences {
            get { return App.Preferences; }
        }
        #endregion

        #region Constructors
        public EditorPrefsController (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

이 클래스는 다음 두 가지 작업을 수행 합니다. 먼저 AppDelegate에 더 쉽게 액세스할 `App` 수 있도록 도와주는 도우미 속성이 있습니다. 두 번째로 속성 `Preferences` 은이 뷰에 있는 UI 컨트롤을 사용 하 여 데이터 바인딩에 대 한 전역 **apppreferences 설정** 클래스를 노출 합니다.

그런 다음 Interface Builder 스토리 보드 파일을 두 번 클릭 하 여 다시 엽니다 (위에서 변경한 내용 참조). 기본 설정 인터페이스를 빌드하는 데 필요한 모든 UI 컨트롤을 뷰로 끌어 옵니다. 각 컨트롤에 대해 **바인딩 검사자** 로 전환 하 고 **apppreference 설정** 클래스의 개별 속성에 바인딩합니다.

[![](dialog-images/prefs13.png "바인딩 검사기")](dialog-images/prefs13.png#lightbox)

필요한 모든 패널 (컨트롤러 보기) 및 기본 설정 속성에 대해 위의 단계를 반복 합니다.

<a name="Applying-Preference-Changes-to-All-Open-Windows" />

### <a name="applying-preference-changes-to-all-open-windows"></a>열려 있는 모든 창에 기본 설정 변경 내용 적용

위에서 설명한 것 처럼, 일반적인 macOS 앱에서 사용자가 앱의 사용자 기본 설정을 변경 하면 해당 변경 내용이 자동으로 저장 되 고 사용자가 응용 프로그램에서 열려 있는 모든 windows에 적용 됩니다.

앱의 기본 설정 및 windows를 신중 하 게 계획 하 고 설계 하면 최소한의 코딩 작업 만으로이 프로세스가 최종 사용자에 게 원활 하 고 투명 하 게 수행 됩니다.

앱 기본 설정을 사용 하는 모든 창에 대해 다음과 같은 도우미 속성을 해당 콘텐츠 뷰 컨트롤러에 추가 하 여 **AppDelegate** 에 더 쉽게 액세스할 수 있도록 합니다.

```csharp
#region Application Access
public static AppDelegate App {
    get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
}
#endregion
```

다음으로, 클래스를 추가 하 여 사용자의 기본 설정에 따라 내용이 나 동작을 구성 합니다.

```csharp
public void ConfigureEditor() {

    // General Preferences
    TextEditor.AutomaticLinkDetectionEnabled = App.Preferences.SmartLinks;
    TextEditor.AutomaticQuoteSubstitutionEnabled = App.Preferences.SmartQuotes;
    ...

}
``` 

사용자의 기본 설정을 준수 하는지 확인 하기 위해 창이 처음 열릴 때 구성 메서드를 호출 해야 합니다.

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Configure editor from user preferences
    ConfigureEditor ();
    ...
}
```

다음으로 `AppDelegate.cs` 파일을 편집 하 고 다음 메서드를 추가 하 여 열려 있는 모든 창에 기본 설정 변경 내용을 적용 합니다.

```csharp
public void UpdateWindowPreferences() {

    // Process all open windows
    for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
        var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
        if (content != null ) {
            // Reformat all text
            content.ConfigureEditor ();
        }
    }

}
```

다음으로 프로젝트에 `PreferenceWindowDelegate` 클래스를 추가 하 고 다음과 같이 만듭니다.

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class PreferenceWindowDelegate : NSWindowDelegate
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public PreferenceWindowDelegate (NSWindow window)
        {
            // Initialize
            this.Window = window;

        }
        #endregion

        #region Override Methods
        public override bool WindowShouldClose (Foundation.NSObject sender)
        {
            
            // Apply any changes to open windows
            App.UpdateWindowPreferences();

            return true;
        }
        #endregion
    }
}
```

이렇게 하면 기본 설정 창이 닫히면 모든 기본 설정 변경 내용이 열려 있는 모든 창에 전송 됩니다.

마지막으로, 기본 설정 창 컨트롤러를 편집 하 고 위에서 만든 대리자를 추가 합니다.

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class PreferenceWindowController : NSWindowController
    {
        #region Constructors
        public PreferenceWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Initialize
            Window.Delegate = new PreferenceWindowDelegate(Window);
            Toolbar.SelectedItemIdentifier = "General";
        }
        #endregion
    }
}
```

이러한 모든 변경 내용을 적용 하 여 사용자가 앱의 기본 설정을 편집 하 고 기본 설정 창을 닫으면 변경 내용이 열려 있는 모든 창에 적용 됩니다.

[![](dialog-images/prefs14.png "예제 기본 설정 창")](dialog-images/prefs14.png#lightbox)

<a name="The_Open_Dialog" />

## <a name="the-open-dialog"></a>열기 대화 상자

열기 대화 상자를 사용 하면 응용 프로그램에서 항목을 찾고 열 수 있는 일관 된 방법을 사용자에 게 제공 합니다. Xamarin.ios 응용 프로그램에서 열려 있는 대화 상자를 표시 하려면 다음 코드를 사용 합니다.

```csharp
var dlg = NSOpenPanel.OpenPanel;
dlg.CanChooseFiles = true;
dlg.CanChooseDirectories = false;
dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

if (dlg.RunModal () == 1) {
    // Nab the first file
    var url = dlg.Urls [0];

    if (url != null) {
        var path = url.Path;

        // Create a new window to hold the text
        var newWindowController = new MainWindowController ();
        newWindowController.Window.MakeKeyAndOrderFront (this);

        // Load the text into the window
        var window = newWindowController.Window as MainWindow;
        window.Text = File.ReadAllText(path);
        window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
        window.RepresentedUrl = url;

    }
}
```

위의 코드에서 파일의 내용을 표시 하는 새 문서 창을 열고 있습니다. 응용 프로그램에 필요한 기능으로이 코드를 바꾸어야 합니다.

로 작업할 때 사용할 수 있는 속성은 `NSOpenPanel`다음과 같습니다.

- **CanChooseFiles** - `true` 사용자가 파일을 선택할 수 있습니다.
- **CanChooseDirectories** -사용자 `true` 가 디렉터리를 선택할 수 있는 경우
- **AllowsMultipleSelection** - `true` 사용자가 한 번에 둘 이상의 파일을 선택할 수 있습니다.
- **Resolvealiases** -및 `true` 별칭을 선택 하는 경우 원본 파일의 경로를 확인 합니다.
- **AllowedFileTypes** -사용자가 확장 또는 _UTI_으로 선택할 수 있는 파일 형식의 문자열 배열입니다. 기본값은입니다. `null`이 경우 모든 파일을 열 수 있습니다.

메서드 `RunModal ()` 는 열기 대화 상자를 표시 하 고 사용자가 속성에 지정 된 대로 파일 또는 디렉터리를 선택 하 고 사용자 `1` 가 **열기** 단추를 클릭 하면를 반환 합니다.

열기 대화 상자에서는 사용자가 선택한 파일이 나 디렉터리를 `URL` 속성의 url 배열로 반환 합니다.

프로그램을 실행 하 고 **파일** 메뉴에서 **열기 ...** 항목을 선택 하면 다음이 표시 됩니다. 

[![](dialog-images/dialog03.png "열기 대화 상자")](dialog-images/dialog03.png#lightbox)

<a name="The_Print_and_Page_Setup_Dialogs" />

## <a name="the-print-and-page-setup-dialogs"></a>인쇄 및 페이지 설정 대화 상자

macOS는 사용자가 사용 하는 모든 응용 프로그램에서 일관 된 인쇄 환경을 제공할 수 있도록 응용 프로그램에서 표시할 수 있는 표준 인쇄 및 페이지 설정 대화 상자를 제공 합니다.

다음 코드는 표준 인쇄 대화 상자를 표시 합니다.

```csharp
public bool ShowPrintAsSheet { get; set;} = true;
...

[Export ("showPrinter:")]
void ShowDocument (NSObject sender) {
    var dlg = new NSPrintPanel();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet(new NSPrintInfo(),this,this,null,new IntPtr());
    } else {
        if (dlg.RunModalWithPrintInfo(new NSPrintInfo()) == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}

```

`ShowPrintAsSheet` 속성을로 `false`설정 하는 경우 응용 프로그램을 실행 하 고 인쇄 대화 상자를 표시 하면 다음과 같은 메시지가 표시 됩니다.

[![](dialog-images/print01.png "인쇄 대화 상자")](dialog-images/print01.png#lightbox)

`ShowPrintAsSheet` 속성을로 `true`설정 하는 경우 응용 프로그램을 실행 하 고 인쇄 대화 상자를 표시 하면 다음과 같은 메시지가 표시 됩니다.

[![](dialog-images/print02.png "인쇄 시트")](dialog-images/print02.png#lightbox)

다음 코드에서는 페이지 레이아웃 대화 상자를 표시 합니다.

```csharp
[Export ("showLayout:")]
void ShowLayout (NSObject sender) {
    var dlg = new NSPageLayout();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet (new NSPrintInfo (), this);
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}
```

`ShowPrintAsSheet` 속성을로 `false`설정 하는 경우 응용 프로그램을 실행 하 고 인쇄 레이아웃 대화 상자를 표시 하면 다음과 같은 메시지가 표시 됩니다.

[![](dialog-images/print03.png "페이지 설정 대화 상자")](dialog-images/print03.png#lightbox)

`ShowPrintAsSheet` 속성을로 `true`설정 하는 경우 응용 프로그램을 실행 하 고 인쇄 레이아웃 대화 상자를 표시 하면 다음과 같은 메시지가 표시 됩니다.

[![](dialog-images/print04.png "페이지 설정 시트")](dialog-images/print04.png#lightbox)

인쇄 및 페이지 설정 대화 상자를 사용 하는 방법에 대 한 자세한 내용은 Apple의 [NSPrintPanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPrintPanel_Class/index.html#//apple_ref/doc/uid/TP40004092), [NSPageLayout](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPageLayout_Class/index.html#//apple_ref/doc/uid/TP40004080) 및 [인쇄 설명서 소개](http://sdg.mesonet.org/people/brad/XCode3/Documentation/DocSets/com.apple.adc.documentation.AppleSnowLeopard.CoreReference.docset/Contents/Resources/Documents/#documentation/Cocoa/Conceptual/Printing/Printing.html#//apple_ref/doc/uid/10000083-SW1) 를 참조 하세요.

<a name="The_Save_Dialog" />

## <a name="the-save-dialog"></a>저장 대화 상자

저장 대화 상자는 사용자에 게 응용 프로그램에 항목을 저장 하는 일관 된 방법을 제공 합니다.

다음 코드는 표준 저장 대화 상자를 표시 합니다.

```csharp
public bool ShowSaveAsSheet { get; set;} = true;
...

[Export("saveDocumentAs:")]
void ShowSaveAs (NSObject sender)
{
    var dlg = new NSSavePanel ();
    dlg.Title = "Save Text File";
    dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

    if (ShowSaveAsSheet) {
        dlg.BeginSheet(mainWindowController.Window,(result) => {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        });
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        }
    }

}
```

`AllowedFileTypes` 속성은 파일을 저장 하기 위해 사용자가 선택할 수 있는 파일 형식의 문자열 배열입니다. 파일 형식은 확장 또는 _UTI_지정할 수 있습니다. 기본값은입니다. `null`이 경우 모든 파일 형식을 사용할 수 있습니다.

`ShowSaveAsSheet` 속성을 `false`로 설정 하는 경우 응용 프로그램을 실행 하 고 **파일** 메뉴에서 다른 **이름으로 저장** ...을 선택 하면 다음이 표시 됩니다.

[![](dialog-images/save01.png "저장 대화 상자")](dialog-images/save01.png#lightbox)

사용자는 대화 상자를 확장할 수 있습니다.

[![](dialog-images/save02.png "확장 된 저장 대화 상자")](dialog-images/save02.png#lightbox)

`ShowSaveAsSheet` 속성을 `true`로 설정 하는 경우 응용 프로그램을 실행 하 고 **파일** 메뉴에서 다른 **이름으로 저장** ...을 선택 하면 다음이 표시 됩니다.

[![](dialog-images/save03.png "저장 시트")](dialog-images/save03.png#lightbox)

사용자는 대화 상자를 확장할 수 있습니다.

[![](dialog-images/save04.png "확장 된 저장 시트")](dialog-images/save04.png#lightbox)

저장 대화 상자를 사용 하는 방법에 대 한 자세한 내용은 Apple의 [Nssavepanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSSavePanel_Class/index.html#//apple_ref/doc/uid/TP40004098) 설명서를 참조 하세요.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 응용 프로그램에서 모달 창, 시트 및 표준 시스템 대화 상자를 사용 하는 방법을 자세히 살펴봅니다. Xcode의 Interface Builder에서 모달 창 및 시트를 만들고 유지 관리 하는 방법 및 코드에서 C# 모달 창, 시트 및 대화 상자를 사용 하는 방법에 대 한 다양 한 형식 및 사용 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [MacWindows (샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/macwindows)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [메뉴](~/mac/user-interface/menu.md)
- [Windows](~/mac/user-interface/window.md)
- [도구 모음](~/mac/user-interface/toolbar.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [시트 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Sheets/Sheets.html#//apple_ref/doc/uid/10000002i)
- [인쇄 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Printing/osxp_aboutprinting/osxp_aboutprt.html)
