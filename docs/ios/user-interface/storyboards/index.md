---
title: Xamarin.iOS에서 스토리 보드 소개
description: 이 문서에서는 Xamarin.iOS에서 스토리 보드를 소개 합니다. Segue를 storyboard를 사용 하는 사용자 인터페이스를 정의 하는 방법을 설명 하 고 iOS 디자이너를 사용 하 여 스토리 보드 파일을 편집 하는 방법입니다.
ms.prod: xamarin
ms.assetid: A3339BD2-9F56-7965-25F5-4B7C991EB775
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: f24be635afcba181efcab85d81a984d93dae4bc8
ms.sourcegitcommit: 650458de1d362cd7de174cacef7838f0e74426f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2019
ms.locfileid: "58071114"
---
# <a name="introduction-to-storyboards-in-xamarinios"></a>Xamarin.iOS에서 스토리 보드 소개

이 가이드의 어떤 스토리 보드 설명할 것 이며 – 고 Segues 같은 주요 구성 요소 중 일부를 검토 합니다. 스토리 보드 수 생성 및 사용 하는 방법을 살펴보겠습니다 있고 어떤 이점을 개발자.

스토리 보드 파일 형식에 의해 정의 된 Apple iOS 응용 프로그램의 UI의 시각적 표현으로, 전에 개발자는 각 보기 컨트롤러에 대 한 XIB 파일을 생성 하 고 수동으로 각 보기 간의 탐색을 프로그래밍 합니다.  스토리 보드를 사용 하 여 뷰 컨트롤러와에서 디자인 화면 간의 탐색을 정의 하는 개발자를 수 있습니다 하 고 응용 프로그램의 사용자 인터페이스의 wysiwyg (위지윅) 편집을 제공 합니다.

스토리 보드를 생성 하 고 사용 열, 게 Xamarin iOS 디자이너를 사용 하 여 편집할 수 있습니다. 이 가이드는 또한 연습 디자이너를 사용 하 여 C# 프로그램 탐색을 사용 하는 동안 스토리 보드를 만드는 방법.


## <a name="requirements"></a>요구 사항

스토리 보드 설치 Xamarin 워크 로드를 사용 하 여 Visual Studio 2017 또는 Mac 용 Visual Studio의 디자이너 iOS를 사용 하 여 사용할 수 있습니다.

## <a name="what-is-a-storyboard"></a>Storyboard 란?

스토리 보드에는 응용 프로그램에서 모든 화면의 시각적 표현입니다. 백그라운드에서 사용 하 여 각 장면 나타내는 시퀀스를 포함 하는 *뷰 컨트롤러* 및 해당 *뷰*합니다. 이러한 뷰 개체를 포함할 수 있습니다 하 고 [컨트롤](~/ios/user-interface/controls/index.md) 사용자 응용 프로그램과 상호 작용할 수 있도록 합니다. 이 컬렉션 보기와 컨트롤 (또는 *하위*) 라고는 *콘텐츠 뷰 계층 구조*합니다. 백그라운드에서 연결 된 여 segue 뷰 컨트롤러 사이 전환을 나타내는 개체입니다. 일반적으로 초기 보기에서 개체 및 연결 보기 사이 segue를 만들어 수행 됩니다. 디자인 화면에서 관계는 아래 이미지에 나와 있습니다.

 [![](images/storyboardsview.png "디자인 화면에서 관계는이 이미지에 나와 있습니다.")](images/storyboardsview.png#lightbox)

표시 된 대로 스토리 보드는 이미 렌더링 된 콘텐츠를 사용 하 여 각에 자동으로 배치 됩니다 하 고 둘 간의 연결을 보여 줍니다.  IPhone에서 장면을 이야기할 때 안전 하다는 가정 하에 시점에서 주목할 만한 것 *장면* 스토리 보드에서 값과 같음 하나 *화면* 장치의 콘텐츠입니다. 그러나 – 한 번에 표시 하는 여러 백그라운드에서 할 수 iPad를 사용 하 여 예를 들어,를 사용 하 여 팝 오버 뷰 컨트롤러.

스토리 보드를 사용 하 여 Xamarin을 사용 하는 경우에 특히 응용 프로그램의 UI를 만들려고 하면 많은 이점이 있습니다. 첫째, 것이 포함 한 모든 개체-로 UI의 시각적 표현 [사용자 지정 컨트롤](~/ios/user-interface/designer/ios-designable-controls-overview.md) – 디자인 타임에 렌더링 됩니다. 즉 빌드하거나 모양과 흐름을 시각화할 수 있습니다 하는 응용 프로그램을 배포 하기 전에 합니다. 위의 그림을 예로 들어 보겠습니다. 우리는 얼마나 많은 장면 있습니다 노출 되는 디자인, 각 뷰의 레이아웃 및 모든 관련 되는 방식을 간략히 보기에서 알 수 있습니다. 스토리 보드 정도로 기능이 막강 때문입니다.

IOS 디자이너를 사용 하는 경우에 특히 이벤트는 스토리 보드를 사용 하 여 보다 잘 관리할 수 있습니다. 대부분의 UI 컨트롤 속성 패드에서 가능한 이벤트 목록을 해야 합니다. 이벤트 처리기를 여기에 추가 하 고 뷰 컨트롤러 클래스에서 partial 메서드에서 완료 수...

스토리 보드의 콘텐츠를 XML 파일로 저장 됩니다. 빌드 시에 모든 `.storyboard` 파일 nibs 이라고 하는 이진 파일로 컴파일됩니다. 런타임 시 이러한 nibs 초기화 되 고 새 뷰를 만들 인스턴스화됩니다.

## <a name="segues"></a>Segue

A *Segue*, 또는 *Segue 개체*, 장면 사이 전환을 나타내는 데 iOS 개발에 사용 됩니다. 누른 segue를 만들려면 합니다 **Ctrl** 키 및 클릭 끌기 하나 장면에서 다른 합니다. 이 마우스를 끌면으로 segue 아래 이미지 에서처럼 될 위치를 나타내는 파란색 커넥터를 나타납니다.

 [![](images/createsegue.png "이 이미지에 설명 된 대로 segue를 만들 위치를 나타내는 파란색 커넥터 나타나면")](images/createsegue.png#lightbox)

마우스 놓기에이 segue에 대 한 작업을 선택할 수 있도록 메뉴가 표시 됩니다. 아래 이미지와 비슷하게 보일 수 있습니다. 

**사전 iOS 8 및 크기 클래스**:

[![](images/segue1.png "Size 클래스 없이 작업 Segue 드롭다운")](images/segue1.png#lightbox)

**Size 클래스 및 적응 Segue를 사용 하는 경우**:

[![](images/16new.png "Size 클래스를 사용 하 여 작업 Segue 드롭다운")](images/16new.png#lightbox)

> [!IMPORTANT]
> VMWare에 대 한 Windows 가상 컴퓨터를 사용할 경우 Ctrl + 클릭으로 매핑되는 _마우스 오른쪽 단추로 클릭_ 기본적으로 마우스 단추입니다. Segue를 만들려면를 통해 기본 키보드 설정 편집 **기본 설정** > **키보드 및 마우스** > **마우스 바로 가기** 및 다시 매핑 사용자 **보조 단추** 아래 그림과 같이:
> 
> [![](images/image22.png "키보드 및 마우스 기본 설정")](images/image22.png#lightbox)
> 
> 이제 정상적으로 보기 컨트롤러 사이 segue를 추가할 수 있어야 합니다.

전환, 사용자에 게 새 뷰 컨트롤러는 표시 하는 방법 및 다른 뷰 컨트롤러 스토리 보드를 사용 하 여 상호 작용 하는 방법을 통해 주어진 각 컨트롤의 다양 한 종류가 있습니다. 이러한 작업은 아래 설명 되어 있습니다. 하위 segue 사용자 지정 전환을 구현 하는 것도 가능 합니다.

-  **표시 / 푸시** – 푸시 segue를 탐색 스택으로 뷰 컨트롤러를 추가 합니다. 푸시를 시작 하 여 뷰 컨트롤러 스택에 추가 되는 뷰 컨트롤러와 동일한 탐색 컨트롤러의 일부인 가정 합니다. 와 동일한 작업을 수행이 `pushViewController` , 화면에서 데이터 간의 관계를 일부 경우에 일반적으로 사용 됩니다. Segue는 푸시를 사용 하 여 luxury 드릴 다운 보기 계층 구조를 탐색할 수 있도록 스택의 각 뷰에 추가한 제목 및 뒤로 단추를 사용 하 여 탐색 모음을 제공 합니다.
-  **모달** – 모달 segue 표시 되는 애니메이션 전환 옵션을 사용 하 여 프로젝트에서 모든 두 뷰 컨트롤러 간의 관계를 만듭니다. 자식 뷰 컨트롤러 뷰로 상태로 전환 될 때 부모 뷰 컨트롤러를 완전히 숨기 됩니다. Segue는 푸시와 달리, 뒤로 단추를 추가 하는 segue는 모달을 사용 하 여 경우 `DismissViewController` 이전 뷰 컨트롤러를 반환 하기 위해 사용 해야 합니다.
-  **사용자 지정** – 모든 사용자 지정의 서브 클래스로 segue를 만들 수 있습니다 ` UIStoryboardSegue`합니다.
-  **해제** – 해제 segue 모달 또는 푸시를 통해 다시 이동할 수 segue – 예를 들어 모달 형식으로 표시 하는 뷰 컨트롤러를 해제 하 여 합니다. 이 외에도 하나 뿐 아니라를 통해 해제할 수 있습니다 하지만 일련의 푸시 및 모달 segue 돌아가서 단일 탐색 계층의 여러 단계 작업을 해제 합니다. 읽기 ios에서 segue 해제를 사용 하는 방법을 알아야 합니다 [Segue 해제 만들기](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/unwind_segue) 작성법 합니다.
-  **원본** – 원본 없는 segue는 초기 뷰 컨트롤러를 포함 하는 장면을 나타내며 하므로 먼저 표시 됩니다 사용자 뷰. Segue 아래 표시 된 것으로 표시 됩니다.  

    [![](images/sourcelesssegue.png "원본 없는 segue를")](images/sourcelesssegue.png#lightbox)

### <a name="adaptive-segue-types"></a>적응 Segue 형식

 iOS 8 도입 [Size 클래스](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) 개발자가 모든 iOS 장치에 대 한 하나의 UI를 만들 수 있도록 하는 모든 사용 가능한 화면 크기, 사용 하려면 iOS 스토리 보드 파일을 허용 하도록 합니다. 기본적으로 모든 새 Xamarin.iOS 응용 프로그램은 size 클래스를 사용 합니다. 이전 프로젝트에서 size 클래스를 사용 하려면 참조는 [통합 스토리 보드 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 가이드입니다. 
 
Size 클래스를 사용 하 여 모든 응용 프로그램을 새 사용할지도 [ *적응 Segue*](~/ios/user-interface/storyboards/unified-storyboards.md)합니다. Size 클래스를 사용 하는 경우 직접 여부 지정 되지 것 기억 iPhone 또는 iPad를 사용 하는 것입니다. 즉 하나의 UI는 항상 동일 하 게 보이지만 얼마나 부동산을 작업 하는 것에 관계 없이 만드는 것입니다. 평가 환경 및 콘텐츠를 제공 하 최상의 방법을 결정 하 여 적응 고 Segues 작동 합니다. 적응 Segue는 다음과 같습니다. 

[![](images/adaptivesegue.png "적응 Segue 드롭다운")](images/adaptivesegue.png#lightbox)

|Segue|설명|
|--- |--- |
|표시|이 매우 비슷합니다 푸시 segue 있지만 화면의 콘텐츠를 고려 합니다.|
|세부 정보를 표시 합니다.|앱 (예: iPad에서 분할 뷰 컨트롤러)에서 마스터 / 세부 뷰를 표시 하는 경우 콘텐츠 세부 정보 뷰에서 바뀝니다. 앱에서 마스터 또는 세부 정보를 표시 하는 경우 콘텐츠 보기 컨트롤러 스택의 최상위를를 바뀝니다.|
|프레젠테이션|이 모달 segue 비슷합니다 및 프레젠테이션 및 전환 스타일을 선택할 수 있도록 합니다.|
|팝 오버 프레젠테이션|이 팝 오버도 콘텐츠를 제공|

### <a name="transferring-data-with-segues"></a>Segue 사용 하 여 데이터를 전송 합니다.

Segue의 이점을 전환으로 끝나지 않습니다. 뷰 컨트롤러 간의 데이터 전송을 관리 하는 것이 사용도 수 있습니다. 재정의 하 여 이렇게는 `PrepareForSegue` 메서드는 초기 뷰 컨트롤러와 직접 데이터를 처리 합니다. Segue 트리거되면 – 예를 들어, 단추 누름 –를 사용 하 여 응용 프로그램은이 메서드를 호출 새 뷰 컨트롤러를 준비 하는 기회를 제공 *하기 전에* 모든 탐색이 발생 합니다. 아래 코드에서의 [Phoneword](https://developer.xamarin.com/samples/monotouch/Hello_iOS/) 샘플,이 보여 줍니다. 


```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, 
NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    var callHistoryController = segue.DestinationViewController 
                                  as CallHistoryController;

    if (callHistoryController != null) {
        callHistoryController.PhoneNumbers = PhoneNumbers;
    }
}
```

이 예제는 `PrepareForSegue` segue가 트리거될 때 메서드가 호출 됩니다. 먼저 '수신' 뷰 컨트롤러의 인스턴스를 만들고이 segue의 대상 뷰 컨트롤러로 설정 해야 합니다. 아래 코드 줄을 하면 됩니다.

```csharp
var callHistoryController = segue.DestinationViewController as CallHistoryController;
```

메서드는 이제 속성을 설정 하는 기능에는 `DestinationViewController`합니다. 이 예제에서는 활용할이 호출 목록을 전달 하 여 `PhoneNumbers` 에 `CallHistoryController` 같은 이름의 개체에 할당 합니다.

```csharp
if (callHistoryController != null) {
        callHistoryController.PhoneNumbers = PhoneNumbers;
    }
```

전환이 완료 되 면 사용자에 게 표시 된 `CallHistoryController` 채워진 목록입니다.

## <a name="adding-a-storyboard-to-a-non-storyboard-project"></a>스토리 보드 Storyboard가 아닌 프로젝트에 추가

경우에 따라 이전에 비-스토리 보드 파일에 스토리 보드를 추가 해야 합니다. 다음 단계를 수행 하 여 한 번 Mac 용 Visual Studio에서 이렇게 간소화 될 수 있습니다.:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 로 이동 하 여 새 스토리 보드 파일을 만듭니다 **파일 > 새 파일 > iOS > 스토리 보드**아래 그림과 같이: 
    
    [![](images/new-storyboard-xs.png "새 파일 대화 상자")](images/new-storyboard-xs.png#lightbox)

2. 스토리 보드 이름을 추가 합니다 **주 인터페이스** 부분을 **Info.plist**아래 표시 된 것 처럼:
    
    [![](images/infoplist.png "Info.plist 편집기")](images/infoplist.png#lightbox)
    
    이 초기 뷰 컨트롤러의 인스턴스화의 같은 작업을 수행 합니다 `FinishedLaunching` 내 앱 대리자에서 메서드. 이 옵션 집합을 사용 하 여 응용 프로그램 창 (아래 참조), 주 스토리 보드를 로드 인스턴스화하고으로 스토리 보드의 Initial View Controller (원본 없는 Segue 옆에 있는 하나)의 인스턴스를 할당 합니다 `RootViewController` 창 찾아 다음 설정의 속성 화면에 표시 하는 창입니다.

3. 에 `AppDelegate`, 기본값을 재정의 `Window` window 속성을 구현 하는 다음 코드를 사용 하 여 메서드:
        
        public override UIWindow Window {
            get;
            set;
            }
            
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 프로젝트를 마우스 오른쪽 단추로 클릭 하 여 새 스토리 보드 파일을 만듭니다 **추가 > 새 파일 > iOS > 빈 스토리 보드**아래 그림과 같이: 
    
    [![](images/new-storyboard-vs.png "새 항목 대화 상자")](images/new-storyboard-vs.png#lightbox)

2. 스토리 보드 이름을 추가 합니다 **주 인터페이스** iOS 아래와 같이 응용 프로그램의 섹션:
    
    [![](images/ios-app.png "Info.plist 편집기")](images/ios-app.png#lightbox)
    
    이 초기 뷰 컨트롤러의 인스턴스화의 같은 작업을 수행 합니다 `FinishedLaunching` 내 앱 대리자에서 메서드. 이 옵션 집합을 사용 하 여 응용 프로그램 창 (아래 참조), 주 스토리 보드를 로드 인스턴스화하고으로 스토리 보드의 Initial View Controller (원본 없는 Segue 옆에 있는 하나)의 인스턴스를 할당 합니다 `RootViewController` 창 찾아 다음 설정의 속성 화면에 표시 하는 창입니다.

3. 에 `AppDelegate`, 기본값을 재정의 `Window` window 속성을 구현 하는 다음 코드를 사용 하 여 메서드:

        public override UIWindow Window {
            get;
            set;
            }
            
-----

## <a name="creating-a-storyboard-with-the-ios-designer"></a>IOS 디자이너를 사용 하 여 스토리 보드 만들기

Xamarin 디자이너를 사용 하 여 iOS, Mac 및 Visual Studio 용 Visual Studio를 사용 하 여 원활 하 게 통합에 대 한 스토리 보드를 만들 수 있습니다.

IOS 디자이너를 사용 하 여 스토리 보드 만들기를 시작 하려면 다음을 수행 합니다 [Hello, iOS 멀티 스크린](~/ios/get-started/hello-ios-multiscreen/index.md) 가이드입니다. 이 연습에서는 컨트롤에 이벤트를 처리 하는 방법과 고 Segues를 사용 하 여 뷰 컨트롤러 간의 탐색을 탐색 합니다.

## <a name="instantiate-storyboards-manually"></a>스토리 보드를 수동으로 인스턴스화합니다

하지만 개별 뷰 컨트롤러 스토리 보드에 여전히 인스턴스화할 수 있습니다 사용 하 여 스토리 보드 프로젝트에서 개별 XIB 파일을 완전히 대체 `Storyboard.InstantiateViewController`합니다.

경우에 따라 응용 프로그램 디자이너에서 제공 하는 기본 제공 스토리 보드 전환을 사용 하 여 처리할 수 없는 특별 한 요구 사항이 있습니다. 예를 들어, 응용 프로그램의 현재 상태에 따라 동일한 단추에서 다른 화면을 시작 하는 응용 프로그램을 만드는 경우를 뷰 컨트롤러를 수동으로 인스턴스화할 때 직접 전환 프로그램 원하는 수 있습니다.

아래 스크린샷에서 서로 segue 없이 디자인 화면에서 두 뷰 컨트롤러를 보여 줍니다. 다음 섹션에서는 코드에를 해당 전환에 설정할 수는 방법을 안내 합니다.

 [![](images/viewcontrollerspink.png "서로 segue 없이 디자인 화면에서 두 뷰 컨트롤러를 보여 주는이 스크린샷")](images/viewcontrollerspink.png#lightbox)

1. 추가 된 _빈 iPhone 스토리 보드_ 기존 프로젝트 프로젝트:
    
    [![](images/add-storyboard1.png "스토리 보드를 추가합니다.")](images/add-storyboard1.png#lightbox)

2. 새로 만든된 스토리 보드를 열을 두 번 클릭 하 고 새 **탐색 컨트롤러** 디자인 화면입니다. 탐색 컨트롤러는 ui를 기본적으로 아래와 같이 루트 뷰 컨트롤러를 사용 하 여 제공 됩니다.

    [![](images/uinavigationcontroller.png "사용 하 여 뷰 컨트롤러 Segue")](images/uinavigationcontroller.png#lightbox)

3. 선택 된 _뷰 컨트롤러_ 맨 아래에 있는 검은색 표시줄을 클릭 하 여 합니다. 디자이너의 **속성 패드**아래에 있는 **Identity** 뷰 컨트롤러에 대 한 고유 ID 뿐만 아니라 사용자 지정 클래스를 지정할 수 있습니다. 설정 합니다 **클래스 이름** 하 고 **스토리 보드 ID** 에 `MainViewController`입니다.

    [![](images/identitypanelnew.png "사용자 지정 클래스를 지정 합니다.")](images/identitypanelnew.png#lightbox)

4. 이 뷰 컨트롤러 스토리 보드에서 인스턴스화할 해야 나중에 코드에서 참고할 수 Storyboard ID를 사용 합니다. 스토리 보드 ID와 일치 하도록 복원 ID를 설정 하면 상태를 복원 해야 하는 경우 뷰 컨트롤러 가져옵니다 다시 올바르게 생성 합니다.

5. 에서는 현재 컨트롤러가 하나만 포함 보기. 다른 뷰 컨트롤러를 디자인 화면으로 끕니다. 에 **속성 패드**, 클래스 및 스토리 보드 ID를 설정 id로 `PinkViewController`아래 그림과 같이:

    [![](images/pinkvcnew.png "속성 패드")](images/pinkvcnew.png#lightbox)
    
    IDE에서는 보기 컨트롤러에 대 한 이러한 사용자 지정 클래스를 만듭니다. 볼 수 있습니다 이러한 합니다 **Solution Pad**와 아래 스크린샷에 표시 된 것과 같이:
    
    [![](images/solution-pad.png "Solution Pad")](images/solution-pad.png#lightbox)

6. 에 `PinkViewController`, 컨트롤러의 프레임의 가운데 방향으로 클릭 하 여 뷰를 선택 합니다. Properties Pad에서 뷰 변경 합니다 **백그라운드** 자홍에:
    
    [![](images/pinkcontroller.png "배경색을 설정")](images/pinkcontroller.png#lightbox)

7. 마지막으로,에서 단추를 끌어 합니다 **도구 상자** 에 `MainViewController`합니다. Properties Pad의 이름을 지정 `PinkButton` 및 제목 GoToPink, 아래 그림과 같이:

    [![](images/pinkbutton.png "단추 이름 설정")](images/pinkbutton.png#lightbox)

스토리 보드 완료 되었지만 빈 화면 하면 프로젝트를 지금 배포 하는 경우. 이 스토리 보드를 사용 하 고 첫 번째 뷰로 제공 하는 루트 보기 컨트롤러를 설정 하도록 IDE에 지시 하기 필요 때문입니다. 일반적으로 위에 표시 된 대로 프로젝트 옵션을 통해이 수행할 수 있습니다. 하지만 다음을 추가 하 여 코드에서 동일한 결과 수행이 예제에서는 합니다 **AppDelegate**:

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

코드의 많은 되지만 몇 줄만 익숙하지 없습니다. 이 storyboard 먼저 등록 합니다 **AppDelegate** 스토리 보드의 이름 전달 하 여 **MainStoryboard**합니다. 다음으로, 응용 프로그램을 호출 하 여 스토리 보드에서 초기 뷰 컨트롤러를 인스턴스화하 알려줍니다 `InstantiateInitialViewController` 이 스토리 보드의 보기 컨트롤러 응용 프로그램의 루트 뷰 컨트롤러로 설정 합니다. 이 메서드는 사용자에 게 표시 되는 첫 번째 화면을 결정 하 고 해당 뷰 컨트롤러의 새 인스턴스를 만듭니다.

IDE 만들어진 솔루션 창에서 `MainViewcontroller.cs` 클래스 및 해당 `corresponding designer.cs` 4 단계에서 Properties Pad에 클래스 이름을 추가한 경우. 기본 클래스를 포함 하는 특수 한 생성자를 생성 합니다.이 클래스에 알 수 있습니다.

```csharp
public MainViewController (IntPtr handle) : base (handle) 
{
}
```


IDE 디자이너를 사용 하 여 스토리 보드를 만들 때 자동으로 추가 됩니다는 [[등록]](xref:Foundation.RegisterAttribute) 맨 위에 있는 특성을 `designer.cs` 클래스 및 Storyboard ID에 지정 된 동일한 문자열 식별자를 전달 합니다 이전 단계입니다. 이 연결 C# 관련 장면에서 스토리 보드.

기존 클래스를 추가 하려는 특정 시점 **되지** 디자이너에서 생성 합니다. 이 경우 정상적으로이 클래스를 등록 합니다.

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

클래스 및 메서드를 등록에 대 한 자세한 내용은 참조는 [형식 등록자](http://docs.xamarin.com/guides/ios/advanced_topics/registrar/) 설명서.

이 클래스의 마지막 단계에서는 단추와 분홍색 뷰 컨트롤러 전환 연결 합니다. 인스턴스화할 수는 `PinkViewController` 스토리 보드;에서 차례로 프로그래밍 푸시를 사용 하 여 segue `PushViewController`아래 예제 코드에 나온 것 처럼,:

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

응용 프로그램을 실행 2 화면 응용 프로그램을 생성 합니다.

![](images/finishedstoryboard.png "화면을 실행 하는 샘플 앱")

## <a name="conditional-segues"></a>조건부 Segue

종종 한 뷰 컨트롤러에서 다음으로 이동 되는 특정 조건에 따라 달라 집니다. 예를 들어, 간단한 로그인 화면을 만드는 경우는 하고자 다음 화면으로 이동 *경우* 사용자 이름 및 암호를 확인 했습니다.

다음 예제에서 위의 샘플 암호 필드를 추가 합니다. 사용자가 액세스할 수 합니다 *PinkViewController* 올바른 암호를 입력, 그렇지 않으면 오류가 표시 됩니다.

시작 하기 전에 1 ~ 8 위의 단계를 따릅니다. 이 단계에서는 스토리 보드를 만드는 것,이 UI를 만들고 사용 하는 뷰 컨트롤러는 앱 대리자를 지시 하려면 RootViewController 그대로입니다.

1. 이제 보겠습니다 UI를 구축 하 고 나열 된 추가 뷰를 추가 `MainViewController` 아래 스크린샷과에서 같은 표시 되도록 합니다.

    - UITextField
        - 이름: PasswordTextField
        - 자리 표시자: ' 암호를 입력 합니다. '
    - UILabel
        - 텍스트: ' 오류: 잘못 된 암호입니다. 전달 하지 않습니다. '
        - 색: 빨강
        - Alignment: 가운데 맞춤
        - 줄: 2
        - '숨겨진된' 확인란을 선택한 상태 
        
    [![](images/passwordvc.png "Center 줄")](images/passwordvc.png#lightbox)
    
2. 분홍색 이동 단추 및 Ctrl-끌어 뷰 컨트롤러 사이 Segue를 만들기는 *PinkButton* 에 *PinkViewController*를 선택 하 고 **푸시** 마우스 놓기에서 . 

3. Segue 클릭 하 고 지정 된 *식별자* `SegueToPink`:

    [![](images/namesegue.png "Segue 클릭 하 고 식별자 SegueToPink 지정")](images/namesegue.png#lightbox)  
    

4. 마지막으로 다음 ShouldPerformSegue 메서드를 추가 합니다 `MainViewController` 클래스:

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

이 코드를 segueIdentifier 일치할 것이 `SegueToPink` segue를 다음 조건을 테스트할 수 있도록이 경우 올바른 암호를 합니다. 이 조건에서 반환 하는 경우 `true`, Segue에서 수행 하 고 표시 됩니다는 `PinkViewController`합니다. 경우 `false`, 새 뷰 컨트롤러 표시 되지 것입니다.

이 방법은 ShouldPerformSegue 메서드에 segueIdentifier 인수를 확인 하 여이 뷰 컨트롤러의 모든 Segue를 적용할 수 있습니다. 이 경우에 있습니다 하나 Segue 식별자 – `SegueToPink`합니다.

Storyboards.Conditional 솔루션에 대 한 참조를 [수동 스토리 보드 샘플](https://developer.xamarin.com/samples/monotouch/ManualStoryboard/) 작업 예제입니다.

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>스토리 보드 참조 사용

스토리 보드 참조를 사용 하는 대규모의 복잡 한 스토리 보드 디자인을 수행 하 고 더 작은 스토리 보드에 원본에서 참조 가져오기 있으므로 복잡성을 제거 하 고 결과 개별 스토리 보드 디자인 및 유지 관리를 쉽게으로 나누는 수 있습니다.

또한 스토리 보드 참조를 제공할 수는 _앵커_ 동일한 스토리 보드 또는 다른 특정 장면 내에서 다른 장면으로 합니다.

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>스토리 보드를 외부 참조

스토리 보드를 외부에 대 한 참조를 추가 하려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**, 프로젝트 이름을 마우스 오른쪽 단추로 **추가** > **새 파일...**   >  **iOS** > **스토리 보드**합니다. 입력을 **이름을** 새 스토리 보드 및 클릭 합니다 **새로 만들기** 단추:
    
    [![](images/ref01.png "새 파일 대화 상자")](images/ref01.png#lightbox)
    
2. 일반적으로 하는 변경 내용을 저장 하는 대로 새 스토리 보드의 백그라운드에서의 레이아웃을 디자인 합니다. 
    
    [![](images/ref02.png "새 장면의 레이아웃")](images/ref02.png#lightbox)
    
3. IOS 디자이너에에서 대 한 참조를 추가 하는 것은 스토리 보드를 엽니다.

4. 끌어서를 **참조를 스토리 보 딩** 에서 합니다 **도구 상자** 디자인 화면으로: 
    
    [![](images/ref03.png "스토리 보드 참조")](images/ref03.png#lightbox)
    
5. **위젯** 탭을 **속성 탐색기**의 이름을 선택 합니다 **스토리 보드** 위에서 만든: 

    [![](images/ref04.png "위젯 탭")](images/ref04.png#lightbox)
    
6. 에 새 Segue를 만들고 제어는 기존 장면에서 UI 위젯 (예: 단추)를 클릭 합니다 **스토리 보드 참조** 방금 만든: 

    [![](images/ref05.png "Segue를 만들기")](images/ref05.png#lightbox) 
    
7. 팝업 메뉴에서 선택 **표시** Segue를 완료 하려면: 

    [![](images/ref06.png "Segue를 완료 하려면 표시를 선택 합니다.")](images/ref06.png#lightbox) 
    
8. 스토리 보드에 변경 내용을 저장 합니다.

앱이 실행 되 고 초기 뷰 컨트롤러 스토리 보드 참조에 지정 된 외부 스토리 보드에서에서 Segue를 생성 하는 사용자가 UI 요소에 표시 됩니다.

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>스토리 보드를 외부에서 특정 장면의 참조

특정 장면에 대 한 참조를 추가 하려면 스토리 보드를 외부 (및 Initial View Controller) 다음을 수행 합니다.

1. 에 **솔루션 탐색기**를 열어 편집 하는 외부 스토리 보드를 두 번 클릭 합니다.

2. 새 장면을 추가 하 고 평소와 같이 해당 레이아웃을 디자인 합니다. 

    [![](images/ref07.png "새 화면 레이아웃")](images/ref07.png#lightbox)
    
3. 에 **위젯** 탭을 **속성 탐색기**, 입력을 **스토리 보드 ID** 새 장면의 보기 컨트롤러에 대 한: 

    [![](images/ref08.png "새 백그라운드에서 뷰 컨트롤러에 대 한 스토리 보드 ID를 입력 합니다.")](images/ref08.png#lightbox)
    
3. IOS 디자이너에에서 대 한 참조를 추가 하는 것은 스토리 보드를 엽니다.

4. 끌어서를 **참조를 스토리 보 딩** 에서 합니다 **도구 상자** 디자인 화면으로: 

    [![](images/ref03.png "스토리 보드 참조")](images/ref03.png#lightbox)
    
5. 에 **위젯** 탭을 **속성 탐색기**의 이름을 선택 합니다 **스토리 보드** 및 **참조 ID** (스토리 보드 ID)의 위에서 만든 장면: 

    [![](images/ref09.png "위젯 탭 ")](images/ref09.png#lightbox)
    
6. 에 새 Segue를 만들고 제어는 기존 장면에서 UI 위젯 (예: 단추)를 클릭 합니다 **스토리 보드 참조** 방금 만든: 

    [![](images/ref10.png "Segue를 만들기")](images/ref10.png#lightbox) 
    
7. 팝업 메뉴에서 선택 **표시** Segue를 완료 하려면: 

    [![](images/ref06.png "Segue를 완료 하려면 표시를 선택 합니다.")](images/ref06.png#lightbox) 
    
8. 스토리 보드에 변경 내용을 저장 합니다.

경우 앱 실행 및 사용자가 만든 Segue를 사용 하 여 장면에서 UI 요소에는 주어진 **스토리 보드 ID** 스토리 보드 참조에 지정 된 외부 스토리 보드에서 표시 됩니다.

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>동일한 스토리 보드에서 특정 장면의 참조

특정 장면 동일한 스토리 보드에 대 한 참조를 추가 하려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**를 열어 편집 하는 스토리 보드를 두 번 클릭 합니다.

2. 새 장면을 추가 하 고 평소와 같이 해당 레이아웃을 디자인 합니다. 

    [![](images/ref11.png "새 화면 레이아웃")](images/ref11.png#lightbox)

3. 에 **위젯** 탭을 **속성 탐색기**, 입력을 **스토리 보드 ID** 새 장면의 보기 컨트롤러에 대 한: 

    [![](images/ref12.png "위젯 탭")](images/ref12.png#lightbox)
    
3. 끌어서를 **참조를 스토리 보 딩** 에서 합니다 **도구 상자** 디자인 화면으로: 

    [![](images/ref03.png "스토리 보드 참조")](images/ref03.png#lightbox)
    
5. 에 **위젯** 탭의 **속성 탐색기**를 선택 **참조 ID** (스토리 보드 ID) 위에서 만든 장면: 

    [![](images/ref13.png "위젯 탭")](images/ref13.png#lightbox)
    
6. 에 새 Segue를 만들고 제어는 기존 장면에서 UI 위젯 (예: 단추)를 클릭 합니다 **스토리 보드 참조** 방금 만든: 

    [![](images/ref14.png "Segue를 만들기")](images/ref14.png#lightbox) 
    
7. 팝업 메뉴에서 선택 **표시** Segue를 완료 하려면: 

    [![](images/ref06.png "Segue를 완료 하려면 표시를 선택 합니다.")](images/ref06.png#lightbox) 
    
8. 스토리 보드에 변경 내용을 저장 합니다.

경우 앱 실행 및 사용자가 만든 Segue를 사용 하 여 장면에서 UI 요소에는 주어진 **스토리 보드 ID** 스토리 보드 참조에 지정 된 동일한 스토리 보드에 표시 됩니다.

## <a name="summary"></a>요약

이 문서에서는 스토리 보드 및 iOS 응용 프로그램 개발에 유용할 수 있습니다 하는 방법의 개념을 소개 합니다. 장면, 뷰 컨트롤러, 뷰 및 뷰 계층 구조 및 다양 한 유형의 고 Segues 함께 자동으로 연결 되는 방법을 설명 합니다.  또한 탐색 인스턴스화하 뷰 컨트롤러 수동으로 만드는 조건부 고 Segues를 스토리 보드에서.



## <a name="related-links"></a>관련 링크

- [수동 스토리 보드 (샘플)](https://developer.xamarin.com/samples/ManualStoryboard/)
- [IOS 디자이너 소개](~/ios/user-interface/designer/introduction.md)
- [스토리 보드에 변환](https://developer.apple.com/library/ios/#releasenotes/Miscellaneous/RN-AdoptingStoryboards/)
- [UIStoryboard 클래스 참조](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UIStoryboard_Class/Reference/Reference.html)
