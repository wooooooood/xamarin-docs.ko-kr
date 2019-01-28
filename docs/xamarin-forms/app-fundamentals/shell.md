---
title: Xamarin.Forms Shell
description: 이 문서에서는 대부분의 애플리케이션에서 필요로 하는 기본 UI 기능을 제공하여 Xamarin.Forms 애플리케이션의 복잡성을 줄이는 Xamarin.Forms Shell을 사용하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 1A674212-72DB-4AA4-B626-A4EC135AD1A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2018
ms.openlocfilehash: 41530399bfc2210e7c3eda461688c12c6235ef79
ms.sourcegitcommit: 56b2f5cda7c37874618736d6129f19a8976826f0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/21/2019
ms.locfileid: "54418675"
---
# <a name="xamarinforms-shell"></a>Xamarin.Forms Shell

![미리 보기](~/media/shared/preview.png)

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/Microsoft/TailwindTraders-Mobile)

Xamarin.Forms Shell은 애플리케이션의 컨테이너로, 대부분의 애플리케이션에서 필요로 하는 기본 UI 기능을 제공하므로 애플리케이션의 핵심 워크로드에 집중할 수 있습니다. 셸 애플리케이션에는 다음과 같은 기능이 제공됩니다.

- 애플리케이션의 시각적 개체 구조를 설명하는 단일 위치입니다.
- 일반적인 탐색 사용자 인터페이스입니다.
- 딥 링크가 포함된 탐색 서비스입니다.
- 통합된 검색 처리기입니다.

이 기능은 애플리케이션의 복잡성을 줄이는 동시에 개발자 생산성을 향상시킵니다. 또한 셸은 렌더링 속도와 메모리 소모를 고려하여 작성되었습니다.

> [!IMPORTANT]
> 기존 iOS 및 Android 애플리케이션은 셸을 채택하고 탐색, 성능 및 확장성 향상으로 즉시 이점을 얻을 수 있습니다.

셸은 플라이아웃 및 탭을 기반으로 한 독자적인 탐색 사용자 인터페이스를 제공합니다. 셸 애플리케이션에서 탐색의 최상위 수준은 플라이아웃입니다.

![플라이아웃](shell-images/flyout.png "플라이아웃")

다음 수준의 탐색은 아래쪽 탭 표시줄입니다.

![아래쪽 탭](shell-images/bottom-tabs-full.png "아래쪽 탭")

플라이아웃이 열려 있지 않은 경우 아래쪽 탭 표시줄이 최상위 수준으로 간주됩니다.

각 아래쪽 탭 내에서 다음 탐색 수준은 상단 탭 표시줄이며, 여기서 각 탭은 `ContentPage`입니다.

![상위 탭](shell-images/top-tabs-full.png "상위 탭")

각 `ContentPage` 내에서 추가 `ContentPage` 인스턴스를 탐색 스택에 추가하거나 제거할 수 있습니다.

## <a name="bootstrapping-a-shell-application"></a>셸 애플리케이션 부트스트래핑

셸 애플리케이션은 `App` 클래스의 `MainPage` 속성을 셸 파일 인스턴스로 설정하여 부트스트랩됩니다.

```csharp
namespace TailwindTraders.Mobile
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();

            MainPage = new TheShell();
        }
    }
}
```

`TheShell` 클래스는 애플리케이션의 시각적 개체 구조를 설명하는 XAML 파일입니다.

> [!IMPORTANT]
> 셸은 현재 시험 단계이며 `Forms.Init` 메서드를 호출하기 전에 플랫폼 프로젝트에 `Forms.SetFlags("Shell_Experimental");`을(를) 추가하는 경우에만 사용할 수 있습니다.

# <a name="androidtabandroid"></a>[Android](#tab/android)

```csharp
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        global::Xamarin.Forms.Forms.SetFlags("Shell_Experimental");

        TabLayoutResource = Resource.Layout.Tabbar;
        ToolbarResource = Resource.Layout.Toolbar;

        base.OnCreate(savedInstanceState);

        global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
        LoadApplication(new App());
    }
}
```

# <a name="iostabios"></a>[iOS](#tab/ios)

```csharp
public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
{
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.SetFlags("Shell_Experimental");

        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new App());

        return base.FinishedLaunching(app, options);
    }
}
```

----

## <a name="shell-file-hierarchy"></a>셸 파일 계층 구조

셸 파일은 다음 세 가지 계층 구조 요소로 구성됩니다.

- `ShellItem`. 플라이아웃에 있는 하나 이상의 항목입니다. 모든 `ShellItem`은 `Shell`의 자식 항목입니다.
- `ShellSection`. 그룹화된 콘텐츠는 아래쪽 탭으로 이동할 수 있습니다. 모든 `ShellSection`은 `ShellItem`의 자식 항목입니다.
- `ShellContent`. 애플리케이션의 `ContentPage` 인스턴스로, 상단 탭으로 이동할 수 있습니다. 모든 `ShellContent`는 `ShellSection`의 자식 항목입니다.

이러한 요소 중 어떤 것도 사용자 인터페이스를 나타내지 않고 애플리케이션의 시각적 개체 구조를 구성합니다. 셸은 이러한 요소를 취해 콘텐츠에 대한 탐색 사용자 인터페이스를 생성합니다.

다음 XAML은 셸 파일의 간단한 예제를 보여 줍니다.

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:local="clr-namespace:TailwindTraders.Mobile.Features.Shell"
       x:Class="TailwindTraders.Mobile.Features.Shell.TheShell"
       Title="TailwindTraders">
    <ShellItem Title="Home">
        <ShellSection>
            <ShellContent>
                <local:HomePage />
            </ShellContent>
        </ShellSection>
    </ShellItem>
</Shell>
```

> [!NOTE]
> 각 `ShellItem`은 `FlyoutIcon` 속성을 제목 왼쪽에 표시되는 `ImageSource`로 설정할 수도 있습니다.

이 XAML은 셸 파일에 선언된 콘텐츠의 첫 번째 항목이므로 `HomePage`를 표시합니다.

![홈페이지](shell-images/homepage.png "홈페이지")

햄버거 단추를 누르면 플라이아웃이 표시됩니다.

![플라이아웃 열기](shell-images/flyout-initial.png "플라이아웃 열기")

플라이아웃의 항목 수는 셸 파일에 추가 `ShellItem` 인스턴스를 추가하여 늘릴 수 있습니다. 그러나 애플리케이션 시작 중에 `HomePage`가 생성되고 이 방법을 사용하여 추가 `ShellItem` 인스턴스를 추가하면 애플리케이션 시작 중에 이러한 페이지도 만들어집니다. `ShellContent.ContentTemplate` 속성을 `DataTemplate`으로 설정하여 이 문제를 방지할 수 있습니다.

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:local="clr-namespace:TailwindTraders.Mobile.Features.Shell"
       x:Class="TailwindTraders.Mobile.Features.Shell.TheShell"
       Title="TailwindTraders">
    <ShellItem Title="Home">
        <ShellSection>
            <ShellContent>
                <local:HomePage />
            </ShellContent>
        </ShellSection>
    <ShellItem>
    <ShellContent Title="Profile"
                  ContentTemplate="{DataTemplate local:ProfilePage}" />
</Shell>
```

이 방법을 사용하면 `ProfilePage`는 애플리케이션 시작 시가 아니라 사용자가 탐색할 때만 생성됩니다. 또한 `ProfilePage`의 경우 `ShellItem` 및 `ShellSection` 개체가 생략되고 `Title` 속성이 `ShellContent` 인스턴스에서 정의됩니다. 이 간결한 방법은 필요한 논리적 래퍼를 제공하는 셸(시각적 트리에 추가 보기를 도입하지 않고)과 함께 태그를 덜 필요로 합니다.

또한 각 `ShellItem` 모양은 `Shell.ItemTemplate` 속성을 `DataTemplate`으로 설정하여 사용자 지정할 수 있습니다.

```xaml
<Shell.ItemTemplate>
    <DataTemplate>
        <ContentView HeightRequest="32">
            <ContentView.Padding>
                <Thickness Left="32"
                           Top="16" />
            </ContentView.Padding>
            <Label Text="{Binding Title}" />
        </ContentView>
    </DataTemplate>
</Shell.ItemTemplate>          
```

이 코드는 플라이아웃 내의 각 `ShellItem`에 대한 텍스트의 위치를 변경합니다. 셸은 `DataTemplate`의 `BindingContext`에 `Title`(및 `Icon`) 속성을 제공합니다.

## <a name="flyout"></a>플라이아웃

플라이아웃은 애플리케이션의 루트 메뉴이며 플라이아웃 헤더와 메뉴 항목으로 구성됩니다.

![주석이 달린 플라이아웃](shell-images/flyout-annotated.png "주석이 달린 플라이아웃")

### <a name="flyout-behavior"></a>플라이아웃 동작

플라이아웃은 햄버거 단추나 화면 측면에서 스와핑하여 액세스할 수 있습니다. 그러나 이 동작은 `Shell.FlyoutBehavior` 속성을 `FlyoutBehavior` 열거형 멤버 중 하나로 설정하여 변경할 수 있습니다.

```xaml
<Shell ...
       FlyoutBehavior="Disabled">
    ...
</Shell>
```

`FlyoutBehavior` 속성을 `Disabled`로 설정하면 플라이아웃이 숨겨지므로 `ShellItem`이 하나만 있는 경우에 유용합니다. 다른 유효한 `FlyoutBehavior` 값은 `Flyout`(기본값) 및 `Locked`입니다.

### <a name="flyout-header"></a>플라이아웃 헤더

플라이아웃 헤더는 필요에 따라 플라이아웃 상단에 표시되는 콘텐츠로, 해당 모양은 `Shell.FlyoutHeader` 속성 값을 통해 설정할 수 있는 `View`에 의해 정의됩니다.

```xaml
<Shell.FlyoutHeader>
    <local:FlyoutHeader />
</Shell.FlyoutHeader>
```

또는 `Shell.FlyoutHeaderTemplate` 속성을 `DataTemplate`으로 설정하여 플라이아웃 헤더 모양을 정의할 수 있습니다.

```xaml
<Shell.FlyoutHeaderTemplate>
    <DataTemplate>
        <StackLayout HorizontalOptions="Fill"
                     VerticalOptions="Fill"
                     BackgroundColor="White"
                     Padding="16">
            <Label FontSize="Medium"
                   Text="Smart Shopping">
                <Label.Margin>
                    <Thickness Left="8" />
                </Label.Margin>
            </Label>
            <Button Image="photo"
                    Text="By taking a photo">
                <Button.Margin>
                    <Thickness Top="16" />
                </Button.Margin>
            </Button>
            <Button Image="ia"
                    Text="By using AR">
                <Button.Margin>
                    <Thickness Top="8" />
                </Button.Margin>
            </Button>
        </StackLayout>
    </DataTemplate>
</Shell.FlyoutHeaderTemplate>
```

이 XAML은 다음 플라이아웃 헤더를 생성합니다.

![플라이아웃 헤더](shell-images/flyout-header.png "플라이아웃 헤더")

기본적으로 플라이아웃 헤더는 플라이아웃에 고정되며 항목이 충분하면 아래 콘텐츠가 스크롤됩니다. 그러나 이 동작은 `Shell.FlyoutHeaderBehavior` 속성을 `FlyoutHeaderBehavior` 열거형 멤버 중 하나로 설정하여 변경할 수 있습니다.

```xaml
<Shell ...
       FlyoutHeaderBehavior="CollapseOnScroll">
    ...
</Shell>
```

`FlyoutHeaderBehavior`를 `CollapseOnScroll`로 설정하면 스크롤이 발생할 때 플라이아웃이 축소됩니다. 다른 유효한 `FlyoutHeaderBehavior` 값은 `Default`, `Fixed` 및 `Scroll`입니다(메뉴 항목으로 스크롤).

## <a name="menu-items"></a>메뉴 항목

플라이아웃의 항목 수는 추가 `ShellItem` 인스턴스를 추가하여 늘릴 수 있습니다. 그러나 플라이아웃에 `MenuItem` 인스턴스를 추가할 수도 있습니다. 이렇게 하면 `MenuItem.CommandParameter` 속성을 통해 데이터를 전달하면서 동일한 페이지로 이동하는 등의 시나리오를 사용할 수 있습니다.

`MenuItem` 인스턴스를 `Shell.MenuItems` 컬렉션에 추가해야 합니다.

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:local="clr-namespace:TailwindTraders.Views"
       x:Class="TailwindTraders.Shell"
       FlyoutHeaderBehavior="Fixed"
       Title="Tailwind Traders"
       ...>
    <Shell.MenuItems>
        <MenuItem Command="{Binding ProductTypeCommand}"
                  CommandParameter="1"
                  Text="Holiday decorations" />
        <MenuItem Command="{Binding ProductTypeCommand}"
                  CommandParameter="2"
                  Text="Appliances" />
        <MenuItem Command="{Binding ProductTypeCommand}"
                  CommandParameter="3"
                  Text="Bathrooms" />
        ...
    </Shell.MenuItems>
    <ShellItem Title="Home">
        <ShellSection>
            <ShellContent>
                <local:HomePage />
            </ShellContent>
        </ShellSection>
    </ShellItem>    
</Shell>
```

> [!NOTE]
> 각 `MenuItem`은 `Icon` 속성을 텍스트 왼쪽에 표시되는 `FileImageSource`로 설정할 수도 있습니다.

이 XAML은 다음 플라이아웃을 생성합니다.

![전체 플라이아웃](shell-images/flyout-full.png "전체 플라이아웃")

또한 `MenuItem` 모양은 `Shell.MenuItemTemplate` 속성을 `DataTemplate`으로 설정하여 사용자 지정할 수 있습니다.

```xaml
<Shell.MenuItemTemplate>
    <DataTemplate>
        <ContentView HeightRequest="32">
            <ContentView.Padding>
                <Thickness Left="32"
                           Top="16" />
            </ContentView.Padding>
            <Label Text="{Binding Text}"
                   FontAttributes="Bold" />
        </ContentView>
    </DataTemplate>
</Shell.MenuItemTemplate>
```

이로 인해 `MenuItem` 인스턴스의 텍스트가 굵게 렌더링됩니다.

![굵게 표시된 메뉴 항목](shell-images/menuitems-bold.png "굵게 표시된 메뉴 항목")

## <a name="tabs"></a>탭

단일 `ShellItem`에 여러 `ShellSection` 인스턴스가 있는 경우 `ShellSection` 인스턴스가 아래쪽 탭으로 렌더링됩니다.

```xaml
<ShellItem Title="Bottom Tab Sample"
           Style="{StaticResource BaseStyle}">
    <ShellSection Title="AR" Icon="ia.png">
        <ShellContent ContentTemplate="{DataTemplate local:ARPage}"/>
    </ShellSection>
    <ShellSection Title="Photo" Icon="photo.png">
        <ShellContent ContentTemplate="{DataTemplate local:PhotoPage}"/>
    </ShellSection>
</ShellItem>
```

이 예제에서는 `ShellSection` 인스턴스가 아래쪽 탭으로 렌더링됩니다.

![아래쪽 탭](shell-images/tabs-bottom.png "아래쪽 탭")

단일 `ShellSection` 내에 여러 `ShellContent` 인스턴스가 있는 경우 `ShellContent` 항목이 위쪽 탭으로 렌더링됩니다.

```xaml
<ShellItem Title="Store Home"
           Shell.TitleView="Store Home"
           Style="{StaticResource BaseStyle}">
    <ShellSection Title="Browse Product">
        <ShellContent Title="Featured"
                      ContentTemplate="{DataTemplate local:FeaturedPage}" />
        <ShellContent Title="On Sale"
                      ContentTemplate="{DataTemplate local:SalePage}" />
    </ShellSection>
</ShellItem>
```

이 예제에서는 `ShellContent` 인스턴스가 위쪽 탭으로 렌더링됩니다.

![상위 탭](shell-images/tabs-top.png "상위 탭")

탭은 XAML 스타일을 사용하거나 사용자 지정 렌더러를 제공하여 스타일을 지정할 수 있습니다. 예를 들어, 다음 예제에서는 탭 색을 설정하는 XAML 스타일을 보여줍니다.

```xaml
<Style x:Key="BaseStyle"
       TargetType="Element">
    <Setter Property="Shell.ShellTabBarBackgroundColor"
            Value="#3498DB" />
    <Setter Property="Shell.ShellTabBarTitleColor"
            Value="White" />
    <Setter Property="Shell.ShellTabBarUnselectedColor"
            Value="#B4FFFFFF" />
</Style>
```

## <a name="navigation"></a>탐색

셸은 URI 기반 탐색 환경을 포함합니다. URI는 집합 탐색 계층 구조를 수행하지 않고도 애플리케이션의 모든 페이지에 대한 탐색을 허용하는 향상된 탐색 환경을 제공합니다. 또한 탐색 스택의 모든 페이지를 방문하지 않고도 뒤로 이동할 수 있는 기능을 제공합니다.

이 URI 기반 탐색은 애플리케이션 내에서 이동하는 데 사용되는 URI 세그먼트인 경로를 통해 수행됩니다. 셸 파일은 경로 구성표, 경로 호스트 및 경로를 선언해야 합니다.

```xaml
<Shell ...
       Route="tailwindtraders"
       RouteHost="www.microsoft.com"
       RouteScheme="app">
    ...
</Shell>
```

`RouteScheme`, `RouteHost` 및 `Route` 속성 값이 결합되어 `app://www.microsoft.com/tailwindtraders` 루트 URI를 형성합니다.

셸 파일의 각 요소는 프로그래밍 방식 탐색에 사용할 수 있는 경로 속성도 정의할 수 있습니다.

셸 파일 생성자 또는 경로를 호출하기 전에 실행되는 다른 위치에서는 셸 요소가 표시되지 않는 모든 페이지(예: `MenuItem` 인스턴스)에 대해 추가 경로를 명시적으로 등록할 수 있습니다.

```csharp
Routing.RegisterRoute("productcategory", typeof(ProductCategoryPage));
```

### <a name="implementing-navigation"></a>탐색 구현

메뉴 항목은 탐색을 구현하는 데 사용할 수 있는 `Command` 속성을 노출합니다.

```csharp
public ICommand ProductTypeCommand { get; } = new Command<string>(NavigateToProductType);

static void NavigateToProductType(string typeId)
{
  (App.Current.MainPage as Xamarin.Forms.Shell).GoToAsync($"app:///tailwindtraders/productcategory?id={typeId}", true);
}
```

탐색을 호출하려면 `Application` 클래스의 `MainPage` 속성을 통해 `Shell` 애플리케이션에 대한 참조를 가져와야 합니다. 그런 다음, `GoToAsync` 메서드를 호출하여 유효한 URI를 인수로 전달하면 탐색이 호출될 수 있습니다. `GoToAsync` 메서드는 `string` 또는 `Uri`에서 구성되는 `ShellNavigationState` 개체를 사용하여 이동합니다.

### <a name="passing-data"></a>데이터 전달

쿼리 문자열 매개 변수를 통해 페이지 간 데이터를 전달할 수 있습니다. 클래스에 쿼리 속성 특성을 추가할 때 셸이 `ContentPage` 또는 ViewModel에 쿼리 문자열 매개 변수 값을 설정합니다.

```csharp
[QueryProperty("TypeID", "id")]
public partial class ProductCategoryPage : ContentPage
{
    private string _typeId;

    public ProductCategoryPage()
    {
        InitializeComponent();

        BindingContext = new ProductCategoryViewModel();
    }

    public string TypeID
    {
        get => _typeId;
        set => MyLabel.Text = value;
    }
}
```

`QueryProperty` 특성은 `TypeID`가 `GoToAsync` 메서드 호출의 URI에서 `id` 쿼리 문자열 매개 변수에 전달된 데이터를 수신하도록 지정합니다.

### <a name="intercepting-navigation"></a>탐색 가로채기

셸은 완료되기 전과 완료된 후에 탐색 경로에 연결하는 기능을 제공합니다. 이는 `Navigating` 및 `Navigated` 이벤트에 대한 이벤트 처리기를 각각 등록하여 수행할 수 있습니다.

```xaml
<Shell ...
       Navigating="OnShellNavigating">
    ...
</Shell>
```

```csharp
void OnShellNavigating(object sender, ShellNavigatingEventArgs e)
{
  if (...)
  {
    e.Cancel(); // Cancel the navigation
  }
}
```

`ShellNavigatingEventArgs` 클래스는 다음과 같은 속성을 제공합니다.

| 속성 | 형식 | 설명 |
|---|---|---|
| 현재 | `ShellNavigationState` | 현재 페이지의 URI입니다. |
| 소스 | `ShellNavigationState` | 탐색이 시작된 위치를 나타내는 URI입니다. |
| 대상 | `ShellNavigationSource`  | 탐색의 목적지를 나타내는 URI입니다. |
| CanCancel  | `bool` | 탐색을 취소할 수 있는지 여부를 나타내는 값입니다. |
| 취소됨  | `bool` | 탐색이 취소되었는지 여부를 나타내는 값입니다. |

또한 `ShellNavigatingEventArgs` 클래스는 `Cancel` 메서드를 제공합니다.

`ShellNavigatedEventArgs` 클래스는 다음과 같은 속성을 제공합니다.

| 속성 | 형식 | 설명 |
|---|---|---|
| 현재 | `ShellNavigationState` | 현재 페이지의 URI입니다. |
| 이전| `ShellNavigationState` | 이전 페이지의 URI입니다. |
| 소스  | `ShellNavigationSource` | 탐색이 시작된 위치를 나타내는 URI입니다. |

또한 셸은 뒤로 단추 누름을 가로채는 데 사용할 수 있는 재정의 가능한 `OnBackButtonPressed` 메서드를 제공합니다.

## <a name="related-links"></a>관련 링크

- [Tailwind Traders(샘플)](https://github.com/Microsoft/TailwindTraders-Mobile)
