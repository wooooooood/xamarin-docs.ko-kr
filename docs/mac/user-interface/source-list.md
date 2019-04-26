---
title: Xamarin.Mac의 원본 목록
description: 이 문서에서는 Xamarin.Mac 응용 프로그램에서 원본 목록 사용 하 여 작업을 설명 합니다. 만들기, Xcode 및 Interface Builder에서 원본 목록을 유지 관리 및 C# 코드에서 상호 작용을 설명 합니다.
ms.prod: xamarin
ms.assetid: 651A3649-5AA8-4133-94D6-4873D99F7FCC
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 82e4dfb9add7002fd7d3568d0ec946ea38dfd530
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61290624"
---
# <a name="source-lists-in-xamarinmac"></a>Xamarin.Mac의 원본 목록

_이 문서에서는 Xamarin.Mac 응용 프로그램에서 원본 목록 사용 하 여 작업을 설명 합니다. 만들기, Xcode 및 Interface Builder에서 원본 목록을 유지 관리 및 C# 코드에서 상호 작용을 설명 합니다._

작업할 때 C# 에 액세스할 수 있는 Xamarin.Mac 응용 프로그램에서.NET, 및는 동일한 소스 작업 하는 개발자를 제공 하는 나열 *Objective-c* 및 *Xcode* 않습니다. Xcode의 Xamarin.Mac이 Xcode와 직접 통합 되므로 사용할 수 있습니다 _Interface Builder_ 만들기 및 프로그램 원본 목록을 유지 관리 (또는 필요에 따라 C# 코드에서 바로 작성).

원본 목록에는 원인을 찾기 또는 iTunes에서 세로 막대와 같은 작업을 표시 하는 데 사용 되는 개요 보기의 특별 한 형식입니다.

[![](source-list-images/source05.png "원본 목록의 예")](source-list-images/source05.png#lightbox)

이 문서에서는 Xamarin.Mac 응용 프로그램에서 원본 목록을 사용 하 여 작업의 기본 사항을 설명 합니다. 것이 가장 좋습니다를 통해 작업 하는 합니다 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서 합니다 [Xcode 및 Interface Builder 소개](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 하 고 [출 선 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션으로 주요 개념 및이 문서를 사용 하는 기술을 설명 합니다.

확인 하려는 경우는 [노출 C# 클래스 / objective-c 메서드](~/mac/internals/how-it-works.md) 섹션을 [Xamarin.Mac 내부 요소](~/mac/internals/how-it-works.md) 설명도 문서를 `Register` 및 `Export` 명령 하 여 통신-C# 클래스 Objective-c 개체 및 UI 요소에 사용 합니다.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-source-lists"></a>원본 목록 소개

위에서 설명한 대로 원본 목록은 Finder 또는 iTunes에서 세로 막대와 같은 작업의 소스를 표시 하는 데 특별 한 유형의 개요 보기입니다. 원본 목록은 사용자를 허용 하는 테이블 형식 확장 또는 축소 계층적 데이터의 행입니다. 표 보기와 달리 플랫 목록에 없는 원본 목록에서 항목, 하드 드라이브의 파일 및 폴더와 같은 계층에 구성 된 합니다. 원본 목록에서 항목에 다른 항목이 포함 된 경우에 확장 또는 사용자가 축소 수 있습니다.

원본 목록에서 특별히 스타일의 개요 뷰입니다 (`NSOutlineView`), 테이블 보기의 하위 클래스는 직접 (`NSTableView`) 따라서 동작의 대부분이 부모 클래스에서 상속 합니다. 결과적으로, 개요 보기를 지 원하는 많은 작업을 원본 목록에서 지원 됩니다. Xamarin.Mac 응용 프로그램이 이러한 기능을 제어 하 고 허용 하거나 특정 작업 (코드 또는 Interface Builder에서 중 하나)를 원본 목록 매개 변수를 구성할 수 있습니다.

원본 목록 자체 데이터를 저장 하지 않으므로, 데이터 원본에서 대신 (`NSOutlineViewDataSource`) 행과 열에는 필요에 따라 필요한 수 있도록 합니다.

개요 보기 대리자의 서브 클래스를 제공 하 여 원본 목록의 동작을 사용자 지정할 수 있습니다 (`NSOutlineViewDelegate`) 선택 및 편집, 사용자 지정 추적 및 개별 항목에 대 한 사용자 지정 보기에 항목을 기능 선택 개요 형식을 지원 합니다.

통해 이동 하려는 테이블 뷰 및 개요 보기를 사용 하 여 대부분의 동작 및 기능을 공유 하는 원본 목록에 있으므로 우리의 [테이블 뷰](~/mac/user-interface/table-view.md) 하 고 [개요 보기](~/mac/user-interface/outline-view.md) 계속 하기 전에 설명서 이 기사.

<a name="Working_with_Source_Lists" />

## <a name="working-with-source-lists"></a>원본 목록 사용

원본 목록에는 원인을 찾기 또는 iTunes에서 세로 막대와 같은 작업을 표시 하는 데 사용 되는 개요 보기의 특별 한 형식입니다. 개요 보기와 달리 Interface Builder에서 원본 목록 정의 전에 지원 클래스를 보겠습니다 Xamarin.Mac에 만듭니다.

먼저 새를 만들어 보겠습니다 `SourceListItem` 원본 목록에 대 한 데이터를 보관 하는 클래스입니다. 에 **솔루션 탐색기**, 프로젝트를 마우스 오른쪽 단추로 **추가** > **새 파일...** 선택 **일반** > **빈 클래스**를 입력 `SourceListItem` 에 대 한를 **이름** 을 클릭 합니다 **새로 만들기** 단추:

[![](source-list-images/source01.png "빈 클래스를 추가합니다.")](source-list-images/source01.png#lightbox)

확인 된 `SourceListItem.cs` 다음과 같은 파일 확인: 

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListItem: NSObject, IEnumerator, IEnumerable
    {
        #region Private Properties
        private string _title;
        private NSImage _icon;
        private string _tag;
        private List<SourceListItem> _items = new List<SourceListItem> ();
        #endregion

        #region Computed Properties
        public string Title {
            get { return _title; }
            set { _title = value; }
        }

        public NSImage Icon {
            get { return _icon; }
            set { _icon = value; }
        }

        public string Tag {
            get { return _tag; }
            set { _tag=value; }
        }
        #endregion

        #region Indexer
        public SourceListItem this[int index]
        {
            get
            {
                return _items[index];
            }

            set
            {
                _items[index] = value;
            }
        }

        public int Count {
            get { return _items.Count; }
        }

        public bool HasChildren {
            get { return (Count > 0); }
        }
        #endregion

        #region Enumerable Routines
        private int _position = -1;

        public IEnumerator GetEnumerator()
        {
            _position = -1;
            return (IEnumerator)this;
        }

        public bool MoveNext()
        {
            _position++;
            return (_position < _items.Count);
        }

        public void Reset()
        {_position = -1;}

        public object Current
        {
            get 
            { 
                try 
                {
                    return _items[_position];
                }

                catch (IndexOutOfRangeException)
                {
                    throw new InvalidOperationException();
                }
            }
        }
        #endregion

        #region Constructors
        public SourceListItem ()
        {
        }

        public SourceListItem (string title)
        {
            // Initialize
            this._title = title;
        }

        public SourceListItem (string title, string icon)
        {
            // Initialize
            this._title = title;
            this._icon = NSImage.ImageNamed (icon);
        }

        public SourceListItem (string title, string icon, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = NSImage.ImageNamed (icon);
            this.Clicked = clicked;
        }

        public SourceListItem (string title, NSImage icon)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
        }

        public SourceListItem (string title, NSImage icon, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this.Clicked = clicked;
        }

        public SourceListItem (string title, NSImage icon, string tag)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this._tag = tag;
        }

        public SourceListItem (string title, NSImage icon, string tag, ClickedDelegate clicked)
        {
            // Initialize
            this._title = title;
            this._icon = icon;
            this._tag = tag;
            this.Clicked = clicked;
        }
        #endregion

        #region Public Methods
        public void AddItem(SourceListItem item) {
            _items.Add (item);
        }

        public void AddItem(string title) {
            _items.Add (new SourceListItem (title));
        }

        public void AddItem(string title, string icon) {
            _items.Add (new SourceListItem (title, icon));
        }

        public void AddItem(string title, string icon, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, clicked));
        }

        public void AddItem(string title, NSImage icon) {
            _items.Add (new SourceListItem (title, icon));
        }

        public void AddItem(string title, NSImage icon, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, clicked));
        }

        public void AddItem(string title, NSImage icon, string tag) {
            _items.Add (new SourceListItem (title, icon, tag));
        }

        public void AddItem(string title, NSImage icon, string tag, ClickedDelegate clicked) {
            _items.Add (new SourceListItem (title, icon, tag, clicked));
        }

        public void Insert(int n, SourceListItem item) {
            _items.Insert (n, item);
        }

        public void RemoveItem(SourceListItem item) {
            _items.Remove (item);
        }

        public void RemoveItem(int n) {
            _items.RemoveAt (n);
        }

        public void Clear() {
            _items.Clear ();
        }
        #endregion

        #region Events
        public delegate void ClickedDelegate();
        public event ClickedDelegate Clicked;

        internal void RaiseClickedEvent() {
            // Inform caller
            if (this.Clicked != null)
                this.Clicked ();
        }
        #endregion
    }
}
```

에 **솔루션 탐색기**, 프로젝트를 마우스 오른쪽 단추로 **추가** > **새 파일...** 선택 **일반** > **빈 클래스**, 입력 `SourceListDataSource` 에 대 한를 **이름** 을 클릭 합니다 **새로 만들기** 단추. 확인 된 `SourceListDataSource.cs` 다음과 같은 파일 확인:

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListDataSource : NSOutlineViewDataSource
    {
        #region Private Variables
        private SourceListView _controller;
        #endregion

        #region Public Variables
        public List<SourceListItem> Items = new List<SourceListItem>();
        #endregion

        #region Constructors
        public SourceListDataSource (SourceListView controller)
        {
            // Initialize
            this._controller = controller;
        }
        #endregion

        #region Override Properties
        public override nint GetChildrenCount (NSOutlineView outlineView, Foundation.NSObject item)
        {
            if (item == null) {
                return Items.Count;
            } else {
                return ((SourceListItem)item).Count;
            }
        }

        public override bool ItemExpandable (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return ((SourceListItem)item).HasChildren;
        }

        public override NSObject GetChild (NSOutlineView outlineView, nint childIndex, Foundation.NSObject item)
        {
            if (item == null) {
                return Items [(int)childIndex];
            } else {
                return ((SourceListItem)item) [(int)childIndex]; 
            }
        }

        public override NSObject GetObjectValue (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item)
        {
            return new NSString (((SourceListItem)item).Title);
        }
        #endregion

        #region Internal Methods
        internal SourceListItem ItemForRow(int row) {
            int index = 0;

            // Look at each group
            foreach (SourceListItem item in Items) {
                // Is the row inside this group?
                if (row >= index && row <= (index + item.Count)) {
                    return item [row - index - 1];
                }

                // Move index
                index += item.Count + 1;
            }

            // Not found 
            return null;
        }
        #endregion
    }
}
```

이 원본 목록에 대 한 데이터를 제공 합니다.

에 **솔루션 탐색기**, 프로젝트를 마우스 오른쪽 단추로 **추가** > **새 파일...** 선택 **일반** > **빈 클래스**, 입력 `SourceListDelegate` 에 대 한를 **이름** 을 클릭 합니다 **새로 만들기** 단추. 확인 된 `SourceListDelegate.cs` 다음과 같은 파일 확인:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacOutlines
{
    public class SourceListDelegate : NSOutlineViewDelegate
    {
        #region Private variables
        private SourceListView _controller;
        #endregion

        #region Constructors
        public SourceListDelegate (SourceListView controller)
        {
            // Initialize
            this._controller = controller;
        }
        #endregion

        #region Override Methods
        public override bool ShouldEditTableColumn (NSOutlineView outlineView, NSTableColumn tableColumn, Foundation.NSObject item)
        {
            return false;
        }

        public override NSCell GetCell (NSOutlineView outlineView, NSTableColumn tableColumn, Foundation.NSObject item)
        {
            nint row = outlineView.RowForItem (item);
            return tableColumn.DataCellForRow (row);
        }

        public override bool IsGroupItem (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return ((SourceListItem)item).HasChildren;
        }

        public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item)
        {
            NSTableCellView view = null;

            // Is this a group item?
            if (((SourceListItem)item).HasChildren) {
                view = (NSTableCellView)outlineView.MakeView ("HeaderCell", this);
            } else {
                view = (NSTableCellView)outlineView.MakeView ("DataCell", this);
                view.ImageView.Image = ((SourceListItem)item).Icon;
            }

            // Initialize view
            view.TextField.StringValue = ((SourceListItem)item).Title;

            // Return new view
            return view;
        }

        public override bool ShouldSelectItem (NSOutlineView outlineView, Foundation.NSObject item)
        {
            return (outlineView.GetParent (item) != null);
        }

        public override void SelectionDidChange (NSNotification notification)
        {
            NSIndexSet selectedIndexes = _controller.SelectedRows;

            // More than one item selected?
            if (selectedIndexes.Count > 1) {
                // Not handling this case
            } else {
                // Grab the item
                var item = _controller.Data.ItemForRow ((int)selectedIndexes.FirstIndex);

                // Was an item found?
                if (item != null) {
                    // Fire the clicked event for the item
                    item.RaiseClickedEvent ();

                    // Inform caller of selection
                    _controller.RaiseItemSelected (item);
                }
            }
        }
        #endregion
    }
}
```

원본 목록의 동작을 제공 합니다.

마지막으로 **솔루션 탐색기**, 프로젝트를 마우스 오른쪽 단추로 **추가** > **새 파일...** 선택 **일반** > **빈 클래스**, 입력 `SourceListView` 에 대 한를 **이름** 을 클릭 합니다 **새로 만들기** 단추. 확인 된 `SourceListView.cs` 다음과 같은 파일 확인:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacOutlines
{
    [Register("SourceListView")]
    public class SourceListView : NSOutlineView
    {
        #region Computed Properties
        public SourceListDataSource Data {
            get {return (SourceListDataSource)this.DataSource; }
        }
        #endregion

        #region Constructors
        public SourceListView ()
        {

        }

        public SourceListView (IntPtr handle) : base(handle)
        {

        }

        public SourceListView (NSCoder coder) : base(coder)
        {

        }

        public SourceListView (NSObjectFlag t) : base(t)
        {

        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();
        }
        #endregion

        #region Public Methods
        public void Initialize() {

            // Initialize this instance
            this.DataSource = new SourceListDataSource (this);
            this.Delegate = new SourceListDelegate (this);

        }
        
        public void AddItem(SourceListItem item) {
            if (Data != null) {
                Data.Items.Add (item);
            }
        }
        #endregion

        #region Events
        public delegate void ItemSelectedDelegate(SourceListItem item);
        public event ItemSelectedDelegate ItemSelected;

        internal void RaiseItemSelected(SourceListItem item) {
            // Inform caller
            if (this.ItemSelected != null) {
                this.ItemSelected (item);
            }
        }
        #endregion
    }
}
```

사용자 지정, 재사용 가능한 하위 클래스를 이렇게 `NSOutlineView` (`SourceListView`) 따라서 사안을 결정할 Xamarin.Mac 응용 프로그램의 원본 목록에서 드라이브를 사용할 수 있는 합니다.

<a name="Creating_and_Maintaining_Source_Lists_in_Xcode" />

## <a name="creating-and-maintaining-source-lists-in-xcode"></a>만들기 및 Xcode에서 원본 목록을 유지 관리

이제 Interface Builder에서 원본 목록을 설계 해 보겠습니다. 두 번 클릭 합니다 `Main.storyboard` Interface Builder에서 편집용으로 연 파일에서 분할 뷰를 끌어를 **라이브러리 검사기**보기 컨트롤러를 추가 하 고 뷰를 사용 하 여 크기 조정 설정를 **제약 조건 편집기** :

[![](source-list-images/source00.png "제약 조건 편집")](source-list-images/source00.png#lightbox)

그런 다음 원본 목록에서 끌어를 **라이브러리 검사기**분할 뷰의 왼쪽에 추가 하 고 뷰를 사용 하 여 크기를 조정 하도록 설정 합니다 **제약 조건 편집기**:

[![](source-list-images/source02.png "제약 조건 편집")](source-list-images/source02.png#lightbox)

다음으로 전환 합니다 **Identity 뷰**, 소스 목록에서 선택 하 고 변경의 **클래스** 에 `SourceListView`:

[![](source-list-images/source03.png "클래스 이름 설정")](source-list-images/source03.png#lightbox)

마지막으로 만듭니다는 **출 선** 소스 목록 이라는 `SourceList` 에 `ViewController.h` 파일:

[![](source-list-images/source04.png "출 선 구성")](source-list-images/source04.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

<a name="Populating the Source List" />

## <a name="populating-the-source-list"></a>원본 목록 채우기

편집할 합니다 `RotationWindow.cs` Mac 용 Visual Studio에서 파일 및 있도록의 `AwakeFromNib` 다음과 같은 메서드 확인:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Populate source list
    SourceList.Initialize ();

    var library = new SourceListItem ("Library");
    library.AddItem ("Venues", "house.png", () => {
        Console.WriteLine("Venue Selected");
    });
    library.AddItem ("Singers", "group.png");
    library.AddItem ("Genre", "cards.png");
    library.AddItem ("Publishers", "box.png");
    library.AddItem ("Artist", "person.png");
    library.AddItem ("Music", "album.png");
    SourceList.AddItem (library);

    // Add Rotation 
    var rotation = new SourceListItem ("Rotation"); 
    rotation.AddItem ("View Rotation", "redo.png");
    SourceList.AddItem (rotation);

    // Add Kiosks
    var kiosks = new SourceListItem ("Kiosks");
    kiosks.AddItem ("Sign-in Station 1", "imac");
    kiosks.AddItem ("Sign-in Station 2", "ipad");
    SourceList.AddItem (kiosks);

    // Display side list
    SourceList.ReloadData ();
    SourceList.ExpandItem (null, true);

}
```

합니다 `Initialize ()` 메서드는 원본 목록에 대해 호출할 필요가 **출 선** _하기 전에_ 모든 항목에 추가 됩니다. 항목의 각 그룹에 대 한 부모 항목을 만들고 해당 그룹 항목에 하위 항목을 추가 합니다. 각 그룹은 원본 목록의 컬렉션에 추가한 `SourceList.AddItem (...)`합니다. 마지막 두 줄을 원본 목록에 대 한 데이터를 로드 한 모든 그룹을 확장 합니다.

```csharp
// Display side list
SourceList.ReloadData ();
SourceList.ExpandItem (null, true);
```

마지막으로 편집 합니다 `AppDelegate.cs` 파일을 확인 합니다 `DidFinishLaunching` 다음과 같은 메서드 확인:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    var rotation = new RotationWindowController ();
    rotation.Window.MakeKeyAndOrderFront (this);
}
```

응용 프로그램을 실행 하는 경우 다음 표시 됩니다.

[![](source-list-images/source05.png "앱의 예 실행")](source-list-images/source05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Mac 응용 프로그램에서 원본 목록을 사용 하 여 작업을 자세히 살펴보려면을 걸렸습니다. 만들고 Xcode의 Interface Builder에서 원본 목록을 유지 관리 하는 방법 및 C# 코드에서 원본 목록을 사용 하는 방법에 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [MacOutlines (샘플)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [테이블 보기](~/mac/user-interface/table-view.md)
- [개요 보기](~/mac/user-interface/outline-view.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [출 선 보기 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
