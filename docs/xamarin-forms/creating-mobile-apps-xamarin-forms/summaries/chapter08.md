---
title: 8 장의 요약입니다. 코드 및 XAML의 조율
description: 'Xamarin.Forms를 사용 하 여 모바일 앱 만들기: 8 장 요약 합니다. 코드 및 XAML의 조율'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5970DEEB-1FC9-4F78-B4F6-D403E16D22ED
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: 1aa5226e1e6867f77eea4d7679650e8d62f5b981
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156992"
---
# <a name="summary-of-chapter-8-code-and-xaml-in-harmony"></a>8 장의 요약입니다. 코드 및 XAML의 조율

이 장의 XAML를 보다 강력 하 게 탐색 및 특히 어떻게 코드 및 XAML 상호 작용 합니다.

## <a name="passing-arguments"></a>인수 전달

일반적인 경우 XAML에서 인스턴스화할 클래스에는 매개 변수가 없는 public 생성자; 있어야 합니다. 결과 개체는 속성 설정을 통해 초기화 됩니다. 그러나 두 가지 다른 개체를 인스턴스화하고 초기화 될 수 있습니다.

범용 기술 이기는 하지만 주로 MVVM 모델 보기와 관련 하 여 사용 됩니다.

### <a name="constructors-with-arguments"></a>인수를 사용 하 여 생성자

합니다 [ **ParameteredConstructorDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ParameteredConstructorDemo) 샘플을 사용 하는 방법에 설명 합니다 `x:Arguments` 생성자 인수를 지정 하는 태그입니다. 이러한 인수는 인수의 형식을 나타내는 요소 태그로 구분 되어야 합니다. 기본.NET 데이터 형식에 대해 다음 태그를 사용할 수 있습니다.

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

### <a name="can-i-call-methods-from-xaml"></a>XAML에서 메서드를 호출할 수 있나요?

합니다 [ **FactoryMethodDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FactoryMethodDemo) 샘플을 사용 하는 방법에 설명 합니다 `x:FactoryMethod` 개체를 만드는 호출 되는 팩터리 메서드를 지정 하는 요소입니다. 이러한 팩터리 메서드는 공용 및 정적 이어야 합니다 하 고 이전에 정의 된 형식의 개체를 만들어야 합니다. (예를 들어 합니다 [ `Color.FromRgb` ](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)) 메서드는 공용 및 정적 형식의 값을 반환 하기 때문에 한 정하는 `Color`.) 내에서 지정 되는 팩터리 메서드에 대 한 인수 `x:Arguments` 태그입니다.

## <a name="the-xname-attribute"></a>X:name 특성

`x:Name` 특성 개체 이름을 지정 하는 XAML에서 인스턴스화할 수 있습니다. 이러한 이름에 대 한 규칙은 C# 변수 이름은 동일 합니다. 반환 되는 다음을 `InitializeComponent` 생성자에서 호출, 코드 숨김 파일을 해당 XAML 요소에 액세스 하려면 이러한 이름을 참조할 수 있습니다. 이름은 생성된 된 partial 클래스의 private 필드에 XAML 파서에 의해 실제로 변환 됩니다.

합니다 [ **XamlClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlClock) 의 사용법을 보여 줍니다 `x:Name` 되도록 두 개의 코드 숨김 파일을 허용 하도록 `Label` 현재 날짜와 시간으로 업데이트 하는 XAML에 정의 된 요소입니다.

이름이 같은 페이지의 여러 요소에 대해 사용할 수 없습니다. 이 특정 문제를 사용 하는 경우 `OnPlatform` 각 플랫폼에 대 한 명명 된 개체를 병렬 만듭니다. 합니다 [ **PlatformSpecificLabele** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/PlatformSpecificLabels) 샘플에 같은 작업을 수행 하는 더 나은 방법을 보여 줍니다.

## <a name="custom-xaml-based-views"></a>XAML 기반 사용자 지정 보기

XAML 태그의 반복을 방지 하는 방법은 여러 가지가 있습니다. 일반적인 방법 중 하나에서 파생 되는 새 XAML 기반 클래스를 만드는 것 [ `ContentView` ](xref:Xamarin.Forms.ContentView)합니다. 이 기술은에 설명 되어는 [ **ColorViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ColorViewList) 샘플입니다. `ColorView` 클래스에서 파생 됩니다 `ContentView` 해당 이름과 특정 색을 표시 하는 동안는 `ColorViewListPage` 클래스에서 파생 됩니다 `ContentPage` 평소 처럼 및 명시적으로 만듭니다 인스턴스 17 `ColorView`합니다.

에 액세스 하는 `ColorView` XAML의 클래스 라는 일반적으로 다른 XML 네임 스페이스 선언이 필요 `local` 동일한 어셈블리의 클래스에 대 한 합니다.

## <a name="events-and-handlers"></a>이벤트 및 처리기

이벤트, XAML에서 이벤트 처리기에 할당할 수는 있지만 자체적으로 이벤트 처리기의 코드 숨김 파일에 구현 되어야 합니다. [ **XamlKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlKeypad) XAML에서 키패드 사용자 인터페이스를 빌드하는 방법 및 구현 하는 방법을 보여 줍니다는 `Clicked` 코드 숨김 파일에는 처리기.

## <a name="tap-gestures"></a>탭 제스처

모든 `View` 터치 입력을 가져올 개체를 해당 입력에서 이벤트를 생성 합니다. 합니다 `View` 클래스 정의 [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) 컬렉션 속성에서 파생 된 클래스의 인스턴스가 하나 이상 포함할 수 있는 [ `GestureRecognizer` ](xref:Xamarin.Forms.GestureRecognizer)합니다.

합니다 [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) 생성 [ `Tapped` ](xref:Xamarin.Forms.TapGestureRecognizer.Tapped) 이벤트입니다. 합니다 [ **MonkeyTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/MonkeyTap) 프로그램에 연결 하는 방법을 보여 줍니다 `TapGestureRecognizer` 4 개체 `BoxView` 모조 게임을 만들 요소:

[![삼중 스크린샷 monkey tap](images/ch08fg07-small.png "모조 게임")](images/ch08fg07-large.png#lightbox "모조 게임")

하지만 **MonkeyTap** 프로그램 소리를 실제로 필요 합니다. (참조 [다음 장에서](chapter09.md).)

## <a name="related-links"></a>관련 링크

- [8 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf)
- [8 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)
- [8 장 F # 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FS/XamlKeypad)
- [XAML의 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)
