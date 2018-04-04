---
title: Xamarin.Forms XAML 기본 사항
description: 모바일 장치에 대 한 플랫폼 간 태그 시작
ms.prod: xamarin
ms.assetid: 67CC2CD6-D10A-4B14-9696-1D3A410EFFBF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 991d928c2c58f05098a41c84aba295a31636ab96
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinforms-xaml-basics"></a>Xamarin.Forms XAML 기본 사항

XAML(Extensible Application Markup Language)을 통해 개발자가 코드가 아닌 태그를 사용하여 Xamarin.Forms 응용 프로그램에서 사용자 인터페이스를 정의할 수 있습니다. XAML은 Xamarin.Forms 프로그램에 필요 하지 않지만 간결 하 고 해당 하는 코드 보다 시각적으로 일관 된 하 고 잠재적으로 높은 합니다. XAML은 인기 있는 MVVM (모델-뷰-ViewModel) 응용 프로그램 아키텍처와 함께 사용 하는 데 특히 적합: XAML XAML 기반 데이터 바인딩을 통해 ViewModel 코드에 연결 된 뷰를 정의 합니다.

## <a name="xaml-basics-contents"></a>XAML 기본 사항 내용

* [개요](#Overview)
* [1부. XAML 시작](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
* [2부. 필수 XAML 구문](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
* [3부. XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
* [4부. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
* [5부. MVVM에 데이터 바인딩](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)

다음 XAML 기본 사항 문서 외에도 책의 장을 다운로드할 수 있습니다 [Xamarin.Forms 사용 하 여 모바일 앱 만들기](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md):

[![](images/cover-sml.png "책 표지")](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)

책의 많은 장에 더 자세히 다루는 XAML 항목은 포함 하 여:

<table style="border:0px; box-shadow:0 0px 0px" cellpadding="0" cellspacing="2" border="0" width="85%">
<tr style="background:#ecf0f1">
  <td style="border:0px;">
    <h4>7 장 합니다. XAML vs입니다. 코드</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf">PDF 다운로드</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md">요약</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>8 장 합니다. 조화의 XAML 및 코드</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf">PDF 다운로드</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter08.md">요약</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>장 10입니다. XAML 태그 확장</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf">PDF 다운로드</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md">요약</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>18 장 합니다. MVVM</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf">PDF 다운로드</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md">요약</a></td></tr>
</table>

이 장에서 수 [무료로 다운로드할](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)합니다.

<a name="Overview" />

## <a name="overview"></a>개요

XAML은 인스턴스화 및 개체, 초기화 및 부모-자식 계층에서 이러한 개체를 구성 하기 위한 코드를 프로그래밍 하는 대신 Microsoft에서 생성 하는 XML 기반 언어입니다. .NET framework 내에서 여러 가지 기술을 사용 하도록 XAML 적용 된에 있지만 가장 큰 활용성 레이아웃 (WPF (Windows Presentation Foundation), Silverlight, Windows 런타임 및 유니버설 Windows 내에서 사용자 인터페이스의 정의에서 발견 되는 플랫폼 (UWP)입니다.

XAML Xamarin.Forms에는 플랫폼 간 고유 하 게 기반 프로그래밍 인터페이스 iOS, Android, 및 UWP의 일부 이기도 모바일 장치입니다. XAML 파일 내에서 Xamarin.Forms 개발자는 사용자 지정도 클래스로 모든 Xamarin.Forms 뷰, 레이아웃 및 페이지를 사용 하 여 사용자 인터페이스를 정의할 수 있습니다. XAML 파일 컴파일된 또는 실행 파일에 포함 될 수 있습니다. 두 가지 경우 모두 XAML 정보가 명명 된 개체를 찾을 빌드 시와 인스턴스화 및 개체를 초기화 하 고 이러한 개체와 프로그래밍 코드 간의 연결을 설정 하려면 런타임에 다시 분석 됩니다.

XAML에 해당 하는 코드의 몇 가지 이점이 있습니다.

-  XAML에는 대개 더 간결 하 고 해당 하는 코드 보다 쉽게 읽을 수 있습니다.
-  XML에 내재 된 부모-자식 계층에는 사용자 인터페이스 개체의 부모-자식 계층 구조를 큰 선명도를 모방 하기 위해 XAML 수 있습니다.
-  XAML 쉽게 직접 작성 하 여 프로그래머 수는 있지만 높은 성과 비주얼 디자인 도구에서 생성 된 인계 합니다.

물론, 제한 사항 태그 언어에는 내장 함수에 주로 관련 단점 있습니다.

-  XAML 코드를 포함할 수 없습니다. 모든 이벤트 처리기 코드 파일에 정의 되어야 합니다.
-  XAML 반복 되는 처리에 대 한 루프를 포함할 수 없습니다. 그러나 (몇 가지 Xamarin.Forms 시각적 개체-특히 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) -에 개체를 기반으로 하는 여러 자식 요소를 생성할 수 해당 `ItemsSource` 컬렉션입니다.)
-  하지만 XAML (, 데이터 바인딩 참조할 수 있습니다 효과적으로 일부 조건부 처리를 허용 하는 코드 기반 바인딩을 변환기.)으로 조건부 처리를 포함할 수 없습니다.
-  일반적으로 XAML 매개 변수가 없는 생성자를 정의 하지 않은 클래스를 인스턴스화할 수 없습니다. 그러나 (방법이 때로는이 제한 해결 합니다.)
-  일반적으로 XAML 메서드를 호출할 수 없습니다. (다시이 제한 사항은 경우에 따라 해결할 수 있습니다.)

아직 하지 않은 비주얼 디자이너 Xamarin.Forms는 응용 프로그램에서 XAML을 생성 합니다. 모든 XAML를 직접 작성 해야 하지만는 [XAML 미리 보기](~/xamarin-forms/xaml/xaml-previewer.md)합니다. 처음 접하는 프로그래머 XAML 수 자주 빌드하고 분명히 올바르지 않을 수 있는 모든 후 특히 응용 프로그램을 실행 하려고 합니다. 에 개발자가 XAML에서 경험으로 실험 가치는 알고 있습니다.

XAML은 XML, 기본적으로 XAML에는 몇 가지 고유한 구문 기능 하지만 합니다. 가장 중요 한 요인은 다음과 같습니다.

- 속성 요소
- 연결 된 속성
- 태그 확장

이러한 기능은 *하지* XML 확장입니다. XAML은 xml이 유효한 전체 XML입니다. 하지만 이러한 XAML 구문 기능 고유한 방법으로 XML을 사용 합니다. 설명 아래, 문서에 자세히는 XAML을 사용 하 여 MVVM 구현 하기 위한 소개로 끝납니다.

## <a name="requirements"></a>요구 사항

이 문서에 대 한 작업 지식이 Xamarin.Forms로 가정합니다. 읽는 [Xamarin.Forms 소개](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md) 것이 좋습니다.

또한이 문서에 XML 네임 스페이스 선언 및 용어 사용 이해를 비롯 한 XML 잘 알고 있다고 가정 *요소*, *태그*, 및 *특성*합니다.

Xamarin.Forms 및 XML에 잘 알고 있는 경우 읽기를 시작 [1 부입니다. XAML 시작](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)합니다.



## <a name="related-links"></a>관련 링크

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Xamarin.Forms 소개](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [모바일 앱 책 만들기](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)
