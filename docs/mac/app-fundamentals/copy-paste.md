---
title: Xamarin.Mac에서 복사 및 붙여넣기
description: 이 문서는 복사를 제공 하 여 Xamarin.Mac 응용 프로그램에 붙여 임시 보드를 사용 하 여 작업을 다룹니다. 작업 하는 방법을 보여 줍니다 표준 데이터 형식에 지정된 된 앱 내의 사용자 지정 데이터를 지원 하는 방법과 여러 앱 간에 공유할 수 있습니다.
ms.prod: xamarin
ms.assetid: 7E9C99FB-B7B4-4C48-B20F-84CB48543083
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 728e0264f7da8f3adfef360dd473772dd7e28a11
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "40251264"
---
# <a name="copy-and-paste-in-xamarinmac"></a>Xamarin.Mac에서 복사 및 붙여넣기

_이 문서는 복사를 제공 하 여 Xamarin.Mac 응용 프로그램에 붙여 임시 보드를 사용 하 여 작업을 다룹니다. 작업 하는 방법을 보여 줍니다 표준 데이터 형식에 지정된 된 앱 내의 사용자 지정 데이터를 지원 하는 방법과 여러 앱 간에 공유할 수 있습니다._

## <a name="overview"></a>개요

Xamarin.Mac 응용 프로그램에서 C# 및.NET을 사용 하 여 작업을 하는 경우 동일한 임시 보드 (복사 및 붙여넣기) 지원 서비스는 Objective-c에서 작업 하는 개발자가 액세스할을 수 있습니다.

이 문서에서는 주제 지 Xamarin.Mac 앱에서 사용 하는 두 가지 주요 방법:

1. **표준 데이터 형식을** -모두 앱을 지 원하는 다른 데이터 형식을 알고 임시 보드 작업은 두 개의 관련 없는 앱 간에 수행 일반적으로, 때문입니다. 공유에 대 한 가능성을 최대화 하려면 지 (공용 데이터 형식의 표준 집합을 사용 하 여) 지정된 된 항목의 여러 표현 보유할 수 있는,이 해당 요구 사항에 가장 적합 한 버전을 선택 하려면 사용 중인 앱을 허용 합니다.
2. **사용자 지정 데이터** -복사 및 붙여 넣는 지에서 처리 되는 사용자 지정 데이터 형식을 정의할 수 있습니다 하 여 Xamarin.Mac 내에서 복잡 한 데이터를 지원 합니다. 예를 들어, 벡터 드로잉 앱을 허용 하는 사용자를 여러 데이터 형식 및 지점으로 구성 되는 복잡 한 셰이프 복사 및 붙여넣기 합니다.

[![실행 중인 앱의 예](copy-paste-images/intro01.png "실행 중인 앱의 예")](copy-paste-images/intro01-large.png#lightbox)

이 문서에서는 Xamarin.Mac 응용 프로그램을 지원 복사 및 붙여넣기 작업이에 지 작업의 기본 사항을 설명 합니다. 것이 가장 좋습니다를 통해 작업 하는 합니다 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서 합니다 [Xcode 및 Interface Builder 소개](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 하 고 [출 선 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션으로 주요 개념 및이 문서를 사용 하는 기술을 설명 합니다.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명도 문서는 `Register` 및 `Export` 특성 요소 Objective-C 개체 및 UI에 C# 클래스를 연결 하는 데 사용 합니다.

## <a name="getting-started-with-the-pasteboard"></a>임시 보드 시작

임시 보드 지정된 된 응용 프로그램 내에서 또는 응용 프로그램 간에 데이터를 교환 하기 위한 표준화 된 메커니즘을 제공 합니다. Xamarin.Mac 응용 프로그램에 대 한 지에 대 한 일반적인 사용은 처리 복사 및 붙여넣기 작업을 인데 다양 한 다른 작업 (예: 끌어서 놓기 및 응용 프로그램 서비스)도 지원 됩니다.

빠르게 순조로운 액세스할, 대지의 오브젝트를 사용 하 여 Xamarin.Mac 앱에서 실제로 간단한 소개를 시작 하려고 합니다. 나중에 지의 작동 방식 및 사용 된 메서드를 대 한 자세한 내용이 제공 됩니다.

이 예에서 우리가 만들 이미지 뷰를 포함 하는 창을 관리 하는 간단한 문서 기반 응용 프로그램을 합니다. 사용자는 복사 하 고 앱에서 및를 또는 다른 앱 또는 동일한 앱 내의 여러 windows에서 문서 간에 이미지를 붙여넣을 수 됩니다.

### <a name="creating-the-xamarin-project"></a>Xamarin 프로젝트 만들기

먼저 하겠습니다는 수 복사본을 추가 하 고 붙여 지원에 대 한 새 기반 문서 Xamarin.Mac 앱을 만듭니다.

다음을 수행합니다.

1. Mac 및 클릭에 대 한 Visual Studio를 시작 합니다 **새 프로젝트...**  링크 합니다.
2. 선택 **Mac** > **앱** > **Cocoa 앱**를 클릭 합니다 **다음** 단추: 

    [![새 Cocoa 앱 프로젝트를 만드는](copy-paste-images/sample01.png "새 Cocoa 앱 프로젝트 만들기")](copy-paste-images/sample01-large.png#lightbox)
3. 입력 `MacCopyPaste` 에 대 한 합니다 **프로젝트 이름** 하 고 나머지는 기본값을 유지 합니다. 다음을 클릭 합니다. 

    [![프로젝트의 이름을 설정할](copy-paste-images/sample01a.png "프로젝트의 이름을 설정 합니다.")](copy-paste-images/sample01a-large.png#lightbox)

4. 클릭 합니다 **만들기** 단추: 

    [![새 프로젝트 설정 확인](copy-paste-images/sample02.png "새 프로젝트 설정 확인")](copy-paste-images/sample02-large.png#lightbox)

### <a name="add-an-nsdocument"></a>NSDocument를 추가 합니다.

다음으로 추가 사용자 지정 `NSDocument` 응용 프로그램의 사용자 인터페이스에 대 한 백그라운드 저장소로 사용할 클래스를 합니다. 단일 이미지 뷰를 포함 하 고 기본 임시 보드는 보기에서 이미지를 복사 하는 방법과 기본 임시 보드에서 이미지를 가져오고 이미지 보기에 표시 하는 방법을 알고 있어야 합니다.

Xamarin.Mac 프로젝트를 마우스 오른쪽 단추로 클릭 합니다 **Solution Pad** 선택한 **추가** > **새 파일...** :

![NSDocument를 프로젝트에 추가](copy-paste-images/sample03.png "NSDocument를 프로젝트에 추가")

**이름**에 대해 `ImageDocument`를 입력하고 **새로 만들기** 단추를 클릭합니다. 편집 된 **ImageDocument.cs** 클래스 및 다음과 같이 표시 되도록 합니다.

```csharp
using System;
using AppKit;
using Foundation;
using ObjCRuntime;

namespace MacCopyPaste
{
    [Register("ImageDocument")]
    public class ImageDocument : NSDocument
    {
        #region Computed Properties
        public NSImageView ImageView {get; set;}

        public ImageInfo Info { get; set; } = new ImageInfo();

        public bool ImageAvailableOnPasteboard {
            get {
                // Initialize the pasteboard
                NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
                Class [] classArray  = { new Class ("NSImage") };

                // Check to see if an image is on the pasteboard
                return pasteboard.CanReadObjectForClasses (classArray, null);
            }
        }
        #endregion

        #region Constructor
        public ImageDocument ()
        {
        }
        #endregion

        #region Public Methods
        [Export("CopyImage:")]
        public void CopyImage(NSObject sender) {

            // Grab the current image
            var image = ImageView.Image;

            // Anything to process?
            if (image != null) {
                // Get the standard pasteboard
                var pasteboard = NSPasteboard.GeneralPasteboard;

                // Empty the current contents
                pasteboard.ClearContents();

                // Add the current image to the pasteboard
                pasteboard.WriteObjects (new NSImage[] {image});

                // Save the custom data class to the pastebaord
                pasteboard.WriteObjects (new ImageInfo[] { Info });

                // Using a Pasteboard Item
                NSPasteboardItem item = new NSPasteboardItem();
                string[] writableTypes = {"public.text"};

                // Add a data provier to the item
                ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
                var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

                // Save to pasteboard
                if (ok) {
                    pasteboard.WriteObjects (new NSPasteboardItem[] { item });
                }
            }

        }

        [Export("PasteImage:")]
        public void PasteImage(NSObject sender) {

            // Initialize the pasteboard
            NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
            Class [] classArray  = { new Class ("NSImage") };

            bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
            if (ok) {
                // Read the image off of the pasteboard
                NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
                NSImage image = (NSImage)objectsToPaste[0];

                // Display the new image
                ImageView.Image = image;
            }

            Class [] classArray2 = { new Class ("ImageInfo") };
            ok = pasteboard.CanReadObjectForClasses (classArray2, null);
            if (ok) {
                // Read the image off of the pasteboard
                NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
                ImageInfo info = (ImageInfo)objectsToPaste[0];

            }

        }
        #endregion
    }
}
```

아래에서 자세히 코드 중 일부에 대해를 살펴보겠습니다.

다음 코드는 이미지를 사용할 수 있으면 기본 지에서 이미지 데이터의 존재 여부를 테스트 하는 속성이 `true` 그렇지 않으면 반환 됩니다 `false`:

```csharp
public bool ImageAvailableOnPasteboard {
    get {
        // Initialize the pasteboard
        NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
        Class [] classArray  = { new Class ("NSImage") };

        // Check to see if an image is on the pasteboard
        return pasteboard.CanReadObjectForClasses (classArray, null);
    }
}
```

다음 코드는 기본 지에 첨부 된 이미지 보기에서 이미지를 복사합니다.

```csharp
[Export("CopyImage:")]
public void CopyImage(NSObject sender) {

    // Grab the current image
    var image = ImageView.Image;

    // Anything to process?
    if (image != null) {
        // Get the standard pasteboard
        var pasteboard = NSPasteboard.GeneralPasteboard;

        // Empty the current contents
        pasteboard.ClearContents();

        // Add the current image to the pasteboard
        pasteboard.WriteObjects (new NSImage[] {image});

        // Save the custom data class to the pastebaord
        pasteboard.WriteObjects (new ImageInfo[] { Info });

        // Using a Pasteboard Item
        NSPasteboardItem item = new NSPasteboardItem();
        string[] writableTypes = {"public.text"};

        // Add a data provider to the item
        ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
        var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

        // Save to pasteboard
        if (ok) {
            pasteboard.WriteObjects (new NSPasteboardItem[] { item });
        }
    }

}
```

및 다음 코드를 기본 지에서 이미지를 붙여넣고 (올바른 이미지를 포함 하는 지) 하는 경우 연결 된 이미지 보기에 표시 합니다.

```csharp
[Export("PasteImage:")]
public void PasteImage(NSObject sender) {

    // Initialize the pasteboard
    NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
    Class [] classArray  = { new Class ("NSImage") };

    bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
        NSImage image = (NSImage)objectsToPaste[0];

        // Display the new image
        ImageView.Image = image;
    }

    Class [] classArray2 = { new Class ("ImageInfo") };
    ok = pasteboard.CanReadObjectForClasses (classArray2, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
        ImageInfo info = (ImageInfo)objectsToPaste[0]
    }
}
```

이 문서의 사용 하 여 Xamarin.Mac 앱의 사용자 인터페이스를 만들겠습니다.

### <a name="building-the-user-interface"></a>사용자 인터페이스 구축

두 번 클릭 합니다 **Main.storyboard** 파일 Xcode에서 엽니다. 다음으로, 도구 모음 및 이미지를도 추가 하 고 같이 구성 합니다.

[![도구 모음 편집](copy-paste-images/sample04.png "도구 모음 편집")](copy-paste-images/sample04-large.png#lightbox)

복사본을 추가 하 고 붙여넣습니다 **도구 모음 항목 이미지** 도구 모음의 왼쪽에 있습니다. 사용할 것으로 바로 가기 편집 메뉴에서 복사 및 붙여넣기를 합니다. 다음으로, 4 개를 추가 **도구 모음 항목 이미지** 도구 모음의 오른쪽에 있습니다. 이러한도 몇 가지 기본 이미지를 사용 하 여 이미지를 채우는 데 사용 하겠습니다.

작업 도구 모음에 대 한 자세한 내용은 참조 하십시오 우리의 [도구 모음](~/mac/user-interface/toolbar.md) 설명서.

다음으로, 보겠습니다 다음 출 선 및 영구적인 도구 모음 항목 및 이미지에 대 한 작업을 잘 노출 합니다.

[![출 선 및 작업 만들기](copy-paste-images/sample05.png "출 선 및 작업 만들기")](copy-paste-images/sample05-large.png#lightbox)

출 선 및 작업을 사용 하 여 작업에 대 한 자세한 내용은 참조 하십시오 합니다 [출 선 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 부분 우리의 [Hello, Mac](~/mac/get-started/hello-mac.md) 설명서.

### <a name="enabling-the-user-interface"></a>사용자 인터페이스를 사용 하도록 설정

Xcode 및 출 선 및 작업을 통해 노출 되는 UI 요소에서 만든 사용자 인터페이스를 사용 하 여 UI를 사용 하도록 설정 하는 코드를 추가 해야 합니다. 두 번 클릭 합니다 **ImageWindow.cs** 파일을 **Solution Pad** 수 있으며 다음과 같은 모양:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacCopyPaste
{
    public partial class ImageWindow : NSWindow
    {
        #region Private Variables
        ImageDocument document;
        #endregion

        #region Computed Properties
        [Export ("Document")]
        public ImageDocument Document {
            get {
                return document;
            }
            set {
                WillChangeValue ("Document");
                document = value;
                DidChangeValue ("Document");
            }
        }

        public ViewController ImageViewController {
            get { return ContentViewController as ViewController; }
        }

        public NSImage Image {
            get {
                return ImageViewController.Image;
            }
            set {
                ImageViewController.Image = value;
            }
        }
        #endregion

        #region Constructor
        public ImageWindow (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Create a new document instance
            Document = new ImageDocument ();

            // Attach to image view
            Document.ImageView = ImageViewController.ContentView;
        }
        #endregion

        #region Public Methods
        public void CopyImage (NSObject sender)
        {
            Document.CopyImage (sender);
        }

        public void PasteImage (NSObject sender)
        {
            Document.PasteImage (sender);
        }

        public void ImageOne (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image01.jpg");

            // Set image info
            Document.Info.Name = "city";
            Document.Info.ImageType = "jpg";
        }

        public void ImageTwo (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image02.jpg");

            // Set image info
            Document.Info.Name = "theater";
            Document.Info.ImageType = "jpg";
        }

        public void ImageThree (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image03.jpg");

            // Set image info
            Document.Info.Name = "keyboard";
            Document.Info.ImageType = "jpg";
        }

        public void ImageFour (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image04.jpg");

            // Set image info
            Document.Info.Name = "trees";
            Document.Info.ImageType = "jpg";
        }
        #endregion
    }
}
```

이 코드 아래에 자세히 살펴보겠습니다.

인스턴스를 위해 노출 먼저는 `ImageDocument` 위에서 만든 클래스:

```csharp
private ImageDocument _document;
...

[Export ("Document")]
public ImageDocument Document {
    get { return _document; }
    set {
        WillChangeValue ("Document");
        _document = value;
        DidChangeValue ("Document");
    }
}
```

사용 하 여 `Export`, `WillChangeValue` 및 `DidChangeValue`를 설치 했으므로 `Document` 키-값 코딩 및 Xcode에서 데이터 바인딩을 허용 하는 속성입니다.

Xcode에서 UI에 다음 속성을 사용 하 여 추가한 잘 이미지에서 이미지도 노출 합니다.

```csharp
public ViewController ImageViewController {
    get { return ContentViewController as ViewController; }
}

public NSImage Image {
    get {
        return ImageViewController.Image;
    }
    set {
        ImageViewController.Image = value;
    }
}
```

인스턴스를 만들 때 주 창 로드 하 고 표시 되는 경우이 `ImageDocument` 클래스 및 연결 UI의 이미지도 다음 코드를 사용 하 여:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create a new document instance
    Document = new ImageDocument ();

    // Attach to image view
    Document.ImageView = ImageViewController.ContentView;
}
```

마지막으로, 복사 및 붙여넣기 도구 모음 항목을 클릭 하 여 사용자에 게 응답 호출의 인스턴스는 `ImageDocument` 실제 작업을 수행 하는 클래스:

```csharp
partial void CopyImage (NSObject sender) {
    Document.CopyImage(sender);
}

partial void PasteImage (Foundation.NSObject sender) {
    Document.PasteImage(sender);
}
```

### <a name="enabling-the-file-and-edit-menus"></a>파일 및 편집 메뉴를 사용 하도록 설정

해야 할 마지막 가장을 사용 하도록 설정 됩니다는 **새로 만들기** 메뉴 항목을는 **파일** 메뉴 (주 창의 새 인스턴스를 만들려면)를 사용 하도록 설정 합니다 **잘라내기**, **복사**  하 고 **붙여넣기** 메뉴에서 항목을 **편집** 메뉴.

사용 하도록 설정 하려면 합니다 **새로 만들기** 메뉴 항목을 편집 합니다 **AppDelegate.cs** 파일과 다음 코드를 추가:

```csharp
public int UntitledWindowCount { get; set;} =1;
...

[Export ("newDocument:")]
void NewDocument (NSObject sender) {
    // Get new window
    var storyboard = NSStoryboard.FromName ("Main", null);
    var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

    // Display
    controller.ShowWindow(this);

    // Set the title
    controller.Window.Title = (++UntitledWindowCount == 1) ? "untitled" : string.Format ("untitled {0}", UntitledWindowCount);
}
```

자세한 내용은 참조 하십시오는 [여러 Windows를 사용 하 여 작업](~/mac/user-interface/window.md) 부분 우리의 [Windows](~/mac/user-interface/window.md) 설명서.

사용 하도록 설정 합니다 **잘라내기**, **복사** 및 **붙여넣기** 메뉴 항목을 편집 합니다 **AppDelegate.cs** 파일과 다음 코드를 추가:

```csharp
[Export("copy:")]
void CopyImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Copy the image to the clipboard
    window.Document.CopyImage (sender);
}

[Export("cut:")]
void CutImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Copy the image to the clipboard
    window.Document.CopyImage (sender);

    // Clear the existing image
    window.Image = null;
}

[Export("paste:")]
void PasteImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Paste the image from the clipboard
    window.Document.PasteImage (sender);
}
```

각 메뉴 항목에 대 한 우리는 현재 키의 최상위 창을 가져오고 캐스트는 `ImageWindow` 클래스:

```csharp
var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
```

여기에서 호출 된 `ImageDocument` 처리 복사 및 붙여넣기 작업이 해당 창의 클래스 인스턴스. 예를 들어: 

```csharp
window.Document.CopyImage (sender);
```

만 포함 하려고 **잘라내기**, **복사** 하 고 **붙여넣기** 없을 경우 액세스할 수 있도록 메뉴 항목 이미지 데이터를 기본 지 또는 현재 활성 창도 이미지입니다.

추가 해 보겠습니다를 **EditMenuDelegate.cs** Xamarin.Mac 프로젝트에 파일을 다음과 같이 표시 되도록 합니다.

```csharp
using System;
using AppKit;

namespace MacCopyPaste
{
    public class EditMenuDelegate : NSMenuDelegate
    {
        #region Override Methods
        public override void MenuWillHighlightItem (NSMenu menu, NSMenuItem item)
        {
        }

        public override void NeedsUpdate (NSMenu menu)
        {
            // Get list of menu items
            NSMenuItem[] Items = menu.ItemArray ();

            // Get the key window and determine if the required images are available
            var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
            var hasImage = (window != null) && (window.Image != null);
            var hasImageOnPasteboard = (window != null) && window.Document.ImageAvailableOnPasteboard;

            // Process every item in the menu
            foreach(NSMenuItem item in Items) {
                // Take action based on the menu title
                switch (item.Title) {
                case "Cut":
                case "Copy":
                case "Delete":
                    // Only enable if there is an image in the view
                    item.Enabled = hasImage;
                    break;
                case "Paste":
                    // Only enable if there is an image on the pasteboard
                    item.Enabled = hasImageOnPasteboard;
                    break;
                default:
                    // Only enable the item if it has a sub menu
                    item.Enabled = item.HasSubmenu;
                    break;
                }
            }
        }
        #endregion
    }
}
```

마찬가지로 현재, 최상위 창 가져오기 하 고 사용 하 여 해당 `ImageDocument` 클래스 인스턴스를 필요한 이미지 데이터가 있는지 확인 합니다. 사용 된 `MenuWillHighlightItem` 이 상태를 기반으로 한 메서드를 사용 하도록 설정 하거나 각 항목을 사용 하지 않도록 설정 합니다.

편집 합니다 **AppDelegate.cs** 파일을 확인 합니다 `DidFinishLaunching` 다음과 같은 메서드 확인:
 
```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Disable automatic item enabling on the Edit menu
    EditMenu.AutoEnablesItems = false;
    EditMenu.Delegate = new EditMenuDelegate ();
}
```

첫째, 자동 및 해제 편집 메뉴에서 메뉴 항목의 비활성화 합니다. 인스턴스에 연결 어는 `EditMenuDelegate` 위에서 만든는 클래스입니다.

자세한 내용은 참조 하십시오 우리의 [메뉴](~/mac/user-interface/menu.md) 설명서.

### <a name="testing-the-app"></a>앱 테스트

곳에서 모든 항목을 사용 하 여 응용 프로그램을 테스트할 준비가 됩니다. 빌드 및 앱을 실행 하 고 기본 인터페이스 표시 됩니다.

![응용 프로그램을 실행](copy-paste-images/run01.png "응용 프로그램 실행")

편집 메뉴를 연 경우 유의 **잘라내기**, **복사** 및 **붙여넣기** 잘 또는 기본 지 이미지에 이미지가 없는 있기 때문에 사용할 수 없습니다.

![편집 메뉴를 열고](copy-paste-images/run02.png "편집 메뉴 열기")

편집 메뉴를 닫고도 이미지에 이미지를 추가 하는 경우 항목을 이제 사용할 수 있습니다.

![사용 하도록 설정 된 편집 메뉴 항목을 보여 주는](copy-paste-images/run03.png "편집 메뉴 항목을 표시를 사용할 수 있습니다.")

이미지를 복사 하 고 선택 하는 경우 **새로 만들기** 파일 메뉴에서 새 창에 해당 이미지를 붙여넣을 수 있습니다.

![새 창에 이미지를 넣는](copy-paste-images/run04.png "이미지를 새 창에 붙여 넣는 방법")

다음 섹션에서는 Xamarin.Mac 응용 프로그램에 지 작업에 대해 자세하게 알아보겠습니다.

## <a name="about-the-pasteboard"></a>임시 보드에 대 한

MacOS (OS X 이전의 함)의 지 (`NSPasteboard`) 예: 복사 및 붙여넣기, 끌어서 놓기 및 응용 프로그램 서비스를 처리 하는 여러 서버에 대 한 지원을 제공 합니다. 다음 섹션에서는 몇 가지 주요 임시 보드 개념에 대해 좀 더 자세히 알아보겠습니다.

### <a name="what-is-a-pasteboard"></a>지에서는 란?

`NSPasteboard` 클래스 또는 지정된 된 앱 내의 응용 프로그램 간에 정보를 교환 하기 위한 표준화 된 메커니즘을 제공 합니다. 복사 및 붙여넣기 작업을 처리 하는 것에 대 한 지에서는의 main 함수는:

1. 사용자 앱에서 항목을 선택 하 고 사용 하는 경우는 **잘라내기** 하거나 **복사** 메뉴 항목을 선택한 항목의 하나 이상의 표현을 지에 배치 됩니다.
2. 사용 하는 경우는 **붙여넣기** 메뉴 항목 (내에서 동일한 앱 또는 다른) 처리할 수 있는 데이터의 버전은 지에서 복사 및 앱에 추가 합니다.

응용 프로그램 서비스 작업 및 찾기, 끌어서, 끌어서 놓기 임시 보드는 덜 명확 포함 합니다.

- 사용자가 끌기 작업을 시작 하는 경우 끌기 데이터를 임시 보드에 복사 됩니다. 다른 앱으로 드롭다운을 사용 하 여 끌기 작업이 종료 되는 경우 해당 앱 지에서 데이터를 복사 합니다.
- 번역 서비스를 요청 하는 앱에서 데이터를 변환할 수는 지에 복사 됩니다. 응용 프로그램 서비스를 대 지에서 데이터를 검색, 임시 보드에 클립보드에서 데이터를 다시 번역 하는 합니다.

가장 간단한 형태로 대지의 오브젝트에 지정된 된 앱 내에서 또는 앱 간에 데이터를 이동 하 고 따라서 앱의 프로세스 외부에서 특별 한 글로벌 메모리 영역에 사용 됩니다. 대지의 오브젝트의 개념은 쉽게 grasps를 고려해 야 할 몇 가지 더 복잡 한 세부 정보가 있습니다. 아래에서 자세히 설명 합니다.

### <a name="named-pasteboards"></a>명명 된 대지의 오브젝트

지에서는 공용 또는 개인 수 있으며 다양 한 용도로 응용 프로그램 내에서 또는 여러 앱 간에 사용할 수 있습니다. macOS에서는 각각 특정 한 잘 정의 된 사용량을 사용 하 여 여러 표준 대지의 오브젝트를 제공합니다.

- `NSGeneralPboard` -에 대 한 기본 지 **잘라내기**를 **복사본** 하 고 **붙여넣기** 작업 합니다.
- `NSRulerPboard` -지원 **잘라내기**를 **복사** 하 고 **붙여넣기** 연산은 **눈금자**합니다.
- `NSFontPboard` -지원 **잘라내기**, **복사** 하 고 **붙여넣기** 연산은 `NSFont` 개체입니다.
- `NSFindPboard` -지원 응용 프로그램별 검색 텍스트를 공유할 수 있는 패널을 찾습니다.
- `NSDragPboard` -지원 **끌어서 놓기** 작업 합니다.

대부분의 경우에 대 한 시스템 정의 대지의 오브젝트 중 하나를 사용 합니다. 하지만 사용자 고유의 대지의 오브젝트를 만들 수는 경우가 있을 수 있습니다. 이러한 상황에서 사용할 수 있습니다 합니다 `FromName (string name)` 메서드는 `NSPasteboard` 지정 된 이름의 사용자 지정 지에서는 만들 클래스입니다.

필요에 따라 호출할 수 있습니다는 `CreateWithUniqueName` 메서드를 `NSPasteboard` 를 고유 하 게 명명 된 임시 보드를 만들 클래스.

### <a name="pasteboard-items"></a>임시 보드 항목

응용 프로그램을 임시 보드에 기록 하는 데이터의 각 부분으로 간주 됩니다는 _임시 보드 항목_ 지에서는 동시에 여러 항목을 저장할 수 있습니다. 이러한 방식으로 앱 지 (예: 일반 텍스트 및 서식 있는 텍스트)에서는 및 검색 앱 복사 되는 데이터의 여러 버전 데이터만 해제 읽을 수 있습니다 (예: 일반 텍스트만) 처리할 수 있도록 작성할 수 있습니다.

### <a name="data-representations-and-uniform-type-identifiers"></a>데이터 표현 및 균일 한 형식 식별자

임시 보드 작업에는 일반적으로 두 개의 (또는 이상) 간에 응용 프로그램을 서로 또는 데이터 형식에 대 한 지식이 없는 수행 각 처리할 수 있습니다. 위의 섹션에서 설명 했 듯이 정보를 공유 하는 것에 대 한 잠재력을 극대화할 수 지에서는 보유할 수 복사 되 고 붙여 넣은 데이터의 여러 표현 합니다.

각 표현을 통해를 Uniform 형식 식별자 (UTI)에 제공 되는 날짜의 형식을 고유 하 게 식별 하는 간단한 문자열에 지나지 인 식별 됩니다 (자세한 내용은 Apple의를 참조 하십시오 [Uniform 형식 식별자 개요 ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) 설명서). 

사용자 지정 데이터 형식 (예를 들어, 앱을 그리기는 벡터 그리기 개체)를 만드는 경우 사용자 고유의 UTI를 고유 하 게 복사에서 쉽게 식별할 붙여넣기 작업을 만들 수 있습니다.

앱을 임시 보드에서 복사한 데이터를 붙여 준비를 하는 경우 (있는 경우) 해당 기능에 가장 적합 한 표현을 찾아야 합니다. 일반적으로 가장 형식을 사용할 수 있습니다 (예를 들어 서식 있는 텍스트, 워드 프로세싱 응용 프로그램)에 됩니다 필요 (일반 텍스트로 간단한 텍스트 편집기에 대 한) 사용할 수 있는 가장 단순한 형태를 대체 합니다.

<a name="Promised_Data" />

### <a name="promised-data"></a>약속된 데이터

일반적으로 앱 간에 공유 최대화 하려면 가능한 한 복사 되는 데이터의 여러 표현을 제공 해야 합니다. 그러나 시간 또는 메모리 제약 조건으로 인해 것 않을 실제로 지에 각 데이터 형식을 작성 하는 데 적합 합니다.

이 경우 첫 번째 데이터 표현을 지에 배치할 수 및 수신 응용 프로그램 생성된에 즉석 붙여넣기 작업 직전 수 있는 다른 표현을 요청할 수 있습니다.

사용 가능한 다른 표현 중 하나 이상의 준수 하는 개체에 의해 제공 되는 지정 지에 초기 항목을 놓으면는 `NSPasteboardItemDataProvider` 인터페이스입니다. 이러한 개체 수신 응용 프로그램에서 요청한 대로 요청 시 추가 표현을 제공 합니다.

### <a name="change-count"></a>변경 수

각 대 지 유지 관리를 _변경 횟수_ 증분 시간 새 소유자를 선언 합니다. 앱 대 지의 내용을 변경 횟수의 값을 확인 하 여 검사 하 고 마지막 이후 변경 되었는지 여부를 확인할 수 있습니다.

사용 합니다 `ChangeCount` 및 `ClearContents` 의 메서드는 `NSPasteboard` 지정 된 지에서는 변경 횟수를 수정 하는 클래스입니다.

## <a name="copying-data-to-a-pasteboard"></a>에 지에서는 데이터를 복사합니다.

첫 번째에 액세스 하 여 지에서는, 기존의 모든 콘텐츠를 지우는 만큼 표현을 지 하는 데 필요한 만큼의 데이터를 쓰는 복사 작업을 수행 합니다.

예를 들어:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add the current image to the pasteboard
pasteboard.WriteObjects (new NSImage[] {image});
```

일반적으로 바로 작성할 수 있습니다 일반 지를 위의 예에서 같이 합니다. 전송 하는 모든 개체를 `WriteObjects` 메서드 *해야* 준수 하는 `INSPasteboardWriting` 인터페이스. 여러 기본 제공 클래스 (같은 `NSString`, `NSImage`, `NSURL`, `NSColor`를 `NSAttributedString`, 및 `NSPasteboardItem`) 자동으로이 인터페이스를 준수 합니다.

임시 보드를 사용자 지정 데이터 클래스를 작성 하는 경우를 준수 해야 합니다는 `INSPasteboardWriting` 인터페이스 또는 인스턴스의에 래핑될 수는 `NSPasteboardItem` 클래스 (참조는 [사용자 지정 데이터 형식](#Custom_Data_Types) 섹션 아래).

## <a name="reading-data-from-a-pasteboard"></a>에 지에서 데이터 읽기

위에서 설명한 대로 지를 앱 간의 데이터 공유에 대 한 잠재력을 극대화할 수 복사한 데이터의 여러 표현을 작성할 수 있습니다. 그가 수신 응용 프로그램에서 (있는 경우) 해당 기능에 대 한 가능한 가장 버전을 선택 합니다.

### <a name="simple-paste-operation"></a>간단한 붙여넣기 작업

임시 보드 사용 하 여 데이터를 읽기는 `ReadObjectsForClasses` 메서드. 두 개의 매개 변수가 필요 합니다.

1. 배열을 `NSObject` 지에서 읽을 수 있는 클래스 형식에 기반 합니다. 주문 해야이 가장 원하는 데이터 형식을 사용 하 여 먼저 모든 나머지 형식 내림차순으로 기본 설정 합니다.
2. 추가 제약 조건 (예: 특정 URL 내용 유형이 제한 됨)를 포함 하는 사전 또는 추가 제한이 필요한 경우 빈 사전입니다.

메서드는에 전달 하는 조건을 충족 하는 항목의 배열을 반환 하 고 요청 된 데이터 형식 수가 최대 포함. 것도 포함 되어 있지 않습니다 요청된 된 형식 있으며 빈 배열이 반환 됩니다.

예를 들어, 다음 코드를 확인 하는 경우는 `NSImage` 일반적인 지에 존재 하 고 그렇지 않으면 잘 이미지에 표시 합니다.

```csharp
[Export("PasteImage:")]
public void PasteImage(NSObject sender) {

    // Initialize the pasteboard
    NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
    Class [] classArray  = { new Class ("NSImage") };

    bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
        NSImage image = (NSImage)objectsToPaste[0];

        // Display the new image
        ImageView.Image = image;
    }

    Class [] classArray2 = { new Class ("ImageInfo") };
    ok = pasteboard.CanReadObjectForClasses (classArray2, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
        ImageInfo info = (ImageInfo)objectsToPaste[0];
            }
}
```

### <a name="requesting-multiple-data-types"></a>여러 데이터 형식을 요청합니다.

만들려는 Xamarin.Mac 응용 프로그램의 유형에 따라 붙여넣을 데이터의 여러 표현 처리 할 수 있습니다. 이 경우 두 가지 시나리오가 있습니다 지에서 데이터를 검색 합니다.

1. 단일 호출 하는 `ReadObjectsForClasses` 메서드와 (기본 순서)에서 원하는 표현의 모든 배열을 제공 합니다.
2. 여러 번 호출할는 `ReadObjectsForClasses` 메서드 다른 배열에 대 한 요청 될 때마다 형식입니다.

참조 된 **간단한 붙여넣기 작업** 지에서는에서 데이터 검색에 대 한 자세한 내용은 위의 섹션입니다.

### <a name="checking-for-existing-data-types"></a>기존 데이터 형식에 대 한 확인

지에서는 실제로 지에서 데이터를 읽지 않고 지정 된 데이터 표현을 포함 되어 있는지 확인할 수 있는 경우가 있습니다 (예를 들어 합니다 **붙여넣기** 메뉴 항목 유효 데이터가 존재 하는 경우에).

호출 된 `CanReadObjectForClasses` 메서드의 지정된 된 형식에 포함 되어 있는지 확인 하려면 임시 보드입니다.

예를 들어 다음 코드는 일반적인 지를 포함 하는 경우 결정을 `NSImage` 인스턴스:

```csharp
public bool ImageAvailableOnPasteboard {
    get {
        // Initialize the pasteboard
        NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
        Class [] classArray  = { new Class ("NSImage") };

        // Check to see if an image is on the pasteboard
        return pasteboard.CanReadObjectForClasses (classArray, null);
    }
}
```

### <a name="reading-urls-from-the-pasteboard"></a>임시 보드에서 읽기 url

지정된 된 Xamarin.Mac 앱의 기능에 따라 것 지에서는에서 Url을 읽어야 하지만 지정된 된 집합이 기준 (예: 파일 또는 특정 데이터 형식의 Url을 가리키는)를 충족 하는 경우. 이 경우 두 번째 매개 변수를 사용 하 여 추가 검색 조건을 지정할 수 있습니다 합니다 `CanReadObjectForClasses` 또는 `ReadObjectsForClasses` 메서드.

<a name="Custom_Data_Types" />

## <a name="custom-data-types"></a>사용자 지정 데이터 형식

Xamarin.Mac 앱에서 사용자 지정 형식을 지를 저장 해야 하는 경우 경우가 있습니다. 예를 들어, 벡터 드로잉을 사용 하면 복사 및 붙여넣기 그리기 개체를 하는 앱입니다.

이 경우 사용자 지정 데이터 클래스에서 상속 하도록 디자인 해야 `NSObject` 몇 가지 인터페이스를 준수 하는 것 (`INSCoding`를 `INSPasteboardWriting` 고 `INSPasteboardReading`). 사용할 수 있습니다는 `NSPasteboardItem` 를 복사 하거나 붙여 넣은 데이터를 캡슐화 합니다.

두이 옵션 모두 아래에서 자세히 설명 합니다.

### <a name="using-a-custom-class"></a>사용자 지정 클래스를 사용 하 여

이 섹션에서는이 문서의 시작 부분에서 만든 간단한 예제에서는 앱에서 확장 되며 windows 간의 복사 및 붙여넣기는 우리는 이미지에 대 한 정보를 추적 하는 사용자 지정 클래스를 추가 합니다.

프로젝트에 새 클래스를 추가 하 고 호출할 **ImageInfo.cs**합니다. 파일 편집 하 고 다음과 같이 표시 되도록 합니다.

```csharp
using System;
using AppKit;
using Foundation;

namespace MacCopyPaste
{
    [Register("ImageInfo")]
    public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
    {
        #region Computed Properties
        [Export("name")]
        public string Name { get; set; }

        [Export("imageType")]
        public string ImageType { get; set; }
        #endregion

        #region Constructors
        [Export ("init")]
        public ImageInfo ()
        {
        }
        
        public ImageInfo (IntPtr p) : base (p)
        {
        }

        [Export ("initWithCoder:")]
        public ImageInfo(NSCoder decoder) {

            // Decode data
            NSString name = decoder.DecodeObject("name") as NSString;
            NSString type = decoder.DecodeObject("imageType") as NSString;

            // Save data
            Name = name.ToString();
            ImageType = type.ToString ();
        }
        #endregion

        #region Public Methods
        [Export ("encodeWithCoder:")]
        public void EncodeTo (NSCoder encoder) {

            // Encode data
            encoder.Encode(new NSString(Name),"name");
            encoder.Encode(new NSString(ImageType),"imageType");
        }

        [Export ("writableTypesForPasteboard:")]
        public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
            string[] writableTypes = {"com.xamarin.image-info", "public.text"};
            return writableTypes;
        }

        [Export ("pasteboardPropertyListForType:")]
        public virtual NSObject GetPasteboardPropertyListForType (string type) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return NSKeyedArchiver.ArchivedDataWithRootObject(this);
            case "public.text":
                return new NSString(string.Format("{0}.{1}", Name, ImageType));
            }

            // Failure, return null
            return null;
        }

        [Export ("readableTypesForPasteboard:")]
        public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
            string[] readableTypes = {"com.xamarin.image-info", "public.text"};
            return readableTypes;
        }

        [Export ("readingOptionsForType:pasteboard:")]
        public static NSPasteboardReadingOptions GetReadingOptionsForType (string type, NSPasteboard pasteboard) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return NSPasteboardReadingOptions.AsKeyedArchive;
            case "public.text":
                return NSPasteboardReadingOptions.AsString;
            }

            // Default to property list
            return NSPasteboardReadingOptions.AsPropertyList;
        }

        [Export ("initWithPasteboardPropertyList:ofType:")]
        public NSObject InitWithPasteboardPropertyList (NSObject propertyList, string type) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return new ImageInfo();
            case "public.text":
                return new ImageInfo();
            }

            // Failure, return null
            return null;
        }
        #endregion
    }
}
    
```

다음 섹션에 자세히 살펴보고가이 클래스에 알아보겠습니다.

#### <a name="inheritance-and-interfaces"></a>상속 및 인터페이스

따라야 하는 사용자 지정 데이터 클래스를 작성 하거나 지에서는에서 읽을 수, 전에 `INSPastebaordWriting` 고 `INSPasteboardReading` 인터페이스입니다. 상속 해야 뿐만 `NSObject` 또한 준수 및는 `INSCoding` 인터페이스:

```csharp
[Register("ImageInfo")]
public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
...
```

Objective-c에 클래스 노출 될 수 있어야 합니다를 사용 하 여 합니다 `Register` 지시문과 해당 필수 속성 또는 메서드를 사용 하 여 노출 해야 `Export`합니다. 예를 들어:

```csharp
[Export("name")]
public string Name { get; set; }

[Export("imageType")]
public string ImageType { get; set; }
```

포함 된 데이터를이 클래스는-이미지의 이름 및 해당 형식 (예: jpg, png)의 두 필드 공개 하는 것입니다. 

자세한 내용은 참조 하세요.를 [노출 C# 클래스 / objective-c 메서드](~/mac/internals/how-it-works.md) 섹션을 [Xamarin.Mac 내부 요소](~/mac/internals/how-it-works.md) 설명 설명서를 `Register` 및 `Export` 특성 요소 Objective-c 개체 및 UI에 C# 클래스에 연결 하는 데 사용 합니다.

#### <a name="constructors"></a>생성자

(Objective-c에 적절히 노출) 하는 두 명의 생성자는 임시 보드에서 읽을 수 있도록 사용자 지정 데이터 클래스에 필요한 됩니다.

```csharp
[Export ("init")]
public ImageInfo ()
{
}

[Export ("initWithCoder:")]
public ImageInfo(NSCoder decoder) {

    // Decode data
    NSString name = decoder.DecodeObject("name") as NSString;
    NSString type = decoder.DecodeObject("imageType") as NSString;

    // Save data
    Name = name.ToString();
    ImageType = type.ToString ();
}
```

노출 먼저 합니다 _빈_ 의 기본 Objective-c 메서드로 생성자 `init`합니다.

위해 노출 하는 다음에 `NSCoding` 내보낸된 이름 아래에 있는 붙여넣을 때 임시 보드에서 개체의 새 인스턴스를 만드는 데 사용할 수 있는 규격 생성자 `initWithCoder`합니다.

이 생성자는 `NSCoder` (생성 되는 `NSKeyedArchiver` 지를 쓸 때), 키/값 쌍을 이루는 데이터를 추출 및 데이터 클래스의 속성 필드에 저장 합니다.

#### <a name="writing-to-the-pasteboard"></a>임시 보드에 쓰기

준수 하 여는 `INSPasteboardWriting` 인터페이스를 먼저 두 가지 방법 및 필요에 따라 세 번째 메서드를 노출 하도록 클래스 지에 쓸 수 있도록 합니다.

먼저 지 데이터 형식 사용자 지정 클래스에 쓸 수 있는 표현 지시 해야 합니다.

```csharp
[Export ("writableTypesForPasteboard:")]
public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
    string[] writableTypes = {"com.xamarin.image-info", "public.text"};
    return writableTypes;
}
```

각 표현을 통해는 균일 한 형식 식별자 (UTI), 표시 되는 데이터의 형식을 고유 하 게 식별 하는 간단한 문자열에 지나지 인 식별 됩니다 (자세한 내용은 Apple의를 참조 하십시오 [Uniform 형식 식별자 개요 ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) 설명서).

이 사용자 지정 형식에 대 한 고유한 UTI 만들고: "com.xamarin.image-info" (앱 식별자와 마찬가지로 역방향 표기법에 있는 참고). 또한이 클래스는 지 표준 문자열을 쓸 수 있는 (`public.text`). 

다음으로, 임시 보드에 실제로 기록 되는 요청 된 형식 개체를 만들려고 해야 합니다.

```csharp
[Export ("pasteboardPropertyListForType:")]
public virtual NSObject GetPasteboardPropertyListForType (string type) {

    // Take action based on the requested type
    switch (type) {
    case "com.xamarin.image-info":
        return NSKeyedArchiver.ArchivedDataWithRootObject(this);
    case "public.text":
        return new NSString(string.Format("{0}.{1}", Name, ImageType));
    }

    // Failure, return null
    return null;
}
```

에 대 한 합니다 `public.text` 형식에서는 반환 하는 단순 하 고 포맷 `NSString` 개체입니다. 사용자 지정 `com.xamarin.image-info` 형식을 사용 하 고는 `NSKeyedArchiver` 및 `NSCoder` 키/값 쌍을 이루는 보관 파일을 사용자 지정 데이터 클래스를 인코딩할 인터페이스입니다. 실제로 인코딩을 처리 하는 다음 메서드를 구현 해야 합니다.

```csharp
[Export ("encodeWithCoder:")]
public void EncodeTo (NSCoder encoder) {

    // Encode data
    encoder.Encode(new NSString(Name),"name");
    encoder.Encode(new NSString(ImageType),"imageType");
}
```

개별 키/값 쌍을 인코더에 기록 되 고 위에서 추가한 두 번째 생성자를 사용 하 여 디코딩할 수 됩니다.

필요에 따라에서는 지에 데이터를 쓸 때 모든 옵션을 정의 하는 다음 메서드를 포함할 수 있습니다.

```csharp
[Export ("writingOptionsForType:pasteboard:"), CompilerGenerated]
public virtual NSPasteboardWritingOptions GetWritingOptionsForType (string type, NSPasteboard pasteboard) {
    return NSPasteboardWritingOptions.WritingPromised;
}
```

현재는 `WritingPromised` 옵션 사용할 수 있고 지정된 된 형식 버전 이후로 예정 이며 지를 실제로 쓸 때 사용 해야 합니다. 자세한 내용은 참조 하십시오 합니다 [약속 데이터](#Promised_Data) 위의 섹션입니다.

현재 위치에서 이러한 메서드를 사용 하 여 임시 보드 사용자 지정 클래스를 쓸 다음 코드를 사용할 수 있습니다.

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add info to the pasteboard
pasteboard.WriteObjects (new ImageInfo[] { Info });
```

#### <a name="reading-from-the-pasteboard"></a>임시 보드에서 읽기

준수 하 여는 `INSPasteboardReading` 지에서 사용자 지정 데이터 클래스를 읽을 수 있도록 세 가지 메서드를 노출 하려고 하는 인터페이스입니다.

먼저 지 데이터 형식 사용자 지정 클래스를 클립보드의 읽을 수 있는 표현 지시 해야 합니다.

```csharp
[Export ("readableTypesForPasteboard:")]
public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
    string[] readableTypes = {"com.xamarin.image-info", "public.text"};
    return readableTypes;
}
```

마찬가지로 이러한 간단한 Uti로 정의 됩니다 및 파일에 정의 된 동일한 형식일 것 이라고 합니다 **지 쓸** 위의 섹션입니다.

이 어 지 지시할 필요가 _어떻게_ 다음 메서드를 사용 하 여 각 UTI 유형에 읽을 수:

```csharp
[Export ("readingOptionsForType:pasteboard:")]
public static NSPasteboardReadingOptions GetReadingOptionsForType (string type, NSPasteboard pasteboard) {

    // Take action based on the requested type
    switch (type) {
    case "com.xamarin.image-info":
        return NSPasteboardReadingOptions.AsKeyedArchive;
    case "public.text":
        return NSPasteboardReadingOptions.AsString;
    }

    // Default to property list
    return NSPasteboardReadingOptions.AsPropertyList;
}
```

에 대 한는 `com.xamarin.image-info` 형식을 사용 하 여 만든 키/값 쌍을 디코딩할 지 지시 했습니다를 `NSKeyedArchiver` 작성 하는 경우 클래스는 지를 호출 하 여는 `initWithCoder:` 생성자 클래스를 추가 합니다.

마지막으로, 다른 UTI 데이터 표현을 지 읽을 다음 메서드를 추가 해야 합니다.

```csharp
[Export ("initWithPasteboardPropertyList:ofType:")]
public NSObject InitWithPasteboardPropertyList (NSObject propertyList, string type) {

    // Take action based on the requested type
    switch (type) {
    case "public.text":
        return new ImageInfo();
    }

    // Failure, return null
    return null;
}
```

현재 위치에서 이러한 모든 메서드를 사용 하 여 사용자 지정 데이터 클래스는 다음 코드를 사용 하 여 임시 보드에서 읽을 수 있습니다.

```csharp
// Initialize the pasteboard
NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
var classArrayPtrs = new [] { Class.GetHandle (typeof(ImageInfo)) };
NSArray classArray = NSArray.FromIntPtrs (classArrayPtrs);

// NOTE: Sending messages directly to the base Objective-C API because of this defect:
// https://bugzilla.xamarin.com/show_bug.cgi?id=31760
// Check to see if image info is on the pasteboard
ok = bool_objc_msgSend_IntPtr_IntPtr (pasteboard.Handle, Selector.GetHandle ("canReadObjectForClasses:options:"), classArray.Handle, IntPtr.Zero);

if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = NSArray.ArrayFromHandle<Foundation.NSObject>(IntPtr_objc_msgSend_IntPtr_IntPtr (pasteboard.Handle, Selector.GetHandle ("readObjectsForClasses:options:"), classArray.Handle, IntPtr.Zero));
    ImageInfo info = (ImageInfo)objectsToPaste[0];
}
```

### <a name="using-a-nspasteboarditem"></a>NSPasteboardItem를 사용 하 여

사용자 지정 클래스의 생성을 보증 하지 않습니다는 지를 사용자 지정 항목을 작성 해야 하거나 필요한 만큼만 공통 형식으로 데이터를에서 제공 하려는 경우 시간이 있을 수 있습니다. 이러한 상황에서는 사용할 수는 `NSPasteboardItem`합니다.

`NSPasteboardItem` 세부적으로 제어할 지에 기록 되 고 임시 액세스 용인지 여부를 지정 하는 데이터에 대해이 삭제 해야 지에 기록한 후 합니다.

#### <a name="writing-data"></a>데이터 쓰기

사용자 지정 데이터를 작성 하는 `NSPasteboardItem` 사용자 지정을 제공 해야 `NSPasteboardItemDataProvider`합니다. 프로젝트에 새 클래스를 추가 하 고 호출할 **ImageInfoDataProvider.cs**합니다. 파일 편집 하 고 다음과 같이 표시 되도록 합니다.

```csharp
using System;
using AppKit;
using Foundation;

namespace MacCopyPaste
{
    [Register("ImageInfoDataProvider")]
    public class ImageInfoDataProvider : NSPasteboardItemDataProvider
    {
        #region Computed Properties
        public string Name { get; set;}
        public string ImageType { get; set;}
        #endregion

        #region Constructors
        [Export ("init")]
        public ImageInfoDataProvider ()
        {
        }

        public ImageInfoDataProvider (string name, string imageType)
        {
            // Initialize
            this.Name = name;
            this.ImageType = imageType;
        }

        protected ImageInfoDataProvider (NSObjectFlag t){
        }

        protected internal ImageInfoDataProvider (IntPtr handle){

        }
        #endregion

        #region Override Methods
        [Export ("pasteboardFinishedWithDataProvider:")]
        public override void FinishedWithDataProvider (NSPasteboard pasteboard)
        {
            
        }

        [Export ("pasteboard:item:provideDataForType:")]
        public override void ProvideDataForType (NSPasteboard pasteboard, NSPasteboardItem item, string type)
        {

            // Take action based on the type
            switch (type) {
            case "public.text":
                // Encode the data to string 
                item.SetStringForType(string.Format("{0}.{1}", Name, ImageType),type);
                break;
            }

        }
        #endregion
    }
}
```

사용자 지정 데이터 클래스를 사용 하 여 했던 것 처럼 사용 해야 합니다 `Register` 및 `Export` 목표 C에 노출 하는 지시문 클래스에서 상속 해야 합니다 `NSPasteboardItemDataProvider` 하 고 구현 해야 합니다 `FinishedWithDataProvider` 및 `ProvideDataForType` 메서드.

사용 합니다 `ProvideDataForType` 에서 래핑될 데이터를 제공 하는 방법의 `NSPasteboardItem` 같이:

```csharp
[Export ("pasteboard:item:provideDataForType:")]
public override void ProvideDataForType (NSPasteboard pasteboard, NSPasteboardItem item, string type)
{

    // Take action based on the type
    switch (type) {
    case "public.text":
        // Encode the data to string 
        item.SetStringForType(string.Format("{0}.{1}", Name, ImageType),type);
        break;
    }

}
```

이 경우에 (이름 및 ImageType) 이미지에 대 한 두 가지 정보를 저장 하 고 간단한 문자열에 쓰기 (`public.text`).

형식 데이터를 쓸 지 다음 코드를 사용 합니다.

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Using a Pasteboard Item
NSPasteboardItem item = new NSPasteboardItem();
string[] writableTypes = {"public.text"};

// Add a data provider to the item
ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

// Save to pasteboard
if (ok) {
    pasteboard.WriteObjects (new NSPasteboardItem[] { item });
}
```

#### <a name="reading-data"></a>데이터 읽기

임시 보드에서 다시 데이터를 읽을 하려면 다음 코드를 사용 합니다.

```csharp
// Initialize the pasteboard
NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
Class [] classArray  = { new Class ("NSImage") };

bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
    NSImage image = (NSImage)objectsToPaste[0];

    // Do something with data
    ...
}
            
Class [] classArray2 = { new Class ("ImageInfo") };
ok = pasteboard.CanReadObjectForClasses (classArray2, null);
if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
    
    // Do something with data
    ...
}
```

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Mac 응용 프로그램을 지원 복사 및 붙여넣기 작업이에 지 작업을 자세히 살펴보려면을 걸렸습니다. 먼저 표준 대지의 오브젝트 작업과 파악 하는 간단한 예제를 소개 했습니다. 다음으로, 데이터 읽기 및 쓰기를 하는 방법과 지 자세히 살펴보고를 걸렸습니다. 마지막으로, 복사 및 붙여넣기 앱 내에서 복잡 한 데이터 형식의 지원 하기 위해 사용자 지정 데이터 형식을 사용 하 여 조회 합니다.



## <a name="related-links"></a>관련 링크

- [MacCopyPaste (샘플)](https://developer.xamarin.com/samples/mac/MacCopyPaste/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [임시 보드 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/PasteboardGuide106/Articles/pbGettingStarted.html)
- [macOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
