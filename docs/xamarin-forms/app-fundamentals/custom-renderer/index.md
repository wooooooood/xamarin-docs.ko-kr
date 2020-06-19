---
title: Xamarin.Forms 사용자 지정 렌더러
description: 개발자는 사용자 지정 렌더러를 통해 각 플랫폼에서 네이티브 컨트롤의 렌더링을 재정의하여 Xamarin.Forms 컨트롤의 모양과 동작을 사용자 지정할 수 있습니다.
ms.prod: xamarin
ms.assetid: BF1CF23A-3BC9-4226-92E6-DAEEB91422F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/03/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: be281f9b7987a8d23ba6ac93f0771e432f277d45
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138920"
---
# <a name="xamarinforms-custom-renderers"></a>Xamarin.Forms 사용자 지정 렌더러

Xamarin.Forms 사용자 인터페이스는 Xamarin.Forms 애플리케이션이 각 플랫폼에 적합한 모양과 느낌을 유지하도록 하는 대상 플랫폼의 네이티브 컨트롤을 사용하여 렌더링됩니다. 개발자는 사용자 지정 렌더러를 통해 이 프로세스를 재정의하여 각 플랫폼에서 Xamarin.Forms 컨트롤의 모양과 동작을 사용자 지정할 수 있습니다.

## <a name="introduction-to-custom-renderers"></a>[사용자 지정 렌더러 소개](introduction.md)

사용자 지정 렌더러를 사용하면 Xamarin.Forms 컨트롤의 모양과 동작을 효과적으로 사용자 지정할 수 있습니다. 작은 스타일 변경 또는 정교한 플랫폼별 레이아웃 및 동작 사용자 지정에 사용할 수 있습니다. 이 문서에서는 사용자 지정 렌더러를 소개하고 사용자 지정 렌더러를 만드는 과정을 간략하게 설명합니다.

## <a name="renderer-base-classes-and-native-controls"></a>[렌더러 기본 클래스 및 네이티브 컨트롤](renderers.md)

모든 Xamarin.Forms 컨트롤의 모양과 동작을 효과적으로 사용자 지정할 수 있습니다. 이 문서에는 각 Xamarin.Forms 페이지, 레이아웃, 보기 및 셀을 구현하는 네이티브 컨트롤 클래스와 렌더러가 나열됩니다.

## <a name="customizing-an-entry"></a>[항목 사용자 지정](entry.md)

Xamarin.Forms [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤을 사용하면 한 줄 텍스트를 편집할 수 있습니다. 이 문서에서는 개발자가 자체적인 플랫폼별 사용자 지정을 통해 기본 네이티브 렌더링을 재정의할 수 있도록 `Entry` 컨트롤에 대한 사용자 지정 렌더러를 만드는 방법을 보여줍니다.

## <a name="customizing-a-contentpage"></a>[ContentPage 사용자 지정](contentpage.md)

[`ContentPage`](xref:Xamarin.Forms.ContentPage)는 단일 보기를 표시하고 대부분의 화면을 차지하는 시각적 요소입니다. 이 문서에서는 개발자가 자체적인 플랫폼별 사용자 지정을 통해 기본 네이티브 렌더링을 재정의할 수 있도록 `ContentPage` 페이지에 대한 사용자 지정 렌더러를 만드는 방법을 보여줍니다.

## <a name="customizing-a-map-pin"></a>[지도 핀 사용자 지정](map-pin.md)

Xamarin.Forms.Maps에는 각 플랫폼에서 네이티브 맵 API를 사용하는 맵을 표시하는 플랫폼 간 추상화 기능이 있어 사용자에게 친숙하고 빠른 맵 환경을 제공할 수 있습니다. 이 항목에서는 개발자가 고유한 플랫폼별 사용자 지정을 통해 기본 네이티브 렌더링을 재정의할 수 있도록 `Map` 컨트롤에 대한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다.

## <a name="customizing-a-listview"></a>[ListView 사용자 지정](listview.md)

Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView)는 데이터의 컬렉션을 세로 목록으로 표시하는 보기입니다. 이 문서에서는 네이티브 목록 컨트롤 성능을 보다 효과적으로 제어할 수 있도록 플랫폼별 리스트 컨트롤과 네이티브 셀 레이아웃을 캡슐화하는 사용자 지정 렌더러를 만드는 방법을 보여줍니다.

## <a name="customizing-a-viewcell"></a>[ViewCell 사용자 지정](viewcell.md)

Xamarin.Forms [`ViewCell`](xref:Xamarin.Forms.ViewCell)은 개발자 정의 보기가 포함된 [`ListView`](xref:Xamarin.Forms.ListView) 또는 [`TableView`](xref:Xamarin.Forms.TableView)에 추가할 수 있는 셀입니다. 이 문서에서는 Xamarin.Forms `ListView` 컨트롤 내부에 호스트되는 `ViewCell`의 사용자 지정 렌더러를 만드는 방법을 보여 줍니다. 이렇게 하면 `ListView`를 스크롤하는 동안 Xamarin.Forms 레이아웃 계산이 반복적으로 호출되지 않습니다.

## <a name="customizing-a-webview"></a>[WebView 사용자 지정](hybridwebview.md)

Xamarin.Forms [`WebView`](xref:Xamarin.Forms.WebView)는 앱에서 웹 및 HTML 콘텐츠를 표시하는 보기입니다. 이 문서에서는 JavaScript에서 C# 코드를 호출할 수 있도록 `WebView`을(를) 확장하는 사용자 지정 렌더러를 만드는 방법을 설명합니다.

## <a name="implementing-a-view"></a>[보기 구현](view.md)

Xamarin.Forms 사용자 지정 사용자 인터페이스 컨트롤은 화면에 레이아웃과 컨트롤을 배치하는 데 사용되는 [`View`](xref:Xamarin.Forms.View) 클래스에서 파생되어야 합니다. 이 문서에서는 디바이스 카메라에서 미리 보기 동영상 스트림을 표시하는 데 사용되는 Xamarin.Forms 사용자 지정 컨트롤의 사용자 지정 렌더러를 만드는 방법을 설명합니다.

## <a name="implementing-a-video-player"></a>[비디오 플레이어 구현](video-player/index.md)

이 문서에서는 웹의 비디오, 애플리케이션 리소스로 포함된 비디오 또는 사용자 디바이스의 비디오 라이브러리에 저장된 비디오를 재생할 수 있는 사용자 지정 `VideoPlayer` 컨트롤을 구현하는 렌더러를 작성하는 방법을 보여줍니다. 메서드 구현 및 읽기 전용 바인딩 가능 속성을 비롯한 여러 가지 기술을 보여줍니다.
