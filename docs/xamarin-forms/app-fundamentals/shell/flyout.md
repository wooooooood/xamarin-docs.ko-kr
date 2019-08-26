---
title: Xamarin.Forms Shell 플라이아웃
description: 플라이아웃은 셸 애플리케이션의 루트 메뉴이며, 아이콘을 통해 또는 화면 측면에서 살짝 밀어 액세스할 수 있습니다. 플라이아웃은 선택적 헤더, 플라이아웃 항목 및 선택적 메뉴 항목으로 구성됩니다.
ms.prod: xamarin
ms.assetid: FEDE51EB-577E-4B3E-9890-B7C1A5E52516
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2019
ms.openlocfilehash: eaa29138f91fb8215e2c7c4e651baaf8e311f713
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69889192"
---
# <a name="xamarinforms-shell-flyout"></a>Xamarin.Forms Shell 플라이아웃

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

플라이아웃은 셸 애플리케이션의 루트 메뉴이며, 아이콘을 통해 또는 화면 측면에서 살짝 밀어 액세스할 수 있습니다. 플라이아웃은 선택적 헤더, 플라이아웃 항목 및 선택적 메뉴 항목으로 구성됩니다.

![셸 주석 처리 플라이아웃의 스크린샷](flyout-images/flyout-annotated.png "주석으로 처리된 플라이아웃")

필요한 경우 `Shell.FlyoutBackgroundColor` 바인딩 가능 속성을 통해 플라이아웃의 배경색을 [`Color`](xref:Xamarin.Forms.Color)로 설정할 수 있습니다. 이 속성은 CSS(CSS 스타일시트)에서 설정할 수도 있습니다. 자세한 내용은 [Xamarin.Forms 셸 특정 속성](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)을 참조하세요.

## <a name="flyout-icon"></a>플라이아웃 아이콘

기본적으로 셸 애플리케이션에는 누르면 플라이아웃을 여는 햄버거 아이콘이 있습니다. [`ImageSource`](xref:Xamarin.Forms.ImageSource) 형식의 `Shell.FlyoutIcon` 바인딩 가능 속성을 적절한 아이콘으로 설정하여 이 아이콘을 변경할 수 있습니다.

```xaml
<Shell ...
       FlyoutIcon="flyouticon.png">
    ...       
</Shell>
```

## <a name="flyout-behavior"></a>플라이아웃 동작

플라이아웃은 햄버거 아이콘을 통해 또는 화면 측면에서 살짝 밀어 액세스할 수 있습니다. 그러나 이 동작은 `Shell.FlyoutBehavior` 연결된 속성을 `FlyoutBehavior` 열거형 멤버 중 하나로 설정하여 변경할 수 있습니다.

- `Disabled` – 사용자가 플라이아웃을 열 수 없음을 나타냅니다.
- `Flyout` – 사용자가 플라이아웃을 열고 닫을 수 있음을 나타냅니다. 이 값은 `FlyoutBehavior` 속성의 기본값입니다.
- `Locked` – 사용자가 플라이아웃을 닫을 수 없고 콘텐츠를 겹치지 않음을 나타냅니다.

다음 예제에서는 플라이아웃을 사용하지 않도록 설정하는 방법을 보여 줍니다.

```xaml
<Shell ...
       FlyoutBehavior="Disabled">
    ...
</Shell>
```

> [!NOTE]
> `FlyoutBehavior` 연결된 속성은 `Shell`, `FlyoutItem`, `ShellContent` 및 페이지 개체에서 설정하여 기본 플라이아웃 동작을 재정의할 수 있습니다.

또한 플라이아웃은 현재 표시되는지 여부에 관계없이 `Shell.FlyoutIsPresented` 바인딩 가능 속성을 `boolean` 값으로 설정하여 프로그래밍 방식으로 열고 닫을 수 있습니다.

```csharp
Shell.Current.FlyoutIsPresented = false;
```

## <a name="flyout-header"></a>플라이아웃 헤더

플라이아웃 헤더는 필요에 따라 플라이아웃 상단에 표시되는 콘텐츠로, 해당 모양은 `Shell.FlyoutHeader` 속성 값을 통해 설정할 수 있는 `object`에 의해 정의됩니다.

```xaml
<Shell.FlyoutHeader>
    <controls:FlyoutHeader />
</Shell.FlyoutHeader>
```

`FlyoutHeader` 형식은 다음 예제에서 확인할 수 있습니다.

```xaml
<ContentView xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Xaminals.Controls.FlyoutHeader"
             HeightRequest="200">
    <Grid BackgroundColor="Black">
        <Image Aspect="AspectFill"
               Source="xamarinstore.jpg"
               Opacity="0.6" />
        <Label Text="Animals"
               TextColor="White"
               FontAttributes="Bold"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />
    </Grid>
</ContentView>
```

이를 통해 다음 플라이아웃 헤더가 생성됩니다.

![플라이아웃 헤더의 스크린샷](flyout-images/flyout-header.png "플라이아웃 헤더")

또는 `Shell.FlyoutHeaderTemplate` 속성을 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)으로 설정하여 플라이아웃 헤더 모양을 정의할 수 있습니다.

```xaml
<Shell.FlyoutHeaderTemplate>
    <DataTemplate>
        <Grid BackgroundColor="Black"
              HeightRequest="200">
            <Image Aspect="AspectFill"
                   Source="xamarinstore.jpg"
                   Opacity="0.6" />
            <Label Text="Animals"
                   TextColor="White"
                   FontAttributes="Bold"
                   HorizontalTextAlignment="Center"
                   VerticalTextAlignment="Center" />
        </Grid>            
    </DataTemplate>
</Shell.FlyoutHeaderTemplate>
```

기본적으로 플라이아웃 헤더는 플라이아웃에 고정되며 항목이 충분하면 아래 콘텐츠가 스크롤됩니다. 그러나 이 동작은 `Shell.FlyoutHeaderBehavior` 바인딩 가능 속성을 `FlyoutHeaderBehavior` 열거형 멤버 중 하나로 설정하여 변경할 수 있습니다.

- `Default` -플랫폼의 기본 동작이 사용됨을 나타냅니다. 이 값은 `FlyoutHeaderBehavior` 속성의 기본값입니다.
- `Fixed` – 플라이아웃 헤더가 항상 표시되고 변경되지 않음을 나타냅니다.
- `Scroll` – 사용자가 항목을 스크롤할 때 플라이아웃 헤더가 보기 밖으로 스크롤됨을 나타냅니다.
- `CollapseOnScroll` – 사용자가 항목을 스크롤할 때 플라이아웃 헤더가 제목으로만 축소됨을 나타냅니다.

다음 예제에서는 사용자가 스크롤할 때 플라이아웃 헤더를 축소하는 방법을 보여 줍니다.

```xaml
<Shell ...
       FlyoutHeaderBehavior="CollapseOnScroll">
    ...
</Shell>
```

## <a name="flyout-background-image"></a>플라이아웃 배경 이미지

플라이아웃은 플라이아웃 헤더 바로 아래, 플라이아웃 항목과 메뉴 항목 뒤에 표시되는 선택적 배경 이미지를 포함할 수 있습니다. [`ImageSource`](xref:Xamarin.Forms.ImageSource) 형식의 `FlyoutBackgroundImage` 바인딩 가능 속성을 파일, 포함 리소스, URI 또는 스트림으로 설정하여 배경 이미지를 지정할 수 있습니다.

[`Aspect`](xref:Xamarin.Forms.Aspect) 형식의 `FlyoutBackgroundImageAspect` 바인딩 가능 속성을 `Aspect` 열거형 멤버 중 하나로 설정하여 배경 이미지의 가로 세로 비율을 구성할 수 있습니다.

- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) - 이미지가 가로 세로 비율을 유지하면서 표시 영역을 채우도록 이미지를 자릅니다.
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) - 이미지가 표시 영역에 맞춰지도록, 필요에 따라 이미지의 너비 또는 높이를 기준으로 위쪽/아래쪽 또는 측면에 공백을 추가하여 이미지 레터박스를 지정합니다.
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) - 완전히 정확하게 표시 영역을 채우도록 이미지를 늘립니다. 이로 인해 이미지가 왜곡될 수 있습니다.

기본적으로 `FlyoutBackgroundImageAspect` 속성은 `AspectFit`로 설정됩니다.

다음 예제에서는 이러한 속성을 설정하는 방법을 보여 줍니다.

```xaml
<Shell ...
       FlyoutBackgroundImage="photo.jpg"
       FlyoutBackgroundImageAspect="AspectFill">
    ...
</Shell>
```

이 설정에 따라 배경 이미지가 플라이아웃에 표시됩니다.

![플라이아웃 배경 이미지 스크린샷](flyout-images/flyout-backgroundimage.png "플라이아웃 배경 이미지")

## <a name="flyout-items"></a>플라이아웃 항목

애플리케이션의 탐색 패턴이 플라이아웃을 포함하는 경우 `Shell` 개체는 각 `FlyoutItem` 개체가 플라이아웃의 항목을 나타내는 `FlyoutItem` 개체를 하나 이상 포함해야 합니다. 각 `FlyoutItem` 개체는 `Shell` 개체의 자식이어야 합니다.

다음 예제에서는 플라이아웃 헤더 및 두 플라이아웃 항목을 포함하는 플라이아웃을 만듭니다.

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>
    <FlyoutItem Title="Cats"
                Icon="cat.png">
        <Tab>
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
    <FlyoutItem Title="Dogs"
                Icon="dog.png">
        <Tab>
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
</Shell>
```

이 예제에서는 각 [`ContentPage`](xref:Xamarin.Forms.ContentPage)는 플라이아웃 항목을 통해서만 액세스할 수 있습니다.

[![iOS 및 Android에서 플라이아웃 항목이 있는 셸 2페이지 앱의 스크린샷](flyout-images/two-page-app-flyout.png "플라이아웃 항목이 있는 셸 2페이지 앱")](flyout-images/two-page-app-flyout-large.png#lightbox "플라이아웃 항목이 있는 셸 2페이지 앱")

> [!NOTE]
> 플라이아웃 헤더가 없으면 플라이아웃 항목이 플라이아웃 위쪽에 나타납니다. 플라이아웃 헤더가 있으면 플라이아웃 항목이 플라이아웃 헤더 아래에 나타납니다.

셸에는 시각적 트리에 추가적인 뷰를 도입하지 않고도 셸 시각적 계층 구조를 단순화할 수 있는 암시적 변환 연산자가 있습니다. 서브클래싱된 `Shell` 개체는 `FlyoutItem` 또는 `TabBar` 개체만 포함할 수 있고, 이 개체도  개체만 포함할 수 있고, 이 개체도 `Tab` 개체만 포함할 수 있고, 이 개체도 `ShellContent` 개체만 포함할 수 있기 때문에 이 작업이 가능합니다. 이 암시적 변환 연산자를 사용하여 이전 예제에서 `FlyoutItem`, `Tab` 및 `ShellContent` 개체를 제거할 수 있습니다.

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>
    <views:CatsPage IconImageSource="cat.png" />
    <views:DogsPage IconImageSource="dog.png" />
</Shell>
```

이 암시적 변환은 자동으로 각 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체를 `ShellContent` 개체로 래핑하고, 이 개체는 `Tab` 개체로 래핑되고, 이 개체는 `FlyoutItem` 개체로 래핑됩니다.

> [!IMPORTANT]
> 셸 애플리케이션에서 `ShellContent` 개체의 자식인 각 [`ContentPage`](xref:Xamarin.Forms.ContentPage)는 애플리케이션을 시작하는 동안 만들어집니다. 이 방법을 사용하여 다른 `ShellContent` 개체를 추가하면 애플리케이션을 시작하는 동안 추가 페이지가 생성되어 시작 환경의 성능이 저하될 수 있습니다. 그러나 셸은 탐색에 대한 응답으로 요청 시 페이지를 만들 수도 있습니다. 자세한 내용은 [Xamarin.Forms Shell 탭](tabs.md) 가이드에서 [효율적인 페이지 로딩](tabs.md#efficient-page-loading)을 참조하세요.

### <a name="flyoutitem-class"></a>FlyoutItem 클래스

`FlyoutItem` 클래스에는 플라이아웃 항목의 및 동작을 제어하는 다음 속성이 포함됩니다.

- `FlyoutDisplayOptions` 형식의 `FlyoutDisplayOptions` - 항목 및 해당 자식이 플라이아웃에 표시되는 방식을 정의합니다. 기본값은 `AsSingleItem`입니다.
- `Tab` 형식의 `CurrentItem` - 선택된 항목입니다.
- `IList<Tab>` 형식의 `Items` - `FlyoutItem` 내의 모든 탭을 정의합니다.
- `ImageSource` 형식의 `FlyoutIcon` - 항목에 사용할 아이콘입니다. 이 속성을 설정하지 않으면 `Icon` 속성 값이 대신 사용됩니다.
- `ImageSource` 형식의 `Icon` - 플라이아웃이 아닌 크롬의 일부로 표시할 아이콘을 정의합니다.
- `boolean` 형식의 `IsChecked` - 플라이아웃에서 현재 항목을 강조 표시할지 여부를 정의합니다.
- `boolean` 형식의 `IsEnabled` - 크롬에서 항목을 선택할 수 있는지 여부를 정의합니다.
- `bool` 형식의 `IsTabStop` - `FlyoutItem`이 탭 탐색에 포함되는지 여부를 나타냅니다. 해당 기본값은 `true`이며, 해당 값이 `false`인 경우 `FlyoutItem`은 `TabIndex`가 설정되었는지 여부에 관계없이 탭 탐색 인프라에서 무시됩니다.
- `int` 형식의 `TabIndex` - 사용자가 Tab 키를 눌러 항목을 탐색할 때 `FlyoutItem` 개체가 포커스를 받는 순서를 나타냅니다. 속성의 기본값은 0입니다.
- `string` 형식의 `Title` - UI에 표시할 제목입니다.
- `string` 형식의 `Route` - 항목을 처리하는 데 사용되는 문자열입니다.

`Route` 속성을 제외한 이 모든 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원되며, 이는 속성이 데이터 바인딩의 대상이 될 수 있음을 의미합니다.

> [!NOTE]
> 서브클래싱된 Shell 개체의 모든 `FlyoutItem` 개체는 플라이아웃에 표시될 항목 목록을 정의하는 `Shell.Items` 컬렉션에 추가됩니다.

또한 `FlyoutItem` 클래스는 다음 재정의 가능한 메서드를 공개합니다.

- `OnTabIndexPropertyChanged` - `TabIndex` 속성이 변경될 때마다 호출됩니다.
- `OnTabStopPropertyChanged` - `IsTabStop` 속성이 변경될 때마다 호출됩니다.
- `TabIndexDefaultValueCreator` - `int`를 반환하고 `TabIndex` 속성의 기본값을 설정하기 위해 호출됩니다.
- `TabStopDefaultValueCreator` - `bool`을 반환하고 `TabStop` 속성의 기본값을 설정하기 위해 호출됩니다.

## <a name="flyout-display-options"></a>플라이아웃 표시 옵션

`FlyoutDisplayOptions` 열거형은 다음 멤버를 정의합니다.

- `AsSingleItem` - 항목이 단일 항목으로 표시됨을 나타냅니다.
- `AsMultipleItems` - 항목 및 해당 자식이 플라이아웃에 항목 그룹으로 표시됨을 나타냅니다.

`FlyoutItem.FlyoutDisplayOptions` 속성을 `AsMultipleItems`로 설정하면 `FlyoutItem` 내의 각 `Tab`에 대한 플라이아웃 항목이 생성됩니다.

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       FlyoutHeaderBehavior="CollapseOnScroll"
       x:Class="Xaminals.AppShell">

    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>

    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        <Tab Title="Domestic"
             Icon="paw.png">
            <ShellContent Title="Cats"
                          Icon="cat.png"
                          ContentTemplate="{DataTemplate views:CatsPage}" />
            <ShellContent Title="Dogs"
                          Icon="dog.png"
                          ContentTemplate="{DataTemplate views:DogsPage}" />
        </Tab>
        <ShellContent Title="Monkeys"
                      Icon="monkey.png"
                      ContentTemplate="{DataTemplate views:MonkeysPage}" />
        <ShellContent Title="Elephants"
                      Icon="elephant.png"
                      ContentTemplate="{DataTemplate views:ElephantsPage}" />  
        <ShellContent Title="Bears"
                      Icon="bear.png"
                      ContentTemplate="{DataTemplate views:BearsPage}" />
    </FlyoutItem>

    <ShellContent Title="About"
                  Icon="info.png"
                  ContentTemplate="{DataTemplate views:AboutPage}" />    
</Shell>
```

이 예제에서 플라이아웃 항목은 `FlyoutItem` 개체의 자식인 `Tab` 개체 및 `FlyoutItem` 개체의 자식인 `ShellContent` 개체에 대해 생성됩니다. `FlyoutItem` 개체의 자식인 각 `ShellContent` 개체가 자동으로 `Tab` 개체로 래핑되기 때문에 이 작업이 수행됩니다. 또한 플라이아웃 항목은 최종 `ShellContent` 개체에 대해 생성되고, 이 개체는 `Tab` 개체로 래핑된 후 `FlyoutItem` 개체로 래핑됩니다.

이를 통해 다음 플라이아웃 항목이 생성됩니다.

[![iOS 및 Android에서 FlyoutItem 개체가 포함된 플라이아웃의 스크린샷](flyout-images/flyout-reduced.png "FlyoutItem 개체가 포함된 셸 플라이아웃")](flyout-images/flyout-reduced-large.png#lightbox "FlyoutItem 개체가 포함된 셸 플라이아웃")

## <a name="define-flyoutitem-appearance"></a>FlyoutItem 모양 정의

각 `FlyoutItem`의 모양은 `Shell.ItemTemplate`에 연결된 속성을 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)으로 설정하여 사용자 지정할 수 있습니다.

```xaml
<Shell ...>
    ...
    <Shell.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.8*" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding FlyoutIcon}"
                       Margin="5"
                       HeightRequest="45" />
                <Label Grid.Column="1"
                       Text="{Binding Title}"
                       FontAttributes="Italic"
                       VerticalTextAlignment="Center" />
            </Grid>
        </DataTemplate>
    </Shell.ItemTemplate>
</Shell>
```

이 예제에서는 각 `FlyoutItem` 개체의 제목을 기울임꼴로 표시합니다.

[![iOS 및 Android에서 템플릿 기반 FlyoutItem 개체의 스크린샷](flyout-images/flyoutitem-templated.png "셸 템플릿 기반 FlyoutItem 개체")](flyout-images/flyoutitem-templated-large.png#lightbox "셸 템플릿 기반 FlyoutItem 개체")

> [!NOTE]
> 셸은 `ItemTemplate`의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)에 `Title` 및 `FlyoutIcon` 속성을 제공합니다.

## <a name="flyoutitem-tab-order"></a>FlyoutItem 탭 순서

기본적으로 `FlyoutItem` 개체의 탭 순서는 XAML에 나열되거나 프로그래밍 방식으로 자식 컬렉션에 추가되는 순서와 동일합니다. 이 순서는 `FlyoutItem` 개체를 키보드로 탐색하는 순서이고 종종 이 기본 순서는 최적의 순서입니다.

사용자가 Tab 키를 눌러 항목을 탐색할 때 `FlyoutItem` 개체가 포커스를 받는 순서를 나타내는 `FlyoutItem.TabIndex` 속성을 설정하여 기본 탭 순서를 변경할 수 있습니다. 속성의 기본값은 0이며 모든 `int` 값으로 설정할 수 있습니다.

다음 규칙은 기본 탭 순서를 사용하거나 `TabIndex` 속성을 설정할 때 적용됩니다.

- `TabIndex`가 0과 동일한 `FlyoutItem` 개체를 XAML 또는 자식 컬렉션의 선언 순서에 따라 탭 순서에 추가합니다.
- `TabIndex`가 0보다 큰 `FlyoutItem` 개체를 해당 `TabIndex` 값에 따라 탭 순서에 추가합니다.
- `TabIndex`가 0 미만인 `FlyoutItem` 개체는 탭 순서에 추가되며 모든 0 값 앞에 나타납니다.
- `TabIndex`에서의 충돌은 선언 순서에 따라 해결됩니다.

탭 순서를 정의한 후 Tab 키를 누르면 `TabIndex` 오름차순으로 `FlyoutItem` 개체를 통해 포커스가 순환되며 마지막 개체에 도달하면 시작 부분으로 래핑됩니다.

`FlyoutItem` 개체의 탭 순서를 설정하는 것 외에 탭 순서에서 일부 개체를 제외해야 할 수도 있습니다. 이 작업은 `FlyoutItem`가 탭 탐색에 포함됐는지 여부를 나타내는 `FlyoutItem.IsTabStop` 속성을 통해 달성할 수 있습니다. 해당 기본값은 `true`이며, 해당 값이 `false`인 경우 `FlyoutItem`은 `TabIndex`가 설정되었는지 여부에 관계없이 탭 탐색 인프라에서 무시됩니다.

## <a name="set-the-current-flyoutitem"></a>현재 FlyoutItem 설정

`Shell` 클래스에는 현재 선택된 `FlyoutItem`을 나타내는 `FlyoutItem` 형식의 `CurrentItem`이라는 바인딩 가능한 속성이 있습니다. 셸 애플리케이션이 처음 실행될 때 이 속성은 서브클래싱된 `Shell` 개체의 첫 번째 `FlyoutItem`으로 설정됩니다. 그러나 이 속성은 다음 예제에 표시된 대로 다른 `FlyoutItem`으로 설정될 수 있습니다.

```xaml
<Shell ...
       CurrentItem="{x:Reference aboutItem}">
    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        ...
    </FlyoutItem>
    <ShellContent x:Name="aboutItem"
                  Title="About"
                  Icon="info.png"
                  ContentTemplate="{DataTemplate views:AboutPage}" />
</Shell>
```

이 코드는 `aboutItem`라는 `ShellContent` 개체를 `CurrentItem` 속성으로 설정하여 표시되도록 합니다. 이 예제에서 암시적 변환은 `ShellContent` 개체를 `Tab` 개체로 래핑하는 데 사용되며, 이 개체는 `FlyoutItem` 개체로 래핑됩니다.

해당하는 C# 코드는 다음과 같습니다.

```csharp
Shell.Current.CurrentItem = aboutItem;
```

## <a name="menu-items"></a>메뉴 항목

메뉴 항목은 플라이아웃에 선택적으로 추가될 수 있으며 각 메뉴 항목은 [`MenuItem`](xref:Xamarin.Forms.MenuItem) 개체로 표시됩니다. 플라이아웃에서 `MenuItem` 개체의 위치는 Shell 시각적 계층 구조에서의 선언 순서에 따라 달라집니다. 따라서 `FlyoutItem` 개체가 플라이아웃 상단에 나타나기 전에 선언된 `MenuItem` 개체 및 `FlyoutItem` 개체 후에 선언된 `MenuItem` 개체는 플라이아웃 하단에 나타납니다.

> [!NOTE]
> `MenuItem` 클래스에는 [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked) 이벤트 및 [`Command`](xref:Xamarin.Forms.MenuItem.Command) 속성이 포함됩니다. 따라서 `MenuItem` 개체를 통해 탭되는 `MenuItem`에 대한 응답으로 작업을 실행하는 시나리오를 실현합니다. 이 시나리오에는 탐색 수행 및 특정 웹 페이지에서 웹 브라우저 열기가 포함됩니다.

[`MenuItem`](xref:Xamarin.Forms.MenuItem) 개체를 다음 예제에서와 같이 플라이아웃에 추가할 수 있습니다.

```xaml
<Shell ...>
    ...            
    <MenuItem Text="Random"
              IconImageSource="random.png"
              Command="{Binding RandomPageCommand}" />
    <MenuItem Text="Help"
              IconImageSource="help.png"
              Command="{Binding HelpCommand}"
              CommandParameter="https://docs.microsoft.com/xamarin/xamarin-forms/app-fundamentals/shell" />    
</Shell>
```

이 코드는 2개의 [`MenuItem`](xref:Xamarin.Forms.MenuItem) 개체를 플라이아웃에서 모든 플라이아웃 항목 아래쪽에 추가합니다.

[![iOS 및 Android에서 MenuItem 개체가 포함된 플라이아웃의 스크린샷](flyout-images/flyout.png "MenuItem 개체가 포함된 셸 플라이아웃")](flyout-images/flyout-large.png#lightbox "MenuItem 개체가 포함된 셸 플라이아웃")

첫 번째 [`MenuItem`](xref:Xamarin.Forms.MenuItem) 개체는 애플리케이션의 임의 페이지로 이동하는 `RandomPageCommand`라는 `ICommand`를 실행합니다. 두 번째 `MenuItem` 개체는 웹 브라우저에서 `CommandParameter` 속성으로 지정된 URL을 여는 `HelpCommand`라는 `ICommand`를 실행합니다.

> [!NOTE]
> 각 `MenuItem`의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)는 서브클래싱된 `Shell` 개체에서 상속됩니다.

## <a name="define-menuitem-appearance"></a>MenuItem 모양 정의

각 `MenuItem`의 모양은 `Shell.MenuItemTemplate`에 연결된 속성을 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)으로 설정하여 사용자 지정할 수 있습니다.

```xaml
<Shell ...>
    <Shell.MenuItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.8*" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding Icon}"
                       Margin="5"
                       HeightRequest="45" />
                <Label Grid.Column="1"
                       Text="{Binding Text}"
                       FontAttributes="Italic"
                       VerticalTextAlignment="Center" />
            </Grid>
        </DataTemplate>
    </Shell.MenuItemTemplate>
    ...
    <MenuItem Text="Random"
              IconImageSource="random.png"
              Command="{Binding RandomPageCommand}" />
    <MenuItem Text="Help"
              IconImageSource="help.png"
              Command="{Binding HelpCommand}"
              CommandParameter="https://docs.microsoft.com/xamarin/xamarin-forms/app-fundamentals/shell" />  
</Shell>
```

이 예제에서는 Shell 수준 `MenuItemTemplate`을 각 `MenuItem` 개체에 첨부하여 각 `MenuItem` 개체의 제목을 기울임꼴로 표시합니다.

[![iOS 및 Android에서 템플릿 기반 MenuItem 개체의 스크린샷](flyout-images/menuitem-templated.png "셸 템플릿 기반 MenuItem 개체")](flyout-images/menuitem-templated-large.png#lightbox "셸 템플릿 기반 MenuItem 개체")

> [!NOTE]
> 셸은 `MenuItemTemplate`의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)에 [`Text`](xref:Xamarin.Forms.MenuItem.Text) 및 [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource) 속성을 제공합니다.

`Shell.MenuItemTemplate`은 연결된 속성이므로 특정 `MenuItem` 개체에 서로 다른 템플릿을 첨부할 수 있습니다.

```xaml
<Shell ...>
    <Shell.MenuItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.8*" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding Icon}"
                       Margin="5"
                       HeightRequest="45" />
                <Label Grid.Column="1"
                       Text="{Binding Text}"
                       FontAttributes="Italic"
                       VerticalTextAlignment="Center" />
            </Grid>
        </DataTemplate>
    </Shell.MenuItemTemplate>
    ...
    <MenuItem Text="Random"
              IconImageSource="random.png"
              Command="{Binding RandomPageCommand}" />
    <MenuItem Text="Help"
              Icon="help.png"
              Command="{Binding HelpCommand}"
              CommandParameter="https://docs.microsoft.com/xamarin/xamarin-forms/app-fundamentals/shell">
        <Shell.MenuItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </Shell.MenuItemTemplate>
    </MenuItem>
</Shell>
```

이 예제에서는 Shell 수준 `MenuItemTemplate`을 첫 번째 `MenuItem` 개체에 첨부하고 인라인 `MenuItemTemplate`을 두 번째 `MenuItem`에 첨부합니다.

## <a name="related-links"></a>관련 링크

- [Xaminals(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
