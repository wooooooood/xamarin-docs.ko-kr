---
title: Xamarin.Forms Shell 소개
description: Xamarin.Forms Shell은 일반 탐색 사용자 환경, URI 기반 탐색 체계 및 통합 검색 처리기를 포함하여 대부분의 애플리케이션에 필요한 기본 기능을 제공합니다.
ms.prod: xamarin
ms.assetid: 4604DCB5-83DA-458A-8B02-6508A740BE0E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 29a99161ff2ef2d71b6c803db994522bfe80ed03
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138738"
---
# <a name="xamarinforms-shell-introduction"></a>Xamarin.Forms Shell 소개

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Xamarin.Forms Shell은 다음을 비롯한 대부분의 모바일 애플리케이션에 필요한 기본 기능을 제공하여 모바일 애플리케이션 개발의 복잡성을 줄입니다.

- 애플리케이션의 시각적 계층 구조를 설명하는 단일 위치입니다.
- 일반적인 탐색 사용자 환경입니다.
- 애플리케이션의 모든 페이지에 대한 탐색을 허용하는 URI 기반 탐색 체계입니다.
- 통합된 검색 처리기입니다.

또한 셸 애플리케이션은 렌더링 속도 향상 및 메모리 사용 감소의 혜택을 받습니다.

> [!IMPORTANT]
> 기존 애플리케이션에 셸을 도입하면 탐색, 성능 및 확장성 향상으로 인한 즉각적인 이점을 얻을 수 있습니다.

## <a name="platform-support"></a>플랫폼 지원

Xamarin.Forms Shell은 iOS 및 Android에서는 완전히 지원되나 UWP(유니버설 Windows 플랫폼)에서는 부분적으로만 지원됩니다. 이에 더해, 셸은 현재 UWP에서 시험 단계에 있으며, UWP 프로젝트에서 `App` 클래스에 다음 코드 줄을 추가하고 `Forms.Init`을 호출해야만 사용 가능합니다.

```csharp
global::Xamarin.Forms.Forms.SetFlags("Shell_UWP_Experimental");
```

UWP 프로젝트를 Xamarin.Forms 솔루션에 추가하는 방법에 대한 자세한 내용은 [Windows 프로젝트 설정](~/xamarin-forms/platform/windows/installation/index.md)을 참조하세요.

## <a name="shell-navigation-experience"></a>셸 탐색 환경

셸은 플라이아웃 및 탭을 기반으로 한 독자적인 탐색 환경을 제공합니다. Shell 애플리케이션에서 탐색의 최상위 수준은 애플리케이션의 탐색 요구 사항에 따라 플라이아웃 또는 아래쪽 탭 표시줄이 됩니다. 다음 예제에서는 탐색의 최상위 수준이 플라이아웃인 애플리케이션을 보여 줍니다.

[![iOS 및 Android의 셸 플라이아웃 스크린샷](introduction-images/flyout.png "셸 플라이아웃")](introduction-images/flyout-large.png#lightbox "셸 플라이아웃")

플라이아웃을 선택하면 항목을 나타내는 아래쪽 탭이 선택되고 표시됩니다.

[![iOS 및 Android의 셸 하단 탭 스크린샷](introduction-images/monkeys.png "셸 아래쪽 탭")](introduction-images/monkeys-large.png#lightbox "셸 아래쪽 탭")

> [!NOTE]
> 플라이아웃이 열리지 않으면 아래쪽 탭 표시줄을 애플리케이션의 최상위 탐색 수준으로 간주할 수 있습니다.

각 탭에는 [`ContentPage`](xref:Xamarin.Forms.ContentPage)가 표시됩니다. 그러나 아래쪽 탭에 둘 이상의 페이지가 포함되면 위쪽 탭 표시줄을 통해 페이지를 탐색할 수 있습니다.

[![iOS 및 Android의 셸 상단 탭 스크린샷](introduction-images/cats.png "셸 위쪽 탭")](introduction-images/cats-large.png#lightbox "셸 위쪽 탭")

각 탭 내에서 추가 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체로 이동할 수 있습니다.

[![iOS 및 Android의 셸 페이지 탐색 스크린샷](introduction-images/cat-details.png "셸 앱 탐색")](introduction-images/cat-details-large.png#lightbox "셸 앱 탐색")

## <a name="related-links"></a>관련 링크

- [Xaminals(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
