---
title: Xamarin.Forms Shell
description: 이 가이드에서는 대부분의 애플리케이션에서 필요로 하는 기본 기능을 제공하여 Xamarin.Forms 애플리케이션의 복잡성을 줄이는 Xamarin.Forms Shell을 사용하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 85B322AA-808F-41B6-953A-5877264AE643
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 40f955d39799598093060f3230629a099885e4a2
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970771"
---
# <a name="xamarinforms-shell"></a>Xamarin.Forms Shell

## <a name="introductionintroductionmd"></a>[소개](introduction.md)

Xamarin.Forms Shell은 대부분의 모바일 애플리케이션에 필요한 기본 기능을 제공하여 모바일 애플리케이션 개발의 복잡성을 줄입니다. 여기에는 일반 탐색 사용자 환경, URI 기반 탐색 체계 및 통합 검색 처리기가 포함됩니다.

## <a name="flyoutflyoutmd"></a>[플라이아웃](flyout.md)

플라이아웃은 셸 애플리케이션의 루트 메뉴이며, 아이콘을 통해 또는 화면 측면에서 살짝 밀어 액세스할 수 있습니다. 플라이아웃은 선택적 헤더, 플라이아웃 항목 및 선택적 메뉴 항목으로 구성됩니다.

## <a name="tabstabsmd"></a>[탭](tabs.md)

플라이아웃 후에 셸 애플리케이션에서 다음 탐색 수준은 아래쪽 탭 표시줄입니다. 탭에 둘 이상의 페이지가 포함되면 위쪽 탭으로 페이지를 탐색할 수 있습니다.

## <a name="navigationnavigationmd"></a>[탐색](navigation.md)

셸 애플리케이션은 세트 탐색 계층 구조를 따르지 않고도 애플리케이션의 모든 페이지로 이동하는 데 경로를 사용하는 URI 기반 탐색 체계를 이용할 수 있습니다.

## <a name="searchsearchmd"></a>[검색](search.md)

셸 애플리케이션은 각 페이지 위쪽에 추가될 수 있는 검색 상자에 제공되는 통합 검색 기능을 사용할 수 있습니다.

## <a name="custom-rendererscustomrenderersmd"></a>[사용자 지정 렌더러](customrenderers.md)

셸 애플리케이션은 다양한 셸 클래스가 공개하는 속성 및 메서드를 통해 세부적으로 사용자 지정할 수 있습니다. 그러나 더 정교한 플랫폼별 사용자 지정이 필요한 경우에는 셸 사용자 지정 렌더러를 만들 수도 있습니다.
