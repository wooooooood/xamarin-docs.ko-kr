---
title: 계층적 탐색
description: 이 문서에서는 NavigationPage 클래스를 사용 하 여 마지막-에서는 lifo (후입선출) 페이지의 스택으로 탐색을 수행 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: f8f8f9b4e5755e8b1707178fef633321b64e4e94
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994678"
---
# <a name="hierarchical-navigation"></a>계층적 탐색

_NavigationPage 클래스에는 사용자가 앞으로 및 뒤로 필요에 따라 페이지를 탐색할 수 있는 계층적 탐색 환경을 제공 합니다. 클래스는 페이지 개체의 마지막에 lifo (후입선출) 스택으로 탐색을 구현합니다. 이 문서에서는 NavigationPage 클래스를 사용 하 여 페이지의 스택으로 탐색을 수행 하는 방법에 설명 합니다._

이 문서에서는 다음 내용을 다룹니다.

- [탐색을 수행](#Performing_Navigation) – 루트 페이지 만들기, 페이지 탐색 스택으로 푸시, 페이지 탐색 스택에서 팝 및 페이지 전환 애니메이션을 적용 합니다.
- [탐색할 때 데이터를 전달](#Passing_Data_when_Navigating) – 데이터 페이지 생성자를 통해 전달는 `BindingContext`합니다.
- [탐색 스택에서 조작](#Manipulating_the_Navigation_Stack) – 스택에 삽입 하거나 페이지를 제거 하 여 조작 합니다.

## <a name="overview"></a>개요

다른 한 페이지에서 이동할 응용 프로그램이 푸시합니다 탐색 스택에 새 페이지 되 게 활성 페이지는 다음 다이어그램과에서 같이:

![](hierarchical-images/pushing.png "페이지 탐색 스택으로 푸시")

이전 페이지로 다시 돌아가려면 응용 프로그램 탐색 스택에서 현재 페이지를 팝 됩니다 하 고 다음 다이어그램에 표시 된 대로 새 최상위 페이지에 페이지를 사용 하는 활성 페이지가 됩니다.

![](hierarchical-images/popping.png "페이지 탐색 스택에서 팝")

탐색 메서드에 의해 노출 되는 [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) 속성에서 [ `Page` ](xref:Xamarin.Forms.Page) 파생 형식입니다. 이러한 메서드는 탐색 스택에서 팝 페이지에 페이지를 탐색 스택으로 푸시 하 고 스택 조작을 수행 하는 기능을 제공 합니다.

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>탐색을 수행합니다.

계층적 탐색에는 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 클래스가 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체의 스택을 탐색하는 데 사용됩니다. 다음 스크린샷에서 표시의 주요 구성 요소는 `NavigationPage` 각 플랫폼에서:

![](hierarchical-images/navigationpage-components.png "NavigationPage 구성 요소")

레이아웃을 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 플랫폼에 따라 달라 집니다.

- IOS, 탐색 모음에 있는 제목을 표시 하 고 있는 페이지의 위쪽에는 *다시* 이전 페이지로 반환 하는 단추입니다.
- Android 탐색 모음에 있는 아이콘을 제목으로 표시 된 페이지의 맨 위에 있는 *다시* 이전 페이지로 반환 하는 단추입니다. 아이콘에 정의 되어는 `[Activity]` 를 데코레이팅하는 특성을 `MainActivity` Android 플랫폼 특정 프로젝트에서 클래스입니다.
- 유니버설 Windows 플랫폼에서 탐색 모음을 제목을 표시 하는 페이지의 맨 위에 있는 없는 경우

모든 플랫폼에서의 값을 [ `Page.Title` ](xref:Xamarin.Forms.Page.Title) 속성 페이지 제목으로 표시 됩니다.

> [!NOTE]
> 하는 것에 `NavigationPage` 채울 `ContentPage` 인스턴스에만 합니다.

### <a name="creating-the-root-page"></a>루트 페이지 만들기

탐색 스택에 추가된 첫 번째 페이지는 응용 프로그램의 *루트* 페이지라고 하며, 다음 코드 예제는 이것을 수행하는 방법을 보여줍니다.

```csharp
public App ()
{
  MainPage = new NavigationPage (new Page1Xaml ());
}
```

이 인해 합니다 `Page1Xaml` [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 푸시되 어 탐색 스택에 있는 활성 페이지 및 응용 프로그램의 루트 페이지에는 인스턴스. 다음 스크린샷과에서 같습니다.

![](hierarchical-images/mainpage.png "탐색 스택의 루트 페이지")

> [!NOTE]
> [ `RootPage` ](xref:Xamarin.Forms.NavigationPage.RootPage) 의 속성을 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 인스턴스 탐색 스택의 첫 번째 페이지에 대 한 액세스를 제공 합니다.

### <a name="pushing-pages-to-the-navigation-stack"></a>탐색 스택에 푸시 페이지

이동할 `Page2Xaml`를 호출 하는 데 필요한 것을 [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) 메서드를를 [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) 다음 코드 예제에서 설명한 것 처럼 현재 페이지의 속성:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new Page2Xaml ());
}
```

`Page2Xaml` 인스턴스가 탐색 스택으로 푸시되어 활성 페이지가 됩니다. 다음 스크린샷과에서 같습니다.

![](hierarchical-images/secondpage.png "페이지를 탐색 스택으로 푸시")

경우는 [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) 메서드를 호출 하면 다음 이벤트가 발생 합니다.

- 페이지 호출 `PushAsync` 에 해당 [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) 재정의 호출 합니다.
- 탐색 중인 페이지에 해당 [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) 재정의 호출 합니다.
- `PushAsync` 작업 완료 합니다.

그러나 이러한 이벤트가 발생 하는 정확한 순서는 플랫폼에 따라 다릅니다. 자세한 내용은 [24 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold의 Xamarin.Forms 책입니다.

> [!NOTE]
> 에 대 한 호출을 [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) 하 고 [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) 재정의 페이지 탐색의 보장 된 표시로 처리할 수 없습니다. 예를 들어 iOS에는 `OnDisappearing` 응용 프로그램이 종료 될 때 재정의 활성 페이지 라고 합니다.

### <a name="popping-pages-from-the-navigation-stack"></a>탐색 스택에서 팝업 페이지

활성 페이지는 장치의 *다시* 단추를 눌러 탐색 스택에서 팝할 수 있습니다. 이때 단추는 장치의 물리적 단추이든 화면상 단추이든 상관없습니다.

프로그래밍 방식으로 원래 페이지로 돌아가려면 `Page2Xaml` 개체가 다음 코드 예제에서 설명한 것처럼 [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync) 메서드를 호출해야 합니다.

```csharp
async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopAsync ();
}
```

`Page2Xaml` 인스턴스가 탐색 스택에서 제거되어 맨 위에 있는 새 페이지가 활성 페이지가 됩니다. 경우는 [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) 메서드를 호출 하면 다음 이벤트가 발생 합니다.

- 페이지 호출 `PopAsync` 에 해당 [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) 재정의 호출 합니다.
- 페이지에 반환 되는 해당 [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) 재정의 호출 합니다.
- `PopAsync` 작업 반환 합니다.

그러나 이러한 이벤트가 발생 하는 정확한 순서는 플랫폼에 따라 다릅니다. 자세한 내용은 참조 [24 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold의 Xamarin.Forms 책입니다.

뿐만 [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) 및 [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) 메서드를 [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) 해줍니다 각 페이지의 속성을 [ `PopToRootAsync` ](xref:Xamarin.Forms.NavigationPage.PopToRootAsync) 메서드를 다음 코드 예제에 표시 됩니다.

```csharp
async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopToRootAsync ();
}
```

이 메서드는 루트를 제외한 모든 팝 [ `Page` ](xref:Xamarin.Forms.Page) 탐색 스택에서 있으므로 활성 페이지가 응용 프로그램의 루트 페이지입니다.

### <a name="animating-page-transitions"></a>이 페이지는 전환에 애니메이션 적용

[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) 각 페이지의 속성은 또한 재정의 푸시 및 팝 메서드를 포함 하는 제공 된 `boolean` 다음 코드 에서처럼를 탐색 하는 동안 페이지 애니메이션을 표시할지 여부를 제어 하는 매개 변수 예:

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

설정 된 `boolean` 매개 변수를 `false` 는 매개 변수를 설정 하는 동안 페이지 전환 애니메이션을 사용 하지 않도록 설정 `true` 내부 플랫폼에서 지원 되는 페이지 전환 애니메이션을 사용 하도록 설정 합니다. 그러나이 매개 변수 없는 push와 pop 메서드는 기본적으로 애니메이션을 사용 합니다.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>탐색할 때 데이터 전달

경우에 따라 다른 페이지로 탐색 하는 동안 데이터를 전달 하는 페이지에 대 한 필요 합니다. 새 페이지를 설정 하 고 페이지 생성자를 통해 데이터를 전달 하는이 작업을 수행 하는 두 가지 기술을 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 데이터입니다. 다시 설명 이제 각 합니다.

### <a name="passing-data-through-a-page-constructor"></a>페이지 생성자를 통해서만 데이터 전달

다른 페이지로 탐색 하는 동안 데이터를 전달 하기 위한 가장 간단한 방법은 다음과 같습니다. 다음 코드 예제에 나와 있는 페이지 생성자 매개 변수를 통해

```csharp
public App ()
{
  MainPage = new NavigationPage (new MainPage (DateTime.Now.ToString ("u")));
}
```

이 코드를 만듭니다는 `MainPage` 인스턴스를 현재 날짜 및 시간 ISO8601 형식에서에 전달 하는 래핑됩니다를 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 인스턴스.

`MainPage` 인스턴스는 다음 코드 예제와 같이 생성자 매개 변수를 통해 데이터를 받습니다.

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

데이터는 다음을 설정 하 여 페이지에 표시 됩니다는 [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) 다음 스크린샷에 표시 된 것 처럼 속성:

![](hierarchical-images/passing-data-constructor.png "페이지 생성자를 통해 전달 되는 데이터")

### <a name="passing-data-through-a-bindingcontext"></a>BindingContext 통해 데이터 전달

새 페이지의 설정 된 경우 다른 페이지로 탐색 하는 동안 데이터를 전달 하는 또 다른 방법은 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 다음 코드 예제에 표시 된 대로 데이터에:

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

이 코드를 설정 합니다 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 의 `SecondPage` 인스턴스를 `Contact` 인스턴스로 이동한 다음 이동할는 `SecondPage`합니다.

`SecondPage` 데이터 바인딩을 사용 하 여 표시 된 `Contact` 인스턴스 데이터를 다음 XAML 코드 예제에 표시 된 대로:

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

다음 코드 예제에서는 C#에서 데이터 바인딩을 수행할 수 있습니다 하는 방법을 보여 줍니다.

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

데이터는 다음 일련의 페이지에 표시 됩니다 [ `Label` ](xref:Xamarin.Forms.Label) 다음 스크린샷과에서 같이 제어 합니다.

![](hierarchical-images/passing-data-bindingcontext.png "BindingContext를 통해 전달 되는 데이터")

데이터 바인딩에 대한 자세한 내용은 [데이터 바인딩 기본](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)을 참조하세요.

<a name="Manipulating_the_Navigation_Stack" />

## <a name="manipulating-the-navigation-stack"></a>탐색 스택에서 조작

합니다 [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) 속성을 노출 된 [ `NavigationStack` ](xref:Xamarin.Forms.INavigation.NavigationStack) 탐색 스택의 페이지를 생성 하는 데 사용 될 수 있는 속성입니다. Xamarin.Forms 탐색 스택으로에 대 한 액세스를 유지 관리 하는 동안 합니다 `Navigation` 속성을 제공 합니다 [ `InsertPageBefore` ](xref:Xamarin.Forms.INavigation.InsertPageBefore*) 및 [ `RemovePage` ](xref:Xamarin.Forms.INavigation.RemovePage*) 스택에 삽입 하 여 조작 하기 위한 메서드 페이지 또는 제거 합니다.

합니다 [ `InsertPageBefore` ](xref:Xamarin.Forms.INavigation.InsertPageBefore*) 메서드 다음 다이어그램에 나와 있는 것 처럼 탐색 스택에서 지정 된 기존 페이지를 앞에 지정된 된 페이지를 삽입 합니다.

![](hierarchical-images/insert-page-before.png "페이지 탐색 스택에서 삽입")

합니다 [ `RemovePage` ](xref:Xamarin.Forms.INavigation.RemovePage*) 메서드 다음 다이어그램에 나와 있는 것 처럼 탐색 스택에서 지정된 된 페이지를 제거 합니다.

![](hierarchical-images/remove-page.png "페이지 탐색 스택에서 제거")

이러한 메서드를 성공적인 로그인을 수행 하는 새 페이지를 사용 하 여 로그인 페이지를 대체 하는 등 사용자 지정 탐색 경험을 사용 합니다. 다음 코드 예제에서는이 시나리오를 보여 줍니다.

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

사용자의 자격 증명이 잘못 된는 `MainPage` 인스턴스가 탐색 스택에서 현재 페이지 앞에 삽입 됩니다. 합니다 [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) 방법을 사용 하 여 탐색 스택에서 현재 페이지를 제거 합니다는 `MainPage` 활성 페이지가 되는 인스턴스.

## <a name="summary"></a>요약

이 문서에 사용 하는 방법을 설명 합니다 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 페이지의 스택으로 탐색을 수행 하는 클래스입니다. 이 클래스는 사용자가 앞으로 및 뒤로 필요에 따라 페이지를 탐색할 수 있는 계층적 탐색 환경을 제공 합니다. 이 모델은 탐색을 [`Page`](xref:Xamarin.Forms.Page) 개체의 LIFO(후입선출) 스택으로 구현합니다.


## <a name="related-links"></a>관련 링크

- [페이지 탐색](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [계층 구조 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/)
- [PassingData (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
- [LoginFlow (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)
- [(Xamarin University 비디오) Xamarin.Forms 샘플에서 흐름 화면에서에서 로그인을 만드는 방법](http://xamarinuniversity.blob.core.windows.net/lightninglectures/CreateASignIn.zip)
- [화면 흐름 Xamarin.Forms (Xamarin University 비디오)에서 로그인을 만드는 방법](https://university.xamarin.com/lightninglectures/how-to-create-a-sign-in-screen-flow-in-xamarinforms)
- [NavigationPage](xref:Xamarin.Forms.NavigationPage)
