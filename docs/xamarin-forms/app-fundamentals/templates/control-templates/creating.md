---
title: ControlTemplate 만들기
description: 컨트롤 템플릿 응용 프로그램 수준 또는 페이지 수준에서 정의할 수 있습니다. 이 문서를 만들고 컨트롤 템플릿을 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: A9AEB052-FBF5-4589-9BD4-6D6F62BED7F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: b83668f6836b1d5d98f67592bf3e2b01e7319edc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998191"
---
# <a name="creating-a-controltemplate"></a>ControlTemplate 만들기

_컨트롤 템플릿 응용 프로그램 수준 또는 페이지 수준에서 정의할 수 있습니다. 이 문서를 만들고 컨트롤 템플릿을 사용 하는 방법을 보여 줍니다._

## <a name="creating-a-controltemplate-in-xaml"></a>XAML의 ControlTemplate 만들기

정의 하는 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) 응용 프로그램 수준를 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 에 추가 되어야 합니다는 `App` 클래스입니다. 기본적으로 템플릿에서 만든 모든 Xamarin.Forms 응용 프로그램 사용 합니다 **앱** 클래스를 구현 하는 [ `Application` ](xref:Xamarin.Forms.Application) 하위 클래스입니다. 선언 하는 `ControlTemplate` 응용 프로그램의 응용 프로그램 수준 `ResourceDictionary` XAML에서 기본값을 사용 하 여 **앱** 클래스는 XAML을 사용 하 여 바꾸어야 합니다 **앱** 클래스와 관련 된 코드 숨김,으로 다음 코드 예제에 표시 합니다.

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

각 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) 인스턴스가의 재사용 가능한 개체로 생성 되는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)합니다.  고유한 각 선언을 제공 하 여 이렇게 `x:Key` 에 설명이 포함 된 키를 사용 하 여 제공 하는 특성을 `ResourceDictionary`입니다.

다음 코드 예제에서는 연결 된 `App` 코드 숨김:

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

설정 뿐만 아니라 합니다 [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) 속성을 코드 숨김도 호출 해야 합니다는 `InitializeComponent` 로드 및 연결 된 XAML을 구문 분석 방법입니다.

다음 코드 예제는 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 적용 합니다 `TealTemplate` 에 [ `ContentView` ](xref:Xamarin.Forms.ContentView):

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

`TealTemplate` 에 할당 되는 [ `ContentView.ControlTemplate` ](xref:Xamarin.Forms.TemplatedView.ControlTemplate) 사용 하 여 속성은 `StaticResource` 태그 확장 합니다. [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) 속성을 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 에 표시할 콘텐츠를 정의 하는 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)합니다. 이 콘텐츠를 표시 합니다 [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter) 에 포함 된를 `TealTemplate`입니다. 이 인해 다음 스크린샷에 표시 된 모양:

![](creating-images/teal-theme.png "청록색 컨트롤 템플릿")

### <a name="re-theming-an-application-at-runtime"></a>런타임 시 응용 프로그램을 다시-테마 설정.

클릭 하는 **테마 변경** 단추를 실행 합니다 `OnButtonClicked` 메서드를 다음 코드 예제에 나와 있는:

```csharp
void OnButtonClicked (object sender, EventArgs e)
{
  originalTemplate = !originalTemplate;
  contentView.ControlTemplate = (originalTemplate) ? tealTemplate : aquaTemplate;
}
```

이 메서드는 활성 바꿉니다 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) 대체를 사용 하 여 인스턴스 `ControlTemplate` 인스턴스, 다음 스크린샷에 결과:

![](creating-images/aqua-theme.png "바다색 컨트롤 템플릿")

> [!NOTE]
> 에 `ContentPage`는 `Content` 속성을 할당할 수 있습니다 및 `ControlTemplate` 속성을 설정할 수도 있습니다. 이 경우 경우는 `ControlTemplate` 포함를 `ContentPresenter` 인스턴스, 할당 된 콘텐츠의 `Content` 속성에서 제공 하는 `ContentPresenter` 내에서 `ControlTemplate`.

### <a name="setting-a-controltemplate-with-a-style"></a>ControlTemplate을 스타일으로 설정합니다.

A [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) 를 통해 적용 될 수도 있습니다는 [ `Style` ](xref:Xamarin.Forms.Style) 를 더 테마 기능을 확장 합니다. 만들어 수행할 수 있습니다는 *암시적* 또는 *명시적* 대상 보기에 대 한 스타일을 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), 설정는 `ControlTemplate` 대상의 속성 보기를 [ `Style` ](xref:Xamarin.Forms.Style) 인스턴스. 다음 코드 예제는 *암시적* 스타일 응용 프로그램 수준에 추가 되었습니다 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary):

```xaml
<Style TargetType="ContentView">
    <Setter Property="ControlTemplate" Value="{StaticResource TealTemplate}" />
</Style>
```

때문에 합니다 [ `Style` ](xref:Xamarin.Forms.Style) 인스턴스가 *암시적*, 모두에 적용할 `ContentView` 응용 프로그램의 인스턴스. 따라서 것이 더 이상 설정 해야 합니다 [ `ContentView.ControlTemplate` ](xref:Xamarin.Forms.TemplatedView.ControlTemplate) 속성을 다음 코드 예제에서 설명한 것 처럼:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0">
      ...
    </ContentView>
</ContentPage>
```

스타일에 대 한 자세한 내용은 참조 하세요. [스타일](~/xamarin-forms/user-interface/styles/index.md)합니다.

### <a name="creating-a-controltemplate-at-page-level"></a>페이지 수준에서 ControlTemplate 만들기

외에도 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) 응용 프로그램 수준 인스턴스도 만들 수 있습니다 페이지 수준에서 다음 코드 예제 에서처럼:

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

추가 하는 경우는 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) 페이지 수준에서을 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) 에 추가 됩니다는 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)를 차례로 `ControlTemplate` 인스턴스가 포함 에 `ResourceDictionary`합니다.

## <a name="creating-a-controltemplate-in-c35"></a>C의 ControlTemplate 만들기&#35;

정의 하는 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) 응용 프로그램 수준를 `class` 나타내는 만들어야 합니다를 `ControlTemplate`입니다. 클래스에서 파생 되어야 합니다 [레이아웃](~/xamarin-forms/user-interface/layouts/index.md) 다음 코드 예제와 같이 서식 파일을 사용 중인:

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

`AquaTemplate` 클래스와 동일 합니다 `TealTemplate` 클래스를 제외 하 고 서로 다른 색에 사용 되는 [ `BoxView.Color` ](xref:Xamarin.Forms.BoxView.Color) 및 [ `Label.TextColor` ](xref:Xamarin.Forms.Label.TextColor) 속성.

다음 코드 예제는 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 적용 합니다 `TealTemplate` 에 [ `ContentView` ](xref:Xamarin.Forms.ContentView):

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

합니다 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) 의 컨트롤 템플릿을 정의 하는 클래스의 형식을 지정 하 여 인스턴스가 만들어집니다는 `ControlTemplate` 생성자입니다.

[ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) 속성을 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 에 표시할 콘텐츠를 정의 하는 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)합니다. 이 콘텐츠를 표시 합니다 [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter) 에 포함 된를 `TealTemplate`입니다. 이전에 설명 된 동일한 메커니즘이 런타임에 테마를 변경 하려면 사용 되는 `AquaTheme`합니다.

## <a name="summary"></a>요약

이 문서를 만들고 컨트롤 템플릿을 사용 하는 방법을 보여 줍니다. 컨트롤 템플릿 응용 프로그램 수준 또는 페이지 수준에서 정의할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [스타일](~/xamarin-forms/user-interface/styles/index.md)
- [간단한 테마 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simpletheme/)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
- [ContentView](xref:Xamarin.Forms.ContentView)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
