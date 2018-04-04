---
title: 8 장의 요약입니다. 조화의 XAML 및 코드
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5970DEEB-1FC9-4F78-B4F6-D403E16D22ED
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: ffd2d508e99508309ec07c6bc65c8d716427bdff
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-8-code-and-xaml-in-harmony"></a>8 장의 요약입니다. 조화의 XAML 및 코드

이 장에서 XAML을 보다 강력 하 게 탐색 및 특히 어떻게 코드 및 XAML 상호 작용 합니다.

## <a name="passing-arguments"></a>인수 전달

일반적인 경우 XAML에서 인스턴스화하기 클래스는 공용 매개 변수 없는 생성자; 있어야 결과 개체는 속성 설정을 통해 초기화 됩니다. 그러나 두 가지 방법으로 개체 인스턴스화 및 초기화 될 수 있습니다.

이러한 옵션은 범용 기술, MVVM 모델 보기와 관련 하 여 주로 사용 됩니다.

### <a name="constructors-with-arguments"></a>인수가 있는 생성자

[ **ParameteredConstructorDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ParameteredConstructorDemo) 샘플에 사용 하는 방법을 보여 줍니다는 `x:Arguments` 태그 생성자 인수를 지정 해야 합니다. 이 인수는 인수의 형식을 나타내는 요소 태그 구분 해야 합니다. 기본.NET 데이터 형식에 대 한 다음과 같은 태그를 사용할 수 있습니다.

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

### <a name="can-i-call-methods-from-xaml"></a>XAML에서 메서드를 호출할 수 있습니까?

[ **FactoryMethodDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FactoryMethodDemo) 샘플에서는 사용 하는 방법을 보여 줍니다.는 `x:FactoryMethod` 개체를 만들려고 호출 되는 팩터리 메서드를 지정 하는 요소입니다. 공용 및 정적 팩터리 메서드가 이어야 합니다 하 고 정의 된 형식의 개체를 만들어야 합니다. (예를 들어는 [ `Color.FromRgb` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Double/System.Double/System.Double/)) 메서드는 public 및 static 형식의 값을 반환 하기 때문에 한정 `Color`.) 인수는 팩터리 메서드에 내에서 지정 `x:Arguments` 태그입니다.

## <a name="the-xname-attribute"></a>X:name 특성

`x:Name` 특성에 이름을 지정 하는 XAML에서 인스턴스화하기 개체가 수 있게 합니다. 이러한 이름에 대 한 규칙 변수 이름은 C#의 경우와 동일합니다. 반환 되는 다음의 `InitializeComponent` 생성자에서 호출, 코드 숨김 파일에 해당 XAML 요소를 액세스 하려면 이러한 이름을 참조할 수 있습니다. 이름은 생성된 된 partial 클래스의 전용 필드에는 XAML 파서에서 실제로 변환 됩니다.

[ **XamlClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlClock) 샘플의 사용법을 보여줍니다 `x:Name` 코드 숨김 파일을 두 개를 허용 하도록 `Label` 현재 날짜 및 시간으로 업데이트 하는 XAML에서 정의 된 요소입니다.

같은 페이지에 여러 요소에 대 한 같은 이름을 사용할 수 없습니다. 이 특정 문제를 사용 하는 경우 `OnPlatform` 각 플랫폼에 대 한 명명 된 개체를 만드는 병렬 합니다. [ **PlatformSpecificLabele** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/PlatformSpecificLabels) 샘플 더 좋은 방법은 작업을 보여 줍니다.

## <a name="custom-xaml-based-views"></a>XAML 기반 사용자 지정 보기

XAML에서 태그의 반복을 방지 하는 방법은 여러 가지가 있습니다. 파생 되는 새 XAML 기반 클래스를 만드는 일반적인 방법 중 하나는 [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)합니다. 이 방법은에서 설명 되는 [ **ColorViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ColorViewList) 샘플. `ColorView` 클래스에서 파생 `ContentView` 특정 색과 해당 이름, 표시 하는 동안는 `ColorViewListPage` 클래스에서 파생 `ContentPage` 평소와 같이 및 명시적으로의 17 인스턴스를 만드는 `ColorView`합니다.

에 액세스 하는 `ColorView` XAML의 클래스에는 일반적으로 명명 된 다른 XML 네임 스페이스 선언, 필요한 `local` 동일한 어셈블리의 클래스에 대 한 합니다.

## <a name="events-and-handlers"></a>이벤트 및 처리기

Xaml에서는 이벤트 처리기에 이벤트를 지정할 수 있지만 자체의 이벤트 처리기 코드 숨김 파일에 구현 되어야 합니다. [ **XamlKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlKeypad) XAML에서 키패드 사용자 인터페이스를 작성 하는 방법 및 구현 하는 방법을 보여 줍니다.는 `Clicked` 처리기에 코드 숨김 파일입니다.

## <a name="tap-gestures"></a>제스처를 탭 합니다.

모든 `View` 개체를 터치식 입력 가져오고 해당 입력에서 이벤트를 생성할 수 있습니다. `View` 클래스 정의 [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) 컬렉션 속성에서 파생 된 클래스의 인스턴스가 하나 이상 포함 될 수 있는 [ `GestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)합니다.

[ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) 생성 [ `Tapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.TapGestureRecognizer.Tapped/) 이벤트입니다. [ **MonkeyTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/MonkeyTap) 프로그램에서는 연결 하는 방법을 보여 줍니다. `TapGestureRecognizer` 4로 개체 `BoxView` 요소를 사용 하는 모조 게임 만들기:

[![원숭이 탭의 삼중 스크린 샷](images/ch08fg07-small.png "모방 게임")](images/ch08fg07-large.png#lightbox "모방 게임")

하지만 **MonkeyTap** 프로그램 소리를 반드시 필요 합니다. (참조 [다음 장에서](chapter09.md).)



## <a name="related-links"></a>관련 링크

- [8 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf)
- [8 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)
- [8 장 F # 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FS/XamlKeypad)
