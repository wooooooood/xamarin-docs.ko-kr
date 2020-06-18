---
title: 'title: “항목 사용자 지정” description: “Xamarin.Forms 항목 컨트롤을 사용하면 한 줄 텍스트를 편집할 수 있습니다.'
description: '이 문서에서는 개발자가 자체적인 플랫폼별 사용자 지정을 통해 기본 네이티브 렌더링을 재정의할 수 있도록 항목 컨트롤에 대한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다.” ms.prod: xamarin ms.assetid: 7B5DD10D-0411-424F-88D8-8A474DF16D8D ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 11/26/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: 7B5DD10D-0411-424F-88D8-8A474DF16D8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/26/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d28a9079d27310dde0e5ea5bf80c83895bbcf1d4
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571573"
---
# <a name="customizing-an-entry"></a>항목 사용자 지정

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-entry)

Xamarin.Forms 항목 컨트롤을 사용하면 한 줄 텍스트를 편집할 수 있습니다. 이 문서에서는 개발자가 자체적인 플랫폼별 사용자 지정을 통해 기본 네이티브 렌더링을 재정의할 수 있도록 항목 컨트롤에 대한 사용자 지정 렌더러를 만드는 방법을 보여줍니다.

모든 Xamarin.Forms 컨트롤의 모양과 동작을 효과적으로 사용자 지정할 수 있습니다. [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤이 Xamarin.Forms 애플리케이션에 의해 렌더링되면 iOS에서 `EntryRenderer` 클래스가 인스턴스화되며, 차례로 네이티브 `UITextField` 컨트롤이 인스턴스화됩니다. Android 플랫폼에서 `EntryRenderer` 클래스는 `EditText` 컨트롤을 인스턴스화합니다. UWP(유니버설 Windows 플랫폼)에서 `EntryRenderer` 클래스는 `TextBox` 컨트롤을 인스턴스화합니다. Xamarin.Forms 컨트롤에 매핑되는 렌더러 및 네이티브 컨트롤 클래스에 대한 자세한 내용은 [렌더러 기본 클래스 및 네이티브 컨트롤](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)을 참조하세요.

다음 다이어그램은 [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤 및 이를 구현하는 해당 네이티브 컨트롤 간의 관계를 보여줍니다.

![](entry-images/entry-classes.png "Relationship Between Entry Control and Implementing Native Controls")

렌더링 프로세스는 각 플랫폼에서 [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤에 대한 사용자 지정 렌더러를 만들어 플랫폼별 사용자 지정을 구현하는 데 활용할 수 있습니다. 이 작업을 수행하는 프로세스는 다음과 같습니다.

1. Xamarin.Forms 사용자 지정 컨트롤을 [만듭니다](#creating-the-custom-entry-control).
1. Xamarin.Forms에서 사용자 지정 컨트롤을 [사용합니다](#consuming-the-custom-control).
1. 각 플랫폼의 컨트롤에 대한 사용자 지정 렌더러를 [만듭니다](#creating-the-custom-renderer-on-each-platform).

이제 각 플랫폼에 서로 다른 배경색을 가진 [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤을 구현하기 위해 각 항목을 차례로 설명합니다.

> [!IMPORTANT]
> 이 문서에서는 간단한 사용자 지정 렌더러를 만드는 방법을 설명합니다. 그러나 각 플랫폼에 서로 다른 배경색을 가진 `Entry`를 구현하기 위해 사용자 지정 렌더러를 만들 필요는 없습니다. 플랫폼별 값을 제공하기 위해 [`Device`](xref:Xamarin.Forms.Device) 클래스 또는 `OnPlatform` 태그 확장을 사용하면 더 쉽게 수행할 수 있습니다. 자세한 내용은 [플랫폼별 값 제공](~/xamarin-forms/platform/device.md#provide-platform-specific-values) 및 [OnPlatform 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension)을 참조하세요.

## <a name="creating-the-custom-entry-control"></a>사용자 지정 항목 컨트롤 만들기

다음 코드 예제와 같이 `Entry` 컨트롤을 서브클래싱하여 사용자 지정 [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤을 만들 수 있습니다.

```csharp
public class MyEntry : Entry
{
}
```

`MyEntry` 컨트롤은 .NET Standard 라이브러리 프로젝트에서 만들어지며 단순히 [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤입니다. 컨트롤의 사용자 지정은 사용자 지정 렌더러에서 수행되므로 `MyEntry` 컨트롤에서 추가 구현이 필요하지 않습니다.

## <a name="consuming-the-custom-control"></a>사용자 지정 컨트롤 사용

`MyEntry` 컨트롤은 해당 위치의 네임스페이스를 선언하고 컨트롤 요소의 네임스페이스 접두사를 사용하여 .NET Standard 라이브러리 프로젝트의 XAML에서 참조할 수 있습니다. 다음 코드 예제에서는 `MyEntry` 컨트롤을 XAML 페이지에서 사용하는 방법을 보여줍니다.

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

`local` 네임스페이스 접두사는 원하는 것으로 이름을 지정할 수 있습니다. 그러나 `clr-namespace` 및 `assembly` 값은 사용자 지정 컨트롤의 세부 정보와 일치해야 합니다. 네임스페이스가 선언되면 사용자 지정 컨트롤을 참조하는 데 사용됩니다.

다음 코드 예제에서는 C# 페이지에서 `MyEntry` 컨트롤을 사용하는 방법을 보여줍니다.

```csharp
public class MainPage : ContentPage
{
  public MainPage ()
  {
    Content = new StackLayout {
      Children = {
        new Label {
          Text = "Hello, Custom Renderer !",
        },
        new MyEntry {
          Text = "In Shared Code",
        }
      },
      VerticalOptions = LayoutOptions.CenterAndExpand,
      HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
  }
}
```

이 코드는 페이지에 가로 및 세로 모두 중앙에 있는 [`Label`](xref:Xamarin.Forms.Label) 및 `MyEntry` 컨트롤을 표시할 새 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체를 인스턴스화합니다.

이제 각 애플리케이션 프로젝트에 사용자 지정 렌더러를 추가하여 각 플랫폼에서 컨트롤의 모양을 사용자 지정할 수 있습니다.

## <a name="creating-the-custom-renderer-on-each-platform"></a>각 플랫폼에서 사용자 지정 렌더러 만들기

사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. 네이티브 컨트롤을 렌더링하는 `EntryRenderer` 클래스의 서브클래스를 만듭니다.
1. 네이티브 컨트롤을 렌더링하고 컨트롤을 사용자 지정하기 위한 논리를 작성하는 `OnElementChanged` 메서드를 재정의합니다. 이 메서드는 해당 Xamarin.Forms 컨트롤이 생성될 때 호출됩니다.
1. 사용자 지정 렌더러 클래스에 `ExportRenderer` 특성을 추가하여 Xamarin.Forms 컨트롤을 렌더링하는 데 사용하도록 지정합니다. 이 특성은 사용자 지정 렌더러를 Xamarin.Forms에 등록하는 데 사용됩니다.

> [!NOTE]
> 각 플랫폼 프로젝트에서 사용자 지정 렌더러를 제공하는 것은 선택 사항입니다. 사용자 지정 렌더러가 등록되지 않은 경우 컨트롤의 기본 클래스에 대한 기본 렌더러가 사용됩니다.

다음 다이어그램은 샘플 애플리케이션에서 각 프로젝트의 책임과 이들 간의 관계를 보여줍니다.

![](entry-images/solution-structure.png "MyEntry Custom Renderer Project Responsibilities")

`MyEntry` 컨트롤은 각 플랫폼의 `EntryRenderer` 클래스에서 모두 파생되는 플랫폼별 `MyEntryRenderer` 클래스에 의해 렌더링됩니다. 그러면 다음 스크린샷과 같이 각 `MyEntry` 컨트롤이 플랫폼별 배경색으로 렌더링됩니다.

![](entry-images/screenshots.png "MyEntry Control on each Platform")

`EntryRenderer` 클래스는 해당 네이티브 컨트롤을 렌더링하기 위해 Xamarin.Forms 컨트롤이 생성될 때 호출되는 `OnElementChanged` 메서드를 노출합니다. 이 메서드는 `OldElement` 및 `NewElement` 속성이 포함된 `ElementChangedEventArgs` 매개 변수를 가져옵니다. 이러한 속성은 렌더러가 ‘연결된’ Xamarin.Forms 요소와 렌더러가 ‘연결되는’ Xamarin.Forms 요소를 각각 나타냅니다.  샘플 애플리케이션에서 `OldElement` 속성은 `null`이고, `NewElement` 속성은 `MyEntry` 컨트롤에 대한 참조를 포함합니다.

`MyEntryRenderer` 클래스에서 `OnElementChanged` 메서드의 재정의된 버전은 네이티브 컨트롤 사용자 지정을 수행하는 위치입니다. 플랫폼에서 사용되는 네이티브 컨트롤에 대한 형식화된 참조는 `Control` 속성을 통해 액세스할 수 있습니다. 또한 렌더링되는 Xamarin.Forms 컨트롤에 대한 참조는 샘플 애플리케이션에서는 사용되지 않지만 `Element` 속성을 통해 얻을 수 있습니다.

각 사용자 지정 렌더러 클래스는 렌더러를 Xamarin.Forms에 등록하는 `ExportRenderer` 특성으로 데코레이트됩니다. 이 특성은 렌더링되는 Xamarin.Forms 컨트롤의 형식 이름과 지정 렌더러의 형식 이름이라는 두 가지 매개 변수를 사용합니다. 특성의 `assembly` 접두사는 특성이 전체 어셈블리에 적용되도록 지정합니다.

다음 섹션에서는 각 플랫폼별 `MyEntryRenderer` 사용자 지정 렌더러 클래스의 구현을 설명합니다.

### <a name="creating-the-custom-renderer-on-ios"></a>iOS에서 사용자 지정 렌더러 만들기

다음 코드 예제에서는 iOS 플랫폼용 사용자 지정 렌더러를 보여줍니다.

```csharp
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer (typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.iOS
{
    public class MyEntryRenderer : EntryRenderer
    {
        protected override void OnElementChanged (ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged (e);

            if (Control != null) {
                // do whatever you want to the UITextField here!
                Control.BackgroundColor = UIColor.FromRGB (204, 153, 255);
                Control.BorderStyle = UITextBorderStyle.Line;
            }
        }
    }
}
```

기본 클래스의 `OnElementChanged` 메서드에 대한 호출은 렌더러의 `Control` 속성에 할당된 컨트롤에 대한 참조와 함께 iOS `UITextField` 컨트롤을 인스턴스화합니다. 그런 다음, 배경색은 `UIColor.FromRGB` 메서드를 사용하여 밝은 보라색으로 설정됩니다.

### <a name="creating-the-custom-renderer-on-android"></a>Android에서 사용자 지정 렌더러 만들기

다음 코드 예제에서는 Android 플랫폼용 사용자 지정 렌더러를 보여줍니다.

```csharp
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.Android
{
    class MyEntryRenderer : EntryRenderer
    {
        public MyEntryRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged(e);

            if (Control != null)
            {
                Control.SetBackgroundColor(global::Android.Graphics.Color.LightGreen);
            }
        }
    }
}
```

기본 클래스의 `OnElementChanged` 메서드에 대한 호출은 렌더러의 `Control` 속성에 할당된 컨트롤에 대한 참조와 함께 Android `EditText` 컨트롤을 인스턴스화합니다. 그런 다음, 배경색은 `Control.SetBackgroundColor` 메서드를 사용하여 밝은 녹색으로 설정됩니다.

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP에서 사용자 지정 렌더러 만들기

다음 코드 예제는 UWP용 사용자 지정 렌더러를 보여줍니다.

```csharp
[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.UWP
{
    public class MyEntryRenderer : EntryRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged(e);

            if (Control != null)
            {
                Control.Background = new SolidColorBrush(Colors.Cyan);
            }
        }
    }
}
```

기본 클래스의 `OnElementChanged` 메서드에 대한 호출은 렌더러의 `Control` 속성에 할당된 컨트롤에 대한 참조와 함께 `TextBox` 컨트롤을 인스턴스화합니다. 그런 다음, 배경색은 `SolidColorBrush` 인스턴스를 생성하여 청록색으로 설정합니다.

## <a name="summary"></a>요약

이 문서에서는 개발자가 자체적인 플랫폼별 렌더링을 통해 기본 네이티브 렌더링을 재정의할 수 있도록 하는 Xamarin.Forms [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤의 사용자 지정 컨트롤 렌더러를 만드는 방법을 보여 줍니다. 사용자 지정 렌더러는 Xamarin.Forms 컨트롤의 모양을 사용자 지정하는 강력한 방법을 제공합니다. 작은 스타일 변경 또는 정교한 플랫폼별 레이아웃 및 동작 사용자 지정에 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [CustomRendererEntry(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-entry)
