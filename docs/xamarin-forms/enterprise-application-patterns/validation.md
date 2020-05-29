---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4a9af91e2d48ba7ef7fdcdb4f8472e0aaafb7854
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138712"
---
# <a name="validation-in-enterprise-apps"></a>엔터프라이즈 앱의 유효성 검사

사용자의 입력을 허용 하는 모든 앱은 입력이 올바른지 확인 해야 합니다. 예를 들어 앱은 특정 범위의 문자만 포함 하는 입력을 확인 하거나 특정 길이 이거나 특정 형식과 일치 시킬 수 있습니다. 유효성 검사를 수행 하지 않으면 사용자가 응용 프로그램 실패를 유발 하는 데이터를 제공할 수 있습니다. 유효성 검사는 비즈니스 규칙을 적용 하 고 공격자가 악성 데이터를 삽입 하는 것을 방지 합니다.

MVVM (모델-뷰-ViewModel) 패턴의 컨텍스트에서는 사용자가 수정할 수 있도록 데이터 유효성 검사를 수행 하 고 뷰에 유효성 검사 오류를 알리기 위해 뷰 모델 또는 모델이 종종 필요 합니다. EShopOnContainers 모바일 앱은 뷰 모델 속성에 대 한 동기 클라이언트 쪽 유효성 검사를 수행 하 고, 잘못 된 데이터가 포함 된 컨트롤을 강조 표시 하 고, 데이터가 잘못 된 이유를 사용자에 게 알리는 오류 메시지를 표시 하 여 사용자에 게 유효성 검사 오류를 알립니다. 그림 6-1에서는 eShopOnContainers 모바일 앱에서 유효성 검사를 수행 하는 데 관련 된 클래스를 보여 줍니다.

[![](validation-images/validation.png "Validation classes in the eShopOnContainers mobile app")](validation-images/validation-large.png#lightbox "Validation classes in the eShopOnContainers mobile app")

**그림 6-1**: eShopOnContainers 모바일 앱의 유효성 검사 클래스

유효성 검사가 필요한 뷰 모델 속성은 유형이 `ValidatableObject<T>` 며 각 `ValidatableObject<T>` 인스턴스의 속성에 유효성 검사 규칙이 추가 됩니다 `Validations` . 유효성 검사는 `Validate` `ValidatableObject<T>` 유효성 검사 규칙을 검색 하 고 속성에 대해 실행 하는 인스턴스의 메서드를 호출 하 여 뷰 모델에서 호출 됩니다 `ValidatableObject<T>` `Value` . 유효성 검사 오류는 인스턴스의 속성에 배치 되 `Errors` `ValidatableObject<T>` 고 `IsValid` 인스턴스의 속성은 `ValidatableObject<T>` 유효성 검사의 성공 또는 실패 여부를 표시 하도록 업데이트 됩니다.

속성 변경 알림은 클래스에서 제공 `ExtendedBindableObject` 하므로, [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤은 `IsValid` `ValidatableObject<T>` 입력 된 데이터가 유효한 지 여부에 대 한 알림을 받기 위해 뷰 모델 클래스의 인스턴스 속성에 바인딩할 수 있습니다.

## <a name="specifying-validation-rules"></a>유효성 검사 규칙 지정

유효성 검사 규칙은 `IValidationRule<T>` 다음 코드 예제와 같이 인터페이스에서 파생 되는 클래스를 만들어 지정 합니다.

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

이 인터페이스는 유효성 검사 규칙 클래스가 필요한 유효성 검사를 수행 하는 데 사용 되는 메서드를 제공 해야 하도록 지정 하 `boolean` `Check` 고, `ValidationMessage` 유효성 검사에 실패 하면 표시 되는 유효성 검사 오류 메시지를 값으로 갖는 속성을 지정 합니다.

다음 코드 예제에서는 `IsNotNullOrEmptyRule<T>` `LoginView` eShopOnContainers 모바일 앱에서 모의 서비스를 사용할 때 사용자가 입력 한 사용자 이름 및 암호의 유효성 검사를 수행 하는 데 사용 되는 유효성 검사 규칙을 보여 줍니다.

```csharp
public class IsNotNullOrEmptyRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        return !string.IsNullOrWhiteSpace(str);  
    }  
}
```

`Check`메서드는 `boolean` 값 인수가 `null` 이거나 비어 있거나 공백 문자로만 구성 되어 있는지 여부를 나타내는을 반환 합니다.

EShopOnContainers 모바일 앱에서 사용 되지 않지만 다음 코드 예제에서는 전자 메일 주소의 유효성을 검사 하는 유효성 검사 규칙을 보여 줍니다.

```csharp
public class EmailRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        Regex regex = new Regex(@"^([\w\.\-]+)@([\w\-]+)((\.(\w){2,3})+)$");  
        Match match = regex.Match(str);  

        return match.Success;  
    }  
}
```

`Check`메서드는 `boolean` 값 인수가 유효한 전자 메일 주소 인지 여부를 나타내는을 반환 합니다. 이는 생성자에 지정 된 정규식 패턴의 첫 번째 항목에 대 한 값 인수를 검색 하 여 수행 됩니다 `Regex` . 입력 문자열에서 정규식 패턴을 찾을 수 있는지 여부는 개체의 속성 값을 확인 하 여 확인할 수 있습니다 `Match` `Success` .

> [!NOTE]
> 속성 유효성 검사에는 종속 속성이 포함 될 수도 있습니다. 종속 속성의 예는 속성 A의 유효한 값 집합이 속성 B에 설정 된 특정 값에 따라 달라 지는 경우입니다. 속성 A의 값이 허용 되는 값 중 하나 인지 확인 하려면 속성 B의 값을 검색 해야 합니다. 또한 속성 B의 값이 변경 되 면 A 속성의 유효성을 다시 검사 해야 합니다.

## <a name="adding-validation-rules-to-a-property"></a>속성에 유효성 검사 규칙 추가

EShopOnContainers 모바일 앱에서 유효성 검사가 필요한 뷰 모델 속성은 형식으로 선언 됩니다 `ValidatableObject<T>` `T` . 여기서은 유효성을 검사할 데이터의 형식입니다. 다음 코드 예제에서는 이러한 두 속성의 예를 보여 줍니다.

```csharp
public ValidatableObject<string> UserName  
{  
    get  
    {  
        return _userName;  
    }  
    set  
    {  
        _userName = value;  
        RaisePropertyChanged(() => UserName);  
    }  
}  

public ValidatableObject<string> Password  
{  
    get  
    {  
        return _password;  
    }  
    set  
    {  
        _password = value;  
        RaisePropertyChanged(() => Password);  
    }  
}
```

유효성 검사를 수행 하려면 `Validations` `ValidatableObject<T>` 다음 코드 예제에서 보여 주는 것 처럼 유효성 검사 규칙을 각 인스턴스의 컬렉션에 추가 해야 합니다.

```csharp
private void AddValidations()  
{  
    _userName.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A username is required."   
    });  
    _password.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A password is required."   
    });  
}
```

이 메서드는 유효성 검사에 실패 한 경우 표시 되는 유효성 검사 오류 메시지를 지정 하는 유효성 검사 `IsNotNullOrEmptyRule<T>` `Validations` 규칙의 `ValidatableObject<T>` 속성에 대 한 값을 지정 하 여 각 인스턴스의 컬렉션에 유효성 검사 규칙을 추가 합니다 `ValidationMessage` .

## <a name="triggering-validation"></a>유효성 검사 트리거

EShopOnContainers 모바일 앱에서 사용 되는 유효성 검사 방식은 속성의 유효성 검사를 수동으로 트리거하고 속성이 변경 될 때 자동으로 유효성 검사를 트리거할 수 있습니다.

### <a name="triggering-validation-manually"></a>수동으로 유효성 검사 트리거

뷰 모델 속성에 대해 유효성 검사를 수동으로 트리거할 수 있습니다. 예를 들어, 사용자가 모의 서비스를 사용할 때에서 **로그인** 단추를 누르면 eShopOnContainers 모바일 앱에서이 오류가 발생 합니다 `LoginView` . 명령 대리자는의 메서드를 호출 합니다 .이 메서드는 `MockSignInAsync` `LoginViewModel` `Validate` 다음 코드 예제와 같이 메서드를 실행 하 여 유효성 검사를 호출 합니다.

```csharp
private bool Validate()  
{  
    bool isValidUser = ValidateUserName();  
    bool isValidPassword = ValidatePassword();  
    return isValidUser && isValidPassword;  
}  

private bool ValidateUserName()  
{  
    return _userName.Validate();  
}  

private bool ValidatePassword()  
{  
    return _password.Validate();  
}
```

`Validate`메서드는 `LoginView` 각 인스턴스에서 Validate 메서드를 호출 하 여 사용자가 입력 한 사용자 이름과 암호의 유효성을 검사 합니다 `ValidatableObject<T>` . 다음 코드 예제에서는 클래스의 Validate 메서드를 보여 줍니다 `ValidatableObject<T>` .

```csharp
public bool Validate()  
{  
    Errors.Clear();  

    IEnumerable<string> errors = _validations  
        .Where(v => !v.Check(Value))  
        .Select(v => v.ValidationMessage);  

    Errors = errors.ToList();  
    IsValid = !Errors.Any();  

    return this.IsValid;  
}
```

이 메서드는 컬렉션을 지운 `Errors` 다음 개체의 컬렉션에 추가 된 유효성 검사 규칙을 검색 `Validations` 합니다. `Check`검색 된 각 유효성 검사 규칙에 대 한 메서드가 실행 되 고, `ValidationMessage` 데이터 유효성 검사에 실패 한 모든 유효성 검사 규칙의 속성 값이 `Errors` 인스턴스의 컬렉션에 추가 됩니다 `ValidatableObject<T>` . 마지막으로 `IsValid` 속성이 설정 되 고, 해당 값이 호출 메서드에 반환 되어 유효성 검사의 성공 여부를 나타냅니다.

### <a name="triggering-validation-when-properties-change"></a>속성이 변경 될 때 유효성 검사 트리거

바인딩된 속성이 변경 될 때마다 유효성 검사를 트리거할 수도 있습니다. 예를 들어의 양방향 바인딩이 `LoginView` 또는 속성을 설정 하는 경우 `UserName` `Password` 유효성 검사가 트리거됩니다. 다음 코드 예제에서는이 작업을 수행 하는 방법을 보여 줍니다.

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    <Entry.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="TextChanged"  
            Command="{Binding ValidateUserNameCommand}" />  
    </Entry.Behaviors>  
    ...  
</Entry>
```

[`Entry`](xref:Xamarin.Forms.Entry)컨트롤이 인스턴스의 속성에 바인딩되고 `UserName.Value` `ValidatableObject<T>` 컨트롤의 `Behaviors` 컬렉션에 `EventToCommandBehavior` 인스턴스가 추가 됩니다. 이 동작은에서 `ValidateUserNameCommand` 발생 하는 [] 이벤트에 대 한 응답으로를 실행 합니다 .이 이벤트는의 `TextChanged` `Entry` 텍스트가 변경 될 때 발생 합니다 `Entry` . 그러면 `ValidateUserNameCommand` 대리자는 `ValidateUserName` 인스턴스에서 메서드를 실행 하는 메서드를 실행 합니다 `Validate` `ValidatableObject<T>` . 따라서 사용자가 컨트롤의 사용자 이름에 대해 문자를 입력할 때마다 `Entry` 입력 한 데이터의 유효성 검사가 수행 됩니다.

동작에 대 한 자세한 내용은 [동작 구현](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors)을 참조 하세요.

<a name="displaying_validation_errors" />

## <a name="displaying-validation-errors"></a>유효성 검사 오류 표시

EShopOnContainers 모바일 앱은 잘못 된 데이터를 포함 하는 컨트롤을 빨간색 줄로 강조 표시 하 고 잘못 된 데이터를 포함 하는 컨트롤 아래에서 데이터가 잘못 된 이유를 사용자에 게 알리는 오류 메시지를 표시 하 여 사용자에 게 유효성 검사 오류를 알립니다. 잘못 된 데이터를 수정 하면 줄이 검정색으로 바뀌고 오류 메시지가 제거 됩니다. 그림 6-2에서는 유효성 검사 오류가 있을 때 eShopOnContainers 모바일 앱의 LoginView를 보여 줍니다.

![](validation-images/validation-login.png "Displaying validation errors during login")

**그림 6-2:** 로그인 중 유효성 검사 오류 표시

### <a name="highlighting-a-control-that-contains-invalid-data"></a>잘못 된 데이터를 포함 하는 컨트롤 강조 표시

`LineColorBehavior`연결 된 동작은 [`Entry`](xref:Xamarin.Forms.Entry) 유효성 검사 오류가 발생 한 컨트롤을 강조 표시 하는 데 사용 됩니다. 다음 코드 예제에서는 `LineColorBehavior` 연결 된 동작을 컨트롤에 연결 하는 방법을 보여 줍니다 `Entry` .

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">
    <Entry.Style>
        <OnPlatform x:TypeArguments="Style">
            <On Platform="iOS, Android" Value="{StaticResource EntryStyle}" />
            <On Platform="UWP" Value="{StaticResource UwpEntryStyle}" />
        </OnPlatform>
    </Entry.Style>
    ...
</Entry>
```

[`Entry`](xref:Xamarin.Forms.Entry)컨트롤은 다음 코드 예제와 같이 명시적 스타일을 사용 합니다.

```xaml
<Style x:Key="EntryStyle"  
       TargetType="{x:Type Entry}">  
    ...  
    <Setter Property="behaviors:LineColorBehavior.ApplyLineColor"  
            Value="True" />  
    <Setter Property="behaviors:LineColorBehavior.LineColor"  
            Value="{StaticResource BlackColor}" />  
    ...  
</Style>
```

이 스타일은 `ApplyLineColor` `LineColor` 컨트롤에 연결 된 동작의 및 연결 된 속성을 설정 합니다 `LineColorBehavior` [`Entry`](xref:Xamarin.Forms.Entry) . 스타일에 대한 자세한 내용은 [스타일](~/xamarin-forms/user-interface/styles/index.md)을 참조하세요.

연결 된 속성의 값 `ApplyLineColor` 이 설정 되거나 변경 되 면 `LineColorBehavior` 연결 된 동작은 메서드를 실행 합니다 `OnApplyLineColorChanged` .이 메서드는 다음 코드 예제에 나와 있습니다.

```csharp
public static class LineColorBehavior  
{  
    ...  
    private static void OnApplyLineColorChanged(  
                BindableObject bindable, object oldValue, object newValue)  
    {  
        var view = bindable as View;  
        if (view == null)  
        {  
            return;  
        }  

        bool hasLine = (bool)newValue;  
        if (hasLine)  
        {  
            view.Effects.Add(new EntryLineColorEffect());  
        }  
        else  
        {  
            var entryLineColorEffectToRemove =   
                    view.Effects.FirstOrDefault(e => e is EntryLineColorEffect);  
            if (entryLineColorEffectToRemove != null)  
            {  
                view.Effects.Remove(entryLineColorEffectToRemove);  
            }  
        }  
    }  
}
```

이 메서드의 매개 변수는 동작이 연결 된 컨트롤의 인스턴스와 연결 된 속성의 이전 값과 새 값을 제공 합니다 `ApplyLineColor` . `EntryLineColorEffect`클래스는 연결 된 속성이 인 경우 컨트롤의 컬렉션에 추가 되 고 [`Effects`](xref:Xamarin.Forms.Element.Effects) `ApplyLineColor` `true` , 그렇지 않으면 컨트롤의 컬렉션에서 제거 됩니다 `Effects` . 동작에 대 한 자세한 내용은 [동작 구현](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors)을 참조 하세요.

`EntryLineColorEffect` [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) 클래스는 클래스를, 다음 코드 예제에서 볼 수 있습니다.

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

클래스는 플랫폼별 효과를 [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) 래핑하여 플랫폼별 효과를 나타냅니다. 이는 플랫폼별 효과에 대한 형식 정보에 컴파일 시간 액세스가 없으므로 효과 제거 프로세스를 간소화합니다. 는 `EntryLineColorEffect` 기본 클래스 생성자를 호출 하 여 해상도 그룹 이름 연결로 구성 된 매개 변수를 전달 하 고 각 플랫폼별 효과 클래스에서 지정 된 고유 ID를 전달 합니다.

다음 코드 예제에서는 `eShopOnContainers.EntryLineColorEffect` iOS에 대 한 구현을 보여 줍니다.

```csharp
[assembly: ResolutionGroupName("eShopOnContainers")]  
[assembly: ExportEffect(typeof(EntryLineColorEffect), "EntryLineColorEffect")]  
namespace eShopOnContainers.iOS.Effects  
{  
    public class EntryLineColorEffect : PlatformEffect  
    {  
        UITextField control;  

        protected override void OnAttached()  
        {  
            try  
            {  
                control = Control as UITextField;  
                UpdateLineColor();  
            }  
            catch (Exception ex)  
            {  
                Console.WriteLine("Can't set property on attached control. Error: ", ex.Message);  
            }  
        }  

        protected override void OnDetached()  
        {  
            control = null;  
        }  

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)  
        {  
            base.OnElementPropertyChanged(args);  

            if (args.PropertyName == LineColorBehavior.LineColorProperty.PropertyName ||  
                args.PropertyName == "Height")  
            {  
                Initialize();  
                UpdateLineColor();  
            }  
        }  

        private void Initialize()  
        {  
            var entry = Element as Entry;  
            if (entry != null)  
            {  
                Control.Bounds = new CGRect(0, 0, entry.Width, entry.Height);  
            }  
        }  

        private void UpdateLineColor()  
        {  
            BorderLineLayer lineLayer = control.Layer.Sublayers.OfType<BorderLineLayer>()  
                                                             .FirstOrDefault();  

            if (lineLayer == null)  
            {  
                lineLayer = new BorderLineLayer();  
                lineLayer.MasksToBounds = true;  
                lineLayer.BorderWidth = 1.0f;  
                control.Layer.AddSublayer(lineLayer);  
                control.BorderStyle = UITextBorderStyle.None;  
            }  

            lineLayer.Frame = new CGRect(0f, Control.Frame.Height-1f, Control.Bounds.Width, 1f);  
            lineLayer.BorderColor = LineColorBehavior.GetLineColor(Element).ToCGColor();  
            control.TintColor = control.TextColor;  
        }  

        private class BorderLineLayer : CALayer  
        {  
        }  
    }  
}
```

`OnAttached`메서드는 컨트롤의 네이티브 컨트롤을 검색 Xamarin.Forms [`Entry`](xref:Xamarin.Forms.Entry) 하 고 메서드를 호출 하 여 선 색을 업데이트 합니다 `UpdateLineColor` . `OnElementPropertyChanged`재정의는 `Entry` 연결 된 속성이 변경 될 경우 선 색을 업데이트 하 여 컨트롤의 바인딩 가능한 속성 변경 내용에 응답 하 고 `LineColor` , [`Height`](xref:Xamarin.Forms.VisualElement.Height) 변경의 속성을 변경 `Entry` 합니다. 효과에 대한 자세한 내용은 [효과](~/xamarin-forms/app-fundamentals/effects/index.md)를 참조하세요.

컨트롤에 유효한 데이터가 입력 되 면 [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤 아래쪽에 검정 줄이 적용 되어 유효성 검사 오류가 없음을 표시 합니다. 그림 6-3에서는이에 대 한 예를 보여 줍니다.

![](validation-images/validation-blackline.png "Black line indicating no validation error")

**그림 6-3**: 유효성 검사 오류를 나타내는 검정 선

[`Entry`](xref:Xamarin.Forms.Entry)컨트롤에는 [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) 해당 컬렉션에 추가 된도 있습니다 [`Triggers`](xref:Xamarin.Forms.VisualElement.Triggers) . 다음 코드 예제에서는을 보여 줍니다 `DataTrigger` .

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    ...  
    <Entry.Triggers>  
        <DataTrigger   
            TargetType="Entry"  
            Binding="{Binding UserName.IsValid}"  
            Value="False">  
            <Setter Property="behaviors:LineColorBehavior.LineColor"   
                    Value="{StaticResource ErrorColor}" />  
        </DataTrigger>  
    </Entry.Triggers>  
</Entry>
```

그러면 [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) 속성이 모니터링 되 `UserName.IsValid` 고, 값이 이면가 실행 되어 연결 된 `false` [`Setter`](xref:Xamarin.Forms.Setter) `LineColor` 동작의 연결 된 속성이 빨강으로 변경 됩니다 `LineColorBehavior` . 그림 6-4에서는이에 대 한 예를 보여 줍니다.

![](validation-images/validation-redline.png "Red line indicating validation error")

**그림 6-4**: 유효성 검사 오류를 나타내는 빨간색 선

입력 한 데이터가 유효 [`Entry`](xref:Xamarin.Forms.Entry) 하지 않으면 컨트롤의 줄은 빨간색으로 유지 되 고, 그렇지 않은 경우에는 입력 된 데이터가 유효한 것으로 표시 되도록 검은색으로 변경 됩니다.

트리거에 대 한 자세한 내용은 [트리거](~/xamarin-forms/app-fundamentals/triggers.md)를 참조 하세요.

### <a name="displaying-error-messages"></a>오류 메시지 표시

UI는 데이터가 유효성 검사에 실패 한 각 컨트롤 아래의 레이블 컨트롤에서 유효성 검사 오류 메시지를 표시 합니다. 다음 코드 예제에서는 사용자가 [`Label`](xref:Xamarin.Forms.Label) 올바른 사용자 이름을 입력 하지 않은 경우 유효성 검사 오류 메시지를 표시 하는를 보여 줍니다.

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

각는 [`Label`](xref:Xamarin.Forms.Label) `Errors` 유효성 검사 중인 뷰 모델 개체의 속성에 바인딩됩니다. `Errors`속성은 클래스에서 제공 되며 `ValidatableObject<T>` 형식입니다 `List<string>` . 속성은 `Errors` 여러 유효성 검사 오류를 포함할 수 있으므로 `FirstValidationErrorConverter` 인스턴스는 표시를 위해 컬렉션에서 첫 번째 오류를 검색 하는 데 사용 됩니다.

## <a name="summary"></a>요약

EShopOnContainers 모바일 앱은 뷰 모델 속성에 대 한 동기식 클라이언트 쪽 유효성 검사를 수행 하 고, 잘못 된 데이터가 포함 된 컨트롤을 강조 표시 하 고, 데이터가 잘못 된 이유를 사용자에 게 알리는 오류 메시지를 표시 하 여 사용자에 게 유효성 검사 오류를 알립니다.

유효성 검사가 필요한 뷰 모델 속성은 유형이 `ValidatableObject<T>` 며 각 `ValidatableObject<T>` 인스턴스의 속성에 유효성 검사 규칙이 추가 됩니다 `Validations` . 유효성 검사는 `Validate` `ValidatableObject<T>` 유효성 검사 규칙을 검색 하 고 속성에 대해 실행 하는 인스턴스의 메서드를 호출 하 여 뷰 모델에서 호출 됩니다 `ValidatableObject<T>` `Value` . 유효성 검사 오류는 인스턴스의 속성에 배치 되 `Errors` `ValidatableObject<T>` 고 `IsValid` 인스턴스의 속성은 `ValidatableObject<T>` 유효성 검사의 성공 또는 실패 여부를 표시 하도록 업데이트 됩니다.

## <a name="related-links"></a>관련 링크

- [다운로드 전자책 (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
