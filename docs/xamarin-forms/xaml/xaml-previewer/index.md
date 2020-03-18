---
title: Xamarin.Forms 용 XAML 미리 보기
description: 이 문서에서는 XAML 미리 보기를 사용하여 표시되는 Xamarin.Forms 레이아웃을 확인하는 방법을 설명합니다. XAML 미리 보기는 Visual Studio 2019 및 Mac 용 Visual Studio 2019에서 사용할 수 있습니다.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/16/2020
ms.openlocfilehash: 465783c0771b666a276d18f47cf5d3d458d52933
ms.sourcegitcommit: 8df67f0d76ff762b517d27b8d4c217d3a3379a18
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/16/2020
ms.locfileid: "79423930"
---
# <a name="xaml-previewer-for-xamarinforms"></a>Xamarin.Forms 용 XAML 미리 보기

_사용자가 입력 한 대로 렌더링 된 Xamarin.ios 레이아웃을 참조 하세요._

## <a name="overview"></a>개요

XAML 미리 보기는 iOS 및 Android에서 Xamarin Forms XAML 페이지가 표시 되는 방식을 보여 줍니다. XAML을 변경 하면 코드와 함께 즉시 미리 보기가 표시 됩니다. XAML 미리 보기는 Visual Studio 및 Mac용 Visual Studio에서 사용할 수 있습니다.

## <a name="getting-started"></a>시작

::: zone pivot="windows"

### <a name="visual-studio-2019"></a>Visual Studio 2019

분할 뷰 창에서 화살표를 클릭 하 여 XAML 미리 보기를 열 수 있습니다. 기본 분할 뷰 동작을 변경 하려는 경우에는 xamarin > xamarin.ios **XAML 미리 보기 대화 상자 > 도구 > 옵션** 을 사용 합니다. 이 대화 상자에서 기본 문서 보기 및 분할 방향을 선택할 수 있습니다.

[![Visual Studio의 Xamarin 폼 미리 보기 옵션](xaml-previewer-images/xamlp-options-vs-sm.png "Visual Studio의 Xamarin 폼 미리 보기 옵션")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

XAML 파일을 열면 도구 > 옵션에서 선택한 설정에 따라 미리 보기 옆에 있는 전체 크기나 옆에 편집기가 열립니다. xamarin **> xamarin. FORMS XAML 미리 보기** 대화 상자를 >. 그러나 편집기 창에서 각 파일에 대 한 분할을 변경할 수 있습니다.

#### <a name="xaml-preview-controls"></a>XAML 미리 보기 컨트롤

분할 뷰 창에서 이러한 단추를 선택 하 여 코드를 표시할지, XAML 미리 보기를 표시할지 또는 두 항목을 모두 표시할지를 선택 합니다. 가운데 단추는 미리 보기와 코드의 측면을 바꿉니다.

[![Visual Studio에서 디자인, 소스 및 분할 뷰 간을 전환 하는 xamarin.ios 미리 보기 컨트롤](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "Visual Studio에서 디자인, 소스 및 분할 뷰 간을 전환 하는 xamarin.ios 미리 보기 컨트롤")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

화면이 세로 또는 가로로 분할 되는지 여부를 변경 하거나 한 창을 완전히 축소할 수 있습니다.

[![Visual Studio의 xamarin.ios 미리 보기 창 방향 컨트롤](xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "Visual Studio의 xamarin.ios 미리 보기 창 방향 컨트롤")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

#### <a name="enable-or-disable-the-xaml-previewer"></a>XAML 미리 보기 사용 또는 사용 안 함

기본 **xaml**편집기로 **기본 XML 편집기** 를 선택 하 여 **Xamarin > xamarin.ios xaml 미리 보기 대화 상자 > > 옵션** 에서 xaml 미리 보기를 해제할 수 있습니다. 그러면 문서 개요, 속성 패널 및 XAML 도구 상자도 꺼집니다. XAML 미리 보기와 이러한 도구를 다시 설정 하려면 **기본 Xaml 편집기** 를 **xamarin.ios 미리 보기**로 변경 합니다.

::: zone-end
::: zone pivot="macos"

### <a name="visual-studio-for-mac"></a>Mac용 Visual Studio

**미리 보기** 단추는 XAML 페이지를 열 때 편집기에 표시 됩니다. XAML 문서 창의 왼쪽 아래에 있는 **미리 보기** 또는 **분할** 단추를 눌러 미리 보기를 표시 하거나 숨깁니다.

[미리 보기 또는 분할 단추를 사용 하 여 Xamarin ![미리 보기 사용](xaml-previewer-images/xamlp-list-sml.png)](xaml-previewer-images/xamlp-list.png#lightbox)

> [!NOTE]
> 이전 버전의 Mac용 Visual Studio에서는 **미리 보기** 단추가 창의 오른쪽 위에 있습니다.

#### <a name="enable-or-disable-the-xaml-previewer"></a>XAML 미리 보기 사용 또는 사용 안 함

기본 **xaml**편집기로 **기본 XML 편집기** 를 선택 하 여 **Visual Studio > 기본 설정 > 텍스트 편집기 > Xaml** 대화 상자에서 xaml 미리 보기를 해제할 수 있습니다. 그러면 문서 개요, 속성 패널 및 XAML 도구 상자도 꺼집니다. XAML 미리 보기와 이러한 도구를 다시 설정 하려면 **기본 Xaml 편집기** 를 **xamarin.ios 미리 보기**로 변경 합니다.

::: zone-end

## <a name="xaml-previewer-options"></a>XAML 미리 보기 옵션

미리 보기 창의 위쪽에 있는 옵션은 다음과 같습니다.

* **Android** – 화면의 android 버전 표시
* **ios** – 화면의 ios 버전을 표시 합니다 (*참고: Windows에서 Visual Studio를 사용 하는 경우이 모드를 사용 하려면 [Mac에 페어링](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 해야 함).*
* 해상도 및 화면 크기를 포함 하는 Android 또는 iOS 장치의 **장치** 드롭다운 목록
* **세로 (아이콘)** – 미리 보기에 세로 방향을 사용 합니다.
* **가로 (아이콘)** – 미리 보기에 가로 방향을 사용 합니다.

## <a name="detect-design-mode"></a>디자인 모드 검색

정적 [`DesignMode.IsDesignModeEnabled`](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled) 속성은 미리 보기에서 응용 프로그램이 실행 되 고 있는지를 알려 줍니다. 이를 사용 하 여 미리 보기에서 응용 프로그램이 실행 되 고 있지 않을 때만 실행 되는 코드를 지정할 수 있습니다.

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}

if (!DesignMode.IsDesignModeEnabled)
{
  // Don't run in the Previewer  
}
```

이 속성은 디자인 타임에 실행 되지 않는 페이지 생성자의 라이브러리를 초기화 하는 경우에 유용 합니다.

## <a name="troubleshooting"></a>문제 해결

미리 보기가 작동 하지 않는 경우 아래 문제 및 [Xamarin 포럼](https://forums.xamarin.com/categories/xamarin-forms)을 확인 하세요.

### <a name="xaml-previewer-isnt-showing-or-shows-an-error"></a>XAML 미리 보기에서 오류를 표시 하지 않거나 오류를 표시 하지 않습니다.

* 미리 보기를 시작 하는 데 다소 시간이 걸릴 수 있습니다. 준비가 될 때까지 "렌더링 초기화 중"이 표시 됩니다.
* XAML 파일을 닫았다가 다시 열어 보세요.
* `App` 클래스에 매개 변수가 없는 생성자가 있는지 확인 합니다.
* Xamarin.ios 버전을 확인 합니다. Xamarin. 양식 3.6 이상 이어야 합니다. NuGet을 통해 최신 Xamarin.ios 버전으로 업데이트할 수 있습니다.
* JDK 설치 확인-Android를 미리 보려면 [jdk 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)이상이 필요 합니다.
* `if (!DesignMode.IsDesignModeEnabled)`에 있는 페이지의 C# 코드에서 초기화 된 클래스를 래핑 해 보세요.

### <a name="custom-controls-arent-rendering"></a>사용자 지정 컨트롤이 렌더링 되지 않습니다.

프로젝트 빌드를 시도 하세요. 미리 보기는 컨트롤의 렌더링에 실패 하거나 컨트롤의 작성자가 디자인 타임 렌더링을 옵트아웃 한 경우 컨트롤의 기본 클래스를 보여 줍니다. 자세한 내용은 [XAML 미리 보기에서 사용자 지정 컨트롤 렌더링](render-custom-controls.md)을 참조 하세요.
