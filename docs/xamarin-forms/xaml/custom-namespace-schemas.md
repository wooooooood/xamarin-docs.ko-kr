---
title: Xamarin.forms에서 XAML Namespace 사용자 지정 스키마
description: 사용자 지정 URL 및 하나 이상의 CLR 네임 스페이스 간의 매핑을 지정 하는 XmlnsDefinitionAttribute 클래스를 사용 하 여 XAML 네임 스페이스 사용자 지정 스키마를 정의할 수 있습니다. 사용자 지정 네임 스페이스 스키마 XAML 네임 스페이스 선언에서 사용할 수 있습니다.
ms.prod: xamarin
ms.assetid: FDF201A1-8C35-4569-A728-F9B0A0C5B31A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/21/2018
ms.openlocfilehash: 8167ff00d3e4d7167772f6f5a578da6197c0d72d
ms.sourcegitcommit: 93c9fe61eb2cdfa530960b4253eb85161894c882
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55832233"
---
# <a name="xaml-custom-namespace-schemas-in-xamarinforms"></a>Xamarin.forms에서 XAML Namespace 사용자 지정 스키마

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/XAML/CustomNamespaceSchemas/)

형식 라이브러리의 공용 언어 런타임 (CLR) 네임 스페이스 이름과 어셈블리 이름을 지정 하는 네임 스페이스 선언을 사용 하 여 라이브러리에 대 한 XAML 네임 스페이스를 선언 하 여 XAML에서 참조할 수 있습니다.

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:MyCompany.Controls;assembly="MyCompany.Controls">
    ...
</ContentPage>
```

그러나에서 CLR 네임 스페이스와 어셈블리 이름을 지정 하는 `xmlns` 정의 까다롭습니다. 오류가 발생 하기 쉽습니다. 또한 여러 네임 스페이스의 형식 라이브러리에 포함 된 경우 여러 개의 XAML 네임 스페이스 선언이 필요할 수 있습니다.

와 같은 사용자 지정 네임 스페이스 스키마를 정의 하는 또 다른 방법은 `http://mycompany.com/schemas/controls`, 하나 이상의 CLR 네임 스페이스에 매핑되는 합니다. 따라서 서로 다른 네임 스페이스에 있을 경우에 단일 XAML 네임 스페이스 선언 참조 어셈블리의 모든 형식에 있습니다. 또한 여러 어셈블리에 참조 형식에 대 한 단일 XAML 네임 스페이스 선언이 있습니다.

XAML 네임 스페이스에 대 한 자세한 내용은 참조 하세요. [Xamarin.Forms의 XAML 네임 스페이스](namespaces.md)합니다.

## <a name="defining-a-custom-namespace-schema"></a>사용자 지정 네임 스페이스 스키마를 정의합니다.

와 같은 몇 가지 간단한 컨트롤을 노출 하는 라이브러리를 포함 하는 샘플 응용 프로그램 `CircleButton`:

```csharp
using Xamarin.Forms;

namespace MyCompany.Controls
{
    public class CircleButton : Button
    {
        ...
    }
}
```

라이브러리에 있는 모든 컨트롤에는 `MyCompany.Controls` 네임 스페이스입니다. 이러한 컨트롤은 사용자 지정 네임 스페이스 스키마를 통해 호출 하는 어셈블리를 노출할 수 있습니다.

사용자 지정 네임 스페이스 스키마를 사용 하 여 정의 `XmlnsDefinitionAttribute` XAML 네임 스페이스와 하나 이상의 CLR 네임 스페이스 간의 매핑을 지정 하는 클래스입니다. `XmlnsDefinitionAttribute` 2 개 인수: XAML 네임 스페이스 이름과 CLR 네임 스페이스 이름입니다. 에 저장 된 XAML 네임 스페이스 이름 합니다 `XmlnsDefinitionAttribute.XmlNamespace` 속성 및 CLR 네임 스페이스 이름에 저장 됩니다는 `XmlnsDefinitionAttribute.ClrNamespace` 속성입니다.

> [!NOTE]
> 합니다 `XmlnsDefinitionAttribute` 클래스에 라는 속성이 `AssemblyName`, 어셈블리의 이름으로 선택적으로 설정할 수 있는 합니다. 만 경우 CLR 네임 스페이스에서 참조할 필요는 `XmlnsDefinitionAttribute` 외부 어셈블리에 있는 합니다.

`XmlnsDefinitionAttribute` 사용자 지정 네임 스페이스 스키마에 매핑되는 CLR 네임 스페이스를 포함 하는 프로젝트에서 어셈블리 수준에서 정의 되어야 합니다. 다음 예제는 **AssemblyInfo.cs** 샘플 응용 프로그램에서 파일:

```csharp
using Xamarin.Forms;
using MyCompany.Controls;

[assembly: Preserve]
[assembly: XmlnsDefinition("http://mycompany.com/schemas/controls", "MyCompany.Controls")]
```

이 코드에 매핑하는 사용자 지정 네임 스페이스 스키마를 만듭니다는 `http://mycompany.com/schemas/controls` 에 대 한 URL을 `MyCompany.Controls` CLR 네임 스페이스입니다. 또한는 `Preserve` 링커가 어셈블리의 모든 형식을 유지 하려면 어셈블리에 특성을 지정 합니다.

> [!IMPORTANT]
> `Preserve` 특성 매핑 사용자 지정 네임 스페이스 스키마를 통해 되거나 전체 어셈블리에 적용 되는 어셈블리의 클래스에 적용할 수 해야 합니다.

그런 다음 사용자 지정 네임 스페이스 스키마 XAML 파일에서 형식 확인에 사용할 수 있습니다.

## <a name="consuming-a-custom-namespace-schema"></a>사용자 지정 네임 스페이스 스키마를 사용합니다.

사용자 지정 네임 스페이스 스키마의 형식을 사용 하려면 XAML 컴파일러 형식을 사용 하는 어셈블리에서 코드 참조가 형식을 정의 하는 어셈블리에 필요 합니다. 포함 하는 클래스를 추가 하 여이 수행할 수 있습니다는 `Init` XAML을 통해 사용 될 형식을 정의 하는 어셈블리에는 메서드:

```csharp
namespace MyCompany.Controls
{
    public static class Controls
    {
        public static void Init()
        {
        }
    }
}
```

`Init` 메서드 형식 사용자 지정 네임 스페이스 스키마에서 사용 하는 어셈블리에서 호출할 수 있습니다.

```csharp
using Xamarin.Forms;
using MyCompany.Controls;

namespace CustomNamespaceSchemaDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            Controls.Init();
            InitializeComponent();
        }
    }
}
```

> [!WARNING]
> 사용자 지정 네임 스페이스 스키마 형식이 포함 된 어셈블리를 찾을 수 없는 XAML 컴파일러에서 참조 되는 이러한 코드를 포함 하지 않으면 발생 합니다.

사용 하 여 `CircleButton` 컨트롤이, XAML 네임 스페이스 선언 된, 사용자 지정 네임 스페이스 스키마 URL을 지정 하는 네임 스페이스 선언을 사용 하 여:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:controls="http://mycompany.com/schemas/controls"
             x:Class="CustomNamespaceSchemaDemo.MainPage">
    <StackLayout Margin="20,35,20,20">
        ...
        <controls:CircleButton Text="+"
                               BackgroundColor="Fuchsia"
                               BorderColor="Black"
                               CircleDiameter="100" />
        <controls:CircleButton Text="-"
                               BackgroundColor="Teal"
                               BorderColor="Silver"
                               CircleDiameter="70" />
        ...
    </StackLayout>
</ContentPage>
```

`CircleButton` 인스턴스를 추가할 수 있습니다 합니다 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 사용 하 여 선언 하 여는 `controls` 네임 스페이스 접두사입니다.

Xamarin.Forms는 참조 된 어셈블리에 대 한 사용자 지정 네임 스페이스 스키마 유형을 찾으려면 검색 `XmlnsDefinitionAttribute` 인스턴스. 경우는 `xmlns` XAML 파일의 요소에 대 한 특성이 일치를 `XmlNamespace` 속성 값을 `XmlnsDefinitionAttribute`, Xamarin.Forms를 사용 하려고 합니다.를 `XmlnsDefinitionAttribute.ClrNamespace` 형식 확인을 위해 속성 값입니다. Xamarin.Forms는 모든 추가 일치를 기준으로 하는 형식 확인을 시도 하도록 계속 형식 확인에 실패 하면 `XmlnsDefinitionAttribute` 인스턴스.

결과 두 개의 `CircleButton` 인스턴스가 표시 됩니다.

![단추 원](custom-namespace-schemas-images/circle-buttons.png "단추 원")

## <a name="related-links"></a>관련 링크

- [사용자 지정 Namespace 스키마 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/XAML/CustomNamespaceSchemas/)
- [Xamarin.Forms의 XAML 네임 스페이스](namespaces.md)
