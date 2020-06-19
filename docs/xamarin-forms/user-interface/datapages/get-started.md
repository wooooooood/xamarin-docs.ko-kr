---
title: DataPages 시작
description: 이 문서에서는 datapages를 사용 하 여 간단한 데이터 기반 페이지 빌드를 시작 하는 방법을 설명 Xamarin.Forms 합니다.
ms.prod: xamarin
ms.assetid: 6416E5FA-6384-4298-BAA1-A89381E47210
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: bb08d359048d53639a700cc5ff526f26d6b077b6
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84571469"
---
# <a name="getting-started-with-datapages"></a>DataPages 시작

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)

![](~/media/shared/preview.png "This API is currently in preview")

> [!IMPORTANT]
> DataPages Xamarin.Forms 를 렌더링 하려면 테마 참조가 필요 합니다. 여기에는를 설치 하는 작업이 포함 됩니다 [ Xamarin.Forms . 테마. 기본](https://www.nuget.org/packages/Xamarin.Forms.Theme.Base/) NuGet 패키지를 프로젝트에 삽입 하 고 다음을 수행 [ Xamarin.Forms 합니다. 테마. Light](https://www.nuget.org/packages/Xamarin.Forms.Theme.Light/) 또는 [ Xamarin.Forms . 테마. 짙은](https://www.nuget.org/packages/Xamarin.Forms.Theme.Dark/) NuGet 패키지.

DataPages 미리 보기를 사용 하 여 간단한 데이터 기반 페이지 빌드를 시작 하려면 다음 단계를 수행 합니다. 이 데모에서는 코드에서 특정 JSON 형식만 사용 하는 미리 보기 빌드에서 하드 코딩 된 스타일 ("이벤트")을 사용 합니다.

[![](get-started-images/demo-sml.png "DataPages Sample Application")](get-started-images/demo.png#lightbox "DataPages Sample Application")

## <a name="1-add-nuget-packages"></a>1. NuGet 패키지 추가

Xamarin.Forms.NET Standard 라이브러리 및 응용 프로그램 프로젝트에 다음 NuGet 패키지를 추가 합니다.

- Xamarin.Forms. 마주보
- Xamarin.Forms. 테마. 기본
- 테마 구현 NuGet (예: Xamarin.Forms. 테마. 조명)

## <a name="2-add-theme-reference"></a>2. 테마 참조 추가

**앱 .xaml** 파일에서 `xmlns:mytheme` 테마에 대 한 사용자 지정을 추가 하 고 테마가 응용 프로그램의 리소스 사전에 병합 되도록 합니다.

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
  xmlns:mytheme="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Light"
  x:Class="DataPagesDemo.App">
    <Application.Resources>
        <ResourceDictionary MergedWith="mytheme:LightThemeResources" />
    </Application.Resources>
</Application>
```

> [!IMPORTANT]
> 또한 iOS 및 Android에 일부 상용구 코드를 추가 하 여 [테마 어셈블리를 로드](#troubleshooting) 하는 단계를 수행 해야 합니다 `AppDelegate` `MainActivity` . 이는 향후 미리 보기 릴리스에서 개선 될 예정입니다.

## <a name="3-add-a-xaml-page"></a>3. XAML 페이지 추가

응용 프로그램에 새 XAML 페이지를 추가 하 Xamarin.Forms 고 *기본 클래스* 를에서 `ContentPage` 로 변경 `Xamarin.Forms.Pages.ListDataPage` 합니다. 이 작업은 c # 및 XAML 모두에서 수행 해야 합니다.

**C # 파일**

```csharp
public partial class SessionDataPage : Xamarin.Forms.Pages.ListDataPage // was ContentPage
{
  public SessionDataPage ()
  {
    InitializeComponent ();
  }
}
```

**XAML 파일**

루트 요소를 `<p:ListDataPage>` 에 대 한 사용자 지정 네임 스페이스로 변경 하는 것 외에 `xmlns:p` 도 추가 해야 합니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage">

    <ContentPage.Content></ContentPage.Content>

</p:ListDataPage>
```

**응용 프로그램 하위 클래스**

`App`가 새를 포함 하는로 설정 되도록 클래스 생성자를 변경 합니다 `MainPage` `NavigationPage` `SessionDataPage` . 탐색 페이지를 사용 *해야* 합니다.

```csharp
MainPage = new NavigationPage (new SessionDataPage ());
```

## <a name="3-add-the-datasource"></a>3. DataSource를 추가 합니다.

요소를 삭제 하 `Content` 고로 바꿔서 `p:ListDataPage.DataSource` 페이지를 데이터로 채웁니다. 아래 예제에서 원격 Json 데이터 파일은 URL에서 로드 됩니다.

> [!NOTE]
> 미리 보기 *requires* 에는 `StyleClass` 데이터 원본에 대 한 렌더링 힌트를 제공 하는 특성이 필요 합니다. 는 `StyleClass="Events"` 미리 보기에 미리 정의 된 레이아웃을 나타내며, 사용 되는 JSON 데이터 원본과 일치 하도록 *하드 코딩* 된 스타일을 포함 합니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage"
             Title="Sessions" StyleClass="Events">

    <p:ListDataPage.DataSource>
        <p:JsonDataSource Source="http://demo3143189.mockable.io/sessions" />
    </p:ListDataPage.DataSource>

</p:ListDataPage>
```

**JSON 데이터**

데모 원본의 JSON 데이터 예제는 다음과 같습니다.

```json
[{
  "end": "2016-04-27T18:00:00Z",
  "start": "2016-04-27T17:15:00Z",
  "abstract": "The new Apple TV has been released, and YOU can be one of the first developers to write apps for it. To make things even better, you can build these apps in C#! This session will introduce the basics of how to create a tvOS app with Xamarin, including: differences between tvOS and iOS APIs, TV user interface best practices, responding to user input, as well as the capabilities and limitations of building apps for a television. Grab some popcorn—this is going to be good!",
  "title": "As Seen On TV … Bringing C# to the Living Room",
  "presenter": "Matthew Soucoup",
  "biography": "Matthew is a Xamarin MVP and Certified Xamarin Developer from Madison, WI. He founded his company Code Mill Technologies and started the Madison Mobile .Net Developers Group.  Matt regularly speaks on .Net and Xamarin development at user groups, code camps and conferences throughout the Midwest. Matt gardens hot peppers, rides bikes, and loves Wisconsin micro-brews and cheese.",
  "image": "http://i.imgur.com/ASj60DP.jpg",
  "avatar": "http://i.imgur.com/ASj60DP.jpg",
  "room": "Crick"
}]
```

## <a name="4-run"></a>4. 실행!

위의 단계를 수행 하면 작업 데이터 페이지가 생성 됩니다.

[![](get-started-images/demo-sml.png "DataPages Sample Application")](get-started-images/demo.png#lightbox "DataPages Sample Application")

이는 미리 작성 된 스타일 **"이벤트"** 가 밝은 테마 NuGet 패키지에 있고 데이터 원본과 일치 하는 스타일이 정의 되어 있기 때문에 작동 합니다 (예: "title", "image", "presenter").

"이벤트"는 `StyleClass` `ListDataPage` 에 정의 된 사용자 지정 컨트롤을 사용 하 여 컨트롤을 표시 하도록 빌드됩니다 `CardView` Xamarin.Forms . 마주보. 컨트롤에는 `CardView` `ImageSource` , 및의 세 가지 속성이 `Text` `Detail` 있습니다. 이 테마는 데이터 원본의 세 필드 (JSON 파일에서)를 표시 하기 위해 이러한 속성에 바인딩하기 위해 하드 코딩 됩니다.

## <a name="5-customize"></a>5. 사용자 지정

상속 된 스타일은 템플릿을 지정 하 고 데이터 소스 바인딩을 사용 하 여 재정의할 수 있습니다. 아래 XAML은에 포함 된 new 및 구문을 사용 하 여 각 행에 대 한 사용자 지정 템플릿을 선언 합니다 `ListItemControl` `{p:DataSourceBinding}` ** Xamarin.Forms . 페이지** NuGet:

```xaml
<p:ListDataPage.DefaultItemTemplate>
    <DataTemplate>
        <ViewCell>
            <p:ListItemControl
                Title="{p:DataSourceBinding title}"
                Detail="{p:DataSourceBinding room}"
                ImageSource="{p:DataSourceBinding image}"
                DataSource="{Binding Value}"
                HeightRequest="90"
            >
            </p:ListItemControl>
        </ViewCell>
    </DataTemplate>
</p:ListDataPage.DefaultItemTemplate>
```

이 코드를 제공 하 여를 `DataTemplate` 재정의 하 `StyleClass` 고 대신의 기본 레이아웃을 사용 `ListItemControl` 합니다.

[![](get-started-images/custom-sml.png "DataPages Sample Application")](get-started-images/custom.png#lightbox "DataPages Sample Application")

C #에 대 한 c #을 선호 하는 개발자는 데이터 소스 바인딩만 만들 수 있습니다 (문은 포함 해야 합니다 `using Xamarin.Forms.Pages;` ).

```csharp
SetBinding (TitleProperty, new DataSourceBinding ("title"));
```

테마를 처음부터 만드는 데 더 많은 작업이 필요 하지만 이후 미리 보기 릴리스에서는이 작업을 보다 쉽게 수행할 수 있습니다.

## <a name="troubleshooting"></a>문제 해결

## <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>' 파일 또는 어셈블리를 로드할 수 없습니다 Xamarin.Forms . ' Theme 또는 종속 항목 중 하나입니다.

미리 보기 릴리스에서 테마는 런타임에 로드 하지 못할 수 있습니다. 아래에 표시 된 코드를 관련 프로젝트에 추가 하 여이 오류를 해결 하십시오.

**iOS**

**AppDelegate.cs** 에서 다음 줄을 추가 합니다.`LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

**MainActivity.cs** 에서 다음 줄을 추가 합니다.`LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```

## <a name="related-links"></a>관련 링크

- [DataPagesDemo 샘플](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)
