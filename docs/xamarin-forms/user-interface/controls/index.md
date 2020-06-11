---
title: "컨트롤 참조" 설명: "응용 프로그램을 생성 하는 데 사용 되는 모든 사용자 인터페이스 요소에 대 한 설명 Xamarin.Forms 입니다. 이 문서에서는 응용 프로그램의 사용자 인터페이스를 구성 하는 컨트롤 그룹을 나열 Xamarin.Forms 합니다. "
assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F: xamarin-forms author: davidbritch: dabritch:: 08/08/2019-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="controls-reference"></a>컨트롤 참조

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

응용 프로그램의 사용자 인터페이스는 Xamarin.Forms 각 대상 플랫폼의 네이티브 컨트롤에 매핑되는 개체를 구성 합니다. 이렇게 하면 iOS, Android 및 유니버설 Windows 플랫폼에 대 한 플랫폼별 응용 프로그램이 Xamarin.Forms [.NET Standard 라이브러리](~/cross-platform/app-fundamentals/net-standard.md)에 포함 된 코드를 사용할 수 있습니다.

응용 프로그램의 사용자 인터페이스를 만드는 데 사용 되는 네 가지 기본 컨트롤 그룹 Xamarin.Forms 은 다음과 같습니다.

- [**페이지**](pages.md)
- [**레이아웃**](layouts.md)
- [**보기**](views.md)
- [**셀**](cells.md)

Xamarin.Forms페이지는 일반적으로 전체 화면을 차지 합니다. 이 페이지에는 일반적으로 뷰 및 다른 레이아웃이 포함 된 레이아웃이 포함 되어 있습니다. 셀은 및와의 연결에 사용 되는 특수 구성 요소 [`TableView`](views.md#tableview) [`ListView`](views.md#listview) 입니다. 에서 사용자 인터페이스를 빌드하는 데 일반적으로 사용 되는 형식 계층 구조를 보여 주는 클래스 다이어그램은 Xamarin.Forms [ Xamarin.Forms Controls 클래스 계층 구조](~/xamarin-forms/internals/class-hierarchy.md)에서 찾을 수 있습니다.

[**페이지**](pages.md), [**레이아웃**](layouts.md), [**보기**](views.md)및 [**셀**](cells.md)에 대 한 네 가지 문서에서 각 컨트롤 형식은 API 설명서에 대 한 링크, 해당 사용 (있는 경우) 및 하나 이상의 샘플 프로그램 (있는 경우)을 설명 하는 문서를 참조 하세요. 각 컨트롤 형식에는 iOS 및 Android 장치에서 실행 되는 [**양식 갤러리**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) 샘플의 페이지를 보여 주는 스크린샷도 함께 제공 됩니다. 각 스크린샷 아래에는 c # 페이지의 소스 코드에 대 한 링크 (해당 되는 경우)와 xaml 페이지에 대 한 c # 코드 숨겨진 파일이 있습니다.

> [!NOTE]
> 페이지, 레이아웃 및 보기는 클래스에서 파생 `VisualElement` 됩니다. `VisualElement`클래스는 클래스를 파생 하는 데 유용한 다양 한 속성, 메서드 및 이벤트를 제공 합니다. 자세한 내용은 [Visualelement 속성, 메서드 및 이벤트](common-properties.md)를 참조 하세요.

에서 제공 하는 컨트롤 외에도 Xamarin.Forms 타사 컨트롤을 사용할 수 있습니다. 자세한 내용은 [타사 컨트롤](thirdparty.md)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms양식 갤러리 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.FormsControls 클래스 계층 구조](~/xamarin-forms/internals/class-hierarchy.md)
- [API 문서](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
