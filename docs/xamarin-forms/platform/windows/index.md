---
제목: "Windows 플랫폼 기능" 설명: "이 문서에서는에서 사용할 수 있는 Windows 플랫폼 지원을 설명 Xamarin.Forms 합니다."
assetid: F6EA9E49-FB3E-442F-AF13-B7AD0C80D11F: xamarin-forms author: davidbritch: dabritch:: 01/16/2020-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="windows-platform-features"></a>Windows 플랫폼 기능

Xamarin.FormsWindows 플랫폼용 응용 프로그램을 개발 하려면 Visual Studio가 필요 합니다. [지원 되는 플랫폼 페이지](~/get-started/supported-platforms.md) 에는 필수 구성 요소에 대 한 자세한 정보가 포함 되어 있습니다.

![](images/allhanselman.png "Xamarin.Forms Applications Running on Windows")

## <a name="platform-specifics"></a>플랫폼 사양

플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다.

다음 플랫폼별 기능은 Xamarin.Forms 유니버설 Windows 플랫폼 (UWP)의 보기, 페이지 및 레이아웃에 대해 제공 됩니다.

- 에 대 한 액세스 키를 설정 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 합니다. 자세한 내용은 [Windows의 Visualelement 액세스 키](visualelement-access-keys.md)를 참조 하세요.
- 지원 되는에서 레거시 색 모드를 사용 하지 않도록 설정 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 합니다. 자세한 내용은 [Windows의 Visualelement 레거시 색 모드](legacy-color-mode.md)를 참조 하세요.

UWP의 뷰에는 다음과 같은 플랫폼별 기능이 제공 됩니다 Xamarin.Forms .

- , 및 인스턴스의 텍스트 콘텐츠에서 읽기 순서 [`Entry`](xref:Xamarin.Forms.Entry) 를 검색 [`Editor`](xref:Xamarin.Forms.Editor) [`Label`](xref:Xamarin.Forms.Label) 합니다. 자세한 내용은 [InputView 읽기용 Order On Windows](inputview-reading-order.md)을 참조 하세요.
- 에서 탭 제스처 지원 사용 [`ListView`](xref:Xamarin.Forms.ListView) . 자세한 내용은 [ListView SelectionMode On Windows](listview-selectionmode.md)을 참조 하세요.
- 변경할의 끌어오기 방향을 사용 하도록 설정 `RefreshView` 합니다. 자세한 내용은 [Windows의 Refreshview 끌어오기 방향](refreshview-pulldirection.md)을 참조 하세요.
- [`SearchBar`](xref:Xamarin.Forms.SearchBar)맞춤법 검사 엔진과 상호 작용할 수 있도록 설정 자세한 내용은 [Windows의 Searchbar Spell Check](searchbar-spell-check.md)를 참조 하세요.
- 를 사용 [`WebView`](xref:Xamarin.Forms.WebView) 하 여 UWP 메시지 대화 상자에서 JavaScript 경고 표시 자세한 내용은 [Windows의 웹 보기 JavaScript 경고](webview-javascript-alert.md)를 참조 하세요.

UWP의 페이지에 대해 다음과 같은 플랫폼별 기능이 제공 됩니다 Xamarin.Forms .

- [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)탐색 모음 축소 자세한 내용은 [Windows의 MasterDetailPage 탐색 모음](masterdetailpage-navigation-bar.md)을 참조 하세요.
- 도구 모음 배치 옵션을 설정 합니다. 자세한 내용은 [Windows의 페이지 도구 모음 배치](page-toolbar-placement.md)를 참조 하세요.
- 도구 모음에 페이지 아이콘이 표시 되도록 설정 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 자세한 내용은 [Windows의 TabbedPage 아이콘](tabbedpage-icons.md)을 참조하세요.

Xamarin.FormsUWP의 클래스에 대해 다음과 같은 플랫폼별 기능이 제공 됩니다 [`Application`](xref:Xamarin.Forms.Application) .

- 이미지 자산이 로드 되는 프로젝트의 디렉터리를 지정 합니다. 자세한 내용은 [Windows의 기본 이미지 디렉터리](default-image-directory.md)를 참조 하십시오.

## <a name="platform-support"></a>플랫폼 지원

Xamarin.FormsVisual Studio에서 사용할 수 있는 템플릿에는 UWP (유니버설 Windows 플랫폼) 프로젝트가 포함 되어 있습니다.

> [!NOTE]
> Xamarin.Forms1.x와 2.x는 _Windows Phone 8 개의 Silverlight_, _Windows Phone 8.1_및 _Windows 8.1_ 응용 프로그램 개발을 지원 합니다. 그러나 이러한 프로젝트 형식은 더 이상 사용 되지 않습니다.

## <a name="getting-started"></a>시작

Visual Studio에서 **파일 > 새 > 프로젝트** 로 이동 하 고, **플랫폼 간 > 빈 앱 ( Xamarin.Forms )** 템플릿 중 하나를 선택 하 여 시작 합니다.

이전 Xamarin.Forms 솔루션 또는 macOS에서 만든 솔루션에는 위에 나열 된 모든 Windows 프로젝트가 없지만 수동으로 추가 해야 합니다. 대상으로 하려는 Windows 플랫폼이 솔루션에 아직 없는 경우 [설치 지침](installation/index.md) 을 방문 하 여 원하는 windows 프로젝트 형식/s를 추가 합니다.

## <a name="samples"></a>샘플

Charles Petzold의 책에 대 한 [모든 샘플은](https://github.com/xamarin/xamarin-forms-book-preview-2) 유니버설 Windows 플랫폼 (Windows 10) 프로젝트를 포함 [*하는 Xamarin.Forms Mobile Apps를 만듭니다*](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) .

["Scott Hanselman" demo 앱](https://github.com/jamesmontemagno/Hanselman.Forms) 은 개별적으로 사용할 수 있으며, Apple Watch 및 Android 출시 프로젝트 (각각 Xamarin.ios 및 xamarin.ios를 사용 하 여 Xamarin.Forms 해당 플랫폼에서 실행 되지 않음)도 포함 합니다.

## <a name="related-links"></a>관련 링크

- [Windows 프로젝트 설정](~/xamarin-forms/platform/windows/installation/index.md)
