---
제목: "" description: "의 XAML 사용자 지정 네임 스페이스 스키마 Xamarin.Forms -사용자 지정 URL과 하나 이상의 CLR 네임 스페이스 간의 매핑을 지정 하는 매핑하기 클래스를 사용 하 여 xaml 사용자 지정 네임 스페이스 스키마를 정의할 수 있습니다. 그런 다음 사용자 지정 네임 스페이스 스키마를 XAML 네임 스페이스 선언에 사용할 수 있습니다. "
assetid: FDF201A1-8C35-4569-A728-F9B0A0C5B31A: xamarin-forms author: davidbritch: dabritch:: 12/21/2018-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xaml-custom-namespace-schemas-in-xamarinforms"></a>의 XAML 사용자 지정 네임 스페이스 스키마Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-customnamespaceschemas)

라이브러리에 대 한 XAML 네임 스페이스를 선언 하 고 CLR (공용 언어 런타임) 네임 스페이스 이름과 어셈블리 이름을 지정 하는 네임 스페이스 선언을 사용 하 여 라이브러리의 형식을 XAML에서 참조할 수 있습니다.

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:MyCompany.Controls;assembly=MyCompany.Controls">
    ...
</ContentPage>
```

그러나 정의에 CLR 네임 스페이스와 어셈블리 이름을 지정 하는 `xmlns` 것은 어렵고 오류가 발생할 수 있습니다. 또한 라이브러리에 여러 네임 스페이스의 형식이 포함 되어 있으면 XAML 네임 스페이스 선언이 여러 개 필요할 수 있습니다.

또 다른 방법은 `http://mycompany.com/schemas/controls` 하나 이상의 CLR 네임 스페이스에 매핑되는와 같은 사용자 지정 네임 스페이스 스키마를 정의 하는 것입니다. 이를 통해 단일 XAML 네임 스페이스 선언이 다른 네임 스페이스에 있는 경우에도 어셈블리의 모든 형식을 참조할 수 있습니다. 또한 단일 XAML 네임 스페이스 선언이 여러 어셈블리의 형식을 참조할 수 있습니다.

XAML 네임 스페이스에 대 한 자세한 내용은 [의 Xamarin.Forms Xaml 네임 스페이스 ](namespaces.md)를 참조 하세요.

## <a name="defining-a-custom-namespace-schema"></a>사용자 지정 네임 스페이스 스키마 정의

샘플 응용 프로그램에는 다음과 같은 몇 가지 간단한 컨트롤을 노출 하는 라이브러리가 포함 되어 있습니다 `CircleButton` .

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

라이브러리의 모든 컨트롤은 `MyCompany.Controls` 네임 스페이스에 있습니다. 이러한 컨트롤은 사용자 지정 네임 스페이스 스키마를 통해 호출 어셈블리에 노출 될 수 있습니다.

사용자 지정 네임 스페이스 스키마는 `XmlnsDefinitionAttribute` XAML 네임 스페이스와 하나 이상의 CLR 네임 스페이스 간의 매핑을 지정 하는 클래스를 사용 하 여 정의 됩니다. 는 `XmlnsDefinitionAttribute` XAML 네임 스페이스 이름과 CLR 네임 스페이스 이름 이라는 두 가지 인수를 사용 합니다. XAML 네임 스페이스 이름은 속성에 저장 되 `XmlnsDefinitionAttribute.XmlNamespace` 고, CLR 네임 스페이스 이름은 속성에 저장 됩니다 `XmlnsDefinitionAttribute.ClrNamespace` .

> [!NOTE]
> `XmlnsDefinitionAttribute`또한 클래스에는 `AssemblyName` 어셈블리 이름으로 선택적으로 설정 될 수 있는 라는 속성이 있습니다. 이는에서 참조 된 CLR 네임 스페이스가 `XmlnsDefinitionAttribute` 외부 어셈블리에 있는 경우에만 필요 합니다.

는 `XmlnsDefinitionAttribute` 사용자 지정 네임 스페이스 스키마에 매핑될 CLR 네임 스페이스를 포함 하는 프로젝트의 어셈블리 수준에서 정의 되어야 합니다. 다음 예제에서는 샘플 응용 프로그램의 **AssemblyInfo.cs** 파일을 보여 줍니다.

```csharp
using Xamarin.Forms;
using MyCompany.Controls;

[assembly: Preserve]
[assembly: XmlnsDefinition("http://mycompany.com/schemas/controls", "MyCompany.Controls")]
```

이 코드는 `http://mycompany.com/schemas/controls` URL을 CLR 네임 스페이스에 매핑하는 사용자 지정 네임 스페이스 스키마를 만듭니다 `MyCompany.Controls` . 또한 어셈블리에서 `Preserve` 특성을 지정 하 여 링커가 어셈블리의 모든 형식을 유지할 수 있도록 합니다.

> [!IMPORTANT]
> `Preserve`특성은 사용자 지정 네임 스페이스 스키마를 통해 매핑되거나 전체 어셈블리에 적용 되는 어셈블리의 클래스에 적용 해야 합니다.

그런 다음 사용자 지정 네임 스페이스 스키마를 XAML 파일의 형식 확인에 사용할 수 있습니다.

## <a name="consuming-a-custom-namespace-schema"></a>사용자 지정 네임 스페이스 스키마 사용

사용자 지정 네임 스페이스 스키마의 형식을 사용 하려면 XAML 컴파일러에서 형식을 정의 하는 어셈블리에 대 한 형식을 사용 하는 어셈블리의 코드 참조가 필요 합니다. 이 작업을 수행 하려면 `Init` XAML을 통해 사용 되는 형식을 정의 하는 어셈블리에 메서드를 포함 하는 클래스를 추가 합니다.

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

`Init`그런 다음 사용자 지정 네임 스페이스 스키마에서 형식을 사용 하는 어셈블리에서 메서드를 호출할 수 있습니다.

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
> 이러한 코드 참조를 포함 하지 않으면 XAML 컴파일러에서 사용자 지정 네임 스페이스 스키마 형식이 포함 된 어셈블리를 찾을 수 없습니다.

컨트롤을 사용 하기 위해 `CircleButton` 사용자 지정 네임 스페이스 스키마 URL을 지정 하는 네임 스페이스 선언을 사용 하 여 XAML 네임 스페이스가 선언 됩니다.

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

`CircleButton`그런 다음 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 네임 스페이스 접두사를 사용 하 여 인스턴스를 선언 하 여에 추가할 수 있습니다 `controls` .

사용자 지정 네임 스페이스 스키마 형식을 찾기 위해에서 Xamarin.Forms 참조 된 어셈블리의 인스턴스를 검색 합니다 `XmlnsDefinitionAttribute` . `xmlns`XAML 파일의 요소에 대 한 특성이의 속성 값과 일치 하면는 `XmlNamespace` `XmlnsDefinitionAttribute` Xamarin.Forms `XmlnsDefinitionAttribute.ClrNamespace` 형식 확인을 위해 속성 값을 사용 하려고 합니다. 형식 확인에 실패 하는 경우는 Xamarin.Forms 추가 일치 하는 인스턴스를 기반으로 하는 형식 확인을 계속 시도 `XmlnsDefinitionAttribute` 합니다.

그러면 두 개의 `CircleButton` 인스턴스가 표시 됩니다.

![원 단추](custom-namespace-schemas-images/circle-buttons.png "원 단추")

## <a name="related-links"></a>관련 링크

- [사용자 지정 네임 스페이스 스키마 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-customnamespaceschemas)
- [XAML Namespace 권장 접두사](custom-prefix.md)
- [XAML 네임 스페이스Xamarin.Forms](namespaces.md)
