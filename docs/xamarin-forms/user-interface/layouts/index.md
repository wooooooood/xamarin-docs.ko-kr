---
title: "레이아웃"
description: "보기 화면에 배치 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 65030DA3-C7C1-4A02-B478-811073C39139
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 1fe290983bf7b130dee6f1a1878a32dce3efc4c4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="layouts"></a>레이아웃

Xamarin.Forms에는 몇 가지 레이아웃 및 기능 화면의 콘텐츠를 구성 하는 데 있습니다. 각 레이아웃 컨트롤은 화면 방향 변경을 처리 하는 방법에 대 한 세부 정보 뿐만 아니라, 다음과 같습니다.

* **[StackLayout](stack-layout.md)**  &ndash; 사용 하 여 뷰를 연속적으로 정렬 가로 또는 세로로 합니다. 뷰는 StackLayout의 왼쪽 또는 오른쪽 레이아웃의 가운데에 정렬할 수 있습니다.
* **[AbsoluteLayout](absolute-layout.md)**  &ndash; 좌표를 설정 하 여 보기를 정렬 및 절대값 또는 비율 크기 조정 하는 데 사용 합니다. AbsoluteLayout 뷰 계층 수 있을 뿐만 아니라 왼쪽, 오른쪽 또는 가운데 고정 데 사용할 수 있습니다.
* **[RelativeLayout](relative-layout.md)**  &ndash; 부모 크기 및 위치를 기준으로 제약 조건을 설정 하 여 보기를 정렬 하는 데 사용 합니다.
* **[그리드](grid.md)**  &ndash; 표로 보기를 정렬 하는 데 사용 합니다. 절대값 또는 비율 측면에서 행과 열을 지정할 수 있습니다.
* **[ScrollView](scroll-view.md)**  &ndash; 뷰 화면의 경계 내에 완전히 표시할 수 없을 때 스크롤을 제공 하는 데 사용 합니다.
* **[LayoutOptions](layout-options.md)**  &ndash; 맞춤 및 부모를 기준으로 보기에 대 한 확장을 정의 합니다.
* **[투명도 입력](#input_transparency)**  &ndash; 요소 입력을 받을지 여부를 지정 합니다.
* **[여백 및 안쪽 여백](margin-and-padding.md)**  &ndash; 사용자 인터페이스에서 요소를 렌더링할 때 레이아웃 동작을 제어 하는 방법을 보여 줍니다.
* **[장치 방향을](device-orientation.md)**  &ndash; 장치 방향 변경 내용을 처리 하는 방법에 설명 합니다.
* **[태블릿 및 데스크톱 장치에서 레이아웃](tablet.md)**  &ndash; 각 플랫폼에서 큰 화면에 대 한 최적화 하는 방법을 보여 줍니다.
* **[사용자 지정 레이아웃을 만드는](custom.md)**  &ndash; 사용자 지정 레이아웃 클래스를 만드는 방법에 설명 합니다.
* **[레이아웃 압축](layout-compression.md)**  &ndash; 페이지 렌더링 성능을 개선 하기 위해 지정 된 레이아웃 시각적 트리에서 제거 합니다.

플랫폼 컨트롤으로 Xamarin.Forms 레이아웃에서 직접 사용할 수도 있습니다 [ **네이티브 포함** ](~/xamarin-forms/platform/native-views/index.md) (Xamarin.Forms 2.2의 새로운)을 수행할 수 있습니다 [ **사용자지정레이아웃만들기** ](custom.md) 특정 요구 사항에 맞게 합니다.

다음 그래픽 레이아웃 컨트롤을 시각화합니다.

[ ![](images/layouts-sml.png "Xamarin.Forms Layouts")](images/layouts.png "Xamarin.Forms Layouts")

## <a name="choosing-the-right-layout"></a>오른쪽 레이아웃 선택

응용 프로그램에서 선택 하는 레이아웃 도움말 하거나 Xamarin.Forms 효율적이 고 사용할 수 있는 응용 프로그램을 만들 때 하면 영향을 미칠 수 있습니다. 각 레이아웃 작동 하는 방법 보다 명확 하 고 확장성이 뛰어난 UI 코드를 작성 하는 것이 좋습니다. 데 약간의 시간이 걸립니다. 화면에는 특정 디자인을 달성 하기 위해 다양 한 레이아웃의 조합을 가질 수 있습니다.

### <a name="stacklayoutstack-layoutmd"></a>[StackLayout](stack-layout.md)

`StackLayout` 가로 또는 세로 있는 줄 함께 뷰를 표시 하기 위해 사용 됩니다. 위치 및 크기가 레이아웃 내에서 보기의에 따라 결정 됩니다 `HeightRequest`, `WidthRequest`, `HorizontalOptions` 및 `VerticalOptions`합니다. `StackLayout` 화면에 다른 레이아웃을 정렬 하는 기본 레이아웃으로 자주 사용 됩니다.

예에 대 한 `StackLayout` 적합는 단추를 왼쪽 맞춤 레이블과 오른쪽 맞춤 단추를 사용 하 여 레이블을 표시 해야 하는 응용 프로그램을 고려 하십시오.

```xaml
<StackLayout Orientation="Horizontal">
  <Label HorizontalOptions="StartAndExpand" Text="Label" />
  <Button HorizontalOptions="End" Text="Button" />
</StackLayout>
```

### <a name="absolutelayoutabsolute-layoutmd"></a>[AbsoluteLayout](absolute-layout.md)

`AbsoluteLayout` 크기 및 위치 중 하나에 지정 되 고 명시적 값 또는 레이아웃의 크기를 기준으로 값으로 사용 하 여 뷰를 표시 하기 위해 사용 됩니다. 와 달리 `StackLayout` 및 `Grid`, `AbsoluteLayout` 자식 뷰 겹칠를 허용 합니다. 와 달리 `RelativeLayout`, `AbsoluteLayout` 화면 요소를 배치할 수 없습니다.

예에 대 한 `AbsoluteLayout` 적합 것, 스택으로 개체의 컬렉션을 제공 하는 앱을 살펴보겠습니다. 이 사진 또는 노래의 앨범을 표시할 때 종종 표시 됩니다. 다음 코드를 누적 내용에서 힌트를 회전 하는 요소와는 더미의 모양을 입력 하면:

Xaml의 경우:

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

위의 코드의 다음 측면을 note:

- 각 `Image` (가운데에 가로 간격) 동일한 위치에 표시 됩니다
- `Padding` 으로 간주 `AbsoluteLayout`달리 `RelativeLayout`, 시 무시 하는 것입니다.
- `AbsoluteLayout.LayoutFlags` 레이아웃 범위는 해석 하는 방법을 지정 합니다. 이 경우 `PositionProportional`, 크기 명시적 크기 해석 하는 동안 좌표는 레이아웃의 크기의 비율 되도록 합니다.
- `AbsoluteLayout.Layoutbounds` 해당 순서로 가로 위치, 세로 위치, 너비 및 높이 지정합니다.

### <a name="relativelayoutrelative-layoutmd"></a>[RelativeLayout](relative-layout.md)

`RelativeLayout` 뷰 크기와 다른 보기 또는 레이아웃의 값에 대 한 값으로 지정 된 위치를 표시 하기 위해 사용 됩니다. 상대 값 관련된 보기에는 값에 해당 하는 그에 맞게 필요가 없습니다. 예를 들어, 보기의 설정할 수는 `Width` 속성을 다른 보기에 비례 하므로 `X` 속성입니다.

RelativeLayout 장치 크기 환경을 비례적으로 확장 하는 Ui를 만드는 데 사용할 수 있습니다. 다음 XAML 센터에서 플래그로 flagpole와 맨 위 모서리에 상자가 디자인을 구현합니다.

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

위의 코드의 다음 측면을 note:

- 위치와 크기 제약 조건으로 지정 됩니다.
- flagpole 라는 있도록 플래그의 (녹색 상자의)는 flagpole에 상대적인 위치를 설정할 수 있습니다.
- 제약 조건 식 `Factor` 및 `Constant` 는 여러 위치 및 크기를 정의 하는 데 사용할 수 있는 속성 (또는 소수로 나타낸 초가) 상수 및 기타 개체의 속성입니다. 상수는 음수일 수 있습니다.

### <a name="gridgridmd"></a>[눈금](grid.md)

`Grid` 요소 행 및 열을 표시 하기 위해 사용 됩니다. Note 눈금 아닌지 테이블 때문 셀 이나 머리글 / 바닥글 행, 행 및 열 사이 테두리의 개념을 되어 있지 않습니다. 일반적으로 그리드 표 형식 데이터를 표시 하기 위한 적합 하지 않습니다. 사용 하는 것이 좋습니다는 [ListView](~/xamarin-forms/user-interface/listview/index.md) 또는 [TableView](~/xamarin-forms/user-interface/tableview.md)합니다.

경우의 예는 `Grid` 오른쪽의 레이아웃, 사용 하 여 숫자 입력 계산기에 대 한 것이 좋습니다. 계산기에 대 한 입력을 숫자를 네 개의 행과 각 단추와 3 개의 열을 구성할 수 있습니다. 다음 코드는이 디자인을 구현합니다.

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

위의 코드의 다음 측면을 note:

- 표 및 열을 명시적으로 지정 내용에서 유추할 수 없습니다.
- `Height` 및 `Width` 표에서 해당 값을 사용 가능 공간 설정 한다는 의미을 시작 함을, 값을 설정할 수 있습니다.
- 각 단추 위치가에 지정 되어 `Grid.Row`  &  `Grid.Column` 속성입니다.

### <a name="layoutoptionslayout-optionsmd"></a>[LayoutOptions](layout-options.md)

[ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) 구조 맞춤 및 확장 부모를 기준으로 보기에 대 한 정의를 사용할 수 있습니다.

### <a name="margin-and-paddingmargin-and-paddingmd"></a>[여백 및 안쪽 여백](margin-and-padding.md)

[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) 및 [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) 속성 사용자 인터페이스에서 요소를 렌더링할 때 레이아웃 동작을 제어 합니다.

<a name="input_transparency" />

### <a name="input-transparency"></a>입력된 투명도

각 요소에는 [ `InputTransparent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.InputTransparent/) 요소 입력을 받을지 여부를 정의 하는 데 사용 되는 속성입니다. 기본값은 `false`, 확인 하는 입력을 받습니다.

이 속성에서 값 전송 자식 요소를 레이아웃 클래스와 같은 컨테이너 클래스에 설정 된 경우 설정의 [ `InputTransparent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.InputTransparent/) 속성을 `true` 클래스 레이아웃에서 요소 입력을 받고 있지 레이아웃 내에서 발생 합니다.

### <a name="device-orientationdevice-orientationmd"></a>[장치 방향](device-orientation.md)

Xamarin.Forms 및 해당 기본 제공 레이아웃 장치 방향에서 변경 내용을 처리할 수 있습니다. 응용 프로그램을 지 원하는 지정 하는 방법에 어떤 방향을 고려 가로 세로 모드에서 제공 되는 공간을 사용 합니다.

### <a name="layout-for-tablet-and-desktop-appstabletmd"></a>[태블릿 및 데스크톱 응용 프로그램 레이아웃](tablet.md)

iOS, Android 및 Windows 태블릿 장치에서 더 큰 크기를 지 원하는 모든 플랫폼 (뿐만 아니라 노트북 및 Windows 용 데스크톱). Xamarin.Forms를 사용 하면 장치 유형 및 페이지 레이아웃을 조정 하거나 검색 하거나 큰 화면에 대 한 완전히 다른 페이지를 모두 사용 하 여 더 큰 화면에 대 한 응용 프로그램을 최적화할 수 있습니다.

### <a name="creating-a-custom-layoutcustommd"></a>[사용자 지정 레이아웃 만들기](custom.md)

-4 개의 레이아웃 클래스를 정의 하는 Xamarin.Forms [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/), [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/), 및 [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), 다른 방식으로 자식을 정렬 각각. 그러나 때로는 레이아웃을 사용 하는 페이지 콘텐츠를 구성 하 고 필요한 제공한 Xamarin.Forms입니다. 이 문서에 사용자 지정 레이아웃 클래스를 작성 하는 방법에 설명 합니다 방향 구분 `WrapLayout` 자식을 페이지에 걸쳐 가로로 정렬 하 고 그런 다음 후속 자식이 추가 행을 표시 하는 클래스입니다.

### <a name="layout-compressionlayout-compressionmd"></a>[레이아웃 압축](layout-compression.md)

레이아웃 압축 페이지 렌더링 성능을 향상 시키기 위해 지정 된 레이아웃 시각적 트리에서 제거 합니다. 이로 인해 얻을 수 있는 성능 이점은 페이지의 복잡성, 사용 중인 운영 체제의 버전, 그리고 응용 프로그램이 실행 중인 장치에 따라 다릅니다. 하지만 가장 큰 성능 이점은 이전 버전의 장치에서 볼 수 있습니다.

## <a name="making-your-choice"></a>선택한

대부분의 경우에서 하나 이상의 레이아웃 선택 사용할 수 있다는 원하는 설계를 구현 하려면 주의 합니다. 사용자가 선택할 수 있는 여러 경우에 어떤 방법이 상황에 가장 쉬운 됩니다 하는 것이 좋습니다.
중첩 레이아웃으로 더 복잡 한 디자인을 만드는 데 필요 하므로 대부분의 디자인 한 레이아웃을 인식할 수 없습니다.


## <a name="related-links"></a>관련 링크

- [Apple의 휴먼 인터페이스 지침](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG)
- [Android 디자인 웹 사이트](https://developer.android.com/design/index.html)
- [레이아웃 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble 예제 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
