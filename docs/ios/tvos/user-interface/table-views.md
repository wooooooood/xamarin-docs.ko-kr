---
title: TvOS Xamarin의 테이블 뷰를 사용 하 여 작업
description: 이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 테이블 뷰 및 테이블 뷰 컨트롤러를 사용 하 여 작업을 설명 합니다.
ms.prod: xamarin
ms.assetid: D8F80FA9-6400-4DB7-AFC9-A28A54AD04E8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 3e7fc3d627b5d7a1dc73caa395a9181efb0b5f08
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61356137"
---
# <a name="working-with-tvos-table-views-in-xamarin"></a>TvOS Xamarin의 테이블 뷰를 사용 하 여 작업

_이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 테이블 뷰 및 테이블 뷰 컨트롤러를 사용 하 여 작업을 설명 합니다._

TvOS 테이블 뷰를 필요에 따라 그룹 또는 섹션으로 구성 될 수 있는 행을 스크롤의 단일 열으로 표시 됩니다. 방법을 이해 하는 암호화 되지 않은 사용자에 게 많은 양의 데이터를 효율적으로 표시 해야 하는 경우 테이블 뷰를 사용 해야 합니다.

테이블 보기의 한 쪽에서 일반적으로 표시 되는 [분할 뷰](~/ios/tvos/user-interface/split-views.md) 반대쪽에 표시 되는 선택한 항목의 세부 정보를 사용 하 여 탐색으로:

[![](table-views-images/intro01.png "샘플 테이블 보기")](table-views-images/intro01.png#lightbox)

<a name="About-Table-Views" />

## <a name="about-table-views"></a>테이블 뷰 정보

`UITableView` 필요에 따라 그룹 또는 섹션으로 구성 될 수 있는 정보의 계층적 목록으로 스크롤할 수 있는 행의 단일 열이 표시 됩니다. 

[![](table-views-images/table01.png "선택한 항목")](table-views-images/table01.png#lightbox)

Apple에 테이블 사용을 위한 다음 제안에 있습니다.

- **인식의 너비로** -사용자 테이블 너비의 올바른 균형 하려고 합니다. 테이블이 너무 넓은 경우 거리를 검색 하기 어려울 수 있습니다 하 고 사용할 수 있는 콘텐츠 영역에서 벗어날 수 있습니다. 테이블이 너무 좁은 경우 잘릴 수 정보를 발생할 수 있습니다 또는 줄 바꿈, 다시이 어려울 수 있습니다 대화방에서에서 읽을 수 있습니다.
- **테이블 내용을 빠르게 표시** -데이터의 대규모 목록에 대 한 콘텐츠를 지연 로드 하 고 정보를 보여 주는 테이블 사용자에 게 표시 되는 즉시 시작 합니다. 테이블에 로드 하려면 오래 걸리는, 앱 또는 생각을 잠근에 관심을 손실 될 수 있습니다.
- **사용자의 긴 콘텐츠 로드 상태를 알릴** -긴 테이블 로드 시간을 피할 수 없는, 있는 경우는 [진행률 표시줄 또는 활동 표시기](~/ios/tvos/user-interface/progress-indicators.md) 앱 알 수 있도록 잠근 되지 않았습니다.

<a name="Table-Cell-Types" />

## <a name="table-view-cell-types"></a>테이블 뷰 셀 형식

`UITableViewCell` 테이블 뷰에서 데이터의 개별 행을 나타내는 데 사용 됩니다. Apple에는 여러 기본 테이블 셀 형식 정의:

- **기본** -옵션 이미지 왼쪽 및 오른쪽에서 왼쪽으로 맞춰져 있으며 제목 셀에이 형식 표시 합니다. 
- **부제목** -첫 번째 줄 및 보다 작은 왼쪽으로 맞춰져 있으며 제목 왼쪽으로 맞춰져 있으며 부제목 다음 줄에서이 형식 표시 합니다.
- **값 1** -이 유형 같은 줄에서 왼쪽으로 맞춰져 있으며 제목 색이 지정 된 밝게, 오른쪽 맞춤 부제목을 제공 합니다.
- **값 2** -이 유형 같은 줄에 밝게 색이 지정 된, 왼쪽으로 맞춰져 있으며 부제목 오른쪽 맞춤 제목을 제공 합니다.

기본 테이블 뷰 셀 형식의 모든 공개 지표 또는 확인 표시와 같은 그래픽 요소도 지원합니다. 

또한 정의할 수 있습니다는 **사용자 지정** 테이블 뷰 셀 형식 및 표시를 _프로토타입 셀_, 하거나 만드는 인터페이스 디자이너 또는 코드를 통해.

Apple는 테이블 뷰 셀을 사용 하 여 작업 하기 위한 다음 제안에 있습니다.

- **텍스트 클리핑 방지** -짧은 텍스트의 개별 행 되도록 유지 하지 결국 잘린 합니다. 잘린 단어나 구를 어려운 대화방에서에서 구문 분석을 사용자에 대 한 합니다.
- **Focused 행 상태를 고려** 행 반올림 더 큰 없으므로-셀의 모양을 모든 상태에서 테스트 해야 할 모서리에 포커스가 있 때. 이미지 또는 텍스트 잘린 될 수 있습니다 또는 Focused 상태에서 잘못 확인 합니다.
- **편집할 수 있는 테이블을 제한적으로 사용 하 여** -이동 하거나 테이블 행을 삭제 하는 보다 많은 시간이 걸립니다 tvOS에 iOS입니다. 이 기능은 추가 또는 tvOS 앱에서 방해가 됩니다 신중 하 게 결정 해야 합니다.
- **사용자 지정 셀 형식에 적절 한 만들기** 기본 제공 테이블 뷰 셀 형식은 대부분의 경우에 적합 하는 동안-제어를 강화 하 고 정보를 더 잘 표현에 비표준 정보에 대 한 사용자 지정 셀 형식을 만드는 것이 좋습니다. 사용자입니다.

<a name="Working-With-Table-Views" />

## <a name="working-with-table-views"></a>테이블 뷰 작업

Xamarin.tvOS 앱에서 테이블 뷰를 사용 하는 가장 쉬운 방법은 만들고 인터페이스 디자이너에서 모양을 수정 하는 경우

시작하려면 다음을 수행합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)
    
1. Mac 용 Visual Studio에서 새 tvOS 앱 프로젝트를 시작 하 고 선택 **tvOS** > **앱** > **단일 뷰 앱** 을 클릭 합니다  **다음** 단추: 

    [![](table-views-images/table02.png "단일 뷰 앱 선택")](table-views-images/table02.png#lightbox)
1. 입력 한 **이름을** 앱 및 클릭 **다음**: 

    [![](table-views-images/table03.png "앱의 이름을 입력 합니다.")](table-views-images/table03.png#lightbox)
1. 조정 하거나 합니다 **프로젝트 이름** 및 **솔루션 이름** 또는 기본값을 사용 하 고 클릭 합니다 **만들기** 새 솔루션을 만드는 단추: 

    [![](table-views-images/table04.png "프로젝트 이름 및 솔루션 이름")](table-views-images/table04.png#lightbox)
1. 에 **Solution Pad**를 두 번 클릭 합니다 `Main.storyboard` iOS 디자이너에서에서 열려는 파일: 

    [![](table-views-images/table05.png "Main.storyboard 파일")](table-views-images/table05.png#lightbox)
1. 선택 하 고 삭제 합니다 **기본 뷰 컨트롤러**: 

    [![](table-views-images/table06.png "선택 하 고 기본 뷰 컨트롤러를 삭제 합니다.")](table-views-images/table06.png#lightbox)
1. 선택 된 **분할 뷰 컨트롤러** 에서 **도구 상자** 디자인 화면으로 끌어 옵니다.
1. 기본적으로 얻을 수는 [분할 뷰](~/ios/tvos/user-interface/split-views.md) 사용 하 여를 **탐색 보기 컨트롤러** 및 **테이블 뷰 컨트롤러** 왼쪽에 및 **뷰 컨트롤러** 오른쪽에 있습니다. TvOS의 테이블 뷰 Apple의 제안 된 사용법입니다. 

    [![](table-views-images/table08.png "분할 된 보기를 추가 합니다.")](table-views-images/table08.png#lightbox)
1. 테이블 보기의 모든 부분을 선택 하 고 사용자 지정을 할당 해야 합니다 **클래스 이름** 에 **위젯** 탭의 **속성 탐색기** C#코드입니다. 예를 들어 합니다 **테이블 뷰 컨트롤러**: 

    [![](table-views-images/table09.png "클래스 이름 할당")](table-views-images/table09.png#lightbox)
1. 에 대 한 사용자 지정 클래스를 만들도록 합니다 **테이블 뷰 컨트롤러**, **테이블 뷰** 임의의 **프로토타입 셀**. 만들 때 Mac 용 visual Studio는 프로젝트 트리에 사용자 지정 클래스를 추가 합니다. 

    [![](table-views-images/table10.png "프로젝트 트리에서 사용자 지정 클래스")](table-views-images/table10.png#lightbox)
1. 다음으로, 디자인 화면에서 테이블 보기를 선택 하 고 필요에 따라 해당 속성을 조정 합니다. 수와 같은 **프로토타입 셀** 하며 **스타일** (일반 또는 그룹화 기준): 

    [![](table-views-images/table11.png "위젯 탭")](table-views-images/table11.png#lightbox)
1. 각각에 대 한 **프로토타입 셀**선택 하 고 고유한 할당 **식별자** 에 **위젯** 탭을 **속성 탐색기**합니다. 이 단계는 _매우 중요 한_ 나중에이 식별자는 필요에 따라 채울 경우 테이블입니다. 예를 들어 `AttrCell`: 

    [![](table-views-images/table12.png "위젯 탭")](table-views-images/table12.png#lightbox)
1. 중 하나로 셀을 표시 하도록 선택할 수도 있습니다는 [기본 테이블 뷰 셀 형식](#table-view-cell-types) 를 통해 합니다 **스타일** 드롭다운으로 설정 하거나 **사용자 지정** 셀 레이아웃을 디자인 화면을 사용 하 여 다른 UI 위젯 내에서 끌어 합니다 **도구 상자**: 

    [![](table-views-images/table13.png "셀 레이아웃")](table-views-images/table13.png#lightbox)
1. 고유한 할당 **이름을** 의 프로토타입 셀 디자인에서는 각 UI 요소에는 **위젯** 탭의 **속성 탐색기** 되므로 나중에 액세스할 수 있습니다 C# 코드: 

    [![](table-views-images/table14.png "이름을 할당합니다")](table-views-images/table14.png#lightbox)
1. 테이블 보기의 프로토타입 셀을 모두에 대해 위의 단계를 반복 합니다.
1. 다음으로, UI 디자인, 세부 정보 보기 레이아웃 및 고유한 할당의 나머지 부분에 사용자 지정 클래스를 할당 **이름을** 세부 정보에서 각 UI 요소에 액세스할 수 있도록 확인 C# 도 합니다. 예를 들면 다음과 같습니다. 

    [![](table-views-images/table15.png "UI 레이아웃")](table-views-images/table15.png#lightbox)
1. 스토리 보드에 변경 내용을 저장 합니다.
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. Visual Studio에서 새 tvOS 앱 프로젝트를 시작 하 고 선택 **tvOS** > **단일 뷰 앱** 앱에 대 한 이름을 입력 합니다. 클릭 합니다 **알겠습니다** 단추를 새 솔루션 만들기: 

    [![](table-views-images/table02-vs.png "단일 뷰 앱 선택")](table-views-images/table02-vs.png#lightbox)
1. 에 **솔루션 탐색기**를 두 번 클릭 합니다 `Main.storyboard` iOS 디자이너에서에서 열려는 파일: 

    [![](table-views-images/table05-vs.png "Main.storyboard 파일")](table-views-images/table05-vs.png#lightbox)
1. 선택 하 고 삭제 합니다 **기본 뷰 컨트롤러**: 

    [![](table-views-images/table06-vs.png "선택 하 고 기본 뷰 컨트롤러를 삭제 합니다.")](table-views-images/table06-vs.png#lightbox)
1. 선택는 **분할 뷰 컨트롤러** 에서 합니다 **도구 상자** 디자인 화면으로 끌어 옵니다. 

    [![](table-views-images/table07-vs.png "분할 뷰 컨트롤러")](table-views-images/table07-vs.png#lightbox)
1. 기본적으로 얻을 수는 [분할 뷰](~/ios/tvos/user-interface/split-views.md) 사용 하 여를 **탐색 보기 컨트롤러** 및 **테이블 뷰 컨트롤러** 왼쪽에 및 **뷰 컨트롤러** 오른쪽에 있습니다. TvOS의 테이블 뷰 Apple의 제안 된 사용법입니다. 

    [![](table-views-images/table08-vs.png "레이아웃 UI")](table-views-images/table08-vs.png#lightbox)
1. 테이블 보기의 모든 부분을 선택 하 고 사용자 지정을 할당 해야 합니다 **클래스 이름** 에 **위젯** 탭의 **속성 탐색기** C#코드입니다. 예를 들어 합니다 **테이블 뷰 컨트롤러**: 

    [![](table-views-images/table09-vs.png "위젯 탭")](table-views-images/table09-vs.png#lightbox)
1. 에 대 한 사용자 지정 클래스를 만들도록 합니다 **테이블 뷰 컨트롤러**, **테이블 뷰** 임의의 **프로토타입 셀**. 만들 때 Mac 용 visual Studio는 프로젝트 트리에 사용자 지정 클래스를 추가 합니다. 

    [![](table-views-images/table10-vs.png "프로젝트 트리에서 사용자 지정 클래스")](table-views-images/table10-vs.png#lightbox)
1. 다음으로, 디자인 화면에서 테이블 보기를 선택 하 고 필요에 따라 해당 속성을 조정 합니다. 수와 같은 **프로토타입 셀** 하며 **스타일** (일반 또는 그룹화 기준): 

    [![](table-views-images/table11-vs.png "위젯 탭")](table-views-images/table11-vs.png#lightbox)
1. 각각에 대 한 **프로토타입 셀**선택 하 고 고유한 할당 **식별자** 에 **위젯** 탭을 **속성 탐색기**합니다. 이 단계는 _매우 중요 한_ 나중에이 식별자는 필요에 따라 채울 경우 테이블입니다. 예를 들어 `AttrCell`: 

    [![](table-views-images/table12-vs.png "식별자를 할당 합니다.")](table-views-images/table12-vs.png#lightbox)
1. 중 하나로 셀을 표시 하도록 선택할 수도 있습니다는 [기본 테이블 뷰 셀 형식](#table-view-cell-types) 를 통해 합니다 **스타일** 드롭다운으로 설정 하거나 **사용자 지정** 셀 레이아웃을 디자인 화면을 사용 하 여 다른 UI 위젯 내에서 끌어 합니다 **도구 상자**: 

    [![](table-views-images/table13-vs.png "스타일 드롭다운")](table-views-images/table13-vs.png#lightbox)
1. 고유한 할당 **이름을** 의 프로토타입 셀 디자인에서는 각 UI 요소에는 **위젯** 탭의 **속성 탐색기** 되므로 나중에 액세스할 수 있습니다 C# 코드: 

    [![](table-views-images/table14-vs.png "위젯 탭")](table-views-images/table14-vs.png#lightbox)
1. 테이블 보기의 프로토타입 셀을 모두에 대해 위의 단계를 반복 합니다.
1. 다음으로, UI 디자인, 세부 정보 보기 레이아웃 및 고유한 할당의 나머지 부분에 사용자 지정 클래스를 할당 **이름을** 세부 정보에서 각 UI 요소에 액세스할 수 있도록 확인 C# 도 합니다. 예를 들면 다음과 같습니다. 

    [![](table-views-images/table15.png "UI 레이아웃")](table-views-images/table15.png#lightbox)
1. 스토리 보드에 변경 내용을 저장 합니다.
    
-----

<a name="Designing-a-Data-Model" />

## <a name="designing-a-data-model"></a>데이터 모델 디자인

테이블 보기 쉽게 표시 하는 정보를 사용 하 여 작업을 수행할 수와 자세한 정보 표시 (사용자 선택 하거나 표 뷰의 행을 강조 표시)으로 쉽게, 사용자 지정 클래스를 만들거나 정보에 대 한 데이터 모델로 작동 하는 클래스를 .

목록을 포함 하는 여행 예약 응용 프로그램의 예로 **도시**의 고유한 목록을 포함 하는 각 **실감** 사용자가 선택할 수 있는 합니다. 사용자는 인력으로 표시할 수는 *즐겨 찾는*가져오려면 선택 합니다 *지침* 는 인력을 및 *항공권 예약* 지정 도시입니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

에 대 한 데이터 모델을 만드는 **인력**에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 합니다 **Solution Pad** 선택한 **추가** > **새 파일...** . 입력 `AttractionInformation` 에 대 한 합니다 **이름** 을 클릭 합니다 **새로 만들기** 단추: 

[![](table-views-images/data01.png "AttractionInformation 이름 입력")](table-views-images/data01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

에 대 한 데이터 모델을 만드는 **인력**에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 합니다 **솔루션 탐색기** 선택한 **추가** > **새 항목 ...** . 선택 **클래스** enter `AttractionInformation` 에 대 한를 **이름** 을 클릭 합니다 **추가** 단추: 

[![](table-views-images/data01-vs.png "클래스를 선택 하 고 이름에 대 한 AttractionInformation 입력")](table-views-images/data01-vs.png#lightbox)

-----

편집 된 `AttractionInformation.cs` 파일을 다음과 같이 표시 되도록 합니다.

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

에 대 한 정보를 저장 하는 속성을 제공 하는이 클래스는 주어진 **인력**합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

그런 다음에 프로젝트 이름을 마우스 오른쪽 단추로 클릭 합니다 **Solution Pad** 다시 선택한 **추가** > **새 파일...** . 입력 `CityInformation` 에 대 한 합니다 **이름** 을 클릭 합니다 **새로 만들기** 단추: 

[![](table-views-images/data02.png "CityInformation 이름 입력")](table-views-images/data02.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

그런 다음에 프로젝트 이름을 마우스 오른쪽 단추로 클릭 합니다 **솔루션 탐색기** 다시 선택한 **추가** > **새 항목...** . 입력 `CityInformation` 에 대 한 합니다 **이름** 을 클릭 합니다 **추가** 단추: 

[![](table-views-images/data02-vs.png "CityInformation 이름 입력")](table-views-images/data02-vs.png#lightbox)

-----

편집 된 `CityInformation.cs` 파일을 다음과 같이 표시 되도록 합니다.

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

이 클래스는 대상에 대 한 모든 정보를 보유 **도시**, 컬렉션인 **실감** 도시에 대 한 두 개의 도우미 메서드를 제공 하 고 (`AddAttraction`) 실감을 추가할 수 있도록는 도시입니다.

<a name="The-Table-Data-Source" />

## <a name="the-table-view-data-source"></a>테이블 뷰에서 데이터 원본

각 테이블 뷰에 필요한 데이터 원본 (`UITableViewDataSource`) 테이블 보기에 필요한 테이블에 데이터를 제공 하 여으로 필요한 행을 생성 합니다.

프로젝트 이름을 마우스 오른쪽 단추로 위의 예에서 합니다 **솔루션 탐색기**를 선택 **추가** > **새 파일...**  호출 `AttractionTableDatasource` 을 클릭 합니다 **새로 만들기** 만들기 단추. 다음에 편집을 `AttractionTableDatasource.cs` 파일을 다음과 같이 표시 되도록 합니다.

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

세부 정보에서 클래스의 여러 부분에 대해를 살펴보겠습니다.

먼저 프로토타입 셀 (이 위의 인터페이스 디자이너에서 할당 된 동일한 식별자)의 고유 식별자를 포함 하는 상수를 정의 바로 가기를 테이블 뷰 컨트롤러를 다시 추가 하 고 데이터에 대 한 저장소를 생성 합니다.

```csharp
const string CellID = "AttrCell";
public AttractionTableViewController Controller { get; set;}
public List<CityInformation> Cities { get; set;}
```

다음으로,에서는 테이블 뷰 컨트롤러를 저장 한 다음를 빌드하고 채워야 (앞에서 정의한 데이터 모델을 사용 하 여) 데이터 원본 클래스의 인스턴스가 만들어질 때:

```csharp
public AttractionTableDatasource (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
    this.Cities = new List<CityInformation> ();
    PopulateCities ();
}
```

예로 `PopulateCities` 메서드 이러한 실제 앱에서 대시보드 또는 웹 서비스에서 쉽게 읽을 수 있지만 메모리에 데이터 모델 개체를 간단히 만듭니다.

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

`NumberOfSections` 메서드 테이블의 섹션의 수를 반환 합니다.

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    // Return number of cities
    return Cities.Count;
}
```

에 대 한 **일반** 테이블 뷰 스타일 항상 1을 반환 합니다.

`RowsInSection` 현재 섹션의 행 수를 반환 합니다.

```csharp
public override nint RowsInSection (UITableView tableView, nint section)
{
    // Return the number of attractions in the given city
    return Cities [(int)section].Attractions.Count;
}
```

다시 **일반** 테이블 뷰 데이터 소스의 항목의 총 수를 반환 합니다.

`TitleForHeader` 메서드 제목을 반환 하는 지정 된 섹션:

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    // Get the name of the current city
    return Cities [(int)section].Name;
}
```

에 대 한는 **Plain** 테이블 뷰 형식에 제목을 비워 둡니다 (`""`).

마지막으로 테이블 보기를 요청한 경우 만들고 사용 하 여 프로토타입 셀을 채우는 `GetCell` 메서드: 

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

작업에 대 한 자세한 내용은 `UITableViewDatasource`, Apple의를 참조 하세요 [UITableViewDatasource](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40006941) 설명서.

<a name="The-Table-View-Delegate" />

## <a name="the-table-view-delegate"></a>테이블 보기 대리자

대리자를 요구 하는 각 테이블 뷰 (`UITableViewDelegate`) 사용자 상호 작용 또는 기타 시스템 이벤트 테이블에 응답할 수 있습니다.

프로젝트 이름을 마우스 오른쪽 단추로 위의 예에서 합니다 **솔루션 탐색기**를 선택 **추가** > **새 파일...**  호출 `AttractionTableDelegate` 을 클릭 합니다 **새로 만들기** 만들기 단추. 다음에 편집을 `AttractionTableDelegate.cs` 파일을 다음과 같이 표시 되도록 합니다.

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

세부 정보에서이 클래스의 여러 섹션에 대해를 살펴보겠습니다.

먼저 클래스를 만들 때 테이블 뷰 컨트롤러의 바로 가기를 만듭니다.

```csharp
public AttractionTableViewController Controller { get; set;}
...

public AttractionTableDelegate (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
}
```

행을 선택 하는 경우에 한 (사용자의 Apple 원격 터치 화면에서 클릭) 표시 하려고 합니다 **인력** 즐겨찾기로 선택한 행으로 표시:

```csharp
public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
    attraction.IsFavorite = (!attraction.IsFavorite);

    // Update UI
    Controller.TableView.ReloadData ();
}
```

다음으로, (Apple 원격 터치 화면을 사용 하 여 포커스를 받으면)에서 행을 강조 표시할 때 원하는의 세부 정보를 제공 하는 **인력** 분할 뷰 컨트롤러의 해당 행의 세부 정보 섹션에 표시:

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

`CanFocusRow` 테이블 보기의 포커스를 가져오기에 있는 각 행에 대해 호출 됩니다. 반환 `true` 행에 포커스를 얻을 수, 하는 경우 다른 반환 `false`합니다. 사용자 지정이이 예제의 경우 만들었습니다 `AttractionHighlighted` 포커스를 받을 때 각 행에서 발생할 수 있는 이벤트입니다.

작업에 대 한 자세한 내용은 `UITableViewDelegate`, Apple의를 참조 하세요 [UITableViewDelegate](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40006942) 설명서.

<a name="The-Table-View-Cell" />

## <a name="the-table-view-cell"></a>테이블 뷰 셀

인터페이스 디자이너에서 테이블 보기에 추가한 각 프로토타입 셀에 대 한 스페이스도 테이블 뷰 셀의 사용자 지정 인스턴스 (`UITableViewCell`)을 만들 때 새 셀 (행)를 채울 수 있도록 합니다.

예제 앱을 두 번 클릭 합니다 `AttractionTableCell.cs` 파일 편집을 위해 열고 다음과 같이 표시 되도록 합니다.

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

이 클래스는 인력 데이터 모델 개체에 대 한 저장소를 제공 (`AttractionInformation` 위에 정의 된 대로)에 지정된 된 행 표시:

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

`UpdateUI` 메서드는 필요에 따라 UI 위젯 (에 추가한 인터페이스 디자이너에서 셀의 프로토타입)를 채웁니다.

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

작업에 대 한 자세한 내용은 `UITableViewCell`, Apple의를 참조 하세요 [UITableViewCell](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewCell_Class/index.html#//apple_ref/doc/uid/TP40006938) 설명서.

<a name="The-Table-View-Controller" />

## <a name="the-table-view-controller"></a>테이블 뷰 컨트롤러

테이블 뷰 컨트롤러 (`UITableViewController`) 인터페이스 디자이너를 통해 스토리 보드에 추가 된 테이블 보기를 관리 합니다.

예제 앱을 두 번 클릭 합니다 `AttractionTableViewController.cs` 파일 편집을 위해 열고 다음과 같이 표시 되도록 합니다.

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

이 클래스에 대해 좀 더 자세히 살펴보겠습니다. 먼저 테이블 보기의 액세스를 쉽게 수행할 수 있도록 바로 가기 키를 만들었습니다 `DataSource` 고 `TableDelegate`입니다. 사용 하 여 해당 나중 분할 뷰의 왼쪽에 있는 테이블 보기와 오른쪽의 세부 정보 뷰 간에 통신 합니다.

마지막으로, 테이블 뷰는 메모리에 로드 되 면을 만들겠습니다. 인스턴스를 `AttractionTableDatasource` 및 `AttractionTableDelegate` (위에서 만든 둘 다)를 테이블 뷰에 연결 합니다.

작업에 대 한 자세한 내용은 `UITableViewController`, Apple의를 참조 하세요 [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523) 설명서.

<a name="Pulling-it-All-Together" />

## <a name="pulling-it-all-together"></a>모든 함께 끌어올

이 문서의 시작 부분에서 설명 했 듯이 테이블 뷰는 일반적으로의 한쪽에 표시 됩니다는 [분할 뷰](~/ios/tvos/user-interface/split-views.md) 반대쪽에 표시 되는 선택한 항목의 세부 정보를 사용 하 여 탐색으로 합니다. 예를 들어: 

[![](table-views-images/intro01.png "샘플 앱 실행")](table-views-images/intro01.png#lightbox)

TvOS의 표준 패턴 이므로 모든 사용자에 게 결합 하는 마지막 단계에 살펴보겠습니다 및 분할 보기의 왼쪽과 오른쪽 서로 상호 작용 합니다.

<a name="The-Detail-View" />

### <a name="the-detail-view"></a>세부 정보 보기

예를 들어 여행 앱의 사용자 지정 클래스 위에 표시 (`AttractionViewController`) 분할 뷰로 세부 정보 보기의 오른쪽에 제공 된 표준 보기 컨트롤러에 대해 정의 됩니다.

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

여기에서 제공 합니다 **인력** (`AttractionInformation`) 속성으로 표시 되 고 생성을 `UpdateUI` UI 위젯 채우는 메서드가 인터페이스 디자이너에서 보기에 추가 합니다.

또한 바로 가기를 분할 뷰 컨트롤러를 다시 정의한 (`SplitView`)를 테이블 보기로 다시 변경를 사용 하 여 통신 (`AcctractionTableView`).

세 가지 사용자 지정 작업 (이벤트) 마지막으로 추가한 `UIButton` 인터페이스 디자이너에서 만든 경우 사용자는 인력으로 표시할 수 있는 _즐겨 찾는_, 가져오기 _지침_ 에 인력 및 _항공권 예약_ 지정 도시입니다.

<a name="The-Navigation-View-Controller" />

### <a name="the-navigation-view-controller"></a>탐색 뷰 컨트롤러

탐색 뷰 컨트롤러는 사용자 지정 클래스에 할당 된 테이블 뷰 컨트롤러는 분할 뷰의 왼쪽의 탐색 보기 컨트롤러에 중첩 되 면 때문에 (`MasterNavigationController`) 인터페이스 디자이너에서 다음과 같이 정의 합니다.

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

마찬가지로이 클래스에는 분할 뷰 컨트롤러의 양쪽을 통해 통신할 수 있도록 몇 가지 바로 가기만 정의 합니다.

* `SplitView` -분할 뷰 컨트롤러에 대 한 링크입니다 (`MainSpiltViewController`)에 속하는 탐색 보기 컨트롤러입니다.
* `TableController` -테이블 뷰 컨트롤러를 가져옵니다 (`AttractionTableViewController`)는 탐색 보기 컨트롤러의 상위 뷰로 표시 됩니다.

<a name="The-Split-View-Controller" />

### <a name="the-split-view-controller"></a>분할 뷰 컨트롤러

사용자 지정 클래스를 만든 분할 뷰 컨트롤러 응용 프로그램의 기본 이기 때문에 (`MasterSplitViewController`) 인터페이스 디자이너에서 다음과 같이 정의 및:

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

먼저 바로 가기를 만듭니다는 **세부 정보** 분할 뷰의 쪽 (`AttractionViewController`)와 **마스터** 쪽 (`MasterNavigationController`). 다시이 쉽게 나중으로 양쪽 간에 통신 합니다.

다음으로 분할 뷰는 메모리에 로드 되 면 분할 보기의 양쪽에 모두 분할 뷰 컨트롤러를 연결 하 고 테이블 보기에는 인력을 강조 표시 하는 사용자에 게 응답 (`AttractionHighlighted`)에서 새 인력을 표시 하 여는 **세부 정보**  분할 뷰의 측면입니다.

참조 하십시오 합니다 [tvTables](https://developer.xamarin.com/samples/monotouch/tvos/tvTable/) 분할 보기 내에서 테이블 뷰의 전체 구현에 대 한 샘플 앱입니다.

## <a name="table-views-in-detail"></a>표 보기에서 세부 정보

TvOS iOS를 기반으로, 이후 테이블 뷰 및 테이블 뷰 컨트롤러 설계 되었습니다 및 유사한 방식으로 작동 합니다. 자세한 내용을 보려면 테이블 뷰를 사용 하 여 Xamarin 앱에서 작업할 때, iOS를 참조 하세요 [테이블 및 셀 작업](~/ios/user-interface/controls/tables/index.md) 설명서.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 테이블 뷰를 사용 하 여 작업 검사가 수행 합니다. TvOS 앱에서 테이블 보기의 일반적인 사용 되는 분할 된 보기 내에서 테이블 보기를 사용 하 여 작업의 예제를 설명 했습니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
