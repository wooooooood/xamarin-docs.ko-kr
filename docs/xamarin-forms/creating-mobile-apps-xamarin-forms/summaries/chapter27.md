---
title: 요약 27 장입니다. 사용자 지정 렌더러
description: 'Xamarin.ios를 사용 하 여 Mobile Apps 만들기: 요약 27 장입니다. 사용자 지정 렌더러'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 49961953-9336-4FD4-A42F-6D9B05FF52E7
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: fd4014fa4db4e90596c100d454cf0467512240a4
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760498"
---
# <a name="summary-of-chapter-27-custom-renderers"></a>요약 27 장입니다. 사용자 지정 렌더러

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)

> [!NOTE] 
> 이 페이지에 대 한 참고 사항 Xamarin.Forms 책의 내용을에서 달라졌는지를 위치 하는 영역을 나타냅니다.

같은 Xamarin.Forms 요소를 `Button` 라는 클래스에 캡슐화 하는 플랫폼 특정 단추를 사용 하 여 렌더링은 `ButtonRenderer`합니다.  같습니다 합니다 [iOS 버전 `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/Renderers/ButtonRenderer.cs)의 [Android 버전의 `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/Renderers/ButtonRenderer.cs), 및 [의 UWP 버전 `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.UAP/ButtonRenderer.cs).

이 장에서 플랫폼별 개체에 매핑되는 사용자 지정 보기를 만들어 사용자 고유의 렌더러를 작성 하는 방법 설명 합니다.

## <a name="the-complete-class-hierarchy"></a>완전 한 클래스 계층 구조

Xamarin.Forms 플랫폼 특정 코드를 포함 하는 4 명의 어셈블리 있습니다.
다음이 링크를 사용 하 여 GitHub에서 소스를 볼 수 있습니다.

- [**Xamarin.Forms.Platform** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform) (소규모)
- [**Xamarin.Forms.Platform.iOS**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.iOS)
- [**Xamarin.Forms.Platform.Android**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.Android)
- [**Xamarin.Forms.Platform.UAP**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.UAP)

> [!NOTE]
> `WinRT` 책에서 설명 하는 어셈블리는이 솔루션의 일부로 더 이상. 

합니다 [ **PlatformClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/PlatformClassHierarchy) 샘플 실행 중인 플랫폼에 대해 사용할 수 있는 어셈블리에 대 한 클래스 계층 구조를 표시 합니다.

명명 된 중요 한 클래스를 보면 `ViewRenderer`합니다. 플랫폼별 렌더러를 만들 때에서 파생 클래스입니다. 대상 플랫폼의 시스템 보기에 연결 됩니다 때문에 세 가지 버전에 있습니다.

IOS [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs#L25) 제네릭 인수를 포함 합니다.

- `TView` 제한 [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeView` 제한 [`UIKit.UIView`](xref:UIKit.UIView)

Android [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/ViewRenderer.cs#L17) 제네릭 인수를 포함 합니다.

- `TView` 제한 [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeView` 제한 [`Android.Views.View`](xref:Android.Views.View)

UWP [ `ViewRenderer<TElement, TNativeElement>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.UAP/ViewRenderer.cs#L6) 에 다르게 명명 된 제네릭 인수:

- `TElement` 제한 [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeElement` 제한 [`Windows.UI.Xaml.FrameworkElement`](/uwp/api/Windows.UI.Xaml.FrameworkElement)

렌더러를 작성 하는 경우 있습니다 됩니다 수에서 클래스를 파생 `View`, 여러 다음 쓰고 `ViewRenderer` 클래스를 지 원하는 각 플랫폼에 대 한 합니다. 각 플랫폼 전용 구현으로 지정 된 형식에서 파생 되는 기본 클래스 참조를 `TNativeView` 또는 `TNativeElement` 매개 변수입니다.

## <a name="hello-custom-renderers"></a>Hello, 사용자 지정 렌더러!

합니다 [ **HelloRenderers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/HelloRenderers) 라는 사용자 지정 뷰를 참조 하는 프로그램 `HelloView` 에 해당 [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/App.cs) 클래스입니다.

[ `HelloView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/HelloView.cs) 클래스가에 포함 되는 **HelloRenderers** 프로젝트에서 파생 되 고 `View`입니다.

합니다 [ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.iOS/HelloViewRenderer.cs) 클래스를 **HelloRenderers.iOS** 프로젝트에서 파생 `ViewRenderer<HelloView, UILabel>`합니다. 에 `OnElementChanged` 재정의 만들면 네이티브 iOS `UILabel` 호출 `SetNativeControl`합니다.

합니다 [ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.Droid/HelloViewRenderer.cs) 클래스를 **HelloRenderers.Droid** 프로젝트에서 파생 `ViewRenderer<HelloView, TextView>`합니다. 에 `OnElementChanged` 재정의 만들면 Android `TextView` 호출 `SetNativeControl`합니다.

합니다 [ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.UWP/HelloViewRenderer.cs) 클래스를 **HelloRenderers.UWP** 에서 파생 되는 다른 Windows 프로젝트 `ViewRenderer<HelloView, TextBlock>`합니다. 에 `OnElementChanged` 재정의 만들면는 Windows `TextBlock` 호출 `SetNativeControl`합니다.

모든는 `ViewRenderer` 파생형 포함는 `ExportRenderer` 연결 하는 어셈블리 수준 특성을 `HelloView` 클래스는 특정 `HelloViewRenderer` 클래스. Xamarin.Forms에서 개별 플랫폼 프로젝트가 렌더러를 찾는 방법입니다.

[![Hello 보기의 삼중 스크린샷](images/ch27fg02-small.png "사용자 지정 렌더러")](images/ch27fg02-large.png#lightbox "사용자 지정 렌더러")

## <a name="renderers-and-properties"></a>렌더러 및 속성

렌더러의 다음 집합 타원 그리기를 구현 하 고 다양 한 프로젝트에는 [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) 솔루션입니다.

합니다 [ `EllipseView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/EllipseView.cs) 클래스에는 **Xamarin.FormsBook.Platform** 플랫폼입니다. 클래스는 비슷합니다 `BoxView` 단일 속성만 정의: `Color` 형식의 `Color`합니다.

렌더러를 설정 하는 속성 값을 전송할 수 있습니다는 `View` 재정의 하 여 네이티브 개체에는 `OnElementPropertyChanged` 렌더러에 메서드. 이 메서드 내에서 (및 대부분의 렌더러에서 내에서), 두 개의 속성이 제공 됩니다.

- `Element`Xamarin.Forms 요소
- `Control`위젯 또는 컨트롤 개체를 네이티브 뷰

이러한 속성의 형식을 제네릭 매개 변수에 의해 결정 됩니다 `ViewRenderer`합니다. 이 예에서 `Element` 유형의 `EllipseView`합니다.

`OnElementPropertyChanged` 재정의 하므로 전송할 수 있습니다 합니다 `Color` 값을 `Element` 네이티브를 `Control` 일종의 변환이 아마도 개체. 세 가지 렌더러는

- iOS: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseViewRenderer.cs)를 사용 하는 프로그램 [ `EllipseUIView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseUIView.cs) 타원에 대 한 클래스입니다.
- Android: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseViewRenderer.cs)를 사용 하는 프로그램 [ `EllipseDrawableView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseDrawableView.cs) 타원에 대 한 클래스입니다.
- : UWP [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/EllipseViewRenderer.cs), 네이티브 Windows를 사용할 수 있는 [ `Ellipse` ](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 클래스입니다.

합니다 [ **EllipseDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/EllipseDemo) 클래스는 여러 가지 표시 `EllipseView` 개체:

[![타원 데모 삼중 스크린 샷](images/ch27fg03-small.png "EllipseView 사용자 지정 렌더러")](images/ch27fg03-large.png#lightbox "EllipseView 사용자 지정 렌더러")

합니다 [ **BouncingBall** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/BouncingBall) 바운스는 `EllipseView` 화면의 측면을 해제 합니다.

## <a name="renderers-and-events"></a>렌더러 및 이벤트

렌더러의 이벤트를 직접 생성 하는 것도 가능 합니다. [ `StepSlider` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/StepSlider.cs) 클래스는 일반 Xamarin.Forms 비슷합니다 `Slider` 사이의 불연속 단계 수를 지정할 수 있습니다 하지만 합니다 `Minimum` 및 `Maximum` 값입니다.

세 가지 렌더러는

- iOS: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/StepSliderRenderer.cs)
- Android: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/StepSliderRenderer.cs)
- UWP: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/StepSliderRenderer.cs)

된 렌더러 네이티브 컨트롤에 변경 내용을 검색 하 고 호출 하 `SetValueFromRenderer`에 정의 된 바인딩 가능한 속성을 참조 하는 합니다 `StepSlider`는 원인에 대 한 변경을 `StepSlider` 시키려면를 `ValueChanged` 이벤트입니다.

합니다 [ **StepSliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/StepSliderDemo) 샘플에서는이 새 슬라이더를 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [27 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)
- [27 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)
- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
