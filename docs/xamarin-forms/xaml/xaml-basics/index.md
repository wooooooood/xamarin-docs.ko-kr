---
title: Xamarin.Forms XAML 기본 사항
description: 이 가이드에서는 모바일 장치용 플랫폼 간 XAML을 사용 하 여 시작 하는 방법을 설명 합니다. XAML은 코드가 아닌 태그를 사용 하 여 Xamarin.Forms 응용 프로그램에서 사용자 인터페이스를 정의 하는 개발자를 수 있습니다.
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

XAML(Extensible Application Markup Language)을 통해 개발자가 코드가 아닌 태그를 사용하여 Xamarin.Forms 응용 프로그램에서 사용자 인터페이스를 정의할 수 있습니다. XAML Xamarin.Forms 프로그램에서 필요 하지 않습니다 하지만 것이 간결 하 고 해당 하는 코드 보다 일관 된 시각적으로 화할 수 있습니다. XAML은 인기 있는 MVVM (Model View ViewModel) 응용 프로그램 아키텍처를 사용 하는 데 특히 적합 합니다: XAML XAML 기반 데이터 바인딩을 통해 ViewModel 코드에 연결 된 뷰를 정의 합니다.

## <a name="xaml-basics-contents"></a>XAML 기본 사항 콘텐츠

* [개요](#Overview)
* [1부. XAML 시작](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
* [2부. 필수 XAML 구문](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
* [3부. XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
* [4부. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
* [5부. MVVM에 데이터 바인딩](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)

다음 XAML 기본 사항 문서 외에도 책의 단원은 다운로드할 수 있습니다 [Creating Mobile Apps with Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md):

[![](images/cover-sml.png "책 표지")](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)

XAML 내용을 책의 여러 장에 있는 좀 더 깊이 다룹니다 포함:

<table style="border:0px; box-shadow:0 0px 0px" cellpadding="0" cellspacing="2" border="0" width="85%">
<tr style="background:#ecf0f1">
  <td style="border:0px;">
    <h4>7 장입니다. XAML vs입니다. 코드</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf">PDF 다운로드</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md">요약</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>8 장입니다. 코드 및 XAML의 조율</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf">PDF 다운로드</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter08.md">요약</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>10 장입니다. XAML 태그 확장</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf">PDF 다운로드</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md">요약</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>18 장입니다. MVVM</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf">PDF 다운로드</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md">요약</a></td></tr>
</table>

이 장에 수 [무료로 다운로드할](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)합니다.

<a name="Overview" />

## <a name="overview"></a>개요

XAML은 프로그래밍 코드를 인스턴스화하고 개체를 초기화 하 고 부모-자식 계층에서 이러한 개체를 구성 하는 대신 Microsoft에서 만든 XML 기반 언어입니다. .NET framework 내에서 여러 가지 기술을 XAML이 수정 되었습니다 하지만 가장 큰 활용성 (WPF (Windows Presentation Foundation), Silverlight, Windows 런타임 및 유니버설 Windows 내에서 사용자 인터페이스 레이아웃 정의에서 발견 되는 플랫폼 (UWP)입니다.

XAML에서는 플랫폼 간 고유 하 게 기반 프로그래밍 인터페이스를 iOS, Android 및 UWP 용 Xamarin.Forms의 일부 이기도 모바일 장치. XAML 파일에서 Xamarin.Forms 개발자 및 사용자 지정 클래스로 모든 Xamarin.Forms 뷰, 레이아웃 및 페이지를 사용 하 여 사용자 인터페이스를 정의할 수 있습니다. XAML 파일 컴파일 또는 실행 파일에 포함 된 수 수 있습니다. 어느 XAML 정보가 명명 된 개체를 찾으려고 빌드 시 및 인스턴스화 및 개체를 초기화 하 고 이러한 개체 및 프로그래밍 코드 간의 링크를 설정 하는 런타임 시 다시 분석 됩니다.

XAML에 해당 하는 코드를 통해 여러 장점이 있습니다.

-  XAML에는 대개 더 간결 하 고 해당 하는 코드 보다 읽기 어렵습니다.
-  Xml은 부모-자식 계층에는 큰 선명도 사용 하 여 사용자 인터페이스 개체의 부모-자식 계층 구조를 모방 하기 위해 XAML 수 있습니다.
-  XAML 프로그래머가, 필기 쉽게 수는 있지만 화할 고 비주얼 디자인 도구에서 생성 된 사실입니다.

물론, 태그 언어에 내장 된 제한과 관련 된 대부분 단점이 있습니다.

-  XAML 코드를 포함할 수 없습니다. 모든 이벤트 처리기 코드 파일에서 정의 되어야 합니다.
-  XAML의 반복적인 처리에 대 한 루프를 포함할 수 없습니다. 그러나 (몇 가지 Xamarin.Forms 시각적 개체-가장 주목할 만한 [ `ListView` ](xref:Xamarin.Forms.ListView) -에서 개체를 기반으로 하는 여러 자식 요소를 생성할 수 해당 `ItemsSource` 컬렉션입니다.)
-  XAML (단, 데이터 바인딩 참조할 수 있습니다 효과적으로 일부 조건부 처리를 허용 하는 코드 기반 바인딩 변환기.) 조건부 처리를 포함할 수 없습니다.
-  일반적으로 XAML 매개 변수가 없는 생성자를 정의 하지 않은 클래스를 인스턴스화할 수 없습니다. 그러나 (방법이 때때로이 제한 해결할.)
-  일반적으로 XAML 메서드를 호출할 수 없습니다. (다시이 제한 사항은 경우에 따라 확장할 수 있습니다.)

아직 하지 않은 비주얼 디자이너 Xamarin.Forms 응용 프로그램에서 XAML을 생성 합니다. 모든 XAML을 직접 작성 해야 하지만 한 [XAML 미리 보기](~/xamarin-forms/xaml/xaml-previewer.md)합니다. 처음 접하는 프로그래머 XAML 자주 빌드하고 분명히 올바르지 않을 수 있는 모든 특히 후 해당 응용 프로그램을 실행 하려고 합니다. XAML에서 경험으로도 개발자는 실험 회원은 알고 있습니다.

XAML은 XML을 기본적으로 XAML에는 몇 가지 고유한 구문 기능이 있지만. 가장 중요 한 요인은 다음과 같습니다.

- 속성 요소
- 연결 된 속성
- 태그 확장

이러한 기능은 *되지* XML 확장 합니다. XAML은 완전히 올바른 XML입니다. 하지만 XAML 구문은 기능이 고유한 방법으로 XML을 사용 합니다. 설명 아래 문서에서 자세히는 XAML을 사용 하 여 MVVM을 구현 하는 것에 대 한 소개로 끝납니다.

## <a name="requirements"></a>요구 사항

이 문서에서는 Xamarin.Forms 작업에 익숙하다고를 가정합니다. 읽는 [An Introduction to Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md) 것이 좋습니다.

이 문서는 또한 XML 네임 스페이스 선언 및 사용 이해를 비롯 한 XML에 어느 정도 익숙하다고 가정 *요소*하십시오 *태그*, 및 *특성*합니다.

Xamarin.Forms 및 XML에 익숙한 경우 읽기를 시작할 [1 부입니다. XAML 시작](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)합니다.



## <a name="related-links"></a>관련 링크

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Xamarin.Forms 소개](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [모바일 앱 책 만들기](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)
