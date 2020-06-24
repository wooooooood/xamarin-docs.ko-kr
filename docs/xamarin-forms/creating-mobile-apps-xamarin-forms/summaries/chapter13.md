---
title: 13장의 요약 정보입니다. 비트맵
description: 'Xamarin.Forms로 모바일 앱 만들기: 13장의 요약 정보입니다. 비트맵'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 43caf088ad6cb816f049e7862a287c17839c2170
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136775"
---
# <a name="summary-of-chapter-13-bitmaps"></a>13장의 요약 정보입니다. 비트맵

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)

> [!NOTE]
> 이 페이지의 정보는 Xamarin.Forms가 책에 제공된 자료와는 다르게 사용되는 경우를 설명합니다.

Xamarin.Forms [`Image`](xref:Xamarin.Forms.Image) 요소는 비트맵을 표시합니다. 모든 Xamarin.Forms 플랫폼은 JPEG, PNG, GIF 및 BMP 파일 형식을 지원합니다.

Xamarin.Forms의 비트맵은 다음 네 위치에서 제공됩니다.

- URL에 지정된 웹
- 공유 라이브러리에 포함된 리소스
- 플랫폼 애플리케이션 프로젝트에 포함된 리소스
- `MemoryStream`을 포함하여 .NET `Stream` 개체가 참조할 수 있는 모든 위치

공유 라이브러리의 비트맵 리소스는 플랫폼에 독립적이지만, 플랫폼 프로젝트의 비트맵 리소스는 플랫폼에 종속적입니다.

> [!NOTE]
> 이 책의 텍스트는 이식 가능한 클래스 라이브러리에 대한 참조를 만듭니다. 이 라이브러리는 .NET Standard 라이브러리로 대체되었습니다. 이 책의 모든 샘플 코드는 .NET Standard 라이브러리를 사용하도록 변환되었습니다.

비트맵은 `Image`의 [`Source`](xref:Xamarin.Forms.Image.Source) 속성을 다음 세 가지 파생 항목을 포함하는 추상 클래스인 [`ImageSource`](xref:Xamarin.Forms.ImageSource) 형식의 개체로 설정합니다.

- [`Uri`](xref:Xamarin.Forms.UriImageSource.Uri) 속성으로 설정된 `Uri` 개체를 기반으로 웹을 통해 비트맵에 액세스하기 위한 [`UriImageSource`](xref:Xamarin.Forms.UriImageSource)
- [`File`](xref:Xamarin.Forms.FileImageSource.File) 속성으로 설정된 폴더 및 파일 경로를 기반으로 플랫폼 애플리케이션 프로젝트에 저장된 비트맵에 액세스하기 위한 [`FileImageSource`](xref:Xamarin.Forms.FileImageSource)
- [`Stream`](xref:Xamarin.Forms.StreamImageSource.Stream) 속성으로 설정된 `Func`에서 `Stream`을 반환하여 지정된 .NET `Stream` 개체를 사용하여 비트맵을 로드하기 위한 [`StreamImageSource`](xref:Xamarin.Forms.StreamImageSource)

또는 (보다 일반적으로) `ImageSource` 클래스의 다음과 같은 정적 메서드를 사용할 수 있으며, 이 모든 메서드는 `ImageSource` 개체를 반환합니다.

- `Uri` 개체를 기반으로 웹을 통해 비트맵에 액세스하기 위한 [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri))
- 애플리케이션 PCL에 포함 리소스로 저장된 비트맵에 액세스하기 위한 [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*). 다른 소스 어셈블리의 비트맵에 액세스하기 위한 [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Type)) 또는 [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Reflection.Assembly))
- 플랫폼 애플리케이션 프로젝트에서 비트맵에 액세스하기 위한 [`ImageSource.FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String))
- `Stream` 개체를 기반으로 비트맵을 로드하기 위한 [`ImageSource.FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream}))

`Image.FromResource` 메서드에 해당하는 클래스는 없습니다. `UriImageSource` 클래스는 캐싱을 제어해야 하는 경우에 유용합니다. `FileImageSource` 클래스는 XAML에서 유용합니다. `StreamImageSource`는 `Stream` 개체의 비동기식 로드에 유용하고, `ImageSource.FromStream`은 동기식 로드에 유용합니다.

## <a name="platform-independent-bitmaps"></a>플랫폼 독립적인 비트맵

[**WebBitmapCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode) 프로젝트는 `ImageSource.FromUri`를 사용하여 웹을 통해 비트맵을 로드합니다. `Image` 요소는 `ContentPage`의 `Content` 속성으로 설정되므로 페이지 크기로 제한됩니다. 비트맵의 크기에 관계 없이, 제한된 `Image` 요소는 해당 컨테이너 크기로 확장되고, 비트맵은 가로 세로 비율을 유지하면서 `Image` 요소 내의 최대 크기로 표시됩니다. 비트맵을 벗어나는 `Image` 영역은 [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)로 색을 지정할 수 있습니다.

[**WebBitmapXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml) 샘플은 이와 유사하지만 단순히 `Source` 속성을 URL로 설정합니다. 변환은 [`ImageSourceConverter`](xref:Xamarin.Forms.ImageSourceConverter) 클래스에서 처리합니다.

### <a name="fit-and-fill"></a>맞춤 및 채우기

`Image`의 [`Aspect`](xref:Xamarin.Forms.Image.Aspect) 속성을 [`Aspect`](xref:Xamarin.Forms.Aspect) 열거형의 다음 멤버 중 하나로 설정하여 비트맵을 확장하는 방법을 제어할 수 있습니다.

- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit): 가로 세로 비율(기본값) 고려
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill): 영역 채우기, 가로 세로 비율을 고려하지 않음
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill): 가로 세로 비율을 고려하여 영역 채우기, 비트맵의 일부를 자르는 방식으로 수행

### <a name="embedded-resources"></a>포함 리소스

PCL 또는 PCL의 폴더에 비트맵 파일을 추가할 수 있습니다. **EmbeddedResource**의 **빌드 작업**을 제공합니다. [**ResourceBitmapCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode) 샘플은 `ImageSource.FromResource`를 사용하여 파일을 로드하는 방법을 보여줍니다. 메서드에 전달되는 리소스 이름은 어셈블리 이름, 점, 선택적 폴더 이름과 점, 파일 이름 순서로 구성됩니다.

프로그램에서는 `Image`의 `VerticalOptions` 및 `HorizontalOptions` 속성을 `LayoutOptions.Center`로 설정하므로 `Image` 요소가 제한되지 않습니다. 다음과 같이 `Image`와 비트맵의 크기는 동일합니다.

- iOS 및 Android에서 `Image`는 비트맵의 픽셀 크기입니다. 비트맵 픽셀과 화면 픽셀이 일대일로 매핑됩니다.
- 유니버설 Windows 플랫폼에서 `Image`는 디바이스 독립적 단위로 비트맵의 픽셀 크기입니다. 대부분의 디바이스에서 각 비트맵 픽셀은 여러 화면 픽셀을 점유합니다.

[**StackedBitmap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) 샘플은 `Image`를 XAML의 세로 `StackLayout`에 배치합니다. [`ImageResourceExtension`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs)이라는 태그 확장은 XAML에서 포함 리소스를 참조하는 데 도움이 됩니다. 이 클래스는 현재 위치의 어셈블리에서만 리소스를 로드하므로 라이브러리에 배치할 수 없습니다.

### <a name="more-on-sizing"></a>크기 조정에 대한 추가 정보

모든 플랫폼에서 일관적으로 비트맵 크기를 조정하는 것이 바람직한 경우가 자주 있습니다.
**StackedBitmap**을 실험해 보면 `Image` 요소에 대한 `WidthRequest`를 세로 `StackLayout`으로 설정하여 플랫폼 간에 일관적인 크기를 유지할 수 있지만, 이 기술을 사용하여 크기를 줄이는 것만 가능합니다.

또한 모든 플랫폼에서 이미지 크기를 일정하게 유지하기 위해 `HeightRequest`를 설정할 수 있지만, 비트맵의 너비가 제한되면 이 기술의 다양성도 제한됩니다. 세로 `StackLayout`의 이미지에는 `HeightRequest`를 사용하면 안 됩니다.

가장 좋은 방법은 디바이스 독립적 단위로 휴대폰 너비보다 넓은 비트맵으로 시작하고, `WidthRequest`를 디바이스 독립적 단위로 원하는 너비로 설정하는 것입니다. 이 방법은 [**DeviceIndBitmapSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize) 샘플에서 보여줍니다.

[**MadTeaParty**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty)는 루이스 캐럴의 *이상한 나라의 앨리스* 7장을 존 테니얼의 원작 일러스트레이션과 함께 표시합니다.

[![미친 티 파티의 세 가지 스크린샷](images/ch13fg16-small.png "Mad Hatters Tea Party 서적 텍스트")](images/ch13fg16-large.png#lightbox "Mad Hatters Tea Party 서적 텍스트")

### <a name="browsing-and-waiting"></a>찾아보기 및 대기

[**ImageBrowser**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser) 샘플은 사용자가 Xamarin 웹 사이트에 저장된 주식 이미지를 탐색할 수 있습니다. 이 샘플은 .NET [`WebRequest`](xref:System.Net.WebRequest) 클래스를 사용하여 비트맵 목록과 함께 JSON 파일을 다운로드합니다.

> [!NOTE]
> Xamarin.Forms 프로그램은 인터넷을 통해 파일에 액세스하기 위해 [`WebRequest`](xref:System.Net.WebRequest)가 아닌 [`HttpClient`](xref:System.Net.Http.HttpClient)를 사용해야 합니다.

프로그램에서는 [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)를 사용하여 무언가가 발생하고 있다는 것을 표시합니다. 각 비트맵이 로드될 때 `Image`의 읽기 전용 [`IsLoading`](xref:Xamarin.Forms.Image.IsLoading) 속성은 `true`입니다. `IsLoading` 속성은 바인딩 가능한 속성에 의해 지원되므로, 해당 속성이 변경될 때 `PropertyChanged` 이벤트가 발생합니다. 프로그램에서는 이 이벤트에 처리기를 연결하고, `IsLoaded`의 현재 설정을 사용하여 `ActivityIndicator`의 [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning) 속성을 설정합니다.

## <a name="streaming-bitmaps"></a>비트맵 스트리밍

`ImageSource.FromStream` 메서드는 .NET `Stream`을 기반으로 `ImageSource`를 만듭니다. 이 메서드에는 `Stream` 개체를 반환하는 `Func` 개체가 전달되어야 합니다.

### <a name="accessing-the-streams"></a>스트림 액세스

[**BitmapStreams**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams) 샘플은 `ImaageSource.FromStream` 메서드를 사용하여 포함 리소스로 저장된 비트맵을 로드하는 방법과 웹 전체의 비트맵을 로드하는 방법을 보여줍니다.

### <a name="generating-bitmaps-at-run-time"></a>런타임에 비트맵 생성

모든 Xamarin.Forms 플랫폼은 코드에서 생성한 후 `MemoryStream`에 저장하기 쉬운 압축되지 않은 BMP 파일 형식을 지원합니다. 이 기술을 사용하면 **Xamrin.FormsBook.Toolkit** 라이브러리의 [`BmpMaker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) 클래스에 구현된 것처럼 런타임에 비트맵을 알고리즘 방식으로 만들 수 있습니다.

"직접 수행" [**DiyGradientBitmap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) 샘플은 `BmpMaker`를 사용하여 그라데이션 이미지가 있는 비트맵을 만드는 방법을 보여줍니다.

## <a name="platform-specific-bitmaps"></a>플랫폼별 비트맵

모든 Xamarin.Forms 플랫폼에서는 플랫폼 애플리케이션 어셈블리에 비트맵을 저장할 수 있습니다. Xamarin.Forms 애플리케이션에서 검색되는 이러한 플랫폼 비트맵은 [`FileImageSource`](xref:Xamarin.Forms.FileImageSource) 형식입니다. 용도는 다음과 같습니다.

- [`MenuItem`](xref:Xamarin.Forms.MenuItem)의 [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) 속성
- [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)의 [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) 속성
- `Button`의 [`Image`](xref:Xamarin.Forms.Button) 속성

플랫폼 어셈블리에는 아이콘 및 시작 화면의 비트맵이 이미 포함되어 있습니다.

- iOS 프로젝트의 **Resources** 폴더
- Android 프로젝트에서 **Resources** 폴더의 하위 폴더
- Windows 프로젝트의 **Assets** 폴더(Windows 플랫폼에서 비트맵을 해당 폴더로 제한하는 것은 아님)

[**PlatformBitmaps**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps) 샘플은 코드를 사용하여 플랫폼 애플리케이션 프로젝트의 아이콘을 표시합니다.

### <a name="bitmap-resolutions"></a>비트맵 해상도

모든 플랫폼에서는 다양한 디바이스 해상도에 대한 여러 버전의 비트맵 이미지를 저장할 수 있습니다. 런타임에 디바이스의 화면 해상도에 따라 적절한 버전이 로드됩니다.

iOS에서 이러한 비트맵은 다음과 같이 파일 이름의 접미사로 구분됩니다.

- 160 DPI 디바이스는 접미사가 없음(디바이스 독립적 단위에 1픽셀)
- 320 DPI 디바이스는 '@2x' 접미사(DIU에 2픽셀)
- 480 DPI 디바이스는 '@3x' 접미사(DIU에 3픽셀)

1인치 크기의 정사각형으로 표시되도록 만들어진 비트맵은 다음 세 가지 버전으로 존재합니다.

- 160픽셀 정사각형의 MyImage.jpg
- 320픽셀 정사각형의 MyImage@2x.jpg
- 480픽셀 정사각형의 MyImage@3x.jpg

프로그램에서는 MyImage.jpg로 이 비트맵을 선호하지만, 런타임에 화면 해상도에 따라 적절한 버전이 검색됩니다. 제약이 없는 경우 비트맵은 항상 160DIU에서 렌더링됩니다.

Android의 경우 비트맵은 다음과 같은 **Resources** 폴더의 다양한 하위 폴더에 저장됩니다.

- 120 DPI 디바이스는 drawable-ldpi(낮은 DPI)(DIU에 0.75픽셀)
- 160 DPI 디바이스는 drawable-mdpi(보통)(DIU에 1픽셀)
- 240 DPI 디바이스는 drawable-hdpi(높음)(DIU에 1.5픽셀)
- 320 DPI 디바이스는 drawable-xhdpi(매우 높음)(DIU에 2픽셀)
- 480 DPI 디바이스는 drawable-xxhdpi(매우 매우 높음)(DIU에 3픽셀)
- 640 DPI 디바이스는 drawable-xxxhdpi(매우 매우 매우 높음)(DIU에 4픽셀)

1인치 크기의 정사각형으로 렌더링되는 비트맵의 경우 다양한 버전의 비트맵이 이름은 같지만 크기는 다르며, 다음 폴더에 저장됩니다.

- 120 픽셀 정사각형에서는 drawable-ldpi/MyImage.jpg
- 160 픽셀 정사각형에서는 drawable-mdpi/MyImage.jpg
- 240 픽셀 정사각형에서는 drawable-hdpi/MyImage.jpg
- 320 픽셀 정사각형에서는 drawable-xhdpi/MyImage.jpg
- 480 픽셀 정사각형에서는 drawable-xxhdpi/MyImage.jpg
- 640 픽셀 정사각형에서는 drawable-xxxhdpi/MyImage.jpg

비트맵은 항상 160DIU에서 렌더링됩니다. (표준 Xamarin.Forms 솔루션 템플릿은 hdpi, xhdpi 및 xxhdpi 폴더만 포함합니다.)

UWP 프로젝트는 DIU당 픽셀 단위의 배율 인수로 구성되는 비트맵 이름 지정 체계를 백분율로 지원하며, 다음은 그 예입니다.

- 320픽셀 정사각형의 MyImage.scale-200.jpg

일부 백분율만 유효합니다. 이 책의 샘플 프로그램에는 **scale-200** 접미사가 있는 이미지만 포함되어 있지만, 현재 Xamarin.Forms 솔루션 템플릿에는 **scale-100**, **scale-125**, **scale-150** 및 **scale-400**이 포함되어 있습니다.

플랫폼 프로젝트에 비트맵을 추가할 때 **빌드 작업**은 다음과 같아야 합니다.

- iOS: **BundleResource**
- Android: **AndroidResource**
- UWP: **콘텐츠**

[**ImageTap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap) 샘플은 `TapGestureRecognizer`가 `Image` 설치된 요소로 구성된 두 개의 단추와 비슷한 개체를 만듭니다. 개체는 1인치 크기의 정사각형이 되도록 의도되었습니다. `Image`의 `Source` 속성은 `OnPlatform` 및 `On` 개체를 사용하여 플랫폼에서 잠재적으로 다른 파일 이름을 참조하도록 설정됩니다. 비트맵 이미지에는 픽셀 크기를 나타내는 숫자가 포함되어 있으므로, 어떤 크기의 비트맵이 검색 및 렌더링되는지 확인할 수 있습니다.

### <a name="toolbars-and-their-icons"></a>도구 모음 및 아이콘

플랫폼별 비트맵의 주요 용도 중 하나는 Xamarin.Forms 도구 모음입니다. 이 도구 모음은 `Page`에서 정의한 [`ToolbarItems`](xref:Xamarin.Forms.Page.ToolbarItems) 컬렉션에 [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) 개체를 추가하여 생성됩니다. `ToobarItem`은 [`MenuItem`](xref:Xamarin.Forms.MenuItem)에서 파생되며 여기서 일부 속성을 상속합니다.

가장 중요한 `ToolbarItem` 속성은 다음과 같습니다.

- 플랫폼에 따라 표시 여부가 결정되는 텍스트의 [`Text`](xref:Xamarin.Forms.MenuItem.Text) 및 `Order`
- 플랫폼에 따라 표시 여부가 결정되는 이미지의 `FileImageSource` 형식 [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) 및 `Order`
- [`ToolbarItemOrder`](xref:Xamarin.Forms.ToolbarItemOrder) 형식의 [`Order`](xref:Xamarin.Forms.ToolbarItem.Order). 열거형이며 세 개의 멤버 [`Default`](xref:Xamarin.Forms.ToolbarItemOrder.Default), [`Primary`](xref:Xamarin.Forms.ToolbarItemOrder.Primary) 및 [`Secondary`](xref:Xamarin.Forms.ToolbarItemOrder.Secondary)를 포함하고 있습니다.

`Primary` 항목의 수는 3 또는 4로 제한되어야 합니다. 모든 항목의 `Text` 설정을 포함해야 합니다. 대부분의 플랫폼에서는 `Primary` 항목에만 `Icon`이 필요하지만, Windows 8.1은 모든 항목에 `Icon`이 필요합니다. 아이콘은 32DIU 사각형이어야 합니다. `FileImageSource` 형식은 플랫폼에 한정됨을 나타냅니다.

`ToolbarItem`을 누르면 `Button`처럼 [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked) 이벤트가 실행됩니다. `ToolbarItem`은 MVVM과 연결하는 데 자주 사용되는 [`Command`](xref:Xamarin.Forms.MenuItem.Command) 및 [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter) 속성도 지원합니다. ([18장, MVVM](chapter18.md)을 참조하세요.)

iOS와 Android는 도구 모음을 표시하는 페이지가 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)이거나 `NavigationPage`에서 탐색하는 페이지여야 합니다. [**ToolbarDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo) 프로그램은 `ContentPage` 인수를 사용하여 `App` 클래스의 `MainPage` 속성을 [`NavigationPage` constructor](xref:Xamarin.Forms.NavigationPage.%23ctor(Xamarin.Forms.Page))로 설정하며, 도구 모음의 생성 및 이벤트 처리기를 보여 줍니다.

### <a name="button-images"></a>단추 이미지

또한 플랫폼별 비트맵을 사용하여 `Button`의 [`Image`](xref:Xamarin.Forms.Button.Image) 속성을 [**ButtonImage**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage) 샘플에 설명된 대로 32DIU 정사각형의 비트맵으로 설정할 수 있습니다.

> [!NOTE]
> 단추에 이미지를 사용하는 기능이 향상되었습니다. [단추에 비트맵 사용](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)을 참조하세요.

## <a name="related-links"></a>관련 링크

- [챕터 13 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [13장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [이미지 작업](~/xamarin-forms/user-interface/images.md)
- [단추에 비트맵 사용](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)
