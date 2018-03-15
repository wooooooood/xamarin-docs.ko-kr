---
title: "DataPages 시작"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6416E5FA-6384-4298-BAA1-A89381E47210
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 3b5346b6d556f6437d9c9fc17897180fd0b41b1e
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
---
# <a name="getting-started-with-datapages"></a>DataPages 시작

![](~/media/shared/preview.png "이 API는 현재 미리 보기")

> [!IMPORTANT]
> DataPages 필요는 [Xamarin.Forms 테마](~/xamarin-forms/user-interface/themes/index.md) 렌더링에 대 한 참조입니다.


DataPages 미리 보기를 사용 하 여 간단한 데이터 드라이브 페이지 구성 시작 하려면 다음 단계를 수행 합니다. 이 데모에서는 미리 보기에서 하드 코드 된 스타일 ("이벤트") 빌드에 코드에서 특정 JSON 형식 에서만 작동 합니다.

[![](get-started-images/demo-sml.png "DataPages 샘플 응용 프로그램")](get-started-images/demo.png#lightbox "DataPages 샘플 응용 프로그램")

## <a name="1-add-nuget-packages"></a>1. NuGet 패키지 추가

Xamarin.Forms PCL 및 응용 프로그램 프로젝트에 이러한 Nuget 패키지를 추가 합니다.

* Xamarin.Forms.Pages
* Xamarin.Forms.Theme.Base
* 테마 구현 Nuget 않음 (예: Xamarin.Forms.Themes.Light)

## <a name="2-add-theme-reference"></a>2. 테마 참조 추가

**App.xaml** 파일, 사용자 지정을 추가 `xmlns:mytheme` 테마에 대 한 테마 응용 프로그램의 리소스 사전에 병합 되는지 확인 하십시오.

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

**중요:** 단계를 따라야 [테마 어셈블리 (아래)를 로드](#loadtheme) 를 ios 일부 상용구 코드를 추가 하 여 `AppDelegate` 및 Android `MainActivity`합니다. 향후 미리 보기 릴리스에서 개선 됩니다.


## <a name="3-add-a-xaml-page"></a>3. XAML 페이지 추가

Xamarin.Forms 응용 프로그램에 새 XAML 페이지를 추가 하 고 *기본 형식을 변경* 에서 `ContentPage` 를 `Xamarin.Forms.Pages.ListDataPage`합니다. 이 C# 및 XAML에서 수행 해야 하는 것 같습니다.

**C# 파일**

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

루트 요소를 변경 하는 것 외에도 `<p:ListDataPage>` 에 대 한 사용자 지정 네임 스페이스 `xmlns:p` 도 추가 해야 합니다.

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

변경은 `App` 클래스 생성자 있도록는 `MainPage` 로 설정 되는 `NavigationPage` 포함 된 새 `SessionDataPage`합니다. 탐색 페이지 *해야* 사용할 수 있습니다.

```csharp
MainPage = new NavigationPage (new SessionDataPage ());
```

## <a name="3-add-the-datasource"></a>3. 데이터 소스 추가

삭제 된 `Content` 요소로 바꿉니다는 `p:ListDataPage.DataSource` 데이터가 있는 페이지를 채우는 데 있습니다. 원격 Json 아래 예제에서는 데이터 파일 URL에서 로드 되 고 있습니다.

**참고:** 미리 보기 *필요* 는 `StyleClass` 특성을 데이터 원본에 대 한 렌더링 힌트를 제공 합니다. `StyleClass="Events"` 참조 하는 미리 보기에 미리 정의 되었으며 스타일을 포함 하는 레이아웃 *하드 코드 된* 사용 하 고 JSON 데이터 원본과 일치 하도록 합니다.

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

JSON 데이터의 예로 [데모 원본](http://demo3143189.mockable.io/sessions) 아래에 표시 됩니다.

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

위의 단계 작업 데이터 페이지에서 발생 해야 합니다.

[![](get-started-images/demo-sml.png "DataPages 샘플 응용 프로그램")](get-started-images/demo.png#lightbox "DataPages 샘플 응용 프로그램")

에서는이 방법이 미리 작성 된 스타일 **"이벤트"** 밝은 테마 Nuget 패키지에 존재 하며 데이터 원본 (예: 일치 하는 정의 된 스타일. "제목", "image", "발표자").

"이벤트" `StyleClass` 표시 하기 위해 빌드된는 `ListDataPage` 를 사용자 지정 컨트롤 `CardView` 즉에 정의 된 Xamarin.Forms.Pages를 제어 합니다. `CardView` 컨트롤에는 세 가지 속성: `ImageSource`, `Text`, 및 `Detail`합니다. 이러한 속성을 디스플레이 대 한 JSON 파일) (에서 datasource의 세 필드에 바인딩할 하드 코드 된은 있습니다.

## <a name="5-customize"></a>5. 사용자 지정

상속 된 스타일 템플릿을 지정 하 고 데이터 소스 바인딩을 사용 하 여 재정의할 수 있습니다. 다음 XAML 선언 새를 사용 하 여 각 행에 대 한 사용자 지정 템플릿을 `ListItemControl` 및 `{p:DataSourceBinding}` 구문에 포함 되어 있는 **Xamarin.Forms.Pages** Nuget:

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

제공 하 여는 `DataTemplate` 재정의이 코드는 `StyleClass` 대신에 기본 레이아웃을 사용 하 여는 `ListItemControl`합니다.

[![](get-started-images/custom-sml.png "DataPages 샘플 응용 프로그램")](get-started-images/custom.png#lightbox "DataPages 샘플 응용 프로그램")

C# XAML로 데이터를 만들 수는 것을 선호 하는 개발자가 원본 바인딩을 너무 (포함 해야는 `using Xamarin.Forms.Pages;` 문):

```csharp
SetBinding (TitleProperty, new DataSourceBinding ("title"));
```


테마를 처음부터 만들 하려면 좀 더 많은 작업이 (참조는 [테마 가이드](~/xamarin-forms/user-interface/themes/index.md)) 있지만, 이후 미리 보기 릴리스에서이 더 편리 합니다.


## <a name="troubleshooting"></a>문제 해결

<a name="loadtheme" />

## <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>파일이 나 어셈블리 'Xamarin.Forms.Theme.Light' 또는 해당 종속성 중 하나를 로드할 수 없습니다.

미리 보기 릴리스에서 테마 런타임에 로드할 수 되지 않을 수 있습니다. 이 오류를 해결 하려면 관련 된 프로젝트에 아래 표시 된 코드를 추가 합니다.

**iOS**

에 **AppDelegate.cs** 후 다음 줄 추가 `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

에 **MainActivity.cs** 후 다음 줄 추가 `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```



## <a name="related-links"></a>관련 링크

- [DataPagesDemo 샘플](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)
