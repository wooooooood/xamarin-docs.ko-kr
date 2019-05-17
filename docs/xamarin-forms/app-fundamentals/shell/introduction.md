---
title: Xamarin.Forms Shell 소개
description: Xamarin.Forms Shell은 일반 탐색 사용자 환경, URI 기반 탐색 체계 및 통합 검색 처리기를 포함하여 대부분 애플리케이션에 필요한 기본 기능을 제공합니다.
ms.prod: xamarin
ms.assetid: 4604DCB5-83DA-458A-8B02-6508A740BE0E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 20d9fb79d03990824dd884b62138a3e29b3ee04f
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65054483"
---
# <a name="xamarinforms-shell"></a>Xamarin.Forms Shell

![](~/media/shared/preview.png "이 API는 현재 시험판임")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/Xaminals/)

Xamarin.Forms Shell은 다음을 비롯한 대부분의 모바일 애플리케이션에 필요한 기본 기능을 제공하여 모바일 애플리케이션 개발의 복잡성을 줄입니다.

- 애플리케이션의 시각적 계층 구조를 설명하는 단일 위치입니다.
- 일반적인 탐색 사용자 환경입니다.
- 애플리케이션의 모든 페이지에 대한 탐색을 허용하는 URI 기반 탐색 체계입니다.
- 통합된 검색 처리기입니다.

또한 셸 애플리케이션은 렌더링 속도 향상 및 메모리 사용 감소의 혜택을 받습니다.

> [!IMPORTANT]
> 기존 iOS 및 Android 애플리케이션은 셸을 채택하고 탐색, 성능 및 확장성 향상으로 즉시 이점을 얻을 수 있습니다.

셸은 현재 시험 단계이며 `Forms.Init` 메서드를 호출하기 전에 플랫폼 프로젝트에 `Forms.SetFlags("Shell_Experimental");`을(를) 추가하는 경우에만 사용할 수 있습니다.

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

## <a name="shell-navigation-experience"></a>셸 탐색 환경

셸은 플라이아웃 및 탭을 기반으로 한 독자적인 탐색 환경을 제공합니다. 셸 애플리케이션에서 탐색의 최상위 수준은 플라이아웃입니다.

[![iOS 및 Android에서 셸 플라이아웃의 스크린샷](introduction-images/flyout.png "셸 플라이아웃")](introduction-images/flyout-large.png#lightbox "셸 플라이아웃")

플라이아웃을 선택하면 항목을 나타내는 아래쪽 탭이 선택되고 표시됩니다.

[![iOS 및 Android에서 셸 아래쪽 탭의 스크린샷](introduction-images/monkeys.png "셸 아래쪽 탭")](introduction-images/monkeys-large.png#lightbox "셸 아래쪽 탭")

> [!NOTE]
> 플라이아웃이 열리지 않으면 아래쪽 탭 표시줄을 애플리케이션의 최상위 탐색 수준으로 간주합니다.

각 탭에는 [`ContentPage`](xref:Xamarin.Forms.ContentPage)가 표시됩니다. 그러나 아래쪽 탭에 둘 이상의 페이지가 포함되면 위쪽 탭 표시줄을 통해 페이지를 탐색할 수 있습니다.

[![iOS 및 Android에서 셸 위쪽 탭의 스크린샷](introduction-images/cats.png "셸 위쪽 탭")](introduction-images/cats-large.png#lightbox "셸 위쪽 탭")

각 탭 내에서 추가 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체로 이동할 수 있습니다.

[![iOS 및 Android에서 셸 페이지 탐색의 스크린샷](introduction-images/cat-details.png "셸 앱 탐색")](introduction-images/cat-details-large.png#lightbox "셸 앱 탐색")

## <a name="subclassing-the-shell-class"></a>셸 클래스 서브클래싱

서브클래싱된 `Shell` 개체는 셸 애플리케이션의 시각적 계층 구조를 설명하고 다음 세 가지 주요 계층적 개체로 구성됩니다.

- `FlyoutItem` - 플라이아웃에서 하나 이상의 항목을 나타냅니다. 모든 `FlyoutItem` 개체는 `Shell` 개체의 자식입니다.
- `Tab` - 아래쪽 탭으로 이동할 수 있는 그룹화된 콘텐츠를 나타냅니다. 모든 `Tab` 개체는 `FlyoutItem` 개체의 자식입니다.
- `ShellContent` - 애플리케이션에서 `ContentPage` 개체를 나타냅니다. 모든 `ShellContent` 개체는 `Tab` 개체의 자식입니다. 둘 이상의 `ShellContent` 개체가 `Tab`에 있으면 위쪽 탭으로 개체를 탐색할 수 있습니다.

이 개체 중 어떤 것도 사용자 인터페이스를 나타내지 않고 애플리케이션의 시각적 계층 구조를 구성합니다. 셸은 이 개체를 사용하고 콘텐츠에 대한 탐색 사용자 인터페이스를 생성합니다.

> [!NOTE]
> `FlyoutItem` 클래스는 `ShellItem` 클래스의 별칭이고 `Tab` 클래스는 `ShellSection` 클래스의 별칭입니다. 이 별칭은 더 친숙하게 사용할 수 있는 API를 만들도록 고안되었습니다.

다음 XAML은 서브클래싱된 `Shell` 개체의 예를 보여 줍니다.

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    ...
    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        <Tab Title="Domestic"
             Icon="paw.png">
            <ShellContent Title="Cats"
                          Icon="cat.png">
                <views:CatsPage />
            </ShellContent>
            <ShellContent Title="Dogs"
                          Icon="dog.png">
                <views:DogsPage />
            </ShellContent>
        </Tab>
        <ShellContent Title="Monkeys"
                      Icon="monkey.png">
            <views:MonkeysPage />
        </ShellContent>
        <ShellContent Title="Elephants"
                      Icon="elephant.png">  
            <views:ElephantsPage />
        </ShellContent>
        <ShellContent Title="Bears"
                      Icon="bear.png">
            <views:BearsPage />
        </ShellContent>
    </FlyoutItem>
    ...
</Shell>
```

실행될 때 이 XAML은 서브클래싱된 `Shell` 개체에 선언된 콘텐츠의 첫 번째 항목이므로 `CatsPage`를 표시합니다.

[![iOS 및 Android에서 셸 앱의 스크린샷](introduction-images/cats.png "셸 앱")](introduction-images/cats-large.png#lightbox "셸 앱")

햄버거 아이콘을 누르거나 왼쪽에서 살짝 밀면 플라이아웃이 표시됩니다.

[![iOS 및 Android에서 셸 플라이아웃의 스크린샷](introduction-images/flyout-reduced.png "셸 플라이아웃")](introduction-images/flyout-reduced-large.png#lightbox "셸 플라이아웃")

> [!IMPORTANT]
> 셸 애플리케이션에서 `ShellContent` 개체의 자식인 각 [`ContentPage`](xref:Xamarin.Forms.ContentPage)는 애플리케이션을 시작하는 동안 만들어집니다. 이 방법을 사용하여 다른 `ShellContent` 개체를 추가하면 애플리케이션을 시작하는 동안 추가 페이지가 생성되어 시작 환경의 성능이 저하될 수 있습니다. 그러나 셸은 탐색에 대한 응답으로 요청 시 페이지를 만들 수도 있습니다. 자세한 내용은 [Xamarin.Forms Shell 탭](tabs.md) 가이드에서 [효율적인 페이지 로딩](tabs.md#efficient-page-loading)을 참조하세요.

## <a name="bootstrapping-a-shell-application"></a>셸 애플리케이션 부트스트래핑

셸 애플리케이션은 `App` 클래스의 [`MainPage`](xref:Xamarin.Forms.Application.MainPage) 속성을 서브클래싱된 `Shell` 개체로 설정하여 부트스트랩됩니다.

```csharp
namespace Xaminals
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new AppShell();
        }
        ...
    }
}
```

이 예제에서 `AppShell` 클래스는 애플리케이션의 시각적 계층을 설명하는 `Shell` 클래스에서 파생되는 XAML 파일입니다.

## <a name="shell-appearance"></a>셸 모양

`Shell` 클래스는 셸 애플리케이션의 모양을 제어하는 다음 속성을 정의합니다.

- `Color` 형식의 `ShellBackgroundColor` - 셸 크롬의 배경색을 정의하는 연결된 속성입니다. 셸 콘텐츠 뒤의 색은 채워지지 않습니다.
- `Color` 형식의 `ShellDisabledColor` - 사용할 수 없는 텍스트 및 아이콘을 음영 처리할 색을 정의하는 연결된 속성입니다.
- `Color` 형식의 `ShellForegroundColor` - 텍스트 및 아이콘을 음영 처리할 색을 정의하는 연결된 속성입니다.
- `Color` 형식의 `ShellTitleColor` - 현재 페이지의 제목에 사용되는 색을 정의하는 연결된 속성입니다.
- `Color` 형식의 `ShellUnselectedColor` - 셸 크롬에서 선택되지 않은 텍스트 및 아이콘에 사용되는 색을 정의하는 연결된 속성입니다.

이 모든 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원되며, 이는 속성이 데이터 바인딩의 대상이 될 수 있음을 의미합니다.

CSS(CSS 스타일시트)를 사용하여 이 속성을 설정할 수도 있습니다. 자세한 내용은 [Xamarin.Forms 셸 특정 속성](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)을 참조하세요.

## <a name="shell-content-layout"></a>셸 콘텐츠 레이아웃

`Shell` 클래스는 셸 애플리케이션 콘텐츠의 레이아웃에 영향을 주는 다음 속성을 정의합니다.

- `boolean` 형식의 `NavBarIsVisible` - 페이지가 제공될 때 탐색 모음을 표시할지 여부를 정의하는 연결된 속성입니다. 이 속성은 페이지에서 설정해야 하며 기본값은 `true`입니다.
- `bool` 형식의 `SetPaddingInsets` - 페이지 콘텐츠가 셸 크롬 아래에 가로질러 표시될지 여부를 제어하는 연결된 속성입니다. 이 속성은 페이지에서 설정해야 하며 기본값은 `false`입니다.
- `bool` 형식의 `TabBarIsVisible` - 페이지가 제공될 때 탭 표시줄을 표시할지 여부를 정의하는 연결된 속성입니다. 이 속성은 페이지에서 설정해야 하며 기본값은 `true`입니다.
- `View` 형식의 `TitleView` - 페이지의 `TitleView`를 정의하는 연결된 속성입니다. 이 속성은 페이지에서 설정해야 합니다.

이 모든 속성은 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 개체에서 지원되며, 이는 속성이 데이터 바인딩의 대상이 될 수 있음을 의미합니다.

## <a name="related-links"></a>관련 링크

- [Xaminals(샘플)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/Xaminals/)
- [Xamarin.Forms 셸 특정 속성](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)
