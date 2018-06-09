---
title: Xamarin.Forms 내게 필요한 옵션
description: 기반 응용 프로그램 빌드를 사용 하면 응용 프로그램 요구 사항 및 경험의 범위를 사용자 인터페이스에 접근 하는 사람에 의해 사용할 수 있습니다.
ms.prod: xamarin
ms.assetid: 99B8A8E8-6F5E-46BC-9639-1C4A6D301049
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 813100acad03684fa9db5343534aee7ca13400c1
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241857"
---
# <a name="xamarinforms-accessibility"></a>Xamarin.Forms 내게 필요한 옵션

_기반 응용 프로그램 빌드를 사용 하면 응용 프로그램 요구 사항 및 경험의 범위를 사용자 인터페이스에 접근 하는 사람에 의해 사용할 수 있습니다._

Xamarin.Forms 응용 프로그램을 액세스할 수 있도록 고안 레이아웃 및 다양 한 사용자 인터페이스 요소가의 디자인에 대 한 의미 합니다. 고려해 야 할 문제에 대 한 지침을 참조 하십시오.는 [내게 필요한 옵션 확인 목록](~/cross-platform/app-fundamentals/accessibility.md)합니다. Xamarin.Forms Api에서 큰 글꼴과 적합 한 색상과 대비 설정 등 많은 액세스 가능성 문제를 해결할 이미 있습니다.

[Android 내게 필요한 옵션](~/android/app-fundamentals/accessibility.md) 및 [iOS 내게 필요한 옵션](~/ios/app-fundamentals/accessibility.md) Xamarin에 의해 노출 되는 네이티브 Api의 세부 정보를 포함 하는 지침 및 [MSDN UWP 내게 필요한 옵션 가이드](https://msdn.microsoft.com/windows/uwp/accessibility/basic-accessibility-information) 설명는 해당 플랫폼에서 네이티브 접근 방식입니다. 이러한 Api는 각 플랫폼에 완벽 하 게 액세스할 수 있는 응용 프로그램을 구현 하는 데 사용 됩니다.

Xamarin.Forms에 현재 없는 *기본 제공* 모든 내게 필요한 옵션 각각의 기본 플랫폼에 사용할 수 있는 Api에 대 한 지원. 그러나 지원지 않습니다 설정 내게 필요한 옵션 값 화면 판독기 및 탐색 지원 도구를 지원 하기 위해 사용자 인터페이스 요소에는 액세스할 수 있는 응용 프로그램을 구축 하는 가장 중요 한 부분 중 하나입니다. 자세한 내용은 참조 [사용자 인터페이스 요소에 설정 내게 필요한 옵션 값](~/xamarin-forms/app-fundamentals/accessibility/setting-accessibility-values.md)합니다.

다른 내게 필요한 옵션 Api (같은 [iOS에서 PostNotification](~/ios/app-fundamentals/accessibility.md))에 더 적합할 수 있습니다는 [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) 또는 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) 구현 합니다. 이이 가이드에서 다루지 않습니다.

## <a name="testing-accessibility"></a>내게 필요한 옵션 테스트

Xamarin.Forms 응용 프로그램 플랫폼에 따라 내게 필요한 옵션 기능을 테스트 하는 것이 즉 여러 플랫폼을 대상 일반적으로 합니다. 각 플랫폼에서 내게 필요한 옵션을 테스트 하는 방법에 알아보려면 다음 링크:

- [**iOS 테스트**](~/ios/app-fundamentals/accessibility.md)
- [**Android 테스트**](~/android/app-fundamentals/accessibility.md)
- [**Windows AccScope (MSDN)**](https://msdn.microsoft.com/library/windows/desktop/dn433239)


## <a name="related-links"></a>관련 링크

- [플랫폼 간 내게 필요한 옵션](~/cross-platform/app-fundamentals/accessibility.md)
- [사용자 인터페이스 요소의 액세스 가능성 값 설정](~/xamarin-forms/app-fundamentals/accessibility/setting-accessibility-values.md)
