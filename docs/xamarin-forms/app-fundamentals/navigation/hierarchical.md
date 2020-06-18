---
title: 'title: “계층적 탐색” description: “이 문서에서는 NavigationPage 클래스를 사용하여 후입선출(LIFO) 페이지 스택 탐색을 수행하는 방법을 보여 줍니다.”'
description: 'ms.prod: xamarin ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 03/10/2020 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ec35b03e7e96f0730813918bdd96e1408cfabde7
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571495"
---
# <a name="hierarchical-navigation"></a>계층적 탐색

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-hierarchical)

_NavigationPage 클래스는 사용자가 필요에 따라 페이지를 앞으로 뒤로 탐색할 수 있는 계층적 탐색 환경을 제공합니다. 이 클래스는 탐색을 Page 개체의 LIFO(후입선출) 스택으로 구현합니다. 이 문서에서는 NavigationPage 클래스를 사용하여 페이지 스택 탐색을 수행하는 방법을 보여 줍니다._

한 페이지에서 다른 페이지로 이동하려면 다음 다이어그램에 표시된 것처럼 애플리케이션은 새 페이지를 탐색 스택으로 푸시하여 활성 페이지가 되게 합니다.

![](hierarchical-images/pushing.png "Pushing a Page to the Navigation Stack")

이전 페이지로 돌아가기 위해 애플리케이션은 다음 다이어그램에 표시된 것처럼 탐색 스택에서 현재 페이지를 꺼내고 맨 위에 있는 새 페이지가 활성 페이지가 됩니다.

![](hierarchical-images/popping.png "Popping a Page from the Navigation Stack")

탐색 메서드는 모든 [`Page`](xref:Xamarin.Forms.Page) 파생 형식의 [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) 속성에 의해 노출됩니다. 이 메서드는 탐색 스택에 페이지를 푸시하고 탐색 스택에서 페이지를 꺼내 스택 조작을 수행하는 기능을 제공합니다.

## <a name="performing-navigation"></a>탐색 수행

계층적 탐색에서는 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 클래스가 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체의 스택을 탐색하는 데 사용됩니다. 다음 스크린샷은 각 플랫폼에서 `NavigationPage`의 주요 구성 요소를 보여 줍니다.

![](hierarchical-images/navigationpage-components.png "NavigationPage Components")

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)의 레이아웃은 플랫폼에 따라 달라집니다.

- iOS에는 탐색 모음이 제목을 표시하는 페이지 맨 위에 나타나며 여기에 이전 페이지로 돌아가는 *뒤로* 단추가 있습니다.
- Android에는 탐색 모음이 제목, 아이콘 및 이전 페이지로 돌아가는 *뒤로* 단추를 표시하는 페이지 맨 위에 나타납니다. 아이콘은 Android 플랫폼 관련 프로젝트의 `MainActivity` 클래스를 데코레이팅하는 `[Activity]` 특성에 정의됩니다.
- 유니버설 Windows 플랫폼에서 제목을 표시하는 페이지 맨 위에 탐색 모음이 나타납니다.

모든 플랫폼에서 [`Page.Title`](xref:Xamarin.Forms.Page.Title) 속성 값이 페이지 제목으로 표시됩니다. 또한 `IconColor` 속성을 탐색 모음의 아이콘에 적용되는 [`Color`](xref:Xamarin.Forms.Color)로 설정할 수 있습니다.

> [!NOTE]
> `NavigationPage`를 `ContentPage` 인스턴스만으로 채우는 것이 좋습니다.

### <a name="creating-the-root-page"></a>루트 페이지 만들기

탐색 스택에 추가된 첫 번째 페이지는 애플리케이션의 *root* 페이지라고 하며, 다음 코드 예제는 해당 수행 방법을 보여줍니다.

```csharp
public App ()
{
  MainPage = new NavigationPage (new Page1Xaml ());
}
```

`Page1Xaml` [`ContentPage`](xref:Xamarin.Forms.ContentPage) 인스턴스가 탐색 스택으로 푸시되어 애플리케이션의 활성 및 루트 페이지가 됩니다. 이 과정은 다음 스크린샷에 나와 있습니다.

![](hierarchical-images/mainpage.png "Root Page of Navigation Stack")

> [!NOTE]
> [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 인스턴스의 [`RootPage`](xref:Xamarin.Forms.NavigationPage.RootPage) 속성은 탐색 스택의 첫 번째 페이지에 대한 액세스를 제공합니다.

### <a name="pushing-pages-to-the-navigation-stack"></a>탐색 스택으로 페이지 푸시

`Page2Xaml`로 이동하려면 다음 코드 예제에서 설명한 것처럼 현재 페이지의 [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) 속성에서 [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*) 메서드를 호출해야 합니다.

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new Page2Xaml ());
}
```

`Page2Xaml` 인스턴스가 탐색 스택으로 푸시되어 활성 페이지가 됩니다. 이 과정은 다음 스크린샷에 나와 있습니다.

![](hierarchical-images/secondpage.png "Page Pushed onto Navigation Stack")

[`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*) 메서드가 호출되는 경우 다음 이벤트가 발생합니다.

- `PushAsync`를 호출하는 페이지에서는 해당 [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) 재정의가 호출됩니다.
- 탐색 중인 페이지에서는 [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) 재정의가 호출됩니다.
- `PushAsync` 작업이 완료됩니다.

그러나 이러한 이벤트가 발생하는 정확한 순서는 플랫폼에 따라 다릅니다. 자세한 내용은 Charles Petzold의 Xamarin.Forms 책 [24장](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)을 참조하세요.

> [!NOTE]
> [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) 및 [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) 재정의에 대한 호출을 페이지 탐색의 보장된 표시로 다룰 수는 없습니다. 예를 들어 iOS에서 `OnDisappearing` 재정의는 애플리케이션이 종료될 때 활성 페이지에 호출됩니다.

### <a name="popping-pages-from-the-navigation-stack"></a>탐색 스택에서 페이지 꺼내기

활성 페이지는 디바이스의 *뒤로* 단추를 눌러 탐색 스택에서 뺄(pop) 수 있습니다. 이때 단추는 디바이스의 물리적 단추든 화면상 단추든 상관없습니다.

프로그래밍 방식으로 원래 페이지로 돌아가려면 `Page2Xaml` 개체가 다음 코드 예제에서 설명한 것처럼 [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync) 메서드를 호출해야 합니다.

```csharp
async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopAsync ();
}
```

`Page2Xaml` 인스턴스가 탐색 스택에서 제거되어 맨 위에 있는 새 페이지가 활성 페이지가 됩니다. [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync) 메서드가 호출되는 경우 다음 이벤트가 발생합니다.

- `PopAsync`를 호출하는 페이지에서는 해당 [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) 재정의가 호출됩니다.
- 반환되는 페이지에서는 [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) 재정의가 호출됩니다.
- `PopAsync` 작업이 반환됩니다.

그러나 이러한 이벤트가 발생하는 정확한 순서는 플랫폼에 따라 다릅니다. 자세한 내용은 Charles Petzold의 Xamarin.Forms 책 [24장](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)을 참조하세요.

각 페이지의 [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) 속성은 [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*) 및 [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync) 메서드뿐만 아니라 [`PopToRootAsync`](xref:Xamarin.Forms.NavigationPage.PopToRootAsync) 메서드도 제공하며 다음 코드 예제에 표시됩니다.

```csharp
async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopToRootAsync ();
}
```

이 메서드는 탐색 스택에서 루트 [`Page`](xref:Xamarin.Forms.Page)를 제외한 모든 항목을 꺼내므로 애플리케이션의 루트 페이지를 활성 페이지로 만듭니다.

### <a name="animating-page-transitions"></a>페이지 전환에 애니메이션 적용

각 페이지의 [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) 속성은 또한 다음 코드 예제에 나온 것처럼 탐색하는 동안 페이지 애니메이션을 표시할지 여부를 제어하는 `boolean` 매개 변수가 포함된 재정의 푸시 및 팝 메서드를 제공합니다.

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PushAsync (new Page2Xaml (), false);
}

async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PopAsync (false);
}

async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PopToRootAsync (false);
}
```

기본 플랫폼에서 지원되는 경우 매개 변수를 `true`로 설정하면 페이지 전환 애니메이션을 사용하도록 설정되고, `boolean` 매개 변수를 `false`로 설정하면 페이지 전환 애니메이션을 사용하지 않도록 설정됩니다. 그러나 이 매개 변수가 없는 푸시 및 팝 메서드는 기본적으로 애니메이션을 사용하도록 설정합니다.

## <a name="passing-data-when-navigating"></a>탐색 시 데이터 전달

경우에 따라 페이지에서 탐색하는 동안 다른 페이지로 데이터를 전달해야 합니다. 페이지 생성자를 통해 데이터를 전달하는 방법과 새 페이지의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)를 데이터로 설정하는 방법의 두 방법으로 이 작업을 수행할 수 있습니다. 각 방법을 이제 차례차례 설명하겠습니다.

### <a name="passing-data-through-a-page-constructor"></a>페이지 생성자를 통해 데이터 전달

탐색하는 동안 다른 페이지로 데이터를 전달하기 위한 가장 간단한 방법은 다음 코드 예제에 나와 있는 페이지 생성자 매개 변수를 통하는 것입니다.

```csharp
public App ()
{
  MainPage = new NavigationPage (new MainPage (DateTime.Now.ToString ("u")));
}
```

이 코드는 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 인스턴스에 래핑되고 ISO8601 형식의 현재 날짜 및 시간으로 전달하는 `MainPage` 인스턴스를 생성합니다.

`MainPage` 인스턴스는 다음 코드 예제에 나온 것처럼 생성자 매개 변수를 통해 데이터를 수신합니다.

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

그런 다음, 다음 스크린샷에 표시된 것처럼 [`Label.Text`](xref:Xamarin.Forms.Label.Text) 속성을 설정하여 데이터를 페이지에 표시합니다.

![](hierarchical-images/passing-data-constructor.png "Data Passed Through a Page Constructor")

### <a name="passing-data-through-a-bindingcontext"></a>BindingContext를 통해 데이터 전달

탐색하는 동안 다른 페이지로 데이터를 전달하는 또 다른 방법은 다음 코드 예제에 표시된 대로 새 페이지의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)를 데이터로 설정하는 것입니다.

```csharp
async void OnNavigateButtonClicked (object sender, EventArgs e)
{
  var contact = new Contact {
    Name = "Jane Doe",
    Age = 30,
    Occupation = "Developer",
    Country = "USA"
  };

  var secondPage = new SecondPage ();
  secondPage.BindingContext = contact;
  await Navigation.PushAsync (secondPage);
}
```

이 코드는 `SecondPage` 인스턴스의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)를 `Contact` 인스턴스로 설정한 다음, `SecondPage`로 이동합니다.

그런 다음, 다음 XAML 코드 예제에 표시된 대로 `SecondPage`는 데이터 바인딩을 사용하여 `Contact` 인스턴스 데이터를 표시합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="PassingData.SecondPage"
             Title="Second Page">
    <ContentPage.Content>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Name}" FontSize="Medium" FontAttributes="Bold" />
            </StackLayout>
            ...
            <Button x:Name="navigateButton" Text="Previous Page" Clicked="OnNavigateButtonClicked" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

다음 코드 예제는 C#에서 데이터 바인딩이 어떻게 수행되는지를 보여 줍니다.

```csharp
public class SecondPageCS : ContentPage
{
  public SecondPageCS ()
  {
    var nameLabel = new Label {
      FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
      FontAttributes = FontAttributes.Bold
    };
    nameLabel.SetBinding (Label.TextProperty, "Name");
    ...
    var navigateButton = new Button { Text = "Previous Page" };
    navigateButton.Clicked += OnNavigateButtonClicked;

    Content = new StackLayout {
      HorizontalOptions = LayoutOptions.Center,
      VerticalOptions = LayoutOptions.Center,
      Children = {
        new StackLayout {
          Orientation = StackOrientation.Horizontal,
          Children = {
            new Label{ Text = "Name:", FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)), HorizontalOptions = LayoutOptions.FillAndExpand },
            nameLabel
          }
        },
        ...
        navigateButton
      }
    };
  }

  async void OnNavigateButtonClicked (object sender, EventArgs e)
  {
    await Navigation.PopAsync ();
  }
}
```

그런 다음, 다음 스크린샷에 표시된 것처럼 일련의 [`Label`](xref:Xamarin.Forms.Label) 컨트롤로 데이터를 페이지에 표시합니다.

![](hierarchical-images/passing-data-bindingcontext.png "Data Passed Through a BindingContext")

데이터 바인딩에 대한 자세한 내용은 [데이터 바인딩 기본](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)을 참조하세요.

## <a name="manipulating-the-navigation-stack"></a>탐색 스택 조작

[`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) 속성은 탐색 스택의 페이지를 얻을 수 있는 [`NavigationStack`](xref:Xamarin.Forms.INavigation.NavigationStack) 속성을 노출합니다. Xamarin.Forms는 탐색 스택에 대한 액세스를 유지 관리하는 반면, `Navigation` 속성은 페이지를 삽입하거나 제거하여 스택을 조작하는 [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore*) 및 [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage*) 메서드를 제공합니다.

[`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore*) 메서드는 다음 다이어그램에 표시된 것처럼 탐색 스택에서 기존 지정된 페이지 앞에 지정된 페이지를 삽입합니다.

![](hierarchical-images/insert-page-before.png "Inserting a Page in the Navigation Stack")

[`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage*) 메서드는 다음 다이어그램에 표시된 것처럼 탐색 스택에서 지정된 페이지를 제거합니다.

![](hierarchical-images/remove-page.png "Removing a Page from the Navigation Stack")

이러한 메소드를 사용하면 로그인 성공 후 로그인 페이지를 새 페이지로 바꾸는 것과 같은 사용자 지정 탐색 환경이 가능합니다. 다음 코드 예제에서는 이 시나리오를 보여 줍니다.

```csharp
async void OnLoginButtonClicked (object sender, EventArgs e)
{
  ...
  var isValid = AreCredentialsCorrect (user);
  if (isValid) {
    App.IsUserLoggedIn = true;
    Navigation.InsertPageBefore (new MainPage (), this);
    await Navigation.PopAsync ();
  } else {
    // Login failed
  }
}

```

사용자의 자격 증명이 정확하다면 `MainPage` 인스턴스가 현재 페이지 앞에 탐색 스택에 삽입됩니다. 그러면 [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync) 메서드가 탐색 스택에서 현재 페이지를 제거하고 `MainPage` 인스턴스가 활성 페이지가 됩니다.

## <a name="displaying-views-in-the-navigation-bar"></a>탐색 모음에서 보기 표시

모든 Xamarin.Forms [`View`](xref:Xamarin.Forms.View)는 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)의 탐색 모음에 표시될 수 있습니다. 이렇게 하려면 [`NavigationPage.TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) 연결된 속성을 `View`로 설정합니다. 이 연결된 속성은 모든 [`Page`](xref:Xamarin.Forms.Page)로 설정할 수 있으며 `Page`가 `NavigationPage`에 푸시되면 `NavigationPage`가 속성 값을 적용합니다.

[제목 보기 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-titleview)에서 가져온 다음 예제에서는 XAML에서 [`NavigationPage.TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) 연결된 속성을 설정하는 방법을 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="NavigationPageTitleView.TitleViewPage">
    <NavigationPage.TitleView>
        <Slider HeightRequest="44" WidthRequest="300" />
    </NavigationPage.TitleView>
    ...
</ContentPage>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
public class TitleViewPage : ContentPage
{
    public TitleViewPage()
    {
        var titleView = new Slider { HeightRequest = 44, WidthRequest = 300 };
        NavigationPage.SetTitleView(this, titleView);
        ...
    }
}
```

그 결과, [`Slider`](xref:Xamarin.Forms.Slider)가 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)의 탐색 모음에 표시됩니다.

[![슬라이더 TitleView](hierarchical-images/titleview-small.png "슬라이더 TitleView")](hierarchical-images/titleview-large.png#lightbox "슬라이더 TitleView")

> [!IMPORTANT]
> 보기의 크기가 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 및 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 속성으로 지정되지 않으면 탐색 모음에 많은 보기가 나타나지 않습니다. 또는 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 및 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성을 적절한 값으로 설정하여 보기를 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에 래핑할 수 있습니다.

[`Layout`](xref:Xamarin.Forms.Layout) 클래스는 [`View`](xref:Xamarin.Forms.View) 클래스에서 파생되므로 여러 보기를 포함하는 레이아웃 클래스를 표시하도록 [`TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) 연결된 속성을 설정할 수 있습니다. iOS 및 UWP(유니버설 Windows 플랫폼)에서는 탐색 모음의 높이를 변경할 수 없으므로 탐색 모음에 표시된 보기가 탐색 모음의 기본 크기보다 클 경우 클리핑이 발생합니다. 그러나 Android에서는 [`NavigationPage.BarHeight`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.BarHeightProperty) 바인딩 가능 속성을 새 높이를 나타내는 `double`로 설정하여 탐색 모음의 높이를 변경할 수 있습니다. 자세한 내용은 [NavigationPage에서 탐색 모음 높이 설정](~/xamarin-forms/platform/android/navigationpage-bar-height.md)을 참조하세요.

또는 탐색 막대에 일부 콘텐츠를 배치하고, 일부는 탐색 막대와 색상이 일치하는 페이지 콘텐츠 맨 위에 있는 보기에 배치하여 확장된 탐색 모음을 제시할 수 있습니다. 또한 iOS에서 [`NavigationPage.HideNavigationBarSeparator`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.HideNavigationBarSeparatorProperty) 바인드 가능 속성을 `true`로 설정하여 탐색 모음의 맨 아래에 있는 구분선과 그림자를 제거할 수 있습니다. 자세한 내용은 [NavigationPage에서 탐색 모음 구분 기호 숨기기](~/xamarin-forms/platform/ios/navigation-bar-separator.md)를 참조하세요.

> [!NOTE]
> [`BackButtonTitle`](xref:Xamarin.Forms.NavigationPage.BackButtonTitleProperty), [`Title`](xref:Xamarin.Forms.Page.Title), [`TitleIcon`](xref:Xamarin.Forms.NavigationPage.TitleIconProperty) 및 [`TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) 속성으로 탐색 모음에서 공간을 차지하는 모든 값을 정의할 수 있습니다. 탐색 모음 크기는 플랫폼 및 화면 크기에 따라 다르지만 이러한 속성을 모두 설정하면 사용 가능한 공간이 제한되어 충돌이 발생합니다. 이러한 속성을 조합하여 사용하는 대신 `TitleView` 속성만 설정하여 원하는 탐색 모음 디자인을 보다 잘 만들 수 있습니다.

### <a name="limitations"></a>제한 사항

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)의 탐색 모음에 [`View`](xref:Xamarin.Forms.View)를 표시할 때 주의해야 할 몇 가지 제한 사항이 있습니다.

- iOS에서는 `NavigationPage`의 탐색 모음에 배치된 보기가 큰 제목의 사용 가능 여부에 따라 다른 위치에 나타납니다. 큰 제목 사용에 대한 자세한 내용은 [큰 제목 표시](~/xamarin-forms/platform/ios/page-large-title.md)를 참조하세요.
- Android에서는 `NavigationPage`의 탐색 모음에 보기를 배치하는 작업을 app-compat을 사용하는 앱에서만 수행할 수 있습니다.
- `NavigationPage`의 탐색 모음에 [`ListView`](xref:Xamarin.Forms.ListView) 및 [`TableView`](xref:Xamarin.Forms.TableView)와 같은 크고 복잡한 보기를 배치하지 않는 것이 좋습니다.

## <a name="related-links"></a>관련 링크

- [페이지 탐색](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Hierarchical(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-hierarchical)
- [PassingData(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-passingdata)
- [LoginFlow(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-loginflow)
- [TitleView(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-titleview)
- [Xamarin.Forms에서 로그인 화면 흐름을 만드는 방법 동영상](https://www.youtube.com/watch?v=qKQ7pyyG1fo)
- [NavigationPage](xref:Xamarin.Forms.NavigationPage)
