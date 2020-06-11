---
제목: " Xamarin.Forms XAML 기본 사항" 설명: "이 가이드에서는 모바일 장치에 대 한 플랫폼 간 XAML을 시작 하는 방법을 설명 합니다. 개발자는 XAML을 Xamarin.Forms 사용 하 여 코드 대신 태그를 사용 하 여 응용 프로그램에서 사용자 인터페이스를 정의할 수 있습니다. "
ms. prod: xamarin ms. custom: video assetid: 67CC2CD6-D10A-4B14-9696-1D3A410EFFBF: xamarin-forms author: davidbritch: dabritch:: 10/25/2017-loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-xaml-basics"></a>Xamarin.FormsXAML 기본 사항

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

XAML (eXtensible Application Markup Language)은 개체를 인스턴스화하고 초기화 하 고 부모-자식 계층으로 개체를 구성 하기 위한 프로그래밍 코드 대신 Microsoft에서 만든 XML 기반 언어입니다. XAML은 .NET framework 내의 여러 기술에 맞게 조정 되었지만 Windows Presentation Foundation (WPF), Silverlight, Windows 런타임 및 UWP (유니버설 Windows 플랫폼) 내에서 사용자 인터페이스의 레이아웃을 정의 하는 가장 큰 유틸리티를 찾았습니다.

개발자는 XAML을 Xamarin.Forms 사용 하 여 코드 대신 태그를 사용 하 여 응용 프로그램에서 사용자 인터페이스를 정의할 수 있습니다. XAML은 프로그램에서 반드시 필요한 것은 Xamarin.Forms 아니지만, 동일한 코드에 비해 더 간결 하 고 시각적으로 일관 되며 잠재적으로 도구를 제공할 수 있는 경우가 많습니다. XAML은 인기 있는 MVVM (모델-뷰-ViewModel) 응용 프로그램 아키텍처와 함께 사용 하는 데 적합 합니다. xaml은 XAML 기반 데이터 바인딩을 통해 ViewModel 코드에 연결 된 뷰를 정의 합니다.

개발자는 XAML 파일 내에서 Xamarin.Forms 모든 Xamarin.Forms 뷰, 레이아웃 및 페이지 뿐만 아니라 사용자 지정 클래스를 사용 하 여 사용자 인터페이스를 정의할 수 있습니다. XAML 파일을 컴파일하거나 실행 파일에 포함할 수 있습니다. 어떤 경우 든 빌드 시 XAML 정보를 구문 분석 하 여 명명 된 개체를 찾고 런타임에 다시 런타임에 개체를 인스턴스화하고 초기화 하 고 이러한 개체와 프로그래밍 코드 간의 링크를 설정 합니다.

XAML은 동등한 코드에 비해 여러 가지 장점이 있습니다.

- XAML은 동등한 코드 보다 간결 하 고 읽기 쉽습니다.
- XML에 내재 된 부모-자식 계층 구조를 통해 XAML은 사용자 인터페이스 개체의 부모-자식 계층 구조를 보다 잘 이해할 수 있습니다.
- 프로그래머는 XAML을 쉽게 직접 작성할 수 있지만 시각적 디자인 도구를 통해 도구를 통해 도구를 만들고 생성할 수도 있습니다.

주로 태그 언어에 내장 된 제한과 관련 된 단점이 있습니다.

- XAML은 코드를 포함할 수 없습니다. 모든 이벤트 처리기는 코드 파일에 정의 되어야 합니다.
- XAML은 반복적인 처리를 위한 루프를 포함할 수 없습니다. 그러나 Xamarin.Forms 대부분의 시각적 개체 (특히)는 [`ListView`](xref:Xamarin.Forms.ListView) 컬렉션의 개체를 기반으로 하 여 여러 자식을 생성할 수 있습니다 `ItemsSource` .
- XAML은 조건부 처리를 포함할 수 없습니다. 그러나 데이터 바인딩은 일부 조건부 처리를 효과적으로 허용 하는 코드 기반 바인딩 변환기를 참조할 수 있습니다.
- XAML은 일반적으로 매개 변수가 없는 생성자를 정의 하지 않는 클래스를 인스턴스화할 수 없습니다. 그러나 이러한 제한을 해결 하는 방법도 있습니다.
- XAML은 일반적으로 메서드를 호출할 수 없습니다. (이 제한 사항을 해결 하는 경우도 있습니다.)

응용 프로그램에서 XAML을 생성 하기 위한 비주얼 디자이너는 아직 없습니다 Xamarin.Forms . 모든 XAML은 직접 작성 되어야 하지만 [Xaml 미리 보기](~/xamarin-forms/xaml/xaml-previewer/index.md)는 있습니다. XAML을 처음 접하는 프로그래머는 응용 프로그램을 자주 빌드하고 실행 하는 것이 좋습니다 (특히 올바른 것은 아닐 수 있음). XAML에서 많은 경험을 가진 개발자 라도 실험이 더욱 생산적인 것을 알고 있습니다.

XAML은 기본적으로 XML 이지만 XAML에는 몇 가지 고유한 구문 기능이 있습니다. 가장 중요 한 사항은 다음과 같습니다.

- 속성 요소
- 연결된 속성
- 태그 확장

이러한 기능은 XML 확장이 *아닙니다* . XAML은 완전히 올바른 XML입니다. 그러나 이러한 XAML 구문 기능은 고유한 방식으로 XML을 사용 합니다. 이러한 항목은 아래 문서에서 자세히 설명 합니다 .이 문서에서는 MVVM 구현에 XAML을 사용 하는 방법을 소개 합니다.

## <a name="requirements"></a>요구 사항

이 문서에서는에 대해 잘 알고 있다고 가정 Xamarin.Forms 합니다. 또한 xml 네임 스페이스 선언 사용에 대 한 이해를 비롯 하 여 XML에 대해 잘 알고 있는 것 *element*으로 가정 *tag* *합니다.*

및 XML에 대해 잘 알고 있는 경우 Xamarin.Forms 1 부를 읽도록 시작 [합니다. XAML 시작](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)

## <a name="related-links"></a>관련 링크

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [Mobile Apps 책 만들기](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)
- [Xamarin.Forms 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Series/Xamarin-101/XamarinForms-UI-with-XAML-5-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
