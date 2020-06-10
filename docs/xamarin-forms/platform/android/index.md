---
제목: "Android 플랫폼 기능" 설명: "이 문서에서는 응용 프로그램에 Android 관련 기능을 추가 하는 방법을 설명 Xamarin.Forms 합니다."
assetid: E24168F3-0138-4814-86EA-B467F6B8A545: xamarin-forms author: davidbritch: dabritch:: 12/11/2019-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="android-platform-features"></a>Android 플랫폼 기능

Xamarin.FormsAndroid 용 응용 프로그램을 개발 하려면 Visual Studio가 필요 합니다. [지원 되는 플랫폼 페이지](~/get-started/supported-platforms.md) 에는 필수 구성 요소에 대 한 자세한 정보가 포함 되어 있습니다.

## <a name="platform-specifics"></a>플랫폼 사양

플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다.

Xamarin.FormsAndroid에서 보기, 페이지 및 레이아웃에 대해 다음과 같은 플랫폼별 기능이 제공 됩니다.

- 그리기 순서를 결정 하기 위해 시각적 요소의 Z 순서를 제어 합니다. 자세한 내용은 [Android에서 Visualelement 권한 상승](visualelement-elevation.md)을 참조 하세요.
- 지원 되는에서 레거시 색 모드를 사용 하지 않도록 설정 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 합니다. 자세한 내용은 [Android의 Visualelement 레거시 색 모드](legacy-color-mode.md)를 참조 하세요.

Android에서 보기에 대해 다음과 같은 플랫폼 관련 기능이 제공 됩니다 Xamarin.Forms .

- Android 단추의 기본 패딩 및 그림자 값을 사용 합니다. 자세한 내용은 [Android의 단추 패딩 및 그림자](button-padding-shadow.md)를 참조 하세요.
- 에 대 한 소프트 키보드의 입력 방법 편집기 옵션을 설정 합니다 [`Entry`](xref:Xamarin.Forms.Entry) . 자세한 내용은 [Android의 입력 입력 방법 편집기 옵션](entry-ime-options.md)을 참조 하세요.
- 에서 그림자를 사용 하도록 설정 `ImageButton` 합니다. 자세한 내용은 [Android의 ImageButton 드롭 그림자](imagebutton-drop-shadow.md)를 참조 하세요.
- 에서 빠른 스크롤 사용에 [`ListView`](xref:Xamarin.Forms.ListView) 대 한 자세한 내용은 [ListView의 빠른 스크롤 (Android](listview-fast-scrolling.md))을 참조 하세요.
- 를 열 때 사용 되는 전환을 제어 `SwipeView` 합니다. 자세한 내용은 [SwipeView 살짝 밀기 전환 모드](swipeview-swipetransitionmode.md)를 참조 하세요.
- 에서 혼합 된 콘텐츠를 표시할 수 있는지 여부를 제어 [`WebView`](xref:Xamarin.Forms.WebView) 합니다. 자세한 내용은 [Android의 웹 보기 혼합 콘텐츠](webview-mixed-content.md)를 참조 하세요.
- 에서 확대/축소를 사용 하도록 설정 [`WebView`](xref:Xamarin.Forms.WebView) 합니다. 자세한 내용은 [Android에서 웹 보기 확대/축소](webview-zoom-controls.md)를 참조 하세요.

Android의 셀에 대해 다음과 같은 플랫폼 관련 기능이 제공 됩니다 Xamarin.Forms .

- 컨텍스트 작업 레거시 모드를 사용 하도록 설정 하면 [`ViewCell`](xref:Xamarin.Forms.ViewCell) 에서 선택한 항목이 변경 될 때 컨텍스트 작업 메뉴가 업데이트 되지 않습니다 [`ListView`](xref:Xamarin.Forms.ListView) . 자세한 내용은 [Android의 Viewcell 컨텍스트 작업](viewcell-context-actions.md)을 참조 하세요.

Android의 페이지에 대해 다음과 같은 플랫폼 관련 기능이 제공 됩니다 Xamarin.Forms .

- 에 탐색 모음의 높이를 설정 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 합니다. 자세한 내용은 [Android의 Navigationpage Bar Height](navigationpage-bar-height.md)를 참조 하세요.
- 에서 페이지를 탐색할 때 전환 애니메이션 사용 안 함 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 자세한 내용은 [TabbedPage 페이지 전환 애니메이션 (Android](tabbedpage-transition-animations.md))을 참조 하세요.
- 의 페이지 간에 살짝 밀기를 사용 하도록 설정 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 합니다. 자세한 내용은 [TabbedPage Page 살짝 밀기 On Android](tabbedpage-page-swiping.md)을 참조 하세요.
- 에서 도구 모음 배치 및 색을 설정 합니다 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . 자세한 내용은 [TabbedPage Toolbar Placement And Color In Android](tabbedpage-toolbar-placement-color.md)항목을 참조 하세요.

Xamarin.FormsAndroid의 클래스에 대해 다음과 같은 플랫폼별 기능이 제공 됩니다 [`Application`](xref:Xamarin.Forms.Application) .

- 소프트 키보드의 운영 모드 설정 자세한 내용은 [Android의 소프트 키보드 입력 모드](soft-keyboard-input-mode.md)를 참조 하세요.
- AppCompat을 [`Disappearing`](xref:Xamarin.Forms.Page.Appearing) [`Appearing`](xref:Xamarin.Forms.Page.Appearing) 사용 하는 응용 프로그램의 경우 각각 일시 중지 및 재개에서 및 페이지 수명 주기 이벤트를 사용 하지 않도록 설정 자세한 내용은 [Android의 페이지 수명 주기 이벤트](page-lifecycle-events.md)를 참조 하세요.

## <a name="platform-support"></a>플랫폼 지원

원래 기본 Xamarin.Forms android 프로젝트는 android 5.0 이전에 일반적인 컨트롤 렌더링의 이전 스타일을 사용 했습니다. 템플릿을 사용 하 여 빌드한 응용 프로그램은 기본 `FormsApplicationActivity` 활동의 기본 클래스로 사용 됩니다.

## <a name="material-design-via-appcompat"></a>AppCompat을 통한 재질 디자인

Xamarin.Forms이제 Android 프로젝트를 `FormsAppCompatActivity` 주 활동의 기본 클래스로 사용 합니다. 이 클래스는 Android에서 제공 하는 **AppCompat** 기능을 사용 하 여 재질 디자인 테마를 구현 합니다.

Android 프로젝트에 재질 디자인 테마를 추가 하려면 Xamarin.Forms [AppCompat 지원에 대 한 설치 지침](appcompat-material-design.md) 을 따르세요.

다음은 기본값을 사용 하는 **Todo** 샘플입니다 `FormsApplicationActivity` .

[![](images/before-appcompat-sml.png "Todo Sample Application Without AppCompat")](images/before-appcompat.png#lightbox "Todo Sample Application Without AppCompat")

이 코드는를 사용 하 `FormsAppCompatActivity` 고 추가 테마 정보를 추가 하 여 프로젝트를 업그레이드 한 후의 코드와 동일 합니다.

[![](images/post-appcompat-sml.png "Todo Sample Application With AppCompat and Theming")](images/post-appcompat.png#lightbox "Todo Sample Application With AppCompat and Theming")

> [!NOTE]
> 를 사용 하 `FormsAppCompatActivity` 는 경우 [일부 Android 사용자 지정 렌더러의 기본 클래스](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md) 는 다릅니다.

## <a name="androidx-migration"></a>AndroidX 마이그레이션

AndroidX는 Android 지원 라이브러리를 대체 합니다. AndroidX 및 AndroidX 라이브러리를 사용 하도록 앱을 마이그레이션하는 방법에 대 한 자세한 내용은 Xamarin.Forms [의 Xamarin.Forms androidx 마이그레이션 ](~/xamarin-forms/platform/android/androidx-migration.md)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [재질 디자인 지원 추가](appcompat-material-design.md)
