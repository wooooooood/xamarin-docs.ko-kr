---
title: Xamarin.Forms Shell 레이아웃
description: 플라이아웃 후에 셸 애플리케이션에서 다음 탐색 수준은 아래쪽 탭 표시줄입니다. 또는 애플리케이션에 대한 탐색 패턴은 아래쪽 탭에서 시작될 수 있으며 플라이아웃을 사용하지 않습니다. 두 경우 모두 아래쪽 탭에 둘 이상의 페이지가 포함되면 위쪽 탭으로 페이지를 탐색할 수 있습니다.
ms.prod: xamarin
ms.assetid: 318D81DB-E456-4E44-B083-36A27DBD9523
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/23/2019
ms.openlocfilehash: a6d99197adca9d07fec5b996d2a74e2ac336d2e9
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69888978"
---
# <a name="xamarinforms-shell-tabs"></a>Xamarin.Forms Shell 탭

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

애플리케이션에 대한 탐색 패턴이 플라이아웃을 포함하는 경우 애플리케이션에서 탐색의 다음 수준은 아래쪽 탭 표시줄입니다. 뿐만 아니라 플라이아웃이 닫히면 아래쪽 탭 표시줄을 최상위 탐색 수준으로 간주될 수 있습니다.

또는 애플리케이션에 대한 탐색 패턴은 아래쪽 탭에서 시작될 수 있으며 플라이아웃을 사용하지 않습니다. 이 시나리오에서 `Shell` 개체의 자식은 아래쪽 탭 표시줄을 나타내는 `TabBar` 개체여야 합니다.

> [!NOTE]
> `TabBar` 형식은 플라이아웃을 사용하지 않도록 설정합니다.

각 `FlyoutItem` 또는 `TabBar` 개체는 하나 이상의 `Tab` 개체를 포함할 수 있고, 각 `Tab` 개체는 아래쪽 탭 표시줄을 나타냅니다. 각 `Tab` 개체는 하나 이상의 `ShellContent` 개체를 포함할 수 있고, 각 `ShellContent` 개체는 단일 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체를 표시합니다. 둘 이상의 `ShellContent` 개체가 `Tab` 개체에 포함되면 위쪽 탭으로 `ContentPage` 개체를 탐색할 수 있습니다.

각 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체 내에서 추가 `ContentPage` 개체로 이동할 수 있습니다. 탐색에 대한 자세한 내용은 [Xamarin.Forms Shell 탐색](navigation.md)을 참조하세요.

## <a name="single-page-application"></a>단일 페이지 애플리케이션

가장 간단한 셸 애플리케이션은 단일 `Tab` 개체를 `TabBar` 개체에 추가하여 만들 수 있는 단일 페이지 애플리케이션입니다. `Tab` 개체 내에 있는 `ShellContent` 개체를 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체로 설정해야 합니다.

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab>
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
    </TabBar>
</Shell>
```

이 코드 예제에서는 다음 단일 페이지 애플리케이션이 생성됩니다.

[![iOS 및 Android에 있는 셸 단일 페이지 앱의 스크린샷](tabs-images/single-page-app.png "셸 단일 페이지 앱")](tabs-images/single-page-app-large.png#lightbox "셸 단일 페이지 앱")

> [!NOTE]
> 필요한 경우 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체에서 `Shell.NavBarIsVisible` 연결된 속성을 `false`로 설정하여 탐색 모음을 숨길 수 있습니다.

셸에는 시각적 트리에 추가적인 뷰를 도입하지 않고도 셸 시각적 계층 구조를 단순화할 수 있는 암시적 변환 연산자가 있습니다. 서브클래싱된 `Shell` 개체는 `FlyoutItem` 또는 `TabBar` 개체만 포함할 수 있고, 이 개체도  개체만 포함할 수 있고, 이 개체도 `Tab` 개체만 포함할 수 있고, 이 개체도 `ShellContent` 개체만 포함할 수 있기 때문에 이 작업이 가능합니다. 이 암시적 변환 연산자를 사용하여 이전 예제에서 `TabBar`, `Tab` 및 `ShellContent` 개체를 제거할 수 있습니다.

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <views:CatsPage />
</Shell>
```

이 암시적 변환은 자동으로 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체를 `ShellContent` 개체로 래핑하고, 이 개체는 `Tab` 개체로 래핑되고, 이 개체는 `FlyoutItem` 개체로 래핑됩니다. 단일 페이지 애플리케이션에는 플라이아웃이 필요하지 않으므로 `Shell.FlyoutBehavior` 속성이 `Disabled`로 설정됩니다.

> [!IMPORTANT]
> 셸 애플리케이션에서 `ShellContent` 개체의 자식인 각 [`ContentPage`](xref:Xamarin.Forms.ContentPage)는 애플리케이션을 시작하는 동안 만들어집니다. 이 방법을 사용하여 다른 `ShellContent` 개체를 추가하면 애플리케이션을 시작하는 동안 추가 페이지가 생성되어 시작 환경의 성능이 저하될 수 있습니다. 그러나 셸은 탐색에 대한 응답으로 요청 시 페이지를 만들 수도 있습니다. 자세한 내용은 [효율적인 페이지 로딩](tabs.md#efficient-page-loading)을 참조하세요.

## <a name="bottom-tabs"></a>아래쪽 탭

단일 `TabBar`에 여러 `Tab` 개체가 있는 경우 `Tab` 개체가 아래쪽 탭으로 렌더링됩니다.

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab Title="Cats"
             Icon="cat.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Dogs"
             Icon="dog.png">
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </TabBar>
</Shell>
```

탭 제목 및 아이콘은 각 `Tab` 개체에서 설정되고 아래쪽 탭에 표시됩니다.

[![iOS 및 Android에서 아래쪽 탭이 있는 셸 2페이지 앱의 스크린샷](tabs-images/two-page-app-bottom-tabs.png "아래쪽 탭이 있는 셸 2페이지 앱")](tabs-images/two-page-app-bottom-tabs-large.png#lightbox "아래쪽 탭이 있는 셸 2페이지 앱")

또는 셸의 암시적 변환 연산자를 사용하여 이전 예제에서 `ShellContent` 및 `Tab` 개체를 제거할 수 있습니다.

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <views:CatsPage IconImageSource="cat.png" />
        <views:DogsPage IconImageSource="dog.png" />
    </TabBar>
</Shell>
```

이 암시적 변환은 자동으로 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체를 `ShellContent` 개체로 래핑하고, 두 개체는 모두 `Tab` 개체로 래핑됩니다.

> [!IMPORTANT]
> 셸 애플리케이션에서 `ShellContent` 개체의 자식인 각 [`ContentPage`](xref:Xamarin.Forms.ContentPage)는 애플리케이션을 시작하는 동안 만들어집니다. 이 방법을 사용하여 다른 `ShellContent` 개체를 추가하면 애플리케이션을 시작하는 동안 추가 페이지가 생성되어 시작 환경의 성능이 저하될 수 있습니다. 그러나 셸은 탐색에 대한 응답으로 요청 시 페이지를 만들 수도 있습니다. 자세한 내용은 [효율적인 페이지 로딩](tabs.md#efficient-page-loading)을 참조하세요.

### <a name="tab-class"></a>Tab 클래스

`Tab` 클래스에는 탭 모양 및 동작을 제어하는 다음 속성이 포함됩니다.

- `ShellContent` 형식의 `CurrentItem` - 선택된 항목입니다.
- `FlyoutDisplayOptions` 형식의 `FlyoutDisplayOptions` - 항목 및 해당 자식이 플라이아웃에 표시되는 방식을 정의합니다. 기본값은 `AsSingleItem`입니다.
- `ImageSource` 형식의 `FlyoutIcon` - 플라이아웃에 표시될 아이콘을 정의합니다.
- `ImageSource` 형식의 `Icon` - 플라이아웃이 아닌 크롬의 일부로 표시할 아이콘을 정의합니다.
- `boolean` 형식의 `IsChecked` - 플라이아웃에서 현재 항목을 강조 표시할지 여부를 정의합니다.
- `boolean` 형식의 `IsEnabled` - 크롬에서 항목을 선택할 수 있는지 여부를 정의합니다.
- `bool` 형식의 `IsTabStop` - `Tab`이 탭 탐색에 포함되는지 여부를 나타냅니다. 해당 기본값은 `true`이며, 해당 값이 `false`인 경우 `Tab`은 `TabIndex`가 설정되었는지 여부에 관계없이 탭 탐색 인프라에서 무시됩니다.
- `IList<ShellContent>` 형식의 `Items` - `Tab` 내의 모든 콘텐츠를 정의합니다.
- `int` 형식의 `TabIndex` - 사용자가 Tab 키를 눌러 항목을 탐색할 때 `Tab` 개체가 포커스를 받는 순서를 나타냅니다. 속성의 기본값은 0입니다.
- `string` 형식의 `Title` - UI에서 탭에 표시할 제목입니다.

## <a name="shell-content"></a>셸 콘텐츠

모든 `Tab` 개체의 자식은 `ShellContent` 개체입니다. 이 개체의 `Content` 속성은 [`ContentPage`](xref:Xamarin.Forms.ContentPage)로 설정됩니다.

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab Title="Cats"
             Icon="cat.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Dogs"
             Icon="dog.png">
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </TabBar>
</Shell>
```

각 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체 내에서 추가 `ContentPage` 개체로 이동할 수 있습니다. 탐색에 대한 자세한 내용은 [Xamarin.Forms Shell 탐색](navigation.md)을 참조하세요.

> [!NOTE]
> 각 `ShellContent` 개체의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)는 부모 `Tab` 개체에서 상속됩니다.

### <a name="shellcontent-class"></a>ShellContent 클래스

`ShellContent` 클래스에는 탭 콘텐츠 모양 및 동작을 제어하는 다음 속성이 포함됩니다.

- `object` 형식의 `Content` - `ShellContent`의 콘텐츠입니다.
- `DataTemplate` 형식의 `ContentTemplate` - `ShellContent` 콘텐츠를 동적으로 팽창시키는 데 사용되는 템플릿입니다.
- `ImageSource` 형식의 `FlyoutIcon` - 플라이아웃에 표시될 아이콘을 정의합니다.
- `ImageSource` 형식의 `Icon` - 플라이아웃이 아닌 크롬의 일부로 표시할 아이콘을 정의합니다.
- `boolean` 형식의 `IsChecked` - 플라이아웃에서 현재 항목을 강조 표시할지 여부를 정의합니다.
- `boolean` 형식의 `IsEnabled` - 크롬에서 항목을 선택할 수 있는지 여부를 정의합니다.
- `MenuItemCollection` 형식의 `MenuItems` - 이 `ShellContent`가 제공된 페이지인 경우 플라이아웃에 표시할 메뉴 항목입니다.
- `string` 형식의 `Title` - UI에 표시할 제목입니다.

이 모든 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원되며, 이는 속성이 데이터 바인딩의 대상이 될 수 있음을 의미합니다.

## <a name="bottom-and-top-tabs"></a>아래쪽 및 위쪽 탭

둘 이상의 `ShellContent` 개체가 `Tab` 개체에 있는 경우 위쪽 탭 표시줄은 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체를 탐색할 수 있는 아래쪽 탭에 추가됩니다.

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab Title="Domestic"
             Icon="domestic.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Monkeys"
             Icon="monkey.png">
            <ShellContent>
                <views:MonkeysPage />
            </ShellContent>
        </Tab>
    </TabBar>
</Shell>
```

이를 통해 다음 스크린샷에 표시된 레이아웃이 생성됩니다.

[![iOS 및 Android에서 위쪽 및 아래쪽 탭이 있는 셸 2페이지 앱의 스크린샷](tabs-images/two-page-app-top-tabs.png "위쪽 및 아래쪽 탭이 있는 셸 2페이지 앱")](tabs-images/two-page-app-top-tabs-large.png#lightbox "위쪽 및 아래쪽 탭이 있는 셸 2페이지 앱")

또는 셸의 암시적 변환 연산자를 사용하여 이전 예제에서 `ShellContent` 개체 및 두 번째 `Tab` 개체를 제거할 수 있습니다.

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab Title="Domestic"
             Icon="domestic.png">
            <views:CatsPage />
            <views:DogsPage />
        </Tab>
        <views:MonkeysPage IconImageSource="monkey.png" />
    </TabBar>
</Shell>
```

이 암시적 변환은 자동으로 `MonkeysPage`를 `ShellContent` 개체로 래핑하고, 이 개체는 `Tab` 개체로 래핑됩니다. 또한 `CatsPage` 및 `DogsPage`는 암시적으로 `ShellContent` 개체로 래핑됩니다.

## <a name="efficient-page-loading"></a>효율적인 페이지 로딩

셸 애플리케이션에서 `ShellContent` 개체의 모든 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체는 애플리케이션을 시작하는 동안 생성되어 시작 환경이 성능이 저하될 수 있습니다. 그러나 셸은 탐색에 대한 응답으로 요청 시 페이지를 만들 수도 있습니다. `DataTemplate` 태그 확장을 사용하여 각 `ContentPage`를 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)으로 변환한 후 결과를 `ShellContent.ContentTemplate` 속성 값으로 설정하여 이 작업을 수행할 수 있습니다.

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">    
    <TabBar>
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
    </TabBar>    
</Shell>
```

이 XAML은 서브클래싱된 `Shell` 개체에 선언된 콘텐츠의 첫 번째 항목이므로 `CatsPage`를 만들고 표시합니다. 아래쪽 탭을 통해 `CatsPage` 및 `MonkeysPage`로 이동할 수 있고 이 페이지는 사용자가 해당 페이지로 이동할 경우에만 생성됩니다. 이 방법의 이점은, 애플리케이션 시작 시가 아니라 탐색에 대한 응답으로 요청 시 페이지가 만들어지므로 시작 환경의 성능이 저하되지 않는다는 것입니다.

## <a name="tab-appearance"></a>탭 모양

`Shell` 클래스는 탭 모양을 제어하는 다음 연결된 속성을 정의합니다.

- 탭 표시줄의 배경색을 정의하는 `Color` 형식의 `TabBarBackgroundColor`. 이 속성을 설정하지 않으면 `BackgroundColor` 속성 값이 사용됩니다.
- 탭 표시줄의 색을 사용하지 않도록 설정하는 `Color` 형식의 `TabBarDisabledColor`. 이 속성을 설정하지 않으면 `DisabledColor` 속성 값이 사용됩니다.
- 탭 표시줄의 전경색을 정의하는 `Color` 형식의 `TabBarForegroundColor`. 이 속성을 설정하지 않으면 `ForegroundColor` 속성 값이 사용됩니다.
- 탭 표시줄의 제목 색을 정의하는 `Color` 형식의 `TabBarTitleColor`. 이 속성을 설정하지 않으면 `TitleColor` 속성 값이 사용됩니다.
- 탭 표시줄의 선택되지 않은 색을 정의하는 `Color` 형식의 `TabBarUnselectedColor`. 이 속성을 설정하지 않으면 `UnselectedColor` 속성 값이 사용됩니다.

이 모든 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원되고 스타일이 지정되며, 이는 속성이 데이터 바인딩의 대상이 될 수 있음을 의미합니다.

다음 예제에서는 다양한 탭 색 속성을 설정하는 XAML 스타일을 보여 줍니다.

```xaml
<Style x:Key="BaseStyle"
       TargetType="Element">
    <Setter Property="Shell.TabBarBackgroundColor"
            Value="#3498DB" />
    <Setter Property="Shell.TabBarTitleColor"
            Value="White" />
    <Setter Property="Shell.TabBarUnselectedColor"
            Value="#B4FFFFFF" />
</Style>
```

CSS(CSS 스타일시트)를 사용하여 탭 스타일을 지정할 수도 있습니다. 자세한 내용은 [Xamarin.Forms 셸 특정 속성](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)을 참조하세요.

## <a name="related-links"></a>관련 링크

- [Xaminals(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Xamarin.Forms Shell 탐색](navigation.md)
- [Xamarin.Forms CSS Shell 특정 속성](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)
