---
제목: " Xamarin.Forms " description: "의 XAML 필드 한정자 x:FieldModifier namespace 특성은 명명 된 XAML 요소에 대해 생성 된 필드의 액세스 수준을 지정 합니다."
assetid: 12357CE0-3C11-4B62-947F-72DB6DFC23A2: xamarin-forms author: davidbritch: dabritch:: 08/02/2019-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xaml-field-modifiers-in-xamarinforms"></a>의 XAML 필드 한정자Xamarin.Forms

`x:FieldModifier`Namespace 특성은 명명 된 XAML 요소에 대해 생성 된 필드의 액세스 수준을 지정 합니다. 특성의 유효한 값은 다음과 같습니다.

- `private`– XAML 요소에 대해 생성 된 필드가 선언 된 클래스의 본문 내 에서만 액세스할 수 있도록 지정 합니다.
- `public`– XAML 요소에 대해 생성 된 필드에 액세스 제한이 없도록 지정 합니다.
- `protected`– XAML 요소에 대해 생성 된 필드를 해당 클래스 내에서 파생 클래스 인스턴스로 액세스할 수 있도록 지정 합니다.
- `internal`– XAML 요소에 대해 생성 된 필드가 동일한 어셈블리의 형식 내 에서만 액세스할 수 있도록 지정 합니다.
- `notpublic`– XAML 요소에 대해 생성 된 필드가 동일한 어셈블리의 형식 내 에서만 액세스할 수 있도록 지정 합니다.

기본적으로 특성 값이 설정 되지 않은 경우 요소에 대해 생성 된 필드는가 됩니다 `private` .

> [!NOTE]
> 특성의 값은 소문자로 변환 되므로 모든 대/소문자를 사용할 수 있습니다 Xamarin.Forms .

특성을 처리 하려면 다음 조건을 충족 해야 합니다 `x:FieldModifier` .

- 최상위 XAML 요소는 유효한 이어야 합니다 `x:Class` .
- 현재 XAML 요소에 지정 된가 `x:Name` 있습니다.

다음 XAML은 특성을 설정 하는 예를 보여 줍니다.

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="internal" />
<Label x:Name="publicLabel" x:FieldModifier="public" />
```

> [!IMPORTANT]
> `x:FieldModifier`특성은 XAML 클래스의 액세스 수준을 지정 하는 데 사용할 수 없습니다.
