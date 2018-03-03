---
title: "사용자 지정 렌더러"
description: "Xamarin.Forms 사용자 인터페이스는 각 플랫폼에 대 한 적절 한 모양 및 느낌을 유지 하려면 Xamarin.Forms 응용 프로그램의 대상 플랫폼에 네이티브 컨트롤을 사용 하 여 렌더링 됩니다. 사용자 지정 렌더러 모양 및 Xamarin.Forms는 각 플랫폼에 있는 컨트롤의 동작을 사용자 지정 하려면이 프로세스를 재정의 하는 개발자를 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: BF1CF23A-3BC9-4226-92E6-DAEEB91422F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: a92da0320addf1569c25ed05873aa11a198b1daa
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="custom-renderers"></a>사용자 지정 렌더러

_Xamarin.Forms 사용자 인터페이스는 각 플랫폼에 대 한 적절 한 모양 및 느낌을 유지 하려면 Xamarin.Forms 응용 프로그램의 대상 플랫폼에 네이티브 컨트롤을 사용 하 여 렌더링 됩니다. 사용자 지정 렌더러 모양 및 Xamarin.Forms는 각 플랫폼에 있는 컨트롤의 동작을 사용자 지정 하려면이 프로세스를 재정의 하는 개발자를 수 있습니다._

## <a name="introduction-to-custom-renderersintroductionmd"></a>[사용자 지정 렌더러 소개](introduction.md)

사용자 지정 렌더러 모양 및 Xamarin.Forms 컨트롤의 동작을 사용자 지정 하기 위한 강력한 도구를 제공 합니다. 작은 스타일 변경 내용 또는 정교한 플랫폼 특정 레이아웃 및 동작 사용자 지정에 사용할 수 있습니다. 이 문서에서는 사용자 지정 렌더러를 소개 하 고 사용자 지정 렌더러 만들기 위한 프로세스를 보여 줍니다.

## <a name="renderer-base-classes-and-native-controlsrenderersmd"></a>[렌더러의 기본 클래스와 기본 컨트롤](renderers.md)

Xamarin.Forms는 모든 컨트롤에 네이티브 컨트롤의 인스턴스를 생성 하는 각 플랫폼에 대 한 함께 제공 되는 렌더러 있습니다. 이 문서에서는 렌더러와 각 Xamarin.Forms 페이지, 레이아웃, 뷰 및 셀을 구현 하는 네이티브 컨트롤 클래스를 나열 합니다.

## <a name="customizing-an-entryentrymd"></a>[항목을 사용자 지정](entry.md)

Xamarin.Forms는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 컨트롤을 사용 하면 단일 텍스트 줄을 편집 합니다. 이 문서에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다는 `Entry` 컨트롤 개발자가 자신의 플랫폼 관련 사용자 지정과 기본 네이티브 렌더링을 재정의할 수 있도록 합니다.

## <a name="customizing-a-contentpagecontentpagemd"></a>[ContentPage 사용자 지정](contentpage.md)

A [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 단일 보기를 표시 하는 화면의 대부분을 차지 하는 시각적 요소입니다. 이 문서에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다는 `ContentPage` 페이지에서 개발자가 자신의 플랫폼 관련 사용자 지정과 기본 네이티브 렌더링을 재정의할 수 있도록 합니다.

## <a name="customizing-a-mapmapindexmd"></a>[지도 사용자 지정](map/index.md)

Xamarin.Forms.Maps는 사용자에 대 한 네이티브 맵의 환경을 신속 하 고 친숙 한 지도 제공 하는 각 플랫폼에서 Api를 사용 하는 맵을 표시 하기 위한 플랫폼 독립적인 추상화를 제공 합니다. 이 항목에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다는 `Map` 컨트롤 개발자가 자신의 플랫폼 관련 사용자 지정과 기본 네이티브 렌더링을 재정의할 수 있도록 합니다.

## <a name="customizing-a-listviewlistviewmd"></a>[ListView에 사용자 지정](listview.md)

Xamarin.Forms는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 세로 목록으로 데이터의 컬렉션을 표시 하는 뷰입니다. 이 문서를 만드는 사용자 지정 렌더러 플랫폼별 목록 컨트롤 및 네이티브 셀 레이아웃을 캡슐화 하는 네이티브 목록 컨트롤 성능에 비해 더 많은 제어를 허용 하는 방법을 보여줍니다.

## <a name="customizing-a-viewcellviewcellmd"></a>[ViewCell 사용자 지정](viewcell.md)

Xamarin.Forms는 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) 에 추가할 수 있는 셀은 한 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 또는 [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/), 개발자 정의 보기를 포함 하 합니다. 이 문서에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다는 `ViewCell` 하는 Xamarin.Forms 내에서 호스팅되는 `ListView` 제어 합니다. 이 인해 중지 하는 동안 반복적으로 호출 하지 못하도록 Xamarin.Forms 레이아웃 계산 `ListView` 스크롤입니다.

## <a name="implementing-a-viewviewmd"></a>[뷰를 구현합니다.](view.md)

Xamarin.Forms 사용자 지정 사용자 인터페이스 정의에서 파생 되어야 합니다는 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) 레이아웃와 화면에 컨트롤을 배치 하는 데 사용 되는 클래스입니다. 이 문서에는 장치의 카메라에서 미리 보기 비디오 스트림을 표시 하는 데 사용 되는 Xamarin.Forms 사용자 지정 컨트롤에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다.

## <a name="implementing-a-hybridwebviewhybridwebviewmd"></a>[HybridWebView 구현](hybridwebview.md)

이 문서에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다는 `HybridWebView` JavaScript에서 C# 코드를 호출할 수 있도록 플랫폼 특정 웹 컨트롤을 개선 하는 방법을 보여 주는 사용자 지정 컨트롤입니다.

## <a name="implementing-a-video-playervideo-playerindexmd"></a>[비디오 플레이어를 구현합니다.](video-player/index.md)

이 문서에서는 사용자 지정을 구현 하는 렌더러를 작성 하는 방법을 보여 줍니다. `VideoPlayer` 웹, 응용 프로그램 리소스로 포함 하는 비디오 또는 사용자의 장치에서 비디오 라이브러리에 저장 된 비디오의 비디오를 재생할 수 있는 컨트롤입니다. 몇 가지 기법을 구현 하는 메서드 및 바인딩 가능한 읽기 전용 속성을 포함 하 여 보여 줍니다. 


## <a name="related-links"></a>관련 링크

- [효과](~/xamarin-forms/app-fundamentals/effects/index.md)
- [사용자 지정 렌더러 (Xamarin 대학 비디오)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
- [사용자 지정 렌더러 (Xamarin 대학 비디오) 샘플](http://bit.ly/xf-customrenderer)
