---
title: Xamarin.Forms Shell 애플리케이션 만들기
description: Xamarin.Forms Shell 애플리케이션을 만들기 위한 프로세스는 Shell 클래스를 서브클래싱하고 애플리케이션의 App 클래스의 MainPage 속성을 서브클래싱된 Shell 개체로 설정한 다음 서브클래싱된 Shell 클래스에서 애플리케이션의 시각적 계층 구조를 설명하는 XAML 파일을 만드는 것입니다.
ms.prod: xamarin
ms.assetid: 2A51D78F-6CD5-4BC4-A62E-11CEFA799987
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/24/2019
ms.openlocfilehash: eec20ff6ceb4aee7e8fde59992576899690616c3
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "68739303"
---
# <a name="create-a-xamarinforms-shell-application"></a>Xamarin.Forms Shell 애플리케이션 만들기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Xamarin.Forms Shell 애플리케이션을 만들기 위한 프로세스는 다음과 같습니다.

1. 새 Xamarin.Forms 애플리케이션을 만들거나 Shell 애플리케이션으로 변환하려는 기존 애플리케이션을 로드합니다.
1. XAML 파일을 `Shell` 클래스를 서브클래싱하는 공유 코드 프로젝트에 추가합니다. 자세한 내용은 [Shell 클래스 서브클래싱](#subclass-the-shell-class)을 참조하세요.
1. 애플리케이션의 `App` 클래스의 [`MainPage`](xref:Xamarin.Forms.Application.MainPage) 속성을 서브클래싱된 `Shell` 개체로 설정합니다. 자세한 내용은 [Shell 애플리케이션 부트스트랩](#bootstrap-the-shell-application)을 참조하세요.
1. 서브클래싱된 `Shell` 클래스에서 애플리케이션의 시각적 계층 구조를 설명합니다. 자세한 내용은 [애플리케이션의 시각적 계층 구조 설명](#describe-the-visual-hierarchy-of-the-application)을 참조하세요.

## <a name="subclass-the-shell-class"></a>Shell 클래스 서브클래싱

Xamarin.Forms Shell 애플리케이션을 만드는 첫 번째 단계는 `Shell` 클래스를 서브클래싱하는 공유 코드 프로젝트에 XAML 파일을 추가하는 것입니다. 파일 이름은 원하는 대로 지정할 수 있지만 **AppShell**로 하는 것이 좋습니다. 다음 코드 예제는 새로 만든 **AppShell.xaml** 파일을 보여 줍니다.

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       x:Class="Xaminals.AppShell">

</Shell>
```

다음 예제는 코드 숨김 파일인 **AppShell.xaml.cs**를 보여 줍니다.

```csharp
using Xamarin.Forms;

namespace Xaminals
{
    public partial class AppShell : Shell
    {
        public AppShell()
        {
            InitializeComponent();
        }
    }
}
```

## <a name="bootstrap-the-shell-application"></a>Shell 애플리케이션 부트스트랩

`Shell` 개체를 서브클래싱하는 XAML 파일을 만든 후 `App` 클래스의 [`MainPage`](xref:Xamarin.Forms.Application.MainPage) 속성을 서브클래싱된 `Shell` 개체로 설정해야 합니다.

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

이 예제에서 `AppShell` 클래스는 `Shell` 클래스에서 파생되는 XAML 파일입니다.

> [!NOTE]
> 빈 Shell 애플리케이션이 빌드되는 동안 실행하면 `InvalidOperationException`이 throw됩니다.

## <a name="describe-the-visual-hierarchy-of-the-application"></a>애플리케이션의 시각적 계층 구조 설명

Xamarin.Forms Shell 애플리케이션을 만드는 마지막 단계는 서브클래싱된 `Shell` 클래스에서 애플리케이션의 시각적 계층을 설명하는 것입니다. 서브클래싱된 `Shell` 클래스는 세 가지 계층적 개체로 구성됩니다.

- `FlyoutItem` 또는 `TabBar` `FlyoutItem`은 플라이아웃에서 하나 이상의 항목을 나타내며, 애플리케이션에 대한 탐색 패턴이 플라이아웃을 포함하는 경우에 사용해야 합니다. `TabBar`는 아래쪽 탭 표시줄을 나타내며, 애플리케이션에 대한 탐색 패턴이 아래쪽 탭으로 시작될 때 사용해야 합니다. 모든 `FlyoutItem` 개체 또는 `TabBar` 개체는 `Shell` 개체의 자식입니다.
- `Tab` - 아래쪽 탭으로 이동할 수 있는 그룹화된 콘텐츠를 나타냅니다. 모든 `Tab` 개체는 `FlyoutItem` 개체 또는 `TabBar` 개체의 자식입니다.
- `ShellContent` - 애플리케이션에서 `ContentPage` 개체를 나타냅니다. 모든 `ShellContent` 개체는 `Tab` 개체의 자식입니다. 둘 이상의 `ShellContent` 개체가 `Tab`에 있으면 위쪽 탭으로 개체를 탐색할 수 있습니다.

이 개체 중 어떤 것도 사용자 인터페이스를 나타내지 않고 애플리케이션의 시각적 계층 구조를 구성합니다. 셸은 이 개체를 사용하고 콘텐츠에 대한 탐색 사용자 인터페이스를 생성합니다.

다음 XAML은 서브클래싱된 `Shell` 클래스의 예를 보여 줍니다.

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

실행될 때 이 XAML은 서브클래싱된 `Shell` 클래스에 선언된 콘텐츠의 첫 번째 항목이므로 `CatsPage`를 표시합니다.

[![iOS 및 Android의 셸 애플리케이션 스크린샷](create-images/cats.png "셸 애플리케이션")](create-images/cats-large.png#lightbox "셸 애플리케이션")

햄버거 아이콘을 누르거나 왼쪽에서 살짝 밀면 플라이아웃이 표시됩니다.

[![iOS 및 Android의 셸 플라이아웃 스크린샷](create-images/flyout-reduced.png "셸 플라이아웃")](create-images/flyout-reduced-large.png#lightbox "셸 플라이아웃")

> [!IMPORTANT]
> 셸 애플리케이션에서 `ShellContent` 개체의 자식인 각 [`ContentPage`](xref:Xamarin.Forms.ContentPage)는 애플리케이션을 시작하는 동안 만들어집니다. 이 방법을 사용하여 다른 `ShellContent` 개체를 추가하면 애플리케이션을 시작하는 동안 추가 페이지가 생성되어 시작 환경의 성능이 저하될 수 있습니다. 그러나 셸은 탐색에 대한 응답으로 요청 시 페이지를 만들 수도 있습니다. 자세한 내용은 [Xamarin.Forms Shell 탭](tabs.md) 가이드에서 [효율적인 페이지 로딩](tabs.md#efficient-page-loading)을 참조하세요.

## <a name="related-links"></a>관련 링크

- [Xaminals(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
