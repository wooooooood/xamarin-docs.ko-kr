---
title: 'title: “Xamarin.Forms 접근성” description: “접근성이 있는 애플리케이션을 구축하면 다양한 요구 사항과 환경으로 사용자 인터페이스에 접근하는 사람들이 애플리케이션을 사용할 수 있습니다.”'
description: 'ms.prod: xamarin ms.assetid: 99B8A8E8-6F5E-46BC-9639-1C4A6D301049 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 05/28/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials] ms.custom: video'
ms.prod: xamarin
ms.assetid: 99B8A8E8-6F5E-46BC-9639-1C4A6D301049
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/28/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.custom: video
ms.openlocfilehash: 7ac8b305ae09e005013aea9f83fb4a3e4740f2b2
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84129809"
---
# <a name="xamarinforms-accessibility"></a>Xamarin.Forms 접근성

_접근성이 있는 애플리케이션을 구축하면 다양한 요구 사항과 환경으로 사용자 인터페이스에 접근하는 사람들이 애플리케이션을 사용할 수 있습니다._

Xamarin.Forms 애플리케이션을 접근성이 있도록 만들려면 다양한 사용자 인터페이스 요소의 레이아웃과 디자인을 고려해야 합니다. 고려해야 하는 문제에 대한 지침은 [접근성 검사 목록](~/cross-platform/app-fundamentals/accessibility.md)을 참조하세요. 큰 글꼴 및 적절한 색상 및 대비 설정과 같은 다양한 접근성 문제는 Xamarin.Forms API로 해결할 수 있습니다.

[Android 접근성](~/android/app-fundamentals/accessibility.md) 및 [iOS 접근성](~/ios/app-fundamentals/accessibility.md) 가이드에는 Xamarin에 노출된 네이티브 API에 대한 세부 정보가 포함되어 있고 [MSDN의 UWP 접근성 가이드](https://msdn.microsoft.com/windows/uwp/accessibility/basic-accessibility-information)에는 해당 플랫폼에 대한 네이티브 접근 방식이 설명되어 있습니다. 이러한 API는 각 플랫폼에서 접근이 가능한 애플리케이션을 완벽하게 구현하는 데 사용됩니다.

Xamarin.Forms에는 각각의 기본 플랫폼에서 사용할 수 있는 모든 접근성 API에 대한 기본 제공 지원이 포함되어 있지 않습니다. 하지만 화면 판독기와 탐색 지원 도구를 지원하는 사용자 인터페이스 요소의 자동화 속성을 설정하는 기능은 지원되며, 이것은 접근성 있는 애플리케이션을 구축하는 데 가장 중요한 부분 중 하나입니다. 자세한 내용은 [자동화 속성](~/xamarin-forms/app-fundamentals/accessibility/automation-properties.md)을 참조하십시오.

Xamarin.Forms 애플리케이션에서는 지정된 컨트롤의 탭 순서를 사용하여 유용성 및 접근성을 개선할 수도 있습니다. 자세한 내용은 [키보드 접근성](~/xamarin-forms/app-fundamentals/accessibility/keyboard.md)을 참조하세요.

다른 접근성 API(예: [iOS의 PostNotification](~/ios/app-fundamentals/accessibility.md))는 [`DependencyService`](~/xamarin-forms/app-fundamentals/dependency-service/index.md) 또는 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) 구현에 더 적합할 수 있습니다. 이 내용은 가이드에서 다루지 않습니다.

## <a name="testing-accessibility"></a>접근성 테스트

Xamarin.Forms 애플리케이션은 대개 여러 플랫폼을 대상으로 합니다. 즉, 플랫폼에 따라 접근성 기능을 테스트해야 합니다. 각 플랫폼에서 접근성을 테스트하는 방법을 알아보려면 아래 링크를 참조하세요.

- [**iOS 테스팅**](~/ios/app-fundamentals/accessibility.md)
- [**Android 테스팅**](~/android/app-fundamentals/accessibility.md)
- [**Windows AccScope(MSDN)** ](https://msdn.microsoft.com/library/windows/desktop/dn433239)

## <a name="related-links"></a>관련 링크

- [플랫폼 간 접근성](~/cross-platform/app-fundamentals/accessibility.md)
- [Automation 속성](~/xamarin-forms/app-fundamentals/accessibility/automation-properties.md)
- [키보드 접근성](~/xamarin-forms/app-fundamentals/accessibility/keyboard.md)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Making-Mobile-Apps-Accessible/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
