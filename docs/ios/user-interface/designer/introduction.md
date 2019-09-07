---
title: iOS 디자이너 기본 사항
description: 이 가이드에서는 Xamarin Designer for iOS를 소개 합니다. IOS 디자이너를 사용 하 여 컨트롤을 시각적으로 레이아웃 하는 방법, 코드에서 해당 컨트롤에 액세스 하는 방법 및 속성을 편집 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: E7045E41-0DEF-416B-BCDB-52502350F61C
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 01/31/2018
ms.openlocfilehash: fa772add96eb17b0a80470210f42b4d9df220a9c
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768933"
---
# <a name="ios-designer-basics"></a>iOS 디자이너 기본 사항

_이 가이드에서는 Xamarin Designer for iOS를 소개 합니다. IOS 디자이너를 사용 하 여 컨트롤을 시각적으로 레이아웃 하는 방법, 코드에서 해당 컨트롤에 액세스 하는 방법 및 속성을 편집 하는 방법을 보여 줍니다._

Xamarin Designer for iOS는 Xcode의 Interface Builder 및 Android Designer와 유사한 시각적 인터페이스 디자이너입니다. 이러한 기능 중 일부에는 Windows 및 Mac 용 Visual Studio의 원활한 통합, 끌어서 놓기 편집 기능, 이벤트 처리기를 설정 하기 위한 인터페이스 및 사용자 지정 컨트롤을 렌더링 하는 기능이 포함 되어 있습니다.

## <a name="requirements"></a>요구 사항

IOS Designer는 Windows의 Mac용 Visual Studio 및 Visual Studio 2017 이상에서 사용할 수 있습니다. Windows 용 Visual Studio에서 iOS Designer는 Xcode가 실행 되지 않아도 되지만 제대로 구성 된 Mac 빌드 호스트에 연결 해야 합니다.

이 가이드에서는 [시작 가이드](~/ios/get-started/index.md)에 설명 된 내용에 대해 잘 알고 있다고 가정 합니다.

<a name="how-it-works" />

## <a name="how-the-ios-designer-works"></a>IOS Designer 작동 방법

이 섹션에서는 iOS Designer를 사용 하 여 사용자 인터페이스를 만들고 코드에 연결 하는 방법을 설명 합니다.

개발자는 iOS Designer를 사용 하 여 응용 프로그램의 사용자 인터페이스를 시각적으로 디자인할 수 있습니다. 스토리 보드 [소개](~/ios/user-interface/storyboards/index.md) 가이드에 설명 된 대로 storyboard는 앱을 구성 하는 화면 (보기 컨트롤러), 해당 뷰 컨트롤러에 배치 되는 인터페이스 요소 (뷰) 및 앱의 전반적인 탐색 흐름을 설명 합니다. 

뷰 컨트롤러에는 iOS 디자이너의 시각적 표현과 연결 된 C# 클래스의 두 부분이 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![IOS 디자이너의 뷰 컨트롤러](introduction-images/1-storyboardwithviewcontroller-vsmac.png "IOS 디자이너의 뷰 컨트롤러")](introduction-images/1-storyboardwithviewcontroller-vsmac-large.png#lightbox)

[![뷰 컨트롤러에 대 한 코드입니다] . (introduction-images/2-viewcontrollercode-vsmac.png "뷰 컨트롤러에 대 한 코드입니다") .](introduction-images/2-viewcontrollercode-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![IOS 디자이너의 뷰 컨트롤러](introduction-images/1-storyboardwithviewcontroller-vs.png "IOS 디자이너의 뷰 컨트롤러")](introduction-images/1-storyboardwithviewcontroller-vs-large.png#lightbox)

[![뷰 컨트롤러에 대 한 코드입니다] . (introduction-images/2-viewcontrollercode-vs.png "뷰 컨트롤러에 대 한 코드입니다") .](introduction-images/2-viewcontrollercode-vs-large.png#lightbox)

-----

기본 상태에서 뷰 컨트롤러는 기능을 제공 하지 않습니다. 컨트롤로 채워야 합니다. 이러한 컨트롤은 화면 내용이 모두 들어 있는 사각형 영역인 뷰 컨트롤러의 보기에 배치 됩니다. 대부분의 뷰 컨트롤러에는 단추가 포함 된 뷰 컨트롤러를 보여 주는 다음 스크린샷에 설명 된 것 처럼 단추, 레이블 및 텍스트 필드와 같은 일반적인 컨트롤이 포함 되어 있습니다. 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![단추를 포함 하는 뷰 컨트롤러](introduction-images/3-viewcontrollerwithbutton-vsmac.png "단추를 포함 하는 뷰 컨트롤러")](introduction-images/3-viewcontrollerwithbutton-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![단추를 포함 하는 뷰 컨트롤러](introduction-images/3-viewcontrollerwithbutton-vs.png "단추를 포함 하는 뷰 컨트롤러")](introduction-images/3-viewcontrollerwithbutton-vs-large.png#lightbox)

-----

정적 텍스트를 포함 하는 레이블과 같은 일부 컨트롤은 뷰 컨트롤러 및 왼쪽에만 추가할 수 있습니다. 그러나 컨트롤을 프로그래밍 방식으로 사용자 지정 해야 하는 경우가 많습니다. 예를 들어 위에 추가 된 단추는 탭 할 때 어떤 작업을 수행 해야 하므로 이벤트 처리기를 코드에 추가 해야 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

코드에서 단추를 액세스 하 고 조작 하려면 고유 식별자가 있어야 합니다. 단추를 선택 하 고 **Properties Pad**를 연 다음 **이름** 필드를 "submitbutton"과 같은 값으로 설정 하 여 고유 식별자를 제공 합니다.

[![Properties Pad에서 단추의 이름 설정](introduction-images/4-settingbuttonname-vsmac.png "Properties Pad에서 단추의 이름 설정")](introduction-images/4-settingbuttonname-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

코드에서 단추를 액세스 하 고 조작 하려면 고유 식별자가 있어야 합니다. 단추를 선택 하 고 **속성 창을**연 다음 **이름** 필드를 "submitbutton"과 같은 값으로 설정 하 여 고유 식별자를 제공 합니다.

[![속성 창에서 단추의 이름 설정](introduction-images/4-settingbuttonname-vs.png "속성 창에서 단추의 이름 설정")](introduction-images/4-settingbuttonname-vs-large.png#lightbox)

-----

이제 단추에 이름이 있으므로 코드에서 액세스할 수 있습니다. 그러나 어떻게 작동 하나요?

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

**Solution Pad**에서 **ViewController.cs** 로 이동 하 고 노출 표시기를 클릭 하면 뷰 컨트롤러의 `ViewController` 클래스 정의가 각각 [partial 클래스](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) 정의를 포함 하는 두 개의 파일로 확장 됩니다.

[![ViewController 클래스를 구성 하는 두 파일은 다음과 같습니다. ViewController.cs 및 ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vsmac.png "는 viewcontroller 클래스를 구성 하는 두 파일을 만듭니다. ViewController.cs 및 ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

**솔루션 탐색기**에서 **ViewController.cs** 으로 이동 하 고 노출 표시기를 클릭 하면 뷰 컨트롤러의 `ViewController` 클래스 정의가 각각 [partial 클래스](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) 를 포함 하는 두 개의 파일로 확장 됨을 나타냅니다. 정의

[![ViewController 클래스를 구성 하는 두 파일은 다음과 같습니다. ViewController.cs 및 ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vs.png "는 viewcontroller 클래스를 구성 하는 두 파일을 만듭니다. ViewController.cs 및 ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vs-large.png#lightbox)

-----

- **ViewController.cs** 는 `ViewController` 클래스와 관련 된 사용자 지정 코드로 채워야 합니다. 이 파일에서 클래스는 `ViewController` 다양 한 iOS 뷰 컨트롤러 수명 주기 메서드에 응답 하 고, UI를 사용자 지정 하 고, 단추 탭과 같은 사용자 입력에 응답할 수 있습니다.

- **ViewController.designer.cs** 는 iOS 디자이너에서 코드에 시각적으로 생성 된 인터페이스를 매핑하기 위해 만든 생성 된 파일입니다. 이 파일의 변경 내용은 덮어쓰므로 수정 하면 안 됩니다. 이 파일의 속성 선언을 사용 하면 `ViewController` 클래스의 코드가 iOS 디자이너에서 설정 된 컨트롤을 **이름**으로 사용 하 여 액세스할 수 있습니다. **ViewController.designer.cs** 를 열면 다음 코드가 표시 됩니다.

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

속성 `SubmitButton` 선언은 **ViewController.designer.cs** 파일 뿐만 아니라 `ViewController` 전체 클래스를 storyboard에 정의 된 단추에 연결 합니다. **ViewController.cs** 는 `ViewController` 클래스의 일부를 정의 하기 때문에에 액세스할 `SubmitButton`수 있습니다.

다음 스크린샷에서 IntelliSense는 이제 **ViewController.cs**에서 참조를 `SubmitButton` 인식 한다는 것을 보여 줍니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![SubmitButton 참조를 인식 하는 IntelliSense](introduction-images/6-submitbuttonintellisense-vsmac.png "SubmitButton 참조를 인식 하는 IntelliSense")](introduction-images/6-submitbuttonintellisense-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![SubmitButton 참조를 인식 하는 IntelliSense](introduction-images/6-submitbuttonintellisense-vs.png "SubmitButton 참조를 인식 하는 IntelliSense")](introduction-images/6-submitbuttonintellisense-vs-large.png#lightbox)

-----

이 섹션에서는 iOS 디자이너에서 단추를 만들고 코드에서 해당 단추에 액세스 하는 방법을 보여 주었습니다.

이 문서의 나머지 부분에서는 iOS 디자이너에 대 한 자세한 개요를 제공 합니다.

## <a name="ios-designer-basics"></a>iOS 디자이너 기본 사항

이 섹션에서는 iOS 디자이너의 일부를 소개 하 고 해당 기능 둘러보기를 제공 합니다.

### <a name="launching-the-ios-designer"></a>IOS Designer 시작

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac용 Visual Studio를 사용 하 여 만든 xamarin.ios 프로젝트에는 storyboard가 포함 됩니다. 스토리 보드의 내용을 보려면 **Solution Pad**에서 storyboard 파일을 두 번 클릭 합니다.

[![IOS 디자이너에서 열려 있는 스토리 보드](introduction-images/7-storyboardopen-vsmac.png "IOS 디자이너에서 열려 있는 스토리 보드")](introduction-images/7-storyboardopen-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio를 사용 하 여 만든 대부분의 Xamarin.ios 프로젝트에는 storyboard가 포함 됩니다. 스토리 보드의 내용을 보려면 **솔루션 탐색기**에서 storyboard 파일을 두 번 클릭 합니다.

[![IOS 디자이너에서 열려 있는 스토리 보드](introduction-images/7-storyboardopen-vs.png "IOS 디자이너에서 열려 있는 스토리 보드")](introduction-images/7-storyboardopen-vs-large.png#lightbox)

-----

<a name="iOS_Designer_features"/>

### <a name="ios-designer-features"></a>iOS Designer 기능

IOS 디자이너에는 다음과 같은 6 개의 주요 섹션이 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![IOS 디자이너의 섹션](introduction-images/8-sixpartsofiosdesigner-vsmac.png "IOS 디자이너의 섹션")](introduction-images/8-sixpartsofiosdesigner-vsmac-large.png#lightbox)

1. **Design Surface** – iOS 디자이너의 기본 작업 영역입니다. 문서 영역에 표시 되는 사용자 인터페이스의 시각적 생성을 활성화 합니다.
2. **제약 조건 도구 모음** – 사용자 인터페이스에서 요소를 배치 하는 두 가지 다른 방법으로 프레임 편집 모드와 제약 조건 편집 모드 간을 전환할 수 있습니다.
3. **도구 상자** – 디자인 화면으로 끌어 오고 사용자 인터페이스에 추가할 수 있는 컨트롤러, 개체, 컨트롤, 데이터 뷰, 제스처 인식기, 창 및 막대를 표시 합니다.
4. **Properties Pad** – id, 비주얼 스타일, 접근성, 레이아웃 및 동작을 포함 하 여 선택한 컨트롤의 속성을 표시 합니다.
5. **문서 개요** – 편집 중인 인터페이스에 대 한 레이아웃을 구성 하는 컨트롤의 트리를 표시 합니다. 트리의 항목을 클릭 하면 iOS 디자이너에서 항목을 선택 하 고 **Properties Pad**에 해당 속성을 표시 합니다. 이 기능은 깊게 중첩 된 사용자 인터페이스에서 특정 컨트롤을 선택 하는 데 유용 합니다.
6. **아래쪽 도구 모음** – iOS Designer에서 장치, 방향 및 확대/축소를 포함 하 여 storyboard 또는. xib 파일을 표시 하는 방법을 변경 하는 옵션을 포함 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![IOS 디자이너의 섹션](introduction-images/8-sixpartsofiosdesigner-vs.png "IOS 디자이너의 섹션")](introduction-images/8-sixpartsofiosdesigner-vs-large.png#lightbox)

1. **Design Surface** – iOS 디자이너의 기본 작업 영역입니다. 문서 영역에 표시 되는 사용자 인터페이스의 시각적 생성을 활성화 합니다.
2. **제약 조건 도구 모음** – 사용자 인터페이스에서 요소를 배치 하는 두 가지 다른 방법으로 프레임 편집 모드와 제약 조건 편집 모드 간을 전환할 수 있습니다.
3. **도구 상자** – 디자인 화면으로 끌어 오고 사용자 인터페이스에 추가할 수 있는 컨트롤러, 개체, 컨트롤, 데이터 뷰, 제스처 인식기, 창 및 막대를 표시 합니다.
4. **속성 창** -id, 비주얼 스타일, 접근성, 레이아웃 및 동작을 포함 하 여 선택한 컨트롤의 속성을 표시 합니다.
5. **문서 개요** – 편집 중인 인터페이스에 대 한 레이아웃을 구성 하는 컨트롤의 트리를 표시 합니다. 트리의 항목을 클릭 하면 iOS 디자이너에서 항목을 선택 하 고 **속성 창**에 해당 속성을 표시 합니다. 이 기능은 깊게 중첩 된 사용자 인터페이스에서 특정 컨트롤을 선택 하는 데 유용 합니다.
6. **아래쪽 도구 모음** – iOS Designer에서 장치, 방향 및 확대/축소를 포함 하 여 storyboard 또는. xib 파일을 표시 하는 방법을 변경 하는 옵션을 포함 합니다.

-----

### <a name="design-workflow"></a>디자인 워크플로

#### <a name="adding-a-control-to-the-interface"></a>인터페이스에 컨트롤 추가

인터페이스에 컨트롤을 추가 하려면 **도구 상자** 에서 컨트롤을 끌어 디자인 화면에 놓습니다. 세로 및 가로 안내선은 컨트롤을 추가 하거나 위치를 지정 하는 경우 세로 가운데, 가로 가운데 및 여백과 같이 일반적으로 사용 되는 레이아웃 위치를 강조 표시 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![디자인 화면에서 지침은 일반적으로 사용 되는 레이아웃 위치를 강조 표시 합니다] . (introduction-images/9-layoutguides-vsmac.png "디자인 화면에서 지침은 일반적으로 사용 되는 레이아웃 위치를 강조 표시 합니다") .

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![디자인 화면에서 지침은 일반적으로 사용 되는 레이아웃 위치를 강조 표시 합니다] . (introduction-images/9-layoutguides-vs.png "디자인 화면에서 지침은 일반적으로 사용 되는 레이아웃 위치를 강조 표시 합니다") .

-----

위 예제의 파란색 점선은 단추 배치에 도움이 되는 가로 가운데 시각적 맞춤 안내선을 제공 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

#### <a name="context-menu-commands"></a>상황에 맞는 메뉴 명령

상황에 맞는 메뉴는 디자인 화면과 **문서 개요**에서 모두 사용할 수 있습니다. 이 메뉴는 선택한 컨트롤과 부모에 대 한 명령을 제공 합니다 .이 명령은 중첩 된 계층의 뷰를 사용 하는 경우에 유용 합니다.

[![디자인 화면의 상황에 맞는 메뉴](introduction-images/10-contextmenudesignsurface-vsmac.png "디자인 화면의 상황에 맞는 메뉴")](introduction-images/10-contextmenudesignsurface-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

-----

### <a name="constraints-toolbar"></a>제약 조건 도구 모음

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![제약 조건 도구 모음](introduction-images/11-constraintstoolbar-vsmac.png "제약 조건 도구 모음")](introduction-images/11-constraintstoolbar-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![제약 조건 도구 모음](introduction-images/11-constraintstoolbar-vs.png "제약 조건 도구 모음")](introduction-images/11-constraintstoolbar-vs-large.png#lightbox)

-----

제약 조건 도구 모음이 업데이트 되어 이제 프레임 편집 모드/제약 조건 편집 모드 설정/해제 및 업데이트 제약 조건/프레임 업데이트 단추 라는 두 개의 컨트롤로 구성 됩니다.

#### <a name="frame-editing-mode--constraint-editing-mode-toggle"></a>프레임 편집 모드/제약 조건 편집 모드 설정/해제

이전 버전의 iOS 디자이너에서는 디자인 화면에서 프레임 편집 모드와 제약 조건 편집 모드 간에 전환 된 이미 선택 된 뷰를 클릭 합니다. 이제 제약 조건 도구 모음의 토글 컨트롤이 이러한 편집 모드 사이를 전환 합니다.

- 프레임 편집 모드:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![프레임 편집 모드 단추](introduction-images/12a-frameeditingmode-vsmac.png "프레임 편집 모드 단추")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![프레임 편집 모드 단추](introduction-images/12a-frameeditingmode-vs.png "프레임 편집 모드 단추")

-----

- 제약 조건 편집 모드:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![제약 조건 편집 모드 단추](introduction-images/12b-constrainteditingmode-vsmac.png "제약 조건 편집 모드 단추")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![제약 조건 편집 모드 단추](introduction-images/12b-constrainteditingmode-vs.png "제약 조건 편집 모드 단추")

-----

#### <a name="update-constraints--update-frames-button"></a>제약 조건 업데이트/프레임 업데이트 단추

업데이트 제약 조건/프레임 업데이트 단추는 프레임 편집 모드/제약 조건 편집 모드 토글의 오른쪽에 있습니다.

- 프레임 편집 모드에서이 단추를 클릭 하면 선택한 요소의 프레임이 해당 제약 조건과 일치 하도록 조정 됩니다.
- 제약 조건 편집 모드에서이 단추를 클릭 하면 선택한 요소의 제약 조건이 프레임에 맞게 조정 됩니다.

### <a name="bottom-toolbar"></a>아래쪽 도구 모음

아래쪽 도구 모음은 iOS 디자이너에서 storyboard 또는. xib 파일을 보는 데 사용 되는 장치, 방향 및 확대/축소를 선택 하는 방법을 제공 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![디자인 화면에 대 한 장치 및 방향을 선택 하는 데 사용 되는 아래쪽 도구 모음](introduction-images/13-bottomtoolbar-vsmac.png "디자인 화면에 대 한 장치 및 방향을 선택 하는 데 사용 되는 아래쪽 도구 모음")](introduction-images/13-bottomtoolbar-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![디자인 화면에 대 한 장치 및 방향을 선택 하는 데 사용 되는 아래쪽 도구 모음](introduction-images/13-bottomtoolbar-vs.png "디자인 화면에 대 한 장치 및 방향을 선택 하는 데 사용 되는 아래쪽 도구 모음")](introduction-images/13-bottomtoolbar-vs-large.png#lightbox)

-----

#### <a name="device-and-orientation"></a>장치 및 방향

확장 하면 아래쪽 도구 모음에 현재 문서에 적용 가능한 모든 장치, 방향 및/또는 adaptation가 표시 됩니다. 이 단추를 클릭 하면 디자인 화면에 표시 되는 뷰가 변경 됩니다. 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![장치 및 방향을 표시 하도록 확장 된 아래쪽 도구 모음](introduction-images/14-bottomtoolbarexpanded-vsmac.png "장치 및 방향을 표시 하도록 확장 된 아래쪽 도구 모음")](introduction-images/14-bottomtoolbarexpanded-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![장치 및 방향을 표시 하도록 확장 된 아래쪽 도구 모음](introduction-images/14-bottomtoolbarexpanded-vs.png "장치 및 방향을 표시 하도록 확장 된 아래쪽 도구 모음")](introduction-images/14-bottomtoolbarexpanded-vs-large.png#lightbox)

-----

장치 및 방향 선택은 iOS 디자이너가 디자인을 미리 보는 방법에만 적용 됩니다. **특성 편집** 단추를 사용 하 여 달리 지정 하지 않는 한, 현재 선택 항목에 관계 없이 새로 추가 된 제약 조건이 모든 장치 및 방향에 적용 됩니다.

[Size 클래스](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) 를 [사용](~/ios/user-interface/storyboards/unified-storyboards.md#enabling-size-classes)하는 경우 **특성 편집** 단추가 확장 된 아래쪽 도구 모음에 표시 됩니다.  **특성 편집** 단추를 클릭 하면 선택한 장치와 방향이 나타내는 size 클래스에 따라 인터페이스 변형을 만드는 옵션이 표시 됩니다. 다음 예를 참조하세요.

- **IPhone SE** / **세로**를 선택 하는 경우 팝 오버는 컴팩트 너비, 일반 높이 크기 클래스에 대 한 인터페이스 변형을 만드는 옵션을 제공 합니다. 
- **IPad Pro 9.7 "**  / **가로** / **전체 화면** 을 선택 하면 팝 오버는 일반 너비, 일반 높이 크기 클래스에 대 한 인터페이스 변형을 만드는 옵션을 제공 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![크기 클래스 별로 인터페이스를 변경 하는 데 사용 되는 아래쪽 도구 모음](introduction-images/15-edittraitsbutton-vsmac.png "크기 클래스 별로 인터페이스를 변경 하는 데 사용 되는 아래쪽 도구 모음")](introduction-images/15-edittraitsbutton-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![크기 클래스 별로 인터페이스를 변경 하는 데 사용 되는 아래쪽 도구 모음](introduction-images/15-edittraitsbutton-vs.png "크기 클래스 별로 인터페이스를 변경 하는 데 사용 되는 아래쪽 도구 모음")](introduction-images/15-edittraitsbutton-vs-large.png#lightbox)

-----

#### <a name="zoom-controls"></a>확대/축소 컨트롤

디자인 화면에서는 여러 컨트롤을 통한 확대/축소를 지원 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![아래쪽 도구 모음의 확대/축소 컨트롤](introduction-images/16-zoomcontrols-vsmac.png "아래쪽 도구 모음의 확대/축소 컨트롤")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![아래쪽 도구 모음의 확대/축소 컨트롤](introduction-images/16-zoomcontrols-vs.png "아래쪽 도구 모음의 확대/축소 컨트롤")

-----

컨트롤에는 다음이 포함 됩니다.

1. 크기에 맞게
2. 축소
3. 확대
4. 실제 크기 (1:1 픽셀 크기)

이러한 컨트롤은 디자인 화면의 확대/축소를 조정 합니다. 런타임에 응용 프로그램의 사용자 인터페이스에는 영향을 주지 않습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

### <a name="properties-pad"></a>Properties Pad

**Properties Pad** 를 사용 하 여 컨트롤의 id, 비주얼 스타일, 접근성 및 동작을 편집 합니다. 다음 스크린샷은 단추에 대 한 **Properties Pad** 옵션을 보여 줍니다.

[![단추에 대 한 Properties Pad입니다] . (introduction-images/17-buttonpropertiespad-vsmac.png "단추에 대 한 Properties Pad입니다") .](introduction-images/17-buttonpropertiespad-vsmac-large.png#lightbox)
#### <a name="properties-pad-sections"></a>Properties Pad 섹션

이 **Properties Pad** 에는 다음과 같은 세 개의 섹션이 있습니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

### <a name="properties-window"></a>속성 창

**속성 창을** 사용 하 여 컨트롤의 id, 비주얼 스타일, 접근성 및 동작을 편집할 수 있습니다. 다음 스크린샷은 단추의 **속성 창** 옵션을 보여 줍니다.

[![단추에 대 한 속성 창](introduction-images/17-buttonpropertieswindow-vs.png "단추에 대 한 속성 창")](introduction-images/17-buttonpropertieswindow-vs-large.png#lightbox)

#### <a name="properties-window-sections"></a>속성 창 섹션

**속성 창** 에는 다음과 같은 세 개의 섹션이 있습니다.

-----

1. **위젯** – 컨트롤의 주요 속성 (예: 이름, 클래스, 스타일 속성 등)입니다. 컨트롤의 콘텐츠를 관리 하기 위한 속성은 일반적으로 여기에 배치 됩니다.
2. **레이아웃** – 제약 조건 및 프레임을 포함 하 여 컨트롤의 위치 및 크기를 추적 하는 속성은 여기에 나열 되어 있습니다.
3. **이벤트** – 이벤트 및 이벤트 처리기가 여기에 지정 됩니다. Touch, 탭핑, drag 등의 이벤트를 처리 하는 데 유용 합니다. 이벤트는 코드에서 직접 처리할 수도 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

#### <a name="editing-properties-in-the-properties-pad"></a>Properties Pad에서 속성 편집

IOS 디자이너는 디자인 화면에서의 시각적 편집 외에도 **Properties Pad**의 속성 편집을 지원 합니다. 사용 가능한 속성은 아래 스크린샷에 표시 된 것 처럼 선택 된 컨트롤에 따라 변경 됩니다.

[![단추 속성](introduction-images/18a-buttonpropertiespad-vsmac.png "단추 속성")](introduction-images/18a-buttonpropertiespad-vsmac-large.png#lightbox)

[![컨트롤러 속성 보기](introduction-images/18b-viewcontrollerpropertiespad-vsmac.png "컨트롤러 속성 보기")](introduction-images/18b-viewcontrollerpropertiespad-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

#### <a name="editing-properties-in-the-properties-window"></a>속성 창에서 속성 편집

IOS 디자이너는 디자인 화면에서의 시각적 편집 외에도 **속성 창**에서 속성 편집을 지원 합니다. 사용 가능한 속성은 아래 스크린샷에 표시 된 것 처럼 선택 된 컨트롤에 따라 변경 됩니다.

[![단추 속성](introduction-images/18a-buttonpropertieswindow-vs.png "단추 속성")](introduction-images/18a-buttonpropertieswindow-vs-large.png#lightbox)

[![컨트롤러 속성 보기](introduction-images/18b-viewcontrollerpropertieswindow-vs.png "컨트롤러 속성 보기")](introduction-images/18b-viewcontrollerpropertieswindow-vs-large.png#lightbox)

-----

> [!IMPORTANT]
> 이제 Properties Pad의 Identity 섹션에 **모듈** 필드가 표시 됩니다. Swift 클래스와 상호 운용 하는 경우에만이 섹션을 채워야 합니다. 이를 사용 하 여 Swift 클래스에 대 한 모듈 이름 (namespaced)을 입력 합니다.

#### <a name="default-values"></a>기본값

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

**Properties Pad** 의 많은 속성은 값 또는 기본값을 표시 하지 않습니다. 그러나 응용 프로그램 코드는 이러한 값을 계속 수정할 수 있습니다. **Properties Pad** 코드에 설정 된 값을 표시 하지 않습니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

**속성 창의** 속성 대부분은 값 또는 기본값을 표시 하지 않습니다. 그러나 응용 프로그램 코드는 이러한 값을 계속 수정할 수 있습니다. **속성 창** 에는 코드에 설정 된 값이 표시 되지 않습니다.

-----

#### <a name="event-handlers"></a>이벤트 처리기

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

여러 이벤트에 대 한 사용자 지정 이벤트 처리기를 지정 하려면 **Properties Pad**의 **이벤트** 탭을 사용 합니다. 예를 들어 아래 스크린샷에서 메서드는 `HandleClick` 이벤트 내에서 단추의 **터치** 를 처리 합니다.

[![단추에 대 한 이벤트 처리기가 설정 된 Properties Pad](introduction-images/19-buttonpropertiespadevents-vsmac.png "단추에 대 한 이벤트 처리기가 설정 된 Properties Pad")](introduction-images/19-buttonpropertiespadevents-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

여러 이벤트에 대 한 사용자 지정 이벤트 처리기를 지정 하려면 **속성 창의** **이벤트** 탭을 사용 합니다. 예를 들어 아래 스크린샷에서 메서드는 `HandleClick` 이벤트 내에서 단추의 **터치** 를 처리 합니다.

[![단추에 대 한 이벤트 처리기가 설정 된 속성 창](introduction-images/19-buttonpropertieswindowevents-vs.png "단추에 대 한 이벤트 처리기가 설정 된 속성 창")](introduction-images/19-buttonpropertieswindowevents-vs-large.png#lightbox)

-----

이벤트 처리기를 지정한 후에는 동일한 이름의 메서드를 해당 뷰 컨트롤러 클래스에 추가 해야 합니다. 그렇지 않으면 단추를 누를 때 예외가발생합니다.`unrecognized selector`

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![인식할 수 없는 선택기 예외] 입니다. (introduction-images/20-unrecognizedselector-vsmac.png "인식할 수 없는 선택기 예외") 입니다.](introduction-images/20-unrecognizedselector-vsmac-large.png#lightbox)

**Properties Pad**에서 이벤트 처리기를 지정한 후에는 iOS Designer가 해당 코드 파일을 즉시 열고 메서드 선언을 삽입 합니다. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![인식할 수 없는 선택기 예외] 입니다. (introduction-images/20-unrecognizedselector-vs.png "인식할 수 없는 선택기 예외") 입니다.](introduction-images/20-unrecognizedselector-vs-large.png#lightbox)

-----

사용자 지정 이벤트 처리기를 사용 하는 예제는 [Hello, IOS 시작 가이드](~/ios/get-started/hello-ios/index.md)를 참조 하세요.

### <a name="outline-view"></a>개요 보기

IOS 디자이너는 인터페이스의 컨트롤 계층 구조를 윤곽선으로 표시할 수도 있습니다. 개요는 아래와 같이 **문서 개요** 탭을 선택 하 여 사용할 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![문서 개요](introduction-images/21-buttonoutlineview-vsmac.png "문서 개요")](introduction-images/21-buttonoutlineview-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![문서 개요](introduction-images/21-buttonoutlineview-vs.png "문서 개요")](introduction-images/21-buttonoutlineview-vs-large.png#lightbox)

-----

개요 뷰에서 선택한 컨트롤이 디자인 화면에서 선택한 컨트롤과 동기화 된 상태로 유지 됩니다.  이 기능은 깊게 중첩 된 인터페이스 계층 구조에서 항목을 선택 하는 데 유용 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="revert-to-xcode"></a>Xcode로 되돌리기

IOS 디자이너와 Xcode Interface Builder를 교대로 사용할 수 있습니다. Xcode Interface Builder에서 storyboard 또는 xib 파일을 열려면 파일을 마우스 오른쪽 단추로 클릭 하 고 아래 스크린샷에 표시 된 것 처럼 **> Xcode Interface Builder를 사용 하 여 열기**를 선택 합니다.

[![Xcode Interface Builder에서 스토리 보드 열기](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "Xcode Interface Builder에서 스토리 보드 열기")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png#lightbox)

Xcode Interface Builder에서 편집한 후 파일을 저장 하 고 Mac용 Visual Studio로 돌아갑니다. 변경 내용이 Xamarin.ios 프로젝트와 동기화 됩니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="revert-to-xcode"></a>Xcode로 되돌리기

IOS 디자이너와 Xcode Interface Builder를 서로 바꿔 사용할 수 있지만 Xcode Interface Builder Mac 에서만 사용할 수 있습니다. Mac의 Xcode Interface Builder에서 storyboard 또는. xib 파일을 열려면 [Mac용 Visual Studio](/visualstudio/mac/)에서 xamarin.ios 프로젝트가 포함 된 솔루션을 열고 파일을 마우스 오른쪽 단추로 클릭 한 다음에 설명 된 대로 **> Xcode Interface Builder를 사용**하 여 열기를 선택 합니다. 아래 스크린샷:

[![Xcode Interface Builder에서 스토리 보드 열기](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "Xcode Interface Builder에서 스토리 보드 열기")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png#lightbox)

Xcode Interface Builder에서 편집한 후 파일을 저장 하 고 Mac용 Visual Studio로 돌아갑니다. 변경 내용이 Xamarin.ios 프로젝트와 동기화 됩니다.

-----

## <a name="xib-support"></a>. xib 지원

IOS 디자이너는 xib 파일 생성, 편집 및 관리를 지원 합니다. 응용 프로그램의 뷰 계층 구조에 추가할 수 있는 단일 사용자 지정 보기를 제공 하는 XML 파일입니다. 일반적으로 xib 파일은 응용 프로그램의 단일 보기 또는 화면에 대 한 인터페이스를 나타내지만 storyboard는 여러 화면 및 이들 간의 전환을 나타냅니다.

사용자 인터페이스를 만들고 유지 관리 하는 데 가장 적합 한 솔루션 (xib 파일, 스토리 보드 또는 코드)에 대해 많은 의견이 있습니다. 실제로 완벽 한 솔루션은 없으며, 현재 작업에 가장 적합 한 도구를 고려할 가치가 있습니다. 즉, xib 파일은 사용자 지정 테이블 뷰 셀과 같이 응용 프로그램의 여러 위치에서 필요한 사용자 지정 보기를 나타내는 데 사용 될 때 일반적으로 가장 강력 합니다. 

Xib 파일을 사용 하는 방법에 대 한 자세한 내용은 다음 조리법에서 찾을 수 있습니다.

- [Xib 템플릿 사용](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/using_the_ios_view_xib_template)
- [. Xib를 사용 하 여 사용자 지정 TableViewCell 만들기](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/tables/custom-tableviewcell)
- [. Xib를 사용 하 여 시작 화면 만들기](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/launchscreen-xib)

스토리 보드 사용에 대 한 자세한 내용은 [Storyboard 소개](~/ios/user-interface/storyboards/index.md)를 참조 하세요.

이 및 기타 iOS 디자이너 관련 가이드에서는 대부분의 Xamarin.ios 새 프로젝트 템플릿이 기본적으로 storyboard를 제공 하므로 사용자 인터페이스를 빌드하기 위한 표준 방법으로 storyboard 사용을 참조 합니다.

## <a name="summary"></a>요약

이 가이드에서는 iOS Designer를 소개 하 고, 기능을 설명 하 고, 멋진 사용자 인터페이스를 디자인 하는 데 제공 하는 도구에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [Storyboards 소개](~/ios/user-interface/storyboards/index.md)
- [iOS 디자인 가능한 컨트롤 연습](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, iOS 멀티스크린](~/ios/get-started/hello-ios-multiscreen/index.md)
- [Android Designer 개요](~/android/user-interface/android-designer/index.md)
- [Partial 클래스 및 메서드](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)
- [Xamarin Designer for iOS 살펴보기-2014 (비디오)](https://www.youtube.com/watch?v=W4H9uLjoEjM)
- [IOS Designer를 사용 하 여 시작 화면 만들기 (비디오)](https://university.xamarin.com/lightninglectures/using-the-ios-designer-to-create-a-launch-screen)
