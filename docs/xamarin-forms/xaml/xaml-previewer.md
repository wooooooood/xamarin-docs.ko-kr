---
title: Xamarin.Forms 용 XAML 미리 보기
description: 이 문서에서는 XAML 미리 보기를 사용하여 표시되는 Xamarin.Forms 레이아웃을 확인하는 방법을 설명합니다. XAML 미리 보기는 Visual Studio 2017, Mac 및 Visual Studio 2019 (미리 보기) 용 Visual Studio에서 사용할 수 있습니다.
zone_pivot_groups: platform-dev16
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 02/04/2019
ms.openlocfilehash: 4258eca8e18229420ceb043a010c896137187653
ms.sourcegitcommit: c6ff24b524d025d7e87b7b9c25f04c740dd93497
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56240385"
---
# <a name="xaml-previewer-for-xamarinforms"></a>Xamarin.Forms 용 XAML 미리 보기

_입력할 때 렌더링되는 Xamarin.Forms 레이아웃을 확인하세요._

## <a name="requirements"></a>요구 사항

프로젝트를 수행하려면 XAML 미리 보기에 대한 최신 Xamarin.Forms NuGet 패키지가 필요합니다. Android 앱을 미리 보기하려면 [JDK 1.8 x64](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)가 필요합니다.

자세한 정보는 [릴리스](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Xamarin_Forms_Previewer)를 참조하십시오.

## <a name="getting-started"></a>시작

Xamarin.Forms XAML 미리 보기를 편집 하 고 실시간으로 XAML 파일의 플랫폼별 미리 보기를 표시 합니다.

::: zone pivot="windows"

### <a name="visual-studio"></a>Visual Studio

XAML 미리 보기는 분할 보기 창으로 확장 하 여 사용할 수 있습니다. 기본 뷰 동작 분할에서 제어할 수 있습니다 합니다 **도구 > 옵션 > Xamarin > Forms 미리 보기** 대화 합니다. 이 대화 상자에서 기본 문서 보기 및 분할 방향을 선택할 수 있습니다.

[![Visual Studio에서 Xamarin.Forms 미리 보기 옵션](xaml-previewer-images/xamlp-options-vs-sm.png "Visual Studio에서 Xamarin.Forms 미리 보기 옵션")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

XAML 페이지를 열고, 편집기에서 선택한 설정에 따라 분할 된 **도구 > 옵션 > Xamarin > Forms 미리 보기** 대화 합니다. 그러나 분할 편집기 창에서 각 파일에 대 한 변경할 수 있습니다.

#### <a name="xaml-preview-controls"></a>XAML 미리 보기 컨트롤

편집기 창의 맨 위에는 사용 중인 창을 선택하는 단추가 있으며, 맨 위에 있는 단추는 디자인 창으로 전환되고, 하단의 단추는 소스 창으로 전환됩니다. 가운데 단추는 창 순서를 바꿉니다.

[![Xamarin.Forms 미리 보기를 디자인, 원본 및 Visual Studio에서 분할 뷰를 전환 하려면 제어](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "Xamarin.Forms 미리 보기를 디자인, 원본 및 Visual Studio에서 분할 뷰를 전환 하려면 제어")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

편집기 창의 아래쪽에는 창을 세로 및 가로로 분할하고 현재 창을 확장하거나 축소할 수 있는 단추가 있습니다.

[![Visual Studio에서 Xamarin.Forms 미리 보기 창 방향 컨트롤](xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "Visual Studio에서 Xamarin.Forms 미리 보기 창 방향 컨트롤")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

::: zone-end
::: zone pivot="macos"

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

XAML 페이지를 열면 편집기에 **미리 보기** 단추가 표시됩니다. 미리 보기 창은 다음과 같이 XAML 문서 창의 오른쪽 위 모서리에 있는 **미리 보기** 단추를 눌러 표시하거나 숨길 수 있습니다.

[![Mac 용 Visual Studio에서 Xamarin.Forms 미리 보기](xaml-previewer-images/xamlp-list-sml.png "Mac 용 Visual Studio에서 Xamarin.Forms 미리 보기")](xaml-previewer-images/xamlp-list.png#lightbox)

::: zone-end
::: zone pivot="win-dev16"

### <a name="visual-studio-2019-preview"></a>Visual Studio 2019 (미리 보기)

XAML 미리 보기는 분할 보기 창으로 확장 하 여 사용할 수 있습니다. 기본 뷰 동작 분할에서 제어할 수 있습니다 합니다 **도구 > 옵션 > Xamarin > Forms 미리 보기** 대화 합니다. 이 대화 상자에서 기본 문서 보기 및 분할 방향을 선택할 수 있습니다.

[![Visual Studio에서 Xamarin.Forms 미리 보기 옵션](xaml-previewer-images/xamlp-options-vs-sm.png "Visual Studio에서 Xamarin.Forms 미리 보기 옵션")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

XAML 페이지를 열고, 편집기에서 선택한 설정에 따라 분할 된 **도구 > 옵션 > Xamarin > Forms 미리 보기** 대화 합니다. 그러나 분할 편집기 창에서 각 파일에 대 한 변경할 수 있습니다.

#### <a name="xaml-preview-controls"></a>XAML 미리 보기 컨트롤

편집기 창의 맨 위에는 사용 중인 창을 선택하는 단추가 있으며, 맨 위에 있는 단추는 디자인 창으로 전환되고, 하단의 단추는 소스 창으로 전환됩니다. 가운데 단추는 창 순서를 바꿉니다.

[![Xamarin.Forms 미리 보기를 디자인, 원본 및 Visual Studio에서 분할 뷰를 전환 하려면 제어](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "Xamarin.Forms 미리 보기를 디자인, 원본 및 Visual Studio에서 분할 뷰를 전환 하려면 제어")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

편집기 창의 아래쪽에는 창을 세로 및 가로로 분할하고 현재 창을 확장하거나 축소할 수 있는 단추가 있습니다.

[![Visual Studio에서 Xamarin.Forms 미리 보기 창 방향 컨트롤](xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "Visual Studio에서 Xamarin.Forms 미리 보기 창 방향 컨트롤")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

::: zone-end

## <a name="xaml-previewer-options"></a>XAML 미리 보기 옵션

미리 보기 창 맨 위에 있는 옵션은 다음과 같습니다.

* **Phone** – 크기 전화 화면에서 미리 보기
* **태블릿** -태블릿 크기의 화면에서 미리 보기
* **Android** – Android 버전의 화면 표시
* **iOS** – iOS 버전의 화면 표시 (*참고 합니다. Visual Studio에서 Windows를 사용 하는 경우 해야 [Mac에 페어링](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 이 모드를 사용 하도록*)
* **세로 (아이콘)** – 미리 보기에 세로 방향을 사용
* **가로 (아이콘)** – 사용 하 여 가로 방향 미리 보기

::: zone pivot="win-dev16"

## <a name="device-drop-down-menu"></a>장치 드롭다운 메뉴

Visual Studio 2019 (미리 보기)에서 장치에서 Android 또는 iOS 장치의 드롭다운 목록에서 미리 보기를 선택할 수 있습니다. 이 드롭다운에 "Phone" 및 "태블릿" 옵션을 대체합니다. IOS를 미리 볼 때 iPhone 및 iPad 장치 드롭다운 목록에 표시 됩니다. Android를 미리 볼 때 다양 한 Android 휴대폰 및 태블릿 드롭다운이 표시 됩니다. 화면 크기 및 화면 해상도 두 목록에 포함 됩니다.

::: zone-end

## <a name="detect-design-mode"></a>디자인 모드를 검색 합니다.

정적 [ `DesignMode.IsDesignModeEnabled` ](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled) 속성을 검사하여 응용 프로그램이 미리 보기에서 실행 중인지 여부를 확인할 수 있습니다. 해당 옵션을 사용하면 다음과 같이 미리 보기에서 응용 프로그램이 실행될 때만 실행되는 코드를 지정할 수 있습니다.

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}
```

::: zone pivot="win-dev16"

## <a name="enable-design-time-rendering-for-custom-controls"></a>사용자 지정 컨트롤에 대 한 디자인 타임 렌더링을 사용 하도록 설정

사용자 지정 컨트롤 디자인 시간 렌더링 여부에 관계 없이 컨트롤 프로젝트에 있는 라이브러리에서 가져온 미리 보기에서 표시할 옵트인 하는 경우 이 추가 하 여 수행할 수 있습니다 합니다 [ `[DesignTimeVisible(true)]` ](xref:System.ComponentModel.DesignTimeVisibleAttribute) 컨트롤의 클래스:

```csharp
namespace MyProject
{
  [DesignTimeVisible(true)]
  public class MyControl : BaseControl
  {
    // Your control's code here
  }

}
```

이 예제에서 볼 수 있습니다 [James Montemagno의 ImageCirclePlugin의 기본 클래스](https://github.com/jamesmontemagno/ImageCirclePlugin/blob/master/src/ImageCircle/CircleImage.shared.cs)합니다.

::: zone-end

## <a name="add-design-time-data"></a>디자인 타임 데이터를 추가 합니다.

일부 레이아웃은 사용자 인터페이스 컨트롤에 바인딩된 데이터가 없으면 시각화하기 어려울 수 있습니다. 미리 보기를 더 유용하게 만들려면 바인딩 컨텍스트(코드 숨김 또는 XAML 사용)를 하드 코딩하여 컨트롤에 일부 정적 데이터를 할당하십시오.

XAML에서 정적 ViewModel에 바인딩하는 방법을 보려면 James Montemagno의 [디자인 시 데이터 추가에 대한 블로그 게시물](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data)을 참조하십시오.

## <a name="troubleshooting"></a>문제 해결

아래 문제를 확인하고 문제가 발생하면 [Xamarin 포럼](https://forums.xamarin.com/categories/xamarin-forms)을 확인하십시오.

### <a name="xaml-previewer-isnt-showing-or-shows-an-error"></a>XAML 미리 보기 표시 되지 않으면 또는 오류를 보여 줍니다.

다음을 확인하십시오.

* 디자이너 에이전트는 처음 XAML 파일을 미리 보기할 때 설정해야 합니다. 준비가 완료될 때까지 진행률 표시기가 진행률 메시지와 함께 미리보기에 나타납니다.
* XAML 파일을 닫았다가 다시 열어 봅니다.
* `App` 클래스가 매개 변수 없는 생성자를 가지고 있는지 확인합니다.

::: zone pivot="win-dev16"

### <a name="custom-controls-arent-rendering"></a>사용자 지정 컨트롤 렌더링 되지 않습니다.

프로젝트를 빌드하고 있는지 확인 합니다. 미리 보기는 오류 없이 컨트롤을 렌더링할 수 없습니다, 컨트롤의 작성자가 되지에 옵트인 디자인 시간 렌더링 하는 경우 미리 보기 컨트롤의 기본 클래스를 보여 줍니다.

::: zone-end
