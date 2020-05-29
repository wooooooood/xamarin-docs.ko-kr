---
title: Xamarin.Forms응용 프로그램 테마
description: Xamarin.Forms응용 프로그램은 각 테마에 대해 ResourceDictionary를 만든 다음 DynamicResource 태그 확장을 사용 하 여 리소스를 로드 하 여 테마를 지원 합니다.
ms.prod: ''
ms.assetId: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 80660ae7d3af0fe5948a5ae4ffdb35d2f9c2a40f
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136138"
---
# <a name="theming-a-xamarinforms-application"></a>Xamarin.Forms응용 프로그램 테마

## <a name="theme-an-application"></a>[응용 프로그램 테마](theming.md)

Xamarin.Forms [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 각 테마에 대해를 만든 다음 태그 확장을 사용 하 여 리소스를 로드 하 여 응용 프로그램에서 테마를 구현할 수 있습니다 `DynamicResource` .

## <a name="respond-to-system-theme-changes"></a>[시스템 테마 변경에 응답](system-theme-changes.md)

장치에는 일반적으로 운영 체제 수준에서 설정할 수 있는 다양 한 모양 기본 설정 집합을 참조 하는 밝은 테마 및 어두운 테마가 포함 됩니다. 응용 프로그램은 이러한 시스템 테마를 준수 하 고 시스템 테마가 변경 될 때 즉시 응답 해야 합니다.
