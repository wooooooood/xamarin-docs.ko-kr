---
title: Xamarin.Forms 내게 필요한 옵션
description: 응용 프로그램 요구 사항 및 환경을 범위를 사용 하 여 사용자 인터페이스에 접근 하는 사용자가 사용할 수 있는 인지 확인 액세스할 수 있는 응용 프로그램을 작성 합니다.
ms.prod: xamarin
ms.assetid: 99B8A8E8-6F5E-46BC-9639-1C4A6D301049
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/15/2018
ms.openlocfilehash: ac0ffbdce6b0c55e8ad9d774d80e3d9b8bf84089
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116448"
---
# <a name="xamarinforms-accessibility"></a>Xamarin.Forms 내게 필요한 옵션

_응용 프로그램 요구 사항 및 환경을 범위를 사용 하 여 사용자 인터페이스에 접근 하는 사용자가 사용할 수 있는 인지 확인 액세스할 수 있는 응용 프로그램을 작성 합니다._

Xamarin.Forms 응용 프로그램을 액세스할 수 있도록 레이아웃 및 디자인의 많은 사용자 인터페이스 요소에 대 한 생각을 의미 합니다. 고려해 야 할 문제에 대 한 지침을 참조 하세요. 합니다 [접근성 검사 목록](~/cross-platform/app-fundamentals/accessibility.md)합니다. Xamarin.Forms Api에서 큰 글꼴 및 적절 한 색과 대비 설정과 같은 많은 접근성 문제를 해결할 이미 있습니다.

[Android 내게 필요한 옵션](~/android/app-fundamentals/accessibility.md) 및 [iOS 내게 필요한 옵션](~/ios/app-fundamentals/accessibility.md) 네이티브 Xamarin에서 노출 한 Api 세부 정보를 포함 하는 가이드 및 [MSDN의 UWP 내게 필요한 옵션 가이드](https://msdn.microsoft.com/windows/uwp/accessibility/basic-accessibility-information) 에 대해 설명 합니다 해당 플랫폼에서 네이티브 접근 방식입니다. 이러한 Api는 각 플랫폼에 액세스할 수 있는 응용 프로그램을 완전히 구현 하는 데 사용 됩니다.

Xamarin.Forms에 현재 없는 *기본 제공* 모든 각 기본 플랫폼에서 사용할 수 있는 Api 액세스 가능성에 대 한 지원. 그러나 지원지 않습니다 자동화 속성 설정 화면 판독기 및 탐색 지원 도구를 지원 하기 위해 사용자 인터페이스 요소에 액세스할 수 있는 응용 프로그램 빌드에 대 한 가장 중요 한 부분 중 하나입니다. 자세한 내용은 [자동화 속성](~/xamarin-forms/app-fundamentals/accessibility/automation-properties.md)합니다.

Xamarin.Forms 응용 프로그램에서 지정 된 컨트롤의 탭 순서를 포함할 수도 있습니다. 자세한 내용은 [키보드 탐색](~/xamarin-forms/app-fundamentals/accessibility/keyboard.md)합니다.

다른 접근성 Api (같은 [iOS에서 PostNotification](~/ios/app-fundamentals/accessibility.md))에 더 적합할 수 있습니다를 [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) 또는 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) 구현 합니다. 이러한이 가이드에서 다루지 않습니다.

## <a name="testing-accessibility"></a>액세스 가능성 테스트

Xamarin.Forms 응용 프로그램은 일반적으로 여러 플랫폼을 대상 즉, 플랫폼에 따라 내게 필요한 옵션 기능을 테스트 합니다. 각 플랫폼에서 내게 필요한 옵션을 테스트 하는 방법을 알아보려면 다음이 링크를 수행 합니다.

- [**iOS 테스트**](~/ios/app-fundamentals/accessibility.md)
- [**Android 테스트**](~/android/app-fundamentals/accessibility.md)
- [**Windows AccScope (MSDN)**](https://msdn.microsoft.com/library/windows/desktop/dn433239)

## <a name="related-links"></a>관련 링크

- [플랫폼 간 내게 필요한 옵션](~/cross-platform/app-fundamentals/accessibility.md)
- [자동화 속성](~/xamarin-forms/app-fundamentals/accessibility/automation-properties.md)
- [키보드 접근성](~/xamarin-forms/app-fundamentals/accessibility/keyboard.md)
