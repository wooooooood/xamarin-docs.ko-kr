---
title: 계층적 탐색
description: 이 문서에서는 방식으로 lifo (후입선출) 페이지의 스택에 탐색을 수행 하려면 NavigationPage 클래스를 사용 하는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: 20dfb6e935d08c35da73a81fb401a613aa6c9bac
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242458"
---
# <a name="hierarchical-navigation"></a>계층적 탐색

_NavigationPage 클래스에는 사용자가을 앞으로 및 뒤로 필요에 따라 페이지를 탐색할 수 계층적 탐색 환경을 제공 합니다. 클래스는 페이지 개체 lifo (후입선출) 방식으로 스택에서으로 탐색을 구현합니다. 이 문서에서 페이지의 스택을 탐색을 수행 하는 NavigationPage 클래스를 사용 하는 방법을 보여 줍니다._

이 문서에서는 다음 내용을 다룹니다.

- [탐색을 수행](#Performing_Navigation) – 루트 페이지 만들기, 페이지 탐색 스택에 밀어, 페이지 탐색 스택에서 팝 및 페이지 전환에 애니메이션을 적용 합니다.
- [데이터를 탐색할 때 전달](#Passing_Data_when_Navigating) – 데이터 및 페이지 생성자를 통해 전달는 `BindingContext`합니다.
- [탐색 스택의 조작](#Manipulating_the_Navigation_Stack) – 삽입 하거나 페이지를 제거 하 여 스택을 조작 합니다.

## <a name="overview"></a>개요

다른 한 페이지에서 이동할 응용 프로그램은 푸시합니다 탐색 스택에 새 페이지를 여기서 사이트용으로 활성 페이지는 다음 다이어그램에 나와 있는 것 처럼:

![](hierarchical-images/pushing.png "페이지 탐색 스택에 푸시")

이전 페이지로 돌아가려면 응용 프로그램 현재 페이지 탐색 스택에서 팝 됩니다 매핑되며 새 맨 위에 있는 페이지의 활성 페이지는 다음 다이어그램에 나와 있는 것 처럼:

![](hierarchical-images/popping.png "페이지 탐색 스택에서 팝")

탐색 메서드에 의해 노출 되는 [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) 속성 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 파생 형식입니다. 이러한 메서드는 탐색 스택에 페이지 탐색 스택에서 팝 페이지를 강제 하 고 스택 조작을 수행 하는 기능을 제공 합니다.

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>탐색을 수행합니다.

계층적 탐색에는 [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 클래스가 [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 개체의 스택을 탐색하는 데 사용됩니다. 주요 구성 요소를 표시 하는 다음 스크린샷에서 `NavigationPage` 각 플랫폼에서:

![](hierarchical-images/navigationpage-components.png "NavigationPage 구성 요소")

레이아웃을 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 플랫폼에 따라 달라 집니다.

- Ios에서 탐색 모음이 제목을 표시 하 고 있는 페이지의 위쪽에 있어야는 *다시* 이전 페이지로 반환 하는 단추입니다.
- Android에서는 탐색 모음 있는지 제목, 아이콘을 표시 하는 페이지의 위쪽 및 *다시* 이전 페이지로 반환 하는 단추입니다. 아이콘에 정의 된는 `[Activity]` 를 데코레이팅하는 특성의 `MainActivity` Android 플랫폼 관련 프로젝트의 클래스.
- 유니버설 Windows 플랫폼에서 탐색 모음은 제목을 표시 하는 페이지의 위쪽에 있는입니다.

모든 플랫폼에서의 값은 [ `Page.Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) 속성 페이지 제목으로 표시 됩니다.

> [!NOTE]
> 하는 것이 좋습니다는 `NavigationPage` 으로 채워져야 할지 `ContentPage` 인스턴스만 있습니다.

### <a name="creating-the-root-page"></a>루트 페이지 만들기

탐색 스택에 추가된 첫 번째 페이지는 응용 프로그램의 *루트* 페이지라고 하며, 다음 코드 예제는 이것을 수행하는 방법을 보여줍니다.

```csharp
public App ()
{
  MainPage = new NavigationPage (new Page1Xaml ());
}
```

이 인해는 `Page1Xaml` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 인스턴스를 현재 페이지와 응용 프로그램의 루트 페이지 수 없는 탐색 스택에 밀어넣을 수 있습니다. 이 다음 스크린샷에 나와 있습니다.

![](hierarchical-images/mainpage.png "탐색 스택의 루트 페이지")

> [!NOTE]
> [ `RootPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.RootPage/) 속성은 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 인스턴스 탐색 스택의 첫 번째 페이지에 대 한 액세스를 제공 합니다.

### <a name="pushing-pages-to-the-navigation-stack"></a>탐색 스택에 푸시 페이지

이동 하려면 `Page2Xaml`, 호출 해야 하는 [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) 에서 메서드는 [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) 현재 페이지에 있는 다음 코드 예제에서 설명한로 속성:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new Page2Xaml ());
}
```

`Page2Xaml` 인스턴스가 탐색 스택으로 푸시되어 활성 페이지가 됩니다. 이 다음 스크린샷에 나와 있습니다.

![](hierarchical-images/secondpage.png "페이지 탐색 스택에 푸시된")

경우는 [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) 메서드가 호출 되 면 다음 이벤트가 발생 합니다.

- 페이지 호출 `PushAsync` 가 해당 [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) 재정의 호출 합니다.
- 탐색 대상인 페이지에 해당 [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) 재정의 호출 합니다.
- `PushAsync` 작업을 완료 합니다.

그러나 이러한 이벤트가 발생 하는 정확한 순서는 플랫폼에 따라 다릅니다. 자세한 내용은 참조 [24 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold Xamarin.Forms 책의 합니다.

> [!NOTE]
> 에 대 한 호출이 [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) 및 [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) 재정의 보장 된 페이지 탐색으로 처리할 수 없습니다. 예를 들어 iOS의 경우에 `OnDisappearing` 재정의 응용 프로그램이 종료 될 때 현재 페이지에서 호출 됩니다.

### <a name="popping-pages-from-the-navigation-stack"></a>탐색 스택에서 팝 페이지

활성 페이지는 장치의 *다시* 단추를 눌러 탐색 스택에서 팝할 수 있습니다. 이때 단추는 장치의 물리적 단추이든 화면상 단추이든 상관없습니다.

프로그래밍 방식으로 원래 페이지로 돌아가려면 `Page2Xaml` 개체가 다음 코드 예제에서 설명한 것처럼 [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) 메서드를 호출해야 합니다.

```csharp
async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopAsync ();
}
```

`Page2Xaml` 인스턴스가 탐색 스택에서 제거되어 맨 위에 있는 새 페이지가 활성 페이지가 됩니다. 경우는 [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) 메서드가 호출 되 면 다음 이벤트가 발생 합니다.

- 페이지 호출 `PopAsync` 가 해당 [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) 재정의 호출 합니다.
- 반환 되는 페이지에 해당 [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) 재정의 호출 합니다.
- `PopAsync` 작업 반환 합니다.

그러나 이러한 이벤트가 발생 하는 정확한 순서는 플랫폼에 따라 다릅니다. 자세한 내용은 참조 [24 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold Xamarin.Forms 책의 합니다.

으로 [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) 및 [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) 메서드는 [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) 각 페이지의 속성도 제공 합니다. 한 [ `PopToRootAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopToRootAsync()/) 메서드를 다음 코드 예제에 표시 됩니다.

```csharp
async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopToRootAsync ();
}
```

이 메서드는 루트를 제외한 모든 팝 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 탐색 스택에서 있으므로 현재 페이지 응용 프로그램의 루트 페이지.

### <a name="animating-page-transitions"></a>이 페이지는 전환에 애니메이션 적용

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) 재정의 된 푸시 및 팝 메서드를 포함 하는 각 페이지의 속성 제공는 `boolean` 다음 코드에 나와 있는 것 처럼를 탐색 하는 동안 페이지 애니메이션을 표시할지 여부를 제어 하는 매개 변수 예:

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

설정의 `boolean` 매개 변수를 `false` 는 매개 변수를 설정 하는 동안 페이지 전환 애니메이션을 사용 하지 않도록 설정 `true` 은 기본 플랫폼에서 지원 제공 페이지 전환 애니메이션을 사용할 수 있습니다. 그러나이 매개 변수 없는 푸시 및 팝 메서드는 기본적으로 애니메이션을 사용 합니다.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>탐색할 때 데이터 전달

경우에 따라 다른 페이지를 탐색 하는 동안 데이터를 전달 하는 페이지에 대 한 필요 합니다. 페이지 생성자 있고 새 페이지를 설정 하 여 데이터를 전달 하는이 작업을 수행 하는 두 가지 기술을 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 데이터입니다. 다시 설명 이제 각 합니다.

### <a name="passing-data-through-a-page-constructor"></a>페이지 생성자를 통해 데이터 전달

다른 페이지를 탐색 하는 동안 데이터를 전달 하기 위한 가장 간단한 방법은 다음 코드 예제에 표시 되는 페이지 생성자 매개 변수를 통해:

```csharp
public App ()
{
  MainPage = new NavigationPage (new MainPage (DateTime.Now.ToString ("u")));
}
```

이 코드에서는 `MainPage` 인스턴스를 현재 날짜 및 시간 ISO8601 형식에서 전달는 요소에 래핑되는 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 인스턴스.

`MainPage` 인스턴스는 다음 코드 예제와 같이 생성자 매개 변수를 통해 데이터를 수신 합니다.

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

이후에 데이터가 설정 하 여 페이지에 표시 되는 [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) 속성을 다음 스크린샷에 표시 된 것 처럼:

![](hierarchical-images/passing-data-constructor.png "페이지 생성자를 통해 전달 되는 데이터")

### <a name="passing-data-through-a-bindingcontext"></a>BindingContext 통해 데이터 전달

새 페이지를 설정 하 여 다른 페이지를 탐색 하는 동안 데이터를 전달 하기 위한 다른 방법은 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 다음 코드 예제와 같이 데이터에:

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

설정 하는이 코드는 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 의 `SecondPage` 인스턴스는 `Contact` 인스턴스를 선택한 다음로 이동는 `SecondPage`합니다.

`SecondPage` 데이터 바인딩을 사용 하 여 표시 된 `Contact` 다음 XAML 코드 예제와 같이 인스턴스 데이터:

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

다음 코드 예제에서는 C#에서 데이터 바인딩 수 수행 하는 방법을 보여 줍니다.

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

일련의 데이터 페이지에 표시 한 다음는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 다음 스크린샷에서 같이 제어:

![](hierarchical-images/passing-data-bindingcontext.png "BindingContext를 통해 전달 되는 데이터")

데이터 바인딩에 대한 자세한 내용은 [데이터 바인딩 기본](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)을 참조하세요.

<a name="Manipulating_the_Navigation_Stack" />

## <a name="manipulating-the-navigation-stack"></a>탐색 스택의 조작

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) 속성 노출 한 [ `NavigationStack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.NavigationStack/) 탐색 스택에서 페이지 생성 하는 데 사용 될 수 있는 속성입니다. Xamarin.Forms 탐색 스택에서에 대 한 액세스를 유지 관리 하는 동안는 `Navigation` 속성은 제공 된 [ `InsertPageBefore` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page)/) 및 [ `RemovePage` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page)/) 삽입 하 여 스택을 조작 하기 위한 메서드 페이지 또는 제거 합니다.

[ `InsertPageBefore` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page)/) 메서드는 다음 다이어그램에 나와 있는 것 처럼 지정된 된 페이지 탐색 스택에서 지정 된 기존 페이지 앞에 삽입 합니다.

![](hierarchical-images/insert-page-before.png "페이지 탐색 스택에서 삽입")

[ `RemovePage` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page)/) 메서드는 다음 다이어그램에 나와 있는 것 처럼 지정된 된 페이지 탐색 스택에서 제거:

![](hierarchical-images/remove-page.png "페이지 탐색 스택에서 제거")

이러한 메서드는 새 페이지를 성공적으로 로그인으로 로그인 페이지를 대체 하는 등 사용자 지정 탐색 경험을를 사용 합니다. 다음 코드 예제에서는이 시나리오를 보여 줍니다.

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

사용자의 자격 증명이 올바른지 하는 `MainPage` 인스턴스 탐색 스택에서 현재 페이지 앞에 삽입 됩니다. [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) 메서드 다음으로 탐색 스택에서 현재 페이지를 제거는 `MainPage` 인스턴스 활성 페이지가 되 고 있습니다.

## <a name="summary"></a>요약

이 문서에 사용 하는 방법을 보여 주는 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 페이지의 스택을에서 탐색을 수행 하는 클래스입니다. 이 클래스는 계층적 탐색 환경을 사용자가을 앞으로 및 뒤로 필요에 따라 페이지를 탐색할 수 있습니다. 이 모델은 탐색을 [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 개체의 LIFO(후입선출) 스택으로 구현합니다.


## <a name="related-links"></a>관련 링크

- [페이지 탐색](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [계층 구조 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/)
- [PassingData (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
- [LoginFlow (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)
- [Xamarin.Forms (Xamarin 대학 비디오) 샘플에서 화면 흐름에는 기호를 만들어야 하는 방법](http://xamarinuniversity.blob.core.windows.net/lightninglectures/CreateASignIn.zip)
- [Xamarin.forms (Xamarin 대학 비디오) 화면 흐름에는 기호를 만들어야 하는 방법](https://university.xamarin.com/lightninglectures/how-to-create-a-sign-in-screen-flow-in-xamarinforms)
- [NavigationPage](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)
