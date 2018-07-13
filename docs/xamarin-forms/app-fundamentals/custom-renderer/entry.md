---
title: 항목을 사용자 지정
description: Xamarin.Forms 항목 컨트롤을 한 줄을 텍스트를 편집할 수 있습니다. 이 문서에는 개발자가 자신의 플랫폼 특정 사용자 지정을 사용 하 여 기본 네이티브 렌더링을 재정의할 수 있도록 항목 컨트롤에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 7B5DD10D-0411-424F-88D8-8A474DF16D8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 30326b8d52f39268015bdcbee1b84b9d9e5516b9
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998962"
---
# <a name="customizing-an-entry"></a>항목을 사용자 지정

_Xamarin.Forms 항목 컨트롤을 한 줄을 텍스트를 편집할 수 있습니다. 이 문서에는 개발자가 자신의 플랫폼 특정 사용자 지정을 사용 하 여 기본 네이티브 렌더링을 재정의할 수 있도록 항목 컨트롤에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다._

모든 Xamarin.Forms 컨트롤에 네이티브 컨트롤의 인스턴스를 만드는 각 플랫폼에 대 한는 함께 제공 되는 렌더러. 경우는 [ `Entry` ](xref:Xamarin.Forms.Entry) iOS에서 Xamarin.Forms 응용 프로그램에서 컨트롤이 렌더링 되는 `EntryRenderer` 클래스가 인스턴스화되면 네이티브 인스턴스화합니다 결과적으로 `UITextField` 컨트롤입니다. Android 플랫폼에는 `EntryRenderer` 클래스를 인스턴스화하는 `EditText` 제어 합니다. Windows 플랫폼 (UWP (유니버설)을는 `EntryRenderer` 클래스를 인스턴스화하는 `TextBox` 제어 합니다. 렌더러 및 Xamarin.Forms 컨트롤에 매핑되는 네이티브 컨트롤 클래스에 대 한 자세한 내용은 참조 하세요. [렌더러 기본 클래스 및 네이티브 컨트롤](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)합니다.

다음 다이어그램 간의 관계는 [ `Entry` ](xref:Xamarin.Forms.Entry) 제어 및 구현 하는 해당 네이티브 컨트롤:

![](entry-images/entry-classes.png "항목 컨트롤과 네이티브 컨트롤을 구현 간의 관계")

렌더링 프로세스에 대 한 사용자 지정 렌더러를 만들어 플랫폼별 사용자 지정을 구현 하 여 활용 취할 수 있습니다 합니다 [ `Entry` ](xref:Xamarin.Forms.Entry) 각 플랫폼에서 제어 합니다. 이 수행 하는 프로세스는 다음과 같습니다.

1. [만들](#Creating_the_Custom_Entry_Control) Xamarin.Forms 사용자 지정 컨트롤입니다.
1. [사용할](#Consuming_the_Custom_Control) Xamarin.Forms에서 사용자 지정 컨트롤입니다.
1. [만들](#Creating_the_Custom_Renderer_on_each_Platform) 각 플랫폼에서 컨트롤에 대 한 사용자 지정 렌더러.

각 항목 이제 살펴봅니다를 구현 하는 [ `Entry` ](xref:Xamarin.Forms.Entry) 각 플랫폼에서 다른 배경색을 보유 하는 컨트롤입니다.

<a name="Creating_the_Custom_Entry_Control" />

## <a name="creating-the-custom-entry-control"></a>사용자 지정 항목 컨트롤 만들기

사용자 지정 [ `Entry` ](xref:Xamarin.Forms.Entry) 컨트롤을 서브클래싱하 여 생성할 수는 `Entry` 다음 코드 예제와 같이 제어:

```csharp
public class MyEntry : Entry
{
}
```

합니다 `MyEntry` 제어.NET Standard 라이브러리 프로젝트에서 생성 되 고 인지는 단순히 [ `Entry` ](xref:Xamarin.Forms.Entry) 제어 합니다. 컨트롤의 사용자 지정을 수행할 사용자 지정 렌더러에 추가 구현이 필요 하므로 `MyEntry` 제어 합니다.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>사용자 지정 컨트롤 사용

`MyEntry` 컨트롤에서에서 참조할 수 있습니다 XAML.NET Standard 라이브러리 프로젝트에서 해당 위치에 대 한 네임 스페이스를 선언 하 고 컨트롤 요소에서 네임 스페이스 접두사를 사용 하 여 합니다. 다음 코드 예제에서는 방법을 `MyEntry` XAML 페이지에서 컨트롤을 사용할 수 있습니다.

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

`local` 원하는 네임 스페이스 접두사를 이름을 지정할 수 있습니다. 그러나 합니다 `clr-namespace` 고 `assembly` 값 사용자 지정 컨트롤의 세부 정보를 일치 해야 합니다. 네임 스페이스를 선언 하는 사용자 지정 컨트롤을 참조 하는 접두사가 사용 됩니다.

다음 코드 예제에서는 방법을 `MyEntry` C# 페이지에서 컨트롤을 사용할 수 있습니다.

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

이 코드는 새 인스턴스화합니다 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 표시 되는 개체를 [ `Label` ](xref:Xamarin.Forms.Label) 및 `MyEntry` 컨트롤 둘 다 세로 및 가로로 가운데에 맞추도록 페이지입니다.

사용자 지정 렌더러는 이제 각 플랫폼에서 컨트롤의 모양을 사용자 지정할 각 응용 프로그램 프로젝트에 추가할 수 있습니다.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>각 플랫폼에서 사용자 지정 렌더러 만들기

사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. 서브 클래스를 만든를 `EntryRenderer` 네이티브 컨트롤을 렌더링 하는 클래스입니다.
1. 재정의 `OnElementChanged` 네이티브 컨트롤 및 쓰기는 논리를 컨트롤을 사용자 지정 렌더링 하는 메서드. 이 메서드는 해당 하는 Xamarin.Forms 컨트롤이 만들어질 때 호출 됩니다.
1. 추가 `ExportRenderer` 특성을 사용자 지정 렌더러 클래스 Xamarin.Forms 컨트롤을 렌더링 하 사용 수를 지정할 수 있습니다. 이 특성은 Xamarin.Forms를 사용 하 여 사용자 지정 렌더러를 등록 하는 데 사용 됩니다.

> [!NOTE]
> 각 플랫폼 프로젝트에서 사용자 지정 렌더러를 제공 하는 선택 사항입니다. 사용자 지정 렌더러를 등록 하지 않은 경우 컨트롤의 기본 클래스에 대 한 기본 렌더러 사용 됩니다.

다음 다이어그램은 이들 간의 관계와 함께 샘플 응용 프로그램에서 각 프로젝트의 책임을 보여 줍니다.

![](entry-images/solution-structure.png "MyEntry 사용자 지정 렌더러 프로젝트 책임")

합니다 `MyEntry` 플랫폼 관련 하 여 컨트롤이 렌더링 되는지 `MyEntryRenderer` 클래스에서 파생 되는 `EntryRenderer` 각 플랫폼에 대 한 클래스입니다. 이 인해 각 `MyEntry` 다음 스크린샷과에서 같이 플랫폼별 배경색을 사용 하 여 렌더링 되 고 제어 합니다.

![](entry-images/screenshots.png "각 플랫폼에서 MyEntry 컨트롤")

합니다 `EntryRenderer` 노출 클래스는 `OnElementChanged` 해당 네이티브 컨트롤을 렌더링 하는 Xamarin.Forms 컨트롤을 만들 때 호출 되는 메서드. 이 메서드는 `ElementChangedEventArgs` 포함 된 매개 변수 `OldElement` 및 `NewElement` 속성입니다. 이러한 속성은 Xamarin.Forms 요소를 나타냅니다는 렌더러 *되었습니다* 에 연결 및 Xamarin.Forms 요소는 렌더러 *는* 을 각각 연결 합니다. 샘플 응용 프로그램에서을 `OldElement` 속성이 `null` 및 `NewElement` 속성에 대 한 참조가 포함 됩니다는 `MyEntry` 제어 합니다.

재정의 된 버전을 `OnElementChanged` 의 메서드는 `MyEntryRenderer` 클래스는 네이티브 컨트롤 사용자 지정을 수행 하는 위치입니다. 플랫폼에서 사용 되는 네이티브 컨트롤에 대 한 형식화 된 참조를 통해 액세스할 수는 `Control` 속성입니다. 또한를 통해 렌더링 되는 Xamarin.Forms 컨트롤에 대 한 참조를 가져올 수 있습니다는 `Element` 속성인 있지만 샘플 응용 프로그램에서 사용 되지 않습니다.

으로 데코 레이트 된 각 사용자 지정 렌더러 클래스는 `ExportRenderer` Xamarin.Forms를 사용 하 여 렌더러를 등록 하는 특성입니다. 특성 매개 변수 두 개 – 렌더링 되 고 Xamarin.Forms 컨트롤의 형식 이름 및 사용자 지정 렌더러의 형식 이름입니다. `assembly` 접두사 특성에 특성을 전체 어셈블리에 적용 되도록 지정 합니다.

다음 섹션에서는 각 플랫폼별 구현을 다룹니다 `MyEntryRenderer` 사용자 지정 렌더러 클래스입니다.

### <a name="creating-the-custom-renderer-on-ios"></a>IOS에서 사용자 지정 렌더러 만들기

다음 코드 예제에서는 iOS 플랫폼에 대 한 사용자 지정 렌더러를 보여 줍니다.

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

기본 클래스에 대 한 호출 `OnElementChanged` 메서드는 iOS를 인스턴스화하고 `UITextField` 렌더러의에 할당 되는 컨트롤에 대 한 참조를 사용 하 여 컨트롤 `Control` 속성. 배경색을 사용 하 여 간단한 보라색으로 설정 됩니다는 `UIColor.FromRGB` 메서드.

### <a name="creating-the-custom-renderer-on-android"></a>Android에서 사용자 지정 렌더러 만들기

다음 코드 예제에서는 Android 플랫폼에 대 한 사용자 지정 렌더러를 보여 줍니다.

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

기본 클래스에 대 한 호출 `OnElementChanged` 메서드는 Android 인스턴스화합니다 `EditText` 렌더러의에 할당 되는 컨트롤에 대 한 참조를 사용 하 여 컨트롤 `Control` 속성입니다. 배경색으로 밝은 녹색으로 설정 됩니다는 `Control.SetBackgroundColor` 메서드.

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP의 사용자 지정 렌더러 만들기

다음 코드 예제에서는 UWP에 대 한 사용자 지정 렌더러를 보여 줍니다.

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

기본 클래스에 대 한 호출 `OnElementChanged` 메서드를 만드는 데는 `TextBox` 렌더러의에 할당 되는 컨트롤에 대 한 참조를 사용 하 여 컨트롤 `Control` 속성입니다. 배경색은 다음을 만들어 cyan으로 설정 됩니다는 `SolidColorBrush` 인스턴스.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Forms에 대 한 사용자 지정 컨트롤 렌더러를 만드는 방법을 보여 주었습니다 [ `Entry` ](xref:Xamarin.Forms.Entry) 컨트롤 개발자가 자신의 플랫폼별 렌더링을 사용 하 여 기본 네이티브 렌더링을 재정의할 수 있도록 합니다. 사용자 지정 렌더러 Xamarin.Forms 컨트롤의 모양을 사용자 지정 하는 강력한 방법을 제공 합니다. 이러한 작은 스타일 변경 또는 복잡 한 플랫폼 특정 레이아웃 동작 사용자 지정에 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [CustomRendererEntry (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/entry/)
