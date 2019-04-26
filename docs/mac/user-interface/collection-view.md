---
title: Xamarin.mac에서 컬렉션 뷰
description: 이 문서에서는 Xamarin.Mac 앱에서 컬렉션 뷰를 사용 하 여 작업을 설명 합니다. Xcode 및 Interface Builder에서 컬렉션 뷰를 유지 관리 만들고 프로그래밍 방식으로 작업 하는 내용을 다룹니다.
ms.prod: xamarin
ms.assetid: 6EE32256-5948-4AE4-8133-6D0B3F4173E8
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 05/24/2017
ms.openlocfilehash: 82dd175222289676c3dacd80522c55e71ba12180
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61236028"
---
# <a name="collection-views-in-xamarinmac"></a>Xamarin.mac에서 컬렉션 뷰

_이 문서에서는 Xamarin.Mac 앱에서 컬렉션 뷰를 사용 하 여 작업을 설명 합니다. Xcode 및 Interface Builder에서 컬렉션 뷰를 유지 관리 만들고 프로그래밍 방식으로 작업 하는 내용을 다룹니다._

작업할 때 C# Xamarin.Mac 앱에서.NET 개발자가에 대 한 액세스 및 동일한 AppKit 컬렉션 뷰 컨트롤에서 작업을 수행할 *Objective-c* 하 고 *Xcode* 않습니다. 개발자는 Xcode의를 사용 하 여 Xamarin.Mac이 Xcode와 직접 통합 되므로 _Interface Builder_ 를 만들어 컬렉션 뷰를 유지 관리 합니다.

A `NSCollectionView` 표를 사용 하 여 구성 하는 하위 뷰의 표시는 `NSCollectionViewLayout`합니다. 표의 각 하위 뷰 표시 됩니다는 `NSCollectionViewItem` 뷰의 콘텐츠의 로드를 관리 하는 `.xib` 파일입니다.

[![앱의 예 실행](collection-view-images/intro01.png)](collection-view-images/intro01.png#lightbox)

이 문서에서는 Xamarin.Mac 앱에서 컬렉션 뷰를 사용 하는 기본 사항을 다룹니다. 것이 가장 좋습니다를 통해 작업 하는 합니다 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서 합니다 [Xcode 및 Interface Builder 소개](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 하 고 [출 선 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션으로 주요 개념 및이 문서 전체에서 사용 되는 기술을 설명 합니다.

확인 하려는 경우는 [노출 C# 클래스 / objective-c 메서드](~/mac/internals/how-it-works.md) 섹션을 [Xamarin.Mac 내부 요소](~/mac/internals/how-it-works.md) 설명도 문서를 `Register` 및 `Export` 명령 하 여 통신-C# 클래스 Objective-c 개체 및 UI 요소에 사용 합니다.

<a name="About_Collection_Views"/>

## <a name="about-collection-views"></a>컬렉션 뷰 정보

컬렉션 뷰는 주요 목적은 (`NSCollectionView`) 시각적 개체 컬렉션 보기 레이아웃을 사용 하 여 구성 된 방식으로 그룹을 정렬 하는 것 (`NSCollectionViewLayout`), 각 개별 개체를 사용 하 여 (`NSCollectionViewItem`) 더 큰 컬렉션에서 고유한 보기를 시작 합니다. 컬렉션 뷰는 데이터 바인딩 및 키-값 코딩 기술을 통해 작동 하 고 이와 같이 읽어야 하는 [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md) 이 문서를 진행 하기 전에 설명서.

개발자는 디자인 및 구현 하는 일을 담당 하므로 컬렉션 보기에 없는 표준, 기본 제공 컬렉션 보기 항목 (예: 개요 또는 테이블 뷰 않습니다)는 _프로토타입 보기_ 이미지 필드와 같은 다른 AppKit 컨트롤을 사용 하 여 텍스트 필드, 레이블 등입니다. 이 프로토타입 표시 하 고 컬렉션 보기에 의해 관리 되는 각 항목을 사용 하 여 작업 하는 데 사용할 뷰와에 저장 되는 `.xib` 파일입니다.

개발자 컬렉션 보기 항목의 모양과 느낌 담당 하기 때문에 컬렉션 뷰에 있는지 표에 선택한 항목을 강조 표시에 대 한 기본 제공 지원 되지 않습니다. 이 기능의 구현은이 문서에서 설명 합니다.

<a name="Defining_your_Data_Model"/>

## <a name="defining-the-data-model"></a>데이터 모델 정의

에서 데이터 바인딩 컬렉션 뷰 Interface Builder를 키-값 코딩 (KVC) 하기 전에 / 역할을 Xamarin.Mac 앱의 키-값을 관찰 (KVO) 호환 클래스를 정의 해야 합니다 _데이터 모델_ 바인딩에 대 한 합니다. 데이터 모델의 모든 컬렉션에 표시 됩니다 하 고 응용 프로그램을 실행 하는 동안 UI에서 사용자가 데이터에 대 한 수정은 받는 데이터를 제공 합니다.

직원의 그룹을 관리 하는 앱의 예로, 다음 클래스는 데이터 모델 정의를 사용할 수 있습니다.

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        #endregion

        #region Computed Properties
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon
        {
            get
            {
                if (isManager)
                {
                    return NSImage.ImageNamed("IconGroup");
                }
                else
                {
                    return NSImage.ImageNamed("IconUser");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion
    }
}
```

`PersonModel` 이 문서의 나머지 부분 전체에서 사용할 데이터 모델입니다.

<a name="Working_with_a_Collection_View"/>

## <a name="working-with-a-collection-view"></a>컬렉션 뷰를 사용 하 여 작업

컬렉션 뷰를 사용 하 여 데이터 바인딩은 표 보기를 사용 하 여 매우 많은 like 바인딩을 `NSCollectionViewDataSource` 컬렉션에 대 한 데이터를 제공 하는 데 사용 됩니다. 컬렉션 뷰에서 미리 설정 된 표시 형식 없으므로 더 많은 작업이 필요 사용자 상호 작용 피드백을 제공 하 고 사용자가 선택한 추적 합니다.

<a name="Creating-the-Cell-Prototype"/>

### <a name="creating-the-cell-prototype"></a>셀 프로토타입 만들기

컬렉션 뷰에서 기본 셀 프로토타입을 포함 하지 않습니다 되므로 개발자는 하나 이상의 추가 해야 `.xib` Xamarin.Mac 앱 레이아웃 및 개별 셀의 내용을 정의 하는 파일입니다.

다음을 수행합니다.

1. 에 **솔루션 탐색기**, 프로젝트 이름을 마우스 오른쪽 단추로 **추가** > **새 파일...**
2. 선택 **Mac** > **뷰 컨트롤러**, 이름을 (같은 `EmployeeItem` 이 예제의) 클릭 합니다 **새로 만들기** 만들기 단추: 

    ![새 뷰 컨트롤러를 추가합니다.](collection-view-images/proto01.png)

    이렇게 하면 추가 됩니다는 `EmployeeItem.cs`, `EmployeeItemController.cs` 및 `EmployeeItemController.xib` 프로젝트의 솔루션 파일입니다.
3. 두 번 클릭 합니다 `EmployeeItemController.xib` Xcode의 Interface Builder에서 편집용으로 열 파일입니다.
4. 추가 `NSBox`, `NSImageView` 두 개의 `NSLabel` 보기에 컨트롤 배치 하 여 다음과 같이 및:

    ![셀 프로토타입의 레이아웃 디자인](collection-view-images/proto02.png)
5. 엽니다는 **도우미 편집기** 만들고는 **출 선** 에 대 한는 `NSBox` 셀의 선택 상태를 나타내기 위해 사용할 수 있도록:

    ![출 선에서 NSBox 노출](collection-view-images/proto03.png)
6. 반환 합니다 **표준 편집기** 이미지 보기를 선택 합니다.
7. 에 **바인딩 검사기**를 선택 **바인딩** > **파일의 소유자** enter를 **모델 키 경로** 의 `self.Person.Icon`:

    ![바인딩 아이콘](collection-view-images/proto04.png)
8. 첫 번째 레이블을 선택 및 합니다 **바인딩 검사기**를 선택 **바인딩** > **파일의 소유자** enter를 **모델 키 경로**의 `self.Person.Name`:

    ![이름 바인딩](collection-view-images/proto05.png)
9. 두 번째 레이블을 선택 및 합니다 **바인딩 검사기**를 선택 **바인딩** > **파일의 소유자** enter를 **모델 키 경로**의 `self.Person.Occupation`:

    ![직업 바인딩](collection-view-images/proto06.png)
10. 변경 내용을 저장 합니다 `.xib` 파일 하 고 변경 내용을 동기화 하려면 Visual Studio로 돌아갑니다.

편집 된 `EmployeeItemController.cs` 파일을 다음과 같이 표시 되도록 합니다.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Foundation;
using AppKit;

namespace MacCollectionNew
{
    /// <summary>
    /// The Employee item controller handles the display of the individual items that will
    /// be displayed in the collection view as defined in the associated .XIB file.
    /// </summary>
    public partial class EmployeeItemController : NSCollectionViewItem
    {
        #region Private Variables
        /// <summary>
        /// The person that will be displayed.
        /// </summary>
        private PersonModel _person;
        #endregion

        #region Computed Properties
        // strongly typed view accessor
        public new EmployeeItem View
        {
            get
            {
                return (EmployeeItem)base.View;
            }
        }

        /// <summary>
        /// Gets or sets the person.
        /// </summary>
        /// <value>The person that this item belongs to.</value>
        [Export("Person")]
        public PersonModel Person
        {
            get { return _person; }
            set
            {
                WillChangeValue("Person");
                _person = value;
                DidChangeValue("Person");
            }
        }

        /// <summary>
        /// Gets or sets the color of the background for the item.
        /// </summary>
        /// <value>The color of the background.</value>
        public NSColor BackgroundColor {
            get { return Background.FillColor; }
            set { Background.FillColor = value; }
        }

        /// <summary>
        /// Gets or sets a value indicating whether this <see cref="T:MacCollectionNew.EmployeeItemController"/> is selected.
        /// </summary>
        /// <value><c>true</c> if selected; otherwise, <c>false</c>.</value>
        /// <remarks>This also changes the background color based on the selected state
        /// of the item.</remarks>
        public override bool Selected
        {
            get
            {
                return base.Selected;
            }
            set
            {
                base.Selected = value;

                // Set background color based on the selection state
                if (value) {
                    BackgroundColor = NSColor.DarkGray;
                } else {
                    BackgroundColor = NSColor.LightGray;
                }
            }
        }
        #endregion

        #region Constructors
        // Called when created from unmanaged code
        public EmployeeItemController(IntPtr handle) : base(handle)
        {
            Initialize();
        }

        // Called when created directly from a XIB file
        [Export("initWithCoder:")]
        public EmployeeItemController(NSCoder coder) : base(coder)
        {
            Initialize();
        }

        // Call to load from the XIB/NIB file
        public EmployeeItemController() : base("EmployeeItem", NSBundle.MainBundle)
        {
            Initialize();
        }

        // Added to support loading from XIB/NIB
        public EmployeeItemController(string nibName, NSBundle nibBundle) : base(nibName, nibBundle) {

            Initialize();
        }

        // Shared initialization code
        void Initialize()
        {
        }
        #endregion
    }
}
```

클래스 상속 세부 정보에서이 코드를 살펴보면 `NSCollectionViewItem` 컬렉션 뷰 셀에 대 한 프로토타입이으로 작동할 수 있도록 합니다. `Person` 속성은 이미지 뷰 및 Xcode의 레이블 지정에 데이터 바인딩하는 데 사용한 하는 클래스를 노출 합니다. 인스턴스는 `PersonModel` 위에서 만든 합니다.

`BackgroundColor` 속성은 바로 가기를 합니다 `NSBox` 컨트롤의 `FillColor` 셀의 선택 상태를 표시 하는 데 수. 재정의 하 여를 `Selected` 의 속성을 `NSCollectionViewItem`, 다음 코드를 설정 하거나이 선택 상태를 지웁니다.

```csharp
public override bool Selected
{
    get
    {
        return base.Selected;
    }
    set
    {
        base.Selected = value;

        // Set background color based on the selection state
        if (value) {
            BackgroundColor = NSColor.DarkGray;
        } else {
            BackgroundColor = NSColor.LightGray;
        }
    }
}
```

<a name="Creating-the-Collection-View-Data-Source"/>

### <a name="creating-the-collection-view-data-source"></a>컬렉션 뷰에서 데이터 원본 만들기

컬렉션 뷰에서 데이터 원본 (`NSCollectionViewDataSource`) 컬렉션 보기에 대 한 모든 데이터를 제공 하 고 만들고 컬렉션 뷰 셀을 채웁니다 (사용 하 여는 `.xib` 프로토타입) 컬렉션의 각 항목에 대해 필요에 따라 합니다.

호출 프로젝트에 새 클래스를 추가 `CollectionViewDataSource` 다음과 같을 수 있도록 합니다.

```csharp
using System;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacCollectionNew
{
    /// <summary>
    /// Collection view data source provides the data for the collection view.
    /// </summary>
    public class CollectionViewDataSource : NSCollectionViewDataSource
    {
        #region Computed Properties
        /// <summary>
        /// Gets or sets the parent collection view.
        /// </summary>
        /// <value>The parent collection view.</value>
        public NSCollectionView ParentCollectionView { get; set; }

        /// <summary>
        /// Gets or sets the data that will be displayed in the collection.
        /// </summary>
        /// <value>A collection of PersonModel objects.</value>
        public List<PersonModel> Data { get; set; } = new List<PersonModel>();
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.CollectionViewDataSource"/> class.
        /// </summary>
        /// <param name="parent">The parent collection that this datasource will provide data for.</param>
        public CollectionViewDataSource(NSCollectionView parent)
        {
            // Initialize
            ParentCollectionView = parent;

            // Attach to collection view
            parent.DataSource = this;

        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Gets the number of sections.
        /// </summary>
        /// <returns>The number of sections.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        public override nint GetNumberOfSections(NSCollectionView collectionView)
        {
            // There is only one section in this view
            return 1;
        }

        /// <summary>
        /// Gets the number of items in the given section.
        /// </summary>
        /// <returns>The number of items.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="section">The Section number to count items for.</param>
        public override nint GetNumberofItems(NSCollectionView collectionView, nint section)
        {
            // Return the number of items
            return Data.Count;
        }

        /// <summary>
        /// Gets the item for the give section and item index.
        /// </summary>
        /// <returns>The item.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPath">Index path specifying the section and index.</param>
        public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
        {
            var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
            item.Person = Data[(int)indexPath.Item];

            return item;
        }
        #endregion
    }
}
```

클래스 상속 세부 정보에서이 코드를 살펴보면 `NSCollectionViewDataSource` 목록을 노출 `PersonModel` 인스턴스를 통해 해당 `Data` 속성입니다.

이 컬렉션에는 하나의 섹션에 있는 경우 하므로 코드를 재정의 합니다 `GetNumberOfSections` 메서드 및 항상 반환 `1`. 또한 합니다 `GetNumberofItems` 메서드를 재정의에 있는 항목의 수를 반환 합니다 `Data` 속성 목록입니다.

`GetItem` 새 셀을 필수 항목이 며 다음과 같이 표시 될 때마다 호출 됩니다.

```csharp
public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
{
    var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
    item.Person = Data[(int)indexPath.Item];

    return item;
}
```

`MakeItem` 만들거나의 재사용 가능한 인스턴스를 반환 된 컬렉션 뷰의 메서드가 호출 되어 합니다 `EmployeeItemController` 및 해당 `Person` 속성 항목 요청 된 셀에 표시 되 고 합니다. 

`EmployeeItemController` 다음 코드를 사용 하 여 미리 컬렉션 뷰 컨트롤러에 등록 해야 합니다.

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
``` 

합니다 **식별자** (`EmployeeCell`)에 사용 되는 합니다 `MakeItem` 호출 _해야_ 컬렉션 뷰를 사용 하 여 등록 된 뷰 컨트롤러의 이름과 일치 합니다. 이 단계는 아래에서 자세히 설명 합니다.

<a name="Handling-Item-Selection"/>

### <a name="handling-item-selection"></a>처리 항목 선택

선택 및 선택 항목 컬렉션에서의 취소를 처리 하는 `NSCollectionViewDelegate` 필요 합니다. 이 예제에서는 기본 제공에서 사용할 때문 `NSCollectionViewFlowLayout` 레이아웃 형식을 `NSCollectionViewDelegateFlowLayout` 이 대리자의 특정 버전을 해야 합니다.

호출 프로젝트에 새 클래스를 추가 `CollectionViewDelegate` 다음과 같을 수 있도록 합니다.

```csharp
using System;
using Foundation;
using AppKit;

namespace MacCollectionNew
{
    /// <summary>
    /// Collection view delegate handles user interaction with the elements of the 
    /// collection view for the Flow-Based layout type.
    /// </summary>
    public class CollectionViewDelegate : NSCollectionViewDelegateFlowLayout
    {
        #region Computed Properties
        /// <summary>
        /// Gets or sets the parent view controller.
        /// </summary>
        /// <value>The parent view controller.</value>
        public ViewController ParentViewController { get; set; }
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.CollectionViewDelegate"/> class.
        /// </summary>
        /// <param name="parentViewController">Parent view controller.</param>
        public CollectionViewDelegate(ViewController parentViewController)
        {
            // Initialize
            ParentViewController = parentViewController;
        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Handles one or more items being selected.
        /// </summary>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPaths">The Index paths of the items being selected.</param>
        public override void ItemsSelected(NSCollectionView collectionView, NSSet indexPaths)
        {
            // Dereference path
            var paths = indexPaths.ToArray<NSIndexPath>();
            var index = (int)paths[0].Item;

            // Save the selected item
            ParentViewController.PersonSelected = ParentViewController.Datasource.Data[index];

        }

        /// <summary>
        /// Handles one or more items being deselected.
        /// </summary>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPaths">The Index paths of the items being deselected.</param>
        public override void ItemsDeselected(NSCollectionView collectionView, NSSet indexPaths)
        {
            // Dereference path
            var paths = indexPaths.ToArray<NSIndexPath>();
            var index = paths[0].Item;

            // Clear selection
            ParentViewController.PersonSelected = null;
        }
        #endregion
    }
}
``` 

합니다 `ItemsSelected` 및 `ItemsDeselected` 메서드 재정의 되며 설정 또는 해제 하는 데는 `PersonSelected` 컬렉션 뷰를 선택 하거나 항목을 선택 취소 하는 경우 처리 하는 뷰 컨트롤러의 속성입니다. 이 아래에 자세히 표시 됩니다.

<a name="Creating-the-Collection-View-in-Interface-Builder"/>

### <a name="creating-the-collection-view-in-interface-builder"></a>Interface Builder에서 컬렉션 뷰 만들기

모든 위치에 필요한 지원 부분이 사용 하 여 주 스토리 보드 편집할 수 있으며 컬렉션 뷰를 추가 합니다.

다음을 수행합니다.

1. 두 번 클릭 합니다 `Main.Storyboard` 파일를 **솔루션 탐색기** 의 Interface Builder를 Xcode에서 편집을 위해 엽니다.
2. 컬렉션 뷰를 기본 뷰로 끌어서 뷰를 채우도록 크기를 조정:

    ![컬렉션 뷰 레이아웃 추가](collection-view-images/collection01.png)
3. 선택한 컬렉션 뷰를 사용 하 여 크기가 조정 되 면 보기에 고정 하려면 제약 조건 편집기를 사용 합니다.

    ![제약 조건 추가](collection-view-images/collection02.png)
4. 컬렉션 뷰에서 선택 되어 있는지 확인 합니다 **디자인 화면** (아니라 합니다 **테두리가 있는 스크롤 뷰** 또는 **클립 보기** 포함 된), 전환할는  **도우미 편집기** 만들고는 **출 선** 컬렉션 뷰에 대 한 합니다.

    ![제약 조건 추가](collection-view-images/collection03.png)
5. 변경 내용을 저장 하 고 동기화를 Visual Studio로 돌아갑니다.

<a name="Bringing-it-all-Together"/>

## <a name="bringing-it-all-together"></a>모든 통합 상태로 전환

데이터 모델로 사용할 클래스를 사용 하 여 현재 위치에 이제 배치 된 모든 지원 부분이 (`PersonModel`), `NSCollectionViewDataSource` 추가한 데이터를 제공 하는 `NSCollectionViewDelegateFlowLayout` 항목 선택을 처리 하려면 생성 된 및 `NSCollectionView` 주 스토리 보드에 추가 된 출 선으로 표시 (`EmployeeCollection`).

마지막 단계는 컬렉션 뷰를 포함 하는 뷰 컨트롤러를 편집 하 고 모든 부분이 함께 하는 컬렉션을 채우는 항목 선택을 처리를 제공 하는 것입니다.

편집 된 `ViewController.cs` 파일을 다음과 같이 표시 되도록 합니다.

```csharp
using System;
using AppKit;
using Foundation;
using CoreGraphics;

namespace MacCollectionNew
{
    /// <summary>
    /// The View controller controls the main view that houses the Collection View.
    /// </summary>
    public partial class ViewController : NSViewController
    {
        #region Private Variables
        private PersonModel _personSelected;
        private bool shouldEdit = true;
        #endregion

        #region Computed Properties
        /// <summary>
        /// Gets or sets the datasource that provides the data to display in the 
        /// Collection View.
        /// </summary>
        /// <value>The datasource.</value>
        public CollectionViewDataSource Datasource { get; set; }

        /// <summary>
        /// Gets or sets the person currently selected in the collection view.
        /// </summary>
        /// <value>The person selected or <c>null</c> if no person is selected.</value>
        [Export("PersonSelected")]
        public PersonModel PersonSelected
        {
            get { return _personSelected; }
            set
            {
                WillChangeValue("PersonSelected");
                _personSelected = value;
                DidChangeValue("PersonSelected");
                RaiseSelectionChanged();
            }
        }
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.ViewController"/> class.
        /// </summary>
        /// <param name="handle">Handle.</param>
        public ViewController(IntPtr handle) : base(handle)
        {
        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Called after the view has finished loading from the Storyboard to allow it to
        /// be configured before displaying to the user.
        /// </summary>
        public override void ViewDidLoad()
        {
            base.ViewDidLoad();

            // Initialize Collection View
            ConfigureCollectionView();
            PopulateWithData();
        }
        #endregion

        #region Private Methods
        /// <summary>
        /// Configures the collection view.
        /// </summary>
        private void ConfigureCollectionView()
        {
            EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");

            // Create a flow layout
            var flowLayout = new NSCollectionViewFlowLayout()
            {
                ItemSize = new CGSize(150, 150),
                SectionInset = new NSEdgeInsets(10, 10, 10, 20),
                MinimumInteritemSpacing = 10,
                MinimumLineSpacing = 10
            };
            EmployeeCollection.WantsLayer = true;

            // Setup collection view
            EmployeeCollection.CollectionViewLayout = flowLayout;
            EmployeeCollection.Delegate = new CollectionViewDelegate(this);

        }

        /// <summary>
        /// Populates the Datasource with data and attaches it to the collection view.
        /// </summary>
        private void PopulateWithData()
        {
            // Make datasource
            Datasource = new CollectionViewDataSource(EmployeeCollection);

            // Build list of employees
            Datasource.Data.Add(new PersonModel("Craig Dunn", "Documentation Manager", true));
            Datasource.Data.Add(new PersonModel("Amy Burns", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Joel Martinez", "Web & Infrastructure"));
            Datasource.Data.Add(new PersonModel("Kevin Mullins", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Mark McLemore", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Tom Opgenorth", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Larry O'Brien", "API Docs Manager", true));
            Datasource.Data.Add(new PersonModel("Mike Norman", "API Documentor"));

            // Populate collection view
            EmployeeCollection.ReloadData();
        }
        #endregion

        #region Events
        /// <summary>
        /// Selection changed delegate.
        /// </summary>
        public delegate void SelectionChangedDelegate();

        /// <summary>
        /// Occurs when selection changed.
        /// </summary>
        public event SelectionChangedDelegate SelectionChanged;

        /// <summary>
        /// Raises the selection changed event.
        /// </summary>
        internal void RaiseSelectionChanged() {
            // Inform caller
            if (this.SelectionChanged != null) SelectionChanged();
        }
        #endregion
    }
}
```

이 코드를 자세히 수행를 `Datasource` 속성의 인스턴스를 포함 하도록 정의 된는 `CollectionViewDataSource` 컬렉션 뷰에 대 한 데이터를 제공 합니다. A `PersonSelected` 보유 하기 위해 속성 정의 되는 `PersonModel` 컬렉션 뷰에서 현재 선택한 항목을 나타내는입니다. 이 속성이 발생을 `SelectionChanged` 선택을 변경 하면 이벤트입니다.

`ConfigureCollectionView` 클래스에 다음 줄을 사용 하 여 컬렉션 뷰를 사용 하 여 셀 프로토타입으로 역할 하는 뷰 컨트롤러를 등록 하는 데 사용 됩니다.

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
```

있음을 합니다 **식별자** (`EmployeeCell`)에서 이라는 한 프로토타입 일치 항목을 등록 하는 데 사용 합니다 `GetItem` 메서드의 `CollectionViewDataSource` 위에 정의 된:

```csharp
var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
...
```

또한 뷰 컨트롤러의 유형을 **해야 합니다** 의 이름과 일치 합니다 `.xib` 프로토타입을 정의 하는 파일 **정확 하 게**합니다. 이 예제의 경우 `EmployeeItemController` 고 `EmployeeItemController.xib`입니다.

컬렉션 뷰의 항목의 실제 레이아웃 컬렉션 보기 레이아웃 클래스에 의해 제어 되 고 새 인스턴스를 할당 하 여 런타임에 동적으로 변경할 수 있습니다는 `CollectionViewLayout` 속성입니다. 컬렉션 뷰 모양을 변경에 애니메이션을 적용 하지 않고 업데이트이 속성을 변경 합니다.

Apple는 가장 일반적인 용도 처리 하는 컬렉션 뷰를 사용 하 여 두 개의 기본 제공 레이아웃 형식: `NSCollectionViewFlowLayout` 고 `NSCollectionViewGridLayout`입니다. 개발자 원에서 항목을 레이아웃 하는 등의 사용자 지정 형식으로 필요한 경우의 사용자 지정 인스턴스를 만들 수 있습니다 `NSCollectionViewLayout` 원하는 효과 달성 하기 위해 필요한 메서드를 재정의 합니다.

인스턴스를 만들고 있으므로이 예제에서는 기본 선형 레이아웃을 `NSCollectionViewFlowLayout` 클래스와 같이 구성 합니다.

```csharp
var flowLayout = new NSCollectionViewFlowLayout()
{
    ItemSize = new CGSize(150, 150),
    SectionInset = new NSEdgeInsets(10, 10, 10, 20),
    MinimumInteritemSpacing = 10,
    MinimumLineSpacing = 10
};
```

`ItemSize` 속성 컬렉션의 각 개별 셀의 크기를 정의 합니다. `SectionInset` 속성 셀에서 레이아웃 되는 컬렉션의 가장자리에서 너비를 정의 합니다. `MinimumInteritemSpacing` 항목 간의 최소 간격을 정의 하 고 `MinimumLineSpacing` 컬렉션에 있는 줄 사이의 최소 간격을 정의 합니다.

컬렉션 뷰에서의 인스턴스를 할당 된 레이아웃을 `CollectionViewDelegate` 항목 선택을 처리에 연결 됩니다.

```csharp
// Setup collection view
EmployeeCollection.CollectionViewLayout = flowLayout;
EmployeeCollection.Delegate = new CollectionViewDelegate(this);
```

합니다 `PopulateWithData` 메서드의 새 인스턴스를 만듭니다 합니다 `CollectionViewDataSource`데이터로 채우는, 컬렉션 보기 및 호출에 연결 합니다.는 `ReloadData` 항목을 표시 하는 방법:

```csharp
private void PopulateWithData()
{
    // Make datasource
    Datasource = new CollectionViewDataSource(EmployeeCollection);

    // Build list of employees
    Datasource.Data.Add(new PersonModel("Craig Dunn", "Documentation Manager", true));
    ...

    // Populate collection view
    EmployeeCollection.ReloadData();
}
```

`ViewDidLoad` 메서드 재정의 되 고 호출 합니다 `ConfigureCollectionView` 및 `PopulateWithData` 사용자에 게 최종 컬렉션 뷰를 표시 하는 방법.

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();

    // Initialize Collection View
    ConfigureCollectionView();
    PopulateWithData();
}
```

<a name="Summary"/>

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Mac 응용 프로그램에서 컬렉션 뷰를 사용 하 여 작업을 자세히 살펴보려면을 걸렸습니다. 첫째,이 키-값 코딩 (KVC) 및 키-값을 관찰 (KVO)를 사용 하 여 Objective-c를 C# 클래스를 공개 살펴보았습니다. 다음으로 KVO 호환 클래스를 사용 하는 방법에 알아보았습니다 하 고 데이터 Xcode의 Interface Builder에서 컬렉션 보기에 바인딩합니다. 마지막으로 C# 코드에서 컬렉션 뷰를 사용 하 여 상호 작용 하는 방법에 알아보았습니다.

## <a name="related-links"></a>관련 링크

- [MacCollectionNew (샘플)](https://developer.xamarin.com/samples/mac/MacCollectionNew/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md)
- [NSCollectionView](https://developer.apple.com/reference/appkit/nscollectionview)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
