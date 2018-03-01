---
title: "컬렉션 뷰 작업"
description: "이 문서에서는 디자인 하 고 컬렉션 뷰 Xamarin.tvOS 앱 내에서 작업을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5125C4C7-2DDF-4C19-A362-17BB2B079178
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: f0201e114f55e0610aceb68f98fae60a801afc68
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-collection-views"></a>컬렉션 뷰 작업

_이 문서에서는 디자인 하 고 컬렉션 뷰 Xamarin.tvOS 앱 내에서 작업을 설명 합니다._

임의의 레이아웃을 사용 하 여 표시 콘텐츠의 그룹에 대 한 컬렉션 뷰를 통해. 사용자 지정 레이아웃을 지원 하면서 쉽게 만들기 표 모양의 또는 선형 레이아웃에 대 한 허용 기본 제공 지원을 사용 하 여, 합니다.

[ ![](collection-views-images/collection01.png "샘플 컬렉션 보기")](collection-views-images/collection01.png)

컬렉션 뷰에서 사용자 상호 작용 및 컬렉션의 콘텐츠를 제공 하는 데이터 소스 및 대리자를 사용 하 여 항목의 컬렉션을 유지 합니다. 컬렉션 뷰에서 뷰 자체와 독립적인 레이아웃 하위 시스템을 기반으로 하기 때문 다른 레이아웃을 제공 하 쉽게 변경할 수 컬렉션 뷰 데이터에 즉시 표시 합니다.

<a name="About-Collection-Views" />

## <a name="about-collection-views"></a>컬렉션 뷰 정보

컬렉션 뷰 위에서 설명한 것 처럼 (`UICollectionView`) 항목의 정렬된 된 컬렉션을 관리 하 고 사용자 지정할 수 있는 레이아웃으로 해당 항목을 표시 합니다. 컬렉션 뷰 비슷한 방식으로 표 보기에서 작업 (`UITableView`)를 제외한 보다 더 단일 열에 있는 항목에는 레이아웃을 사용할 수 있습니다.

컬렉션 뷰 tvOS에를 사용할 때 앱이 데이터 원본을 사용 하 여 컬렉션에 연결 된 데이터를 제공 합니다 (`UICollectionViewDataSource`). 필요에 따라 데이터 보기 컬렉션을 구성 및 다른 그룹 (섹션)으로 제공 되 수 있습니다.

컬렉션 뷰 화면 셀을 사용 하 여 개별 항목을 제공 (`UICollectionViewCell`) 표현의 정보 (예: 이미지와 해당 제목을) 컬렉션에서 지정된 된 부분을 제공 하는 합니다.

필요에 따라 역할을 할 머리글 및 바닥글 섹션 및 셀 컬렉션 뷰 프레젠테이션에 보충 뷰를 추가할 수 있습니다. 컬렉션 뷰에서 레이아웃은 개별 셀 더불어 이러한 보기의 배치를 정의할 담당 합니다.

컬렉션 뷰에서 대리자를 사용 하는 사용자 상호 작용에 응답할 수 있습니다 (`UICollectionViewDelegate`). 이 대리자는 또한 셀이 강조 표시 된 경우 지정된 된 셀이 포커스를을 얻을 수 또는 선택 된 경우를 결정 합니다. 일부 경우 대리자는 개별 셀의 크기를 결정합니다.

<a name="Collection-View-Layouts" />

## <a name="collection-view-layouts"></a>컬렉션 뷰 레이아웃

컬렉션 뷰에서의 주요 기능은 표시 하는 데이터와 레이아웃의 구분입니다. 컬렉션 뷰 레이아웃 (`UICollectionViewLayout`)는 컬렉션 보기의 화면에 나타나는 프레젠테이션에서 조직과 셀 (및 모든 보조 뷰)의 위치를 제공 합니다.

개별 셀과 연결 된 데이터 원본의 컬렉션 뷰에 의해 만들어집니다 및 다음 정렬 되어 표시 하 여 지정 된 컬렉션 뷰 레이아웃 합니다.

컬렉션 뷰 레이아웃 컬렉션 뷰를 작성 하는 경우에 일반적으로 제공 됩니다. 그러나 언제 든 지 컬렉션 뷰 레이아웃을 변경할 수 있습니다 하 고 제공 된 새 레이아웃을 사용 하 여 데이터 컬렉션 보기의 화면 표시 업데이트 자동으로 됩니다.

컬렉션 뷰 레이아웃 (기본적으로, 애니메이션이 없는 수행 됩니다)는 두 개의 다른 레이아웃 간 전환 애니메이션 효과 주는 데 사용할 수 있는 몇 가지 방법을 제공 합니다. 또한 컬렉션 뷰 레이아웃 제스처 인식기 레이아웃에서 변경 하는 사용자 상호 작용을 추가로 애니메이션 효과를 사용할 수 있습니다.

<a name="Creating-Cells-and-Supplementary-Views" />

## <a name="creating-cells-and-supplementary-views"></a>셀 및 보충 뷰 만들기

컬렉션 보기의 데이터 소스는만 콘텐츠를 표시 하는 데 사용 되는 셀 뿐만 아니라 항목, 컬렉션의 백업 하 여 데이터를 제공 하는 일을 담당 합니다.

컬렉션 뷰 항목의 광범위 한 모음을 처리 하도록 디자인 된, 때문에 개별 셀을 큐에서 제거 하 고 메모리 제한으로 인해 사용 오버런 하지 못하도록 하려면 다시 사용할 수 있습니다. 이상의 뷰에 대 한 두 가지 방법이 있습니다.

- `DequeueReusableCell` -만들거나 (지정 된 대로 응용 프로그램의 스토리 보드에서) 지정 된 유형의 셀을 반환 합니다.
- `DequeueReusableSupplementaryView` -만들거나 보충 (지정 된 대로 응용 프로그램의 스토리 보드에서) 지정 된 형식의 뷰를 반환 합니다.

이러한 방법 중 하나를 호출 하기 전에 클래스를 등록 해야 스토리 보드 또는 `.xib` 컬렉션 뷰를 사용 하 여 셀의 보기를 만들려면 사용 되는 파일입니다. 예:

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    ...
}
```

여기서 `typeof(CityCollectionViewCell)` 보기를 지 원하는 클래스를 제공 하 고 `CityViewDatasource.CardCellId` 셀 (또는 뷰) 큐에서 제거 하는 경우 사용 되는 ID를 제공 합니다.

셀의 큐에서 제거 후가 나타내는 항목에 대 한 데이터를 구성 하 고 표시 하기 위한 컬렉션 보기로 돌아갑니다.

<a name="About-Collection-View-Controllers" />

## <a name="about-collection-view-controllers"></a>컬렉션 뷰 컨트롤러에 대 한

컬렉션 뷰 컨트롤러 (`UICollectionViewController`)는 특수 한 보기 컨트롤러 (`UIViewController`)는 다음 동작을 제공 하는:

- 여기서는 해당 스토리 보드에서 로드 컬렉션 뷰를 또는 `.xib` 파일 및 보기를 인스턴스화입니다. 코드에서 만들어진 새 구성 되지 않은 컬렉션 보기가 자동으로 만듭니다.
- 컨트롤러를 스토리 보드에서 해당 데이터 원본 및 대리자를 로드 하려고 컬렉션 보기 로드 되 면 또는 `.xib` 파일입니다. 사용할 수 없는 경우 설정 자체적 둘 다의 원본으로 합니다.
- 사용 하면 데이터 컬렉션 뷰를 표시 하는 첫 번째 채워집니다 되기 전에 로드 됩니다 및 다시 로드 하는 각 후속 디스플레이에 선택의 선택을 취소 합니다.

컬렉션 뷰 컨트롤러와 같은 컬렉션 뷰에서의 수명 주기를 관리 하는 데 사용할 수 있는 재정의 가능한 메서드를 제공 하는 또한 `AwakeFromNib` 및 `ViewWillDisplay`합니다.

<a name="Collection-Views-and-Storyboards" />

## <a name="collection-views-and-storyboards"></a>스토리 보드와 컬렉션 뷰

Xamarin.tvOS 응용 프로그램에서 컬렉션 뷰를 사용 하는 가장 쉬운 방법은 해당 스토리 보드에 추가 하는 것입니다. 간단한 예로 하겠습니다 이미지, 제목 및 선택 단추를 표시 하는 샘플 응용 프로그램을 만들려고 합니다. 사용자 선택 단추를 클릭 컬렉션 뷰 표시 됩니다 사용자를 새 이미지를 선택할 수 있도록 합니다. 이미지를 선택 하면 컬렉션 뷰 닫히고 새 이미지와 제목이 표시 됩니다.

다음을 수행 하겠습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

    
1. 새 시작 **단일 보기 tvOS 앱** Mac.에 대 한 Visual Studio에서
1. 에 **솔루션 탐색기**, 두 번 클릭은 `Main.storyboard` 파일 및 iOS 디자이너에서에서 엽니다.
1. 기존 보기를 이미지 보기, 레이블 및 단추를 추가 하 고 다음과 같이 표시 하도록 구성 합니다. 

    [ ![](collection-views-images/collection02.png "예제 레이아웃")](collection-views-images/collection02.png)
1. 할당 한 **이름** 이미지 보기와에서 레이블을에 **위젯 탭** 의 **속성 탐색기**합니다. 예: 

    [ ![](collection-views-images/collection03.png "설정 이름")](collection-views-images/collection03.png)
1. 스토리 보드도 컬렉션 뷰 컨트롤러를 다음으로 끌어 옵니다. 

    [ ![](collection-views-images/collection04.png "컬렉션 뷰 컨트롤러")](collection-views-images/collection04.png)
1. 컨트롤-단추에서 컬렉션 뷰 컨트롤러를 끌어서 선택 **푸시** 팝업에서: 

    [ ![](collection-views-images/collection05.png "팝업에서 푸시를 선택 합니다.")](collection-views-images/collection05.png)
1. 앱이 실행 되는 경우 사용자가 단추를 클릭할 때마다 표시 수 컬렉션 뷰 확인 합니다.
1. 컬렉션 뷰를 선택 하 고 다음 값 입력의 **레이아웃 탭** 의 **속성 탐색기**: 

    [ ![](collection-views-images/collection06.png "속성 탐색기")](collection-views-images/collection06.png)
1. 개별 셀과 셀 컬렉션 보기의 외부 가장자리 사이의 테두리의 크기를 제어합니다.
1. 컬렉션 뷰 컨트롤러를 선택 하 고 해당 클래스로 설정 `CityCollectionViewController` 에 **위젯을 탭**: 

    [ ![](collection-views-images/collection07.png "클래스 CityCollectionViewController로 설정")](collection-views-images/collection07.png)
1. 컬렉션 뷰를 선택 하 고 해당 클래스로 설정 `CityCollectionView` 에 **위젯을 탭**: 

    [ ![](collection-views-images/collection08.png "클래스 CityCollectionView로 설정")](collection-views-images/collection08.png)
1. 컬렉션 뷰 셀을 선택 하 고 해당 클래스로 설정 `CityCollectionViewCell` 에 **위젯을 탭**: 

    [ ![](collection-views-images/collection09.png "클래스 CityCollectionViewCell로 설정")](collection-views-images/collection09.png)
1. 에 **위젯을 탭** 되도록는 **레이아웃** 은 `Flow` 및 **스크롤 방향을** 은 `Vertical` 컬렉션 보기에 대 한: 

    [ ![](collection-views-images/collection10.png "위젯 탭")](collection-views-images/collection10.png)
1. 컬렉션 뷰 셀을 선택 하 고 설정의 **Identity** 를 `CityCell` 에 **위젯을 탭**: 

    [ ![](collection-views-images/collection11.png "CityCell으로 Id 설정")](collection-views-images/collection11.png)
1. 변경 내용을 저장합니다.
    

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

    
1. 새 시작 **단일 보기 tvOS 앱** Visual Studio에서.
1. 에 **솔루션 탐색기**, 두 번 클릭은 `Main.storyboard` 파일 및 iOS 디자이너에서에서 엽니다.
1. 기존 보기를 이미지 보기, 레이블 및 단추를 추가 하 고 다음과 같이 표시 하도록 구성 합니다. 

    [ ![](collection-views-images/collection02vs.png "레이아웃 구성")](collection-views-images/collection02vs.png)
1. 할당 한 **이름** 이미지 보기와에서 레이블을에 **위젯 탭** 의 **속성 탐색기**합니다. 예: 

    [ ![](collection-views-images/collection03vs.png "속성 탐색기")](collection-views-images/collection03vs.png)
1. 스토리 보드도 컬렉션 뷰 컨트롤러를 다음으로 끌어 옵니다. 

    [ ![](collection-views-images/collection04vs.png "컬렉션 뷰 컨트롤러")](collection-views-images/collection04vs.png)
1. 컨트롤-단추에서 컬렉션 뷰 컨트롤러를 끌어서 선택 **푸시** 팝업에서: 

    [ ![](collection-views-images/collection05vs.png "팝업에서 푸시를 선택 합니다.")](collection-views-images/collection05vs.png)
1. 앱이 실행 되는 경우 사용자가 단추를 클릭할 때마다 표시 수 컬렉션 뷰 확인 합니다.
1. 컬렉션 뷰를 선택 및는 **레이아웃 탭** 의 **속성 탐색기** 입력는 **너비** 으로 _361_ 및  **높이** 으로 _256_ 
1. 개별 셀과 셀 컬렉션 보기의 외부 가장자리 사이의 테두리의 크기를 제어합니다.
1. 컬렉션 뷰 컨트롤러를 선택 하 고 해당 클래스로 설정 `CityCollectionViewController` 에 **위젯을 탭**: 

    [ ![](collection-views-images/collection07vs.png "클래스 CityCollectionViewController로 설정")](collection-views-images/collection07vs.png)
1. 컬렉션 뷰를 선택 하 고 해당 클래스로 설정 `CityCollectionView` 에 **위젯을 탭**: 

    [ ![](collection-views-images/collection08vs.png "클래스 CityCollectionView로 설정")](collection-views-images/collection08vs.png)
1. 컬렉션 뷰 셀을 선택 하 고 해당 클래스로 설정 `CityCollectionViewCell` 에 **위젯을 탭**: 

    [ ![](collection-views-images/collection09vs.png "클래스 CityCollectionViewCell로 설정")](collection-views-images/collection09vs.png)
1. 에 **위젯을 탭** 되도록는 **레이아웃** 은 `Flow` 및 **스크롤 방향을** 은 `Vertical` 컬렉션 보기에 대 한: 

    [ ![](collection-views-images/collection10vs.png "사항 위젯을 탭")](collection-views-images/collection10vs.png)
1. 컬렉션 뷰 셀을 선택 하 고 설정의 **Identity** 를 `CityCell` 에 **위젯을 탭**: 

    [ ![](collection-views-images/collection11vs.png "CityCell으로 Id 설정")](collection-views-images/collection11vs.png)
1. 변경 내용을 저장합니다.
    

-----

선택한 경우 `Custom` 컬렉션 보기에 대 한 **레이아웃**, 사용자 지정 레이아웃을 지정할 수도 있습니다. Apple에서는 기본 제공 `UICollectionViewFlowLayout` 및 `UICollectionViewDelegateFlowLayout` 그리드 기반 레이아웃에 데이터를 쉽게 제공할 수 있는 (이러한 커넥터에서 사용 되는 `flow` 레이아웃 스타일) 합니다. 

스토리 보드를 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Hello, tvOS 퀵 스타트 가이드](~/ios/tvos/get-started/hello-tvos.md)합니다.

<a name="Providing-Data-for-the-Collection-View" />

## <a name="providing-data-for-the-collection-view"></a>컬렉션 보기에 대 한 데이터를 제공합니다.

이 스토리 보드에 추가 된이 컬렉션 뷰 (및 컬렉션 뷰-컨트롤러)를 만들었으므로 이제 해당 컬렉션에 대 한 데이터를 제공 해야 합니다. 

<a name="The-Data-Model" />

### <a name="the-data-model"></a>데이터 모델

첫째, 제목 및 도시를 선택 해야 허용 하기 위한 플래그를 표시 하려면 이미지의 파일 이름을 보유 하는 데이터에 대 한 모델을 만들 하려고 합니다.

만들기는 `CityInfo` 클래스와 다음과 비슷하게 표시:

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

이제 각 셀에 대 한 데이터가 표시 되는 방법을 정의 해야 합니다. 편집의 `CityCollectionViewCell.cs` (에서 만든 파일을 자동으로 스토리 보드 파일) 다음과 같이 만듭니다.

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

우리의 tvOS 앱에 대 한 것을 나타내려면 이미지와 선택적 제목을 합니다. 지정 된 도시를 선택할 수 없는 경우 다음 코드를 사용 하 여 이미지 뷰 흐리게 표시는 했습니다.

```csharp
CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
```

기본 제공 사용 하려는 이미지가 포함 된 셀을 사용자가 포커스 상태로, 시차 효과에 다음 속성을 설정할 수 있습니다.

```csharp
CityView.AdjustsImageWhenAncestorFocused = true;
```

탐색 및 포커스에 대 한 자세한 내용은 참조 하십시오 우리의 [탐색 및 포커스 작업](~/ios/tvos/app-fundamentals/navigation-focus.md) 및 [Siri 원격 인스턴스 및 Bluetooth 컨트롤러](~/ios/tvos/platform/remote-bluetooth.md) 설명서입니다.


<a name="The-Collection-View-Data-Provider" />

### <a name="the-collection-view-data-provider"></a>컬렉션 뷰 데이터 공급자

만든 데이터 모델 및 정의 하는 셀 레이아웃 사용 하 여이 컬렉션 보기에 대 한 데이터 소스를 만들어 보겠습니다. 데이터 소스 뿐 아니라 제공 하는 백업 데이터 뿐만 아니라 이상의 화면의 개별 셀을 표시 하려면 셀에 대 한 수도 있습니다.

만들기는 `CityViewDatasource` 클래스와 다음과 비슷하게 표시:

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

이 클래스를 자세히 살펴볼 수 있도록 합니다. 상속 우리는 먼저 `UICollectionViewDataSource` (iOS 디자이너에에서 할당 하는 것)이 표시 하는 셀 ID에 바로 가기를 제공 합니다.

```csharp
public static NSString CardCellId = new NSString ("CityCell");
```

다음 컬렉션 데이터에 대 한 저장소를 제공 하 고 데이터를 채우는 데는 클래스를 제공 합니다.

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

다음 재정의 `NumberOfSections` 메서드 및 컬렉션 보기 섹션 (항목 그룹)의 수에 반환 합니다. 이 경우 하나 뿐입니다.

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    return 1;
}
```

다음으로, 다음 코드를 사용 하 여 컬렉션의 항목 수가 반환 합니다.

```csharp
public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    return Cities.Count;
}
```

마지막으로, 컬렉션 뷰를 다음 코드로 요청 하는 경우 다시 사용할 수 있는 셀을 큐 했습니다.

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

컬렉션 뷰 셀 얻을 수 있는 후 우리의 `CityCollectionViewCell` 형식에서는으로 채우려면 지정된 된 항목입니다.

<a name="Responding-to-User-Events" />

## <a name="responding-to-user-events"></a>사용자 이벤트에 응답

사용자 업체 컬렉션에서 항목을 선택할 수 있게 되기를 원하는 때문에이 상호 작용을 처리 하는 컬렉션 뷰 대리자를 제공 해야 합니다. 고 우리의 호출 중인 뷰의 항목에 대해 사용자를 알 수 있도록이 선택한를 제공 해야 합니다.

<a name="The-App-Delegate" />

### <a name="the-app-delegate"></a>응용 프로그램 대리자

호출 보기로 다시 컬렉션 보기에서 현재 선택 된 항목을 연결 하는 방법이 필요 합니다. 사용할 사용자 지정 속성에 우리의 `AppDelegate`합니다. 편집 된 `AppDelegate.cs` 파일을 다음 코드를 추가 합니다.

```csharp
public CityInfo SelectedCity { get; set;} = new CityInfo("City02.jpg", "Turning Circle", true);
```

이 속성을 정의 하 고 처음 표시 되는 기본 도시를 설정 합니다. 나중 사용자의 선택 표시 하 고 select를 변경할 수 있도록 하려면이 속성을 사용 합니다 했습니다.

<a name="The-Collection-View-Delegate" />

### <a name="the-collection-view-delegate"></a>컬렉션 뷰 대리자

다음으로, 새 추가 `CityViewDelegate` 프로젝트에 클래스와 다음과 비슷하게 표시:


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

이 클래스는 좀 더 자세히 살펴보고를 보겠습니다. 상속 우리는 먼저 `UICollectionViewDelegateFlowLayout`합니다. 에서는이 클래스에서 상속 하는 이유 및 not는 `UICollectionViewDelegate` 기본 제공 사용 하는 것은 `UICollectionViewFlowLayout` 우리의 항목 및 사용자 지정 레이아웃 형식이 아닌에 게 표시할 합니다.

다음으로,이 코드를 사용 하 여 개별 항목의 크기를 반환 합니다.

```csharp
public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
{
    return new CGSize (361, 256);
}
```

그런 다음 특정된 셀 경우 다음 코드를 사용 하 여 포커스를 얻을 수 결정 합니다. 

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

백업 데이터의 지정된 된 부분에 있는지를 확인 해당 `CanSelect` 로 플래그 설정 `true` 해당 값을 반환 합니다. 탐색 및 포커스에 대 한 자세한 내용은 참조 하십시오 우리의 [탐색 및 포커스 작업](~/ios/tvos/app-fundamentals/navigation-focus.md) 및 [Siri 원격 인스턴스 및 Bluetooth 컨트롤러](~/ios/tvos/platform/remote-bluetooth.md) 설명서입니다.

마지막으로, 다음 코드와 항목을 선택 하면 사용자에 게 응답 합니다.

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    var controller = collectionView as CityCollectionView;
    App.SelectedCity = controller.Source.Cities [indexPath.Row];

    // Close Collection
    controller.ParentController.DismissViewController(true,null);
}
```

설정 여기는 `SelectedCity` 속성 우리의 `AppDelegate` 하는 사용자가 선택한 않으며이 닫습니다 컬렉션 뷰 컨트롤러 us를 호출 하는 뷰를 반환 하는 항목에 있습니다. 정의 하지는 `ParentController` 아직이 컬렉션 보기의 속성, 작업을 수행 합니다 다음입니다.

<a name="Configuring-the-Collection-View" />

## <a name="configuring-the-collection-view"></a>컬렉션 뷰를 구성합니다.

이제를 우리의 컬렉션 뷰를 편집 하 여이 데이터 원본 및 대리자를 할당 해야 합니다. 편집의 `CityCollectionView.cs` 파일 (자동 만들어집니다 우리의 스토리 보드에서) 다음과 같이 만듭니다.

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

에 액세스 하려면 바로 가기를 제공 먼저 우리의 `AppDelegate`: 

```csharp
public static AppDelegate App {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
```

다음으로, 컬렉션 보기의 데이터 원본 및 속성 (컬렉션을 사용자가 항목을 선택 하는 때 위의 우리의 대리자에 의해 사용) 컬렉션 뷰 컨트롤러에 액세스할 수 있는 바로 가기 제공:

```csharp
public CityViewDatasource Source {
    get { return DataSource as CityViewDatasource;}
}

public CityCollectionViewController ParentController { get; set;}
```

그런 다음, 다음 코드 컬렉션 뷰를 초기화 하 고 우리의 셀 클래스, 데이터 원본 및 대리자 할당을 사용 합니다.

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    DataSource = new CityViewDatasource (this);
    Delegate = new CityViewDelegate ();
}
```

마지막으로,만 볼 수 있도록 강조 표시 된 사용자가 이미지 아래의 제목 (포커스 내) 합니다. 다음 코드를 사용 하는 수행:

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

영 (0)으로 포커스를 잃을 이전 항목 transparence를 설정 하는 이유 및 다음 항목의 transparence 100%로 포커스를 얻을 합니다. 이러한 전환도 애니메이션 가져옵니다.


## <a name="configuring-the-collection-view-controller"></a>컬렉션 뷰 컨트롤러를 구성합니다.

이제 하이 컬렉션 뷰에 최종 구성을 수행 하 고 사용자가 항목을 선택 하는 후 컬렉션 뷰 닫으십시오 정의 속성을 설정 하는 컨트롤러를 허용 해야 합니다.

편집 된 `CityCollectionViewController.cs` 파일 (이 스토리 보드에서 자동으로 생성 됨) 다음과 비슷하게 표시 및:

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

## <a name="putting-it-all-together"></a>통합 

모든 았 마련해 부분의 채우고 제어 우리의 컬렉션 뷰를 확인 해야 최종 편집 모든 항목이 함께 가져올 수는 주 보기에 있습니다.

편집 된 `ViewController.cs` 파일 (이 스토리 보드에서 자동으로 생성 됨) 다음과 비슷하게 표시 및:

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

다음 코드에서 선택된 된 항목을 처음 표시는 `SelectedCity` 의 속성은 `AppDelegate` 만들고 컬렉션 보기에서 사용자가을 선택 했는지 표시:

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

## <a name="testing-the-app"></a>응용 프로그램 테스트

있는 모든 위치에서 빌드 및 응용 프로그램을 실행 하는 경우 기본 도시와 주 보기가 표시 됩니다.

[ ![](collection-views-images/run01.png "주 화면")](collection-views-images/run01.png)

사용자가 클릭 하는 경우는 **보기를 선택** 단추, 컬렉션 뷰에 표시 됩니다.

[ ![](collection-views-images/run02.png "컬렉션 보기")](collection-views-images/run02.png)

모든 도시 해당 `CanSelect` 속성이로 설정 `false` 나타납니다 흐리게 표시 되 고 사용자에 포커스를 설정할 수 없습니다. 사용자는 항목을 강조 하는 경우 (있도록 포커스 내) 제목이 표시 되 고 3d에서 시차 효과 미묘한 기울기 이미지를 사용할 수 있습니다.

사용자가 이미지 선택를 클릭 하면 컬렉션 뷰 닫히고 새 이미지와 함께 주 보기 다시 표시 됩니다.

[ ![](collection-views-images/run03.png "홈 화면에서 새 이미지")](collection-views-images/run03.png)

<a name="Creating-Custom-Layout-and-Reordering-Items" />

## <a name="creating-custom-layout-and-reordering-items"></a>사용자 지정 레이아웃 만들기 및 항목 다시 정렬

컬렉션 뷰를 사용 하 여의 주요 기능 중 하나를 사용자 지정 레이아웃을 만들 수입니다. IOS에서 tvOS 상속, 이후 사용자 지정 레이아웃을 만드는 과정은 같습니다. 참조 하십시오 우리의 [컬렉션 뷰 소개](~/ios/user-interface/controls/uicollectionview.md) 자세한 정보에 대 한 설명서입니다.

IOS에 대 한 컬렉션 뷰에 최근에 추가 9는 쉽게 컬렉션에 있는 항목의 순서를 바꿀 수 있도록 하는 기능 이었습니다. 다시, tvOS 9 iOS 9의 하위 집합 이므로이 작업은 수행을 동일한 방식으로 합니다. 참조 하십시오 우리의 [변경 내용 컬렉션 보기](~/ios/user-interface/controls/uicollectionview.md) 자세한 내용을 보려면 문서.


<a name="Summary" />

## <a name="summary"></a>요약

이 문서의 디자인 하 고 컬렉션 뷰 Xamarin.tvOS 앱 내에서 작업 검사가 수행 합니다. 첫째, 컬렉션 뷰를 구성 하는 요소를 모두 설명 합니다. 다음으로 디자인 하 고 스토리 보드를 사용 하 여 컬렉션 뷰를 구현 하는 방법을 설명 했습니다. 마지막으로 제공 됩니다 정보 링크를 사용자 지정 레이아웃 만들기 및 항목 다시 정렬 합니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 응용 프로그램 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
