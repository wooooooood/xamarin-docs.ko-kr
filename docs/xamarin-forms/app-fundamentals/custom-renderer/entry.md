---
title: 항목을 사용자 지정
description: Xamarin.Forms 입력 컨트롤을 편집할 수는 텍스트 한 줄 수 있습니다. 이 문서에는 개발자가 자신의 플랫폼 관련 사용자 지정과 기본 네이티브 렌더링을 재정의할 수 있도록 항목 컨트롤에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 7B5DD10D-0411-424F-88D8-8A474DF16D8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 5f23b65fab24b447a9f534ed7403797a60cc284f
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847227"
---
# <a name="customizing-an-entry"></a>항목을 사용자 지정

_Xamarin.Forms 입력 컨트롤을 편집할 수는 텍스트 한 줄 수 있습니다. 이 문서에는 개발자가 자신의 플랫폼 관련 사용자 지정과 기본 네이티브 렌더링을 재정의할 수 있도록 항목 컨트롤에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다._

Xamarin.Forms는 모든 컨트롤에 네이티브 컨트롤의 인스턴스를 생성 하는 각 플랫폼에 대 한 함께 제공 되는 렌더러 있습니다. 경우는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 컨트롤이 ios에서 Xamarin.Forms 응용 프로그램에서 렌더링 되는 `EntryRenderer` 결과적으로 인스턴스화합니다 네이티브 클래스를 인스턴스화할 `UITextField` 제어 합니다. Android 플랫폼의 `EntryRenderer` 클래스를 인스턴스화하는 `EditText` 제어 합니다. 에 플랫폼 UWP (유니버설 Windows)는 `EntryRenderer` 클래스를 인스턴스화하는 `TextBox` 제어 합니다. 렌더러 및 Xamarin.Forms 컨트롤에 매핑되는 네이티브 컨트롤 클래스에 대 한 자세한 내용은 참조 [렌더러 기본 클래스와 기본 컨트롤](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)합니다.

다음 다이어그램에서는 간의 관계는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 제어 및 구현 하는 해당 네이티브 컨트롤:

![](entry-images/entry-classes.png "항목 컨트롤과 네이티브 컨트롤을 구현 간의 관계")

렌더링 프로세스 활용 하기 위해 취할 수에 대 한 사용자 지정 렌더러를 만들어 플랫폼 관련 사용자 지정을 구현는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 각 플랫폼에서 제어 합니다. 이 수행 하는 프로세스는 다음과 같습니다.

1. [만들](#Creating_the_Custom_Entry_Control) Xamarin.Forms 사용자 지정 컨트롤입니다.
1. [사용할](#Consuming_the_Custom_Control) Xamarin.Forms에서 사용자 지정 컨트롤입니다.
1. [만들](#Creating_the_Custom_Renderer_on_each_Platform) 각 플랫폼에서 컨트롤에 대 한 사용자 지정 렌더러 합니다.

각 항목 이제 살펴봅니다 차례로 구현 하는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 에 각 플랫폼에 다른 배경색을 제어 합니다.

<a name="Creating_the_Custom_Entry_Control" />

## <a name="creating-the-custom-entry-control"></a>사용자 지정 항목 컨트롤 만들기

사용자 지정 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 서브클래싱하 여 컨트롤을 만들 수 있습니다는 `Entry` 다음 코드 예제와 같이 제어:

```csharp
public class MyEntry : Entry
{
}
```

`MyEntry` 컨트롤은 표준.NET 라이브러리 프로젝트에서 만들어지며는 단순히 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 제어 합니다. 컨트롤의 사용자 지정을 수행할 사용자 지정 렌더러의 하므로 추가 구현이 필요는 `MyEntry` 제어 합니다.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>사용자 지정 컨트롤 사용

`MyEntry` 컨트롤 수 XAML에서 참조할 수.NET 표준 라이브러리 프로젝트에서 해당 위치에 대 한 네임 스페이스를 선언 하 고 컨트롤 요소에 네임 스페이스 접두사를 사용 합니다. 다음 코드 예제는 방법을 `MyEntry` 컨트롤 XAML 페이지 에서도 사용할 수 있습니다.

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

`local` 원하는 네임 스페이스 접두사를 이름을 지정할 수 있습니다. 그러나는 `clr-namespace` 및 `assembly` 값에는 사용자 지정 컨트롤의 세부 정보과 일치 해야 합니다. 네임 스페이스 선언 되 면 사용자 지정 컨트롤을 참조 하는 접두사가 사용 됩니다.

다음 코드 예제는 방법을 `MyEntry` 컨트롤 C# 페이지 에서도 사용할 수 있습니다.

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

이 코드를 인스턴스화합니다 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 표시 하는 개체는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 및 `MyEntry` 컨트롤 둘 다 가로 및 세로로 페이지의 중앙에 있습니다.

사용자 지정 렌더러는 이제 각 플랫폼에서 컨트롤의 모양을 사용자 지정할 각 응용 프로그램 프로젝트에 추가할 수 있습니다.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>각 플랫폼에서 사용자 지정 렌더러 만들기

사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. 서브 클래스를 만든는 `EntryRenderer` 네이티브 컨트롤을 렌더링 하는 클래스입니다.
1. 재정의 `OnElementChanged` 컨트롤을 사용자 지정 하는 기본 권한 및 쓰기 논리를 렌더링 하는 메서드입니다. 이 메서드는 해당 하는 Xamarin.Forms 컨트롤이 만들어질 때 호출 됩니다.
1. 추가 `ExportRenderer` 특성을 사용자 지정 렌더러 클래스 Xamarin.Forms 컨트롤을 렌더링 하 사용 수를 지정할 수 있습니다. 이 특성은 Xamarin.Forms를 사용한 사용자 지정 렌더러를 등록 하려면 사용 합니다.

> [!NOTE]
> 이 각 플랫폼 프로젝트에서 사용자 지정 렌더러를 제공 하는 선택 사항. 사용자 지정 렌더러 등록 되지 않은 경우 컨트롤의 기본 클래스에 대 한 기본 렌더러 사용 됩니다.

다음 다이어그램은 이들 간의 관계와 함께 샘플 응용 프로그램의 각 프로젝트의 책임을 보여줍니다.

![](entry-images/solution-structure.png "MyEntry 사용자 지정 렌더러 프로젝트 책임")

`MyEntry` 플랫폼 관련 하 여 컨트롤의 렌더링 `MyEntryRenderer` 모든에서 파생 되는 클래스는 `EntryRenderer` 각 플랫폼에 대 한 클래스입니다. 이 인해 각 `MyEntry` 다음 스크린샷에 표시 된 것 처럼는 플랫폼 특정 배경색으로 렌더링 되 고 제어 합니다.

![](entry-images/screenshots.png "각 플랫폼에서 MyEntry 컨트롤")

`EntryRenderer` 클래스가 노출은 `OnElementChanged` 메서드를 해당 네이티브 컨트롤을 렌더링 하는 Xamarin.Forms 컨트롤을 만들 때 호출 됩니다. 이 메서드는 `ElementChangedEventArgs` 포함 하는 매개 변수 `OldElement` 및 `NewElement` 속성입니다. Xamarin.Forms 요소를 나타내야 하는 이러한 속성 하는 렌더러 *되었습니다* 에, 연결 된 및 Xamarin.Forms 요소는 렌더러 *은* 에, 각각 연결 된 합니다. 샘플 응용 프로그램에서의 `OldElement` 속성이 `null` 및 `NewElement` 속성에 대 한 참조에 포함 됩니다는 `MyEntry` 제어 합니다.

재정의 된 버전의 `OnElementChanged` 에서 메서드는 `MyEntryRenderer` 클래스는 네이티브 컨트롤 사용자 지정을 수행할 수 있는 곳입니다. 플랫폼에서 사용 되는 네이티브 컨트롤에 대 한 형식화 된 참조를 통해 액세스할 수는 `Control` 속성입니다. 또한 통해 렌더링 되는 Xamarin.Forms 컨트롤에 대 한 참조를 가져올 수 있습니다는 `Element` 속성을 있지만 샘플 응용 프로그램에서 사용 되지 않습니다.

각 사용자 지정 렌더러 클래스도 데코레이팅되 어는 `ExportRenderer` xamarin.forms 렌더러를 등록 하는 특성입니다. 속성에는 두 개의 매개 변수-렌더링 되 고 Xamarin.Forms 컨트롤의 형식 이름 및 사용자 지정 렌더러의 유형 이름을 사용 합니다. `assembly` 특성에 대 한 접두사 특성이 전체 어셈블리에 적용 되도록 지정 합니다.

다음 섹션에서는 각 플랫폼 특정 구현에 설명 `MyEntryRenderer` 사용자 지정 렌더러 클래스입니다.

### <a name="creating-the-custom-renderer-on-ios"></a>Ios 사용자 지정 렌더러 만들기

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

기본 클래스에 대 한 호출 `OnElementChanged` 메서드를 만드는 데 iOS `UITextField` 렌더러의에 할당 되는 컨트롤에 대 한 참조 컨트롤 `Control` 속성입니다. 명령 프롬프트 창의 배경색으로 밝은 자주색으로 설정 됩니다는 `UIColor.FromRGB` 메서드.

### <a name="creating-the-custom-renderer-on-android"></a>Android 사용자 지정 렌더러 만들기

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

기본 클래스에 대 한 호출 `OnElementChanged` 메서드를 만드는 데는 Android `EditText` 렌더러의에 할당 되는 컨트롤에 대 한 참조 컨트롤 `Control` 속성입니다. 명령 프롬프트 창의 배경색을 연한 녹색으로 설정 됩니다는 `Control.SetBackgroundColor` 메서드.

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP에 사용자 지정 렌더러 만들기

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

기본 클래스에 대 한 호출 `OnElementChanged` 메서드를 만드는 데는 `TextBox` 렌더러의에 할당 되는 컨트롤에 대 한 참조 컨트롤 `Control` 속성입니다. 명령 프롬프트 창의 배경색 설정 됩니다 녹청 만들어는 `SolidColorBrush` 인스턴스.

## <a name="summary"></a>요약

이 문서는 Xamarin.Forms에 대 한 사용자 지정 컨트롤 렌더러를 만드는 방법을 보여 주었습니다 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 컨트롤 개발자가 자신의 플랫폼 특정 렌더링 기본 네이티브 렌더링을 재정의할 수 있도록 합니다. 사용자 지정 렌더러 Xamarin.Forms 컨트롤의 모양 사용자 지정 하는 강력한 접근 방법을 제공 합니다. 작은 스타일 변경 내용 또는 정교한 플랫폼 특정 레이아웃 및 동작 사용자 지정에 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [CustomRendererEntry (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/entry/)
