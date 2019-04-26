---
title: Xamarin.Forms의 XAML 필드 한정자
description: x:Fieldmodifier 네임 스페이스 특성은 명명된 XAML 요소를 위해 생성된 필드에 대한 접근 수준을 지정합니다.
ms.prod: xamarin
ms.assetid: 12357CE0-3C11-4B62-947F-72DB6DFC23A2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2018
ms.openlocfilehash: 8be56524ec1c5331f30418fcc29a4bd2c26ccde1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075300"
---
# <a name="xaml-field-modifiers-in-xamarinforms"></a>Xamarin.Forms의 XAML 필드 한정자

_`x:FieldModifier` 네임 스페이스 특성은 명명된 XAML 요소를 위해 생성된 필드에 대한 접근 수준을 지정 합니다._

## <a name="overview"></a>개요

특성의 유효한 값은 다음과 같습니다.

- `Public` – XAML 요소에 대해 생성된 필드를 `public`으로 지정합니다.
- `NotPublic` – XAML 요소에 대해 생성된 필드를 어셈블리에 `internal`로 지정합니다.

특성의 값이 설정되지 않은 경우, 요소에 대해 생성된 필드는 `private`이 됩니다.

다음 조건을 처리하려면 `x:FieldModifier` 특성이 있어야 합니다.

- 최상위 수준 XAML 요소는 유효한 `x:Class`여야 합니다.
- 현재 XAML 요소에는 `x:Name` 지정이 있습니다.

다음 XAML은 특성 설정의 예를 보여 줍니다.

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="NotPublic" />
<Label x:Name="publicLabel" x:FieldModifier="Public" />
```

> [!NOTE]
> `x:FieldModifier` 특성은 XAML 클래스의 접근 수준을 지정 사용할 수 없습니다.
