---
title: 이미지Xamarin.Forms
description: 이미지는를 사용 하 여 플랫폼 간에 공유 될 수 있으며 Xamarin.Forms , 각 플랫폼에 대해 특별히 로드 하거나 표시를 위해 다운로드할 수 있습니다.
ms.prod: xamarin
ms.assetid: C025AB53-05CC-49BA-9815-75D6DF9E40B7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/19/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7ae6e5e764dc066940971dd9b5a8fdc36c7a1970
ms.sourcegitcommit: cd0c0999b53e825b60471bfbfd4144cfcd783587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86225496"
---
# <a name="images-in-xamarinforms"></a>이미지Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages)

_이미지는를 사용 하 여 플랫폼 간에 공유 될 수 있으며 Xamarin.Forms , 각 플랫폼에 대해 특별히 로드 하거나 표시를 위해 다운로드할 수 있습니다._

이미지는 응용 프로그램 탐색, 유용성 및 브랜딩의 중요 한 부분입니다. Xamarin.Forms응용 프로그램은 모든 플랫폼에서 이미지를 공유할 수 있어야 하 고 각 플랫폼 마다 다른 이미지를 표시할 수도 있습니다.

플랫폼 관련 이미지는 아이콘 및 시작 화면에도 필요 합니다. 이러한 기능은 플랫폼별로 구성 해야 합니다.

## <a name="display-images"></a>이미지 표시

Xamarin.Forms뷰를 사용 하 여 [`Image`](xref:Xamarin.Forms.Image) 페이지에 이미지를 표시 합니다. 여기에는 몇 가지 중요 한 속성이 있습니다.

- [`Source`](xref:Xamarin.Forms.Image.Source)- [`ImageSource`](xref:Xamarin.Forms.ImageSource) 표시할 이미지를 설정 하는 파일, Uri 또는 리소스 중 하나입니다.
- [`Aspect`](xref:Xamarin.Forms.Image.Aspect)-이미지가 표시 되는 범위 내에서 이미지의 크기를 조정 하는 방법 (스트레치, 자르기 또는 레터 박스).

[`ImageSource`](xref:Xamarin.Forms.ImageSource)각 이미지 원본 형식에 대해 정적 메서드를 사용 하 여 인스턴스를 가져올 수 있습니다.

- [`FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String))-각 플랫폼에서 확인할 수 있는 파일 이름 또는 파일 경로가 필요 합니다.
- [`FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri))-Uri 개체가 필요 합니다 (예:).  `new Uri("http://server.com/image.jpg")` .
- [`FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*)- **빌드 작업 (EmbeddedResource**)을 사용 하 여 응용 프로그램 또는 .NET Standard 라이브러리 프로젝트에 포함 된 이미지 파일에 대 한 리소스 식별자가 필요 합니다.
- [`FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream}))-이미지 데이터를 제공 하는 스트림이 필요 합니다.

[`Aspect`](xref:Xamarin.Forms.Image.Aspect)속성은 이미지가 표시 영역에 맞게 조정 되는 방법을 결정 합니다.

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill)-표시 영역을 완전히 그리고 정확 하 게 채우도록 이미지를 확장 합니다. 이로 인해 이미지가 왜곡 될 수 있습니다.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill)-화면을 유지 하면서 화면에 표시 영역을 채우도록 이미지를 클리핑 합니다 (즉, 왜곡이 발생 하지 않음).
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit)-이미지 (필요한 경우)를 Letterboxes 하 여 전체 이미지가 표시 영역에 들어가도록 이미지의 너비 또는 높이에 따라 위쪽/아래쪽 또는 옆쪽에 공백이 추가 됩니다.

이미지는 [로컬 파일](#local-images)에서 로드 하거나, [포함 된 리소스](#embedded-images)에서 로드 하거나, [다운로드](#download-images)하거나, 스트림에서 로드할 수 있습니다. 또한 [`Image`](xref:Xamarin.Forms.Image) 개체에 글꼴 아이콘 데이터를 지정 하 여 보기에서 글꼴 아이콘을 표시할 수 있습니다 `FontImageSource` . 자세한 내용은 글꼴 가이드에서 [글꼴 아이콘 표시](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons) [를 참조](~/xamarin-forms/user-interface/text/fonts.md) 하세요.

## <a name="local-images"></a>로컬 이미지

이미지 파일은 각 응용 프로그램 프로젝트에 추가 되 고 공유 코드에서 참조 될 수 있습니다 Xamarin.Forms . 이미지를 배포하는 이 메서드는 다양한 플랫폼 또는 약간 다른 디자인의 다양한 해결 방법을 사용하는 경우와 같이 이미지가 특정 플랫폼을 지정하는 경우에 필요합니다.

모든 앱에서 단일 이미지를 사용 하려면 *동일한 파일 이름이 모든 플랫폼에서 사용*되어야 하며 유효한 Android 리소스 이름 이어야 합니다 (예: 소문자, 숫자, 밑줄 및 마침표만 사용할 수 있음).

- **ios** -ios 9 이후 이미지를 관리 하 고 지 원하는 기본 방법은 응용 프로그램의 다양 한 장치 및 크기 조정 요소를 지 원하는 데 필요한 모든 이미지 버전을 포함 해야 하는 **Asset Catalog 이미지 집합**을 사용 하는 것입니다. 자세한 내용은 [자산 카탈로그 이미지 집합에 이미지 추가](~/ios/app-fundamentals/images-icons/displaying-an-image.md)를 참조 하세요.
- **Android** - **빌드 작업: AndroidResource**를 사용 하 여 **리소스/그릴** 때 디렉터리에 이미지를 추가 합니다. 이미지의 높은 및 낮은 DPI 버전 ( **ldpi**, 그릴 수 있는- **hdpi**및 그릴 수 있는 **-xhdpi**와 같이 적절 하 게 명명 된 **리소스** 하위 디렉터리)을 제공할 수도 있습니다.
- **유니버설 Windows 플랫폼 (UWP)** -기본적으로 이미지는 **빌드 작업: 콘텐츠**를 사용 하 여 응용 프로그램의 루트 디렉터리에 배치 되어야 합니다. 또는 이미지를 다른 디렉터리에 배치 하 여 플랫폼별로 지정할 수도 있습니다. 자세한 내용은 [Windows의 기본 이미지 디렉터리](~/xamarin-forms/platform/windows/default-image-directory.md)를 참조 하십시오.

> [!IMPORTANT]
> IOS 9 이전에 이미지는 일반적으로 **BundleResource 빌드 작업**을 사용 하 여 **Resources** 폴더에 배치 되었습니다. 그러나 iOS 앱에서 이미지를 사용 하는이 방법은 Apple에서 더 이상 사용 되지 않습니다. 자세한 내용은 [이미지 크기 및 파일 이름](~/ios/app-fundamentals/images-icons/displaying-an-image.md)을 참조 하세요.

이러한 파일 이름 지정 및 배치 규칙을 준수 하면 다음 XAML이 모든 플랫폼에서 이미지를 로드 하 고 표시할 수 있습니다.

```xaml
<Image Source="waterfront.jpg" />
```

해당 c # 코드는 다음과 같습니다.

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

다음 스크린샷에는 각 플랫폼에서 로컬 이미지를 표시 한 결과가 나와 있습니다.

[![로컬 이미지를 표시 하는 샘플 응용 프로그램](images-images/local-sml.png)](images-images/local.png#lightbox)

`Device.RuntimePlatform`이 코드 예제에 표시 된 것 처럼 더 많은 유연성을 위해 속성을 사용 하 여 일부 또는 모든 플랫폼에 대해 다른 이미지 파일 또는 경로를 선택할 수 있습니다.

```csharp
image.Source = Device.RuntimePlatform == Device.Android
                ? ImageSource.FromFile("waterfront.jpg")
                : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> 모든 플랫폼에서 동일한 이미지 파일 이름을 사용 하려면 모든 플랫폼에서 이름이 유효 해야 합니다. Android drawables에는 이름 지정 제한이 있습니다. 소문자, 숫자, 밑줄 및 마침표만 사용할 수 있으며 플랫폼 간 호환성을 위해 다른 모든 플랫폼 에서도이를 준수 해야 합니다. 예제 파일 이름 **waterfront.png** 는 규칙을 따르며, 잘못 된 파일 이름에 대 한 예로는 "물 front.png", "WaterFront.png", "water-front.png" 및 "wåterfront.png"가 있습니다.

### <a name="native-resolutions-retina-and-high-dpi"></a>네이티브 해상도 (레 티 나 및 높은 DPI)

iOS, Android 및 UWP에는 다양 한 이미지 해상도에 대 한 지원이 포함 되어 있습니다. 여기서 운영 체제는 장치 기능에 따라 런타임에 적절 한 이미지를 선택 합니다. Xamarin.Forms는 네이티브 플랫폼의 Api를 사용 하 여 로컬 이미지를 로드 하므로 파일의 이름을 올바르게 지정 하 고 프로젝트에 배치 하는 경우 자동으로 대체 해상도를 지원 합니다.

IOS 9부터 이미지를 관리 하는 기본 방법은 적절 한 자산 카탈로그 이미지 집합에 필요한 각 해상도에 대해 이미지를 끄는 것입니다. 자세한 내용은 [자산 카탈로그 이미지 집합에 이미지 추가](~/ios/app-fundamentals/images-icons/displaying-an-image.md)를 참조 하세요.

IOS 9 이전에는 레 티 나 버전의 이미지를 **리소스** 폴더에 배치할 수 있습니다 **@2x** **@3x** . 즉, 파일 확장명 앞에 파일 이름 (예: **myimage@2x.png**). 그러나 iOS 앱에서 이미지를 사용 하는이 방법은 Apple에서 더 이상 사용 되지 않습니다. 자세한 내용은 [이미지 크기 및 파일 이름](~/ios/app-fundamentals/images-icons/displaying-an-image.md)을 참조 하세요.

Android 대체 해상도 이미지는 다음 스크린샷에 표시 된 것 처럼 Android 프로젝트의 [특수 하 게 명명 된 디렉터리](https://developer.android.com/guide/practices/screens_support.html) 에 배치 해야 합니다.

[![Android 다중 해상도 이미지 위치](images-images/xs-highdpisolution-sml.png)](images-images/xs-highdpisolution.png#lightbox)

UWP 이미지 파일 이름은 [ `.scale-xxx` 파일 확장명 앞에 접미사를 추가할 수 있습니다](https://docs.microsoft.com/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast) `xxx` . 여기서은 자산에 적용 되는 크기 조정의 백분율입니다 (예: **myimage.scale-200.png**). 그런 다음 크기 조정 한정자 없이 코드 또는 XAML에서 이미지를 참조할 수 있습니다 (예: **myimage.png**). 이 플랫폼은 디스플레이의 현재 DPI를 기준으로 가장 가까운 적절 한 자산 크기를 선택 합니다.

### <a name="additional-controls-that-display-images"></a>이미지를 표시 하는 추가 컨트롤

일부 컨트롤에는 다음과 같은 이미지를 표시 하는 속성이 있습니다.

- [`Button`](xref:Xamarin.Forms.Button)에 [`ImageSource`](xref:Xamarin.Forms.Button.ImageSource) 는에 표시할 비트맵 이미지로 설정할 수 있는 속성이 있습니다 `Button` . 자세한 내용은 [단추를 사용 하 여 비트맵 사용](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)을 참조 하세요.
- [`ImageButton`](xref:Xamarin.Forms.Button)에 [`Source`](xref:Xamarin.Forms.ImageButton.Source) 는에 표시할 이미지에 설정할 수 있는 속성이 있습니다 `ImageButton` . 자세한 내용은 [이미지 원본 설정](~/xamarin-forms/user-interface/imagebutton.md#setting-the-image-source)을 참조 하세요.
- [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)에는 [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource) 파일, 포함 리소스, URI 또는 스트림에서 로드 된 이미지로 설정할 수 있는 속성이 있습니다.
- [`ImageCell`](xref:Xamarin.Forms.ImageCell)에는 [`ImageSource`](xref:Xamarin.Forms.ImageCell.ImageSource) 파일, 포함 리소스, URI 또는 스트림에서 검색 된 이미지로 설정할 수 있는 속성이 있습니다.
- [`Page`](xref:Xamarin.Forms.Page). 에서 파생 되는 모든 페이지 형식 `Page` 에는 [`IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource) [`BackgroundImageSource`](xref:Xamarin.Forms.Page.BackgroundImageSource) 파일, 포함 리소스, URI 또는 스트림이 할당 될 수 있는 및 속성이 있습니다. 가를 표시 하는 경우와 같은 특정 상황에서는 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) [`ContentPage`](xref:Xamarin.Forms.ContentPage) 플랫폼이 지 원하는 경우 아이콘이 표시 됩니다.

  > [!IMPORTANT]
  > IOS에서는 [`Page.IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource) 자산 카탈로그 이미지 집합의 이미지에서 속성을 채울 수 없습니다. 대신 `Page.IconImageSource` 파일, 포함 리소스, URI 또는 스트림에서 속성의 아이콘 이미지를 로드 합니다.

## <a name="embedded-images"></a>포함 이미지

포함 된 이미지는 응용 프로그램과 함께 제공 됩니다 (예: 로컬 이미지). 하지만 이미지 복사본을 각 응용 프로그램의 파일 구조에 포함 하는 대신 이미지 파일이 리소스로 어셈블리에 포함 됩니다. 이미지를 배포 하는이 방법은 각 플랫폼에서 동일한 이미지를 사용 하는 경우에 권장 되며, 이미지를 코드와 함께 제공 하므로 구성 요소를 만드는 데 특히 적합 합니다.

프로젝트에 이미지를 포함 하려면 마우스 오른쪽 단추를 클릭 하 여 새 항목을 추가 하 고 추가할 이미지/s를 선택 합니다. 기본적으로 이미지에는 **빌드 작업 (None**)이 포함 됩니다. **빌드 작업: EmbeddedResource**로 설정 해야 합니다.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![빌드 작업을 포함 리소스에 설정](images-images/vs-buildaction-sml.png)](images-images/vs-buildaction.png#lightbox)

파일에 대 한 **속성** 창에서 **빌드 작업** 을 보고 변경할 수 있습니다.

이 예제에서는 리소스 ID를 **WorkingWithImages.beach.jpg**합니다.
IDE는 각 값 사이에 마침표 (.)를 사용 하 여이 프로젝트의 **기본 네임 스페이스** 를 파일 이름과 연결 하 여이 기본값을 생성 했습니다.
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

![](images-images/xs-buildaction.png "Set Build Action: EmbeddedResource")

파일에 대 한 **속성** 패드에서 **빌드 작업** 을 보고 변경할 수도 있습니다.
이 패드는 코드에서 리소스를 참조 하는 데 사용 되는 **리소스 ID** 를 표시 합니다. 아래 스크린샷에서 **리소스 ID** 가 **WorkingWithImages.beach.jpg**됩니다.
IDE는 각 값 사이에 마침표 (.)를 사용 하 여이 프로젝트의 **기본 네임 스페이스** 를 파일 이름과 연결 하 여이 기본값을 생성 했습니다.
이 ID는 **속성** 패드에서 편집할 수 있지만 이러한 예제에서는 **WorkingWithImages.beach.jpg** 값이 사용 됩니다.

[![포함 리소스 속성 패드](images-images/xs-embeddedproperties-sml.png)](images-images/xs-embeddedproperties.png#lightbox)

-----

포함 된 이미지를 프로젝트 내에 있는 폴더에 저장 하면 리소스 ID에서 폴더 이름도 마침표 (.)로 구분 됩니다. **beach.jpg** 이미지를 **myimages** 라는 폴더로 이동 하면의 리소스 ID가 **WorkingWithImages.MyImages.beach.jpg**

포함 이미지를 로드 하는 코드는 아래와 같이 단순히 **리소스 ID** 를 메서드에 전달 합니다 [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) .

```csharp
Image embeddedImage = new Image
{
    Source = ImageSource.FromResource("WorkingWithImages.beach.jpg", typeof(MyClass).GetTypeInfo().Assembly)
};
```

> [!NOTE]
> 유니버설 Windows 플랫폼에 릴리스 모드에서 포함 이미지를 표시할 수 있도록 지원 하려면 `ImageSource.FromResource` 이미지를 검색할 소스 어셈블리를 지정 하는의 오버 로드를 사용 해야 합니다.

현재 리소스 식별자에 대 한 암시적 변환은 없습니다. 대신 [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) 또는를 사용 하 여 `new ResourceImageSource()` 포함 이미지를 로드 해야 합니다.

다음 스크린샷에는 각 플랫폼에 포함 이미지를 표시 한 결과가 나와 있습니다.

[![포함 된 이미지를 표시 하는 샘플 응용 프로그램](images-images/resource-sml.png)](images-images/resource.png#lightbox)

### <a name="xaml"></a>XAML

에서로의 기본 제공 형식 변환기가 없으므로 `string` `ResourceImageSource` 이러한 형식의 이미지를 XAML에서 고유 하 게 로드할 수 없습니다. 대신 XAML로 지정 된 **리소스 ID** 를 사용 하 여 이미지를 로드 하도록 간단한 사용자 지정 xaml 태그 확장을 작성할 수 있습니다.

```csharp
[ContentProperty (nameof(Source))]
public class ImageResourceExtension : IMarkupExtension
{
 public string Source { get; set; }

 public object ProvideValue (IServiceProvider serviceProvider)
 {
   if (Source == null)
   {
     return null;
   }

   // Do your translation lookup here, using whatever method you require
   var imageSource = ImageSource.FromResource(Source, typeof(ImageResourceExtension).GetTypeInfo().Assembly);

   return imageSource;
 }
}
```

> [!NOTE]
> 유니버설 Windows 플랫폼에 릴리스 모드에서 포함 이미지를 표시할 수 있도록 지원 하려면 `ImageSource.FromResource` 이미지를 검색할 소스 어셈블리를 지정 하는의 오버 로드를 사용 해야 합니다.

이 확장을 사용 하려면 `xmlns` 프로젝트의 올바른 네임 스페이스 및 어셈블리 값을 사용 하 여 사용자 지정를 XAML에 추가 합니다. 그런 다음이 구문을 사용 하 여 이미지 소스를 설정할 수 있습니다 `{local:ImageResource WorkingWithImages.beach.jpg}` . 전체 XAML 예제는 다음과 같습니다.

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage
   xmlns="http://xamarin.com/schemas/2014/forms"
   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
   xmlns:local="clr-namespace:WorkingWithImages;assembly=WorkingWithImages"
   x:Class="WorkingWithImages.EmbeddedImagesXaml">
 <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
   <!-- use a custom Markup Extension -->
   <Image Source="{local:ImageResource WorkingWithImages.beach.jpg}" />
 </StackLayout>
</ContentPage>
```

### <a name="troubleshoot-embedded-images"></a>포함 이미지 문제 해결

#### <a name="debug-code"></a>코드 디버그

특정 이미지 리소스가 로드 되지 않는 이유를 이해 하기 어렵기 때문에, 리소스가 올바르게 구성 되었는지 확인 하는 데 도움이 되도록 다음 디버그 코드를 응용 프로그램에 일시적으로 추가할 수 있습니다. 그러면 지정 된 어셈블리에 포함 된 알려진 모든 리소스를 **콘솔** 에 출력 하 여 리소스 로드 문제를 디버그할 수 있습니다.

```csharp
using System.Reflection;
// ...
// NOTE: use for debugging, not in released app code!
var assembly = typeof(MyClass).GetTypeInfo().Assembly;
foreach (var res in assembly.GetManifestResourceNames())
{
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

#### <a name="images-embedded-in-other-projects"></a>다른 프로젝트에 포함 된 이미지

기본적으로 메서드는 `ImageSource.FromResource` 메서드를 호출 하는 코드와 동일한 어셈블리에서 이미지를 찾습니다 `ImageSource.FromResource` . 위의 디버그 코드를 사용 하 여 `typeof()` 문을 `Type` 각 어셈블리에 있는 알려진으로 변경 하 여 특정 리소스를 포함 하는 어셈블리를 확인할 수 있습니다.

그러나 포함 이미지를 검색 하는 소스 어셈블리를 메서드에 대 한 인수로 지정할 수 있습니다 `ImageSource.FromResource` .

```csharp
var imageSource = ImageSource.FromResource("filename.png",
            typeof(MyClass).GetTypeInfo().Assembly);
```

## <a name="download-images"></a>다운로드 이미지

다음 XAML과 같이 이미지를 표시 하기 위해 자동으로 다운로드할 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
       x:Class="WorkingWithImages.DownloadImagesXaml">
  <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
    <Label Text="Image UriSource Xaml" />
    <Image Source="https://aka.ms/campus.jpg" />
    <Label Text="campus.jpg gets downloaded from microsoft.com" />
  </StackLayout>
</ContentPage>
```

해당 c # 코드는 다음과 같습니다.

```csharp
var webImage = new Image {
     Source = ImageSource.FromUri(
        new Uri("https://aka.ms/campus.jpg")
     ) };
```

[`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri))메서드는 `Uri` 개체를 필요로 하며에서 읽는 새을 반환 합니다 [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) `Uri` .

URI 문자열에 대 한 암시적 변환도 있으므로 다음 예제도 작동 합니다.

```csharp
webImage.Source = "https://aka.ms/campus.jpg";
```

다음 스크린샷에는 각 플랫폼에 원격 이미지를 표시 한 결과가 나와 있습니다.

[![다운로드 한 이미지를 표시 하는 샘플 응용 프로그램](images-images/download-sml.png)](images-images/download.png#lightbox)

### <a name="downloaded-image-caching"></a>다운로드 한 이미지 캐싱

는 [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) 다음 속성을 통해 구성 된 다운로드 된 이미지의 캐싱도 지원 합니다.

- [`CachingEnabled`](xref:Xamarin.Forms.UriImageSource.CachingEnabled)-캐싱이 기본적으로 사용 되는지 여부 `true` 입니다.
- [`CacheValidity`](xref:Xamarin.Forms.UriImageSource.CacheValidity)- `TimeSpan` 이미지가 로컬로 저장 되는 기간을 정의 하는입니다.

캐싱은 기본적으로 사용 하도록 설정 되며 24 시간 동안 로컬로 이미지를 저장 합니다. 특정 이미지에 대 한 캐싱을 사용 하지 않도록 설정 하려면 다음과 같이 이미지 원본을 인스턴스화합니다.

```csharp
image.Source = new UriImageSource { CachingEnabled = false, Uri = new Uri("https://server.com/image") };
```

특정 캐시 기간을 설정 하려면 (예: 5 일) 다음과 같이 이미지 원본을 인스턴스화합니다.

```csharp
webImage.Source = new UriImageSource
{
    Uri = new Uri("https://aka.ms/campus.jpg"),
    CachingEnabled = true,
    CacheValidity = new TimeSpan(5,0,0,0)
};
```

기본 제공 캐싱을 사용 하면 각 셀에서 이미지를 설정 (또는 바인딩) 할 수 있는 이미지의 스크롤 목록과 같은 시나리오를 쉽게 지원할 수 있으며, 셀이 다시 보기로 스크롤될 때 기본 제공 캐시가 이미지를 다시 로드 하도록 할 수 있습니다.

## <a name="animated-gifs"></a>애니메이션 Gif

Xamarin.Forms에는 작은 애니메이션 Gif를 표시할 수 있는 기능이 포함 되어 있습니다. 이 작업을 수행 [`Image.Source`](xref:Xamarin.Forms.Image.Source) 하려면 속성을 애니메이션 GIF 파일로 설정 합니다.

```xaml
<Image Source="demo.gif" />
```

> [!IMPORTANT]
> 에서 애니메이션 GIF 지원은 Xamarin.Forms 파일을 다운로드할 수 있는 기능을 포함 하지만 애니메이션 gif의 캐싱 또는 스트리밍을 지원 하지 않습니다.

기본적으로 애니메이션 GIF가 로드 되 면 재생 되지 않습니다. 이는 `IsAnimationPlaying` 애니메이션 GIF의 재생 또는 중지 여부를 제어 하는 속성이의 기본값 이기 때문입니다 `false` . 형식의이 속성 `bool` 은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상이 될 수 있고 스타일을 지정할 수 있습니다.

따라서 애니메이션 GIF가 로드 될 때 속성이로 설정 될 때까지 재생 되지 않습니다 `IsAnimationPlaying` `true` . 그런 다음 속성을로 설정 하 여 재생을 중지할 수 있습니다 `IsAnimationPlaying` `false` . 이 속성은 비 GIF 이미지 원본을 표시할 때 영향을 주지 않습니다.

> [!NOTE]
> Android에서 애니메이션 GIF 지원은 응용 프로그램에서 빠른 렌더러를 사용 해야 하며, 레거시 렌더러를 사용 하도록 옵트인 한 경우 작동 하지 않습니다.
> UWP에서 애니메이션 GIF를 지원 하려면 최소 버전의 Windows 10 기념일 업데이트 (버전 1607)가 필요 합니다.

## <a name="icons-and-splash-screens"></a>아이콘 및 시작 화면

[`Image`](xref:Xamarin.Forms.Image)응용 프로그램 아이콘 및 시작 화면은 뷰와 관련이 없지만 프로젝트에서 이미지를 사용 하는 것이 중요 Xamarin.Forms 합니다.

앱에 대 한 아이콘 및 시작 화면 설정은 Xamarin.Forms 각 응용 프로그램 프로젝트에서 수행 됩니다. 즉, iOS, Android 및 UWP에 맞게 올바른 크기의 이미지를 생성 합니다. 이러한 이미지는 각 플랫폼 요구 사항에 따라 이름이 지정 되 고 배치 되어야 합니다.

## <a name="icons"></a>아이콘

이러한 응용 프로그램 리소스를 만드는 방법에 대 한 자세한 내용은 이미지, [Google Iconography](https://developer.android.com/design/style/iconography.html)및 [UWP 지침에서 타일 및 아이콘 자산에 대 한](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/) [작업](~/ios/app-fundamentals/images-icons/index.md)을 참조 하세요.

또한 [`Image`](xref:Xamarin.Forms.Image) 개체에 글꼴 아이콘 데이터를 지정 하 여 보기에서 글꼴 아이콘을 표시할 수 있습니다 `FontImageSource` . 자세한 내용은 글꼴 가이드에서 [글꼴 아이콘 표시](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons) [를 참조](~/xamarin-forms/user-interface/text/fonts.md) 하세요.

## <a name="splash-screens"></a>시작 화면

IOS 및 UWP 응용 프로그램에만 시작 화면 (시작 화면 또는 기본 이미지 라고도 함)이 필요 합니다.

Windows 개발자 센터에서 이미지 및 [시작 화면](/windows/uwp/launch-resume/splash-screens/) [을 사용](~/ios/app-fundamentals/images-icons/index.md) 하는 iOS에 대 한 설명서를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [WorkingWithImages (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages)
- [iOS 이미지 작업](~/ios/app-fundamentals/images-icons/index.md)
- [Android Iconography](https://developer.android.com/design/style/iconography.html)
- [타일 및 아이콘 자산에 대한 지침](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)
