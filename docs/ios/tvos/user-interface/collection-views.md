---
title: Xamarin에서 tvOS Collection 뷰 작업
description: 이 문서에서는 Xamarin을 사용 하 여 빌드된 tvOS 앱에서 컬렉션 뷰를 사용 하는 방법을 설명 합니다. 컬렉션 뷰 레이아웃, 셀 및 보충 뷰 만들기, 사용자 이벤트에 응답 등에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 5125C4C7-2DDF-4C19-A362-17BB2B079178
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 3246dcbf58a1b6dda6838b5eb81442fdbc429af5
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68652346"
---
# <a name="working-with-tvos-collection-views-in-xamarin"></a>Xamarin에서 tvOS Collection 뷰 작업

컬렉션 뷰를 사용 하면 임의의 레이아웃을 사용 하 여 콘텐츠 그룹을 표시할 수 있습니다. 기본 제공 지원을 사용 하면 사용자 지정 레이아웃을 지 원하는 동시에 쉽게 만들 수 있습니다.

[![](collection-views-images/collection01.png "샘플 컬렉션 뷰")](collection-views-images/collection01.png#lightbox)

컬렉션 뷰는 대리자와 데이터 소스를 모두 사용 하 여 항목 컬렉션을 유지 관리 하 여 사용자 조작 및 컬렉션의 내용을 제공 합니다. 컬렉션 뷰는 뷰 자체와 독립적인 레이아웃 하위 시스템을 기반으로 하기 때문에 다른 레이아웃을 제공 하면 컬렉션 뷰의 데이터 프레젠테이션이 즉석에서 쉽게 변경 될 수 있습니다.

<a name="About-Collection-Views" />

## <a name="about-collection-views"></a>컬렉션 뷰 정보

위에서 설명한 것 처럼 컬렉션 뷰 (`UICollectionView`)는 항목의 순서가 지정 된 컬렉션을 관리 하 고 해당 항목을 사용자 지정 가능한 레이아웃으로 표시 합니다. 컬렉션 뷰는 테이블 뷰 (`UITableView`)와 비슷한 방식으로 작동 합니다. 단, 레이아웃을 사용 하 여 단일 열 이상으로 항목을 표시할 수 있습니다.

TvOS에서 컬렉션 뷰를 사용 하는 경우 응용 프로그램은 데이터 원본 (`UICollectionViewDataSource`)을 사용 하 여 컬렉션에 연결 된 데이터를 제공 해야 합니다. 컬렉션 뷰 데이터를 선택적으로 구성 하 고 다른 그룹으로 표시할 수 있습니다 (섹션).

컬렉션 뷰는 이미지와 같은 컬렉션에서 지정 된 정보를`UICollectionViewCell`표시 하는 셀 ()을 사용 하 여 화면에 개별 항목을 표시 합니다.

필요에 따라 섹션 및 셀의 머리글 및 바닥글 역할을 하는 보조 보기를 컬렉션 보기의 프레젠테이션에 추가할 수 있습니다. 컬렉션 뷰의 레이아웃은 개별 셀과 함께 이러한 뷰의 배치를 정의 합니다.

컬렉션 뷰는 대리자 (`UICollectionViewDelegate`)를 사용 하 여 사용자 상호 작용에 응답할 수 있습니다. 또한이 대리자는 셀이 강조 표시 되었거나 선택 된 경우 지정 된 셀이 포커스를 받을 수 있는지 여부를 확인 합니다. 대리자가 개별 셀의 크기를 결정 하는 경우도 있습니다.

<a name="Collection-View-Layouts" />

## <a name="collection-view-layouts"></a>컬렉션 뷰 레이아웃

컬렉션 뷰의 핵심 기능은 제공 되는 데이터와 해당 레이아웃 간의 분리입니다. 컬렉션 뷰 레이아웃 (`UICollectionViewLayout`)은 컬렉션 뷰의 화면 표시에 있는 셀 (및 모든 보조 뷰)의 위치와 조직을 제공 합니다.

개별 셀은 연결 된 데이터 원본에서 컬렉션 뷰로 생성 된 다음 지정 된 컬렉션 뷰 레이아웃에 의해 정렬 되 고 표시 됩니다.

컬렉션 뷰 레이아웃은 컬렉션 뷰를 만들 때 일반적으로 제공 됩니다. 그러나 언제 든 지 컬렉션 뷰 레이아웃을 변경할 수 있으며 컬렉션 뷰 데이터의 화면 프레젠테이션은 제공 된 새 레이아웃을 사용 하 여 자동으로 업데이트 됩니다.

컬렉션 뷰 레이아웃은 두 개의 서로 다른 레이아웃 간의 전환에 애니메이션 효과를 주는 데 사용할 수 있는 여러 메서드를 제공 합니다. 기본적으로 애니메이션은 수행 되지 않습니다. 또한 컬렉션 뷰 레이아웃을 사용 하면 제스처 인식기를 사용 하 여 레이아웃 변경을 수행 하는 사용자 상호 작용에 더 많은 애니메이션 효과를 적용할 수 있습니다.

<a name="Creating-Cells-and-Supplementary-Views" />

## <a name="creating-cells-and-supplementary-views"></a>셀 및 보조 뷰 만들기

컬렉션 뷰의 데이터 소스는 컬렉션의 항목을 지 원하는 데이터 뿐만 아니라 콘텐츠를 표시 하는 데 사용 되는 셀도 제공 합니다.

컬렉션 뷰는 많은 항목 컬렉션을 처리 하도록 디자인 되었으므로 개별 셀을 큐에서 제거 하 고 다시 사용 하 여 overrunning 메모리 제한을 유지할 수 있습니다. 큐를 제거 하는 두 가지 방법이 있습니다.

- `DequeueReusableCell`-지정 된 형식의 셀을 만들거나 반환 합니다 (앱의 스토리 보드에 지정 된 대로).
- `DequeueReusableSupplementaryView`-지정 된 형식의 보충 뷰 (앱의 스토리 보드에 지정 된)를 만들거나 반환 합니다.

이러한 메서드 중 하나를 호출 하기 전에 컬렉션 뷰를 사용 하 여 셀 `.xib` 의 뷰를 만드는 데 사용 되는 클래스, Storyboard 또는 파일을 등록 해야 합니다. 예:

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    ...
}
```

여기서 `typeof(CityCollectionViewCell)` 는 뷰를 지 원하는 클래스를 제공 `CityViewDatasource.CardCellId` 하 고는 셀 (또는 뷰)이 큐에서 제거 될 때 사용 되는 ID를 제공 합니다.

셀이 큐에서 제거 된 후에는 해당 셀이 나타내는 항목에 대 한 데이터로 구성 하 고 표시를 위해 컬렉션 뷰로 돌아옵니다.

<a name="About-Collection-View-Controllers" />

## <a name="about-collection-view-controllers"></a>컬렉션 뷰 컨트롤러 정보

컬렉션 뷰 컨트롤러 (`UICollectionViewController`)는 다음과 같은 동작을 제공 하는 특수 한 뷰 컨트롤러 (`UIViewController`)입니다.

- 스토리 보드 또는 `.xib` 파일에서 컬렉션 뷰를 로드 하 고 뷰를 인스턴스화하는 일을 담당 합니다. 코드에서 만든 경우 구성 되지 않은 새 컬렉션 뷰를 자동으로 만듭니다.
- 컬렉션 뷰가 로드 되 면 컨트롤러는 해당 데이터 소스를 로드 하 고 스토리 보드 또는 `.xib` 파일에서 대리자를 시도 합니다. 사용할 수 없는 경우이를 모두의 소스로 설정 합니다.
- 처음 표시 될 때 컬렉션 뷰를 채우기 전에 데이터를 로드 하 고 각 후속 디스플레이에서 선택을 취소 하 고 지웁니다.

또한 컬렉션 뷰 컨트롤러는 `AwakeFromNib` 및 `ViewWillDisplay`와 같은 컬렉션 뷰의 수명 주기를 관리 하는 데 사용할 수 있는 재정의 가능한 메서드를 제공 합니다.

<a name="Collection-Views-and-Storyboards" />

## <a name="collection-views-and-storyboards"></a>컬렉션 뷰 및 스토리 보드

TvOS 앱에서 컬렉션 뷰를 사용 하는 가장 쉬운 방법은 스토리 보드에 하나를 추가 하는 것입니다. 간단한 예로 이미지, 제목 및 선택 단추를 표시 하는 샘플 앱을 만들어 보겠습니다. 사용자가 선택 단추를 클릭 하면 사용자가 새 이미지를 선택할 수 있는 컬렉션 뷰가 표시 됩니다. 이미지를 선택 하면 컬렉션 뷰가 닫히고 새 이미지와 제목이 표시 됩니다.

다음 작업을 수행해 보겠습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

    
1. Mac용 Visual Studio에서 새 **단일 뷰 TvOS 앱** 을 시작 합니다.
1. **솔루션 탐색기**에서 `Main.storyboard` 파일을 두 번 클릭 하 여 iOS 디자이너에서 엽니다.
1. 기존 뷰에 이미지 뷰, 레이블 및 단추를 추가 하 고 다음과 같이 구성 합니다. 

    [![](collection-views-images/collection02.png "샘플 레이아웃")](collection-views-images/collection02.png#lightbox)
1. **속성 탐색기**의 **위젯 탭** 에서 이미지 뷰 및 레이블에 **이름을** 지정 합니다. 예: 

    [![](collection-views-images/collection03.png "이름 설정")](collection-views-images/collection03.png#lightbox)
1. 다음으로, 컬렉션 뷰 컨트롤러를 Storyboard로 끌어 옵니다. 

    [![](collection-views-images/collection04.png "컬렉션 뷰 컨트롤러")](collection-views-images/collection04.png#lightbox)
1. 단추에서 컬렉션 뷰 컨트롤러로 컨트롤을 끌고 팝업에서 **푸시** 를 선택 합니다. 

    [![](collection-views-images/collection05.png "팝업에서 푸시를 선택 합니다.")](collection-views-images/collection05.png#lightbox)
1. 앱이 실행 되 면 사용자가 단추를 클릭할 때마다 컬렉션 보기가 표시 됩니다.
1. 컬렉션 뷰를 선택 하 고 **속성 탐색기**의 **레이아웃 탭** 에서 다음 값을 입력 합니다. 

    [![](collection-views-images/collection06.png "속성 탐색기")](collection-views-images/collection06.png#lightbox)
1. 이는 개별 셀의 크기와 컬렉션 뷰의 셀과 외부 가장자리 사이의 테두리를 제어 합니다.
1. 컬렉션 뷰 컨트롤러를 선택 하 고 **위젯 탭**에서 `CityCollectionViewController` 해당 클래스를로 설정 합니다. 

    [![](collection-views-images/collection07.png "클래스를 CityCollectionViewController로 설정 합니다.")](collection-views-images/collection07.png#lightbox)
1. 컬렉션 뷰를 선택 하 고 **위젯 탭**에서 `CityCollectionView` 해당 클래스를로 설정 합니다. 

    [![](collection-views-images/collection08.png "클래스를 CityCollectionView로 설정 합니다.")](collection-views-images/collection08.png#lightbox)
1. 컬렉션 뷰 셀을 선택 하 고 **위젯 탭**에서 `CityCollectionViewCell` 해당 클래스를로 설정 합니다. 

    [![](collection-views-images/collection09.png "클래스를 CityCollectionViewCell로 설정 합니다.")](collection-views-images/collection09.png#lightbox)
1. **위젯 탭** 에서 **레이아웃** 은이 `Flow` 고 **스크롤 방향은** 컬렉션 보기에 대 한 `Vertical` 것입니다. 

    [![](collection-views-images/collection10.png "위젯 탭")](collection-views-images/collection10.png#lightbox)
1. 컬렉션 뷰 셀을 선택 하 고 **위젯 탭**에서 `CityCell` **id** 를로 설정 합니다. 

    [![](collection-views-images/collection11.png "Id를 CityCell로 설정 합니다.")](collection-views-images/collection11.png#lightbox)
1. 변경 내용을 저장합니다.
    

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    
1. Visual Studio에서 새 **단일 뷰 TvOS 앱** 을 시작 합니다.
1. **솔루션 탐색기**에서 `Main.storyboard` 파일을 두 번 클릭 하 여 iOS 디자이너에서 엽니다.
1. 기존 뷰에 이미지 뷰, 레이블 및 단추를 추가 하 고 다음과 같이 구성 합니다. 

    [![](collection-views-images/collection02vs.png "레이아웃 구성")](collection-views-images/collection02vs.png#lightbox)
1. **속성 탐색기**의 **위젯 탭** 에서 이미지 뷰 및 레이블에 **이름을** 지정 합니다. 예: 

    [![](collection-views-images/collection03vs.png "속성 탐색기")](collection-views-images/collection03vs.png#lightbox)
1. 다음으로, 컬렉션 뷰 컨트롤러를 Storyboard로 끌어 옵니다. 

    [![](collection-views-images/collection04vs.png "컬렉션 뷰 컨트롤러")](collection-views-images/collection04vs.png#lightbox)
1. 단추에서 컬렉션 뷰 컨트롤러로 컨트롤을 끌고 팝업에서 **푸시** 를 선택 합니다. 

    [![](collection-views-images/collection05vs.png "팝업에서 푸시를 선택 합니다.")](collection-views-images/collection05vs.png#lightbox)
1. 앱이 실행 되 면 사용자가 단추를 클릭할 때마다 컬렉션 보기가 표시 됩니다.
1. 컬렉션 뷰를 선택 하 고 **속성 탐색기** 의 **레이아웃 탭** 에서 **너비** 를 _361_ 으로, **높이** 를 _256_ 으로 입력 합니다. 
1. 이는 개별 셀의 크기와 컬렉션 뷰의 셀과 외부 가장자리 사이의 테두리를 제어 합니다.
1. 컬렉션 뷰 컨트롤러를 선택 하 고 **위젯 탭**에서 `CityCollectionViewController` 해당 클래스를로 설정 합니다. 

    [![](collection-views-images/collection07vs.png "클래스를 CityCollectionViewController로 설정 합니다.")](collection-views-images/collection07vs.png#lightbox)
1. 컬렉션 뷰를 선택 하 고 **위젯 탭**에서 `CityCollectionView` 해당 클래스를로 설정 합니다. 

    [![](collection-views-images/collection08vs.png "클래스를 CityCollectionView로 설정 합니다.")](collection-views-images/collection08vs.png#lightbox)
1. 컬렉션 뷰 셀을 선택 하 고 **위젯 탭**에서 `CityCollectionViewCell` 해당 클래스를로 설정 합니다. 

    [![](collection-views-images/collection09vs.png "클래스를 CityCollectionViewCell로 설정 합니다.")](collection-views-images/collection09vs.png#lightbox)
1. **위젯 탭** 에서 **레이아웃** 은이 `Flow` 고 **스크롤 방향은** 컬렉션 보기에 대 한 `Vertical` 것입니다. 

    [![](collection-views-images/collection10vs.png "화면이 어떻게 보일지 위젯 탭")](collection-views-images/collection10vs.png#lightbox)
1. 컬렉션 뷰 셀을 선택 하 고 **위젯 탭**에서 `CityCell` **id** 를로 설정 합니다. 

    [![](collection-views-images/collection11vs.png "Id를 CityCell로 설정 합니다.")](collection-views-images/collection11vs.png#lightbox)
1. 변경 내용을 저장합니다.
    

-----

컬렉션 뷰의 **레이아웃**을 `Custom` 선택한 경우 사용자 지정 레이아웃을 지정할 수 있습니다. Apple은 기본 `UICollectionViewFlowLayout` 제공 되는를 `UICollectionViewDelegateFlowLayout` 제공 하며, 표 기반 레이아웃에 데이터를 쉽게 제공할 수 있습니다 ( `flow` 레이아웃 스타일에서 사용 됨). 

스토리 보드 사용에 대 한 자세한 내용은 [Hello, tvOS 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md)를 참조 하세요.

<a name="Providing-Data-for-the-Collection-View" />

## <a name="providing-data-for-the-collection-view"></a>컬렉션 뷰에 대 한 데이터 제공

이제 컬렉션 뷰 (및 컬렉션 뷰 컨트롤러)를 스토리 보드에 추가 했으므로 컬렉션에 대 한 데이터를 제공 해야 합니다. 

<a name="The-Data-Model" />

### <a name="the-data-model"></a>데이터 모델

먼저 표시할 이미지의 파일 이름, 제목 및 도시를 선택할 수 있는 플래그를 포함 하는 데이터에 대 한 모델을 만듭니다.

클래스를 `CityInfo` 만들고 다음과 같이 만듭니다.

```csharp
using System;

namespace tvCollection
{
    public class CityInfo
    {
        #region Computed Properties
        public string ImageFilename { get; set; }
        public string Title { get; set; }
        public bool CanSelect{ get; set; }
        #endregion

        #region Constructors
        public CityInfo (string filename, string title, bool canSelect)
        {
            // Initialize
            this.ImageFilename = filename;
            this.Title = title;
            this.CanSelect = canSelect;
        }
        #endregion
    }
}
```

### <a name="the-collection-view-cell"></a>컬렉션 뷰 셀

이제 각 셀에 대 한 데이터가 표시 되는 방법을 정의 해야 합니다. 스토리 보드 `CityCollectionViewCell.cs` 파일에서 자동으로 생성 된 파일을 편집 하 여 다음과 같이 만듭니다.

```csharp
using System;
using Foundation;
using UIKit;
using CoreGraphics;

namespace tvCollection
{
    public partial class CityCollectionViewCell : UICollectionViewCell
    {
        #region Private Variables
        private CityInfo _city;
        #endregion

        #region Computed Properties
        public UIImageView CityView { get ; set; }
        public UILabel CityTitle { get; set; }

        public CityInfo City {
            get { return _city; }
            set {
                _city = value;
                CityView.Image = UIImage.FromFile (City.ImageFilename);
                CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
                CityTitle.Text = City.Title;
            }
        }
        #endregion

        #region Constructors
        public CityCollectionViewCell (IntPtr handle) : base (handle)
        {
            // Initialize
            CityView = new UIImageView(new CGRect(22, 19, 320, 171));
            CityView.AdjustsImageWhenAncestorFocused = true;
            AddSubview (CityView);

            CityTitle = new UILabel (new CGRect (22, 209, 320, 21)) {
                TextAlignment = UITextAlignment.Center,
                TextColor = UIColor.White,
                Alpha = 0.0f
            };
            AddSubview (CityTitle);
        }
        #endregion
    

    }
}
```

TvOS 앱의 경우 이미지 및 선택적 제목을 표시 합니다. 지정 된 도시를 선택할 수 없는 경우 다음 코드를 사용 하 여 이미지 뷰를 흐리게 표시 합니다.

```csharp
CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
```

사용자가 이미지를 포함 하는 셀에 포커스를 가져오면 다음 속성을 설정할 때 기본 제공 시차 효과를 사용 하려고 합니다.

```csharp
CityView.AdjustsImageWhenAncestorFocused = true;
```

탐색 및 포커스에 대 한 자세한 내용은 [탐색 및 포커스](~/ios/tvos/app-fundamentals/navigation-focus.md) 및 [Siri 원격 및 Bluetooth 컨트롤러](~/ios/tvos/platform/remote-bluetooth.md) 사용 설명서를 참조 하세요.


<a name="The-Collection-View-Data-Provider" />

### <a name="the-collection-view-data-provider"></a>컬렉션 뷰 Data Provider

데이터 모델을 만들고 셀 레이아웃을 정의 하면 컬렉션 뷰에 대 한 데이터 소스를 만들어 보겠습니다. 데이터 원본은 지원 데이터를 제공 하는 것 뿐만 아니라 개별 셀을 화면에 표시 하는 셀의 큐를 제거 합니다.

클래스를 `CityViewDatasource` 만들고 다음과 같이 만듭니다.

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;
using ObjCRuntime;

namespace tvCollection
{
    public class CityViewDatasource : UICollectionViewDataSource
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Static Constants
        public static NSString CardCellId = new NSString ("CityCell");
        #endregion

        #region Computed Properties
        public List<CityInfo> Cities { get; set; } = new List<CityInfo>();
        public CityCollectionView ViewController { get; set; }
        #endregion

        #region Constructors
        public CityViewDatasource (CityCollectionView controller)
        {
            // Initialize
            this.ViewController = controller;
            PopulateCities ();
        }
        #endregion

        #region Public Methods
        public void PopulateCities() {

            // Clear existing cities
            Cities.Clear();

            // Add new cities
            Cities.Add(new CityInfo("City01.jpg", "Houses by Water", false));
            Cities.Add(new CityInfo("City02.jpg", "Turning Circle", true));
            Cities.Add(new CityInfo("City03.jpg", "Skyline at Night", true));
            Cities.Add(new CityInfo("City04.jpg", "Golden Gate Bridge", true));
            Cities.Add(new CityInfo("City05.jpg", "Roads by Night", true));
            Cities.Add(new CityInfo("City06.jpg", "Church Domes", true));
            Cities.Add(new CityInfo("City07.jpg", "Mountain Lights", true));
            Cities.Add(new CityInfo("City08.jpg", "City Scene", false));
            Cities.Add(new CityInfo("City09.jpg", "House in Winter", true));
            Cities.Add(new CityInfo("City10.jpg", "By the Lake", true));
            Cities.Add(new CityInfo("City11.jpg", "At the Dome", true));
            Cities.Add(new CityInfo("City12.jpg", "Cityscape", true));
            Cities.Add(new CityInfo("City13.jpg", "Model City", true));
            Cities.Add(new CityInfo("City14.jpg", "Taxi, Taxi!", true));
            Cities.Add(new CityInfo("City15.jpg", "On the Sidewalk", true));
            Cities.Add(new CityInfo("City16.jpg", "Midnight Walk", true));
            Cities.Add(new CityInfo("City17.jpg", "Lunchtime Cafe", true));
            Cities.Add(new CityInfo("City18.jpg", "Coffee Shop", true));
            Cities.Add(new CityInfo("City19.jpg", "Rustic Tavern", true));
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView)
        {
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section)
        {
            return Cities.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            var cityCell = (CityCollectionViewCell)collectionView.DequeueReusableCell (CardCellId, indexPath);
            var city = Cities [indexPath.Row];

            // Initialize city
            cityCell.City = city;

            return cityCell;
        }
        #endregion
    }
}
```

이 클래스에 대해 자세히 살펴보겠습니다. 먼저에서 `UICollectionViewDataSource` 를 상속 하 고, iOS 디자이너에서 할당 한 셀 ID에 대 한 바로 가기를 제공 합니다.

```csharp
public static NSString CardCellId = new NSString ("CityCell");
```

다음으로 컬렉션 데이터에 대 한 저장소를 제공 하 고 데이터를 채울 클래스를 제공 합니다.

```csharp
public List<CityInfo> Cities { get; set; } = new List<CityInfo>();
...

public void PopulateCities() {

    // Clear existing cities
    Cities.Clear();

    // Add new cities
    Cities.Add(new CityInfo("City01.jpg", "Houses by Water", false));
    Cities.Add(new CityInfo("City02.jpg", "Turning Circle", true));
    ...
}
```

그런 다음 메서드를 `NumberOfSections` 재정의 하 고 컬렉션 뷰에 있는 섹션 (항목 그룹)의 수를 반환 합니다. 이 경우에는 다음 하나만 있습니다.

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    return 1;
}
```

다음으로 다음 코드를 사용 하 여 컬렉션의 항목 수를 반환 합니다.

```csharp
public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    return Cities.Count;
}
```

마지막으로, 다음 코드를 사용 하 여 컬렉션 뷰 요청 시 재사용 가능한 셀을 큐에서 제거 합니다.

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    var cityCell = (CityCollectionViewCell)collectionView.DequeueReusableCell (CardCellId, indexPath);
    var city = Cities [indexPath.Row];

    // Initialize city
    cityCell.City = city;

    return cityCell;
}
```

`CityCollectionViewCell` 형식의 컬렉션 뷰 셀을 가져온 후에는 지정 된 항목으로 채웁니다.

<a name="Responding-to-User-Events" />

## <a name="responding-to-user-events"></a>사용자 이벤트에 응답

사용자가 컬렉션에서 항목을 선택할 수 있도록 하려면이 상호 작용을 처리 하는 컬렉션 뷰 대리자를 제공 해야 합니다. 그리고 사용자가 선택한 항목을 호출 하는 뷰에 알려 주는 방법을 제공 해야 합니다.

<a name="The-App-Delegate" />

### <a name="the-app-delegate"></a>앱 대리자

현재 선택한 항목을 컬렉션 보기에서 호출 하는 보기에 다시 연결 하는 방법이 필요 합니다. 에서 사용자 지정 속성을 사용 `AppDelegate`합니다. 파일을 `AppDelegate.cs` 편집 하 고 다음 코드를 추가 합니다.

```csharp
public CityInfo SelectedCity { get; set;} = new CityInfo("City02.jpg", "Turning Circle", true);
```

이 속성은 속성을 정의 하 고 처음 표시 될 기본 도시를 설정 합니다. 나중에이 속성을 사용 하 여 사용자의 선택 항목을 표시 하 고 선택 항목을 변경할 수 있습니다.

<a name="The-Collection-View-Delegate" />

### <a name="the-collection-view-delegate"></a>컬렉션 뷰 대리자

다음으로 프로젝트에 새 `CityViewDelegate` 클래스를 추가 하 고 다음과 같이 만듭니다.


```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;

namespace tvCollection
{
    public class CityViewDelegate : UICollectionViewDelegateFlowLayout
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        public CityViewDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
        {
            return new CGSize (361, 256);
        }

        public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
        {
            if (indexPath == null) {
                return false;
            } else {
                var controller = collectionView as CityCollectionView;
                return controller.Source.Cities[indexPath.Row].CanSelect;
            }
        }

        public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
        {
            var controller = collectionView as CityCollectionView;
            App.SelectedCity = controller.Source.Cities [indexPath.Row];

            // Close Collection
            controller.ParentController.DismissViewController(true,null);
        }
        #endregion
    }
}
```

이 클래스를 좀 더 자세히 살펴보겠습니다. 먼저에서 `UICollectionViewDelegateFlowLayout`를 상속 합니다. 이 아닌 `UICollectionViewDelegate` 이 클래스에서 상속 하는 이유는 사용자 지정 레이아웃 형식이 아니라 기본 제공 `UICollectionViewFlowLayout` 을 사용 하 여 항목을 제공 하는 것입니다.

다음으로,이 코드를 사용 하 여 개별 항목의 크기를 반환 합니다.

```csharp
public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
{
    return new CGSize (361, 256);
}
```

그런 다음, 다음 코드를 사용 하 여 지정 된 셀이 포커스를 받을 수 있는지 여부를 결정 합니다. 

```csharp
public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
{
    if (indexPath == null) {
        return false;
    } else {
        var controller = collectionView as CityCollectionView;
        return controller.Source.Cities[indexPath.Row].CanSelect;
    }
}
```

지정 된 지원 데이터 부분에 `CanSelect` 플래그가로 `true` 설정 되어 있는지 확인 하 고 해당 값을 반환 합니다. 탐색 및 포커스에 대 한 자세한 내용은 [탐색 및 포커스](~/ios/tvos/app-fundamentals/navigation-focus.md) 및 [Siri 원격 및 Bluetooth 컨트롤러](~/ios/tvos/platform/remote-bluetooth.md) 사용 설명서를 참조 하세요.

마지막으로 다음 코드를 사용 하 여 항목을 선택 하는 사용자에 게 응답 합니다.

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    var controller = collectionView as CityCollectionView;
    App.SelectedCity = controller.Source.Cities [indexPath.Row];

    // Close Collection
    controller.ParentController.DismissViewController(true,null);
}
```

여기서는 `SelectedCity` `AppDelegate` 의 속성을 사용자가 선택한 항목으로 설정 하 고 컬렉션 뷰 컨트롤러를 닫은 후 us 라는 뷰로 돌아갑니다. 컬렉션 뷰의 `ParentController` 속성을 아직 정의 하지 않았으므로 다음 작업을 수행 합니다.

<a name="Configuring-the-Collection-View" />

## <a name="configuring-the-collection-view"></a>컬렉션 뷰 구성

이제 컬렉션 뷰를 편집 하 고 데이터 원본 및 대리자를 할당 해야 합니다. `CityCollectionView.cs` 파일 (스토리 보드에서 자동으로 만들어짐)을 편집 하 여 다음과 같이 만듭니다.

```csharp
using System;
using Foundation;
using UIKit;

namespace tvCollection
{
    public partial class CityCollectionView : UICollectionView
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public CityViewDatasource Source {
            get { return DataSource as CityViewDatasource;}
        }

        public CityCollectionViewController ParentController { get; set;}
        #endregion

        #region Constructors
        public CityCollectionView (IntPtr handle) : base (handle)
        {
            // Initialize
            RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
            DataSource = new CityViewDatasource (this);
            Delegate = new CityViewDelegate ();
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections ()
        {
            return 1;
        }

        public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
        {
            var previousItem = context.PreviouslyFocusedView as CityCollectionViewCell;
            if (previousItem != null) {
                Animate (0.2, () => {
                    previousItem.CityTitle.Alpha = 0.0f;
                });
            }

            var nextItem = context.NextFocusedView as CityCollectionViewCell;
            if (nextItem != null) {
                Animate (0.2, () => {
                    nextItem.CityTitle.Alpha = 1.0f;
                });
            }
        }
        #endregion
    }
}
```

먼저, `AppDelegate`다음에 액세스할 수 있는 바로 가기를 제공 합니다. 

```csharp
public static AppDelegate App {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
```

다음으로 컬렉션 보기의 데이터 소스에 대 한 바로 가기를 제공 하 고, 컬렉션 뷰 컨트롤러에 액세스 하기 위한 속성을 제공 합니다 .이는 위의 대리자가 사용자를 선택할 때 컬렉션을 닫는 데 사용 됩니다.

```csharp
public CityViewDatasource Source {
    get { return DataSource as CityViewDatasource;}
}

public CityCollectionViewController ParentController { get; set;}
```

그런 다음, 다음 코드를 사용 하 여 컬렉션 뷰를 초기화 하 고 셀 클래스, 데이터 소스 및 대리자를 할당 합니다.

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    DataSource = new CityViewDatasource (this);
    Delegate = new CityViewDelegate ();
}
```

마지막으로, 사용자가 강조 표시 (포커스 내) 할 때만 이미지 아래의 제목을 표시 하려고 합니다. 다음 코드를 사용 하 여이 작업을 수행 합니다.

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    var previousItem = context.PreviouslyFocusedView as CityCollectionViewCell;
    if (previousItem != null) {
        Animate (0.2, () => {
            previousItem.CityTitle.Alpha = 0.0f;
        });
    }

    var nextItem = context.NextFocusedView as CityCollectionViewCell;
    if (nextItem != null) {
        Animate (0.2, () => {
            nextItem.CityTitle.Alpha = 1.0f;
        });
    }
}
```

포커스를 잃은 이전 항목의 transparence을 0으로 설정 하면 다음 항목의 transparence이 100%로 포커스를 얻습니다. 이러한 전환에도 애니메이션 효과가 적용 됩니다.


## <a name="configuring-the-collection-view-controller"></a>컬렉션 뷰 컨트롤러 구성

이제 컬렉션 뷰에서 최종 구성을 수행 하 고 컨트롤러에서 사용자가 선택 하 고 나면 컬렉션 뷰를 닫을 수 있도록 정의한 속성을 설정할 수 있도록 해야 합니다.

`CityCollectionViewController.cs` 파일 (스토리 보드에서 자동으로 만들어짐)을 편집 하 여 다음과 같이 만듭니다.

```csharp
// This file has been autogenerated from a class added in the UI designer.

using System;

using Foundation;
using UIKit;

namespace tvCollection
{
    public partial class CityCollectionViewController : UICollectionViewController
    {
        #region Computed Properties
        public CityCollectionView Collection {
            get { return CollectionView as CityCollectionView; }
        }
        #endregion

        #region Constructors
        public CityCollectionViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Save link to controller
            Collection.ParentController = this;
        }
        #endregion
    }
}

```

## <a name="putting-it-all-together"></a>모두 함께 배치 

컬렉션 뷰를 채우고 제어 하기 위해 모든 부분을 통합 했으므로 이제 기본 보기를 최종 편집 하 여 모든 항목을 함께 가져와야 합니다.

`ViewController.cs` 파일 (스토리 보드에서 자동으로 만들어짐)을 편집 하 여 다음과 같이 만듭니다.

```csharp
using System;
using Foundation;
using UIKit;
using tvCollection;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update image with the currently selected one
            CityView.Image = UIImage.FromFile(App.SelectedCity.ImageFilename);
            BackgroundView.Image = CityView.Image;
            CityTitle.Text = App.SelectedCity.Title;
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion
    }
}
```

다음 코드는 처음에 `SelectedCity` `AppDelegate` 의 속성에서 선택한 항목을 표시 하 고 사용자가 컬렉션 뷰에서 항목을 선택 했을 때 해당 항목을 다시 표시 합니다.

```csharp
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update image with the currently selected one
    CityView.Image = UIImage.FromFile(App.SelectedCity.ImageFilename);
    BackgroundView.Image = CityView.Image;
    CityTitle.Text = App.SelectedCity.Title;
}
```

<a name="Testing-the-app" />

## <a name="testing-the-app"></a>앱 테스트

모든 것이 준비 되 면 앱을 빌드하고 실행 하는 경우 기본 도시와 함께 주 보기가 표시 됩니다.

[![](collection-views-images/run01.png "주 화면")](collection-views-images/run01.png#lightbox)

사용자가 **보기 선택** 단추를 클릭 하면 컬렉션 뷰가 표시 됩니다.

[![](collection-views-images/run02.png "컬렉션 뷰")](collection-views-images/run02.png#lightbox)

`CanSelect` 속성이 로`false` 설정 된 모든 도시는 흐리게 표시 되며 사용자가 포커스를 설정할 수 없습니다. 사용자가 항목을 강조 표시 하면 (포커스 내에서) 제목이 표시 되 고 시차 효과를 사용 하 여 이미지를 3D로 미묘한 수 있습니다.

사용자가 선택 이미지를 클릭 하면 컬렉션 뷰가 닫히고 주 뷰가 새 이미지와 함께 다시 표시 됩니다.

[![](collection-views-images/run03.png "홈 화면의 새 이미지")](collection-views-images/run03.png#lightbox)

<a name="Creating-Custom-Layout-and-Reordering-Items" />

## <a name="creating-custom-layout-and-reordering-items"></a>사용자 지정 레이아웃 만들기 및 항목 순서 변경

컬렉션 뷰를 사용 하는 주요 기능 중 하나는 사용자 지정 레이아웃을 만드는 기능입니다. TvOS는 iOS에서 상속 하므로 사용자 지정 레이아웃을 만드는 프로세스는 동일 합니다. 자세한 내용은 [컬렉션 뷰 소개 설명서를](~/ios/user-interface/controls/uicollectionview.md) 참조 하세요.

최근 iOS 9에 대 한 컬렉션 보기에 추가 된 컬렉션의 항목 다시 정렬을 쉽게 허용할 수 있는 기능입니다. TvOS 9는 iOS 9의 하위 집합 이므로 동일한 방식으로 수행 됩니다. 자세한 내용은 [컬렉션 뷰 변경 내용](~/ios/user-interface/controls/uicollectionview.md) 문서를 참조 하세요.


<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 tvOS 앱 내에서 컬렉션 보기를 디자인 하 고 작업 하는 방법에 대해 설명 했습니다. 먼저 컬렉션 뷰를 구성 하는 모든 요소에 대해 설명 했습니다. 다음으로 Storyboard를 사용 하 여 컬렉션 뷰를 디자인 하 고 구현 하는 방법을 살펴보았습니다. 마지막으로,에는 사용자 지정 레이아웃을 만들고 항목을 다시 정렬 하는 정보에 대 한 링크가 제공 됩니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
