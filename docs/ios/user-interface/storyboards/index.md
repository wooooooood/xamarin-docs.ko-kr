---
title: "스토리 보드에는 소개"
description: "스토리 보드에는 응용 프로그램의 흐름 및 모양의 시각적 표시는 합니다. Xamarin에는 디자이너 Xamarin.iOS 응용 프로그램이 응용 프로그램 화면을 시각적으로 디자인 하 고 보기에는 액세스할 수 있도록 스토리 보드를 활용할 수 있도록 도입 컨트롤러 더 많은 제어에 대 한 C#과 함께 segues 및 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: A3339BD2-9F56-7965-25F5-4B7C991EB775
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 342e8189d9dec6eaa60a999d56a7891da845d247
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="introduction-to-storyboards"></a>스토리 보드에는 소개

_스토리 보드에는 응용 프로그램의 흐름 및 모양의 시각적 표시는 합니다. Xamarin에는 디자이너 Xamarin.iOS 응용 프로그램이 응용 프로그램 화면을 시각적으로 디자인 하 고 보기에는 액세스할 수 있도록 스토리 보드를 활용할 수 있도록 도입 컨트롤러 더 많은 제어에 대 한 C#과 함께 segues 및 합니다._

이 가이드에서 어떤 스토리 보드 설명할 것 이며 – Segues 같은 주요 구성 요소 중 일부를 검사 합니다. 스토리 보드 수 생성 및을 사용 하는 방법을 살펴보겠습니다 하 고 개발자에 대 한 한 어떤 장점입니다.

스토리 보드 파일 형식 Apple에서 iOS 응용 프로그램의 UI의 시각적 표현으로 도입 되기 전에 개발자가 각 보기 컨트롤러에 대 한 XIB 파일을 만들고 각 보기 간의 탐색을 직접 프로그래밍할 합니다.  스토리 보드를 사용 하 여 컨트롤러 보기와 디자인 화면에 간의 탐색을 모두 정의 하는 개발자와 응용 프로그램의 사용자 인터페이스의 WYSIWYG 편집을 제공 합니다.

스토리 보드를 작성, 열 및 Xamarin iOS 디자이너를 사용 하 여 편집할 합니다. 이 가이드는 또한 연습 스토리 보드를 사용 하 여 C# 프로그래밍 탐색 하는 동안 만들려면 디자이너를 사용 하는 방법입니다.


## <a name="requirements"></a>요구 사항

스토리 보드 설치 Xamarin 워크 로드와 디자이너 Mac 용 Visual Studio에서 iOS 또는 Visual Studio 2015 및 2017 사용할 수 있습니다.

## <a name="what-is-a-storyboard"></a>스토리 보드는 무엇입니까?

스토리 보드는 응용 프로그램에서 모든 화면의 시각적 표현입니다. 백그라운드에서 각 화면 표시로 시퀀스를 포함 한 *뷰-컨트롤러* 및 해당 *뷰*합니다. 개체는 이러한 뷰에 포함 될 수 있습니다 및 [컨트롤](~/ios/user-interface/controls/index.md) 를 사용자가 직접 응용 프로그램 상호 작용 하도록 허용 됩니다. 이 컬렉션 보기와 컨트롤의 (또는 *하위*) 라고는 *콘텐츠 계층 구조 보기*합니다. 장면 연결 되어 여 segue 개체를 컨트롤러 보기 간 전환을 나타냅니다. 이 일반적으로 초기 보기에 있는 개체와 연결 보기 간에 segue 만들어 달성 됩니다. 디자인 화면에서 관계는 아래 이미지에서 설명 합니다.

 [![](images/storyboardsview.png "이 이미지에 디자인 화면에서 관계는 설명")](images/storyboardsview.png#lightbox)

표시 된 대로 스토리 보드 이미 렌더링 콘텐츠로 각 프로그램 장면에 레이아웃 됩니다 및 둘 간의 연결을 보여 줍니다.  IPhone에서 장면에 대해 논의할 때 안전 하다는 하나 생각 하는 것이 시점에서 체제 *장면* 스토리 보드에는 1과 같은 *화면* 장치에서 콘텐츠입니다. 그러나 여러 장면이 – 한 번에 표시할 지 있을 수는 iPad와 예를 들어 컨트롤러를 사용 하 Popover 보기.

Xamarin을 사용 하는 경우에 특히 응용 프로그램의 UI를 만드는 스토리 보드를 사용 하 여 많은 장점이 있습니다. 모든 개체 – 포함으로 UI의 시각적 표현 첫째, [사용자 지정 컨트롤](~/ios/user-interface/designer/ios-designable-controls-overview.md) – 디자인 타임에 렌더링 됩니다. 따라서 작성 하거나 해당 모양 및 흐름을 시각화할 수 응용 프로그램을 배포 하기 전에. 위의 이미지를 예로 들어 보겠습니다. 디자인 화면의 개수 장면 있습니다 모두, 각 보기의 레이아웃 및 모든 관계를 빠르게 확인에서 찾아 알 수 있습니다. 스토리 보드 훨씬 편리 하기 때문입니다.

이벤트는 iOS 디자이너를 사용 하는 경우에 특히 스토리 보드와 편리 합니다. 대부분의 UI 컨트롤 속성 패드에서 가능한 이벤트 목록을 갖습니다. 이벤트 처리기를 여기에 추가 하 고 뷰 컨트롤러 클래스에 부분 메서드에에서 완료 수 있습니다...

스토리 보드의 콘텐츠를 XML 파일로 저장 됩니다. 빌드 타임에 모든 `.storyboard` 파일이 nibs 라고 하는 이진 파일로 컴파일됩니다. 런타임 시 이러한 nibs 초기화 되 고 새 뷰를 인스턴스화됩니다.

## <a name="segues"></a>Segues

A *Segue*, 또는 *Segue 개체*, 장면 전환을 나타내기 위해 iOS 개발에 사용 됩니다. segue 만들려는 키를 누른 채는 **Ctrl** 키와 클릭 한 후 드래그 한 장면에서 다른 합니다. 마우스를 끌어 우리는 segue 아래 이미지에서와 같이 될 위치를 나타내는 파란색 커넥터가 나타납니다.

 [![](images/createsegue.png "이 이미지에서와 같이 segue 될 위치를 나타내는 파란색 커넥터가 나타납니다.")](images/createsegue.png#lightbox)

마우스 놓기에 우리의 segue에 대 한 작업을 선택할 수는 메뉴가 표시 됩니다. 아래 이미지와 유사한 나타낼 수 있습니다. 

**사전 iOS 8 및 크기 클래스**:

[![](images/segue1.png "크기 클래스 없이 작업 Segue 드롭다운")](images/segue1.png#lightbox)

**크기 클래스와 적응 Segues를 사용 하는 경우**:

[![](images/16new.png "크기 클래스와 함께 작업 Segue 드롭다운")](images/16new.png#lightbox)

> [!IMPORTANT]
> **참고:** Ctrl + 클릭으로 매핑되어 VMWare Windows 가상 컴퓨터에 대 한 사용 중인 경우는 _마우스 오른쪽 단추로 클릭_ 기본적으로 마우스 단추입니다. 편집을 통해 키보드 기본 설정에는 Segue를 만들려면 **기본 설정** > **키보드 및 마우스** > **마우스 바로 가기** 다시 매핑 및 사용자 **보조 단추** 아래 그림과 같이:
> 
> [![](images/image22.png "키보드 및 마우스 기본 설정")](images/image22.png#lightbox)
> 
> 이제 일반적인 방법으로 뷰 컨트롤러 간의 segue를 추가할 수 있습니다.

다양 한 유형의 전환, 각 주어진 제어할 스토리 보드에 있는 다른 뷰 컨트롤러와 상호 작용 하는 방법 및 새 뷰 컨트롤러 사용자에 게 표시 되는 방식이 있습니다. 이러한 옵션은 아래 설명 되어 있습니다. 하위 클래스는 사용자 지정 전환을 구현 하는 segue 개체 수 이기도 합니다.

-  **푸시 / 표시** – 밀어넣기 segue 뷰 컨트롤러 탐색 스택에 추가 합니다. 푸시를 시작 하는 보기 컨트롤러 스택에 추가 되는 뷰 컨트롤러와 동일한 탐색 컨트롤러의 일부인 가정 합니다. 와 같은 작업을 수행이 `pushViewController` , 화면에서 데이터 간의 관계가 일부 경우에 일반적으로 사용 됩니다. 푸시를 사용 하 여 segue luxury 뒤로 단추와 제목이 드릴 다운 계층 보기를 탐색할 수 있도록 스택의 각 보기에 추가 된 탐색 모음을 제공 합니다.
-  **모달** –는 모달 segue 표시 되는 애니메이션된 전환 옵션의 프로젝트에는 두 개의 뷰 컨트롤러 간의 관계를 만듭니다. 자식 뷰 컨트롤러 보기에 맞게 수정할 때 부모 뷰 컨트롤러를 완전히 숨기 됩니다. 푸시와 달리 segue, 우리; 뒤로 단추 추가 때 segue 모달을 사용 하 여 `DismissViewController` 이전 뷰 컨트롤러를 반환 하기 위해 사용 해야 합니다.
-  **사용자 지정** -모든 사용자 지정의 서브 클래스를 만들 수 있습니다 segue ` UIStoryboardSegue`합니다.
-  **해제** – 해제는 segue 밀어넣기 또는 모달 다시 탐색 하는 데 사용할 segue-예를 들어 뷰 모달 형식으로 제공 된 컨트롤러를 해제 하 여 합니다. 이외에 뿐만 아니라 하나를 통해 해제할 수 있습니다 하지만 일련의 푸시 및 모달 segues 하 고 단일 탐색 계층의 여러 단계 작업을 해제 돌아갑니다. 읽기는 ios에서 segue 해제를 사용 하는 방법을 이해 하는 [Segues 해제 만드는](https://developer.xamarin.com/recipes/ios/general/storyboard/unwind_segue/) 조리법 합니다.
-  **Sourceless** – sourceless segue 나타냅니다 장면 초기 뷰 컨트롤러를 포함 하 고 따라서 먼저 볼 수 있는 사용자를 볼 합니다. 아래에 표시 된 segue로 표현 됩니다.  

    [![](images/sourcelesssegue.png "Sourceless segue")](images/sourcelesssegue.png#lightbox)

### <a name="adaptive-segue-types"></a>적응 Segue 형식

 iOS 8 도입 [크기 클래스](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) 개발자가 모든 iOS 장치에 대 한 하나의 UI를 만들 수 있도록 하는 모든 사용 가능한 화면 크기에 맞게 iOS 스토리 보드 파일 수 있도록 합니다. 기본적으로 모든 새 Xamarin.iOS 응용 프로그램 크기 클래스를 사용 합니다. 이전 프로젝트에서 크기 클래스를 사용 하려면 참조는 [스토리 보드 통합 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 가이드입니다. 
 
크기 클래스를 사용 하 여 모든 응용 프로그램에서 새 사용할도 [ *적응 Segues*](~/ios/user-interface/storyboards/unified-storyboards.md)합니다. 크기 클래스를 사용 하면 직접 사용할지 지정 되지 म 기억 iPhone 또는 iPad를 사용 합니다. 즉 하나의 UI는 항상 동일 하 게 보이지만 얼마나 부동산 작업 하는 것에 관계 없이 만듭니다. 환경을 판단 하 고 콘텐츠를 제공 하 최상의 방법을 결정 하 여 적응 Segues 작동 합니다. 적응 Segues 다음과 같습니다. 

[![](images/adaptivesegue.png "적응 Segues 드롭다운")](images/adaptivesegue.png#lightbox)

<table>
    <thead>
        <tr>
            <th>Segue</th>
            <th>설명</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>표시</td>
            <td>이 매우 비슷합니다 밀어넣기 segue 하지만 화면 콘텐츠 반영 됩니다. </td>
        </tr>
        <tr>
            <td>세부 정보를 표시 합니다.</td>
            <td>응용 프로그램 (예를 들어 iPAd에서 분할 뷰 컨트롤러)에 마스터 및 세부 정보 보기를 표시 하는 경우 콘텐츠 세부 정보 보기를 대체 합니다. ऍ प 마스터만 <strong>또는</strong> 세부 콘텐츠 보기 컨트롤러 스택 맨 바뀝니다.</td>
        </tr>
        <tr>
            <td>프레젠테이션</td>
            <td>이 모달 segue 비슷합니다 고 프레젠테이션 및 전환 스타일 허용 합니다.</td>
        </tr>
        <tr>
            <td>Popover 프레젠테이션</td>
            <td>이 경우 콘텐츠는 popover로 발생</td>
        </tr>
    </tbody>
</table>

### <a name="transferring-data-with-segues"></a>Segues 사용 하 여 데이터를 전송 합니다.

segue 활용 하지 않는 전환에서 끝나지 않으며, 또한 보기 컨트롤러 간의 데이터 전송을 관리 하려면 사용 수 있습니다. 재정의 하 여 이렇게는 `PrepareForSegue` 메서드를 처음 보기 컨트롤러와 직접 데이터를 처리 합니다. segue – 트리거되면 예를 들어 단추 누름 –와 응용 프로그램은이 메서드를 호출 새 뷰 컨트롤러를 준비 하는 기회를 제공 하 *전에* 모든 탐색이 발생 합니다. 아래 코드에서는 [Phoneword](https://developer.xamarin.com/samples/monotouch/Hello_iOS/) 샘플,이 보여 줍니다. 


```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, 
NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    var callHistoryContoller = segue.DestinationViewController 
                                  as CallHistoryController;

    if (callHistoryContoller != null) {
        callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
}
```

이 예제는 `PrepareForSegue` 는 segue 사용자에 의해 트리거될 때 메서드가 호출 됩니다. 먼저 '수신' 보기 컨트롤러의 인스턴스를 만들고이 뷰-컨트롤러 segue의 대상으로 설정 해야 합니다. 이 작업은 아래 코드의 줄에 의해 수행 됩니다.

```csharp
var callHistoryContoller = segue.DestinationViewController as CallHistoryController;
```

메서드는 이제에 속성을 설정할 수 있습니다는 `DestinationViewController`합니다. 이 예제에서는 이용한이 호출 목록을 전달 하 여 `PhoneNumbers` 에 `CallHistoryController` 같은 이름의 개체에 할당:

```csharp
if (callHistoryContoller != null) {
        callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
```

전환을 완료 되 면 보기가 표시 됩니다는 `CallHistoryController` 채워진된 목록을 사용 합니다.

## <a name="adding-a-storyboard-to-a-non-storyboard-project"></a>스토리 보드를 스토리 보드 아닌 프로젝트에 추가

경우에 따라 스토리 보드를 이전에 비-스토리 보드 파일에 추가 해야 합니다. 한 번 Mac 용 Visual Studio에서이 수행 하는 다음 단계를 수행 하 여 간소화 수 있습니다.:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 탐색 하 여 새 스토리 보드 파일을 만들 **파일 > 새 파일 > iOS > 스토리 보드**아래 그림과 같이,: 
    
    [![](images/new-storyboard-xs.png "새 파일 대화 상자")](images/new-storyboard-xs.png#lightbox)

2. 에 사용자 스토리 보드의 이름을 추가 **주 인터페이스** 의 섹션은 **Info.plist**다음과 같이:
    
    [![](images/infoplist.png "Info.plist 편집기")](images/infoplist.png#lightbox)
    
    초기 보기 컨트롤러를 인스턴스화하는 것에 해당 않습니다는 `FinishedLaunching` 내 앱 대리자에서 메서드. 이 옵션을 설정 응용 프로그램 창 (아래 참조), 주 스토리 보드 로드 인스턴스화하고으로 스토리 보드의 초기 보기 컨트롤러 (sourceless Segue 옆에 있는 하나)의 인스턴스를 할당 된 `RootViewController` 창 찾아 다음 설정의 속성 화면에 표시 하는 창입니다.

3. 에 `AppDelegate`, 기본값을 재정의할 `Window` 창의 속성을 구현 하는 다음 코드로 메서드:
        
        public override UIWindow Window {
            get;
            set;
            }
            
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 프로젝트를 마우스 오른쪽 단추로 클릭 하 여 새 스토리 보드 파일을 만들 **추가 > 새 파일 > iOS > 빈 스토리 보드**아래 그림과 같이,: 
    
    [![](images/new-storyboard-vs.png "새 항목 대화 상자")](images/new-storyboard-vs.png#lightbox)

2. 에 사용자 스토리 보드의 이름을 추가 **주 인터페이스** iOS 응용 프로그램을 다음과 같이의 섹션:
    
    [![](images/ios-app.png "Info.plist 편집기")](images/ios-app.png#lightbox)
    
    초기 보기 컨트롤러를 인스턴스화하는 것에 해당 않습니다는 `FinishedLaunching` 내 앱 대리자에서 메서드. 이 옵션을 설정 응용 프로그램 창 (아래 참조), 주 스토리 보드 로드 인스턴스화하고으로 스토리 보드의 초기 보기 컨트롤러 (sourceless Segue 옆에 있는 하나)의 인스턴스를 할당 된 `RootViewController` 창 찾아 다음 설정의 속성 화면에 표시 하는 창입니다.

3. 에 `AppDelegate`, 기본값을 재정의할 `Window` 창의 속성을 구현 하는 다음 코드로 메서드:

        public override UIWindow Window {
            get;
            set;
            }
            
-----

## <a name="creating-a-storyboard-with-the-ios-designer"></a>IOS 디자이너를 사용 하 여 스토리 보드 만들기

Xamarin 디자이너를 사용 하 여 iOS, Mac 및 Visual Studio 용 Visual Studio와 함께 원활 하 게 통합에 대 한 스토리 보드를 만들 수 있습니다.

IOS 디자이너를 사용 하 여 스토리 보드 만들기 시작 하려면 따라는 [Hello, iOS 멀티 스크린](~/ios/get-started/hello-ios-multiscreen/index.md) 가이드입니다. 이 연습에서는 컨트롤러 Segues, 및 컨트롤에서 이벤트를 처리 하는 방법을 사용 하 여 보기 간의 탐색을 탐색 합니다.

## <a name="instantiate-storyboards-manually"></a>스토리 보드를 수동으로 인스턴스화합니다

사용 하 여 스토리 보드의 컨트롤러 개별 보기 인스턴스화할 여전히 수 있지만 스토리 보드 프로젝트의 개별 XIB 파일을 완전히 대체 `Storyboard.InstantiateViewController`합니다.

응용 프로그램 디자이너에서 제공 하는 기본 제공 스토리 보드 전환과 처리할 수 없는 특별 한 요구 사항이 해야 하는 경우가 있습니다. 예를 들어 응용 프로그램의 현재 상태에 따라 표시 되는 동일한 단추에서 서로 다른 화면을 실행 하는 응용 프로그램을 만들려는 अ स म र 컨트롤러 보기를 수동으로 인스턴스화하고 전환 직접 프로그래밍할 되기를 원하는 수 있습니다.

아래 스크린샷에서 디자인 화면에서 두 명의 뷰 컨트롤러 없이 서로 segue 보여 줍니다. 다음 섹션에서는에 코드를 해당 전환에 설정할 수는 방법을 안내 합니다.

 [![](images/viewcontrollerspink.png "이 스크린샷은 서로 없이 디자인 화면에 두 명의 뷰 컨트롤러 segue 보여 줍니다.")](images/viewcontrollerspink.png#lightbox)

1. 추가 _iPhone 스토리 보드를 빈_ 를 기존 프로젝트 프로젝트:
    
    [![](images/add-storyboard1.png "스토리 보드를 추가합니다.")](images/add-storyboard1.png#lightbox)

2. 대시보드를 열 새로 만든된 스토리 보드에서 두 번 클릭 하 고 새 추가 **탐색 컨트롤러** 디자인 화면으로 합니다. 탐색 컨트롤러는 아래 그림과 같이 루트 뷰 컨트롤러와 나올지 기본적으로 UI가 아닌:

    [![](images/uinavigationcontroller.png "포함 된 컨트롤러 Segues 보기")](images/uinavigationcontroller.png#lightbox)

3. 선택 된 _뷰-컨트롤러_ 아래쪽의 검은색 표시줄을 클릭 하 여 합니다. 디자이너에서 **속성 패드**아래 **Identity** 보기 컨트롤러에 대 한 고유 ID 뿐만 아니라 사용자 지정 클래스를 지정할 수 있습니다. 설정의 **클래스 이름** 및 **스토리 보드 ID** 를 `MainViewController`합니다.

    [![](images/identitypanelnew.png "사용자 지정 클래스를 지정 합니다.")](images/identitypanelnew.png#lightbox)

4. 인스턴스화하는 스토리 보드에서 컨트롤러 우리의 보기 위해 필요한 나중 코드에서 참조 하는 스토리 보드 ID를 사용 합니다. 상태를 복원 해야 하는 경우 뷰 컨트롤러 가져옵니다 다시 올바르게 생성 하도록 스토리 보드 ID와 일치 하도록 복원 ID를 설정 합니다.

5. 에서는 현재 컨트롤러가 하나만 포함 보기. 다른 뷰-컨트롤러를 디자인 화면으로 끕니다. 에 **속성 패드**, 클래스 및 스토리 보드 ID를 설정 id로 `PinkViewController`아래 그림과 같이,:

    [![](images/pinkvcnew.png "속성 채움")](images/pinkvcnew.png#lightbox)
    
    IDE에서는 컨트롤러 보기에 대 한 이러한 사용자 지정 클래스를 만듭니다. 볼 수 있습니다는 **솔루션 패드**아래 스크린샷에 표시 된 것 처럼,:
    
    [![](images/solution-pad.png "솔루션 채움")](images/solution-pad.png#lightbox)

6. 에 `PinkViewController`를 컨트롤러의 프레임의 중심 쪽으로 클릭 하 여 보기를 선택 합니다. 속성 패드 보기에서 변경 된 **배경** 자홍에:
    
    [![](images/pinkcontroller.png "배경색 설정")](images/pinkcontroller.png#lightbox)

7. 마지막으로,에서 단추를 끌어는 **도구 상자** 에 `MainViewController`합니다. 속성 패드에 이름을 지정 `PinkButton` 및 아래 그림과 같이 제목 GoToPink:

    [![](images/pinkbutton.png "단추 이름 설정")](images/pinkbutton.png#lightbox)

스토리 보드 완료 되었지만 빈 화면 프로젝트를 지금 배포 하는 경우 발생 합니다. 이 스토리 보드를 사용 하 고 첫 번째 보기 역할을 하는 루트 보기 컨트롤러를 설정 하도록 IDE에 지시 해야 때문입니다. 일반적으로 위에 표시 된 대로 우리의 프로젝트 옵션 통해이 수행할 수 있습니다. 하지만 다음을 추가 하 여 코드에서 동일한 결과 달성이 예에서 예정 된 **AppDelegate**:

```csharp
public partial class AppDelegate : UIApplicationDelegate
    {
        UIWindow window;
        public static UIStoryboard Storyboard = UIStoryboard.FromName ("MainStoryboard", null);
        public static UIViewController initialViewController;

        public override bool FinishedLaunching (UIApplication app, NSDictionary options)
        {
            window = new UIWindow (UIScreen.MainScreen.Bounds);

            initialViewController = Storyboard.InstantiateInitialViewController () as UIViewController;

            window.RootViewController = initialViewController;
            window.MakeKeyAndVisible ();
            return true;
        }

    }
```

많은 코드를 있는 않으며 단 몇 줄 익숙하지입니다. 이 스토리 보드와 등록 먼저는 **AppDelegate** 스토리 보드의 이름 전달 하 여 **MainStoryboard**합니다. 다음으로 응용 프로그램을 호출 하 여 스토리 보드의 초기 보기 컨트롤러를 인스턴스화하 알립니다 `InstantiateInitialViewController` 우리의 스토리 보드의 보기 컨트롤러 해당 응용 프로그램의 루트 뷰-컨트롤러를 설정 했습니다. 이 메서드는 사용자에 게 표시 되는 첫 번째 화면의 확인 하 고 해당 뷰-컨트롤러의 새 인스턴스를 만듭니다.

IDE에서 만들어진 솔루션 창에서는 `MainViewcontroller.cs` 클래스 및 해당 `corresponding designer.cs` 4 단계에서 속성 패드에 클래스 이름이 추가 했습니다. 이 클래스는 기본 클래스를 포함 하는 특수 한 생성자를 만든 볼 수 있습니다.

```csharp
public MainViewController (IntPtr handle) : base (handle) 
{
}
```


IDE의 디자이너를 사용 하 여 스토리 보드를 만들 때 자동으로 추가 됩니다는 [[등록]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) 맨 위에 있는 특성은 `designer.cs` 클래스에 지정 된 스토리 보드 ID는 문자열 식별자를 전달 하 고는 이전 단계입니다. 이 연결 됩니다 C# 관련 장면 스토리 보드에는.

하 던 기존 클래스를 추가 하려는 어느 시점 **하지** 디자이너에서 만든 합니다. 이 경우 일반적인 방법으로이 클래스를 등록 합니다.

```csharp
[Register ("MainViewController")]
public partial class MainViewController : UIViewController
{
public MainViewController (IntPtr handle) : base (handle) 
{
}

...
}
```

클래스 및 메서드를 등록 방법에 대 한 자세한 내용은 참조는 [형식 등록 기관](http://docs.xamarin.com/guides/ios/advanced_topics/registrar/) 설명서입니다.

이 클래스의 마지막 단계에서는 단추와 분홍색 뷰 컨트롤러로의 전환을를 연결 합니다. 인스턴스화할 수 우리는 `PinkViewController` ; 스토리 보드에서 우리는 프로그램을 작성 푸시를와 segue `PushViewController`아래 예제에서는 코드 예와 같이:

```csharp
public partial class MainViewController : UIViewController
{
    UIViewController pinkViewController;

    public MainViewController (IntPtr handle) : base (handle)
    {

    }

    public override void AwakeFromNib ()
    {
    // Called when loaded from xib or storyboard.

    this.Initialize ();
    }

    public void Initialize(){

    //Instantiating View Controller with Storyboard ID 'PinkViewController'
    pinkViewController = Storyboard.InstantiateViewController ("PinkViewController") as PinkViewController;
    }

    public override void ViewDidLoad ()
    {
    base.ViewDidLoad ();

    //When we push the button, we will push the pinkViewController onto our current Navigation Stack
    PinkButton.TouchUpInside += (o, e) =&gt; {
        this.NavigationController.PushViewController (pinkViewController, true);
    };
    }

}
```

응용 프로그램을 실행 하는 2 화면 응용 프로그램을 생성 합니다.

![](images/finishedstoryboard.png "화면을 실행 하는 샘플 응용 프로그램")

## <a name="conditional-segues"></a>조건부 Segues

종종 하나 뷰-컨트롤러에서 다음 이동 되는 특정 조건에 따라 달라 집니다. 예를 들어 간단한 로그인 화면을 만들려는 된 경우 것만 포함 하려고 다음 화면으로 이동 하려면 *경우* 사용자 이름 및 암호를 확인 했습니다.

다음 예제에서에서는 위의 샘플에 암호 필드를 추가 합니다. 사용자는 액세스할 수는 *PinkViewController* 올바른 암호를 입력 하지 않으면 오류가 표시 됩니다.

시작 하기 전에 1-8 위의 단계를 따릅니다. 이 단계에서는 스토리 보드 만들기에서는,이을 우리의 UI 만들 보기 컨트롤러를 사용 하는 응용 프로그램 대리자를 알 RootViewController 있기 때문입니다.

1. 이제 보겠습니다 UI를 빌드하고에 나열 된 다른 보기를 추가할는 `MainViewController` 그렇게 아래 스크린샷에 표시:

    - UITextField
        - 이름: PasswordTextField
        - 자리 표시자: ' 보안 암호 입력 '
    - UILabel
        - 텍스트: ' 오류: 암호를 잘못 되었습니다. 들어갈 수 없다!'
        - 색: 빨간색
        - 맞춤: 센터
        - 줄: 2
        - 'Hidden' checkbox 선택 
        
    [![](images/passwordvc.png "Center 줄")](images/passwordvc.png#lightbox)
    
2. 분홍색 이동 단추와에서 Ctrl 끌어 뷰 컨트롤러 사이의 Segue 만들기는 *PinkButton* 에 *PinkViewController*를 선택 하 고 **푸시** 마우스 놓기에 . 

3. Segue에서을 클릭 하 고는 *식별자* `SegueToPink`:

    [![](images/namesegue.png "Segue 클릭 하 고 식별자 SegueToPink 제공")](images/namesegue.png#lightbox)  
    

4. 마지막으로 다음 ShouldPerformSegue 메서드를 추가 `MainViewController` 클래스:

    ```csharp
    public override bool ShouldPerformSegue (string segueIdentifier, NSObject sender)
    {
        
        if(segueIdentifier == "SegueToPink"){
            if (PasswordTextField.Text == "password") {
                PasswordTextField.ResignFirstResponder ();
                return true;
            }
            else{
                ErrorLabel.Hidden = false;
                return false;
            }
        }
        return base.ShouldPerformSegue (segueIdentifier, sender);
    }
    ```

이 코드를 segueIdentifier 일치할 것 우리의 `SegueToPink` segue 조건; 다음 테스트할 수 있습니다이 경우 올바른 암호입니다. 이 조건에서 반환 하는 경우 `true`는 Segue 수행 합니다 및 제시 합니다는 `PinkViewController`합니다. 경우 `false`, 새 뷰 컨트롤러 표시 되지 것입니다.

SegueIdentifier 인수 ShouldPerformSegue 메서드를 확인 하 여이 방법을 사용이 뷰-컨트롤러에서 모든 Segue를 적용할 수도 있습니다. 이 경우에 있습니다 하나 이상의 Segue 식별자 – `SegueToPink`합니다.

Storyboards.Conditional 솔루션에 대 한 참조는 [수동 스토리 보드 예제](https://developer.xamarin.com/samples/monotouch/ManualStoryboard/) 작업 예제입니다.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>스토리 보드 참조 사용

따라서 제거 복잡성을 제거 하 고 그 결과 개별 만드는 Storyboard 쉽게 디자인, 스토리 보드 참조를 사용 하면 대규모의 복잡 한 스토리 보드 디자인 하 고 원래에서 참조 가져오기는 더 작은 스토리 보드를 중단 하 고 유지 관리 합니다.

또한 스토리 보드 참조를 제공할 수는 _앵커_ 동일한 스토리 보드 또는 다른 특정 장면 내에서 다른 장면에 있습니다.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>외부 스토리 보드를 참조합니다.

외부 스토리 보드에 대 한 참조를 추가 하려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...**   >  **iOS** > **스토리 보드**합니다. 입력 한 **이름** 새 스토리 보드 및 클릭에 대 한는 **새로** 단추:
    
    [![](images/ref01.png "새 파일 대화 상자")](images/ref01.png#lightbox)
    
2. 일반적으로 하는 변경 내용을 저장 하는 대로 새 스토리 보드의 장면의 레이아웃을 디자인 합니다. 
    
    [![](images/ref02.png "새 화면 레이아웃")](images/ref02.png#lightbox)
    
3. 스토리 보드 디자이너 iOS에 대 한 참조를 추가 하는 것을 엽니다.

4. 끌어서는 **참조 스토리 보드** 에서 **도구 상자** 디자인 화면으로: 
    
    [![](images/ref03.png "스토리 보드 참조")](images/ref03.png#lightbox)
    
5. 에 **위젯** 탭은 **속성 탐색기**의 이름을 선택 된 **스토리 보드** 위에서 만든: 

    [![](images/ref04.png "위젯 탭")](images/ref04.png#lightbox)
    
6. 기존 장면에 (예: 단추) UI 위젯에 컨트롤을 클릭 하 고에 새 Segue 만들기는 **스토리 보드 참조** 방금 만든: 

    [![](images/ref05.png "segue 만들기")](images/ref05.png#lightbox) 
    
7. 팝업 메뉴에서 선택 **표시** 는 Segue를 완료 하려면: 

    [![](images/ref06.png "Segue 완료 하는 보기를 선택 하")](images/ref06.png#lightbox) 
    
8. 스토리 보드에 변경 내용을 저장 합니다.

앱이 실행 되 고는 Segue 스토리 보드 파일에 대 한 참조에 지정 된 외부 스토리 보드의 초기 보기 컨트롤러에서 만든 UI 요소에는 사용자가 클릭할 때 표시 됩니다.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>외부 스토리 보드의 특정 장면이 참조

특정 장면에 대 한 참조를 추가 하는 외부 스토리 보드 (및 초기 뷰 컨트롤러) 다음을 수행 합니다.

1. 에 **솔루션 탐색기**를 편집 하기 위해 열려는 외부 스토리 보드를 두 번 클릭 합니다.

2. 새 장면을 추가 하 고 일반적인 방법으로 해당 레이아웃을 디자인 합니다. 

    [![](images/ref07.png "새 화면 레이아웃")](images/ref07.png#lightbox)
    
3. 에 **위젯** 탭은 **속성 탐색기**를 입력 한 **스토리 보드 ID** 새 장면이 뷰-컨트롤러에 대 한: 

    [![](images/ref08.png "새 화면 보기 컨트롤러에 대 한 스토리 보드 ID를 입력 합니다.")](images/ref08.png#lightbox)
    
3. 스토리 보드 디자이너 iOS에 대 한 참조를 추가 하는 것을 엽니다.

4. 끌어서는 **참조 스토리 보드** 에서 **도구 상자** 디자인 화면으로: 

    [![](images/ref03.png "스토리 보드 참조")](images/ref03.png#lightbox)
    
5. 에 **위젯** 탭은 **속성 탐색기**의 이름을 선택는 **스토리 보드** 및 **참조 ID** (스토리 보드 ID)의 위에서 만든 장면: 

    [![](images/ref09.png "위젯 탭 ")](images/ref09.png#lightbox)
    
6. 기존 장면에 (예: 단추) UI 위젯에 컨트롤을 클릭 하 고에 새 Segue 만들기는 **스토리 보드 참조** 방금 만든: 

    [![](images/ref10.png "segue 만들기")](images/ref10.png#lightbox) 
    
7. 팝업 메뉴에서 선택 **표시** 는 Segue를 완료 하려면: 

    [![](images/ref06.png "Segue 완료 하는 보기를 선택 하")](images/ref06.png#lightbox) 
    
8. 스토리 보드에 변경 내용을 저장 합니다.

때 응용 프로그램 실행 및 사용자가는 Segue와 장면에서 만든 UI 요소에는 주어진 **스토리 보드 ID** 스토리 보드 파일에 대 한 참조에 지정 된 외부 스토리 보드에서 표시 됩니다.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>동일한 스토리 보드의 특정 장면이 참조

특정 장면 동일한 스토리 보드에 대 한 참조를 추가 하려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**를 편집 하기 위해 열려는 스토리 보드를 두 번 클릭 합니다.

2. 새 장면을 추가 하 고 일반적인 방법으로 해당 레이아웃을 디자인 합니다. 

    [![](images/ref11.png "새 화면 레이아웃")](images/ref11.png#lightbox)

3. 에 **위젯** 탭은 **속성 탐색기**를 입력 한 **스토리 보드 ID** 새 장면이 뷰-컨트롤러에 대 한: 

    [![](images/ref12.png "위젯 탭")](images/ref12.png#lightbox)
    
3. 끌어서는 **참조 스토리 보드** 에서 **도구 상자** 디자인 화면으로: 

    [![](images/ref03.png "스토리 보드 참조")](images/ref03.png#lightbox)
    
5. 에 **위젯** 탭은 **속성 탐색기**선택, **참조 ID** (스토리 보드 ID) 위에서 만든 장면의: 

    [![](images/ref13.png "위젯 탭")](images/ref13.png#lightbox)
    
6. 기존 장면에 (예: 단추) UI 위젯에 컨트롤을 클릭 하 고에 새 Segue 만들기는 **스토리 보드 참조** 방금 만든: 

    [![](images/ref14.png "segue 만들기")](images/ref14.png#lightbox) 
    
7. 팝업 메뉴에서 선택 **표시** 는 Segue를 완료 하려면: 

    [![](images/ref06.png "Segue 완료 하는 보기를 선택 하")](images/ref06.png#lightbox) 
    
8. 스토리 보드에 변경 내용을 저장 합니다.

때 응용 프로그램 실행 및 사용자가는 Segue와 장면에서 만든 UI 요소에는 주어진 **스토리 보드 ID** 스토리 보드 파일에 대 한 참조에 지정 된 동일한 스토리 보드에 표시 됩니다.

## <a name="summary"></a>요약

이 문서는 스토리 보드와 될 수 있는 방법을 iOS 응용 프로그램을 개발 하는 데에서 유용의 개념을 소개 합니다. 장면, 컨트롤러 보기, 보기 및 보기 계층 구조 및 장면 다양 한 유형의 Segues 함께 연결 되는 방법을 설명 합니다.  또한 살펴봅니다 인스턴스화하는 동안 컨트롤러를 보기 수동으로 만드는 조건부 Segues 스토리 보드를 합니다.



## <a name="related-links"></a>관련 링크

- [수동 스토리 보드 (샘플)](https://developer.xamarin.com/samples/ManualStoryboard/)
- [IOS 디자이너 소개](~/ios/user-interface/designer/introduction.md)
- [스토리 보드에 변환](http://developer.apple.com/library/ios/#releasenotes/Miscellaneous/RN-AdoptingStoryboards/)
- [UIStoryboard 클래스 참조](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UIStoryboard_Class/Reference/Reference.html)
