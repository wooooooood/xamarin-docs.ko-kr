---
title: "요약 장 27입니다. 사용자 지정 렌더러"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0c1dc9ba5cf382551a1142110c68d16421db07e4
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="summary-of-chapter-27-custom-renderers"></a>요약 장 27입니다. 사용자 지정 렌더러

와 같은 Xamarin.Forms 요소가 `Button` 라는 클래스에 캡슐화 하는 플랫폼 특정 단추가 렌더링 `ButtonRenderer`합니다.  다음은 [iOS 버전 `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/Renderers/ButtonRenderer.cs), [Android 버전의 `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/Renderers/ButtonRenderer.cs), 및 [의 Windows 런타임 버전 `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ButtonRenderer.cs)합니다.

이 장에서 플랫폼 특정 개체에 매핑되는 사용자 지정 보기를 만들어 사용자 고유의 렌더러를 작성 하는 방법을 설명 합니다.

## <a name="the-complete-class-hierarchy"></a>완전 한 클래스 계층 구조

Xamarin.Forms 플랫폼별 코드를 포함 하는 7 명의 어셈블리 있습니다.
이러한 링크를 사용 하 여 GitHub의 소스를 볼 수 있습니다.

- [**Xamarin.Forms.Platform** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform) (매우 작음)
- [**Xamarin.Forms.Platform.iOS**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.iOS)
- [**Xamarin.Forms.Platform.Android**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.Android)
- [**Xamarin.Forms.Platform.WinRT** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT) (다음 3 보다 큰)
- [**Xamarin.Forms.Platform.UAP**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.UAP)
- [**Xamarin.Forms.Platform.WinRT.Tablet**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Tablet)
- [**Xamarin.Forms.Platform.WinRT.Phone**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Phone)

[ **PlatformClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/PlatformClassHierarchy) 샘플 실행 중인 플랫폼에 대해 사용할 수 있는 어셈블리에 대 한 클래스 계층 구조를 표시 합니다.

라는 중요 한 클래스를 보면 `ViewRenderer`합니다. 플랫폼 관련 렌더러를 만들 때 파생 클래스입니다. 대상 플랫폼의 보기 시스템에 연결 되기 때문에 3 가지 다른 버전에 있습니다.

IOS [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs#L26) 제네릭 인수를 포함 합니다.

- `TView` 로 제한 [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeView` 로 제한 [`UIKit.UIView`](https://developer.xamarin.com/api/type/UIKit.UIView/)

Android [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/ViewRenderer.cs#L14) 제네릭 인수를 포함 합니다.

- `TView` 로 제한 [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeView` 로 제한 [`Android.Views.View`](https://developer.xamarin.com/api/type/Android.Views.View/)

Windows 런타임 [ `ViewRenderer<TElement, TNativeElement>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ViewRenderer.cs#L12) 에 다르게 명명 된 제네릭 인수:

- `TElement` 로 제한 [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeElement` 로 제한 [`Windows.UI.Xaml.FrameworkElement`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx)

렌더러를 작성할 때 있습니다 됩니다 수에서 클래스를 파생 `View`, 다음 여러 작성 `ViewRenderer` 각 지원된 플랫폼에 대 한 클래스입니다. 각 플랫폼 특정 구현으로 지정 하는 형식에서 파생 되는 기본 클래스를 참조 합니다는 `TNativeView` 또는 `TNativeElement` 매개 변수입니다.

## <a name="hello-custom-renderers"></a>Hello, 사용자 지정 렌더러!

[ **HelloRenderers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/HelloRenderers) 라는 사용자 지정 보기를 참조 하는 프로그램 `HelloView` 에 해당 [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/App.cs) 클래스입니다.

[ `HelloView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/HelloView.cs) 에 클래스가 포함 되는 **HelloRenderers** 프로젝트 및에서 단순히 파생 `View`합니다.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.iOS/HelloViewRenderer.cs) 클래스에 **HelloRenderers.iOS** 프로젝트에서 파생 `ViewRenderer<HelloView, UILabel>`합니다. 에 `OnElementChanged` 네이티브 iOS 만듭니다 override, `UILabel` 및 호출 `SetNativeControl`합니다.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.Droid/HelloViewRenderer.cs) 클래스에 **HelloRenderers.Droid** 프로젝트에서 파생 `ViewRenderer<HelloView, TextView>`합니다. 에 `OnElementChanged` 는 Android 만듭니다 override, `TextView` 및 호출 `SetNativeControl`합니다.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.UWP/HelloViewRenderer.cs) 클래스에 **HelloRenderers.UWP** 에서 파생 되는 다른 Windows 프로젝트 `ViewRenderer<HelloView, TextBlock>`합니다. 에 `OnElementChanged` 재정의 Windows 만듭니다 `TextBlock` 및 호출 `SetNativeControl`합니다.

모든는 `ViewRenderer` 파생 항목 포함는 `ExportRenderer` 연결 하는 어셈블리 수준에서 특성의 `HelloView` 클래스는 특정 `HelloViewRenderer` 클래스. 다음은 Xamarin.Forms 개별 플랫폼 프로젝트에서는 렌더러를 찾는 방법입니다.

[![Hello 보기의 삼중 스크린 샷](images/ch27fg02-small.png "사용자 지정 렌더러")](images/ch27fg02-large.png "사용자 지정 렌더러")

## <a name="renderers-and-properties"></a>렌더러 및 속성

다음 렌더러 집합이 타원 그리기를 구현 하 고의 다양 한 프로젝트에는 [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) 솔루션입니다.

[ `EllipseView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/EllipseView.cs) 클래스에는 **Xamarin.FormsBook.Platform** 플랫폼입니다. 클래스는 비슷합니다 `BoxView` 단일 속성만 정의: `Color` 형식의 `Color`합니다.

된 렌더러에서 설정한 속성 값을 전송할 수 있습니다는 `View` 재정의 하 여 네이티브 개체에는 `OnElementPropertyChanged` 렌더러에 메서드. 이 메서드 내에서 (및 대부분의 렌더러에서 내), 두 개의 속성이 제공 됩니다.

- `Element`Xamarin.Forms 요소
- `Control`네이티브 뷰 또는 위젯 또는 컨트롤 개체

이러한 속성의 형식은 제네릭 매개 변수에 의해 결정 됩니다 `ViewRenderer`합니다. 이 예제에서는 `Element` 유형의 `EllipseView`합니다.

`OnElementPropertyChanged` 재정의 따라서 전송 될 수는 `Color` 값은 `Element` 는 네이티브 `Control` 변환의 일종으로 아마도 개체입니다. 세 가지 렌더러는

- iOS: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseViewRenderer.cs)를 사용 하는 [ `EllipseUIView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseUIView.cs) 타원에 대 한 클래스입니다.
- Android: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseViewRenderer.cs)를 사용 하는 [ `EllipseDrawableView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseDrawableView.cs) 타원에 대 한 클래스입니다.
- Windows 런타임: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/EllipseViewRenderer.cs), 네이티브 Windows를 사용할 수 있는 [ `Ellipse` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.ellipse.aspx) 클래스입니다.

[ **EllipseDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/EllipseDemo) 클래스 표시 중 몇 `EllipseView` 개체:

[![타원 데모의 삼중 스크린 샷](images/ch27fg03-small.png "EllipseView 사용자 지정 렌더러")](images/ch27fg03-large.png "EllipseView 사용자 지정 렌더러")

[ **BouncingBall** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/BouncingBall) 반송는 `EllipseView` 화면의 측면을 해제 합니다.

## <a name="renderers-and-events"></a>렌더러 및 이벤트

렌더러의 이벤트를 직접 생성 하는 것도 가능 합니다. [ `StepSlider` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/StepSlider.cs) 클래스는 일반 Xamarin.Forms 비슷합니다 `Slider` 되지만 보조 데이터베이스가 간에 개별 단계 수를 지정 하는 `Minimum` 및 `Maximum` 값입니다.

세 가지 렌더러는

- iOS: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/StepSliderRenderer.cs)
- Android: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/StepSliderRenderer.cs)
- Windows 런타임: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/StepSliderRenderer.cs)

된 렌더러 네이티브 컨트롤에 대 한 변경 내용을 검색 한 다음 호출 `SetValueFromRenderer`에 정의 된 바인딩 가능한 속성을 참조 하는 `StepSlider`,으로 구독이에 대 한 변경의 `StepSlider` 를 발생 시키는 `ValueChanged` 이벤트입니다.

[ **StepSliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/StepSliderDemo) 샘플에서는이 새 슬라이더를 보여 줍니다.



## <a name="related-links"></a>관련 링크

- [27 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)
- [27 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)
- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
