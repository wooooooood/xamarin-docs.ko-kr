---
ms.assetid: 7C132A7C-4973-4B2D-98DC-3661C08EA33F
title: WPF 및 합니다. Xamarin.Forms 앱 수명 주기
description: 앱 시작 프로세스를 이해 하 고 상태는 백그라운드 처리
ms.technology: xamarin-crossplatform
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: dfcc00fa34d3bd5026653a4550e16634bd2c6238
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
---
# <a name="wpf-vs-xamarinforms-app-lifecycle"></a>WPF 및 합니다. Xamarin.Forms 앱 수명 주기

Xamarin.Forms에는 디자인 지침가지고 특히 WPF는 XAML 기반 프레임 워크에서 많이 사용 합니다. 그러나 다른 방법으로 것 편차가 큰 크게를 통해 마이그레이션할 하려고 하는 사용자를 위한 스티커 지점 일 수 있습니다. 이 문서는 이러한 문제 중 일부를 식별 하 고 xamarin.forms WPF 기술 브리지에 가능한 지침을 제공 하려고 합니다.

## <a name="app-lifecycle"></a>앱 수명 주기

WPF 및 Xamarin.Forms 사이의 응용 프로그램 수명 주기가 비슷합니다. 모두 (플랫폼) 외부 코드에서 시작 하 고 메서드 호출을 통해 UI 시작 합니다. Xamarin.Forms 항상 다음 초기화 하 고 응용 프로그램에 대 한 UI를 만들 되는 플랫폼 특정 어셈블리에 시작 하는 것이 다릅니다.

**WPF**

- `Main method > App > MainWindow`

> [!NOTE]
> `Main` 메서드가 기본적으로 자동으로 생성 하 고 코드에 표시 되지 않습니다.

**Xamarin.Forms**

- **IOS** &ndash; `Main method > AppDelegate > App > ContentPage`
- **Android** &ndash; `MainActivity > App > ContentPage`
- **UWP** &ndash; `Main method > App(UWP) > MainPage(UWP) > App > ContentPage`

### <a name="application-class"></a>응용 프로그램 클래스

WPF와 Xamarin.Forms에는 `Application` 단일 항목으로 생성 된 클래스입니다. 대부분의 경우에서 앱은 필수는 아니지만이 엄격 하 게 WPF에서 사용자 지정 응용 프로그램을 제공 하이 클래스에서 파생 됩니다. 둘 다 노출는 `Application.Current` 속성 만들어진된 singleton을 찾으려고 합니다.

### <a name="global-properties--persistence"></a>전역 속성 + 지 속성

WPF와 Xamarin.Forms에는 `Application.Properties` 사전을 사용할 수 있는 응용 프로그램에 액세스할 수 있는 전역 응용 프로그램 수준 개체를 저장할 수 있습니다. 중요 한 차이점은 Xamarin.Forms은 _유지_ 앱이 일시 중단 및 다시 시작할는 때를 다시 로드 하는 경우는 컬렉션에 저장 된 모든 기본 형식입니다. 대신 대부분의 개발자 격리 된 저장소를 사용 하거나 기본 제공 사용-WPF 해당 동작을 자동으로 지원 되지 않습니다 `Settings` 지원 합니다.

## <a name="defining-pages-and-the-visual-tree"></a>페이지 및 시각적 트리를 정의합니다.

사용 하 여 WPF는 `Window` 모든 최상위 시각적 요소에 대해 루트 요소로 합니다. 이 정보를 표시 하려면 Windows 환경에서 HWND를 정의 합니다. 만들 수 있으며 WPF에서 원하는 만큼에 동시에 많은 창을 표시 합니다.

Xamarin.forms에 최상위 시각적 개체는 항상 플랫폼이 정의한-iOS에서 예를 들어, 이기는 `UIWindow`합니다. Xamarin.Forms 렌더링 되는 사용 하 여 이러한 네이티브 플랫폼 표현으로 콘텐츠는 `Page` 클래스입니다. 각 `Page` Xamarin.Forms 나타냅니다 응용 프로그램에서 고유한 "페이지" 여기서 하나만 한 번에 표시 됩니다.

두 WPFs `Window` 및 Xamarin.Forms `Page` 포함 한 `Title` 에 표시 된 제목 및 둘 다 영향을 주는 속성에 사용할는 `Icon` 속성 페이지에 대 한 특정 아이콘을 표시 하려면 (**참고** 하는 제목 및 아이콘이 표시 되지 않습니다 항상 Xamarin.Forms에). 또한 명령 프롬프트 창의 배경색 또는 이미지와 같은 둘 다에서 공통 시각적 속성을 변경할 수 있습니다.

기술적으로 두 개의 별도 플랫폼 뷰를 렌더링할 수 (예: 두 개의 정의 `UIWindow` 외부 디스플레이 또는 AirPlay를 두 번째로 하나의 렌더링 있고 개체), 플랫폼별 코드를 필요 하며의 직접 지원 되는 기능이 아닙니다 자체 Xamarin.Forms입니다.

### <a name="views"></a>보기

두 프레임 워크에 대 한 시각적 계층 구조는 유사 합니다. WPF는 WYSIWYG 문서에 대 한 지원의 좀 더 깊은입니다.

**WPF**

```
DependencyObject - base class for all bindable things
   Visual - rendering mechanics
      UIElement - common events + interactions
         FrameworkElement - adds layout
            Shape - 2D graphics
            Control - interactive controls
```

**Xamarin.Forms**

```
BindableObject - base class for all bindable things
   Element - basic parent/child support + resources + effects
      VisualElement - adds visual rendering properties (color, fonts, transforms, etc.)
         View - layout + gesture support
```

### <a name="view-lifcycle"></a>보기 Lifcycle

Xamarin.Forms는 주로 모바일 시나리오 주위 방향으로 나타납니다. 따라서 응용 프로그램은 _활성화_, _일시 중단_, 및 _다시 활성화_ 와 상호 작용 하는 사용자입니다. 클릭 하는 것과 비슷합니다는 `Window` WPF 응용 프로그램에서 되며 일련의 메서드와 해당 이벤트를 재정의 하거나이 동작을 모니터링 하려면 후크하고 수 있습니다.

| 용도 | WPF 메서드 | Xamarin.Forms 메서드 |
|--- |--- |--- |
|초기 활성화|ctor + Window.OnLoaded|ctor + Page.OnStart|
|표시|Window.IsVisibleChanged|Page.Appearing|
|Hidden|Window.IsVisibleChanged|Page.Disapearing|
|일시 중단/Lost 포커스|Window.OnDeactivated|Page.OnSleep|
|활성화/가져왔습니다 포커스|Window.OnActivated|Page.OnResume|
|Closed|Window.OnClosing + Window.OnClosed|N/A|


두 가지 지원을 숨기기/표시 자식 컨트롤에도 WPF에서이 세 가지 상태 속성 `IsVisible` (표시, 숨김, 및 축소) 합니다. Xamarin.forms에 바로 표시 /를 통해 숨겨진는 `IsVisible` 속성입니다.

### <a name="layout"></a>레이아웃

페이지 레이아웃은 동일한 2 패스 (측정값/정렬) WPF에서 일어나는에서 발생 합니다. Xamarin.forms는 다음 메서드를 재정의 하 여 페이지 레이아웃에 후크 할 수 `Page` 클래스:

| 메서드 | 용도 |
|--- |--- |
|OnChildMeasureInvalidated|자식 요소의 기본 크기 변경 되었습니다.|
|OnSizeAllocated|페이지 너비/높이 할당 되었습니다.|
|LayoutChanged 이벤트|페이지의 레이아웃/크기가 변경 되었습니다.|

있습니다 오늘 라는 전역 레이아웃 이벤트가 없고 전역 `CompositionTarget.Rendering` 이벤트와 같은 WPF에서 찾을 수 있습니다.

#### <a name="common-layout-properties"></a>일반 레이아웃 속성

WPF 및 Xamarin.Forms 둘 다 지원 `Margin` 제어 간격, 요소 주위에 및 `Padding` 제어 간격 _내_ 요소입니다. 또한 대부분의 Xamarin.Forms 레이아웃 보기는 간격 (예: 행 또는 열)을 제어 하는 속성이 있습니다.

또한 대부분의 요소는 부모 컨테이너에 배치 되는 방식을 영향을 줍니다. 속성을 포함 합니다.

| WPF | Xamarin.Forms | 용도 |
|--- |--- |--- |
|HorizontalAlignment|HorizontalOptions|왼쪽/센터/오른쪽/스트레치 옵션|
|VerticalAlignment|VerticalOptions|Top/센터/아래쪽/스트레치 옵션|

> [!NOTE]
> 이러한 속성의 실제 해석 부모 컨테이너에 따라 달라 집니다.

#### <a name="layout-views"></a>레이아웃 뷰

WPF 및 Xamarin.Forms 자식 요소의 위치를 레이아웃 컨트롤을 사용 합니다. 대부분의 경우 이러한 항목은 매우 서로 가까이 기능에 따라 합니다.

| WPF | Xamarin.Forms | 레이아웃 스타일 |
|--- |--- |--- |
|StackPanel|StackLayout|왼쪽에서 오른쪽 또는 상-무한 스택|
|표|표|테이블 형식 (행과 열)|
|DockPanel|N/A|창의 가장자리에 도킹|
|Canvas|AbsoluteLayout|픽셀/좌표 위치 지정|
|WrapPanel|N/A|스택 래핑|
|N/A|RelativeLayout|상대 규칙을 만들 위치 지정|

> [!NOTE]
> Xamarin.Forms를 지원 하지 않습니다는 `GridSplitter`합니다.

두 플랫폼은 사용 _연결 된 속성_ 자식 세밀 하 게 조정 합니다.

### <a name="rendering"></a>렌더링

WPF 및 Xamarin.Forms에 대 한 렌더링 메커니즘은 매우 다릅니다. WPF에서 직접 만들 컨트롤 화면에서 픽셀에 콘텐츠를 렌더링 합니다. WPF 두 개체 그래프 유지 관리 (_트리_)를 나타내는이 대화 상자는 _논리적 트리_ 코드 또는 XAML로 정의 된 대로 컨트롤을 나타내는 및 _시각적 트리_ 나타냅니다는 다음 화면에서 발생 하는 실제 렌더링 (가상 그리기 방법의 경우)를 통해 시각적 요소 하 여 간접적으로 수행 하거나 XAML 정의 통해 `ControlTemplate` 교체 하거나 사용자 지정할 수 있는 합니다. 일반적으로 시각적 트리는 더 복잡 하 게 컨트롤, 암시적 콘텐츠 등에 대 한 레이블 주위에 테두리 등이 포함 됩니다. WPF Api 집합이 포함 되어 (`LogicalTreeHelper` 및 `VisualTreeHelper`) 두 개체 그래프 이러한 오류를 확인 합니다.

Xamarin.forms에 컨트롤에서 정의 하는 `Page` 는 실제로 단순 데이터 개체입니다. 이러한 작업은 논리 트리 표현으로 유사 하지만 자신의에서 콘텐츠를 렌더링 하지 않습니다. 이들은 _데이터 모델_ 요소의 렌더링에는 영향을 줍니다. 실제 렌더링 하면 됩니다는 [구분할 집합이 _visual 렌더러_ 각 컨트롤 형식에 매핑되는](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)합니다. 이 렌더러는 각 플랫폼별 프로젝트에 플랫폼별 Xamarin.Forms 어셈블리에 등록 됩니다. 목록을 볼 수 [여기](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)합니다. 바꾸거나 렌더러를 확장 하는 것 외에도 Xamarin.Forms 역시에 대 한 지원을 [효과](~/xamarin-forms/app-fundamentals/effects/index.md) 플랫폼 당 기준 네이티브 렌더링에 영향을 줍니다 사용할 수 있는 합니다.

#### <a name="the-logicalvisual-tree"></a>논리/시각적 트리

트리를 탐색 하는 논리 xamarin.forms-노출 된 API가 없습니다. 이지만 리플렉션을 사용 하 여 동일한 정보를 얻을 수 있습니다. 예를 들어 [논리적 자식을 열거할 수 있는 메서드를 다음은](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Extensions/ElementExtensions.cs#L108) 리플렉션을 사용 하 여 합니다.

## <a name="graphics"></a>그래픽

Xamarin.Forms 간단한 사각형 초과 기본 형식에 대해 그래픽 시스템을 포함 하지 않습니다 (`BoxView`). 와 같은 타사 라이브러리를 포함할 수 있습니다 [SkiaSharp](~/graphics-games/skiasharp/index.md) 플랫폼 2D 드로잉을 얻으려고 또는 [UrhoSharp](~/graphics-games/urhosharp/index.md) 3D에 있습니다.

## <a name="resources"></a>자료

WPF 및 Xamarin.Forms 둘 다 리소스 및 리소스 사전은의 개념입니다. 임의의 개체 형식으로 배치할 수 있습니다는 `ResourceDictionary` 키로 사용을 조회 하 고 `{StaticResource}` 변경 되지 것입니다 있는 작업에 대해 또는 `{DynamicResource}` 실행 시 사전에 변경할 수 있는 작업에 대해 합니다. 사용 현황 및 메커니즘은 한 가지 차이점 동일: 정의 하는 Xamarin.Forms 필요는 `ResourceDictionary` 에 할당 하는 `Resources` 속성 WPF 미리 하나를 만들고 사용자에 대 한 할당 하는 반면 합니다.

예를 들어 아래 정의 참조 하십시오.

**WPF**

```xaml
<Window.Resources>
   <Color x:Key="redColor">#ff0000</Color>
   ...
</Window.Resources>
```

**Xamarin.Forms**

```xaml
<ContentPage.Resources>
   <ResourceDictionary>
      <Color x:Key="redColor">#ff0000</Color>
      ...
   </ResourceDictionary>
</ContentPage.Resources>
```

정의 하지 않는 경우는 `ResourceDictionary`, 런타임 오류가 생성 됩니다.

## <a name="styles"></a>스타일

스타일 Xamarin.Forms에도 완벽 하 게 지원 하 고 수 UI 구성 하는 Xamarin.Forms 요소 테마를 사용 합니다. 트리거 (속성, 이벤트 및 데이터)를 통해 상속 지원 `BasedOn`, 및 값에 대 한 리소스를 조회 합니다. 요소에 적용 된 스타일을 통해 하거나 명시적으로 되어 있어야는 `Style` 속성 또는 하지-WPF와 동일 하 게 리소스 키를 제공 하 여 암시적으로 합니다.

### <a name="device-styles"></a>장치 스타일

WPF는 미리 정의 된 속성 집합이 (같은 일련의 정적 클래스에 정적 값으로 저장 `SystemColors`) 시스템 색, 글꼴 및 메트릭 값과 리소스 키의 형태로 지정입니다. Xamarin.Forms는 유사 하지만 집합을 정의 [장치 스타일](~/xamarin-forms/user-interface/styles/device.md) 같은 항목을 나타냅니다. 이러한 스타일은 frameowrk에서 제공 되며 런타임 환경 (예: 내게 필요한 옵션)에 따라 값으로 설정 됩니다.

**WPF**

```xaml
<Label Text="Title" Foreground="{DynamicResource {x:Static SystemColors.DesktopBrushKey}}" />
```

**Xamarin.Forms**

```xaml
<Label Text="Title" Style="{DynamicResource TitleStyle}" />
```
