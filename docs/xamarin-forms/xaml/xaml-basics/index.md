---
title: Xamarin.Forms XAML 기본 사항
description: 이 가이드에서는 모바일 장치용 교차 플랫폼 XAML을 사용하여 시작하는 방법을 설명 합니다. XAML은 개발자가 코드가 아닌 태그를 사용하여 Xamarin.Forms 응용 프로그램의 사용자 인터페이스를 정의할 수 있도록 해줍니다.
ms.prod: xamarin
ms.assetid: 67CC2CD6-D10A-4B14-9696-1D3A410EFFBF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: 0f39eb78d46b6156231a165f950f4698e63fc073
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53055743"
---
# <a name="xamarinforms-xaml-basics"></a>Xamarin.Forms XAML 기본 사항

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)

XAML(eXtensible Application Markup Language)은 개발자가 코드가 아닌 태그를 사용하여 Xamarin.Forms 응용 프로그램의 사용자 인터페이스를 정의할 수 있도록 해줍니다. XAML은 Xamarin.Forms 프로그램에서 반드시 필요한 것은 아니지만 종종 동일한 코드보다 더 간결하고 시각적으로 더 일관되는 경향이 있습니다. XAML은 인기 있는 MVVM (Model View ViewModel) 응용 프로그램 아키텍처를 사용하는데 특히 더 적합 합니다: XAML은 XAML 기반 데이터 바인딩을 통해 ViewModel 코드에 연결 된 View를 정의 합니다.

## <a name="xaml-basics-contents"></a>XAML 기본 콘텐츠

* [개요](#Overview)
* [1부. XAML 시작](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
* [2부. 필수 XAML 구문](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
* [3부. XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
* [4부. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
* [5부. 데이터 바인딩부터 MVVM까지](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)

해당 XAML 기본 사항 문서 외에도 [Creating Mobile Apps with Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) 책의 단원은 다운로드할 수 있습니다 :

[![](images/cover-sml.png "책 표지")](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)

다음과 같이 XAML 주제를 책의 여러 장에서 좀 더 깊이 다룹니다 :

<table style="border:0px; box-shadow:0 0px 0px" cellpadding="0" cellspacing="2" border="0" width="85%">
<tr style="background:#ecf0f1">
  <td style="border:0px;">
    <h4>7장. XAML vs 코드</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf">PDF 다운로드</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md">요약</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>8장. 코드 및 XAML의 조율</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf">PDF 다운로드</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter08.md">요약</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>10장. XAML 태그 확장</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf">PDF 다운로드</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md">요약</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>18장. MVVM</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf">PDF 다운로드</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md">요약</a></td></tr>
</table>

해당 장들은 [무료 다운로드](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)가 가능 합니다.

<a name="Overview" />

## <a name="overview"></a>개요

XAML은 개체를 인스턴스화 및 초기화하고 부모-자식 계층에서 해당 개체를 구성하는 프로그래밍 코드를 대신하도록 Microsoft에서 만든 XML 기반 언어입니다. XAML에는 .NET framework 내에서 여러 가지 기술을 적용하였지만, 자체가 WPF(Windows Presentation Foundation), Silverlight, Windows 런타임 및 UWP(Universal Windows Platform)내에서 사용자 인터페이스 레이아웃을 정의하는 최상의 유틸리티를 포함하고 있습니다.

XAML은 또한 iOS, Android 및 UWP 모바일 장치 용 교차 플랫폼의 고유한 기반 프로그래밍 인터페이스인  Xamarin.Forms의 일부 이기도 합니다. XAML 파일 내에서 Xamarin.Forms 개발자는 사용자 지정 클래스 뿐만 아니라 모든 Xamarin.Forms 뷰, 레이아웃 및 페이지를 사용하여 사용자 인터페이스를 정의할 수 있습니다. XAML 파일은 컴파일 또는 실행 시에 포함될 수 있습니다. 어느 방식이든, XAML 정보는 명명 된 개체 위치가 빌드 시 분석되고, 개체 인스턴스화 및  초기화, 그리고 해당 개체와 프로그래밍 코드 간의 링크를 설정하는 런타임 시 다시 분석 됩니다.

동일한 코드에 비해 XAML은 다음과 같은 여러 장점이 있습니다.

-  XAML은 대개 동일한 코드 보다 더 간결하고 가독성이 좋습니다.
-  Xml에 고유한 부모-자식 계층은 사용자 인터페이스 개체의 부모-자식 계층 구조를 더욱 명확하게 XAML이 모방하도록 해줍니다.
-  XAML 프로그래머가 쉽게 손으로 작성할 수 있지만, 또한 비주얼 디자인 도구를 통해 도구 사용성 및 생성을 대신할 수 있습니다.

물론, 대부분의 태그 언어에 내재되어 있는 한계와 관련 된 다음과 같은 단점도 있습니다.

-  XAML에는 코드를 포함할 수 없습니다. 모든 이벤트 처리기는 코드 파일에 정의 되어야 합니다.
-  XAML에는 반복적인 처리에 대한 반복문을 포함할 수 없습니다. (그러나, 몇 가지 Xamarin.Forms 시각적 개체- [ `ListView` ](xref:Xamarin.Forms.ListView)가 가장 주목 할 만하다 -는 자신의 `ItemsSource` 컬렉션 안에 개체를 기반으로 하는 다수의 자식 요소를 생성할 수 있습니다.)
-  XAML에는 조건부 처리를 포함할 수 없습니다. (단, 데이터 바인딩은 일부 조건부 처리를 허용하는 코드 기반 바인딩 변환기를 참조할 수 있습니다.) 
-  일반적으로 XAML에서는 매개 변수가 없는 생성자 클래스가 아니면 인스턴스화 할 수 없습니다. (그러나 때로 해당 제한을 해결할 수 있는 방법이 있습니다.)
-  일반적으로 XAML에서는 메서드를 호출할 수 없습니다. (역시, 이 제한 사항은 경우에 따라 해결할 수 있습니다.)

아직 Xamarin.Forms 응용 프로그램에서 XAML을 생성하는 비주얼 디자이너는 없습니다. 모든 XAML을 직접 작성해야 하지만 [XAML 미리 보기](~/xamarin-forms/xaml/xaml-previewer.md)가 있습니다. 특히 종종 XAML을 처음 접하는 프로그래머는 명백하게 올바른지를 확인한 후에 자신의 응용 프로그램을 빌드하고 실행하기를 바랄 수 있습니다. XAML 경험이 많은 개발자일지라도 미리보기가 중요하다는 것을 알고 있습니다.

XAML은 기본적으로 XML이지만 XAML에는 몇 가지 고유한 구문 기능이 있습니다. 가장 중요 것은 다음과 같습니다.

- 속성 요소
- 연결된 속성
- 태그 확장

위 기능은 XML 확장이 *아닙*니다. XAML은 완전히 올바른 XML입니다. 하지만 위 XAML 구문 기능은 고유한 방법으로 XML을 사용 합니다. 해당 내용은 MVVM을 구현하기 위해 XAML 사용을 소개하는 결론으로 아래 문서에서 자세히 논의하고 있습니다.

## <a name="requirements"></a>요구 사항

이 문서에서는 Xamarin.Forms 작업에 익숙하다고를 가정합니다. [Xamarin.Forms 소개](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md) 읽기를 강하게 권장드립니다.

이 문서는 또한 XML 네임 스페이스 선언 및 *요소*, *태그* 및 *특성*의 용어 사용의 이해를 포함하여 몇 가지 XML에 익숙하다고 가정합니다.

Xamarin.Forms 및 XML에 익숙한 경우, [1부. XAML 시작하기](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md) 읽기를 시작하십시오.



## <a name="related-links"></a>관련 링크

- [Xaml 샘플](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Xamarin.Forms 소개](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [모바일 앱 책 만들기](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)
