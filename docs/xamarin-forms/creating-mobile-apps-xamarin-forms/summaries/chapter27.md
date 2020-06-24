---
title: 요약 - 27장. 사용자 지정 렌더러
description: 'Xamarin.Forms로 모바일 앱 만들기: 요약 - 27장. 사용자 지정 렌더러'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 49961953-9336-4FD4-A42F-6D9B05FF52E7
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4b0a3e14126dc8c92a1d1b60db7dbb2afd8e8c12
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136593"
---
# <a name="summary-of-chapter-27-custom-renderers"></a>요약 - 27장. 사용자 지정 렌더러

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)

> [!NOTE] 
> 이 페이지의 정보는 Xamarin.Forms가 책에 제공된 자료와는 다르게 사용되는 경우를 설명합니다.

`Button`와 같은 Xamarin.Forms 요소는 `ButtonRenderer`라는 클래스에 캡슐화된 플랫폼별 단추로 렌더링됩니다.  [`ButtonRenderer`의 iOS 버전](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/Renderers/ButtonRenderer.cs), [`ButtonRenderer`의 Android 버전](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/Renderers/ButtonRenderer.cs) 및 [`ButtonRenderer`의 UWP 버전](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.UAP/ButtonRenderer.cs)이 있습니다.

이 장에서는 고유한 렌더러를 작성하여 플랫폼별 개체에 매핑되는 사용자 지정 보기를 만드는 방법을 설명합니다.

## <a name="the-complete-class-hierarchy"></a>전체 클래스 계층 구조

Xamarin.Forms 플랫폼별 코드를 포함하는 4개의 어셈블리가 있습니다.
다음 링크를 사용하여 GitHub에서 원본을 볼 수 있습니다.

- [ **Xamarin.Forms.Platform**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform)(매우 작음)
- [ **Xamarin.Forms.Platform.iOS**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.iOS)
- [ **Xamarin.Forms.Platform.Android**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.Android)
- [ **Xamarin.Forms.Platform.UAP**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.UAP)

> [!NOTE]
> 이 책에 언급된 `WinRT` 어셈블리는 더 이상 이 솔루션에 포함되지 않습니다. 

[**PlatformClassHierarchy**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/PlatformClassHierarchy) 샘플은 실행 중인 플랫폼에 유효한 어셈블리에 대한 클래스 계층 구조를 표시합니다.

`ViewRenderer`라는 중요한 클래스를 확인할 수 있습니다. 이는 플랫폼별 렌더러를 만들 때 파생되는 클래스입니다. 대상 플랫폼의 뷰 시스템에 연결되어 있기 때문에 세 가지 버전으로 존재합니다.

iOS [`ViewRenderer<TView, TNativeView>`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs#L25)에는 다음과 같은 제네릭 인수가 있습니다.

- [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)의 제약을 받는 `TView`
- [`UIKit.UIView`](xref:UIKit.UIView)의 제약을 받는 `TNativeView`

Android [`ViewRenderer<TView, TNativeView>`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/ViewRenderer.cs#L17)에는 다음과 같은 제네릭 인수가 있습니다.

- [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)의 제약을 받는 `TView`
- [`Android.Views.View`](xref:Android.Views.View)의 제약을 받는 `TNativeView`

UWP [`ViewRenderer<TElement, TNativeElement>`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.UAP/ViewRenderer.cs#L6)에는 서로 다르게 명명된 제네릭 인수가 있습니다.

- [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)의 제약을 받는 `TElement`
- [`Windows.UI.Xaml.FrameworkElement`](/uwp/api/Windows.UI.Xaml.FrameworkElement)의 제약을 받는 `TNativeElement`

렌더러를 작성할 때는 `View`에서 클래스를 파생한 후 지원되는 각 플랫폼당 하나씩 여러 개의 `ViewRenderer` 클래스를 작성합니다. 각 플랫폼별 구현은 `TNativeView` 또는 `TNativeElement` 매개 변수로 지정하는 형식에서 파생되는 네이티브 클래스를 참조합니다.

## <a name="hello-custom-renderers"></a>Hello, 사용자 지정 렌더러!

[**HelloRenderers**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/HelloRenderers) 프로그램은 [`App`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/App.cs) 클래스의 `HelloView`라는 사용자 지정 뷰를 참조합니다.

[`HelloView`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/HelloView.cs) 클래스는 **HelloRenderers** 프로젝트에 포함되며 `View`에서 간단히 파생됩니다.

**HelloRenderers.iOS** 프로젝트의 [`HelloViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.iOS/HelloViewRenderer.cs) 클래스는 `ViewRenderer<HelloView, UILabel>`에서 파생됩니다. `OnElementChanged` 재정의에서는 네이티브 iOS `UILabel`을 만들고 `SetNativeControl`을 호출합니다.

**HelloRenderers.Droid** 프로젝트의 [`HelloViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.Droid/HelloViewRenderer.cs) 클래스는 `ViewRenderer<HelloView, TextView>`에서 파생됩니다. `OnElementChanged` 재정의에서는 Android `TextView`를 만들고 `SetNativeControl`을 호출합니다.

**HelloRenderers.UWP** 및 기타 Windows 프로젝트의 [`HelloViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.UWP/HelloViewRenderer.cs) 클래스는 `ViewRenderer<HelloView, TextBlock>`에서 파생됩니다. `OnElementChanged` 재정의에서는 Windows `TextBlock`을 만들고 `SetNativeControl`을 호출합니다.

모든 `ViewRenderer` 파생 항목에는 `HelloView` 클래스를 특정 `HelloViewRenderer` 클래스와 연결하는 어셈블리 수준에 대한 `ExportRenderer` 특성이 포함되어 있습니다. 이는 Xamarin.Forms가 개별 플랫폼 프로젝트에서 렌더러를 찾는 방법입니다.

[![Hello View의 삼중 스크린샷](images/ch27fg02-small.png "사용자 지정 렌더러")](images/ch27fg02-large.png#lightbox "사용자 지정 렌더러")

## <a name="renderers-and-properties"></a>렌더러 및 속성

다음 렌더러 세트는 타원 그리기를 구현하며, [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) 솔루션의 다양한 프로젝트에 있습니다.

[`EllipseView`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/EllipseView.cs) 클래스는 **Xamarin.FormsBook.Platform** 플랫폼에 있습니다. 이 클래스는 `BoxView`와 비슷하며 `Color` 형식의 `Color`라는 단일 속성만 정의합니다.

렌더러는 렌더러의 `OnElementPropertyChanged` 메서드를 재정의하여 `View`에 설정된 속성 값을 네이티브 개체로 전송할 수 있습니다. 이 메서드(및 대부분의 렌더러) 내에서 다음 두 가지 속성을 사용할 수 있습니다.

- `Element`, Xamarin.Forms 요소
- `Control`, 네이티브 뷰 또는 위젯 또는 컨트롤 개체

이러한 속성의 형식은 `ViewRenderer`에 대한 제네릭 매개 변수에 의해 결정됩니다. 이 예에서 `Element`는 `EllipseView` 형식입니다.

따라서 `OnElementPropertyChanged` 재정의는 일종의 변환을 통해 `Element`의 `Color` 값을 네이티브 `Control` 개체로 전송할 수 있습니다. 3개의 렌더러는 다음과 같습니다.

- iOS: [`EllipseViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseViewRenderer.cs). 타원에 [`EllipseUIView`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseUIView.cs) 클래스를 사용합니다.
- Android: [`EllipseViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseViewRenderer.cs). 타원에 [`EllipseDrawableView`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseDrawableView.cs) 클래스를 사용합니다.
- UWP: [`EllipseViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/EllipseViewRenderer.cs). 네이티브 Windows [`Ellipse`](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 클래스를 사용할 수 있습니다.

[**EllipseDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/EllipseDemo) 클래스는 다음과 같은 몇 가지 `EllipseView` 개체를 표시합니다.

[![Ellipse Demo의 삼중 스크린샷](images/ch27fg03-small.png "EllipseView 사용자 지정 렌더러")](images/ch27fg03-large.png#lightbox "EllipseView 사용자 지정 렌더러")

[**BouncingBall**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/BouncingBall)은 화면 양쪽에서 `EllipseView`를 튕깁니다.

## <a name="renderers-and-events"></a>렌더러 및 이벤트

또한 렌더러에서 이벤트를 간접적으로 생성할 수 있습니다. [`StepSlider`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/StepSlider.cs) 클래스는 일반적인 Xamarin.Forms `Slider`와 유사하지만 `Minimum` 값과 `Maximum` 값 사이에 여러 불연속 단계를 지정할 수 있습니다.

3개의 렌더러는 다음과 같습니다.

- iOS: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/StepSliderRenderer.cs)
- Android: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/StepSliderRenderer.cs)
- UWP: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/StepSliderRenderer.cs)

렌더러는 네이티브 컨트롤의 변경 내용을 검색한 다음, `StepSlider`에 정의된 바인딩 가능한 속성을 참조하는 `SetValueFromRenderer`를 호출합니다. 이 항목이 변경되면 `StepSlider`가 `ValueChanged` 이벤트를 발생시킵니다.

[**StepSliderDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/StepSliderDemo) 샘플에서는 이 새 슬라이더를 보여줍니다.

## <a name="related-links"></a>관련 링크

- [27장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)
- [27장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)
- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
