---
title: Xamarin.Forms의 XAML 필드 한정자
description: x:Fieldmodifier 네임 스페이스 특성은 명명된 XAML 요소를 위해 생성된 필드에 대한 접근 수준을 지정합니다.
ms.prod: xamarin
ms.assetid: 12357CE0-3C11-4B62-947F-72DB6DFC23A2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/02/2019
ms.openlocfilehash: 0f6050de943ca9878cf41b448d44bf222689be56
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739444"
---
# <a name="xaml-field-modifiers-in-xamarinforms"></a>Xamarin.Forms의 XAML 필드 한정자

`x:FieldModifier` 네임스페이스 특성을 통해 명명된 XAML 요소에서 생성된 필드에 대한 액세스 수준을 지정할 수 있습니다. 특성의 유효한 값은 다음과 같습니다.

- `private`– XAML 요소에 대해 생성 된 필드가 선언 된 클래스의 본문 내 에서만 액세스할 수 있도록 지정 합니다.
- `public`– XAML 요소에 대해 생성 된 필드에 액세스 제한이 없도록 지정 합니다.
- `protected`– XAML 요소에 대해 생성 된 필드를 해당 클래스 내에서 파생 클래스 인스턴스로 액세스할 수 있도록 지정 합니다.
- `internal`– XAML 요소에 대해 생성 된 필드가 동일한 어셈블리의 형식 내 에서만 액세스할 수 있도록 지정 합니다.
- `notpublic`– XAML 요소에 대해 생성 된 필드가 동일한 어셈블리의 형식 내 에서만 액세스할 수 있도록 지정 합니다.

기본적으로 특성 값이 설정 되지 않은 경우 요소에 대해 생성 된 필드는가 `private`됩니다.

> [!NOTE]
> 특성의 값은 모든 대/소문자를 사용할 수 있습니다.

다음 조건을 처리하려면 `x:FieldModifier` 특성이 있어야 합니다.

- 최상위 수준 XAML 요소는 유효한 `x:Class`여야 합니다.
- 현재 XAML 요소에는 `x:Name` 지정이 있습니다.

다음 XAML은 특성 설정의 예를 보여 줍니다.

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="internal" />
<Label x:Name="publicLabel" x:FieldModifier="public" />
```

> [!IMPORTANT]
> `x:FieldModifier` 특성은 XAML 클래스의 접근 수준을 지정 사용할 수 없습니다.
