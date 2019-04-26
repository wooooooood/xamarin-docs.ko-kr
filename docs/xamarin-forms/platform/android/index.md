---
title: Android 플랫폼 기능
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에 Android 관련 기능을 추가 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/07/2018
ms.openlocfilehash: 3bd606ae87c524202fd5ea7c141223042e90ec1a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61363047"
---
# <a name="android-platform-features"></a>Android 플랫폼 기능

Android 용 Xamarin.Forms 응용 프로그램 개발에 Visual Studio가 필요 합니다. 합니다 [요구 사항 페이지](~/get-started/requirements.md) 필수 조건에 대 한 자세한 정보를 포함 합니다.

## <a name="platform-specifics"></a>플랫폼별

플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다.

Xamarin.Forms 뷰, 페이지 및 Android에서 레이아웃에 대해 다음 플랫폼 특정 기능을 제공 됩니다.

- Z-순서 시각적 요소의 그리기 순서를 결정을 제어 합니다. 자세한 내용은 [Android에서 VisualElement 권한 상승](visualelement-elevation.md)합니다.
- 지원 되는 레거시 색 모드 사용 안 함 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다. 자세한 내용은 [VisualElement 레거시 색 모드의 경우 Android](legacy-color-mode.md)합니다.

다음 플랫폼 특정 기능을 Android에서 Xamarin.Forms 보기에 대 한 제공 됩니다.

- 기본 안쪽 여백 및 Android 단추의 섀도 값을 사용합니다. 자세한 내용은 [단추에 안쪽 여백 및 Android에서 그림자](button-padding-shadow.md)합니다.
- 입력된 방법에 대 한 소프트 키보드에 대 한 편집기 옵션 설정 된 [ `Entry` ](xref:Xamarin.Forms.Entry)합니다. 자세한 내용은 [android 항목 입력 방법 편집기 옵션](entry-ime-options.md)합니다.
- 에 그림자를 사용 하도록 설정 된 `ImageButton`합니다. 자세한 내용은 [Android에서 Drop 그림자 ImageButton](imagebutton-drop-shadow.md)합니다.
- 빠른 스크롤을 사용 하도록 설정 된 [ `ListView` ](xref:Xamarin.Forms.ListView) 자세한 내용은 참조 하세요. [Android에서 ListView 빠른 스크롤](listview-fast-scrolling.md).
- 제어 여부는 [ `WebView` ](xref:Xamarin.Forms.WebView) 혼합 된 콘텐츠를 표시할 수 있습니다. 자세한 내용은 [WebView 혼합 콘텐츠 Android에서](webview-mixed-content.md)합니다.

다음 플랫폼 특정 기능을 Android에서 Xamarin.Forms 페이지에 제공 됩니다.

- 탐색 막대의 높이에 설정 된 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)합니다. 자세한 내용은 [Android의 NavigationPage 막대 높이](navigationpage-bar-height.md)합니다.
- 페이지 사이 탐색할 때 전환 애니메이션을 사용 하지 않도록 설정 된 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)합니다. 자세한 내용은 [Android에서 TabbedPage 페이지 전환 애니메이션](tabbedpage-transition-animations.md)합니다.
- 내 페이지 간 살짝 사용 하도록 설정 된 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)합니다. 자세한 내용은 [Android에서 TabbedPage 페이지 넘기기가](tabbedpage-page-swiping.md)합니다.
- 도구 모음 위치와 색에 설정 된 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)합니다. 자세한 내용은 [TabbedPage 도구 모음 배치 및 Android에서 색](tabbedpage-toolbar-placement-color.md)합니다.

Xamarin.Forms에 대 한 다음과 같은 플랫폼별 기능 제공 됩니다 [ `Application` ](xref:Xamarin.Forms.Application) Android 클래스:

- 소프트 키보드의 운영 모드를 설정 합니다. 자세한 내용은 [Android에서 소프트 키보드 입력 모드](soft-keyboard-input-mode.md)합니다.
- 사용 하지 않도록 설정 합니다 [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) 하 고 [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) 페이지 수명 주기 이벤트를 일시 중지 하 고 각각 AppCompat를 사용 하는 응용 프로그램에 대 한 다시 시작 합니다. 자세한 내용은 [Android의 페이지 수명 주기 이벤트](page-lifecycle-events.md)합니다.

## <a name="platform-support"></a>플랫폼 지원

원래 기본 Xamarin.Forms Android 프로젝트는 이전 스타일의 컨트롤 렌더링 Android 5.0 이전 공통적으로 사용 됩니다. 이 템플릿을 가지고 만들어진 앱은 `FormsApplicationActivity`를 메인 액티비티의 베이스 클래스로 가지고 있습니다.

## <a name="material-design-via-appcompat"></a>AppCompat 통해 재질 디자인

Xamarin.Forms Android 프로젝트를 현재 `FormsAppCompatActivity` 해당 기본 활동의 기본 클래스입니다. 이 클래스는 **AppCompat** 재료 디자인 테마를 구현 하는 Android에서 제공 하는 기능입니다.

머티리얼 디자인 테마를 자신의 Xamarin.Forms 안드로이드 프로젝트에 추가하려면 [AppCompat 지원에 대한 설치 지침](appcompat-material-design.md)을 참고하세요.

다음은 기본 `FormsApplicationActivity`를 포함한 **Todo** 샘플입니다.

[![](images/before-appcompat-sml.png "AppCompat을 사용하지 않은 Todo 샘플 앱")](images/before-appcompat.png#lightbox "AppCompat 없이 Todo 샘플 응용 프로그램")


그리고 다음은 동일한 코드를 `FormsAppCompatActivity`를 사용하게 하고, 추가 테마 정보를 추가하여 프로젝트를 업그레이드한 결과입니다.

[![](images/post-appcompat-sml.png "AppCompat과 테마를 사용한 Todo 샘플 앱")](images/post-appcompat.png#lightbox "AppCompat 및 테마 설정 Todo 샘플 응용 프로그램")

> [!NOTE]
> `FormsAppCompatActivity`를 사용하는 경우, [몇몇 안드로이드 커스텀 랜더러들의 베이스 클래스](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)가 달라질 수 있습니다.

## <a name="related-links"></a>관련 링크

- [재질 디자인 지원 추가](appcompat-material-design.md)
