---
title: 소스 목록
description: 이 문서에서는 소스 목록 Xamarin.Mac 응용 프로그램에서 사용 하 여 작업을 설명합니다. 만들기, Xcode 및 인터페이스 작성기에서 소스 목록을 유지 관리 및 C# 코드에서 상호 작용 하 설명 합니다.
ms.prod: xamarin
ms.assetid: 651A3649-5AA8-4133-94D6-4873D99F7FCC
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: a8d3a67768b9e47833d1819c3bf44774a52d2438
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="source-lists"></a>소스 목록

_이 문서에서는 소스 목록 Xamarin.Mac 응용 프로그램에서 사용 하 여 작업을 설명합니다. 만들기, Xcode 및 인터페이스 작성기에서 소스 목록을 유지 관리 및 C# 코드에서 상호 작용 하 설명 합니다._

C# 및.NET Xamarin.Mac 응용 프로그램에서에서 작업할 때는 동일 하 게 액세스할 수 있습니다 소스 개발자의 작업 목록은 *Objective-c* 및 *Xcode* 않습니다. Xamarin.Mac Xcode에 직접 통합을 사용 하면 Xcode의 _인터페이스 작성기_ 만들기 및 소스 목록 유지 관리 (또는 C# 코드에서 직접 만들 필요에 따라).

소스 목록에는 소스 찾기 또는 iTunes에서 세로 막대와 같은 작업을 표시 하는 데 사용 되는 개요 보기의 특별 한 형식입니다.

[![](source-list-images/source05.png "예제 원본 목록")](source-list-images/source05.png#lightbox)

이 문서에서는의 기본적인 Xamarin.Mac 응용 프로그램에서 소스 목록 사용 하겠습니다. 것이 가장 좋습니다를 통해 협력 하는 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서는 [Xcode 및 인터페이스 작성기 소개](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) 및 [콘센트 및 동작](~/mac/get-started/hello-mac.md#Outlets_and_Actions) 섹션으로이 문서에서 사용할 수 있는 주요 개념 및 기술을 설명 합니다.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명도 문서는 `Register` 및 `Export` 명령 와이어 접속 C# 클래스 Objective-c 개체 및 UI 요소에 하는 데 사용 합니다.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-source-lists"></a>소스 목록 소개

위에서 설명 했 듯이 소스 목록은 Finder 또는 iTunes에서 세로 막대와 같은 작업의 소스를 표시 하는 데는 특별 한 유형의 개요 보기입니다. 소스 목록은 사용자 수 있는 테이블의 형식을 확장 하거나 계층적 데이터의 행을 축소 합니다. 표 보기와 달리 플랫 목록에 없는 원본 목록에 있는 항목, 하드 드라이브에서 파일 및 폴더와 같은 계층에 구성 된 합니다. 소스 목록에 있는 항목에 다른 항목이 포함 된 경우 확장 또는 사용자가을 축소할 수 있습니다.

소스 목록은 특수 스타일의 개요 보기 (`NSOutlineView`), 표 보기의 하위 클래스는 논리 (`NSTableView`) 이므로 해당 부모 클래스에서 상속 하는 동작의 대부분이 및 합니다. 결과적으로, 개요 보기에서 지 원하는 많은 작업 소스 목록 에서도 지원 됩니다. Xamarin.Mac 응용 프로그램을 이러한 기능을 제어 하는 및를 허용할지 또는 특정 작업을 허용 하지 않습니다 (코드 또는 인터페이스 작성기 중 하나) 소스 목록 매개 변수를 구성할 수 있습니다.

원본 목록 자신의 데이터를 저장 하지 않으므로, 데이터 원본에서 대신 (`NSOutlineViewDataSource`) 행과 열에는 필요에 따라 필요한 수 있도록 합니다.

개요 보기 대리자의 서브 클래스를 제공 하 여 원본 목록의 동작을 사용자 지정할 수 있습니다 (`NSOutlineViewDelegate`) 선택 및 편집, 사용자 지정 추적 및 개별 항목에 대 한 사용자 지정 보기에 항목을 기능 선택 개요 구분 형식을 지원 합니다.

통해 이동 하려는 표 보기 및 개요 보기와의 동작 및 기능 많이 공유 하는 원본 목록, 이후 우리의 [테이블 뷰](~/mac/user-interface/table-view.md) 및 [개요 뷰](~/mac/user-interface/outline-view.md) 계속 하기 전에 문서 이 문서입니다.

<a name="Working_with_Source_Lists" />

## <a name="working-with-source-lists"></a>소스 목록 작업

소스 목록에는 소스 찾기 또는 iTunes에서 세로 막대와 같은 작업을 표시 하는 데 사용 되는 개요 보기의 특별 한 형식입니다. 개요 보기와 달리 소스 목록 작성기 인터페이스를 정의 하기 전에 지원 클래스를 보겠습니다 Xamarin.Mac에 만듭니다.

첫째, 새를 만들어 보겠습니다 `SourceListItem` 소스 목록에 대 한 데이터를 보유 하는 클래스입니다. 에 **솔루션 탐색기**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...** 선택 **일반** > **빈 클래스**, 입력 `SourceListItem` 에 대 한는 **이름** 클릭는 **새로** 단추:

[![](source-list-images/source01.png "빈 클래스 추가")](source-list-images/source01.png#lightbox)

확인 된 `SourceListItem.cs` 다음과 같은 파일 보기: 

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

에 **솔루션 탐색기**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...** 선택 **일반** > **빈 클래스**, 입력 `SourceListDataSource` 에 대 한는 **이름** 클릭는 **새로** 단추입니다. 확인 된 `SourceListDataSource.cs` 다음과 같은 파일 보기:

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

이렇게 하면 데이터 소스 목록에 대 한 제공 됩니다.

에 **솔루션 탐색기**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...** 선택 **일반** > **빈 클래스**, 입력 `SourceListDelegate` 에 대 한는 **이름** 클릭는 **새로** 단추입니다. 확인 된 `SourceListDelegate.cs` 다음과 같은 파일 보기:

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

소스 목록의 동작을 제공 합니다.

마지막으로 **솔루션 탐색기**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...** 선택 **일반** > **빈 클래스**, 입력 `SourceListView` 에 대 한는 **이름** 클릭는 **새로** 단추입니다. 확인 된 `SourceListView.cs` 다음과 같은 파일 보기:

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

이 사용자 지정 하 고 재사용 가능한 서브 클래스를 만드는의 `NSOutlineView` (`SourceListView`)를 실행 하 고 원본 목록 하도록 하는 모든 Xamarin.Mac 응용 프로그램에서 사용할 수 있는 합니다.

<a name="Creating_and_Maintaining_Source_Lists_in_Xcode" />

## <a name="creating-and-maintaining-source-lists-in-xcode"></a>만들기 및 Xcode에서 소스 목록 유지 관리

이제 인터페이스 작성기의 소스 목록 설계 해 보겠습니다. 두 번 클릭는 `Main.storyboard` 파일 인터페이스 작성기에서 편집을 위해 열고에서 분할 뷰를 끌어 놓을 **라이브러리 관리자**을 보기 컨트롤러에 추가 하 고 보기에 크기를 조정 하도록 설정 된 **제약 조건 편집기** :

[![](source-list-images/source00.png "제약 조건 편집")](source-list-images/source00.png#lightbox)

원본 목록에서를 끌어는 **라이브러리 관리자**을 분할 보기의 왼쪽에 추가 하 고 보기에 크기를 조정 하도록 설정 된 **제약 조건 편집기**:

[![](source-list-images/source02.png "제약 조건 편집")](source-list-images/source02.png#lightbox)

다음으로 전환 하는 **Identity 보기**, 소스 목록에서 선택 하 고 변경의 **클래스** 를 `SourceListView`:

[![](source-list-images/source03.png "설정 클래스 이름")](source-list-images/source03.png#lightbox)

마지막으로 작성 한 **콘센트** 호출 된 원본 목록 `SourceList` 에 `ViewController.h` 파일:

[![](source-list-images/source04.png "콘센트에 연결 구성")](source-list-images/source04.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

<a name="Populating the Source List" />

## <a name="populating-the-source-list"></a>소스 목록 채우기

편집 된 `RotationWindow.cs` Mac 용 Visual Studio에서 파일을 쉽게의 `AwakeFromNib` 다음과 같은 메서드 모양:

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

`Initialize ()` 메서드는 소스 목록에 대해 호출할 필요가 **콘센트** _전에_ 모든 항목이 추가 됩니다. 각 항목 그룹에 대 한 부모 항목에는 만든 하 다음 해당 그룹 항목에 하위 항목을 추가 합니다. 각 그룹 소스 목록 컬렉션에 추가 됩니다 `SourceList.AddItem (...)`합니다. 마지막 두 줄 소스 목록에 대 한 데이터를 로드 하 고 모든 그룹을 확장 합니다.

```csharp
// Display side list
SourceList.ReloadData ();
SourceList.ExpandItem (null, true);
```

마지막으로 편집 된 `AppDelegate.cs` 파일을 확인는 `DidFinishLaunching` 다음과 같은 메서드 모양:

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

[![](source-list-images/source05.png "실행 하는 예제 응용 프로그램")](source-list-images/source05.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에 자세히 살펴보고 소스 목록 작업 Xamarin.Mac 응용 프로그램에서 수행 했습니다. 만들고 소스 Xcode의 인터페이스 구성 관리자에서 목록 유지 관리 하는 방법과 소스 목록 C# 코드에서 작업 하는 방법에 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [MacOutlines (샘플)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [테이블 보기](~/mac/user-interface/table-view.md)
- [개요 보기](~/mac/user-interface/outline-view.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [개요 보기 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
