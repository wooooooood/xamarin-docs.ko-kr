---
title: Xamarin.Forms 용 XAML 미리 보기
description: 이 문서에서는 XAML 미리 보기를 사용하여 표시되는 Xamarin.Forms 레이아웃을 확인하는 방법을 설명합니다. XAML 미리 보기는 Visual Studio 2019 및 mac 용 Visual Studio 2019 제공 됩니다.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 02/04/2019
ms.openlocfilehash: db243a9c8dcb25f51bc7926a7aa239531e9c24f6
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58859013"
---
# <a name="xaml-previewer-for-xamarinforms"></a>Xamarin.Forms 용 XAML 미리 보기

_입력할 때 렌더링 하 여 Xamarin.Forms 레이아웃을 참조 하세요._

## <a name="overview"></a>개요

XAML 미리 보기에서는 iOS 및 Android에서 Xamarin.Forms XAML 페이지 모양을 보여 줍니다. 에 XAML을 변경한 경우 코드와 함께 즉시 미리 보기에 표시 됩니다. XAML 미리 보기는 Visual Studio 및 mac 용 Visual Studio에서 사용할 수 있습니다.

## <a name="getting-started"></a>시작

::: zone pivot="windows"

### <a name="visual-studio-2019"></a>Visual Studio 2019

분할 뷰 창에 있는 화살표를 클릭 하 여 XAML 미리 보기를 열 수 있습니다. 기본 분할 보기 동작을 변경 하려는 경우, 사용 된 **도구 > 옵션 > Xamarin > Forms 미리 보기** 대화 합니다. 이 대화 상자에서 기본 문서 보기 및 분할 방향을 선택할 수 있습니다.

[![Xamarin 합니다. Visual Studio에서 미리 보기 옵션 구성](xaml-previewer-images/xamlp-options-vs-sm.png "Visual Studio에서 Xamarin.Forms 미리 보기 옵션")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

XAML 파일을 열면 편집기가 열립니다 큰 또는 옆에서 선택한 설정에 따라 미리 보기에는 **도구 > 옵션 > Xamarin > Forms 미리 보기** 대화 합니다. 그러나 분할 편집기 창에서 각 파일에 대 한 변경할 수 있습니다.

#### <a name="xaml-preview-controls"></a>XAML 미리 보기 컨트롤

XAML 미리 보기 코드를 확인 하려는 여부 창을 보려면 분할에서 다음과 같은이 단추를 선택 하 여 둘 다 선택 합니다. 가운데 단추 쪽 미리 보기 및 코드에 있는 교환 합니다.

[![Xamarin 합니다. Forms 디자인, 원본 및 Visual Studio의 분할 보기 간에 전환 하려면 미리 보기 컨트롤](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "Xamarin.Forms 미리 보기를 디자인, 원본 및 Visual Studio에서 분할 뷰를 전환 하려면 제어")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

세로 또는 가로로 화면 분할 여부를 변경 하거나 하나의 창을 완전히 축소할 수 있습니다.

[![Xamarin 합니다. Visual Studio의 forms 미리 보기 창 방향 컨트롤](xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "Visual Studio에서 Xamarin.Forms 미리 보기 창 방향 컨트롤")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

::: zone-end
::: zone pivot="macos"

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

XAML 페이지를 열면 편집기에 **미리 보기** 단추가 표시됩니다. 키를 눌러 미리 보기를 표시할지를 **미리 보기** 모든 XAML 문서 창의 오른쪽 위 모서리에 있는 단추:

[![Xamarin 합니다. Mac 용 Visual Studio에서 미리 보기를 형성](xaml-previewer-images/xamlp-list-sml.png "Mac 용 Visual Studio에서 Xamarin.Forms 미리 보기")](xaml-previewer-images/xamlp-list.png#lightbox)

::: zone-end

## <a name="xaml-previewer-options"></a>XAML 미리 보기 옵션

미리 보기 창 맨 위에 있는 옵션은 다음과 같습니다.

* **Android** – Android 버전의 화면 표시
* **iOS** – iOS 버전의 화면 표시 (*참고 합니다. Visual Studio에서 Windows를 사용 하는 경우 해야 [Mac에 페어링](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 이 모드를 사용 하도록*)
* **장치** -Android 또는 iOS 장치 해상도 및 화면 크기를 포함 하 여 드롭다운 목록
* **세로 (아이콘)** – 미리 보기에 세로 방향을 사용
* **가로 (아이콘)** – 사용 하 여 가로 방향 미리 보기

## <a name="detect-design-mode"></a>디자인 모드를 검색 합니다.

정적 [ `DesignMode.IsDesignModeEnabled` ](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled) 속성 알려 미리 보기에서 실행 중인 경우. 를 사용 응용 프로그램은 또는 미리 보기에서 실행 중이 아닌 경우에 실행 됩니다 하는 코드를 지정할 수 있습니다.

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

이 속성은 디자인 타임에 실행 되지 않는 페이지 생성자에서 라이브러리를 초기화 하는 경우에 유용 합니다.

## <a name="troubleshooting"></a>문제 해결

아래의 문제를 확인 하며 [Xamarin 포럼](https://forums.xamarin.com/categories/xamarin-forms)미리 보기에서 작동 하지 않는 경우.

### <a name="xaml-previewer-isnt-showing-or-shows-an-error"></a>XAML 미리 보기 표시 되지 않으면 또는 오류를 보여 줍니다.

* 미리 보기를 시작 하려면 약간의 시간이 걸릴 수 있습니다-준비 될 때까지 "렌더링 초기화"를 표시 됩니다.
* 시도 닫은 후 XAML 파일입니다.
* `App` 클래스가 매개 변수 없는 생성자를 가지고 있는지 확인합니다.
* Xamarin.Forms 버전-확인이 최소 수를 Xamarin.Forms 3.6. NuGet 통해 최신 Xamarin.Forms 버전으로 업데이트할 수 있습니다.
* 확인 Android 미리 보기-JDK 설치 필요 [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)합니다.
* 모든 래핑 시도 페이지의 클래스를 초기화 C# 에서 코드 숨김을 `if (!DesignMode.IsDesignModeEnabled)`합니다.

### <a name="custom-controls-arent-rendering"></a>사용자 지정 컨트롤 렌더링 되지 않습니다.

프로젝트 빌드를 시도 합니다. 미리 보기 컨트롤을 렌더링할 수 없는 경우 또는 컨트롤의 작성자 옵트인 옵트아웃 디자인 타임 렌더링 하는 경우 컨트롤의 기본 클래스를 보여 줍니다. 자세한 내용은 [XAML 미리 보기에서 사용자 지정 컨트롤 렌더링](render-custom-controls.md)합니다.
