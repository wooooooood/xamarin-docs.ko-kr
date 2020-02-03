---
title: 요약 13 장입니다. 비트맵
description: 'Xamarin.Forms를 사용 하 여 모바일 앱 만들기: 13 장 요약 합니다. 비트맵'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: e4746ed94a008d382ce15bb9cd7c52365d9ba574
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725530"
---
# <a name="summary-of-chapter-13-bitmaps"></a>요약 13 장입니다. 비트맵

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)

> [!NOTE]
> 이 페이지에 대 한 참고 사항 Xamarin.Forms 책의 내용을에서 달라졌는지를 위치 하는 영역을 나타냅니다.

Xamarin.ios [`Image`](xref:Xamarin.Forms.Image) 요소는 비트맵을 표시 합니다. 모든 Xamarin.Forms 플랫폼 JPEG, PNG, GIF 및 BMP 파일 형식을 지원합니다.

Xamarin.Forms에서 비트맵 네 곳에서 제공합니다.

- URL에 지정 된 대로 웹을 통해
- 공유 라이브러리에 리소스로 포함
- 플랫폼 응용 프로그램 프로젝트에 리소스로 포함
- .NET `Stream` 개체에서 참조할 수 있는 모든 위치에서 (`MemoryStream` 포함)

공유 라이브러리에서 비트맵 리소스 플랫폼 독립적인은 플랫폼 프로젝트에서는 비트맵 리소스가 플랫폼 마다 다릅니다.

> [!NOTE]
> 책의 텍스트는.NET Standard 라이브러리 바뀌었습니다는 이식 가능한 클래스 라이브러리에 대 한 참조를 만듭니다. 이 책에서 모든 샘플 코드는 .NET 표준 라이브러리를 사용하도록 변경되었습니다.

비트맵은 `Image`의 [`Source`](xref:Xamarin.Forms.Image.Source) 속성을 [`ImageSource`](xref:Xamarin.Forms.ImageSource)형식의 개체로 설정 하 여 지정 됩니다 .이 클래스는 세 개의 파생물을 포함 하는 추상 클래스입니다.

- [`Uri`](xref:Xamarin.Forms.UriImageSource.Uri) 속성으로 설정 된 `Uri` 개체를 기반으로 하 여 웹을 통해 비트맵에 액세스 하기 위한 [`UriImageSource`](xref:Xamarin.Forms.UriImageSource)
- [`File`](xref:Xamarin.Forms.FileImageSource.File) 속성으로 설정 된 폴더 및 파일 경로를 기반으로 플랫폼 응용 프로그램 프로젝트에 저장 된 비트맵에 액세스 하기 위한 [`FileImageSource`](xref:Xamarin.Forms.FileImageSource)
- `Func` 집합의 [`Stream`](xref:Xamarin.Forms.StreamImageSource.Stream) 속성으로 `Stream`를 반환 하 여 지정 된 .net `Stream` 개체를 사용 하 여 비트맵을 로드 하는 [`StreamImageSource`](xref:Xamarin.Forms.StreamImageSource)

또는 보다 일반적으로는 `ImageSource` 클래스의 다음과 같은 정적 메서드를 사용할 수 있습니다 .이 메서드는 모두 `ImageSource` 개체를 반환 합니다.

- `Uri` 개체를 기반으로 하 여 웹을 통해 비트맵에 액세스 하기 위한 [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri))
- PCL 응용 프로그램에 포함 리소스로 저장 된 비트맵에 액세스 하기 위한 [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) 다른 소스 어셈블리의 비트맵에 액세스 하기 위한 [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Type)) 또는 [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Reflection.Assembly))
- 플랫폼 응용 프로그램 프로젝트에서 비트맵에 액세스 하기 위한 [`ImageSource.FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String))
- `Stream` 개체를 기준으로 비트맵을 로드 하기 위한 [`ImageSource.FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream}))

`Image.FromResource` 메서드에 해당 하는 클래스가 없습니다. `UriImageSource` 클래스는 캐싱을 제어 해야 하는 경우에 유용 합니다. `FileImageSource` 클래스는 XAML에서 유용 합니다. `StreamImageSource`은 `Stream` 개체의 비동기 로드에 유용 하지만 `ImageSource.FromStream`은 동기식입니다.

## <a name="platform-independent-bitmaps"></a>플랫폼 독립적 비트맵

[**WebBitmapCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode) 프로젝트는 `ImageSource.FromUri`를 사용 하 여 웹에 비트맵을 로드 합니다. `Image` 요소는 `ContentPage`의 `Content` 속성으로 설정 되므로 페이지 크기로 제한 됩니다. 비트맵의 크기에 관계 없이 제한 된 `Image` 요소는 해당 컨테이너의 크기로 확장 되 고 비트맵의 가로 세로 비율을 유지 하면서 `Image` 요소 내의 최대 크기로 비트맵이 표시 됩니다. 비트맵을 벗어나는 `Image` 영역에는 [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)색을 지정할 수 있습니다.

[**WebBitmapXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml) 샘플은 유사 하지만 `Source` 속성을 URL로 설정 하기만 하면 됩니다. 변환은 [`ImageSourceConverter`](xref:Xamarin.Forms.ImageSourceConverter) 클래스에 의해 처리 됩니다.

### <a name="fit-and-fill"></a>맞춤 및 채우기

`Image`의 [`Aspect`](xref:Xamarin.Forms.Image.Aspect) 속성을 [`Aspect`](xref:Xamarin.Forms.Aspect) 열거의 다음 멤버 중 하나로 설정 하 여 비트맵을 확장 하는 방법을 제어할 수 있습니다.

- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit): 측면 가로 세로 비율 (기본값)
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill): 채우기 영역, 가로 세로 비율을 고려 하지 않음
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill): 영역을 채운 다음 비트맵의 일부를 자르는 방식으로 가로 세로 비율을 고려 합니다.

### <a name="embedded-resources"></a>포함된 리소스

PCL, 또는 PCL의 폴더에 비트맵 파일을 추가할 수 있습니다. **EmbeddedResource**의 **빌드 작업** 을 제공 합니다. [**ResourceBitmapCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode) 샘플에서는 `ImageSource.FromResource`를 사용 하 여 파일을 로드 하는 방법을 보여 줍니다. 메서드에 전달 된 리소스 이름 뒤에 선택적 폴더 이름 뒤에 점, 및 파일 이름 뒤에 점, 어셈블리 이름으로 구성 됩니다.

프로그램은 `Image`의 `VerticalOptions` 및 `HorizontalOptions` 속성을 `LayoutOptions.Center`로 설정 하므로 `Image` 요소를 제한 하지 않습니다. `Image`와 비트맵의 크기는 동일 합니다.

- IOS 및 Android에서 `Image`은 비트맵의 픽셀 크기입니다. 비트맵 픽셀과 화면의 픽셀 간에 일대일 매핑이 있습니다.
- 유니버설 Windows 플랫폼에서 `Image`는 장치 독립적 단위에서 비트맵의 픽셀 크기입니다. 대부분의 장치에서 각 비트맵 픽셀 여러 화면 픽셀을 차지합니다.

[**StackedBitmap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) 샘플은 `Image`를 XAML의 세로 `StackLayout`에 배치 합니다. [`ImageResourceExtension`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) 이라는 태그 확장은 XAML에서 포함 된 리소스를 참조 하는 데 도움이 됩니다. 이 클래스는에서 어셈블리에서 리소스를 로드만 있는 라이브러리에 배치 될 수 없습니다.

### <a name="more-on-sizing"></a>크기 조정에 대 한 자세한 내용

크기 비트맵 모든 플랫폼 간에 일관 되 게 하는 것이 좋습니다.
**StackedBitmap**를 사용 하면 `Image` 요소에 대 한 `WidthRequest`를 세로 `StackLayout`으로 설정 하 여 플랫폼 간에 일관성을 유지할 수 있지만이 기법을 사용 하 여 크기를 줄일 수 있습니다.

또한 플랫폼에서 이미지 크기를 일관 되 게 유지 하기 위해 `HeightRequest`를 설정할 수 있지만, 비트맵의 제한 된 너비는이 기술의 다양성을 제한 합니다. 세로 `StackLayout`에 있는 이미지의 경우 `HeightRequest`를 피해 야 합니다.

가장 좋은 방법은 장치 독립적 단위의 휴대폰 너비 보다 넓은 비트맵으로 시작 하 고 `WidthRequest`를 장치 독립적 단위에서 원하는 너비로 설정 하는 것입니다. 이는 [**DeviceIndBitmapSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize) 샘플에 설명 되어 있습니다.

[**MadTeaParty**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty) 은 Wonderland에 Lewis Carroll의 *Alice 탐험* 의 7 장, John Tenniel의 원본 그림을 표시 합니다.

[![매드 tea 파티의 삼중 스크린샷](images/ch13fg16-small.png "매드 Hatters Tea 파티 서적 텍스트")](images/ch13fg16-large.png#lightbox "매드 Hatters Tea 파티 서적 텍스트")

### <a name="browsing-and-waiting"></a>검색 및 대기

[**Imagebrowser**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser) 샘플을 통해 사용자는 Xamarin 웹 사이트에 저장 된 주식 이미지를 탐색할 수 있습니다. .NET [`WebRequest`](xref:System.Net.WebRequest) 클래스를 사용 하 여 비트맵 목록과 함께 JSON 파일을 다운로드 합니다.

> [!NOTE]
> Xamarin Forms 프로그램은 인터넷을 통해 파일에 액세스 하는 [`WebRequest`](xref:System.Net.WebRequest) 가 아닌 [`HttpClient`](xref:System.Net.Http.HttpClient) 를 사용 해야 합니다.

프로그램은 [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) 를 사용 하 여 무언가 진행 중임을 표시 합니다. 각 비트맵이 로드 될 때 `Image`의 읽기 전용 [`IsLoading`](xref:Xamarin.Forms.Image.IsLoading) 속성이 `true`됩니다. `IsLoading` 속성은 바인딩 가능한 속성에 의해 지원 되므로 해당 속성이 변경 될 때 `PropertyChanged` 이벤트가 발생 합니다. 프로그램은이 이벤트에 처리기를 연결 하 고 `IsLoaded`의 현재 설정을 사용 하 여 `ActivityIndicator`의 [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning) 속성을 설정 합니다.

## <a name="streaming-bitmaps"></a>스트리밍 비트맵

`ImageSource.FromStream` 메서드는 .NET `Stream`을 기반으로 `ImageSource`를 만듭니다. 메서드에는 `Stream` 개체를 반환 하는 `Func` 개체가 전달 되어야 합니다.

### <a name="accessing-the-streams"></a>스트림 액세스

[**BitmapStreams**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams) 샘플에서는 `ImaageSource.FromStream` 메서드를 사용 하 여 포함 리소스로 저장 된 비트맵을 로드 하 고 웹 전체에 비트맵을 로드 하는 방법을 보여 줍니다.

### <a name="generating-bitmaps-at-run-time"></a>런타임에 비트맵 생성

모든 Xamarin.ios 플랫폼은 코드에서 생성 하 고 `MemoryStream`에 저장 하기 쉬운 압축 되지 않은 BMP 파일 형식을 지원 합니다. 이 방법을 사용 하면 **용 xamrin** 라이브러리의 [`BmpMaker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) 클래스에서 구현 된 대로 런타임에 비트맵을 알고리즘 방식으로 만들 수 있습니다.

"직접 수행" [**DiyGradientBitmap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) 샘플은 `BmpMaker`를 사용 하 여 그라데이션 이미지를 사용 하는 비트맵을 만드는 방법을 보여 줍니다.

## <a name="platform-specific-bitmaps"></a>플랫폼별 비트맵

모든 Xamarin.Forms 플랫폼 플랫폼 응용 프로그램 어셈블리에서 비트맵을 저장할 수 있습니다. Xamarin Forms 응용 프로그램에서 검색 되는 경우 이러한 플랫폼 비트맵은 [`FileImageSource`](xref:Xamarin.Forms.FileImageSource)형식입니다. 에 대 한 사용:

- 의 [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) 속성 [`MenuItem`](xref:Xamarin.Forms.MenuItem)
- 의 [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) 속성 [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)
- 의 [`Image`](xref:Xamarin.Forms.Button) 속성 `Button`

플랫폼 어셈블리에는 이미 아이콘 및 시작 화면에 대 한 비트맵에

- IOS 프로젝트의 **리소스** 폴더에서
- Android 프로젝트의 **Resources** 폴더에 있는 하위 폴더
- Windows 프로젝트의 **자산** 폴더 (windows 플랫폼에서 비트맵을 해당 폴더로 제한 하지 않음)

[**Platformbitmaps**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps) 샘플은 코드를 사용 하 여 플랫폼 응용 프로그램 프로젝트의 아이콘을 표시 합니다.

### <a name="bitmap-resolutions"></a>비트맵 해상도

모든 플랫폼에 다른 장치 해상도 대해 비트맵 이미지의 여러 버전을 저장할 수 있습니다. 런타임 시 적절 한 버전 화면 장치 해상도에 따라 로드 됩니다.

Ios의 경우 이러한 비트맵 파일 이름에 접미사 구분 됩니다.

- 160 DPI 장치 (장치 독립적 단위 1 픽셀)에 대 한 접미사 없음
- 320 DPI 장치의 '@2x' 접미사 (2 픽셀-DIU)
- 480 DPI 장치의 '@3x' 접미사 (DIU에 대 한 3 픽셀)

1 인치의 사각형으로 표시 하기 위한 비트맵은 세 가지 버전에 존재 합니다.

- 160 픽셀 사각형에서 MyImage.jpg
- 320 픽셀 사각형에서 MyImage@2x.jpg
- 480 픽셀 사각형에서 MyImage@3x.jpg

이 비트맵 MyImage.jpg으로 프로그램 참조 하지만 적절 한 버전의 화면 해상도에 따라 런타임에 검색 됩니다. 제한 되지 않은, 비트맵은 항상 160 장치 독립적 단위에서 렌더링 됩니다.

Android의 경우 비트맵은 **Resources** 폴더의 다양 한 하위 폴더에 저장 됩니다.

- drawable-ldpi (낮은 DPI) 120 DPI 장치 (0.75를 DIU 픽셀)에 대 한
- drawable-mdpi (중간) (160DPI 장치용 (DIU는 1 픽셀)
- drawable-hdpi (높음) 240 DPI 장치 (1.5는 DIU 픽셀)에 대 한
- drawable-xhdpi (매우 높음) 320 DPI 장치 (2는 DIU 픽셀)에 대 한
- drawable-xxhdpi (추가 매우 높은) 480 DPI 장치 (3는 DIU 픽셀)에 대 한
- drawable-xxxhdpi (세 개의 추가 최대값) 640 DPI 장치 (4는 DIU 픽셀)에 대 한

정사각형 1 인치에서 렌더링 되는 데는 비트맵에 대 한 비트맵의 다양 한 버전에는 이름은 같지만 다른 크기로 있고 이러한 폴더에 저장:

- drawable-ldpi/MyImage.jpg 120 픽셀 사각형
- drawable-mdpi/MyImage.jpg 160 픽셀 사각형
- drawable-hdpi/MyImage.jpg 240 픽셀 사각형
- drawable-xhdpi/MyImage.jpg 320 픽셀 사각형
- drawable-xxhdpi/MyImage.jpg 480 픽셀 사각형
- drawable-xxxhdpi/MyImage.jpg 640 픽셀 사각형

비트맵은 항상 160 장치 독립적 단위에 렌더링 합니다. (표준 Xamarin.Forms 솔루션 템플릿을 포함 hdpi, xhdpi, 및 xxhdpi 폴더.)

UWP 프로젝트에 구성 된 장치 독립적 단위 당 픽셀 수 크기 조정 비율을 백분율로 예를 들어 비트맵 이름 지정 체계를 지원 합니다.

- 320 픽셀 사각형 MyImage.scale 200.jpg

일부 백분율에만 유효 합니다. 이 책의 샘플 프로그램에는 **200** 접미사가 있는 이미지만 포함 되어 있지만 현재 xamarin.ios 솔루션 템플릿에는 **100**, **규모 125**, **규모 150**및 **확장 400**이 포함 됩니다.

플랫폼 프로젝트에 비트맵을 추가할 때 **빌드 작업** 은 다음과 같아야 합니다.

- iOS: **BundleResource**
- Android: **Androidresource**
- UWP: **콘텐츠**

[**Imagetap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap) 샘플은 `TapGestureRecognizer` 설치 된 `Image` 요소로 구성 된 두 개의 단추와 유사한 개체를 만듭니다. 개체 1 인치의 사각형 되도록 작성 되었습니다. `Image`의 `Source` 속성은 `OnPlatform` 및 `On` 개체를 사용 하 여 플랫폼에서 잠재적으로 다른 파일 이름을 참조 하도록 설정 됩니다. 비트맵 이미지는 크기 비트맵을 검색 하 고 렌더링을 볼 수 있도록 해당 픽셀 크기를 나타내는 숫자를 포함 합니다.

### <a name="toolbars-and-their-icons"></a>도구 모음 및 해당 아이콘

플랫폼별 비트맵의 주요 용도 중 하나는 `Page`으로 정의 된 [`ToolbarItems`](xref:Xamarin.Forms.Page.ToolbarItems) 컬렉션에 [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) 개체를 추가 하 여 생성 되는 xamarin.ios 도구 모음입니다. `ToobarItem`은 일부 속성을 상속 하는 [`MenuItem`](xref:Xamarin.Forms.MenuItem) 에서 파생 됩니다.

가장 중요 한 `ToolbarItem` 속성은 다음과 같습니다.

- 플랫폼 및 `Order`에 따라 나타날 수 있는 텍스트에 대 한 [`Text`](xref:Xamarin.Forms.MenuItem.Text)
- 플랫폼 및 `Order`에 따라 나타날 수 있는 이미지에 대 한 `FileImageSource` 형식의 [`Icon`](xref:Xamarin.Forms.MenuItem.Icon)
- [`ToolbarItemOrder`](xref:Xamarin.Forms.ToolbarItemOrder)형식의 [`Order`](xref:Xamarin.Forms.ToolbarItem.Order) , [`Default`](xref:Xamarin.Forms.ToolbarItemOrder.Default), [`Primary`](xref:Xamarin.Forms.ToolbarItemOrder.Primary)및 [`Secondary`](xref:Xamarin.Forms.ToolbarItemOrder.Secondary)의 세 멤버를 포함 하는 열거형입니다.

`Primary` 항목의 수는 3 또는 4 개로 제한 되어야 합니다. 모든 항목에 대 한 `Text` 설정을 포함 해야 합니다. 대부분의 플랫폼에서는 `Primary` 항목에만 `Icon` 필요 하지만 Windows 8.1 모든 항목에 대 한 `Icon` 필요 합니다. 아이콘에는 32 장치 독립적 단위 정사각형 이어야 합니다. `FileImageSource` 형식은 플랫폼에 한정 됨을 나타냅니다.

`ToolbarItem`은 `Button`와 매우 비슷하게 탭 할 때 [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked) 이벤트를 발생 시킵니다. MVVM와의 연결에서 자주 사용 되는 [`Command`](xref:Xamarin.Forms.MenuItem.Command) 및 [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter) 속성도 지원 `ToolbarItem`. ( [18 장, MVVM)을](chapter18.md)참조 하세요.

IOS와 Android 모두 도구 모음을 표시 하는 페이지가 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 또는 `NavigationPage`탐색 하는 페이지 여야 합니다. 도구 집합 [**데모**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo) 프로그램은 `ContentPage` 인수를 사용 하 여 `App` 클래스의 `MainPage` 속성을 [`NavigationPage` 생성자](xref:Xamarin.Forms.NavigationPage.%23ctor(Xamarin.Forms.Page)) 로 설정 하 고 도구 모음의 생성 및 이벤트 처리기를 보여 줍니다.

### <a name="button-images"></a>단추 이미지

또한 [**Buttonimage**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage) 샘플에서 설명한 대로 플랫폼별 비트맵을 사용 하 여 `Button`의 [`Image`](xref:Xamarin.Forms.Button.Image) 속성을 32 장치 독립적 단위 사각형의 비트맵으로 설정할 수 있습니다.

> [!NOTE]
> 단추의 이미지를 사용 하는 향상 되었습니다. [단추에 비트맵 사용을](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)참조 하세요.

## <a name="related-links"></a>관련 링크

- [13 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [13 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [이미지 작업](~/xamarin-forms/user-interface/images.md)
- [단추에 비트맵 사용](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)
