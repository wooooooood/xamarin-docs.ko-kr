---
title: Xamarin.Forms 애플리케이션 테마 설정
description: Xamarin Forms 응용 프로그램은 각 테마에 대해 ResourceDictionary를 만든 다음 DynamicResource 태그 확장을 사용 하 여 리소스를 로드 하 여 테마를 지원 합니다.
ms.prod: xamarin
ms.assetId: BF92AEDD-EF23-4D08-A972-B089066E75F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2020
ms.openlocfilehash: 5988437b40ac875b8b59f9af0f25d4b5c60ded97
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517139"
---
# <a name="theming-a-xamarinforms-application"></a>Xamarin Forms 응용 프로그램 테마

## <a name="theme-an-application"></a>[응용 프로그램 테마](theming.md)

테마는 각 테마에 대해를 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 만든 다음 `DynamicResource` 태그 확장을 사용 하 여 리소스를 로드 하 여 xamarin.ios 응용 프로그램에서 구현할 수 있습니다.

## <a name="respond-to-system-theme-changes"></a>[시스템 테마 변경 내용에 응답](system-theme-changes.md)

장치에는 일반적으로 운영 체제 수준에서 설정할 수 있는 다양 한 모양 기본 설정 집합을 참조 하는 밝은 테마 및 어두운 테마가 포함 됩니다. 응용 프로그램은 이러한 시스템 테마를 준수 하 고 시스템 테마가 변경 될 때 즉시 응답 해야 합니다.
