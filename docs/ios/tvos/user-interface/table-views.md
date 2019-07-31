---
title: Xamarin에서 tvOS 테이블 뷰 작업
description: 이 문서에서는 tvOS 앱 내에서 테이블 뷰 및 테이블 뷰 컨트롤러를 디자인 하 고 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: D8F80FA9-6400-4DB7-AFC9-A28A54AD04E8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 3df0d8f686ec521a55948a9eb4632d77e5c3691f
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68652331"
---
# <a name="working-with-tvos-table-views-in-xamarin"></a>Xamarin에서 tvOS 테이블 뷰 작업

_이 문서에서는 tvOS 앱 내에서 테이블 뷰 및 테이블 뷰 컨트롤러를 디자인 하 고 사용 하는 방법을 설명 합니다._

TvOS에서는 테이블 뷰가 선택적으로 그룹 또는 섹션으로 구성 될 수 있는 스크롤 행의 단일 열로 표시 됩니다. 테이블 뷰를 사용 하 여 많은 양의 데이터를 사용자에 게 효율적으로 표시 해야 하는 경우에는 명확 하 게 이해 해야 합니다.

테이블 뷰는 일반적으로 [분할 보기](~/ios/tvos/user-interface/split-views.md) 의 한쪽에 탐색으로 표시 되 고, 선택한 항목의 세부 정보는 반대쪽에 표시 됩니다.

[![](table-views-images/intro01.png "예제 테이블 뷰")](table-views-images/intro01.png#lightbox)

<a name="About-Table-Views" />

## <a name="about-table-views"></a>테이블 뷰 정보

은 `UITableView` 스크롤할 수 있는 행의 단일 열을 선택적으로 그룹 또는 섹션으로 구성할 수 있는 정보 계층 목록으로 표시 합니다. 

[![](table-views-images/table01.png "선택한 항목")](table-views-images/table01.png#lightbox)

Apple에는 테이블 작업에 대 한 다음과 같은 제안이 있습니다.

- **너비를 알고 있어야** 합니다. 테이블 너비의 올바른 균형을 유지 하려고 합니다. 테이블이 너무 넓은 경우 거리에서 검색 하기 어려울 수 있으며 사용 가능한 콘텐츠 영역에서 벗어날 수 있습니다. 테이블이 너무 좁아서 정보를 자르거나 래핑할 수 있습니다 .이 경우 사용자가 대화방에서 읽기 어려울 수 있습니다.
- **테이블 콘텐츠를 빠르게 표시** -데이터를 대량으로 표시 하 고, 콘텐츠를 지연 로드 하 고, 사용자에 게 테이블을 표시 하는 즉시 정보를 표시 하기 시작 합니다. 테이블을 로드 하는 데 시간이 오래 걸리는 경우에는 사용자가 앱에서 관심이 손실 되거나 잠긴 것으로 간주 될 수 있습니다.
- **긴 콘텐츠 로드를 사용자** 에 게 알립니다. 긴 테이블 로드 시간이 피할 수 없는 경우 앱이 잠기지 않았음을 알 수 있도록 [진행률 표시줄이 나 작업 표시기](~/ios/tvos/user-interface/progress-indicators.md) 를 표시 합니다.

<a name="Table-Cell-Types" />

## <a name="table-view-cell-types"></a>테이블 뷰 셀 형식

는 `UITableViewCell` 테이블 뷰에서 데이터의 개별 행을 나타내는 데 사용 됩니다. Apple은 몇 가지 기본 테이블 셀 형식을 정의 합니다.

- **기본값** -이 형식은 셀 왼쪽에 옵션 이미지를 표시 하 고 오른쪽에 왼쪽 맞춤 제목을 표시 합니다. 
- **부제** -이 형식은 첫 번째 줄에 왼쪽 맞춤 제목을 표시 하 고 다음 줄에 더 작은 왼쪽에 맞춰진 부제목을 표시 합니다.
- **값 1** -이 형식은 같은 줄에서 더 밝은 색, 오른쪽 맞춤 부제목이 있는 왼쪽 맞춤 제목을 표시 합니다.
- **값 2** -이 형식은 같은 줄에서 더 밝은 색으로 왼쪽에 맞춰진 부제목을 사용 하 여 오른쪽 맞춤 제목을 표시 합니다.

모든 기본 테이블 뷰 셀 유형은 노출 표시기 또는 확인 표시와 같은 그래픽 요소도 지원 합니다. 

또한 **사용자 지정** 테이블 뷰 셀 형식을 정의 하 고 사용자가 인터페이스 디자이너 또는 코드를 통해 만든 _프로토타입 셀_을 표시할 수 있습니다.

Apple에는 테이블 뷰 셀을 사용 하기 위한 다음과 같은 제안이 있습니다.

- **텍스트 클리핑 방지** -잘림 방지 되도록 개별 텍스트 줄을 짧게 유지 합니다. 잘린 단어나 구는 대화방에서 구문을 분석 하는 데 사용 하기 어렵습니다.
- 포커스가 있는 **행 상태를 고려** 합니다. 포커스가 있으면 모든 상태에서 셀의 모양을 테스트 해야 합니다. 포커스가 있는 상태에서 이미지나 텍스트가 잘리거나 잘못 표시 될 수 있습니다.
- **편집 가능한 테이블 사용** -테이블 행 이동 또는 삭제는 IOS 보다 tvOS에서 더 많은 시간이 걸립니다. 이 기능이 tvOS 앱을 추가 하거나 방해 여부를 신중 하 게 결정 해야 합니다.
- **적절 한 경우 사용자 지정 셀 유형을 만듭니다** . 기본 제공 테이블 뷰 셀 유형은 여러 상황에서 매우 유용 하지만, 비표준 정보에 대 한 사용자 지정 셀 유형을 만들어 더 많은 제어를 제공 하 고 정보를에 더 효과적으로 제공 하는 것이 좋습니다. 정의.

<a name="Working-With-Table-Views" />

## <a name="working-with-table-views"></a>테이블 뷰 작업

TvOS 앱에서 테이블 뷰로 작업 하는 가장 쉬운 방법은 인터페이스 디자이너에서 모양을 만들고 수정 하는 것입니다.

시작하려면 다음을 수행합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)
    
1. Mac용 Visual Studio에서 새 tvOS app 프로젝트를 시작 하 고 **tvOS** > **app** > **단일 뷰 앱** 을 선택 하 고 **다음** 단추를 클릭 합니다. 

    [![](table-views-images/table02.png "단일 뷰 앱 선택")](table-views-images/table02.png#lightbox)
1. 앱의 **이름을** 입력 하 고 **다음**을 클릭 합니다. 

    [![](table-views-images/table03.png "앱의 이름 입력")](table-views-images/table03.png#lightbox)
1. **프로젝트 이름** 및 **솔루션 이름을** 조정 하거나 기본값을 적용 하 고 **만들기** 단추를 클릭 하 여 새 솔루션을 만듭니다. 

    [![](table-views-images/table04.png "프로젝트 이름 및 솔루션 이름")](table-views-images/table04.png#lightbox)
1. **Solution Pad**에서 `Main.storyboard` 파일을 두 번 클릭 하 여 iOS 디자이너에서 엽니다. 

    [![](table-views-images/table05.png "주 storyboard 파일")](table-views-images/table05.png#lightbox)
1. **기본 뷰 컨트롤러**를 선택 하 고 삭제 합니다. 

    [![](table-views-images/table06.png "기본 뷰 컨트롤러를 선택 하 고 삭제 합니다.")](table-views-images/table06.png#lightbox)
1. **도구 상자** 에서 **분할 뷰 컨트롤러** 를 선택 하 고 Design Surface 끌어 옵니다.
1. 기본적으로 왼쪽에는 **탐색 뷰 컨트롤러** 와 뷰 컨트롤러, 오른쪽에는 **뷰 컨트롤러** 를 사용 하 여 [분할 뷰가](~/ios/tvos/user-interface/split-views.md) 표시 됩니다. 다음은 tvOS에서 테이블 뷰에 대 한 Apple의 제안 된 사용법입니다. 

    [![](table-views-images/table08.png "분할 뷰 추가")](table-views-images/table08.png#lightbox)
1. C# 나중에 코드에서 액세스할 수 있도록 테이블 뷰의 모든 부분을 선택 하 고 **속성 탐색기** 의 **위젯** 탭에서 사용자 지정 **클래스 이름을** 할당 해야 합니다. 예를 들어 **테이블 뷰 컨트롤러**는 다음과 같습니다. 

    [![](table-views-images/table09.png "클래스 이름 할당")](table-views-images/table09.png#lightbox)
1. **테이블 뷰 컨트롤러**, **테이블 뷰** 및 **프로토타입 셀**에 대 한 사용자 지정 클래스를 만들어야 합니다. Mac용 Visual Studio는 사용자 지정 클래스를 만들 때 프로젝트 트리에 추가 합니다. 

    [![](table-views-images/table10.png "프로젝트 트리의 사용자 지정 클래스")](table-views-images/table10.png#lightbox)
1. 그런 다음 Design Surface에서 테이블 뷰를 선택 하 고 필요에 따라 속성을 조정 합니다. 예: **프로토타입 셀** 수 및 **스타일** (일반 또는 그룹화 됨): 

    [![](table-views-images/table11.png "위젯 탭")](table-views-images/table11.png#lightbox)
1. 각 **프로토타입 셀**에 대해 해당 셀을 선택 하 고 **속성 탐색기**의 **위젯** 탭에서 고유한 **식별자** 를 할당 합니다. 이 단계는 나중에 테이블을 채울 때이 식별자가 필요 하므로 _매우 중요_ 합니다. 예를 `AttrCell`들면 다음과 같습니다. 

    [![](table-views-images/table12.png "위젯 탭")](table-views-images/table12.png#lightbox)
1. 또한 **스타일** 드롭다운을 통해 [기본 테이블 뷰 셀 형식](#table-view-cell-types) 으로 셀을 표시 하거나 **사용자 지정** 으로 설정 하 고 Design Surface를 사용 하 여 **도구 상자**에서 다른 UI 위젯에서 끌어 셀을 레이아웃 하도록 선택할 수 있습니다. 

    [![](table-views-images/table13.png "셀 레이아웃")](table-views-images/table13.png#lightbox)
1. **속성 탐색기** 의 C# **위젯** 탭에서 프로토타입 셀 디자인의 각 UI 요소에 고유한 **이름을** 할당 하 여 나중에 코드에서 액세스할 수 있도록 합니다. 

    [![](table-views-images/table14.png "이름 할당")](table-views-images/table14.png#lightbox)
1. 테이블 뷰의 모든 프로토타입 셀에 대해 위의 단계를 반복 합니다.
1. 그런 다음 UI 디자인의 나머지 부분에 사용자 지정 클래스를 할당 하 고 자세히 보기를 레이아웃 한 다음에서 C# 액세스할 수 있도록 세부 정보 보기의 각 Ui 요소에 고유한 이름을 할당 합니다. 예를 들면 다음과 같습니다. 

    [![](table-views-images/table15.png "UI 레이아웃")](table-views-images/table15.png#lightbox)
1. 스토리 보드에 변경 내용을 저장 합니다.
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. Visual Studio에서 새 tvOS app 프로젝트를 시작 하 고 **tvOS** > **단일 뷰 앱** 을 선택 하 고 앱의 이름을 입력 합니다. 확인 단추 **를** 클릭 하 여 새 솔루션을 만듭니다. 

    [![](table-views-images/table02-vs.png "단일 뷰 앱 선택")](table-views-images/table02-vs.png#lightbox)
1. **솔루션 탐색기**에서 `Main.storyboard` 파일을 두 번 클릭 하 여 iOS 디자이너에서 엽니다. 

    [![](table-views-images/table05-vs.png "주 storyboard 파일")](table-views-images/table05-vs.png#lightbox)
1. **기본 뷰 컨트롤러**를 선택 하 고 삭제 합니다. 

    [![](table-views-images/table06-vs.png "기본 뷰 컨트롤러를 선택 하 고 삭제 합니다.")](table-views-images/table06-vs.png#lightbox)
1. **도구 상자** 에서 **분할 보기 컨트롤러** 를 선택 하 고 Design Surface 끌어 옵니다. 

    [![](table-views-images/table07-vs.png "분할 뷰 컨트롤러")](table-views-images/table07-vs.png#lightbox)
1. 기본적으로 왼쪽에는 **탐색 뷰 컨트롤러** 와 뷰 컨트롤러, 오른쪽에는 **뷰 컨트롤러** 를 사용 하 여 [분할 뷰가](~/ios/tvos/user-interface/split-views.md) 표시 됩니다. 다음은 tvOS에서 테이블 뷰에 대 한 Apple의 제안 된 사용법입니다. 

    [![](table-views-images/table08-vs.png "UI 레이아웃")](table-views-images/table08-vs.png#lightbox)
1. C# 나중에 코드에서 액세스할 수 있도록 테이블 뷰의 모든 부분을 선택 하 고 **속성 탐색기** 의 **위젯** 탭에서 사용자 지정 **클래스 이름을** 할당 해야 합니다. 예를 들어 **테이블 뷰 컨트롤러**는 다음과 같습니다. 

    [![](table-views-images/table09-vs.png "위젯 탭")](table-views-images/table09-vs.png#lightbox)
1. **테이블 뷰 컨트롤러**, **테이블 뷰** 및 **프로토타입 셀**에 대 한 사용자 지정 클래스를 만들어야 합니다. Mac용 Visual Studio는 사용자 지정 클래스를 만들 때 프로젝트 트리에 추가 합니다. 

    [![](table-views-images/table10-vs.png "프로젝트 트리의 사용자 지정 클래스")](table-views-images/table10-vs.png#lightbox)
1. 그런 다음 Design Surface에서 테이블 뷰를 선택 하 고 필요에 따라 속성을 조정 합니다. 예: **프로토타입 셀** 수 및 **스타일** (일반 또는 그룹화 됨): 

    [![](table-views-images/table11-vs.png "위젯 탭")](table-views-images/table11-vs.png#lightbox)
1. 각 **프로토타입 셀**에 대해 해당 셀을 선택 하 고 **속성 탐색기**의 **위젯** 탭에서 고유한 **식별자** 를 할당 합니다. 이 단계는 나중에 테이블을 채울 때이 식별자가 필요 하므로 _매우 중요_ 합니다. 예를 `AttrCell`들면 다음과 같습니다. 

    [![](table-views-images/table12-vs.png "식별자 할당")](table-views-images/table12-vs.png#lightbox)
1. 또한 **스타일** 드롭다운을 통해 [기본 테이블 뷰 셀 형식](#table-view-cell-types) 으로 셀을 표시 하거나 **사용자 지정** 으로 설정 하 고 Design Surface를 사용 하 여 **도구 상자**에서 다른 UI 위젯에서 끌어 셀을 레이아웃 하도록 선택할 수 있습니다. 

    [![](table-views-images/table13-vs.png "스타일 드롭다운")](table-views-images/table13-vs.png#lightbox)
1. **속성 탐색기** 의 C# **위젯** 탭에서 프로토타입 셀 디자인의 각 UI 요소에 고유한 **이름을** 할당 하 여 나중에 코드에서 액세스할 수 있도록 합니다. 

    [![](table-views-images/table14-vs.png "위젯 탭")](table-views-images/table14-vs.png#lightbox)
1. 테이블 뷰의 모든 프로토타입 셀에 대해 위의 단계를 반복 합니다.
1. 그런 다음 UI 디자인의 나머지 부분에 사용자 지정 클래스를 할당 하 고 자세히 보기를 레이아웃 한 다음에서 C# 액세스할 수 있도록 세부 정보 보기의 각 Ui 요소에 고유한 이름을 할당 합니다. 예를 들면 다음과 같습니다. 

    [![](table-views-images/table15.png "UI 레이아웃")](table-views-images/table15.png#lightbox)
1. 스토리 보드에 변경 내용을 저장 합니다.
    
-----

<a name="Designing-a-Data-Model" />

## <a name="designing-a-data-model"></a>데이터 모델 디자인

사용자가 테이블 뷰에서 행을 선택 하거나 강조 표시 함에 따라 테이블 보기가 더 쉽게 표시 하 고 자세한 정보를 쉽게 표시 하기 위해 정보를 사용 하 여 작업을 수행 하려면 사용자 지정 클래스를 만들어 표시 되는 정보에 대 한 데이터 모델 역할을 수행 합니다. .

각각 사용자가 선택할 수 있는 고유한 **맛보기** 목록을 포함 하는 **도시**목록이 포함 된 여행 예약 앱의 예를 사용 합니다. 사용자는 인력를 *즐겨찾기로*표시할 수 있습니다. 인력에 대 한 *지침* 을 얻고 지정 된 도시에 *비행* 을 이동 하도록 선택 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

**인력**에 대 한 데이터 모델을 만들려면 **Solution Pad** 에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다. 이름 `AttractionInformation` 으로를 입력 하 고 **새로 만들기** 단추를 클릭 합니다. 

[![](table-views-images/data01.png "이름에 AttractionInformation를 입력 합니다.")](table-views-images/data01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

**인력**에 대 한 데이터 모델을 만들려면 **솔루션 탐색기** 에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고**새 항목** **추가** > ...를 선택 합니다. **클래스** 를 선택 하 `AttractionInformation` 고 **이름** 으로를 입력 한 다음 **추가** 단추를 클릭 합니다. 

[![](table-views-images/data01-vs.png "클래스를 선택 하 고 이름으로 AttractionInformation을 입력 합니다.")](table-views-images/data01-vs.png#lightbox)

-----

`AttractionInformation.cs` 파일을 편집 하 여 다음과 같이 만듭니다.

```csharp
using System;
using Foundation;

namespace tvTable
{
    public class AttractionInformation : NSObject
    {
        #region Computed Properties
        public CityInformation City { get; set;}
        public string Name { get; set;}
        public string Description { get; set;}
        public string ImageName { get; set;}
        public bool IsFavorite { get; set;}
        public bool AddDirections { get; set;}
        #endregion

        #region Constructors
        public AttractionInformation (string name, string description, string imageName)
        {
            // Initialize
            this.Name = name;
            this.Description = description;
            this.ImageName = imageName;
        }
        #endregion
    }
}
```

이 클래스는 지정 된 **인력**에 대 한 정보를 저장 하는 속성을 제공 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

그런 다음 **Solution Pad** 에서 프로젝트 이름을 다시 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다. 이름 `CityInformation` 으로를 입력 하 고 **새로 만들기** 단추를 클릭 합니다. 

[![](table-views-images/data02.png "이름에 CityInformation를 입력 합니다.")](table-views-images/data02.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

그런 다음 **솔루션 탐색기** 에서 프로젝트 이름을 다시 마우스 오른쪽 단추로 클릭 하 고 **추가** > **새 항목**...을 선택 합니다. 이름 `CityInformation` 으로를 입력 하 고 **추가** 단추를 클릭 합니다. 

[![](table-views-images/data02-vs.png "이름에 CityInformation를 입력 합니다.")](table-views-images/data02-vs.png#lightbox)

-----

`CityInformation.cs` 파일을 편집 하 여 다음과 같이 만듭니다.

```csharp
using System;
using System.Collections.Generic;
using Foundation;

namespace tvTable
{
    public class CityInformation : NSObject
    {
        #region Computed Properties
        public string Name { get; set; }
        public List<AttractionInformation> Attractions { get; set;}
        public bool FlightBooked { get; set;}
        #endregion

        #region Constructors
        public CityInformation (string name)
        {
            // Initialize
            this.Name = name;
            this.Attractions = new List<AttractionInformation> ();
        }
        #endregion

        #region Public Methods
        public void AddAttraction (AttractionInformation attraction)
        {
            // Mark as belonging to this city
            attraction.City = this;

            // Add to collection
            Attractions.Add (attraction);
        }

        public void AddAttraction (string name, string description, string imageName)
        {
            // Create attraction
            var attraction = new AttractionInformation (name, description, imageName);

            // Mark as belonging to this city
            attraction.City = this;

            // Add to collection
            Attractions.Add (attraction);
        }
        #endregion
    }
}
```

이 클래스는 해당 도시의 **맛보기** 컬렉션인 대상 **도시**에 대 한 모든 정보를 포함 하 고, 도시에 맛보기를 더 쉽게`AddAttraction`추가할 수 있도록 두 개의 도우미 메서드 ()를 제공 합니다.

<a name="The-Table-Data-Source" />

## <a name="the-table-view-data-source"></a>테이블 뷰 데이터 원본

각 테이블 뷰에서는 테이블에 대 한`UITableViewDataSource`데이터를 제공 하 고 테이블 뷰에 필요한 행을 생성 하기 위해 데이터 원본 ()이 필요 합니다.

위의 예에서는 **솔루션 탐색기**의 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 `AttractionTableDatasource` **새 파일** **추가** > ...를 선택한 다음 새로 만들기 단추를 클릭 하 여 **새로** 만듭니다. 그런 다음 `AttractionTableDatasource.cs` 파일을 편집 하 여 다음과 같이 만듭니다.

```csharp
using System;
using System.Collections.Generic;
using UIKit;

namespace tvTable
{
    public class AttractionTableDatasource : UITableViewDataSource
    {
        #region Constants
        const string CellID = "AttrCell";
        #endregion

        #region Computed Properties
        public AttractionTableViewController Controller { get; set;}
        public List<CityInformation> Cities { get; set;}
        #endregion

        #region Constructors
        public AttractionTableDatasource (AttractionTableViewController controller)
        {
            // Initialize
            this.Controller = controller;
            this.Cities = new List<CityInformation> ();
            PopulateCities ();
        }
        #endregion

        #region Public Methods
        public void PopulateCities ()
        {
            // Clear existing
            Cities.Clear ();

            // Define cities and attractions
            var Paris = new CityInformation ("Paris");
            Paris.AddAttraction ("Eiffel Tower", "Is a wrought iron lattice tower on the Champ de Mars in Paris, France.", "EiffelTower");
            Paris.AddAttraction ("Musée du Louvre", "is one of the world's largest museums and a historic monument in Paris, France.", "Louvre");
            Paris.AddAttraction ("Moulin Rouge", "French for 'Red Mill', is a cabaret in Paris, France.", "MoulinRouge");
            Paris.AddAttraction ("La Seine", "Is a 777-kilometre long river and an important commercial waterway within the Paris Basin.", "RiverSeine");
            Cities.Add (Paris);

            var SanFran = new CityInformation ("San Francisco");
            SanFran.AddAttraction ("Alcatraz Island", "Is located in the San Francisco Bay, 1.25 miles (2.01 km) offshore from San Francisco.", "Alcatraz");
            SanFran.AddAttraction ("Golden Gate Bridge", "Is a suspension bridge spanning the Golden Gate strait between San Francisco Bay and the Pacific Ocean", "GoldenGateBridge");
            SanFran.AddAttraction ("San Francisco", "Is the cultural, commercial, and financial center of Northern California.", "SanFrancisco");
            SanFran.AddAttraction ("Telegraph Hill", "Is primarily a residential area, much quieter than adjoining North Beach.", "TelegraphHill");
            Cities.Add (SanFran);

            var Houston = new CityInformation ("Houston");
            Houston.AddAttraction ("City Hall", "It was constructed in 1938-1939, and is located in Downtown Houston.", "CityHall");
            Houston.AddAttraction ("Houston", "Is the most populous city in Texas and the fourth most populous city in the US.", "Houston");
            Houston.AddAttraction ("Texas Longhorn", "Is a breed of cattle known for its characteristic horns, which can extend to over 6 ft.", "LonghornCattle");
            Houston.AddAttraction ("Saturn V Rocket", "was an American human-rated expendable rocket used by NASA between 1966 and 1973.", "Rocket");
            Cities.Add (Houston);
        }
        #endregion

        #region Override Methods
        public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            // Get cell
            var cell = tableView.DequeueReusableCell (CellID) as AttractionTableCell;

            // Populate cell
            cell.Attraction = Cities [indexPath.Section].Attractions [indexPath.Row];

            // Return new cell
            return cell;
        }

        public override nint NumberOfSections (UITableView tableView)
        {
            // Return number of cities
            return Cities.Count;
        }

        public override nint RowsInSection (UITableView tableView, nint section)
        {
            // Return the number of attractions in the given city
            return Cities [(int)section].Attractions.Count;
        }

        public override string TitleForHeader (UITableView tableView, nint section)
        {
            // Get the name of the current city
            return Cities [(int)section].Name;
        }
        #endregion
    }
}
```

클래스의 몇 가지 섹션을 자세히 살펴보겠습니다.

먼저 프로토타입 셀의 고유 식별자 (위의 인터페이스 디자이너에서 할당 된 식별자)를 포함 하는 상수를 정의 하 고 테이블 뷰 컨트롤러에 바로 가기를 추가 하 고 데이터에 대 한 저장소를 만들었습니다.

```csharp
const string CellID = "AttrCell";
public AttractionTableViewController Controller { get; set;}
public List<CityInformation> Cities { get; set;}
```

다음으로, 클래스를 만들 때 테이블 뷰 컨트롤러를 저장 한 다음 위에 정의 된 데이터 모델을 사용 하 여 데이터 소스를 작성 하 고 채웁니다.

```csharp
public AttractionTableDatasource (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
    this.Cities = new List<CityInformation> ();
    PopulateCities ();
}
```

예를 들어, 메서드는 `PopulateCities` 단순히 메모리에 데이터 모델 개체를 만들지만 실제 응용 프로그램의 데이터베이스 또는 웹 서비스에서 쉽게 읽을 수 있습니다.

```csharp
public void PopulateCities ()
{
    // Clear existing
    Cities.Clear ();

    // Define cities and attractions
    var Paris = new CityInformation ("Paris");
    Paris.AddAttraction ("Eiffel Tower", "Is a wrought iron lattice tower on the Champ de Mars in Paris, France.", "EiffelTower");
    ...
}
```

메서드 `NumberOfSections` 는 테이블에 있는 섹션의 수를 반환 합니다.

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    // Return number of cities
    return Cities.Count;
}
```

**일반** 스타일의 테이블 뷰에서는 항상 1을 반환 합니다.

메서드 `RowsInSection` 는 현재 섹션의 행 수를 반환 합니다.

```csharp
public override nint RowsInSection (UITableView tableView, nint section)
{
    // Return the number of attractions in the given city
    return Cities [(int)section].Attractions.Count;
}
```

다시, **일반** 테이블 보기의 경우 데이터 원본에 있는 총 항목 수를 반환 합니다.

메서드 `TitleForHeader` 는 지정 된 섹션에 대 한 제목을 반환 합니다.

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    // Get the name of the current city
    return Cities [(int)section].Name;
}
```

**일반** 테이블 뷰 형식의 경우 제목 (`""`)을 비워 둡니다.

마지막으로 테이블 뷰에서 요청 하는 경우 메서드를 `GetCell` 사용 하 여 프로토타입 셀을 만들고 채웁니다. 

```csharp
public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    // Get cell
    var cell = tableView.DequeueReusableCell (CellID) as AttractionTableCell;

    // Populate cell
    cell.Attraction = Cities [indexPath.Section].Attractions [indexPath.Row];

    // Return new cell
    return cell;
}
```

로 작업 하는 `UITableViewDatasource`방법에 대 한 자세한 내용은 Apple의 [uitableviewdatasource](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40006941) 설명서를 참조 하세요.

<a name="The-Table-View-Delegate" />

## <a name="the-table-view-delegate"></a>테이블 뷰 대리자

각 테이블 뷰에서는 테이블의 사용자`UITableViewDelegate`상호 작용 또는 다른 시스템 이벤트에 응답 하는 대리자 ()가 필요 합니다.

위의 예에서는 **솔루션 탐색기**의 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 `AttractionTableDelegate` **새 파일** **추가** > ...를 선택한 다음 새로 만들기 단추를 클릭 하 여 **새로** 만듭니다. 그런 다음 `AttractionTableDelegate.cs` 파일을 편집 하 여 다음과 같이 만듭니다.

```csharp
using System;
using System.Collections.Generic;
using UIKit;

namespace tvTable
{
    public class AttractionTableDelegate : UITableViewDelegate
    {
        #region Computed Properties
        public AttractionTableViewController Controller { get; set;}
        #endregion

        #region Constructors
        public AttractionTableDelegate (AttractionTableViewController controller)
        {
            // Initializw
            this.Controller = controller;
        }
        #endregion

        #region Override Methods
        public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
            attraction.IsFavorite = (!attraction.IsFavorite);

            // Update UI
            Controller.TableView.ReloadData ();
        }

        public override bool CanFocusRow (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            // Inform caller of highlight change
            RaiseAttractionHighlighted (Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row]);
            return true;
        }
        #endregion

        #region Events
        public delegate void AttractionHighlightedDelegate (AttractionInformation attraction);
        public event AttractionHighlightedDelegate AttractionHighlighted;

        internal void RaiseAttractionHighlighted (AttractionInformation attraction)
        {
            // Inform caller
            if (this.AttractionHighlighted != null) this.AttractionHighlighted (attraction);
        }
        #endregion
    }
}
```

이 클래스의 몇 가지 섹션을 자세히 살펴보겠습니다.

먼저 클래스를 만들 때 테이블 뷰 컨트롤러에 대 한 바로 가기를 만듭니다.

```csharp
public AttractionTableViewController Controller { get; set;}
...

public AttractionTableDelegate (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
}
```

그런 다음, 행이 선택 되 면 (사용자가 Apple 원격의 터치 화면을 클릭 하면) 선택한 행으로 표시 되는 **인력** 을 즐겨찾기로 표시 하려고 합니다.

```csharp
public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
    attraction.IsFavorite = (!attraction.IsFavorite);

    // Update UI
    Controller.TableView.ReloadData ();
}
```

그런 다음 사용자가 행을 강조 표시할 때 (Apple 원격 터치 화면을 사용 하 여 포커스를 제공) 분할 보기 컨트롤러의 세부 정보 섹션에 해당 행으로 표시 되는 **인력** 의 세부 정보를 표시 하려고 합니다.

```csharp
public override bool CanFocusRow (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    // Inform caller of highlight change
    RaiseAttractionHighlighted (Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row]);
    return true;
}
...

public delegate void AttractionHighlightedDelegate (AttractionInformation attraction);
public event AttractionHighlightedDelegate AttractionHighlighted;

internal void RaiseAttractionHighlighted (AttractionInformation attraction)
{
    // Inform caller
    if (this.AttractionHighlighted != null) this.AttractionHighlighted (attraction);
}
```

테이블 뷰에서 포커스를 가져오는 각 행에 대해 메서드가호출됩니다.`CanFocusRow` 행 `true` 이 포커스를 받을 수 있으면를 반환 하 `false`고, 그렇지 않으면를 반환 합니다. 이 예제의 경우 포커스를 받을 때 각 행에 대해 발생 `AttractionHighlighted` 하는 사용자 지정 이벤트를 만들었습니다.

로 작업 하는 `UITableViewDelegate`방법에 대 한 자세한 내용은 Apple의 [uitableviewdelegate](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40006942) 설명서를 참조 하세요.

<a name="The-Table-View-Cell" />

## <a name="the-table-view-cell"></a>테이블 뷰 셀

인터페이스 디자이너에서 테이블 뷰에 추가한 각 프로토타입 셀에 대해서도 테이블 뷰 셀`UITableViewCell`()의 사용자 지정 인스턴스를 만들어 새 셀 (행)을 만들 때 채울 수 있습니다.

예제 앱의 경우 `AttractionTableCell.cs` 파일을 두 번 클릭 하 여 편집용으로 열고 다음과 같이 표시 합니다.

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionTableCell : UITableViewCell
    {
        #region Private Variables
        private AttractionInformation _attraction = null;
        #endregion

        #region Computed Properties
        public AttractionInformation Attraction {
            get { return _attraction; }
            set {
                _attraction = value;
                UpdateUI ();
            }
        }
        #endregion

        #region Constructors
        public AttractionTableCell (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Private Methods
        private void UpdateUI ()
        {
            // Trap all errors
            try {
                Title.Text = Attraction.Name;
                Favorite.Hidden = (!Attraction.IsFavorite);
            } catch {
                // Since the UI might not be fully loaded, ignore
                // all errors at this point
            }
        }
        #endregion
    }
}
```

이 클래스는 지정 된 행에 표시 된 인력 데이터`AttractionInformation` 모델 개체에 대 한 저장소를 제공 합니다.

```csharp
private AttractionInformation _attraction = null;
...

public AttractionInformation Attraction {
    get { return _attraction; }
    set {
        _attraction = value;
        UpdateUI ();
    }
}
```

메서드 `UpdateUI` 는 필요에 따라 인터페이스 디자이너에서 셀의 프로토타입에 추가 된 UI 위젯을 채웁니다.

```csharp
private void UpdateUI ()
{
    // Trap all errors
    try {
        Title.Text = Attraction.Name;
        Favorite.Hidden = (!Attraction.IsFavorite);
    } catch {
        // Since the UI might not be fully loaded, ignore
        // all errors at this point
    }
}
```

로 작업 하는 `UITableViewCell`방법에 대 한 자세한 내용은 Apple의 [uitableviewcell](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewCell_Class/index.html#//apple_ref/doc/uid/TP40006938) 설명서를 참조 하세요.

<a name="The-Table-View-Controller" />

## <a name="the-table-view-controller"></a>테이블 뷰 컨트롤러

테이블 뷰 컨트롤러 (`UITableViewController`)는 인터페이스 디자이너를 통해 Storyboard에 추가 된 테이블 뷰를 관리 합니다.

예제 앱의 경우 `AttractionTableViewController.cs` 파일을 두 번 클릭 하 여 편집용으로 열고 다음과 같이 표시 합니다.

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionTableViewController : UITableViewController
    {
        #region Computed Properties
        public AttractionTableDatasource Datasource {
            get { return TableView.DataSource as AttractionTableDatasource; }
        }

        public AttractionTableDelegate TableDelegate {
            get { return TableView.Delegate as AttractionTableDelegate; }
        }
        #endregion

        #region Constructors
        public AttractionTableViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Setup table
            TableView.DataSource = new AttractionTableDatasource (this);
            TableView.Delegate = new AttractionTableDelegate (this);
            TableView.ReloadData ();
        }
        #endregion
    }
}
```

이 클래스를 좀 더 자세히 살펴보겠습니다. 먼저 테이블 뷰의 `DataSource` 및 `TableDelegate`에 보다 쉽게 액세스할 수 있도록 하는 바로 가기를 만들었습니다. 나중에이를 사용 하 여 분할 뷰의 왼쪽에 있는 테이블 뷰와 오른쪽의 자세히 보기 사이에 통신 합니다.

마지막으로 테이블 뷰가 메모리에 로드 되 면 `AttractionTableDatasource` 및 `AttractionTableDelegate` (위에서 만든)의 인스턴스를 만들고이를 테이블 뷰에 연결 합니다.

로 작업 하는 `UITableViewController`방법에 대 한 자세한 내용은 Apple의 [uitableviewcontroller](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523) 설명서를 참조 하세요.

<a name="Pulling-it-All-Together" />

## <a name="pulling-it-all-together"></a>모두 함께 끌어오기

이 문서의 시작 부분에서 설명한 것 처럼 테이블 뷰는 일반적으로 [분할 보기](~/ios/tvos/user-interface/split-views.md) 의 한쪽에 탐색으로 표시 되 고 반대쪽에는 선택한 항목의 세부 정보가 표시 됩니다. 예를 들어: 

[![](table-views-images/intro01.png "샘플 앱 실행")](table-views-images/intro01.png#lightbox)

이 패턴은 tvOS의 표준 패턴 이므로 모든 항목을 함께 가져오고 분할 보기의 왼쪽과 오른쪽이 서로 상호 작용 하는 마지막 단계를 살펴보겠습니다.

<a name="The-Detail-View" />

### <a name="the-detail-view"></a>자세히 보기

위에서 언급 한 여행 앱의 예제에서는 사용자 지정 클래스 (`AttractionViewController`)가 분할 뷰의 오른쪽에 자세히 보기로 표시 되는 표준 뷰 컨트롤러에 대해 정의 됩니다.

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionViewController : UIViewController
    {
        #region Private Variables
        private AttractionInformation _attraction = null;
        #endregion

        #region Computed Properties
        public AttractionInformation Attraction {
            get { return _attraction; }
            set {
                _attraction = value;
                UpdateUI ();
            }
        }

        public MasterSplitView SplitView { get; set;}
        #endregion

        #region Constructors
        public AttractionViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Public Methods
        public void UpdateUI ()
        {
            // Trap all errors
            try {
                City.Text = Attraction.City.Name;
                Title.Text = Attraction.Name;
                SubTitle.Text = Attraction.Description;

                IsFlighBooked.Hidden = (!Attraction.City.FlightBooked);
                IsFavorite.Hidden = (!Attraction.IsFavorite);
                IsDirections.Hidden = (!Attraction.AddDirections);
                BackgroundImage.Image = UIImage.FromBundle (Attraction.ImageName);
                AttractionImage.Image = BackgroundImage.Image;
            } catch {
                // Since the UI might not be fully loaded, ignore
                // all errors at this point
            }
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Ensure the UI Updates
            UpdateUI ();
        }
        #endregion

        #region Actions
        partial void BookFlight (NSObject sender)
        {
            // Ask user to book flight
            AlertViewController.PresentOKCancelAlert ("Book Flight",
                                                      string.Format ("Would you like to book a flight to {0}?", Attraction.City.Name),
                                                      this,
                                                      (ok) => {
                Attraction.City.FlightBooked = ok;
                IsFlighBooked.Hidden = (!Attraction.City.FlightBooked);
            });
        }

        partial void GetDirections (NSObject sender)
        {
            // Ask user to add directions
            AlertViewController.PresentOKCancelAlert ("Add Directions",
                                                     string.Format ("Would you like to add directions to {0} to you itinerary?", Attraction.Name),
                                                     this,
                                                     (ok) => {
                                                         Attraction.AddDirections = ok;
                                                         IsDirections.Hidden = (!Attraction.AddDirections);
                                                     });
        }

        partial void MarkFavorite (NSObject sender)
        {
            // Flip favorite state
            Attraction.IsFavorite = (!Attraction.IsFavorite);
            IsFavorite.Hidden = (!Attraction.IsFavorite);

            // Reload table
            SplitView.Master.TableController.TableView.ReloadData ();
        }
        #endregion
    }
}
```

여기서는 속성으로 표시 되 는 인력`AttractionInformation`()를 제공 하 고, 인터페이스 디자이너 `UpdateUI` 에서 뷰에 추가 된 UI 위젯을 채우는 메서드를 만들었습니다.

또한 변경 내용을 테이블 뷰 (`SplitView``AcctractionTableView`)로 다시 전달 하는 데 사용할 분할 뷰 컨트롤러 ()에 대 한 바로 가기를 정의 했습니다.

마지막으로 사용자 지정 작업 (이벤트)이 인터페이스 디자이너에서 `UIButton` 만든 세 개의 인스턴스에 추가 되었습니다 .이를 통해 사용자는 인력을 _즐겨찾기로_표시 하 고, 인력에 게 _방향을_ 가져오고, 지정 된으로 _비행_ 을 이동할 수 있습니다. 대도시.

<a name="The-Navigation-View-Controller" />

### <a name="the-navigation-view-controller"></a>탐색 뷰 컨트롤러

테이블 뷰 컨트롤러는 분할 뷰의 왼쪽에 있는 탐색 뷰 컨트롤러에 중첩 되어 있으므로, 탐색 뷰 컨트롤러는 인터페이스 디자이너에서 사용자 지정 클래스 (`MasterNavigationController`)에 할당 되 고 다음과 같이 정의 됩니다.

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class MasterNavigationController : UINavigationController
    {
        #region Computed Properties
        public MasterSplitView SplitView { get; set;}
        public AttractionTableViewController TableController {
            get { return TopViewController as AttractionTableViewController; }
        }
        #endregion

        #region Constructors
        public MasterNavigationController (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

이 클래스는 분할 뷰 컨트롤러의 두 면에서 보다 쉽게 통신할 수 있도록 몇 가지 바로 가기를 정의 하기만 합니다.

* `SplitView`-탐색 뷰 컨트롤러가 속한 분할 뷰 컨트롤러 (`MainSpiltViewController`)에 대 한 링크입니다.
* `TableController`-탐색 뷰 컨트롤러에서 최상위 뷰로`AttractionTableViewController`표시 되는 테이블 뷰 컨트롤러 ()를 가져옵니다.

<a name="The-Split-View-Controller" />

### <a name="the-split-view-controller"></a>분할 뷰 컨트롤러

분할 뷰 컨트롤러는 응용 프로그램의 기반 이므로 인터페이스 디자이너에서 해당 클래스에 대 한 사용자`MasterSplitViewController`지정 클래스 ()를 만들고 다음과 같이 정의 했습니다.

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class MasterSplitView : UISplitViewController
    {
        #region Computed Properties
        public AttractionViewController Details {
            get { return ViewControllers [1] as AttractionViewController; }
        }

        public MasterNavigationController Master {
            get { return ViewControllers [0] as MasterNavigationController; }
        }
        #endregion

        #region Constructors
        public MasterSplitView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            Master.SplitView = this;
            Details.SplitView = this;

            // Wire-up events
            Master.TableController.TableDelegate.AttractionHighlighted += (attraction) => {
                // Display new attraction
                Details.Attraction = attraction;
            };
        }
        #endregion
    }
}
```

먼저 분할 보기 (`AttractionViewController`)와 **마스터** 쪽 (`MasterNavigationController`)에 대 한 바로 가기를 만듭니다. 이를 통해 나중에 두 쪽 간에 더 쉽게 통신할 수 있습니다.

그런 다음 분할 뷰가 메모리에 로드 되 면 분할 보기의 양쪽에 분할 뷰 컨트롤러를 연결 하 고의`AttractionHighlighted` **세부 정보** 쪽에 새 인력를 표시 하 여 테이블 보기 ()의 인력를 강조 표시 하는 사용자에 게 응답 합니다. 분할 뷰입니다.

분할 뷰 내에서 테이블 뷰의 전체 구현을 보려면 [tvTables](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvtable) 샘플 앱을 참조 하세요.

## <a name="table-views-in-detail"></a>테이블 보기 세부 정보

TvOS는 iOS의 기반 이므로 테이블 뷰와 테이블 뷰 컨트롤러는 비슷한 방식으로 디자인 되 고 동작 합니다. Xamarin 앱에서 테이블 뷰로 작업 하는 방법에 대 한 자세한 내용은 iOS [테이블 및 셀 작업](~/ios/user-interface/controls/tables/index.md) 설명서를 참조 하세요.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 tvOS 앱 내에서 테이블 뷰를 디자인 하 고 작업 하는 방법에 대해 설명 했습니다. 및는 tvOS 앱에서 테이블 뷰의 일반적인 사용 인 분할 뷰 내의 테이블 뷰로 작업 하는 예를 제공 했습니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
