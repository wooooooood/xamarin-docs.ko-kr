---
title: Xamarin.ios의 원본 목록
description: 이 문서에서는 Xamarin.ios 응용 프로그램의 원본 목록 작업에 대해 설명 합니다. Xcode에서 소스 목록을 만들고 유지 관리 하는 방법과 Interface Builder 하 고 코드에서 C# 상호 작용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 651A3649-5AA8-4133-94D6-4873D99F7FCC
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 33ef45fa08748e70ef376e43cb5ed9b12ba55198
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655267"
---
# <a name="source-lists-in-xamarinmac"></a>Xamarin.ios의 원본 목록

_이 문서에서는 Xamarin.ios 응용 프로그램의 원본 목록 작업에 대해 설명 합니다. Xcode에서 소스 목록을 만들고 유지 관리 하는 방법과 Interface Builder 하 고 코드에서 C# 상호 작용 하는 방법을 설명 합니다._

Xamarin.ios 응용 프로그램 C# 에서 및 .net을 사용 하는 경우 *목표-C* 및 *Xcode* 에서 작업 하는 개발자가 동일한 소스 목록에 액세스할 수 있습니다. Xamarin.ios는 Xcode와 직접 통합 되므로 Xcode의 _Interface Builder_ 를 사용 하 여 소스 목록을 만들고 유지 관리 하거나 (필요에 따라 코드에서 C# 직접 만들 수 있습니다.)

원본 목록은 Finder 또는 iTunes의 사이드 표시줄과 같은 동작의 원본을 표시 하는 데 사용 되는 특별 한 유형의 개요 뷰입니다.

[![](source-list-images/source05.png "예제 소스 목록")](source-list-images/source05.png#lightbox)

이 문서에서는 Xamarin.ios 응용 프로그램의 원본 목록 작업에 대 한 기본 사항을 다룹니다. [Hello, Mac](~/mac/get-started/hello-mac.md) 문서를 먼저 사용 하는 것이 가장 좋습니다. 특히 [Xcode 및 Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 및 [콘센트 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션을 소개 하 고,에서 사용할 주요 개념 및 기술을 설명 하 고 있습니다. 이 문서를 참조 하세요.

[Xamarin.ios 내부](~/mac/internals/how-it-works.md) 문서의 [클래스/메서드를 대상으로 노출 C# -C](~/mac/internals/how-it-works.md) 섹션을 살펴볼 수 있습니다. C# 클래스를 연결 하는 데 사용 되는 `Register` 및 `Export` 명령을 설명 합니다. 목표-C 개체 및 UI 요소입니다.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-source-lists"></a>원본 목록 소개

위에서 설명한 것 처럼 원본 목록은 Finder 또는 iTunes의 사이드 표시줄과 같은 동작의 원본을 표시 하는 데 사용 되는 특별 한 유형의 개요 뷰입니다. 원본 목록은 사용자가 계층적 데이터의 행을 확장 하거나 축소할 수 있도록 하는 테이블 유형입니다. 테이블 뷰와 달리 원본 목록의 항목은 단순 목록에 있지 않으며 하드 드라이브의 파일 및 폴더와 같은 계층 구조로 구성 됩니다. 소스 목록의 항목에 다른 항목이 포함 되어 있으면 사용자가 확장 하거나 축소할 수 있습니다.

원본 목록은 특별히 스타일을 지정 하는 개요 보기`NSOutlineView`()로 서, 자체는 테이블 뷰 (`NSTableView`)의 서브 클래스 이며, 그에 따라 부모 클래스에서 대부분의 동작을 상속 합니다. 따라서 개요 보기에서 지원 되는 많은 작업은 원본 목록 에서도 지원 됩니다. Xamarin.ios 응용 프로그램은 이러한 기능을 제어할 수 있으며, 코드 또는 Interface Builder에서 소스 목록의 매개 변수를 구성 하 여 특정 작업을 허용 하거나 허용 하지 않을 수 있습니다.

원본 목록은 자체 데이터를 저장 하지 않으며, 대신 데이터 원본 (`NSOutlineViewDataSource`)을 사용 하 여 필요한 경우 필요한 행과 열을 모두 제공 합니다.

개요 유형 ()을 지원 하 여 기능, 항목 선택 및 편집, 사용자 지정 추적`NSOutlineViewDelegate`, 개별 항목에 대 한 사용자 지정 보기를 선택할 수 있도록 개요 뷰 대리자 ()의 하위 클래스를 제공 하 여 원본 목록의 동작을 사용자 지정할 수 있습니다.

원본 목록에서는 대부분의 동작 및 기능을 테이블 뷰와 개요 보기와 공유 하므로이 문서를 계속 하기 전에 [테이블 보기](~/mac/user-interface/table-view.md) 및 [개요 보기](~/mac/user-interface/outline-view.md) 문서를 진행 하는 것이 좋습니다.

<a name="Working_with_Source_Lists" />

## <a name="working-with-source-lists"></a>원본 목록 작업

원본 목록은 Finder 또는 iTunes의 사이드 표시줄과 같은 동작의 원본을 표시 하는 데 사용 되는 특별 한 유형의 개요 뷰입니다. 개요 뷰와 달리 Interface Builder에서 원본 목록을 정의 하기 전에 Xamarin.ios에서 지원 클래스를 만들어 보겠습니다.

먼저 원본 목록에 대 한 데이터 `SourceListItem` 를 보유할 새 클래스를 만들어 보겠습니다. **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다. `SourceListItem` **일반**빈 클래스를 선택 하 고 이름으로를 입력 한 다음 새로 만들기 단추를 클릭 합니다. > 

[![](source-list-images/source01.png "빈 클래스 추가")](source-list-images/source01.png#lightbox)

파일이 다음과 `SourceListItem.cs` 같이 표시 되도록 합니다. 

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

**솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다. `SourceListDataSource` **일반**빈 클래스를 선택 하 고 이름으로를 입력 한 다음 새로 만들기 단추를 클릭 합니다. >  파일이 다음과 `SourceListDataSource.cs` 같이 표시 되도록 합니다.

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

그러면 원본 목록에 대 한 데이터가 제공 됩니다.

**솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다. `SourceListDelegate` **일반**빈 클래스를 선택 하 고 이름으로를 입력 한 다음 새로 만들기 단추를 클릭 합니다. >  파일이 다음과 `SourceListDelegate.cs` 같이 표시 되도록 합니다.

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

그러면 원본 목록의 동작이 제공 됩니다.

마지막으로 **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다. `SourceListView` **일반**빈 클래스를 선택 하 고 이름으로를 입력 한 다음 새로 만들기 단추를 클릭 합니다. >  파일이 다음과 `SourceListView.cs` 같이 표시 되도록 합니다.

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

그러면 사용자가 만든 모든 xamarin.ios 응용 프로그램 `NSOutlineView` 에서`SourceListView`원본 목록을 구동 하는 데 사용할 수 있는 ()의 재사용 가능한 사용자 지정 하위 클래스가 만들어집니다.

<a name="Creating_and_Maintaining_Source_Lists_in_Xcode" />

## <a name="creating-and-maintaining-source-lists-in-xcode"></a>Xcode에서 원본 목록 만들기 및 유지 관리

이제 Interface Builder에서 소스 목록을 디자인 하겠습니다. 파일을 `Main.storyboard` 두 번 클릭 하 여 열고 Interface Builder 편집을 위해 열고, **라이브러리 검사기**에서 분할 보기를 끌어서 뷰 컨트롤러에 추가 하 고, **제약 조건 편집기**에서 뷰를 사용 하 여 크기를 조정 하도록 설정 합니다.

[![](source-list-images/source00.png "제약 조건 편집")](source-list-images/source00.png#lightbox)

그런 다음 **라이브러리 검사자**에서 원본 목록을 끌어 분할 뷰의 왼쪽에 추가 하 고 **제약 조건 편집기**에서 뷰를 사용 하 여 크기를 조정 하도록 설정 합니다.

[![](source-list-images/source02.png "제약 조건 편집")](source-list-images/source02.png#lightbox)

그런 다음 **Id 뷰로**전환 하 고 원본 목록을 선택한 후 **클래스** 를로 `SourceListView`변경 합니다.

[![](source-list-images/source03.png "클래스 이름 설정")](source-list-images/source03.png#lightbox)

마지막으로, `ViewController.h` 파일 에서 호출 `SourceList` 되는 원본 목록의 콘센트를 만듭니다.

[![](source-list-images/source04.png "콘센트 구성")](source-list-images/source04.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

<a name="Populating the Source List" />

## <a name="populating-the-source-list"></a>소스 목록 채우기

Mac용 Visual Studio에서 `RotationWindow.cs` 파일을 편집 하 고 `AwakeFromNib` 메서드를 다음과 같이 설정 해 보겠습니다.

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

항목을 추가 _하기 전에_ 소스 목록의 **콘센트** 에 대해 메서드를호출해야합니다.`Initialize ()` 각 항목 그룹에 대해 부모 항목을 만든 다음 해당 그룹 항목에 하위 항목을 추가 합니다. 그러면 각 그룹이 원본 목록의 컬렉션 `SourceList.AddItem (...)`에 추가 됩니다. 마지막 두 줄은 원본 목록에 대 한 데이터를 로드 하 고 모든 그룹을 확장 합니다.

```csharp
// Display side list
SourceList.ReloadData ();
SourceList.ExpandItem (null, true);
```

마지막으로 `AppDelegate.cs` 파일을 편집 하 고 `DidFinishLaunching` 메서드를 다음과 같이 만듭니다.

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    var rotation = new RotationWindowController ();
    rotation.Window.MakeKeyAndOrderFront (this);
}
```

응용 프로그램을 실행 하는 경우 다음이 표시 됩니다.

[![](source-list-images/source05.png "예제 앱 실행")](source-list-images/source05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 응용 프로그램의 원본 목록 작업에 대해 자세히 살펴봅니다. Xcode의 Interface Builder에서 소스 목록을 만들고 유지 관리 하는 방법 및 코드에서 C# 소스 목록을 사용 하는 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [MacOutlines (샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/macoutlines)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [테이블 보기](~/mac/user-interface/table-view.md)
- [개요 보기](~/mac/user-interface/outline-view.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [개요 보기 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
