---
title: iOS Designer 기본 사항
description: 이 가이드에서는 Xamarin iOS 디자이너 소개 합니다. IOS 디자이너를 사용 하 여 시각적으로 컨트롤을 배치 하는 방법, 코드에서 해당 컨트롤에 액세스 하는 방법 및 속성을 편집 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: E7045E41-0DEF-416B-BCDB-52502350F61C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/31/2018
ms.openlocfilehash: 6905eddbc4488b08f9c9e896efe5f980e0e03345
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242370"
---
# <a name="ios-designer-basics"></a>iOS Designer 기본 사항

_이 가이드에서는 Xamarin iOS 디자이너 소개 합니다. IOS 디자이너를 사용 하 여 시각적으로 컨트롤을 배치 하는 방법, 코드에서 해당 컨트롤에 액세스 하는 방법 및 속성을 편집 하는 방법을 보여 줍니다._

IOS 용 Xamarin 디자이너에는 Xcode의 Interface Builder 비슷합니다는 시각적 인터페이스 디자이너와 Android Designer는입니다. 여러 기능 중 일부 Mac 및 Visual Studio 2015 및 2017 용 Visual Studio, 끌어서 놓기, 이벤트 처리기를 설정에 대 한 인터페이스 및 사용자 지정 컨트롤을 렌더링 하는 기능을 사용 하 여 원활한 통합을 포함 합니다.

## <a name="requirements"></a>요구 사항

IOS 디자이너는 Windows에서 Visual Studio 2015 및 2017 및 Mac 용 Visual Studio에서 사용할 수 있습니다. Visual Studio 2015 또는 2017에서 iOS 디자이너 Xcode에 필요한 실행 되 고 있지 않더라도 올바르게 구성 된 Mac 빌드 호스트에 대 한 연결을 필요 합니다.

이 가이드에서 다루는 내용에 익숙하다고 가정 합니다 [시작 하기 가이드](~/ios/get-started/index.md)합니다.

<a name="how-it-works" />

## <a name="how-the-ios-designer-works"></a>IOS 디자이너의 작동 원리

이 섹션에서는 iOS 디자이너 사용자 인터페이스를 만들고 코드에 연결할 용이 하 게 하는 방법을 설명 합니다.

IOS 디자이너는 개발자가 응용 프로그램의 사용자 인터페이스를 시각적으로 디자인할 수 있습니다. 에 설명 된 대로 합니다 [스토리 보드 소개](~/ios/user-interface/storyboards/index.md) 가이드, 스토리 보드 화면 (뷰 컨트롤러) 앱을 구성 하는 이러한 뷰 컨트롤러 및 앱의 전체 탐색 흐름에 배치 (views) 인터페이스에서 요소를 설명 합니다. . 

뷰 컨트롤러에는 두 부분이 있습니다: iOS 디자이너의에서 시각적 표현과 연결된 된 C# 클래스:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![IOS 디자이너에서에서 뷰 컨트롤러](introduction-images/1-storyboardwithviewcontroller-vsmac.png "iOS 디자이너에서에서 뷰 컨트롤러")](introduction-images/1-storyboardwithviewcontroller-vsmac-large.png#lightbox)

[![뷰 컨트롤러에 대 한 코드](introduction-images/2-viewcontrollercode-vsmac.png "뷰 컨트롤러에 대 한 코드")](introduction-images/2-viewcontrollercode-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![IOS 디자이너에서에서 뷰 컨트롤러](introduction-images/1-storyboardwithviewcontroller-vs.png "iOS 디자이너에서에서 뷰 컨트롤러")](introduction-images/1-storyboardwithviewcontroller-vs-large.png#lightbox)

[![뷰 컨트롤러에 대 한 코드](introduction-images/2-viewcontrollercode-vs.png "뷰 컨트롤러에 대 한 코드")](introduction-images/2-viewcontrollercode-vs-large.png#lightbox)

-----

뷰 컨트롤러를 해당 기본 상태인 모든 기능을 제공 하지 않습니다. 컨트롤을 사용 하 여 채워야 합니다. 이러한 컨트롤은 뷰의 뷰 컨트롤러의 모든 화면 콘텐츠를 포함 하는 사각형 영역에 배치 됩니다. 단추를 포함 하는 뷰 컨트롤러를 보여 주는 다음 스크린샷에 표시 된 것과 같이 단추, 레이블 및 텍스트 필드와 같은 일반적인 컨트롤을 포함 하는 대부분의 뷰 컨트롤러: 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![단추를 포함 하는 보기 컨트롤러](introduction-images/3-viewcontrollerwithbutton-vsmac.png "단추가 포함 된 뷰 컨트롤러")](introduction-images/3-viewcontrollerwithbutton-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![단추를 포함 하는 보기 컨트롤러](introduction-images/3-viewcontrollerwithbutton-vs.png "단추가 포함 된 뷰 컨트롤러")](introduction-images/3-viewcontrollerwithbutton-vs-large.png#lightbox)

-----

정적 텍스트를 포함 하는 레이블 같은 일부 컨트롤 보기 컨트롤러에 추가 하 고 그대로 둘 수 있습니다. 그러나 더 경우가 컨트롤 사용자 지정 해야 프로그래밍 방식으로 합니다. 예를 들어 위에서 추가한 단추 이벤트 처리기 코드에 추가 해야 하므로 변하는데 것으로 수행 해야 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

에 액세스 하 고 코드에서 단추를 조작 하기 위해 고유 식별자가 있어야 합니다. 열기 단추를 선택 하 여 고유 식별자를 제공 합니다 **Properties Pad**, 및 설정을 해당 **이름** 제출 "단추"와 같은 값으로 필드:

[![Properties Pad에서 단추의 이름을 설정할](introduction-images/4-settingbuttonname-vsmac.png "Properties Pad에서 단추의 이름을 설정")](introduction-images/4-settingbuttonname-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

에 액세스 하 고 코드에서 단추를 조작 하기 위해 고유 식별자가 있어야 합니다. 열기 단추를 선택 하 여 고유 식별자를 제공 합니다 **속성 창**, 및 설정을 해당 **이름** 제출 "단추"와 같은 값으로 필드:

[![속성 창에서 단추의 이름을 설정할](introduction-images/4-settingbuttonname-vs.png "속성 창에서 단추의 이름을 설정")](introduction-images/4-settingbuttonname-vs-large.png#lightbox)

-----

단추 이름이 했으므로 코드에서 액세스할 수 있습니다. 그러나이 작업은?

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

에 **Solution Pad**로 이동 하면 **ViewController.cs** 함을 의미할 공개 표시기를 클릭 하 고 뷰 컨트롤러의 `ViewController` 각각 두 개의 클래스 정의 범위 파일 포함 된 [partial 클래스](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) 정의:

[![ViewController 클래스를 구성 하는 두 개의 파일: ViewController.cs 및 ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vsmac.png "ViewController 클래스를 구성 하는 두 개의 파일: ViewController.cs 및 ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

에 **솔루션 탐색기**로 이동 하면 **ViewController.cs** 함을 의미할 공개 표시기를 클릭 하 고 뷰 컨트롤러의 `ViewController` 클래스 정의의 각 두 파일에 걸쳐 포함 하는 [partial 클래스](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) 정의:

[![ViewController 클래스를 구성 하는 두 개의 파일: ViewController.cs 및 ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vs.png "ViewController 클래스를 구성 하는 두 개의 파일: ViewController.cs 및 ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vs-large.png#lightbox)

-----

- **ViewController.cs** 와 관련 된 사용자 지정 코드를 사용 하 여 채워져야 합니다 `ViewController` 클래스입니다. 이 파일에는 `ViewController` 클래스 수 보기 컨트롤러 수명 주기 메서드를 다양 한 iOS 응답할 UI를 사용자 지정 및 단추 탭과 같은 사용자 입력에 응답 합니다.

- **ViewController.designer.cs** iOS 코드를 시각적으로 생성 된 인터페이스를 매핑하는 디자이너에서 생성 된, 생성 된 파일입니다. 이 파일에 변경 내용을 덮어쓰게 되므로 수정 하지 않아야 합니다. 이 파일의 속성 선언을 확인 코드에 대 한 가능한 합니다 `ViewController` 액세스 하 여 클래스 **이름**, iOS 디자이너에서에서 구성 집합을 제어 합니다. 열기 **ViewController.designer.cs** 다음 코드를 보여 줍니다.

```csharp
namespace Designer
{
    [Register ("ViewController")]
    partial class ViewController
    {
        [Outlet]
        [GeneratedCode ("iOS Designer", "1.0")]
        UIKit.UIButton SubmitButton { get; set; }

        void ReleaseDesignerOutlets ()
        {
            if (SubmitButton != null) {
                SubmitButton.Dispose ();
                SubmitButton = null;
            }
        }
    }
}
```

`SubmitButton` 전체를 연결 하는 속성 선언을 `ViewController` 클래스-만 **ViewController.designer.cs** 파일 – 스토리 보드에 정의 된 단추입니다. 이후 **ViewController.cs** 의 부분을 정의 합니다 `ViewController` 클래스에 액세스할 수 있습니다 `SubmitButton`합니다.

다음 스크린샷에서 IntelliSense 인식할 수 있게 하는 방법을 보여 줍니다 합니다 `SubmitButton` 에서 참조할 **ViewController.cs**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![IntelliSense SubmitButton 참조 인식](introduction-images/6-submitbuttonintellisense-vsmac.png "SubmitButton 파일에 대 한 참조를 인식 하는 IntelliSense")](introduction-images/6-submitbuttonintellisense-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![IntelliSense SubmitButton 참조 인식](introduction-images/6-submitbuttonintellisense-vs.png "SubmitButton 파일에 대 한 참조를 인식 하는 IntelliSense")](introduction-images/6-submitbuttonintellisense-vs-large.png#lightbox)

-----

이 섹션에 설명 된 iOS 디자이너에서에서 단추를 만드는 방법과 코드에서 해당 단추에 액세스 합니다.

이 문서의 나머지 부분 iOS 디자이너의 자세한 개요를 제공합니다.

## <a name="ios-designer-basics"></a>iOS Designer 기본 사항

이 섹션에서는 iOS 디자이너의 일부를 소개 하 고 해당 기능을 살펴보고를 제공 합니다.

### <a name="launching-the-ios-designer"></a>IOS 디자이너를 시작합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Xamarin.iOS 프로젝트 Mac 용 Visual Studio를 사용 하 여 만든 스토리 보드를 포함 합니다. 스토리 보드의 콘텐츠를 보려면에서.storyboard 파일을 두 번 클릭 합니다 **Solution Pad**:

[![IOS 디자이너에서에서 스토리 보드를 엽니다](introduction-images/7-storyboardopen-vsmac.png "iOS 디자이너에서에서 스토리 보드 열기")](introduction-images/7-storyboardopen-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

대부분의 Xamarin.iOS 프로젝트를 Visual Studio 2015 또는 2017을 사용 하 여 만든 스토리 보드를 포함 합니다. 스토리 보드의 콘텐츠를 보려면에서.storyboard 파일을 두 번 클릭 합니다 **솔루션 탐색기**:

[![IOS 디자이너에서에서 스토리 보드를 엽니다](introduction-images/7-storyboardopen-vs.png "iOS 디자이너에서에서 스토리 보드 열기")](introduction-images/7-storyboardopen-vs-large.png#lightbox)

-----

<a name="iOS_Designer_features"/>

### <a name="ios-designer-features"></a>iOS 디자이너 기능

IOS 디자이너에 6 개의 주요 섹션으로 이루어져 있습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![IOS 디자이너의 섹션](introduction-images/8-sixpartsofiosdesigner-vsmac.png "iOS 디자이너의 섹션")](introduction-images/8-sixpartsofiosdesigner-vsmac-large.png#lightbox)

1. **디자인 화면** – iOS 디자이너의 기본 작업 영역입니다. 사용자 인터페이스의 시각적 생성 수 있도록 문서 영역에 표시 합니다.
2. **제약 조건 도구 모음** – 모드 및 제약 조건 편집 모드 라는 두 가지 방법으로 사용자 인터페이스에서 요소를 배치 하는 편집 프레임을 전환할 수 있습니다.
3. **도구 상자** – 컨트롤러, 개체, 컨트롤, 데이터 뷰, 제스처 인식기, windows를 나열 하 고 막대는 디자인 화면으로 끌어 및 사용자 인터페이스를 추가할 수 있습니다.
4. **Properties Pad** – id, 비주얼 스타일, 내게 필요한 옵션, 레이아웃 및 동작을 포함 하 여 선택된 된 컨트롤에 대 한 속성을 보여 줍니다.
5. **문서 개요** – 편집 중인 인터페이스에 대 한 레이아웃을 구성 하는 컨트롤의 트리를 보여 줍니다. IOS 디자이너에서에서 선택 하 고에서 해당 속성을 보여 줍니다. 트리에서 항목을 클릭 하 여 **Properties Pad**합니다. 깊게 중첩 된 사용자 인터페이스에서 특정 컨트롤을 선택할 때 유용 합니다.
6. **도구 모음을 아래쪽** – iOS 디자이너 장치, 방향 및 확대/축소를 포함 하는.storyboard 또는.xib 파일을 표시 하는 방법을 변경 하기 위한 옵션을 포함 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![IOS 디자이너의 섹션](introduction-images/8-sixpartsofiosdesigner-vs.png "iOS 디자이너의 섹션")](introduction-images/8-sixpartsofiosdesigner-vs-large.png#lightbox)

1. **디자인 화면** – iOS 디자이너의 기본 작업 영역입니다. 사용자 인터페이스의 시각적 생성 수 있도록 문서 영역에 표시 합니다.
2. **제약 조건 도구 모음** – 모드 및 제약 조건 편집 모드 라는 두 가지 방법으로 사용자 인터페이스에서 요소를 배치 하는 편집 프레임을 전환할 수 있습니다.
3. **도구 상자** – 컨트롤러, 개체, 컨트롤, 데이터 뷰, 제스처 인식기, windows를 나열 하 고 막대는 디자인 화면으로 끌어 및 사용자 인터페이스를 추가할 수 있습니다.
4. **속성 창** – id, 비주얼 스타일, 내게 필요한 옵션, 레이아웃 및 동작을 포함 하 여 선택된 된 컨트롤에 대 한 속성을 보여 줍니다.
5. **문서 개요** – 편집 중인 인터페이스에 대 한 레이아웃을 구성 하는 컨트롤의 트리를 보여 줍니다. IOS 디자이너에서에서 선택 하 고에서 해당 속성을 보여 줍니다. 트리에서 항목을 클릭 하 여 **속성 창**합니다. 깊게 중첩 된 사용자 인터페이스에서 특정 컨트롤을 선택할 때 유용 합니다.
6. **도구 모음을 아래쪽** – iOS 디자이너 장치, 방향 및 확대/축소를 포함 하는.storyboard 또는.xib 파일을 표시 하는 방법을 변경 하기 위한 옵션을 포함 합니다.

-----

### <a name="design-workflow"></a>워크플로 디자인

#### <a name="adding-a-control-to-the-interface"></a>인터페이스에 컨트롤 추가

컨트롤에 인터페이스를 추가 하려면 놓습니다 합니다 **도구 상자** 디자인 화면에 놓습니다. 에 추가 하거나 컨트롤의 위치를 지정 하는 경우 여백 세로 가운데, 가로 가운데 등 일반적으로 사용 되는 레이아웃 위치 세로 및 가로 지침에 요약 되어 있습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
![디자인 화면의 지침에는 일반적으로 사용 되는 레이아웃 위치가 강조 표시](introduction-images/9-layoutguides-vsmac.png "지침 디자인 화면에서 자주 사용 하는 레이아웃 위치를 강조 표시")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![디자인 화면의 지침에는 일반적으로 사용 되는 레이아웃 위치가 강조 표시](introduction-images/9-layoutguides-vs.png "지침 디자인 화면에서 자주 사용 하는 레이아웃 위치를 강조 표시")

-----

위의 예제에서 파란색 점선 단추 배치 하는 데는 가로 가운데 맞춤 visual 지침을 제공 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

#### <a name="context-menu-commands"></a>상황에 맞는 메뉴 명령

상황에 맞는 메뉴를 디자인 화면에서와에서 모두 사용할 수는 **문서 개요**합니다. 이 메뉴는 명령을 제공 합니다.는 선택한 컨트롤 및 해당 부모 작업의 중첩된 된 계층 구조 보기를 사용 하는 경우 유용 합니다.

[![디자인 화면에서 상황에 맞는 메뉴](introduction-images/10-contextmenudesignsurface-vsmac.png "디자인 화면의 상황에 맞는 메뉴")](introduction-images/10-contextmenudesignsurface-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-----

### <a name="constraints-toolbar"></a>제약 조건 도구 모음

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
[![제약 조건 도구 모음](introduction-images/11-constraintstoolbar-vsmac.png "제약 조건 도구 모음")](introduction-images/11-constraintstoolbar-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![제약 조건 도구 모음](introduction-images/11-constraintstoolbar-vs.png "제약 조건 도구 모음")](introduction-images/11-constraintstoolbar-vs-large.png#lightbox)

-----

제약 조건 도구 모음 업데이트 되었습니다 및 이제 두 개의 구성: 프레임 편집 모드 제약 조건 모드 설정/해제 하 고 업데이트 제약 조건을 편집 / 업데이트 프레임 단추 /입니다.

#### <a name="frame-editing-mode--constraint-editing-mode-toggle"></a>프레임 편집 모드 / 제약 조건 모드 설정/해제 편집

IOS 디자이너의 이전 버전에서는 프레임 편집 모드 및 제약 조건 편집 모드 간 전환 디자인 화면에서 이미 선택한 보기를 클릭 합니다. 이제 제약 조건 도구 모음에서 토글 컨트롤이 편집 모드 간에 전환 합니다.

- 프레임 편집 모드:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![프레임 편집 모드 단추](introduction-images/12a-frameeditingmode-vsmac.png "프레임 편집 모드 단추")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![프레임 편집 모드 단추](introduction-images/12a-frameeditingmode-vs.png "프레임 편집 모드 단추")

-----

- 제약 조건 편집 모드:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![제약 조건 편집 모드 단추](introduction-images/12b-constrainteditingmode-vsmac.png "제약 조건 편집 모드 단추")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![제약 조건 편집 모드 단추](introduction-images/12b-constrainteditingmode-vs.png "제약 조건 편집 모드 단추")

-----

#### <a name="update-constraints--update-frames-button"></a>제약 조건 업데이트/업데이트 프레임 단추

업데이트 제약 조건을 업데이트 프레임 편집 모드 프레임의 오른쪽에 단추 위치 / / 제약 조건 편집 모드 설정/해제 합니다.

- 프레임 편집 모드에서에서이 단추를 클릭 하면 해당 제약 조건에 맞게 선택한 요소의 프레임을 조정 합니다.
- 제약 조건 편집 모드에서이 단추를 클릭 하면 해당 프레임에 맞게 선택한 요소의 제약 조건을 조정 합니다.

### <a name="bottom-toolbar"></a>아래쪽 도구 모음

아래쪽 도구 모음의 장치, 방향 및 스토리 보드 또는.xib 파일을 보려면 iOS 디자이너에서에서 하는 데 사용 하는 확대/축소를 선택 하는 방법을 제공 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![장치 및 디자인 화면 방향을 선택 하는 데 아래쪽 도구 모음](introduction-images/13-bottomtoolbar-vsmac.png "장치 및 디자인 화면 방향을 선택 하는 데 아래쪽 도구 모음")](introduction-images/13-bottomtoolbar-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![장치 및 디자인 화면 방향을 선택 하는 데 아래쪽 도구 모음](introduction-images/13-bottomtoolbar-vs.png "장치 및 디자인 화면 방향을 선택 하는 데 아래쪽 도구 모음")](introduction-images/13-bottomtoolbar-vs-large.png#lightbox)

-----

#### <a name="device-and-orientation"></a>장치 및 방향

확장 하면 아래쪽 도구 모음의 모든 장치, 방향 및/또는 적응 현재 문서에 적용할 수를 표시 합니다. 클릭 하 디자인 화면에 표시 된 보기를 변경 합니다. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![아래쪽 도구 모음 확장 하 여 장치 및 방향 표시할](introduction-images/14-bottomtoolbarexpanded-vsmac.png "확장 하 여 장치 및 방향 표시할 아래쪽 도구 모음")](introduction-images/14-bottomtoolbarexpanded-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![아래쪽 도구 모음 확장 하 여 장치 및 방향 표시할](introduction-images/14-bottomtoolbarexpanded-vs.png "확장 하 여 장치 및 방향 표시할 아래쪽 도구 모음")](introduction-images/14-bottomtoolbarexpanded-vs-large.png#lightbox)

-----

선택 하면 장치 및 방향을 변경 하는 방법만 iOS 디자이너 디자인을 미리 봅니다. 현재 선택에 관계 없이 새로 추가 된 경우가 아니면 모든 장치 및 방향에서 제약 조건이 적용 됩니다는 **특성 편집** 다르게 지정 하는 데 사용 된 단추입니다.

때 [크기 클래스](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) 됩니다 [사용 하도록 설정](~/ios/user-interface/storyboards/unified-storyboards.md#enabling-size-classes)의 **특성 편집** 확장된 아래쪽 도구 모음의 단추가 나타납니다.  클릭 하는 **특성 편집** 단추 만들기, 선택한 장치, 방향 등을 나타내는 크기 클래스를 기반으로 하는 인터페이스 변형에 대 한 옵션을 표시 합니다. 다음 예제를 살펴보세요.

- 하는 경우 **iPhone SE** / **세로**에 선택한 팝 오버는 옵션을 제공 compact 너비, 높이 일반 크기 클래스는 인터페이스 변형 만들기. 
- 하는 경우 **iPad Pro 9.7 인치** / **가로** / **전체 화면** 는 선택한 팝 오버는 옵션을 제공 된 인터페이스 변형에 대 한 만들기 일반 너비, 높이 일반 크기 클래스입니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Size 클래스에서 인터페이스를 다를 사용 하 고 아래쪽 도구 모음의](introduction-images/15-edittraitsbutton-vsmac.png "크기 클래스에서 인터페이스를 다를 사용 하 고 아래쪽 도구 모음")](introduction-images/15-edittraitsbutton-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Size 클래스에서 인터페이스를 다를 사용 하 고 아래쪽 도구 모음의](introduction-images/15-edittraitsbutton-vs.png "크기 클래스에서 인터페이스를 다를 사용 하 고 아래쪽 도구 모음")](introduction-images/15-edittraitsbutton-vs-large.png#lightbox)

-----

#### <a name="zoom-controls"></a>확대/축소 컨트롤

디자인 화면에서는 몇 가지 컨트롤을 통해 확대/축소를 지원 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
![아래쪽 도구 모음에서 확대/축소 컨트롤](introduction-images/16-zoomcontrols-vsmac.png "아래쪽 도구 모음에서 확대/축소 컨트롤")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![아래쪽 도구 모음에서 확대/축소 컨트롤](introduction-images/16-zoomcontrols-vs.png "아래쪽 도구 모음에서 확대/축소 컨트롤")

-----

컨트롤은 다음과 같습니다.

1. 에 맞게 확대/축소
2. 축소
3. 확대
4. 실제 크기 (1:1 픽셀 크기)

이러한 컨트롤을 디자인 화면의 확대/축소를 조정 합니다. 런타임에 응용 프로그램의 사용자 인터페이스는 영향을 주지 않습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="properties-pad"></a>Properties Pad

사용 합니다 **Properties Pad** id, 비주얼 스타일, 내게 필요한 옵션 및 컨트롤의 동작을 편집 합니다. 다음 스크린샷에서 보여 줍니다 합니다 **Properties Pad** 단추에 대 한 옵션:

[![단추에 대 한 Properties Pad](introduction-images/17-buttonpropertiespad-vsmac.png "단추의 The Properties Pad")](introduction-images/17-buttonpropertiespad-vsmac-large.png#lightbox)
#### <a name="properties-pad-sections"></a>속성 패드 섹션

합니다 **Properties Pad** 세 가지 섹션이 포함 되어 있습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="properties-window"></a>속성 창

사용 합니다 **속성 창** id, 비주얼 스타일, 내게 필요한 옵션 및 컨트롤의 동작을 편집 합니다. 다음 스크린샷에서 보여 줍니다 합니다 **속성 창** 단추에 대 한 옵션:

[![단추에 대 한 속성 창](introduction-images/17-buttonpropertieswindow-vs.png "단추의 속성 창")](introduction-images/17-buttonpropertieswindow-vs-large.png#lightbox)

#### <a name="properties-window-sections"></a>속성 창 섹션

합니다 **속성 창** 세 가지 섹션이 포함 되어 있습니다.

-----

1.  **위젯** – 이름, 클래스, 스타일 속성 등과 같은 컨트롤의 기본 속성입니다. 컨트롤의 콘텐츠 관리에 대 한 속성 일반적으로 여기에 배치 됩니다.
2.  **레이아웃** – 속성 제약 조건 및 프레임을 포함 하는 컨트롤의 크기와 위치에 추적 하는 다음과 같습니다.
3.  **이벤트** – 이벤트 및 이벤트 처리기는 여기에 지정 합니다. 터치, 누르기, 끌기 등 이벤트를 처리 하는 데 유용 합니다. 코드에서 직접 이벤트를 처리할 수도 있습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

#### <a name="editing-properties-in-the-properties-pad"></a>Properties Pad에서 속성 편집

IOS 디자이너 디자인 화면에서 비주얼 편집 하는 것 외에도 속성을 편집을 지원 합니다 **Properties Pad**합니다. 아래 스크린샷에서 나온 것 처럼 선택된 된 컨트롤에 따라 사용 가능한 속성 변경:

[![속성 단추](introduction-images/18a-buttonpropertiespad-vsmac.png "속성 단추")](introduction-images/18a-buttonpropertiespad-vsmac-large.png#lightbox)

[![컨트롤러 속성 보기](introduction-images/18b-viewcontrollerpropertiespad-vsmac.png "컨트롤러 속성 보기")](introduction-images/18b-viewcontrollerpropertiespad-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

#### <a name="editing-properties-in-the-properties-window"></a>속성 창에서 속성 편집

IOS 디자이너 디자인 화면에서 비주얼 편집 하는 것 외에도 속성을 편집을 지원 합니다 **속성 창**합니다. 아래 스크린샷에서 나온 것 처럼 선택된 된 컨트롤에 따라 사용 가능한 속성 변경:

[![속성 단추](introduction-images/18a-buttonpropertieswindow-vs.png "속성 단추")](introduction-images/18a-buttonpropertieswindow-vs-large.png#lightbox)

[![컨트롤러 속성 보기](introduction-images/18b-viewcontrollerpropertieswindow-vs.png "컨트롤러 속성 보기")](introduction-images/18b-viewcontrollerpropertieswindow-vs-large.png#lightbox)

-----

> [!IMPORTANT]
> Properties Pad 이제의 Id 섹션을 **모듈** 필드입니다. 이 섹션에서는 Swift 클래스와의 상호 운용 하는 경우에 입력 해야 하는 합니다. 모듈 이름을 지정 하는 Swift 클래스에 대 한 입력을 사용 합니다.

#### <a name="default-values"></a>기본값

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

여러 속성을 **Properties Pad** 값 또는 기본값을 표시 합니다. 그러나 응용 프로그램의 코드는 이러한 값을 수정할 여전히 수 있습니다. 합니다 **Properties Pad** 코드에 설정 된 값이 표시 되지 않습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

여러 속성을 **속성 창** 값 또는 기본값을 표시 합니다. 그러나 응용 프로그램의 코드는 이러한 값을 수정할 여전히 수 있습니다. 합니다 **속성 창** 코드에 설정 된 값이 표시 되지 않습니다.

-----

#### <a name="event-handlers"></a>이벤트 처리기

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

다양 한 이벤트에 대 한 사용자 지정 이벤트 처리기를 지정 하려면 사용 합니다 **이벤트** 탭을 **Properties Pad**. 예를 들어 아래 스크린샷에서에 `HandleClick` 메서드 버튼의 처리 **Touch 구성 내에서** 이벤트:

[![단추에 대해 설정 하는 이벤트 처리기를 사용 하 여는 속성 패드](introduction-images/19-buttonpropertiespadevents-vsmac.png "단추에 대해 설정 하는 이벤트 처리기를 사용 하 여 The Properties Pad")](introduction-images/19-buttonpropertiespadevents-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

다양 한 이벤트에 대 한 사용자 지정 이벤트 처리기를 지정 하려면 사용 합니다 **이벤트** 탭을 **속성 창**. 예를 들어 아래 스크린샷에서에 `HandleClick` 메서드 버튼의 처리 **Touch 구성 내에서** 이벤트:

[![단추에 대해 설정 하는 이벤트 처리기를 사용 하 여 속성 창](introduction-images/19-buttonpropertieswindowevents-vs.png "단추에 대해 설정 하는 이벤트 처리기의 속성 창")](introduction-images/19-buttonpropertieswindowevents-vs-large.png#lightbox)

-----

이벤트 처리기가 지정 된 후 동일한 이름의 메서드는 해당 뷰 컨트롤러 클래스에 추가 되어야 합니다. 그렇지 않은 경우는 `unrecognized selector` 단추를 탭 할 때 예외가 발생 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![인식할 수 없는 선택기 예외가](introduction-images/20-unrecognizedselector-vsmac.png "인식할 수 없는 선택기 예외")](introduction-images/20-unrecognizedselector-vsmac-large.png#lightbox)

에 지정 된 이벤트 처리기 뒤에 **Properties Pad**, iOS 디자이너는 즉시 해당 코드 파일을 열고 메서드 선언을 삽입할 제공 합니다. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![인식할 수 없는 선택기 예외가](introduction-images/20-unrecognizedselector-vs.png "인식할 수 없는 선택기 예외")](introduction-images/20-unrecognizedselector-vs-large.png#lightbox)

-----

사용자 지정 이벤트 처리기를 사용 하는 예제를 참조 합니다 [Hello, iOS Getting Started Guide](~/ios/get-started/hello-ios/index.md)합니다.

### <a name="outline-view"></a>개요 보기

IOS 디자이너는 윤곽선으로 컨트롤의 인터페이스 계층 구조를 표시할 수도 있습니다. 선택 하 여 윤곽선을 사용할 수는 **문서 개요** 아래와 같이 탭:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![문서 개요](introduction-images/21-buttonoutlineview-vsmac.png "문서 개요")](introduction-images/21-buttonoutlineview-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![문서 개요](introduction-images/21-buttonoutlineview-vs.png "문서 개요")](introduction-images/21-buttonoutlineview-vs-large.png#lightbox)

-----

개요 뷰에서 선택 된 컨트롤에 디자인 화면에서 선택한 컨트롤을 사용 하 여 동기화 상태로 유지 됩니다.  이 기능은 깊이 중첩 된 인터페이스 계층 구조에서 항목을 선택할 때 유용 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="revert-to-xcode"></a>Xcode를 되돌리려면

IOS 디자이너 및 Xcode Interface Builder를 서로 교환해 서 사용할 수는 것입니다. Xcode Interface Builder 스토리 보드 또는.xib 파일을 열려면에서 마우스 오른쪽 단추로 클릭는 파일 및 선택 **프로그램 > Xcode Interface Builder**아래 스크린샷에 나온 것 처럼,:

[![Xcode Interface Builder에서 스토리 보드 열기](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "Xcode Interface Builder에서 스토리 보드 열기")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png#lightbox)

Xcode Interface Builder에서 편집한 후 파일을 저장 하 고 mac 용 Visual Studio로 돌아가서 Xamarin.iOS 프로젝트에 변경 내용을 동기화 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-----

## <a name="xib-support"></a>.xib 지원

IOS 디자이너는 만들기, 편집 및.xib 파일 관리를 지원 합니다. 이러한 응용 프로그램의 뷰 계층 구조에 추가할 수 있는 해당 폐쇄형 단일, 사용자 지정 뷰는 XML 파일입니다. 스토리 보드를 나타내고, 여러 화면 간 전환을 반면.xib 파일에서 일반적으로 단일 뷰 또는 응용 프로그램에서 화면에 대 한 인터페이스를 나타냅니다.

많은 의견에 대 한 솔루션.xib 파일, 스토리 보드 또는 코드 생성 및 사용자 인터페이스를 유지 관리에 대 한 가장 적합 한 가지가 있습니다. 실제로 완벽 한 방법이 없는 것으로 항상 당면한 작업에 대 한 가장 적합 한 도구를 고려 합니다. 즉,.xib 파일은 같은 사용자 지정 테이블 뷰 셀을 앱에서 여러 위치에 필요한 사용자 지정 보기를 나타내는 데 사용 하는 경우 일반적으로 가장 강력 합니다. 

.Xib 파일을 사용 하는 방법에 대 한 자세한 설명서 레시피를 다음에서 찾을 수 있습니다.

- [.Xib 템플릿 보기를 사용 하 여](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/using_the_ios_view_xib_template)
- [.Xib을 사용 하는 사용자 지정 TableViewCell 만들기](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/tables/custom-tableviewcell)
- [시작 화면을.xib을 사용 하 여 만들기](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/launchscreen-xib)

스토리 보드의 사용과 관련 된 자세한 내용은 참조는 [스토리 보드 소개](~/ios/user-interface/storyboards/index.md)합니다.

이 및 다른 iOS 디자이너 관련 가이드 대부분의 Xamarin.iOS 새 프로젝트 템플릿은 기본적으로 스토리 보드를 제공 하므로 사용자 인터페이스를 구축 하기 위한 표준 접근 방식으로 스토리 보드의 사용을 참조 하십시오.

## <a name="summary"></a>요약

이 가이드에서는 iOS 디자이너에서 해당 기능을 설명 하 고 멋진 사용자 인터페이스 디자인에 대 한 제공 된 도구 개요에 대 한 소개를 제공 합니다.

## <a name="related-links"></a>관련 링크

- [Storyboards 소개](~/ios/user-interface/storyboards/index.md)
- [iOS 디자인할 수 있는 컨트롤 연습](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, iOS 멀티스크린](~/ios/get-started/hello-ios-multiscreen/index.md)
- [Android 디자이너 개요](~/android/user-interface/android-designer/index.md)
- [Partial 클래스 및 메서드](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)
- [IOS-발전 2014 (비디오)에 대 한 Xamarin 디자이너 살펴보기](https://www.youtube.com/watch?v=W4H9uLjoEjM)
- [IOS 디자이너를 사용 하 여 화면을 만드는 시작 (비디오)](https://university.xamarin.com/lightninglectures/using-the-ios-designer-to-create-a-launch-screen)
