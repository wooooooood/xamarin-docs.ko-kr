---
ms.assetid: 7C132A7C-4973-4B2D-98DC-3661C08EA33F
title: WPF 및 Xamarin.Forms 앱 수명 주기
description: 이 문서에서는 Xamarin.ios 및 WPF 응용 프로그램에 대 한 응용 프로그램 수명 주기의 유사성과 차이점을 비교 합니다. 시각적 트리, 그래픽, 리소스 및 스타일도 살펴봅니다.
author: conceptdev
ms.author: crdun
ms.date: 04/26/2017
ms.openlocfilehash: 1d1c207672baf37c91d39fca1d9f628d85971577
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765898"
---
# <a name="wpf-vs-xamarinforms-app-lifecycle"></a>WPF 및 Xamarin.Forms 앱 수명 주기

Xamarin.ios는 특히 WPF 이전에 제공 된 XAML 기반 프레임 워크에서 많은 디자인 지침을 사용 합니다. 그러나 다른 방법으로는 마이그레이션을 시도 하는 사용자에 대 한 고정 지점이 될 수 있습니다. 이 문서에서는 이러한 문제 중 일부를 식별 하 고 가능한 경우 WPF 정보를 Xamarin.ios에 연결할 수 있는 지침을 제공 합니다.

## <a name="app-lifecycle"></a>앱 수명 주기

WPF와 Xamarin.ios 간의 응용 프로그램 수명 주기는 비슷합니다. 모두 외부 (플랫폼) 코드에서 시작 하 고 메서드 호출을 통해 UI를 시작 합니다. 차이점은 Xamarin.ios는 항상 플랫폼별 어셈블리에서 시작 하 여 앱에 대 한 UI를 초기화 하 고 만드는 것입니다.

**WPF**

- `Main method > App > MainWindow`

> [!NOTE]
> 메서드 `Main` 는 기본적으로 자동 생성 되 고 코드에 표시 되지 않습니다.

**Xamarin.Forms**

- **iOS** &ndash; `Main method > AppDelegate > App > ContentPage`
- **Android** &ndash; `MainActivity > App > ContentPage`
- **UWP** &ndash; `Main method > App(UWP) > MainPage(UWP) > App > ContentPage`

### <a name="application-class"></a>응용 프로그램 클래스

WPF와 xamarin.ios 모두 단일 항목으로 생성 `Application` 되는 클래스를 포함 합니다. 대부분의 경우 앱은이 클래스에서 파생 되므로 사용자 지정 응용 프로그램을 제공 하는 것은 WPF에서 반드시 필요한 것은 아니지만 이러한 응용 프로그램을 제공 합니다. 둘 다 생성 `Application.Current` 된 단일 항목을 찾기 위한 속성을 노출 합니다.

### <a name="global-properties--persistence"></a>전역 속성 + 지 속성

WPF와 xamarin.ios에는 모두 응용 프로그램 `Application.Properties` 의 어디에서 나 액세스할 수 있는 전역 앱 수준 개체를 저장할 수 있는 사전이 있습니다. 주요 차이점은 Xamarin.ios가 앱이 일시 중단 될 때 컬렉션에 저장 된 모든 기본 형식을 _유지_ 하 고 고 때 다시 로드 한다는 것입니다. WPF는 이러한 동작을 자동으로 지원 하지 않습니다. 대부분의 개발자는 격리 된 저장소에 의존 하거나 기본 제공 `Settings` 지원을 활용 했습니다.

## <a name="defining-pages-and-the-visual-tree"></a>페이지 및 시각적 트리 정의

WPF는를 `Window` 최상위 시각적 요소에 대 한 루트 요소로 사용 합니다. 이는 정보를 표시 하기 위해 Windows 환경의 HWND를 정의 합니다. WPF에서 원하는 만큼 창을 동시에 만들고 표시할 수 있습니다.

Xamarin.ios에서 최상위 시각적 개체는 항상 플랫폼에 의해 정의 됩니다 `UIWindow`. 예를 들어 iOS의 경우입니다. Xamarin.ios는 클래스를 `Page` 사용 하 여 해당 콘텐츠를 이러한 네이티브 플랫폼 표현으로 렌더링 합니다. 각각 `Page` xamarin.ios는 응용 프로그램의 고유한 "페이지"를 나타내며, 한 번에 하나만 표시 됩니다.

WPFs `Window` 와 xamarin.ios 모두 표시 된 `Page` 제목에 `Title` 영향을 주는 속성을 포함 하며, 둘 다 페이지 `Icon` 에 대 한 특정 아이콘을 표시**하는 속성을 가집니다 (제목** 및 아이콘은 항상 그렇지 않음). Xamarin.ios에서 표시 됩니다. 또한 배경색 또는 이미지와 같은 일반적인 시각적 속성을 변경할 수 있습니다.

기술적으로는 두 개의 개별 플랫폼 보기 (예: 두 개의 `UIWindow` 개체를 정의 하 고 두 번째 개체는 외부 표시 또는 연극 소리에 렌더링 됨)로 렌더링할 수 있습니다 .이 작업을 수행 하려면 플랫폼별 코드가 필요 합니다. Xamarin 양식 자체.

### <a name="views"></a>뷰

두 프레임 워크의 시각적 계층 구조는 유사 합니다. WPF는 WYSIWYG 문서에 대 한 지원으로 인해 조금 더 자세히 설명 합니다.

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

### <a name="view-lifecycle"></a>뷰 수명 주기

Xamarin.ios는 주로 모바일 시나리오를 중심으로 합니다. 따라서 응용 프로그램은 사용자가 상호 작용할 때 _활성화_, _일시 중단_및 다시 _활성화_ 됩니다. 이는 WPF 응용 프로그램 `Window` 에서를 클릭 하는 것과 유사 하며,이 동작을 모니터링 하기 위해 재정의 하거나 후크 할 수 있는 메서드 및 해당 이벤트 집합이 있습니다.

| 용도 | WPF 메서드 | Xamarin.ios 메서드 |
|--- |--- |--- |
|초기 활성화|ctor + OnLoaded|ctor + 페이지. OnStart|
|같이|Window.IsVisibleChanged|Page.Appearing|
|숨김|Window.IsVisibleChanged|Page.Disappearing|
|일시 중단/포커스 손실|Window.OnDeactivated|Page.OnSleep|
|활성화/포커스를 가져왔습니다.|Window.OnActivated|Page.OnResume|
|Closed|창. OnClosing + 창. Onclosing|n/a|

두 가지 모두 자식 컨트롤을 숨기 거 나 표시 하는 기능을 지원 합니다. WPF에서는 `IsVisible` 세 가지 상태 속성 (visible, hidden 및 축소)입니다. Xamarin.ios에서는 `IsVisible` 속성을 통해 표시 하거나 숨길 수 있습니다.

### <a name="layout"></a>레이아웃

페이지 레이아웃은 WPF에서 발생 하는 동일한 2 패스 (측정/정렬)에서 발생 합니다. Xamarin.ios `Page` 클래스에서 다음 메서드를 재정의 하 여 페이지 레이아웃에 후크 할 수 있습니다.

| 메서드 | 용도 |
|--- |--- |
|OnChildMeasureInvalidated|자식의 기본 설정 크기가 변경 되었습니다.|
|OnSizeAllocated|페이지에 너비/높이가 할당 되었습니다.|
|LayoutChanged 이벤트|페이지의 레이아웃/크기가 변경 되었습니다.|

현재 호출 되는 전역 레이아웃 이벤트는 없으며 WPF에서와 같은 전역 `CompositionTarget.Rendering` 이벤트도 없습니다.

#### <a name="common-layout-properties"></a>공통 레이아웃 속성

WPF 및 Xamarin은 모두 요소 주위의 `Margin` 간격을 제어 하 고 `Padding` 요소 _내부의_ 간격을 제어 하도록 지원 합니다. 또한 대부분의 Xamarin.ios 레이아웃 보기에는 간격 (예: 행 또는 열)을 제어 하는 속성이 있습니다.

또한 대부분의 요소에는 부모 컨테이너에 배치 되는 방법에 영향을 주는 속성이 있습니다.

| WPF | Xamarin.Forms | 용도 |
|--- |--- |--- |
|HorizontalAlignment|HorizontalOptions|왼쪽/가운데/오른쪽/늘이기 옵션|
|VerticalAlignment|VerticalOptions|위쪽/가운데/아래쪽/늘이기 옵션|

> [!NOTE]
> 이러한 속성의 실제 해석은 부모 컨테이너에 따라 다릅니다.

#### <a name="layout-views"></a>레이아웃 뷰

WPF 및 Xamarin.ios는 모두 레이아웃 컨트롤을 사용 하 여 자식 요소를 배치 합니다. 대부분의 경우 이러한 기능은 기능 측면에서 서로 매우 가깝습니다.

| WPF | Xamarin.Forms | 레이아웃 스타일 |
|--- |--- |--- |
|StackPanel|StackLayout|왼쪽에서 오른쪽 또는 위쪽에서 아래쪽 무한 누적|
|표 형태|표 형태|테이블 형식 (행 및 열)|
|DockPanel|n/a|창의 가장자리에 도킹|
|Canvas|AbsoluteLayout|픽셀/좌표 위치 지정|
|WrapPanel|n/a|줄 바꿈 스택|
|n/a|RelativeLayout|상대 규칙 기반 위치 지정|

> [!NOTE]
> Xamarin.ios는를 `GridSplitter`지원 하지 않습니다.

두 플랫폼 모두 _연결 된 속성_ 을 사용 하 여 자식 항목을 미세 조정 합니다.

### <a name="rendering"></a>렌더링

WPF 및 Xamarin.ios의 렌더링 메커니즘은 완전히 다릅니다. WPF에서 직접 만든 컨트롤은 화면에서 콘텐츠를 픽셀에 직접 렌더링 합니다. WPF는이를 나타내기 위해 두 개의 개체 그래프 (_트리_)를 유지 관리 합니다. _논리 트리_ 는 코드 또는 XAML에 정의 된 컨트롤을 나타내고 _시각적 트리_ 는 수행 되는 화면에서 발생 하는 실제 렌더링을 나타냅니다. 시각적 요소 (가상 그리기 메서드를 통해) 또는 바꾸거나 사용자 지정할 수 있는 XAML 정의 `ControlTemplate` 를 통해 직접. 일반적으로 시각적 트리는 컨트롤 주위에 테두리, 암시적 내용의 레이블 등을 비롯 하 여 더 복잡 합니다. WPF에는 이러한 두 개체 그래프`LogicalTreeHelper` 를 `VisualTreeHelper`검사 하는 api 집합 (및)이 포함 되어 있습니다.

Xamarin.ios에서에 정의 `Page` 하는 컨트롤은 단순히 단순 데이터 개체입니다. 논리적 트리 표현과 비슷하지만 콘텐츠를 자체적으로 렌더링 하지 않습니다. 대신 요소 렌더링에 영향을 주는 _데이터 모델_ 입니다. 실제 렌더링은 [각 컨트롤 형식에 매핑되는 개별 _시각적 렌더러_ 집합](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)에 의해 수행 됩니다. 이러한 렌더러는 플랫폼별 Xamarin.ios 어셈블리에서 각 플랫폼별 프로젝트에 등록 됩니다. [여기](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)에서 목록을 볼 수 있습니다. Xamarin.ios는 렌더러를 교체 하거나 확장 하는 것 외에도 플랫폼 단위로 네이티브 렌더링에 영향을 주는 데 사용할 수 있는 [효과](~/xamarin-forms/app-fundamentals/effects/index.md) 를 지원 합니다.

#### <a name="the-logicalvisual-tree"></a>논리적/시각적 트리

Xamarin.ios에서 논리적 트리를 탐색 하기 위한 API는 제공 되지 않지만 리플렉션을 사용 하 여 동일한 정보를 얻을 수 있습니다. 예를 들어 다음은 리플렉션을 사용 하 여 [논리적 자식을 열거할 수 있는 메서드입니다](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Extensions/ElementExtensions.cs#L108) .

## <a name="graphics"></a>그래픽

Xamarin에는 단순 사각형 (`BoxView`)을 초과 하는 기본 형식에 대 한 그래픽 시스템이 포함 되지 않습니다. [SkiaSharp](~/graphics-games/skiasharp/index.md) 등의 타사 라이브러리를 포함 하 여 플랫폼 간 2d 그리기 또는 3D 용 [urhosharp](~/graphics-games/urhosharp/index.md) 를 가져올 수 있습니다.

## <a name="resources"></a>리소스

WPF 및 Xamarin.ios에는 모두 리소스와 리소스 사전의 개념이 있습니다. 키를 사용 하 여 개체 형식을에 `ResourceDictionary` 배치한 다음 변경 되지 않는 항목 또는 `{DynamicResource}` 런타임에 사전 `{StaticResource}` 에서 변경 될 수 있는 항목에 대 한 항목을 찾을 수 있습니다. 사용 및 메커니즘은 다음과 같이 한 가지 차이가 있습니다. Xamarin.ios를 사용 하려면 `ResourceDictionary` `Resources` 속성에 할당할를 정의 해야 합니다. 반면 WPF는이를 미리 만들어 할당 합니다.

예를 들어 아래 정의를 참조 하세요.

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

를 정의 `ResourceDictionary`하지 않으면 런타임 오류가 발생 합니다.

## <a name="styles"></a>스타일

스타일은 Xamarin.ios 에서도 완전히 지원 되며 UI를 구성 하는 Xamarin. Forms 요소에 테마를 지정 하는 데 사용할 수 있습니다. 트리거 (속성, 이벤트 및 데이터),를 통한 `BasedOn`상속 및 값에 대 한 리소스 조회를 지원 합니다. 스타일은 `Style` 속성을 통해 명시적으로 또는 WPF와 마찬가지로 리소스 키를 제공 하지 않고 암시적으로 요소에 적용 됩니다.

### <a name="device-styles"></a>디바이스 스타일

WPF에는 시스템 색, 글꼴 및 메트릭을 값 및 리소스 키 형식으로 지시 하는 미리 정의 `SystemColors`된 속성 집합이 있습니다. 여기에는와 같은 정적 클래스 집합에 정적 값으로 저장 됩니다. Xamarin.ios는 유사 하지만 동일한 작업을 나타내는 [장치 스타일](~/xamarin-forms/user-interface/styles/device.md) 집합을 정의 합니다. 이러한 스타일은 프레임 워크에서 제공 하 고 런타임 환경 (예: 액세스 가능성)을 기반으로 하는 값으로 설정 됩니다.

**WPF**

```xaml
<Label Text="Title" Foreground="{DynamicResource {x:Static SystemColors.DesktopBrushKey}}" />
```

**Xamarin.Forms**

```xaml
<Label Text="Title" Style="{DynamicResource TitleStyle}" />
```
