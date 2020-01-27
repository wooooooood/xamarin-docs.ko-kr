---
title: 开始使用 DataPages
description: 本文介绍如何开始构建使用 Xamarin.Forms DataPages 一个简单的数据驱动页面。
ms.prod: xamarin
ms.assetid: 6416E5FA-6384-4298-BAA1-A89381E47210
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 1f7917784ea66c31979b87f43639a7d03756692c
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725597"
---
# <a name="getting-started-with-datapages"></a>开始使用 DataPages

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)

![](~/media/shared/preview.png "This API is currently in preview")

> [!IMPORTANT]
> DataPages 要求使用 Xamarin. Forms 主题引用来呈现。 这涉及到将[xamarin. Base](https://www.nuget.org/packages/Xamarin.Forms.Theme.Base/) nuget 包安装到项目中，然后将其后跟[xamarin](https://www.nuget.org/packages/Xamarin.Forms.Theme.Light/)或[xamarin. 暗体](https://www.nuget.org/packages/Xamarin.Forms.Theme.Dark/)nuget 包。

若要开始构建使用 DataPages 预览一个简单的数据驱动页面，请执行以下步骤。 在预览中的硬编码样式 （"事件"） 生成此演示使用仅适用于在代码中特定的 JSON 格式。

[![](get-started-images/demo-sml.png "DataPages Sample Application")](get-started-images/demo.png#lightbox "DataPages Sample Application")

## <a name="1-add-nuget-packages"></a>1. NuGet 패키지 추가

이러한 NuGet 패키지를 Xamarin.ios .NET Standard 라이브러리 및 응용 프로그램 프로젝트에 추가 합니다.

- Xamarin.Forms.Pages
- Xamarin.Forms.Theme.Base
- 테마 구현 NuGet (예: Xamarin.Forms.Theme.Light)

## <a name="2-add-theme-reference"></a>2. 테마 참조 추가

在中**App.xaml**文件中，添加一个自定义`xmlns:mytheme`主题，并确保主题合并到应用程序的资源字典：

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
> IOS `AppDelegate` 및 Android `MainActivity`에 몇 가지 상용구 코드를 추가 하 여 테마 어셈블리를 로드 하는 단계를 수행 해야 합니다 [(아래 참조)](#loadtheme) . 这将在将来的预览版的版本中得到改进。

## <a name="3-add-a-xaml-page"></a>3. XAML 페이지 추가

将新的 XAML 页面添加到 Xamarin.Forms 应用程序，并*将基类更改*从`ContentPage`到`Xamarin.Forms.Pages.ListDataPage`。 这必须在 C# 和 XAML 中进行：

**C# 文件**

```csharp
public partial class SessionDataPage : Xamarin.Forms.Pages.ListDataPage // was ContentPage
{
  public SessionDataPage ()
  {
    InitializeComponent ();
  }
}
```

**XAML 文件**

除了更改到的根元素之外`<p:ListDataPage>`的自定义命名空间`xmlns:p`还必须添加：

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage">

    <ContentPage.Content></ContentPage.Content>

</p:ListDataPage>
```

**应用程序子类**

更改`App`类构造函数，以便`MainPage`设置为`NavigationPage`包含新`SessionDataPage`。 一个导航页*必须*使用。

```csharp
MainPage = new NavigationPage (new SessionDataPage ());
```

## <a name="3-add-the-datasource"></a>3. DataSource를 추가 합니다.

删除`Content`元素并将其替换为`p:ListDataPage.DataSource`来填充的数据页。 在远程 Json 下面的示例从 URL 是正在加载数据文件。

> [!NOTE]
> 미리 보기에는 데이터 원본에 대 한 렌더링 힌트를 제공 하는 `StyleClass` 특성이 *필요* 합니다. `StyleClass="Events"`指的是在预览中预定义的包含样式的布局*硬编码*以匹配正在使用的 JSON 数据源。

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

**JSON 数据**

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

上述步骤应导致工作数据页：

[![](get-started-images/demo-sml.png "DataPages Sample Application")](get-started-images/demo.png#lightbox "DataPages Sample Application")

이는 미리 작성 된 스타일 **"이벤트"** 가 밝은 테마 NuGet 패키지에 있고 데이터 원본과 일치 하는 스타일이 정의 되어 있기 때문에 작동 합니다 (예: "title"、"图像"、"表示器"）。

"事件"`StyleClass`旨在显示`ListDataPage`具有一个自定义控件`CardView`Xamarin.Forms.Pages 中定义的控件。 `CardView`控件具有三个属性： `ImageSource`， `Text`，和`Detail`。 主题是硬编码要绑定的数据源 （从 JSON 文件中） 到显示这些属性的三个字段。

## <a name="5-customize"></a>5. 사용자 지정

可以通过指定的模板并使用数据源绑定中重写继承的样式。 아래 XAML은 새 `ListItemControl` 및 `{p:DataSourceBinding}` 구문을 사용 하 여 각 행에 대 한 사용자 지정 템플릿을 **선언 합니다.**

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

通过提供`DataTemplate`此代码将重写`StyleClass`，而是使用的默认布局`ListItemControl`。

[![](get-started-images/custom-sml.png "DataPages Sample Application")](get-started-images/custom.png#lightbox "DataPages Sample Application")

开发人员喜欢 C# 到 XAML 可以创建数据源绑定过 (请记住包括`using Xamarin.Forms.Pages;`语句):

```csharp
SetBinding (TitleProperty, new DataSourceBinding ("title"));
```

테마를 처음부터 만드는 데 더 많은 작업이 필요 하지만 이후 미리 보기 릴리스에서는이 작업을 보다 쉽게 수행할 수 있습니다.

## <a name="troubleshooting"></a>故障排除

<a name="loadtheme" />

## <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>无法加载文件或程序集 Xamarin.Forms.Theme.Light 或其某个依赖项

在预览版本中，主题可能不能在运行时加载。 添加代码，如下所示在相关项目来修复此错误。

**Android**

在中**AppDelegate.cs**添加以下行后 `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**OWA(Outlook Web Access)**

在中**MainActivity.cs**添加以下行后 `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```

## <a name="related-links"></a>관련 링크

- [DataPagesDemo 示例](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)
