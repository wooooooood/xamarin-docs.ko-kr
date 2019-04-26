---
title: 엔터프라이즈 앱의 유효성 검사
description: 이 장에서 eShopOnContainers 모바일 앱에서 사용자 입력의 유효성 검사를 수행 하는 방법을 설명 합니다. 여기에 유효성 검사 규칙을 지정 하 고, 유효성 검사를 트리거하지 유효성 검사 오류를 표시 합니다.
ms.prod: xamarin
ms.assetid: 56e4f0fc-48d9-4033-91ec-173bb46a5e4d
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 22b5fe703486f0ded3a5b91241e3fe5ce41bbc98
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61298945"
---
# <a name="validation-in-enterprise-apps"></a>엔터프라이즈 앱의 유효성 검사

사용자 로부터 입력을 허용 하는 모든 앱에 유효한 입력 되었는지 확인 해야 합니다. 앱 특정 범위에 있는 문자만 포함, 특정 길이입니다. 또는 특정 형식과 일치 하는 입력에 대 한 확인 예를 들어 수 있습니다. 유효성 검사 없이 사용자는 응용 프로그램에서 실패를 발생 시키는 데이터를 제공할 수 있습니다. 비즈니스 규칙을 적용 하 고 공격자가 악성 데이터 주입 하지 않도록 설정 하는 유효성 검사 합니다.

모델-뷰-ViewModel (MVVM)의 컨텍스트에서 패턴, 보기 모델 또는 모델 종종 해야 데이터 유효성 검사를 수행 하 고 사용자 해결할 수 있도록 보기로 유효성 검사 오류를 알립니다. EShopOnContainers 모바일 앱 보기 모델 속성의 동기 클라이언트 쪽 유효성 검사를 수행 하 고 사용자를 제공 하는 오류 메시지를 표시 하 고 잘못 된 데이터를 포함 하는 컨트롤을 강조 표시 하 여 유효성 검사 오류가 발생 하는 사용자에 게 알립니다. 데이터를 잘못 된 이유입니다. 그림 6-1 eShopOnContainers 모바일 앱에서 유효성 검사를 수행과 관련 된 클래스를 보여 줍니다.

[![](validation-images/validation.png "EShopOnContainers 모바일 앱에서 유효성 검사 클래스")](validation-images/validation-large.png#lightbox "eShopOnContainers 모바일 앱에서 유효성 검사 클래스")

**그림 6-1**: EShopOnContainers 모바일 앱에서 유효성 검사 클래스

유효성 검사를 요구 하는 보기 모델 속성은 `ValidatableObject<T>`, 및 각 `ValidatableObject<T>` 인스턴스를 추가 하는 유효성 검사 규칙에 해당 `Validations` 속성입니다. 유효성 검사를 호출 하 여 뷰 모델에서 호출를 `Validate` 메서드의 `ValidatableObject<T>` 인스턴스 유효성 검사를 검색 하는 규칙 및에 대해 실행 합니다 `ValidatableObject<T>` `Value` 속성입니다. 유효성 검사 오류에 배치 됩니다는 `Errors` 의 속성을 `ValidatableObject<T>` 인스턴스를 및 `IsValid` 의 속성은 `ValidatableObject<T>` 인스턴스 유효성 검사의 성공 여부를 나타내도록 업데이트 됩니다.

속성 변경 알림을 제공 하는 `ExtendedBindableObject` 클래스 등을 [ `Entry` ](xref:Xamarin.Forms.Entry) 컨트롤에 바인딩할 수는 `IsValid` 속성 `ValidatableObject<T>` 설정할지 여부를 알릴 보기 모델 클래스의 인스턴스 입력 한 데이터가 유효합니다.

## <a name="specifying-validation-rules"></a>유효성 검사 규칙 지정

파생 되는 클래스를 만들어 지정 된 유효성 검사 규칙을 `IValidationRule<T>` 다음 코드 예제에 나와 있는 인터페이스:

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

이 인터페이스에 유효성 검사 규칙 클래스를 제공 하는 지정 된 `boolean` `Check` 필요한 유효성 검사를 수행 하는 데 사용 되는 메서드 및 `ValidationMessage` 속성 값인 경우 표시 되는 유효성 검사 오류 메시지 유효성 검사에 실패 합니다.

다음 코드 예제는 `IsNotNullOrEmptyRule<T>` 사용자 이름 및 암호에 사용자가 입력의 유효성을 검사 하는 데 사용 되는 유효성 검사 규칙을 `LoginView` eShopOnContainers 모바일 앱에서 모의 서비스를 사용 하는 경우:

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

`Check` 메서드가 반환 되는 `boolean` 값 인수 인지 여부를 나타내는 `null`이거나 비어 있거나 공백 문자로 구성 되어 합니다.

EShopOnContainers 모바일 앱에서 사용 되지 않지만 다음 코드 예제에서는 전자 메일 주소 유효성 검사에 대 한 유효성 검사 규칙을 보여 줍니다.

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

합니다 `Check` 메서드가 반환 되는 `boolean` 값 인수를 유효한 전자 메일 주소 인지 여부를 나타내는입니다. 에 지정 된 정규식 패턴의 첫 번째 값 인수를 검색 하 여 이렇게는 `Regex` 생성자입니다. 값을 검사 하 여 입력된 문자열에서 정규식 패턴을 찾을 수 있는지 여부를 확인할 수 있습니다 합니다 `Match` 개체의 `Success` 속성입니다.

> [!NOTE]
> 경우에 따라 속성 유효성 검사에는 종속 속성 포함 될 수 있습니다. 종속 속성의 예로 속성에 대 한 유효한 값 집합이 2. 속성에 설정 된 특정 값에 따라 다른 경우 2. 속성의 값을 검색 관련는 속성의 값이 허용 되는 값 중 하나를 확인 하려면 또한 B 속성의 값이 변경 되는 속성 유효성 재검사 해야 합니다.

## <a name="adding-validation-rules-to-a-property"></a>속성에 유효성 검사 규칙 추가

EShopOnContainers 모바일 앱에서 유효성 검사를 요구 하는 보기 모델 속성 형식으로 선언 된 `ValidatableObject<T>`여기서 `T` 유효성을 검사할 데이터의 형식입니다. 다음 코드 예제에서는 이러한 두 속성의 예를 보여 줍니다.

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

유효성 검사를 실행, 유효성 검사 규칙에 추가 해야 합니다 `Validations` 각 컬렉션 `ValidatableObject<T>` 다음 코드 예제에 설명 된 대로 인스턴스:

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

이 메서드를 추가 합니다 `IsNotNullOrEmptyRule<T>` 유효성 검사 규칙을를 `Validations` 각 컬렉션 `ValidatableObject<T>` 유효성 검사 규칙의 값을 지정 하는 인스턴스를 `ValidationMessage` 경우 표시 되는 유효성 검사 오류 메시지를 지정 하는 속성 유효성 검사에 실패 합니다.

## <a name="triggering-validation"></a>유효성 검사를 트리거하지

EShopOnContainers 모바일 앱에서 사용 된 유효성 검사 방법은 수동으로 트리거할 수 속성의 유효성 검사 및 자동으로 트리거 유효성 검사 속성을 변경 하는 경우.

### <a name="triggering-validation-manually"></a>유효성 검사를 수동으로 트리거

보기 모델 속성에 대 한 유효성 검사를 수동으로 트리거할 수 있습니다. 예를 들어,이 경우 eShopOnContainers 모바일 앱에서 사용자 탭의 **로그인** 단추를 `LoginView`모의 서비스를 사용 하는 경우. 명령 대리자 호출을 `MockSignInAsync` 에서 메서드를 `LoginViewModel`를 실행 하 여 유효성 검사를 호출 하는 `Validate` 메서드를 다음 코드 예제에 나와 있는:

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

합니다 `Validate` 사용자 이름 및 암호에 사용자가 입력의 유효성 검사를 수행 하는 메서드를 `LoginView`, 각 유효성 검사 메서드를 호출 하 여 `ValidatableObject<T>` 인스턴스. 다음 코드 예제에서 유효성 검사 메서드를 보여 줍니다.는 `ValidatableObject<T>` 클래스:

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

이 메서드는 `Errors` 컬렉션 및 검색 하는 개체의 추가 된 모든 유효성 검사 규칙 `Validations` 컬렉션입니다. `Check` 각 검색된 유효성 검사 규칙에 대 한 메서드를 실행 하 및 `ValidationMessage` 데이터 유효성 검사에 실패 하는 모든 유효성 검사 규칙에 대 한 속성 값을 추가 합니다 `Errors` 컬렉션을 `ValidatableObject<T>` 인스턴스. 마지막으로 `IsValid` 속성을 설정 하 고 해당 값이 유효성 검사의 성공 여부를 나타내는 호출 메서드로 반환 합니다.

### <a name="triggering-validation-when-properties-change"></a>속성이 변경 될 때 유효성 검사를 트리거하지

바인딩된 속성이 변경 될 때마다 유효성 검사를 트리거할 수도 있습니다. 등의 경우 양방향 바인딩 합니다 `LoginView` 설정 합니다 `UserName` 또는 `Password` 속성, 유효성 검사가 트리거됩니다. 다음 코드 예제에서는이 발생 하는 방법을 보여 줍니다.

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

[ `Entry` ](xref:Xamarin.Forms.Entry) 컨트롤에 바인딩되는 `UserName.Value` 속성을 `ValidatableObject<T>` 인스턴스 및 컨트롤의 `Behaviors` 컬렉션에는 `EventToCommandBehavior` 인스턴스를 추가 합니다. 이 동작은 실행를 `ValidateUserNameCommand` 대 한 응답으로 [`TextChanged`] 이벤트에서 발생 합니다 `Entry`는 발생할 때의 텍스트를는 `Entry` 변경. 차례로 `ValidateUserNameCommand` 대리자를 실행 합니다 `ValidateUserName` 메서드를 실행 하는 `Validate` 메서드를는 `ValidatableObject<T>` 인스턴스. 따라서 될 때마다 사용자가 입력의 문자는 `Entry` 사용자 이름, 입력된 데이터의 유효성 검사에 대 한 제어 수행 됩니다.

동작에 대 한 자세한 내용은 참조 하세요. [동작 구현](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors)합니다.

<a name="displaying_validation_errors" />

## <a name="displaying-validation-errors"></a>유효성 검사 오류를 표시합니다.

EShopOnContainers 모바일 앱 사용자에 게 모든 유효성 검사 오류는 빨간색 선이 사용 하 여 잘못 된 데이터를 포함 하는 컨트롤을 강조 표시 하 여 알리고 이유를 알리는 오류 메시지를 표시 하 여 데이터를 올바르지 포함 하는 컨트롤 아래는 데이터가 잘못 되었습니다. 잘못 된 데이터를 수정 하는 줄을 검정으로 변경 하 고 오류 메시지가 제거 됩니다. 그림 6-2 유효성 검사 오류가 있을 때 eShopOnContainers 모바일 앱에서 LoginView를 보여 줍니다.

![](validation-images/validation-login.png "로그인 하는 동안 유효성 검사 오류를 표시합니다.")

**그림 6-2:** 로그인 하는 동안 유효성 검사 오류를 표시합니다.

### <a name="highlighting-a-control-that-contains-invalid-data"></a>잘못 된 데이터를 포함 하는 컨트롤을 강조 표시

합니다 `LineColorBehavior` 연결 된 동작을 강조 표시 되 [ `Entry` ](xref:Xamarin.Forms.Entry) 컨트롤 유효성 검사 오류가 발생 했습니다. 다음 코드 예제에서는 하는 방법을 `LineColorBehavior` 연결 된 동작에 연결 되는 `Entry` 제어:

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

합니다 [ `Entry` ](xref:Xamarin.Forms.Entry) 컨트롤에는 다음 코드 예제에 나와 있는 명시적 스타일을 사용 합니다.

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

이 스타일 설정를 `ApplyLineColor` 및 `LineColor` 의 연결 된 속성을 `LineColorBehavior` 동작에 연결 합니다 [ `Entry` ](xref:Xamarin.Forms.Entry) 컨트롤입니다. 스타일에 대한 자세한 내용은 [스타일](~/xamarin-forms/user-interface/styles/index.md)을 참조하세요.

때 값을 `ApplyLineColor` 연결 된 속성은 집합 또는 변경 합니다 `LineColorBehavior` 연결 된 동작을 실행는 `OnApplyLineColorChanged` 메서드를 다음 코드 예제에 나와 있는:

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

동작에 연결 된 컨트롤의 인스턴스 및 이전 및 새 값을 제공 하는이 메서드의 매개 변수는 `ApplyLineColor` 연결 된 속성입니다. `EntryLineColorEffect` 클래스가 컨트롤에 추가 됩니다 [ `Effects` ](xref:Xamarin.Forms.Element.Effects) 컬렉션 경우 합니다 `ApplyLineColor` 연결 된 속성은 `true`, 컨트롤에서 제거 됩니다 그렇지 않으면 `Effects` 컬렉션. 동작에 대 한 자세한 내용은 참조 하세요. [동작 구현](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors)합니다.

합니다 `EntryLineColorEffect` 서브 클래스를 [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) 클래스 하 고 다음 코드 예제에 표시 됩니다.

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

합니다 [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) 클래스 내부 효과 플랫폼별으로를 래핑하는 플랫폼 독립적인 효과 나타냅니다. 이는 플랫폼별 효과에 대한 형식 정보에 컴파일 시간 액세스가 없으므로 효과 제거 프로세스를 간소화합니다. `EntryLineColorEffect` 확인 그룹 이름 및 각 플랫폼별 결과 클래스에 지정 된 고유 ID를 연결으로 구성 된 매개 변수를 전달 하는 기본 클래스 생성자를 호출 합니다.

다음 코드 예제는 `eShopOnContainers.EntryLineColorEffect` iOS에 대 한 구현 합니다.

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

합니다 `OnAttached` Xamarin.Forms에 대 한 네이티브 컨트롤을 검색 하는 메서드 [ `Entry` ](xref:Xamarin.Forms.Entry) 컨트롤을 호출 하 여 선 색을 업데이트 합니다 `UpdateLineColor` 메서드. `OnElementPropertyChanged` 에 바인딩 가능한 속성 변경에 응답 하는 재정의 `Entry` 제어 하는 경우 선 색을 업데이트 하 여 연결 된 `LineColor` 속성이 변경 또는 [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) 속성을 `Entry`변경 합니다. 효과에 대한 자세한 내용은 [효과](~/xamarin-forms/app-fundamentals/effects/index.md)를 참조하세요.

에 유효한 데이터를 입력 하는 경우는 [ `Entry` ](xref:Xamarin.Forms.Entry) 컨트롤 유효성 검사 오류가 없는지를 나타내기 위해 컨트롤의 아래쪽에 검은색 줄 적용 됩니다. 그림 6-3의 예를 보여 줍니다.

![](validation-images/validation-blackline.png "유효성 검사 오류가 없는지를 나타내는 검은 선")

**그림 6-3**: 유효성 검사 오류가 없는지를 나타내는 검은 선

[ `Entry` ](xref:Xamarin.Forms.Entry) 컨트롤을 [ `DataTrigger` ](xref:Xamarin.Forms.DataTrigger) 추가할 해당 [ `Triggers` ](xref:Xamarin.Forms.VisualElement.Triggers) 컬렉션입니다. 다음 코드 예제는 `DataTrigger`:

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

이 [ `DataTrigger` ](xref:Xamarin.Forms.DataTrigger) 모니터를 `UserName.IsValid` 속성을 값인 경우 되며 `false`를 실행 합니다 [ `Setter` ](xref:Xamarin.Forms.Setter), 변경 내용을 `LineColor` 연결 속성의는 `LineColorBehavior` 빨간색 동작을 연결 합니다. 그림 6-4의 예를 보여 줍니다.

![](validation-images/validation-redline.png "유효성 검사 오류를 나타내는 빨간색 선")

**그림 6-4**: 유효성 검사 오류를 나타내는 빨간색 선

줄을 [ `Entry` ](xref:Xamarin.Forms.Entry) 입력 한 데이터가 잘못 되었습니다. 하는 동안 컨트롤을 빨간색 남아, 입력 한 데이터가 유효한 지를 나타내기 위해 검정색으로 변경 됩니다 그렇지 않은 경우.

트리거에 대 한 자세한 내용은 참조 하세요. [트리거](~/xamarin-forms/app-fundamentals/triggers.md)합니다.

### <a name="displaying-error-messages"></a>오류 메시지를 표시합니다.

UI는 데이터 유효성 검사에 실패 하는 각 컨트롤 아래 레이블 컨트롤에 유효성 검사 오류 메시지를 표시 합니다. 다음 코드 예제는 [ `Label` ](xref:Xamarin.Forms.Label) 표시 하는 유효성 검사 오류 메시지를 사용자가 올바른 사용자 이름을 입력 하지 하는 경우:

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

각 [ `Label` ](xref:Xamarin.Forms.Label) 바인딩되는 `Errors` 유효성 검사 중인 뷰 모델 개체의 속성입니다. 합니다 `Errors` 에서 제공 하는 속성을 `ValidatableObject<T>` 클래스 및 형식입니다 `List<string>`합니다. 때문에 합니다 `Errors` 속성에는 여러 유효성 검사 오류가 포함 될 수 있습니다는 `FirstValidationErrorConverter` 인스턴스 표시에 대 한 컬렉션에서 첫 번째 오류를 검색 하는 합니다.

## <a name="summary"></a>요약

EShopOnContainers 모바일 앱 보기 모델 속성의 동기 클라이언트 쪽 유효성 검사를 수행 하 고 사용자를 제공 하는 오류 메시지를 표시 하 고 잘못 된 데이터를 포함 하는 컨트롤을 강조 표시 하 여 유효성 검사 오류가 발생 하는 사용자에 게 알립니다. 이유는 데이터가 올바르지 않습니다.

유효성 검사를 요구 하는 보기 모델 속성은 `ValidatableObject<T>`, 및 각 `ValidatableObject<T>` 인스턴스를 추가 하는 유효성 검사 규칙에 해당 `Validations` 속성입니다. 유효성 검사를 호출 하 여 뷰 모델에서 호출를 `Validate` 메서드의 `ValidatableObject<T>` 인스턴스 유효성 검사를 검색 하는 규칙 및에 대해 실행 합니다 `ValidatableObject<T>` `Value` 속성입니다. 유효성 검사 오류에 배치 됩니다는 `Errors` 의 속성을 `ValidatableObject<T>`인스턴스를 및 `IsValid` 의 속성은 `ValidatableObject<T>` 인스턴스 유효성 검사의 성공 여부를 나타내도록 업데이트 됩니다.


## <a name="related-links"></a>관련 링크

- [2mb PDF 전자책 다운로드](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
