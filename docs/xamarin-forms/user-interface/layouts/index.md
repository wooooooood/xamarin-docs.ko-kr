---
title: Xamarin.Forms에서 레이아웃
description: Xamarin.Forms 화면의 콘텐츠를 구성 하기 위한 몇 가지 기능과 레이아웃 있으며이 문서에 설명 합니다.
ms.prod: xamarin
ms.assetid: 65030DA3-C7C1-4A02-B478-811073C39139
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2018
ms.openlocfilehash: 1889579a48364204a977d63bd9bdb875df37a2bf
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68657027"
---
# <a name="layouts-in-xamarinforms"></a>Xamarin.Forms에서 레이아웃

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

Xamarin.Forms 여러 레이아웃 고 화면의 콘텐츠를 구성 하기 위한 기능이 추가 되었습니다.

> [!VIDEO https://youtube.com/embed/4HlLjTZQzjM]

**Xamarin.ios 레이아웃 비디오**

각 레이아웃 컨트롤은 화면 방향 변경을 처리 하는 방법에 대 한 세부 정보 뿐만 아니라, 아래 설명 되어 있습니다.

* **[Stacklayout](stack-layout.md)** – 가로 또는 세로 방향으로 보기를 선형으로 정렬 하는 데 사용 됩니다. 뷰는 StackLayout의 왼쪽 또는 레이아웃의 오른쪽 가운데에 정렬할 수 있습니다.
* **[AbsoluteLayout](absolute-layout.md)** – 절대값 또는 비율을 기준으로 좌표 & 크기를 설정 하 여 뷰를 정렬 하는 데 사용 됩니다. 왼쪽, 오른쪽 또는 가운데 고정할 수 있을 뿐만 아니라 뷰 계층 AbsoluteLayout은 사용할 수 있습니다.
* **[RelativeLayout](relative-layout.md)** – 부모 크기 & 위치에 상대적인 제약 조건을 설정 하 여 뷰를 정렬 하는 데 사용 됩니다.
* **[Grid](grid.md)** – 모눈에서 보기를 정렬 하는 데 사용 됩니다. 절대값 또는 비율 측면에서 행과 열을 지정할 수 있습니다.
* 상대 **[레이아웃](flex-layout.md)** – 배치를 사용 하 여 가로 또는 세로로 뷰를 정렬 하는 데 사용 됩니다.
* **[ScrollView](scroll-view.md)** – 뷰가 화면 범위 내에 완전히 들어가지 못할 경우 스크롤을 제공 하는 데 사용 됩니다.
* **[Layoutoptions](layout-options.md)** – 부모를 기준으로 뷰의 맞춤 및 확장을 정의 합니다.
* **[입력 투명성](#input_transparency)** – 요소가 입력을 받을지 여부를 지정 합니다.
* **[Margin 및 패딩](margin-and-padding.md)** – 요소가 사용자 인터페이스에서 렌더링 될 때 레이아웃 동작을 제어 하는 방법을 보여 줍니다.
* **[장치 방향](device-orientation.md)** – 장치 방향 변경을 처리 하는 방법을 설명 합니다.
* **[태블릿 및 데스크톱 장치의 레이아웃](tablet.md)** – 각 플랫폼에서 더 큰 화면을 최적화 하는 방법을 보여 줍니다.
* **[바인딩 가능한 레이아웃](bindable-layouts.md)** – 항목 컬렉션에 바인딩하여 레이아웃 클래스에서 콘텐츠를 생성할 수 있도록 합니다.
* **[사용자 지정 레이아웃 만들기](custom.md)** – 사용자 지정 레이아웃 클래스를 만드는 방법을 설명 합니다.
* **[레이아웃 압축](layout-compression.md)** – 페이지 렌더링 성능을 향상 시키기 위해 시각적 트리에서 지정 된 레이아웃을 제거 합니다.

플랫폼 컨트롤 사용 하 여 Xamarin.Forms 레이아웃에서 직접 사용할 수도 있습니다 [ **네이티브 포함** ](~/xamarin-forms/platform/native-views/index.md) (Xamarin.Forms 2.2의 새로운) 할 수 있습니다 [ **사용자지정레이아웃만들기** ](custom.md) 특정 요구 사항을 충족 하도록 합니다.

다음 그래픽 레이아웃 컨트롤을 시각화 합니다.

[![](images/layouts-sml.png "Xamarin.Forms 레이아웃")](images/layouts.png#lightbox "Xamarin.Forms 레이아웃")

## <a name="choosing-the-right-layout"></a>올바른 레이아웃 선택

앱에서 원하는 레이아웃 도움말 하거나 충돌 해도 끄 떡 매력적이 고 사용할 수 있는 Xamarin.Forms 앱을 만들 수 있습니다. 각 레이아웃 작동 하는 방법을 보다 명확 하 고 더 확장성이 뛰어난 UI 코드를 작성 하는 것이 좋습니다. 하는 데 시간이 걸리고 있습니다. 화면에는 특정 디자인을 달성 하기 위해 다른 레이아웃의 조합을 가질 수 있습니다.

### <a name="stacklayoutstack-layoutmd"></a>[StackLayout](stack-layout.md)

`StackLayout` 가로 또는 세로 선 따라 뷰를 표시에 사용 됩니다. 위치 및 레이아웃 내에서 크기는 뷰를 기반으로 결정 됩니다 `HeightRequest`, `WidthRequest`하십시오 `HorizontalOptions` 및 `VerticalOptions`합니다. `StackLayout` 화면에서 다른 레이아웃을 정렬 하 여 기본 레이아웃으로 자주 사용 됩니다.

경우 예 `StackLayout` 좋은 선택이 될 것, 단추 및를 왼쪽으로 맞춰져 있으며 레이블과 오른쪽 맞춤 단추를 사용 하 여 레이블을 표시 하는 앱을 고려해 야 합니다.

```xaml
<StackLayout Orientation="Horizontal">
  <Label HorizontalOptions="StartAndExpand" Text="Label" />
  <Button HorizontalOptions="End" Text="Button" />
</StackLayout>
```

### <a name="flexlayoutflex-layoutmd"></a>[FlexLayout](flex-layout.md)

합니다 `FlexLayout` 비슷합니다 `StackLayout` 가로 또는 세로로 자식 뷰를 표시 합니다.

```xaml
<FlexLayout Direction="Column"
            AlignItems="Center"
            JustifyContent="SpaceEvenly">

    <Label Text="FlexLayout in Action" />
    <Button Text="Button" />
    <Label Text="Another Label" />
</FlexLayout>
```

그러나 단일 행 또는 열에 맞게 너무 많은 자식 경우 `FlexLayout` 해당 뷰를 래핑 수 이기도 합니다. `FlexLayout` CSS 유연한 상자 레이아웃 모듈을 기반으로 하며에 위치 및 해당 자식에 맞게 조정에 대 한 동일한 기본 제공 옵션이 많이 있습니다.

### <a name="absolutelayoutabsolute-layoutmd"></a>[AbsoluteLayout](absolute-layout.md)

`AbsoluteLayout` 크기 및 위치 지정 되 고 명시적 값 또는 레이아웃의 크기를 기준으로 값으로 사용 하 여 뷰를 표시에 사용 됩니다. 와 달리 `StackLayout` 하 고 `Grid`, `AbsoluteLayout` 자식 뷰를 겹칠 수 있습니다. 와 달리 `RelativeLayout`, `AbsoluteLayout` 화면 밖으로 요소를 배치 하도록 허용 하지 않습니다.

경우에 대 한 예 `AbsoluteLayout` 적합 하지는 스택 개체의 컬렉션을 제공 해야 하는 앱을 고려 하십시오. 이 주로 사진 또는 음악의 앨범을 제공 하는 경우에 표시 됩니다. 다음 코드는 누적의 콘텐츠 힌트를 회전 하는 요소를 사용 하 여는 더미의 모양을 제공 합니다.

XAML:

```xaml
<AbsoluteLayout Padding="15">
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" Rotation="30"
    Source="bottom.png" />
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" Rotation="60"
    Source="middle.png" />
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100"
    Source="cover.png" />
</AbsoluteLayout>
```

위 코드의 다음 측면을 note:

- 각 `Image` (가운데에 가로 간격) 동일한 위치에 표시 됩니다
- 합니다 `Padding` 으로 간주 됩니다 `AbsoluteLayout`달리 `RelativeLayout`,이 무시 하는 합니다.
- `AbsoluteLayout.LayoutFlags` 레이아웃 범위를 해석 하는 방법을 지정 합니다. 이 경우 `PositionProportional`, 좌표 되도록 레이아웃의 크기 비율로 크기 크기를 명시적으로 해석 됩니다 하는 동안 의미 합니다.
- `AbsoluteLayout.Layoutbounds` 지정 된 순서로 가로 위치, 세로 위치, 너비 및 높이 지정합니다.

### <a name="relativelayoutrelative-layoutmd"></a>[RelativeLayout](relative-layout.md)

`RelativeLayout` 크기 및 레이아웃 또는 다른 보기의 값을 기준으로 값으로 지정 된 위치를 사용 하 여 뷰를 표시 하기 위해 사용 됩니다. 상대 값 관련된 보기에는 값을 해당 그에 맞게 필요가 없습니다. 예를 들어 뷰를 설정할 수 있기 `Width` 속성을 다른 보기에 비례하여 `X` 속성입니다.

RelativeLayout 장치 크기를 비례적으로 확장 되는 Ui를 만드는 데 사용할 수 있습니다. 다음 XAML 센터에서 플래그를 사용 하 여 flagpole 사용 하 여 맨 위 모퉁이 있는 상자를 사용 하 여 디자인을 구현합니다.

```xaml
<RelativeLayout HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand">
  <BoxView Color="Blue" HeightRequest="50" WidthRequest="50"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = 0}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = 0}" />
  <BoxView Color="Red" HeightRequest="50" WidthRequest="50"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = .9}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = 0}" />
  <BoxView Color="Gray" WidthRequest="15" x:Name="pole"
    RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.75}"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = .45}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = .25}" />
  <BoxView Color="Green"
    RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.10, Constant=10}"
    RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent,Property=Width, Factor=.2, Constant=20}"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=X, Constant=15}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=Y, Constant=0}" />
</RelativeLayout>
```

위 코드의 다음 측면을 note:

- 위치 및 크기 제약 조건으로 지정 됩니다.
- flagpole 이름은 있도록 플래그의 (녹색 상자의)는 flagpole 상대적 위치를 설정할 수 있습니다.
- 제약 조건 식 `Factor` 및 `Constant` 배수로 위치 및 크기를 정의 하는 속성 (또는 소수로 나타낸 초가) 상수 및 기타 개체의 속성입니다. 상수는 음수일 수 있습니다.

### <a name="gridgridmd"></a>[눈금](grid.md)

`Grid` 행 및 열에 요소를 표시 하기 위해 사용 됩니다. Note는 표의 아니므로 테이블 셀 이나 머리글 및 바닥글 행, 행 및 열 사이 테두리 개념이 없습니다. 일반적으로 모눈 표 형식 데이터 표시에 대 한 적합 하지 않습니다. 를 사용 하는 것이 좋습니다는 [ListView](~/xamarin-forms/user-interface/listview/index.md) 하거나 [TableView](~/xamarin-forms/user-interface/tableview.md)합니다.

시기에 대 한 예는 `Grid` 는 오른쪽 레이아웃을 사용 하 여 계산기에 대 한 숫자 입력을 고려 합니다. 계산기에 대 한 숫자 입력을 네 개의 행과 단추를 사용 하 여 각 열 3 개 구성 될 수 있습니다. 다음 코드는이 디자인을 구현합니다.

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
  </Grid.RowDefinitions>

  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
  <Button Text="1" Grid.Row="0" Grid.Column="0" />
  <Button Text="2" Grid.Row="0" Grid.Column="1" />
  <Button Text="3" Grid.Row="0" Grid.Column="2" />
  <Button Text="4" Grid.Row="1" Grid.Column="0" />
  <Button Text="5" Grid.Row="1" Grid.Column="1" />
  <Button Text="6" Grid.Row="1" Grid.Column="2" />
  <Button Text="7" Grid.Row="2" Grid.Column="0" />
  <Button Text="8" Grid.Row="2" Grid.Column="1" />
  <Button Text="9" Grid.Row="2" Grid.Column="2" />
  <Button Text="0" Grid.Row="3" Grid.Column="1" />
  <Button Text="&lt;-" Grid.Row="3" Grid.Column="2" />
</Grid>
```

위 코드의 다음 측면을 note:

- 표 및 열을 명시적으로 지정 콘텐츠에서 유추할 수 없습니다.
- `Height` 및 `Width` 값 표에 사용 가능한 공간을 채우기 위해 해당 값을 설정 됩니다는 의미 하는, 별로 설정할 수 있습니다.
- 각 단추의 위치 지정 `Grid.Row`  &  `Grid.Column` 속성입니다.

### <a name="layoutoptionslayout-optionsmd"></a>[LayoutOptions](layout-options.md)

합니다 [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) 구조 맞춤 및 부모를 기준으로 보기에 대 한 확장 정의에 사용할 수 있습니다.

### <a name="margin-and-paddingmargin-and-paddingmd"></a>[여백 및 안쪽 여백](margin-and-padding.md)

합니다 [ `Margin` ](xref:Xamarin.Forms.View.Margin) 하 고 [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) 속성 사용자 인터페이스에서 요소를 렌더링할 때 레이아웃 동작을 제어 합니다.

<a name="input_transparency" />

### <a name="input-transparency"></a>입력된 투명도

각 요소에는 [ `InputTransparent` ](xref:Xamarin.Forms.VisualElement.InputTransparent) 요소에서 입력을 받을지 여부를 정의 하는 데 사용 되는 속성입니다. 기본값은 `false`, 요소 입력을 받는 있는지 확인 합니다.

이 속성이 자식 요소에 대 한 해당 값 전송 레이아웃 클래스와 같은 컨테이너 클래스에 대해 설정 된 경우. 따라서 설정 된 [ `InputTransparent` ](xref:Xamarin.Forms.VisualElement.InputTransparent) 속성을 `true` 레이아웃의 클래스는 입력을 받고 있지 레이아웃 내에서 요소에 발생 합니다.

### <a name="device-orientationdevice-orientationmd"></a>[장치 방향](device-orientation.md)

Xamarin.Forms 및 해당 기본 제공 레이아웃 장치 방향에서 변경 내용을 처리할 수 있습니다. 앱에서 지원할도 만들어야 하는 방법과 방향을 고려 가로 및 세로 모드에서 제공 된 공간을 활용 합니다.

### <a name="layout-for-tablet-and-desktop-appstabletmd"></a>[태블릿 및 데스크톱 앱에 대 한 레이아웃](tablet.md)

iOS, Android 및 유니버설 Windows 플랫폼에서 큰 화면 크기를 지 원하는 모든 태블릿 장치 (랩톱 및 데스크톱 Windows에 대 한). Xamarin.Forms를 사용 하면 장치 유형 및 페이지 레이아웃을 조정 하거나 검색 하거나 더 큰 화면에 대 한 완전히 다른 페이지를 모두 사용 하 여 더 큰 화면에 대 한 앱을 최적화할 수 있습니다.

### <a name="bindable-layoutsbindable-layoutsmd"></a>[바인딩 가능한 레이아웃](bindable-layouts.md)

클래스를 사용 하면 [`Layout<T>`](xref:Xamarin.Forms.Layout`1) 클래스에서 파생 되는 모든 레이아웃 클래스가 항목 컬렉션에 바인딩하여 항목의 모양을 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)로 설정 하는 옵션을 사용 하 여 콘텐츠를 생성할 수 있습니다. `BindableLayout`

### <a name="creating-a-custom-layoutcustommd"></a>[사용자 지정 레이아웃 만들기](custom.md)

네 가지 레이아웃 클래스를 정의 하는 Xamarin.Forms [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)를 [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout)를 [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout), 및 [ `Grid` ](xref:Xamarin.Forms.Grid)를 다른 방식으로 자식을 정렬 각 및 합니다. 그러나 때때로 레이아웃을 사용 하 여 페이지 콘텐츠를 구성 하 고 필요한 제공한 Xamarin.Forms입니다. 이 문서에서는 사용자 지정 레이아웃 클래스를 작성 하는 방법에 설명 하 고는 방향에 민감한을 보여 줍니다. `WrapLayout` 페이지에 걸쳐 가로로 자식을 정렬 하 고 그런 다음 후속 자식이 추가 행을 표시 하는 클래스입니다.

### <a name="layout-compressionlayout-compressionmd"></a>[레이아웃 압축](layout-compression.md)

레이아웃 압축 페이지 렌더링 성능을 향상 하기 위해 시각적 트리에서 지정 된 레이아웃을 제거 합니다. 이로 인해 얻을 수 있는 성능 이점은 페이지의 복잡성, 사용 중인 운영 체제의 버전, 그리고 응용 프로그램이 실행 중인 디바이스에 따라 다릅니다. 하지만 가장 큰 성능 이점은 이전 버전의 디바이스에서 볼 수 있습니다.

## <a name="making-your-choice"></a>사용자 선택

주의 대부분의 경우에서 하나 이상의 레이아웃 선택 수 원하는 설계를 구현 합니다. 여러 유효한 선택 항목의 경우에 어떤 접근 방식이 상황에 가장 쉬운 방법은 됩니다 하는 것이 좋습니다.
대부분의 디자인 중첩 레이아웃으로 더 복잡 한 디자인을 만들려면 필요 하므로 하나의 레이아웃으로 인식할 수 없습니다.

## <a name="related-links"></a>관련 링크

- [Apple의 휴먼 인터페이스 지침](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG)
- [Android 디자인 웹 사이트](https://developer.android.com/design/index.html)
- [레이아웃 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [BusinessTumble 예제 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
