---
title: 키보드 탐색
description: 기본 탭 시퀀스를 사용하지 않고 TabIndex 속성 및 IsTapStop 속성의 결합으로 탭 시퀀스를 지정하여 경우에 따라 UI를 조정할 필요가 있습니다.
ms.prod: xamarin
ms.assetid: 8be8f498-558a-4894-a01f-91a0d3ef927e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/15/2018
ms.openlocfilehash: f703dff56d2947c35a9bc76e0eb909bfe9023bac
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131956"
---
# <a name="keyboard-navigation-in-xamarinforms"></a>Xamarin.Forms의 키보드 탐색

일부 사용자는 적절한 키보드 액세스 권한을 제공하지 않는 애플리케이션을 사용하는 데 문제가 있을 수 있습니다. 컨트롤에 대한 탭 순서를 지정하면 키보드 탐색을 사용하도록 설정하고 특정 순서로 입력을 수신할 애플리케이션 페이지를 준비할 수 있습니다.

기본적으로 컨트롤의 탭 순서는 XAML에 나열되거나 프로그래밍 방식으로 자식 컬렉션에 추가되는 순서와 동일합니다. 이 순서는 컨트롤을 키보드로 탐색하는 순서이고 종종 이 기본 순서는 최적의 순서입니다. 그러나 기본 순서는 다음 XAML 코드 예제에 표시된 것처럼 예상 순서와 항상 동일하지는 않습니다.

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

다음 스크린샷은 이 코드 예제의 기본 탭 순서를 보여줍니다.

![](keyboard-images/default-tab-order.png "기본 행 기반 탭 순서")

여기서 탭 순서는 행 기반이며 컨트롤을 XAML에 나열한 순서와 같습니다. 따라서 탭 키를 누르면 이름 [`Entry`](xref:Xamarin.Forms.Entry) 인스턴스에 이어 성 `Entry` 인스턴스를 탐색합니다. 그러나 보다 직관적인 환경은 첫 번째 열 탭 탐색을 사용하므로 Tab 키를 누르면 이름-성 쌍을 탐색합니다. 입력 컨트롤의 탭 순서를 지정하여 이를 구현할 수 있습니다.

> [!NOTE]
> 유니버설 Windows 플랫폼에서 터치 또는 마우스 대신 키보드를 통해 사용자가 애플리케이션의 표시되는 UI와 상호 작용하고 빠르게 탐색할 수 있는 직관적인 방법을 제공하는 바로 가기 키를 정의할 수 있습니다. 자세한 내용은 [VisualElement 액세스 키 설정](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#visualelement-accesskeys)을 참조하세요.

## <a name="setting-the-tab-order"></a>탭 순서 설정

`VisualElement.TabIndex` 속성은 사용자가 Tab 키를 눌러 컨트롤을 탐색할 때 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 인스턴스가 포커스를 받는 순서를 나타내는 데 사용됩니다. 속성의 기본값은 0이며 모든 `int` 값으로 설정할 수 있습니다.

다음 규칙은 기본 탭 순서를 사용하거나 `TabIndex` 속성을 설정할 때 적용됩니다.

 - `TabIndex`가 0과 동일한 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 인스턴스를 XAML 또는 자식 컬렉션의 선언 순서에 따라 탭 순서에 추가합니다.
 - `TabIndex`가 0보다 큰 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 인스턴스를 해당 `TabIndex` 값에 따라 탭 순서에 추가합니다.
 - `TabIndex`가 0 미만인 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 인스턴스는 탭 순서에 추가되며 모든 0값 앞에 나타납니다.
 - `TabIndex`에서의 충돌은 선언 순서에 따라 해결됩니다.

탭 순서를 정의한 후 Tab 키를 누르면 `TabIndex` 오름차순으로 컨트롤을 통해 포커스가 순환되며 마지막 컨트롤에 도달하면 시작 부분으로 래핑됩니다.

다음 XAML 예제에서는 첫 번째 열 탭 탐색을 사용하도록 설정하려면 입력 컨트롤에 설정된 `TabIndex` 속성을 보여줍니다.

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

다음 스크린샷은 이 코드 예제의 탭 순서를 보여줍니다.

![](keyboard-images/correct-tab-order.png "열 기반 탭 순서")

여기서 탭 순서는 열 기반입니다. 따라서 Tab 키를 누르면 이름-성 [`Entry`](xref:Xamarin.Forms.Entry) 쌍을 탐색합니다.

## <a name="excluding-controls-from-the-tab-order"></a>탭 순서에서 컨트롤 제외

컨트롤의 탭 순서를 설정하는 것 외에 탭 순서에서 컨트롤을 제외해야 할 수도 있습니다. 이를 달성하는 한 가지 방법은 컨트롤의 [`IsEnabled`](xref:Xamarin.Forms.VisualElement) 속성을 `false`로 설정하는 것입니다. 사용할 수 없는 컨트롤은 탭 순서에서 제외되기 때문입니다.

그러나 컨트롤을 사용할 수 없는 경우에도 탭 순서에서 컨트롤을 제외해야 할 수 있습니다. 이 작업은 [`VisualElement`](xref:Xamarin.Forms.VisualElement)가 탭 탐색에 포함됐는지 여부를 나타내는 `VisualElement.IsTapStop` 속성을 통해 달성할 수 있습니다. 해당 기본값은 `true`이며, 해당 값이 `false`인 경우 컨트롤은 `TabIndex`가 설정됐는지 여부에 관계 없이 탭 탐색 인프라에서 무시됩니다.

## <a name="supported-controls"></a>지원되는 컨트롤

`TabIndex` 및 `IsTapStop` 속성은 하나 이상의 플랫폼에서 키보드 입력을 허용하는 다음 컨트롤에 대해 지원됩니다.

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
> 이러한 컨트롤 각각은 모든 플랫폼에서 탭 포커스를 받을 수 없습니다.

## <a name="related-links"></a>관련 링크

- [액세스 가능성(샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
