---
title: "ControlTemplate 만들기"
description: "컨트롤 템플릿은 응용 프로그램 수준 또는 페이지 수준에서 정의할 수 있습니다. 이 문서를 만들고 컨트롤 템플릿을 사용 하는 방법을 보여 줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: A9AEB052-FBF5-4589-9BD4-6D6F62BED7F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: e09ac502298296277d9264bcd18f1ce1cbbf0c55
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-controltemplate"></a>ControlTemplate 만들기

_컨트롤 템플릿은 응용 프로그램 수준 또는 페이지 수준에서 정의할 수 있습니다. 이 문서를 만들고 컨트롤 템플릿을 사용 하는 방법을 보여 줍니다._

## <a name="creating-a-controltemplate-in-xaml"></a>XAML의 한 ControlTemplate 만들기

정의 하는 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) 응용 프로그램 수준에서 한 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 에 추가 되어야 합니다는 `App` 클래스입니다. 기본적으로 템플릿에서 만든 모든 Xamarin.Forms 응용 프로그램 사용는 **앱** 클래스를 구현 하는 [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) 하위 클래스입니다. 선언 하는 `ControlTemplate` 응용 프로그램의 응용 프로그램 수준 `ResourceDictionary` XAML에서는 기본값을 사용 하 여 **앱** 클래스는 XAML로 바꾸어야 합니다 **앱** 클래스와 관련 된 코드 숨김,으로 다음 코드 예제에 나와 있습니다.

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

각 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) 인스턴스가에서 다시 사용할 수 있는 개체로 생성 되는 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)합니다.  고유한 각 선언을 제공 하 여 이렇게 `x:Key` 에 설명이 포함 된 키가 있는 제공 하는 특성의 `ResourceDictionary`합니다.

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

설정 뿐만 아니라는 [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) 속성을 코드 숨김 호출 또한 해야는 `InitializeComponent` 메서드를 로드 및 연결 된 XAML을 구문 분석 합니다.

다음 코드 예제는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 적용 된 `TealTemplate` 에 [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

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

`TealTemplate` 에 할당 되는 [ `ContentView.ControlTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TemplatedView.ControlTemplate/) 사용 하 여 속성의 `StaticResource` 태그 확장 합니다. [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) 속성이로 설정 되는 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 에 표시할 콘텐츠를 정의 하는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)합니다. 이 콘텐츠 표시 하는 [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) 에 포함 된는 `TealTemplate`합니다. 다음 스크린샷에 표시 된 모양 결과이 됩니다.

![](creating-images/teal-theme.png "청록색 컨트롤 템플릿")

### <a name="re-theming-an-application-at-runtime"></a>런타임에 응용 프로그램을 다시-테마 설정.

클릭 하 고 **테마 변경** 단추 실행는 `OnButtonClicked` 메서드에 다음 코드 예제에 나와 있는:

```csharp
void OnButtonClicked (object sender, EventArgs e)
{
  originalTemplate = !originalTemplate;
  contentView.ControlTemplate = (originalTemplate) ? tealTemplate : aquaTemplate;
}
```

이 메서드는 활성 대신 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) 대신을 사용 하 여 인스턴스 `ControlTemplate` 다음 스크린 샷에 결과 인스턴스:

![](creating-images/aqua-theme.png "바다색 컨트롤 템플릿")

> [!NOTE]
> **참고**:에 `ContentPage`, `Content` 속성을 할당할 수 있습니다 및 `ControlTemplate` 에서도 속성을 설정할 수 있습니다. 이 경우 경우는 `ControlTemplate` 포함는 `ContentPresenter` 인스턴스에 할당 된 콘텐츠는 `Content` 속성으로 표시 됩니다는 `ContentPresenter` 내는 `ControlTemplate`합니다.

### <a name="setting-a-controltemplate-with-a-style"></a>스타일으로 ControlTemplate 설정

A [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) 통해 적용할 수 있습니다는 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 를 더 테마는 기능을 확장 합니다. 이 작업을 만들어 수행할 수는 *암시적* 또는 *명시적* 대상 보기에 대 한 스타일을 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), 설정는 `ControlTemplate` 대상의 속성 뷰에서 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 인스턴스. 다음 코드 예제는 *암시적* 스타일 응용 프로그램 수준에 추가 된 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/):

```xaml
<Style TargetType="ContentView">
    <Setter Property="ControlTemplate" Value="{StaticResource TealTemplate}" />
</Style>
```

때문에 [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) 인스턴스가 *암시적*, 모두에 적용 됩니다 `ContentView` 응용 프로그램에서 인스턴스. 따라서 것은 더 이상 필요 설정 하는 [ `ContentView.ControlTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TemplatedView.ControlTemplate/) 속성을 다음 코드 예제에서와 같이:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0">
      ...
    </ContentView>
</ContentPage>
```

스타일에 대 한 자세한 내용은 참조 [스타일](~/xamarin-forms/user-interface/styles/index.md)합니다.

### <a name="creating-a-controltemplate-at-page-level"></a>ControlTemplate 페이지 수준에서 만들기

생성 될 뿐만 아니라 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) 인스턴스 응용 프로그램 수준에서 있습니다 수를 만들 페이지 수준에서 다음 코드 예제에 나와 있는 것 처럼:

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

추가 하는 경우는 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) 페이지 수준에서는 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) 에 추가 되는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), 한 다음은 `ControlTemplate` 인스턴스가 포함 됩니다. 에 `ResourceDictionary`합니다.

## <a name="creating-a-controltemplate-in-c35"></a>&#35;에서 한 ControlTemplate 만들기

정의 하는 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) 응용 프로그램 수준에서 한 `class` 나타내는 만들어야 합니다는 `ControlTemplate`합니다. 클래스에서 파생 된 [레이아웃](~/xamarin-forms/user-interface/layouts/index.md) 다음 코드 예제에 표시 된 대로 서식 파일에 대 한 사용:

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

`AquaTemplate` 클래스는 동일는 `TealTemplate` 클래스를 제외 하 고 서로 다른 색에 사용 되는 [ `BoxView.Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) 및 [ `Label.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) 속성입니다.

다음 코드 예제는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 적용 된 `TealTemplate` 에 [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

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

[ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) 의 컨트롤 템플릿 정의 하는 클래스의 형식을 지정 하 여 만든 인스턴스는 `ControlTemplate` 생성자입니다.

[ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) 속성이로 설정 되는 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 에 표시할 콘텐츠를 정의 하는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)합니다. 이 콘텐츠 표시 하는 [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) 에 포함 된는 `TealTemplate`합니다. 이전에 설명 된 동일한 메커니즘을 런타임에 테마를 변경 하려면 사용 되는 `AquaTheme`합니다.

## <a name="summary"></a>요약

이 문서를 만들고 컨트롤 템플릿을 사용 하는 방법을 보여 줍니다. 컨트롤 템플릿은 응용 프로그램 수준 또는 페이지 수준에서 정의할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [스타일](~/xamarin-forms/user-interface/styles/index.md)
- [간단한 테마 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simpletheme/)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentPresenter](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)
- [ContentView](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
