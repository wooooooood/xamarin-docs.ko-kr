---
title: Xamarin.forms에서는 XAML 필드 한정자
description: X:fieldmodifier 네임 스페이스 특성에는 명명 된 XAML 요소에 대 한 생성 된 필드에 대 한 액세스 수준을 지정합니다.
ms.prod: xamarin
ms.assetid: 12357CE0-3C11-4B62-947F-72DB6DFC23A2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2018
ms.openlocfilehash: 8be56524ec1c5331f30418fcc29a4bd2c26ccde1
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209512"
---
# <a name="xaml-field-modifiers-in-xamarinforms"></a>Xamarin.forms에서는 XAML 필드 한정자

_`x:FieldModifier` 네임 스페이스 특성 명명 된 XAML 요소에 대 한 생성 된 필드에 대 한 액세스 수준을 지정 합니다._

## <a name="overview"></a>개요

특성의 유효한 값은 같습니다.

- `Public` – XAML 요소에 대 한 생성 된 필드 지정 `public`합니다.
- `NotPublic` – XAML 요소에 대 한 생성 된 필드 지정 `internal` 어셈블리에 있습니다.

특성의 값이 설정 되지 않은 경우 요소에 대해 생성 된 필드 됩니다 `private`합니다.

다음 조건을 만족 해야 합니다는 `x:FieldModifier` 특성을 처리할 수 있습니다.

- 최상위 XAML 요소는 유효 해야 `x:Class`합니다.
- 현재 XAML 요소에는 `x:Name` 지정 합니다.

다음 XAML 특성 설정의 예를 보여 줍니다.

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="NotPublic" />
<Label x:Name="publicLabel" x:FieldModifier="Public" />
```

> [!NOTE]
> `x:FieldModifier` XAML 클래스의 액세스 수준을 지정 하려면 특성을 사용할 수 없습니다.
