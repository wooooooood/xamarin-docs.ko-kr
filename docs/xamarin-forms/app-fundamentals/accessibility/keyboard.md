---
title: 키보드 탐색
description: 기본 탭 순서를 사용 하는 대신는 IsTapStop 및 TabIndex 속성을 조합 하 여 탭 순서를 지정 하 여 UI를 조정 하는 데 필요한 경우가 있습니다.
ms.prod: xamarin
ms.assetid: 8be8f498-558a-4894-a01f-91a0d3ef927e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/15/2018
ms.openlocfilehash: f703dff56d2947c35a9bc76e0eb909bfe9023bac
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131956"
---
# <a name="keyboard-navigation-in-xamarinforms"></a>Xamarin.Forms의 키보드 탐색

일부 사용자에 게 적절 한 키보드 액세스를 제공 하지 않는 응용 프로그램을 사용 하는 데 문제가 있습니다. 컨트롤에 대 한 탭 순서 지정 키보드 탐색을 사용 하도록 설정 하 고 특정 순서로 입력을 수신할 응용 프로그램 페이지를 준비 합니다.

기본적으로 컨트롤의 탭 순서는 XAML에 나오는 하거나 프로그래밍 방식으로 자식 컬렉션에 추가 되는 순서 대로 됩니다. 이 순서는 컨트롤을 이동 하 게 될 통해 키보드를 사용 하 여 순서 이며 종종이 기본 순서는 최적의 순서. 그러나 기본 순서가 다릅니다 항상 예상된 순서와 다음 XAML 코드 예제 에서처럼:

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="0.5*" />
        <ColumnDefinition Width="0.5*" />
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Label Text="You"
           HorizontalOptions="Center" />
    <Label Grid.Column="1"
           Text="Manager"
           HorizontalOptions="Center" />
    <Entry Grid.Row="1"
           Placeholder="Enter forename" />
    <Entry Grid.Column="1"
           Grid.Row="1"
           Placeholder="Enter forename" />
    <Entry Grid.Row="2"
           Placeholder="Enter surname" />
    <Entry Grid.Column="1"
           Grid.Row="2"
           Placeholder="Enter surname" />
</Grid>
```

다음 스크린샷은이 코드 예제에 대 한 기본 탭 순서를 보여 줍니다.

![](keyboard-images/default-tab-order.png "기본 행을 기준으로 탭 순서")

탭 순서의 다음 행 기반 이며 컨트롤의 XAML에 나열 된 순서로 정렬 됩니다. 따라서 forename 탐색 탭 키를 누르거나 [ `Entry` ](xref:Xamarin.Forms.Entry) 성 뒤에 인스턴스 `Entry` 인스턴스. 그러나 보다 직관적인 환경을 forename 성 쌍을 통해 이동 Tab 키를 누를 수 있도록 열의 첫 번째 탭 탐색을 사용할 것입니다. 이 입력된 컨트롤의 탭 순서를 지정 하 여 구현할 수 있습니다.

> [!NOTE]
> 유니버설 Windows 플랫폼에서 바로 가기 키 정의할 수 있습니다 터치 또는 마우스 대신 키보드를 통해 응용 프로그램의 표시 되는 UI를 사용 하 여 사용자가 신속 하 게 이동 하 고 상호 작용 하는 직관적인 방법을 제공 하는. 자세한 내용은 [VisualElement 선택 키 설정](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#visualelement-accesskeys)합니다.

## <a name="setting-the-tab-order"></a>탭 순서 설정

합니다 `VisualElement.TabIndex` 속성은 나타나는 순서를 나타내는 데 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) 인스턴스는 사용자가 Tab 키를 눌러 탐색 컨트롤을 통해 포커스를 받을. 속성의 기본값은 0 이며에 설정할 수 있습니다 `int` 값입니다.

다음 규칙이 적용 될 때 기본 탭 순서를 사용 하 여 가져오거나 설정 합니다 `TabIndex` 속성:

 - [`VisualElement`](xref:Xamarin.Forms.VisualElement) 사용 하 여 인스턴스를 `TabIndex` 0와 같은 XAML 또는 자식 컬렉션의 선언 순서에 따라 탭 순서에 추가 됩니다.
 - [`VisualElement`](xref:Xamarin.Forms.VisualElement) 사용 하 여 인스턴스를 `TabIndex` 탭 순서에 추가할 0 보다 큰 기준으로 해당 `TabIndex` 값입니다.
 - [`VisualElement`](xref:Xamarin.Forms.VisualElement) 사용 하 여 인스턴스를 `TabIndex` 미만인 0 탭 순서에 추가 되 고 값이 0 보다 먼저 나타나야 합니다.
 - 충돌을 `TabIndex` 선언 순서에 따라 확인 됩니다.

탭 순서를 정의한 후 Tab 키를 눌러는 순환 오름차순에서 컨트롤을 통해 포커스가 `TabIndex` 마지막 컨트롤에 도달 하면 시작 부분에 래핑을 순서입니다.

다음 XAML 예제에서는 `TabIndex` 열의 첫 번째 탭 탐색을 사용 하도록 설정 하려면 입력된 컨트롤에 설정 된 속성:

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="0.5*" />
        <ColumnDefinition Width="0.5*" />
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Label Text="You"
           HorizontalOptions="Center" />
    <Label Grid.Column="1"
           Text="Manager"
           HorizontalOptions="Center" />
    <Entry Grid.Row="1"
           Placeholder="Enter forename"
           TabIndex="1" />
    <Entry Grid.Column="1"
           Grid.Row="1"
           Placeholder="Enter forename"
           TabIndex="3" />
    <Entry Grid.Row="2"
           Placeholder="Enter surname"
           TabIndex="2" />
    <Entry Grid.Column="1"
           Grid.Row="2"
           Placeholder="Enter surname"
           TabIndex="4" />
</Grid>
```

다음 스크린샷은이 코드 예제에 대 한 탭 순서를 보여 줍니다.

![](keyboard-images/correct-tab-order.png "열 기반 탭 순서")

다음 탭 순서는 열 기반입니다. 따라서 forename 성 탐색 탭 키를 누르거나 [ `Entry` ](xref:Xamarin.Forms.Entry) 쌍입니다.

## <a name="excluding-controls-from-the-tab-order"></a>컨트롤 탭 순서에서 제외

컨트롤의 탭 순서를 설정 하는 것 외에도 컨트롤 탭 순서에서 제외 해야 할 수도 있습니다. 설정 하 여 이것이 달성 하는 한 가지 방법은 합니다 [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement) 컨트롤의 속성 `false`이므로 사용할 수 없는 컨트롤은 탭 순서에서 제외 됩니다.

그러나 기능이 설정 되지 않은 경우에 컨트롤 탭 순서에서 제외 해야 할 수도 있습니다. 사용 하 여 수행할 수 있습니다는 `VisualElement.IsTapStop` 속성을 나타내는 여부를 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) 탭 탐색에 포함 됩니다. 기본값은 `true`, 및 해당 값이 되는 경우 `false` 컨트롤 하는 경우와 상관 없이 탭 탐색 인프라에서 무시 됩니다는 `TabIndex` 설정 됩니다.

## <a name="supported-controls"></a>지원 되는 컨트롤

합니다 `TabIndex` 및 `IsTapStop` 속성이 하나 이상의 플랫폼에서 키보드 입력을 허용 하는 다음 컨트롤에 대해 지원 됩니다.

- [`Button`](xref:Xamarin.Forms.Button)
- [`DatePicker`](xref:Xamarin.Forms.DatePicker)
- [`Editor`](xref:Xamarin.Forms.Editor)
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)
- [`Picker`](xref:Xamarin.Forms.Picker)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)
- [`SearchBar`](xref:Xamarin.Forms.SearchBar)
- [`Slider`](xref:Xamarin.Forms.Slider)
- [`Stepper`](xref:Xamarin.Forms.Stepper)
- [`Switch`](xref:Xamarin.Forms.Switch)
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)
- [`TimePicker`](xref:Xamarin.Forms.TimePicker)

> [!NOTE]
> 이러한 각각의이 컨트롤 탭에 모든 플랫폼에서 포커스를 받을 수 없습니다.

## <a name="related-links"></a>관련 링크

- [내게 필요한 옵션 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
