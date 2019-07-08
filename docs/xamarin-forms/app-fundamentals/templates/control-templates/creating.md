---
title: ControlTemplate 만들기
description: 컨트롤 템플릿은 애플리케이션 수준 또는 페이지 수준에서 정의할 수 있습니다. 이 문서에서는 컨트롤 템플릿을 만들고 사용하는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: A9AEB052-FBF5-4589-9BD4-6D6F62BED7F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/14/2019
ms.openlocfilehash: 0642f304589d30284bc8d3577c0383099e349033
ms.sourcegitcommit: 0fd04ea3af7d6a6d6086525306523a5296eec0df
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67513038"
---
# <a name="create-a-controltemplate"></a>ControlTemplate 만들기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/Templates/ControlTemplates/SimpleTheme/)

_컨트롤 템플릿은 애플리케이션 수준 또는 페이지 수준에서 정의할 수 있습니다. 이 문서에서는 컨트롤 템플릿을 만들고 사용하는 방법을 보여줍니다._

## <a name="create-a-controltemplate-in-xaml"></a>XAML에서 ControlTemplate 만들기

애플리케이션 수준에서 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)을 정의하려면 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)를 `App` 클래스에 추가해야 합니다. 기본적으로 템플릿에서 만든 모든 Xamarin.Forms 애플리케이션은 **App** 클래스를 사용하여 [`Application`](xref:Xamarin.Forms.Application) 서브클래스를 구현합니다. 애플리케이션 수준에서 `ControlTemplate`을 선언하려면 XAML을 사용하는 애플리케이션의 `ResourceDictionary`에서 다음 코드 예제와 같이 기본 **App** 클래스를 XAML **App** 클래스 및 연결된 코드 숨김으로 바꿔야 합니다.

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.App">
    <Application.Resources>
        <ResourceDictionary>
            <ControlTemplate x:Key="TealTemplate">
                <Grid>
                    ...
                    <BoxView ... />
                    <Label Text="Control Template Demo App"
                           TextColor="White"
                           VerticalOptions="Center" ... />
                    <ContentPresenter ... />
                    <BoxView Color="Teal" ... />
                    <Label Text="(c) Xamarin 2016"
                           TextColor="White"
                           VerticalOptions="Center" ... />
                </Grid>
            </ControlTemplate>
            <ControlTemplate x:Key="AquaTemplate">
                ...
            </ControlTemplate>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

각 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 인스턴스는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에서 재사용 가능한 개체로 생성됩니다.  이 작업은 `ResourceDictionary`에 설명적 키를 제공하는 고유한 `x:Key` 특성을 각 선언에 제공하여 수행합니다.

다음 코드 예제에서는 연결된 `App` 코드 숨김을 보여줍니다.

```csharp
public partial class App : Application
{
  public App ()
  {
    InitializeComponent ();
    MainPage = new HomePage ();
  }
}
```

[`MainPage`](xref:Xamarin.Forms.Application.MainPage) 속성을 설정할 것뿐만 아니라 코드 숨김에서 `InitializeComponent` 메서드를 호출하여 연결된 XAML을 로드하고 구문 분석해야 합니다.

다음 코드 예제에서는 `TealTemplate`을 [`ContentView`](xref:Xamarin.Forms.ContentView)에 적용하는 [`ContentPage`](xref:Xamarin.Forms.ContentPage)를 보여줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0"
                 ControlTemplate="{StaticResource TealTemplate}">
        <StackLayout VerticalOptions="CenterAndExpand">
            <Label Text="Welcome to the app!" HorizontalOptions="Center" />
            <Button Text="Change Theme" Clicked="OnButtonClicked" />
        </StackLayout>
    </ContentView>
</ContentPage>
```

`TealTemplate`은 `StaticResource` 태그 확장을 사용하여 [`ContentView.ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate) 속성에 할당됩니다. [`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content) 속성은 [`ContentPage`](xref:Xamarin.Forms.ContentPage)에 표시되는 콘텐츠를 정의하는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)으로 설정됩니다. 이 콘텐츠는 `TealTemplate`에 포함된 [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)로 표시됩니다. 이로 인해 결국 다음 스크린샷에 표시된 모양이 됩니다.

![](creating-images/teal-theme.png "청록색 컨트롤 템플릿")

### <a name="re-theming-an-application-at-runtime"></a>런타임 시 애플리케이션 테마 다시 설정

**테마 변경** 단추를 클릭하면 다음 코드 예제에 나와 있는 `OnButtonClicked` 메서드가 실행됩니다.

```csharp
void OnButtonClicked (object sender, EventArgs e)
{
  originalTemplate = !originalTemplate;
  contentView.ControlTemplate = (originalTemplate) ? tealTemplate : aquaTemplate;
}
```

이 메서드는 활성 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 인스턴스를 대체 `ControlTemplate` 인스턴스로 바꿔 다음 스크린샷과 같은 결과가 나타납니다.

![](creating-images/aqua-theme.png "바다색 컨트롤 템플릿")

> [!NOTE]
> `ContentPage`에서 `Content` 속성을 할당할 수 있으며 `ControlTemplate` 속성을 설정할 수도 있습니다. 이 경우 `ControlTemplate`에 `ContentPresenter` 인스턴스를 포함하면 `Content` 속성에 할당된 콘텐츠는 `ControlTemplate` 내에서 `ContentPresenter`에 의해 제공됩니다.

### <a name="set-a-controltemplate-with-a-style"></a>스타일을 사용하여 ControlTemplate 설정

[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)은 [`Style`](xref:Xamarin.Forms.Style)을 통해 더 확장된 테마 기능으로 적용될 수도 있습니다. [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에서 대상 보기에 대한 *암시적* 또는 *명시적* 스타일을 만들고, [`Style`](xref:Xamarin.Forms.Style) 인스턴스에서 대상 보기의 `ControlTemplate` 속성을 설정하여 수행할 수 있습니다. 다음 코드 예제는 애플리케이션 수준 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에 추가된 *암시적* 스타일을 보여줍니다.

```xaml
<Style TargetType="ContentView">
    <Setter Property="ControlTemplate" Value="{StaticResource TealTemplate}" />
</Style>
```

[`Style`](xref:Xamarin.Forms.Style) 인스턴스는 *암시적*이므로 애플리케이션의 모든 `ContentView` 인스턴스에 적용됩니다. 따라서 다음 코드 예제에서 설명한 것처럼 더 이상 [`ContentView.ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate) 속성을 설정할 필요가 없습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0">
      ...
    </ContentView>
</ContentPage>
```

스타일에 대한 자세한 내용은 [스타일](~/xamarin-forms/user-interface/styles/index.md)을 참조하세요.

### <a name="create-a-controltemplate-at-page-level"></a>페이지 수준에서 ControlTemplate 만들기

애플리케이션 수준에서 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 인스턴스를 만드는 것 외에도 다음 코드 예제에서처럼 페이지 수준에서 만들 수도 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentPage.Resources>
        <ResourceDictionary>
            <ControlTemplate x:Key="TealTemplate">
                ...
            </ControlTemplate>
            <ControlTemplate x:Key="AquaTemplate">
                ...
            </ControlTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
        ...
    </ContentView>
</ContentPage>
```

페이지 수준에서 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)을 추가하는 경우 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)는 [`ContentPage`](xref:Xamarin.Forms.ContentPage)에 추가된 다음, `ControlTemplate` 인스턴스가 `ResourceDictionary`에 포함됩니다.

## <a name="create-a-controltemplate-in-c35"></a>C&#35;에서 ControlTemplate 만들기

애플리케이션 수준에서 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)을 정의하려면 `ControlTemplate`을 나타내는 `class`를 만들어야 합니다. 클래스는 다음 코드 예제와 같이 템플릿에 사용되는 [레이아웃](~/xamarin-forms/user-interface/layouts/index.md)에서 파생되어야 합니다.

```csharp
class TealTemplate : Grid
{
  public TealTemplate ()
  {
    ...
    var contentPresenter = new ContentPresenter ();
    Children.Add (contentPresenter, 0, 1);
    Grid.SetColumnSpan (contentPresenter, 2);
    ...
  }
}

class AquaTemplate : Grid
{
  ...
}
```

`AquaTemplate` 클래스는 [`BoxView.Color`](xref:Xamarin.Forms.BoxView.Color) 및 [`Label.TextColor`](xref:Xamarin.Forms.Label.TextColor) 속성에 다른 색이 사용된다는 점을 제외하고 `TealTemplate` 클래스와 동일합니다.

다음 코드 예제에서는 `TealTemplate`을 [`ContentView`](xref:Xamarin.Forms.ContentView)에 적용하는 [`ContentPage`](xref:Xamarin.Forms.ContentPage)를 보여줍니다.

```csharp
public class HomePageCS : ContentPage
{
  ...
  ControlTemplate tealTemplate = new ControlTemplate (typeof(TealTemplate));
  ControlTemplate aquaTemplate = new ControlTemplate (typeof(AquaTemplate));

  public HomePageCS ()
  {
    var button = new Button { Text = "Change Theme" };
    var contentView = new ContentView {
      Padding = new Thickness (0, 20, 0, 0),
      Content = new StackLayout {
        VerticalOptions = LayoutOptions.CenterAndExpand,
        Children = {
          new Label { Text = "Welcome to the app!", HorizontalOptions = LayoutOptions.Center },
          button
        }
      },
      ControlTemplate = tealTemplate
    };
    ...
    Content = contentView;
  }
}
```

`ControlTemplate` 생성자에서 컨트롤 템플릿을 정의하는 클래스의 형식을 지정하여 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 인스턴스가 만들어집니다.

[`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content) 속성은 [`ContentPage`](xref:Xamarin.Forms.ContentPage)에 표시되는 콘텐츠를 정의하는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)으로 설정됩니다. 이 콘텐츠는 `TealTemplate`에 포함된 [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)로 표시됩니다. 런타임 시 테마를 `AquaTheme`로 변경하는 데 이전에 설명된 동일한 메커니즘이 사용됩니다.

## <a name="get-a-named-element-from-a-template"></a>템플릿에서 명명된 요소를 가져옵니다.

템플릿이 인스턴스화된 후 컨트롤 템플릿 내에서 명명된 요소를 검색할 수 있습니다. 이것은 인스턴스화된 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 시각적 트리에서 명명된 요소를 반환하는 `GetTemplateChild` 메서드를 사용하면 가능합니다.

컨트롤 템플릿이 인스턴스화된 후 템플릿의 `OnApplyTemplate` 메서드가 호출됩니다. 따라서 `GetTemplateChild` 메서드는 [`ContentPage`](xref:Xamarin.Forms.ContentPage)와 같은 [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage) 파생 페이지 또는 [`ContentView`](xref:Xamarin.Forms.ContentView)와 같은 [`TemplatedView`](xref:Xamarin.Forms.TemplatedView) 파생 보기의 `OnApplyTemplate` 재정의에서 호출되어야 합니다.

> [!IMPORTANT]
> `GetTemplateChild` 메서드는 `OnApplyTemplate` 메서드가 호출된 후에 호출되어야 합니다.

다음 예제에서는 사용자 지정 컨트롤의 컨트롤 템플릿을 보여 줍니다.

```xaml
<controls:MyCustomControl ...>
    <controls:MyCustomControl.ControlTemplate>
         <ControlTemplate>
              <Label x:Name="myLabel" />
         </ControlTemplate>
    <controls:MyCustomControl.ControlTemplate>
</controls:MyCustomControl>
```

[`Label`](xref:Xamarin.Forms.Label) 요소는 명명된 요소이므로 사용자 지정 컨트롤의 코드 숨김에서 검색할 수 있습니다. 이것은 사용자 지정 컨트롤에 대한 `OnApplyTemplate` 재정의에서 `GetTemplateChild` 메서드를 호출하면 가능합니다.

```csharp
class MyCustomControl : ContentView
{
    Label myLabel;

    protected override OnApplyTemplate()
    {  
        myLabel = GetTemplateChild("myLabel");
    }
    //...
}
```

이 예제에서는 `myLabel`로 명명된 [`Label`](xref:Xamarin.Forms.Label) 개체가 검색됩니다. 그런 다음 `myLabel`에 액세스하여 `MyCustomControl` 클래스로 조작할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [스타일](~/xamarin-forms/user-interface/styles/index.md)
- [간단한 테마(샘플)](https://developer.xamarin.com/samples/xamarin-forms/Templates/ControlTemplates/SimpleTheme/)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
- [ContentView](xref:Xamarin.Forms.ContentView)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
