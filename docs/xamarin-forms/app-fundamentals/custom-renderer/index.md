---
title: Xamarin.Forms 사용자 지정 렌더러
description: 사용자 지정 렌더러 Xamarin.Forms 컨트롤의 동작과 모양을 사용자 지정 하려면 각 플랫폼에서 네이티브 컨트롤의 렌더링을 재정의 하는 개발자를 수 있습니다.
ms.prod: xamarin
ms.assetid: BF1CF23A-3BC9-4226-92E6-DAEEB91422F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: c7ae25688b2f8635a9a89318e0b307e58add7a5a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998746"
---
# <a name="xamarinforms-custom-renderers"></a>Xamarin.Forms 사용자 지정 렌더러

_Xamarin.Forms 사용자 인터페이스는 Xamarin.Forms 응용 프로그램이 각 플랫폼에 대 한 적절 한 모양과 느낌을 유지할 수 있도록 하는 대상 플랫폼의 네이티브 컨트롤을 사용 하 여 렌더링 됩니다. 사용자 지정 렌더러는 각 플랫폼에서 Xamarin.Forms 컨트롤의 동작과 모양을 사용자 지정 하려면이 프로세스를 재정의 하는 개발자를 수 있습니다._

## <a name="introduction-to-custom-renderersintroductionmd"></a>[사용자 지정 렌더러 소개](introduction.md)

사용자 지정 렌더러 Xamarin.Forms 컨트롤의 동작과 모양을 사용자 지정 하기 위한 강력한 도구를 제공 합니다. 이러한 작은 스타일 변경 또는 복잡 한 플랫폼 특정 레이아웃 동작 사용자 지정에 사용할 수 있습니다. 이 문서에서는 사용자 지정 렌더러를 소개 하 고 사용자 지정 렌더러를 만드는 과정을 간략하게 설명 합니다.

## <a name="renderer-base-classes-and-native-controlsrenderersmd"></a>[렌더러 기본 클래스 및 네이티브 컨트롤](renderers.md)

모든 Xamarin.Forms 컨트롤에 네이티브 컨트롤의 인스턴스를 만드는 각 플랫폼에 대 한는 함께 제공 되는 렌더러. 이 문서에서는 렌더러 및 각 Xamarin.Forms 페이지, 레이아웃, 뷰 및 셀을 구현 하는 네이티브 컨트롤 클래스를 나열 합니다.

## <a name="customizing-an-entryentrymd"></a>[항목 사용자 지정](entry.md)

Xamarin.Forms [ `Entry` ](xref:Xamarin.Forms.Entry) 컨트롤을 사용 하면 한 줄 텍스트를 편집할 수 있습니다. 이 문서에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다는 `Entry` 컨트롤 개발자가 자신의 플랫폼 특정 사용자 지정을 사용 하 여 기본 네이티브 렌더링을 재정의할 수 있도록 합니다.

## <a name="customizing-a-contentpagecontentpagemd"></a>[ContentPage 사용자 지정](contentpage.md)

A [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 는 단일 보기를 표시 하 고 화면 대부분을 차지 하는 시각적 요소입니다. 이 문서에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다는 `ContentPage` 페이지에서 개발자가 자신의 플랫폼 특정 사용자 지정을 사용 하 여 기본 네이티브 렌더링을 재정의할 수 있도록 합니다.

## <a name="customizing-a-mapmapindexmd"></a>[지도 사용자 지정](map/index.md)

Xamarin.Forms.Maps는 사용자를 위한 빠르고 친숙 한 지도 제공 하려면 각 플랫폼에서 Api 환경을 기본 지도 사용 하는 지도 표시 하기 위한 플랫폼 간 추상화를 제공 합니다. 이 항목에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다는 `Map` 컨트롤 개발자가 자신의 플랫폼 특정 사용자 지정을 사용 하 여 기본 네이티브 렌더링을 재정의할 수 있도록 합니다.

## <a name="customizing-a-listviewlistviewmd"></a>[ListView 사용자 지정](listview.md)

Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) 세로 목록으로 데이터의 컬렉션을 표시 하는 뷰입니다. 이 문서에는 사용자 지정 렌더러를 플랫폼별 목록 컨트롤 및 네이티브 셀 레이아웃을 캡슐화 하는 네이티브 목록 제어 성능 더욱 잘 제어할 수 있도록 만드는 방법을 보여 줍니다.

## <a name="customizing-a-viewcellviewcellmd"></a>[ViewCell 사용자 지정](viewcell.md)

Xamarin.Forms [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) 에 추가할 수 있는 셀을 [ `ListView` ](xref:Xamarin.Forms.ListView) 또는 [ `TableView` ](xref:Xamarin.Forms.TableView), 개발자 정의 보기를 포함 하는 합니다. 이 문서에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다는 `ViewCell` 는 Xamarin.Forms 내에서 호스팅되는 `ListView` 제어 합니다. 이렇게 하면 Xamarin.Forms 레이아웃 계산 중 반복적으로 호출 되 고 중지 `ListView` 스크롤.

## <a name="implementing-a-viewviewmd"></a>[보기 구현](view.md)

Xamarin.Forms 사용자 지정 사용자 인터페이스 컨트롤에서 파생 되어야 합니다 [ `View` ](xref:Xamarin.Forms.View) 레이아웃 및 화면에 컨트롤 배치에 사용 되는 클래스입니다. 이 문서에는 장치의 카메라에서 미리 보기 비디오 스트림을 표시 하는 데 사용 되는 Xamarin.Forms 사용자 지정 컨트롤에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다.

## <a name="implementing-a-hybridwebviewhybridwebviewmd"></a>[HybridWebView 구현](hybridwebview.md)

이 문서에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다는 `HybridWebView` JavaScript에서 C# 코드를 호출할 수 있도록 플랫폼별 웹 컨트롤을 개선 하는 방법을 보여 주는 사용자 지정 컨트롤입니다.

## <a name="implementing-a-video-playervideo-playerindexmd"></a>[비디오 플레이어 구현](video-player/index.md)

이 문서에서는 사용자 지정을 구현 하는 렌더러를 작성 하는 방법을 보여 줍니다. `VideoPlayer` 사용자의 장치에서 비디오 라이브러리에 저장 된 비디오, 응용 프로그램 리소스로 포함 된 비디오 또는 웹에서 비디오를 재생할 수 있는 컨트롤입니다. 구현 메서드 및 읽기 전용으로 바인딩할 수 있는 속성을 포함 하 여 다양 한 기술은 설명 되어 있습니다.


## <a name="related-links"></a>관련 링크

- [효과](~/xamarin-forms/app-fundamentals/effects/index.md)
- [사용자 지정 렌더러 (Xamarin University 비디오)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
- [사용자 지정 렌더러 (Xamarin University 비디오) 샘플](http://bit.ly/xf-customrenderer)
