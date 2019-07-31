---
title: Xamarin.ios의 컬렉션 뷰
description: 이 문서에서는 Xamarin.ios 앱에서 컬렉션 뷰를 사용 하는 방법을 설명 합니다. Xcode 및 Interface Builder에서 컬렉션 뷰를 만들고 유지 관리 하 고 프로그래밍 방식으로 작업 하는 방법을 다룹니다.
ms.prod: xamarin
ms.assetid: 6EE32256-5948-4AE4-8133-6D0B3F4173E8
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 05/24/2017
ms.openlocfilehash: 432e875969164b6481671d769d488c5f34458fe0
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68657269"
---
# <a name="collection-views-in-xamarinmac"></a>Xamarin.ios의 컬렉션 뷰

_이 문서에서는 Xamarin.ios 앱에서 컬렉션 뷰를 사용 하는 방법을 설명 합니다. Xcode 및 Interface Builder에서 컬렉션 뷰를 만들고 유지 관리 하 고 프로그래밍 방식으로 작업 하는 방법을 다룹니다._

Xamarin.ios 앱에서 C# 및 .net을 사용할 때 개발자는 *목표-C* 및 *Xcode* 에서 작업 하는 개발자가 동일한 appkit 컬렉션 뷰 컨트롤에 액세스할 수 있습니다. Xamarin.ios는 Xcode와 직접 통합 되므로 개발자는 Xcode의 _Interface Builder_ 을 사용 하 여 컬렉션 뷰를 만들고 유지 관리 합니다.

A를 사용 하 여 구성 된 하위 뷰 표를 `NSCollectionViewLayout`표시 합니다. `NSCollectionView` 표의 각 하위 뷰는 `NSCollectionViewItem` `.xib` 파일에서 뷰 내용의 로드를 관리 하는로 표시 됩니다.

[![예제 앱 실행](collection-view-images/intro01.png)](collection-view-images/intro01.png#lightbox)

이 문서에서는 Xamarin.ios 앱에서 컬렉션 보기를 사용 하는 기본 사항을 설명 합니다. 사용 되는 주요 개념 및 기술에 대해 설명 하는 대로 [Hello, Mac](~/mac/get-started/hello-mac.md) 문서를 먼저 소개 하 고 특히 [Xcode 및 Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 및 [콘센트 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션을 소개 하는 것이 좋습니다. 이 문서 전체.

[Xamarin.ios 내부](~/mac/internals/how-it-works.md) 문서의 [클래스/메서드를 대상으로 노출 C# -C](~/mac/internals/how-it-works.md) 섹션을 살펴볼 수 있습니다. C# 클래스를 연결 하는 데 사용 되는 `Register` 및 `Export` 명령을 설명 합니다. 목표-C 개체 및 UI 요소입니다.

<a name="About_Collection_Views"/>

## <a name="about-collection-views"></a>컬렉션 뷰 정보

컬렉션 뷰 (`NSCollectionView`)의 주요 목표는 컬렉션 뷰 레이아웃 (`NSCollectionViewLayout`)을 사용 하 여 개체 그룹을 구성 된 방식으로 시각적으로 정렬 하는 것입니다. 각 개별`NSCollectionViewItem`개체 ()는 더 큰 컬렉션에서 자체 뷰를 가져옵니다. 컬렉션 뷰는 데이터 바인딩 및 키-값 코딩 기술을 통해 작동 하며,이 문서를 계속 하기 전에 [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md) 문서를 읽어야 합니다.

컬렉션 뷰에는 개요 나 테이블 뷰와 같은 표준 기본 제공 컬렉션 뷰 항목이 없으므로 개발자는 이미지 필드, 텍스트 필드, 레이블 등의 다른 AppKit 컨트롤을 사용 하 여 _프로토타입 보기_ 를 디자인 하 고 구현할 책임이 있습니다. 등. 이 프로토타입 보기는 컬렉션 뷰에서 관리 되는 각 항목을 표시 하 고 작업 하는 데 사용 되며 `.xib` 파일에 저장 됩니다.

개발자는 컬렉션 뷰 항목의 모양과 느낌을 담당 하므로 컬렉션 뷰에는 표에서 선택한 항목을 강조 표시 하는 기능이 기본적으로 제공 되지 않습니다. 이 기능을 구현 하는 방법에 대해서는이 문서에서 설명 합니다.

<a name="Defining_your_Data_Model"/>

## <a name="defining-the-data-model"></a>데이터 모델 정의

Interface Builder에서 컬렉션 뷰 데이터를 바인딩하기 전에 KVO (키-값 코딩) (KVC)/Key-Value 관찰 () 규격 클래스를 Xamarin.ios 앱에서 정의 하 여 바인딩에 대 한 _데이터 모델_ 역할을 수행 해야 합니다. 데이터 모델은 컬렉션에 표시 되는 모든 데이터를 제공 하 고 사용자가 응용 프로그램을 실행 하는 동안 UI에서 수행 하는 데이터에 대 한 모든 수정 사항을 받습니다.

직원 그룹을 관리 하는 앱의 예를 들어 다음 클래스를 사용 하 여 데이터 모델을 정의할 수 있습니다.

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

`PersonModel` 데이터 모델은이 문서의 나머지 부분에서 사용 됩니다.

<a name="Working_with_a_Collection_View"/>

## <a name="working-with-a-collection-view"></a>컬렉션 뷰 작업

컬렉션 뷰를 사용 하는 데이터 바인딩은 컬렉션에 대 한 데이터를 제공 하는 `NSCollectionViewDataSource` 데 사용 되는 테이블 뷰로 바인딩과 매우 유사 합니다. 컬렉션 뷰에는 미리 설정 된 표시 형식이 없으므로 사용자 상호 작용 피드백을 제공 하 고 사용자 선택을 추적 하려면 더 많은 작업이 필요 합니다.

<a name="Creating-the-Cell-Prototype"/>

### <a name="creating-the-cell-prototype"></a>셀 프로토타입 만들기

컬렉션 뷰에는 기본 셀 프로토타입이 포함 되지 않기 때문에 개발자는 xamarin.ios 앱에 하나 `.xib` 이상의 파일을 추가 하 여 개별 셀의 레이아웃과 콘텐츠를 정의 해야 합니다.

다음을 수행합니다.

1. **솔루션 탐색기**에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다.
2. `EmployeeItem` **Mac** > **보기 컨트롤러**를 선택 하 고 이름을 지정 하 고 (예:이 예의 경우) **새로** 만들기 단추를 클릭 하 여 다음을 만듭니다. 

    ![새 뷰 컨트롤러 추가](collection-view-images/proto01.png)

    그러면 `EmployeeItem.cs` `EmployeeItemController.cs` 및 파일이`EmployeeItemController.xib` 프로젝트의 솔루션에 추가 됩니다.
3. Xcode의 Interface Builder에서 `EmployeeItemController.xib` 편집할 파일을 두 번 클릭 하 여 엽니다.
4. 및 두 개의 `NSImageView` 컨트롤`NSLabel` 을 뷰에 추가 하 고 다음과 같이 레이아웃 합니다. `NSBox`

    ![셀 프로토타입의 레이아웃 디자인](collection-view-images/proto02.png)
5. **길잡이 편집기** 를 열고의 **콘센트** 를 만들어 셀의 `NSBox` 선택 상태를 나타내는 데 사용할 수 있도록 합니다.

    ![콘센트에서 NSBox 노출](collection-view-images/proto03.png)
6. **표준 편집기** 로 돌아가서 이미지 뷰를 선택 합니다.
7. **바인딩 검사기**에서**파일의 소유자** **에** > 바인딩을 선택 하 고 다음의 `self.Person.Icon`모델 키 경로를 입력 합니다.

    ![아이콘 바인딩](collection-view-images/proto04.png)
8. 첫 번째 레이블을 선택 하 고 **바인딩 검사기**에서**파일의 소유자** **에** > 바인딩을 선택 하 고 다음의 `self.Person.Name` **모델 키 경로** 를 입력 합니다.

    ![이름 바인딩](collection-view-images/proto05.png)
9. 두 번째 레이블을 선택 하 고 **바인딩 검사기**에서**파일의 소유자** **에** > 바인딩을 선택 하 고 다음의 `self.Person.Occupation` **모델 키 경로** 를 입력 합니다.

    ![직업 바인딩](collection-view-images/proto06.png)
10. 변경 내용을 `.xib` 파일에 저장 하 고 Visual Studio로 돌아가서 변경 내용을 동기화 합니다.

`EmployeeItemController.cs` 파일을 편집 하 여 다음과 같이 만듭니다.

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

이 코드를 자세히 살펴보면 클래스는에서 `NSCollectionViewItem` 상속 하므로 컬렉션 뷰 셀에 대 한 프로토타입으로 동작할 수 있습니다. 속성 `Person` 은 Xcode의 이미지 뷰 및 레이블에 데이터 바인딩하는 데 사용 된 클래스를 노출 합니다. 위에서 `PersonModel` 만든의 인스턴스입니다.

속성은 셀의 선택 상태를 `NSBox` 표시 하 `FillColor` 는 데 사용 되는 컨트롤의 바로 가기입니다. `BackgroundColor` 다음 코드는 `Selected` `NSCollectionViewItem`의 속성을 재정의 하 여이 선택 상태를 설정 하거나 해제 합니다.

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

### <a name="creating-the-collection-view-data-source"></a>컬렉션 뷰 데이터 원본 만들기

컬렉션 뷰 데이터 원본 (`NSCollectionViewDataSource`)은 컬렉션 뷰에 대 한 모든 데이터를 제공 하 고 컬렉션의 각 항목에 대 한 컬렉션 뷰 셀 ( `.xib` 프로토타입 사용)을 만들고 채웁니다.

프로젝트에 새 클래스를 추가 하 고이 `CollectionViewDataSource` 를 호출 하 여 다음과 같이 만듭니다.

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

이 코드를 자세히 살펴보면 클래스는에서 `NSCollectionViewDataSource` 상속 되 고 `Data` 속성을 통해 `PersonModel` 인스턴스 목록을 노출 합니다.

이 컬렉션에는 하나의 섹션만 있으므로 코드는 메서드를 `GetNumberOfSections` 재정의 하 고 항상를 반환 `1`합니다. 또한 메서드는 `GetNumberofItems` `Data` 속성 목록에 있는 항목 수를 반환 합니다.

새 셀이 필요할 때마다 메서드가호출되고다음과같이표시됩니다.`GetItem`

```csharp
public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
{
    var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
    item.Person = Data[(int)indexPath.Item];

    return item;
}
```

컬렉션 뷰의 `EmployeeItemController`메서드는 의 재사용 가능한 인스턴스를 만들거나 반환 하기 위해 호출 되 고 해당 `Person` 속성은 요청 된 셀에 표시 되는 항목으로 설정 됩니다. `MakeItem` 

는 `EmployeeItemController` 다음 코드를 사용 하 여 미리 컬렉션 뷰 컨트롤러에 등록 해야 합니다.

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
``` 

호출에 사용`EmployeeCell`되는 **식별자** ( )는 컬렉션 뷰에 등록 된 뷰 컨트롤러의 이름과 일치 해야 합니다. `MakeItem` 이 단계는 아래에서 자세히 설명 합니다.

<a name="Handling-Item-Selection"/>

### <a name="handling-item-selection"></a>항목 선택 처리

컬렉션에서 항목의 선택 및 deselection를 처리 하려면 `NSCollectionViewDelegate` 이 필요 합니다. 이 예제에서는 기본 제공 `NSCollectionViewFlowLayout` 레이아웃 형식을 `NSCollectionViewDelegateFlowLayout` 사용 하므로이 대리자의 특정 버전이 필요 합니다.

프로젝트에 새 클래스를 추가 하 고이 `CollectionViewDelegate` 를 호출 하 고 다음과 같이 표시 합니다.

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

및 `ItemsSelected` `PersonSelected` 메서드는 사용자가 항목을 선택 하거나 선택 취소 하는 경우 컬렉션 뷰를 처리 하는 뷰 컨트롤러의 속성을 설정 하거나 해제 하는 데 사용 됩니다. `ItemsDeselected` 이 내용은 아래에 자세히 나와 있습니다.

<a name="Creating-the-Collection-View-in-Interface-Builder"/>

### <a name="creating-the-collection-view-in-interface-builder"></a>Interface Builder에서 컬렉션 뷰 만들기

필요한 모든 지원 조각이 준비 되 면 주 storyboard를 편집 하 고 여기에 컬렉션 뷰를 추가할 수 있습니다.

다음을 수행합니다.

1. `Main.Storyboard` **솔루션 탐색기** 파일을 두 번 클릭 하 여 Xcode의 Interface Builder에서 편집할 수 있도록 엽니다.
2. 컬렉션 뷰를 기본 뷰로 끌고 크기를 조정 하 여 보기를 채웁니다.

    ![레이아웃에 컬렉션 뷰 추가](collection-view-images/collection01.png)
3. 컬렉션 뷰가 선택 된 상태에서 제약 조건 편집기를 사용 하 여 크기를 조정할 때 뷰에 고정 합니다.

    ![제약 조건 추가](collection-view-images/collection02.png)
4. **Design Surface** 에서 컬렉션 뷰가 선택 되어 있는지 확인 하 고 (이를 포함 하는 **테두리가 있는 스크롤 뷰나** **클립 보기가** 아닌) **도우미 편집기** 로 전환 하 고 컬렉션 뷰에 대 한 **콘센트** 를 만듭니다.

    ![제약 조건 추가](collection-view-images/collection03.png)
5. 변경 내용을 저장 하 고 동기화를 위해 Visual Studio로 돌아갑니다.

<a name="Bringing-it-all-Together"/>

## <a name="bringing-it-all-together"></a>모두 함께 가져오기

이제 지원 되는 모든 부분이 데이터 모델`PersonModel`() `NSCollectionViewDataSource` 역할을 하는 클래스와 함께 배치 되 고,가 데이터를 제공 `NSCollectionViewDelegateFlowLayout` 하기 위해 추가 되었으며, 항목이 항목 선택을 처리 하기 위해 생성 되 고 `NSCollectionView` 가 주 스토리 보드에 추가 되었습니다. 그리고 유출로 노출 됩니다 (`EmployeeCollection`).

마지막 단계는 컬렉션 뷰를 포함 하는 뷰 컨트롤러를 편집 하 고 모든 조각을 함께 가져와 컬렉션을 채우고 항목 선택을 처리 하는 것입니다.

`ViewController.cs` 파일을 편집 하 여 다음과 같이 만듭니다.

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

이 코드를 자세히 `Datasource` 살펴보면 속성은 컬렉션 뷰에 대 한 데이터를 제공 `CollectionViewDataSource` 하는의 인스턴스를 포함 하도록 정의 됩니다. 속성은 컬렉션 뷰에서 현재 선택 된 `PersonModel` 항목을 나타내는를 포함 하도록 정의 됩니다. `PersonSelected` 또한이 속성은 선택 `SelectionChanged` 이 변경 될 때 이벤트를 발생 시킵니다.

`ConfigureCollectionView` 클래스는 다음 줄을 사용 하 여 컬렉션 뷰를 사용 하 여 셀 프로토타입의 역할을 하는 뷰 컨트롤러를 등록 하는 데 사용 됩니다.

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
```

프로토타입을 등록 하 는 데`EmployeeCell`사용 되는 식별자 ()는 위에 정의 된 `CollectionViewDataSource` 의 `GetItem` 메서드에서 호출 된 식별자와 일치 합니다.

```csharp
var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
...
```

또한 뷰 컨트롤러의 형식은 프로토타입을 **정확 하 게**정의 하는 `.xib` 파일의 이름과 일치 **해야** 합니다. 이 예제의 `EmployeeItemController` 경우 및 `EmployeeItemController.xib`입니다.

컬렉션 뷰에서 항목의 실제 레이아웃은 컬렉션 뷰 레이아웃 클래스에 의해 제어 되며 런타임에 새 인스턴스를 `CollectionViewLayout` 속성에 할당 하 여 동적으로 변경할 수 있습니다. 이 속성을 변경 하면 변경 내용에 애니메이션을 적용 하지 않고 컬렉션 뷰 모양이 업데이트 됩니다.

Apple은 및 `NSCollectionViewGridLayout`의 일반적인 용도를 처리 하는 컬렉션 뷰를 사용 하 여 두 가지 기본 `NSCollectionViewFlowLayout` 제공 레이아웃 유형을 제공 합니다. 원 안에 항목을 배치 하는 것과 같이 개발자에 게 사용자 지정 형식이 필요한 경우 사용자 지정 인스턴스 `NSCollectionViewLayout` 를 만들고 필요한 메서드를 재정의 하 여 원하는 효과를 달성할 수 있습니다.

이 예제에서는 기본 흐름 레이아웃을 사용 하 여 `NSCollectionViewFlowLayout` 클래스의 인스턴스를 만들고 다음과 같이 구성 합니다.

```csharp
var flowLayout = new NSCollectionViewFlowLayout()
{
    ItemSize = new CGSize(150, 150),
    SectionInset = new NSEdgeInsets(10, 10, 10, 20),
    MinimumInteritemSpacing = 10,
    MinimumLineSpacing = 10
};
```

속성 `ItemSize` 은 컬렉션에 있는 각 개별 셀의 크기를 정의 합니다. 속성 `SectionInset` 은 셀이 배치 될 컬렉션의 가장자리에서 음각을 정의 합니다. `MinimumInteritemSpacing`항목 사이의 최소 간격을 정의 하 `MinimumLineSpacing` 고 컬렉션의 줄 사이의 최소 간격을 정의 합니다.

레이아웃은 컬렉션 뷰에 할당 되 고 인스턴스 `CollectionViewDelegate` 는 핸들 항목 선택 항목에 연결 됩니다.

```csharp
// Setup collection view
EmployeeCollection.CollectionViewLayout = flowLayout;
EmployeeCollection.Delegate = new CollectionViewDelegate(this);
```

메서드 `PopulateWithData` 는 의새`ReloadData` 인스턴스를 만들고 ,데이터로채운다음컬렉션뷰에연결하고,메서드를호출하여항목을표시합니다.`CollectionViewDataSource`

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

메서드는 재정의 되 고 `ConfigureCollectionView` 및 `PopulateWithData` 메서드를 호출 하 여 최종 컬렉션 뷰를 사용자에 게 표시 합니다. `ViewDidLoad`

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

이 문서에서는 Xamarin.ios 응용 프로그램에서 컬렉션 뷰를 사용 하는 방법을 자세히 살펴봅니다. 먼저 KVC (키-값 C# 코딩) 및 키-값 관찰 (KVO)을 사용 하 여 클래스를 목표 C로 노출 하는 방법을 살펴보았습니다. 다음으로 KVO 규격 클래스를 사용 하 고 데이터를 Xcode의 Interface Builder 컬렉션 뷰에 바인딩하는 방법을 살펴보았습니다. 마지막으로, 코드에서 C# 컬렉션 뷰와 상호 작용 하는 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [MacCollectionNew (샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/maccollectionnew)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md)
- [NSCollectionView](https://developer.apple.com/reference/appkit/nscollectionview)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
