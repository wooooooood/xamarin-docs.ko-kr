---
title: C#6 새 기능 개요
description: C# 언어 버전 6은 계속 해 서 상용구 코드를 줄이고, 명확성을 개선 하 고, 일관성을 유지할 수 있도록 언어를 계속 발전 시킵니다. 클리너 초기화 구문, 사용 하는 기능을 catch/finally 블록에서 사용 하는 기능 및 null 조건부? 연산자는 특히 유용 합니다.
ms.prod: xamarin
ms.assetid: 4B4E41A8-68BA-4E2B-9539-881AC19971B
ms.custom: xamu-video
author: conceptdev
ms.author: crdun
ms.date: 03/22/2017
ms.openlocfilehash: 2a62ff12869f886e63fc5c78050e9870431e58bb
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70280615"
---
# <a name="c-6-new-features-overview"></a>C#6 새 기능 개요

_C# 언어 버전 6은 계속 해 서 상용구 코드를 줄이고, 명확성을 개선 하 고, 일관성을 유지할 수 있도록 언어를 계속 발전 시킵니다. 클리너 초기화 구문, 사용 하는 기능을 catch/finally 블록에서 사용 하는 기능 및 null 조건부? 연산자는 특히 유용 합니다._

> [!NOTE]
> 최신 버전의 C# 언어에 대 한 자세한 내용은 버전 7- [7.0 C# 의 새로운 기능](/dotnet/csharp/whats-new/csharp-7) 문서를 참조 하세요.

이 문서에서는 6의 C# 새로운 기능을 소개 합니다. Mono 컴파일러에서 완벽 하 게 지원 되며 개발자는 모든 Xamarin 대상 플랫폼에서 새로운 기능을 사용 하 여 시작할 수 있습니다.

> [!VIDEO https://youtube.com/embed/7UdV7zGPfMU]

**6 비디오의 C# 새로운 기능**

## <a name="using-c-6"></a>6 C# 사용

C# 6 컴파일러는 모든 최신 버전의 Mac용 Visual Studio에 사용 됩니다.
명령줄 컴파일러를 사용 하는 경우에서 `mcs --version` 4.0 이상을 반환 하는지 확인 해야 합니다.
Mac용 Visual Studio 사용자는 **Mac용 Visual Studio > > Mac용 Visual Studio 정보**를 참조 하 여 Mono 4 이상이 설치 되어 있는지 확인할 수 있습니다.

## <a name="less-boilerplate"></a>상용구 낮음
### <a name="using-static"></a>using static
열거형 및와 `System.Math`같은 특정 클래스는 주로 정적 값과 함수의 소유자입니다. 6 C# 에서는 단일 `using static` 문을 사용 하 여 형식의 정적 멤버를 모두 가져올 수 있습니다. 5와 C# C# 6에서 일반적인 삼각 함수를 비교 합니다.

```csharp
// Classic C#
class MyClass
{
    public static Tuple<double,double> SolarAngleOld(double latitude, double declination, double hourAngle)
    {
        var tmp = Math.Sin (latitude) * Math.Sin (declination) + Math.Cos (latitude) * Math.Cos (declination) * Math.Cos (hourAngle);
        return Tuple.Create (Math.Asin (tmp), Math.Acos (tmp));
    }
}

// C# 6
using static System.Math;

class MyClass
{
    public static Tuple<double, double> SolarAngleNew(double latitude, double declination, double hourAngle)
    {
        var tmp = Asin (latitude) * Sin (declination) + Cos (latitude) * Cos (declination) * Cos (hourAngle);
        return Tuple.Create (Asin (tmp), Acos (tmp));
    }
}
```

`using static`는 `const` `Math.PI` 및 와`Math.E`같은 공용 필드를 직접 액세스할 수 없도록 합니다.

```csharp
for (var angle = 0.0; angle <= Math.PI * 2.0; angle += Math.PI / 8) ... 
//PI is const, not static, so requires Math.PI
```

### <a name="using-static-with-extension-methods"></a>확장 메서드와 함께 static 사용

`using static` 기능은 확장 메서드와 약간 다르게 작동 합니다. 확장 메서드는를 사용 하 `static`여 작성 되지만 작업 하는 인스턴스 없이는 의미가 없습니다. 따라서가 `using static` 확장 메서드를 정의 하는 형식과 함께 사용 되는 경우 해당 대상 형식 ( `this` 메서드 형식)에서 확장 메서드를 사용할 수 있게 됩니다. 예를 들어 `using static System.Linq.Enumerable` ,를 사용 하 여 모든 LINQ 형식을 `IEnumerable<T>` 가져오지 않고도 개체의 API를 확장할 수 있습니다.

```csharp
using static System.Linq.Enumerable;
using static System.String;

class Program
{
    static void Main()
    {
        var values = new int[] { 1, 2, 3, 4 };
        var evenValues = values.Where (i => i % 2 == 0);
        System.Console.WriteLine (Join(",", evenValues));
    }
}
```

이전 예제에서는 동작의 차이점을 보여 줍니다. 확장 메서드 `Enumerable.Where` 는 배열에 연결 되 고, 정적 메서드 `String.Join` 는 `String` 형식에 대 한 참조 없이 호출 될 수 있습니다.

### <a name="nameof-expressions"></a>식의 name
경우에 따라 변수나 필드가 지정 된 이름을 참조 하는 것이 좋습니다. 6 C# `nameof(someVariableOrFieldOrType)` 에서는 문자열 `"someVariableOrFieldOrType"`을 반환 합니다. 예를 들어을 throw `ArgumentException` 할 때 잘못 된 인수를 이름으로 지정할 가능성이 매우 높습니다.

```csharp
throw new ArgumentException ("Problem with " + nameof(myInvalidArgument))
```

식의 `nameof` 가장 큰 장점은 형식이 선택 되 고 도구 지원 리팩터링과 호환 된다는 것입니다. 식의 `nameof` 형식 검사는를 `string` 사용 하 여 형식을 동적으로 연결 하는 경우에 특히 시작 합니다. 예를 들어 iOS `string` 에서는 개체 `UITableView`를 프로토타입 `UITableViewCell` 하는 데 사용 되는 형식을 지정 하는 데 사용 됩니다. `nameof`철자가 잘못 되었거나 찾아낼 인 리팩터링으로 인해이 연결이 실패할 수 있도록 보장 합니다.

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (nameof(CellTypeA), indexPath);
    cell.TextLabel.Text = objects [indexPath.Row].ToString ();
    return cell;
}
```

정규화 된 이름을에 `nameof`전달할 수 있지만 마지막 `.`요소만 반환 됩니다. 예를 들어, Xamarin. Forms에서 데이터 바인딩을 추가할 수 있습니다.

```csharp
var myReactiveInstance = new ReactiveType ();
var myLabelOld.BindingContext = myReactiveInstance;
var myLabelNew.BindingContext = myReactiveInstance;
var myLabelOld.SetBinding (Label.TextProperty, "StringField");
var myLabelNew.SetBinding (Label.TextProperty, nameof(ReactiveType.StringField));
```

를 `SetBinding` 두 번 호출 하면 동일한 `nameof(ReactiveType.StringField)` 값이 전달 됩니다 `"StringField"`.는 `"ReactiveType.StringField"` 이며 처음에는 예상치 못할 수 있습니다.

## <a name="null-conditional-operator"></a>Null 조건 연산자
이전에를 C# 업데이트 하면 nullable 형식의 개념과 null 병합 연산자 `??` 를 도입 하 여 nullable 값을 처리할 때 상용구 코드의 양을 줄일 수 있습니다. C#6 "null 조건 연산자" `?.`를 사용 하 여이 테마를 계속 합니다. 식의 오른쪽에 있는 개체에 사용 되는 경우 null 조건 연산자는 개체가이 아닌 `null` 경우 멤버 값을 반환 하 고 `null` 그렇지 않으면입니다.

```csharp
var ss = new string[] { "Foo", null };
var length0 = ss [0]?.Length; // 3
var length1 = ss [1]?.Length; // null
var lengths = ss.Select (s => s?.Length ?? 0); //[3, 0]
```

및 `length0` 모두`length1` 형식 으로`int?`유추 됩니다.

이전 예의 마지막 줄에서는 null-조건부 `?` 연산자를 `??` null 병합 연산자와 함께 보여 줍니다. 새 C# 6 null 조건 연산자는 배열의 두 `null` 번째 요소에서 반환 됩니다. 여기서 null 병합 연산자가 시작 하 고 `lengths` 배열에 0을 제공 합니다 (해당 하는 경우에 해당 하는지 여부에 관계 없이). 문제 관련).

Null 조건 연산자는 여러 응용 프로그램에서 필요한 상용구 null 검사의 양을 크게 줄여야 합니다.

모호성으로 인 한 null 조건 연산자에는 몇 가지 제한 사항이 있습니다. `?` 대리자를 사용 하 여 수행 하는 것이 좋습니다.

```csharp
SomeDelegate?("Some Argument") // Not allowed
```

그러나를 `Invoke` 사용 하 여 인수 목록 `?` 에서를 구분할 수 있으며, 계속 해 서 상용구의 검사 블록 `null`에 대해 표시 된 향상 된 기능을 사용할 수 있습니다.

```csharp
public event EventHandler HandoffOccurred;
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    HandoffOccurred?.Invoke (this, userActivity.UserInfo);
    return true;
}
```

## <a name="string-interpolation"></a>문자열 보간
함수 `String.Format` 는 일반적으로 형식 문자열에서 자리 표시자로 인덱스를 사용 합니다 (예 `String.Format("Expected: {0} Received: {1}.", expected, received`:,). 물론, 새 값을 추가 하는 작업에는 항상 인수를 계산 하 고, 자리 표시자 번호를 매기고, 인수 목록에서 오른쪽 시퀀스에 새 인수를 삽입 하는 등의 작업이 거의 수반 되지 않습니다.

C#6의 새 문자열 보간 기능은에 `String.Format`대해 크게 향상 되었습니다. 이제 접두사가 인 `$`문자열에서 직접 변수 이름을 지정할 수 있습니다. 말합니다.

```csharp
$"Expected: {expected} Received: {received}."
```

예를 들어, 변수를 선택 하 고 철자가 틀렸거나 사용 하지 않는 변수를 사용 하면 컴파일러 오류가 발생 합니다.

자리 표시자는 단순한 변수가 될 필요는 없으며 어떤 식일 수도 있습니다. 이러한 따옴표 안에 따옴표를 이스케이프 *하지 않고* 따옴표를 사용할 수 있습니다. 예를 들어, 다음 `"s"` 에 유의 하십시오.

```csharp
var s = $"Timestamp: {DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}"
```

문자열 보간은의 `String.Format`맞춤 및 서식 지정 구문을 지원 합니다. 이전 `{index, alignment:format}`에 작성 한 것 처럼 C# 6 개는 다음 `{placeholder, alignment:format}`을 작성 합니다.

```csharp
using static System.Linq.Enumerable;
using System;

class Program
{
    static void Main ()
    {
        var values = new int[] { 1, 2, 3, 4, 12, 123456 };
        foreach (var s in values.Select (i => $"The value is { i,10:N2}.")) {
            Console.WriteLine (s);
        }
    Console.WriteLine ($"Minimum is { values.Min(i => i):N2}.");
    }
}
```

결과:

```
The value is       1.00.
The value is       2.00.
The value is       3.00.
The value is       4.00.
The value is      12.00.
The value is 123,456.00.
Minimum is 1.00.
```

문자열 보간은의 `String.Format`구문 sugar입니다 .이는 문자열 리터럴과 함께 `@""` 사용할 수 없으며, 사용 되는 `const`자리 표시 자가 없더라도와 호환 되지 않습니다.

```csharp
const string s = $"Foo"; //Error : const requires value
```

문자열 보간을 사용 하는 함수 인수를 빌드하는 일반적인 사용 사례에서는 이스케이프, 인코딩 및 문화권 문제에 주의 해야 합니다. 물론 SQL 및 URL 쿼리를 삭제 하는 것은 중요 합니다. 와 `String.Format`마찬가지로 문자열 보간은를 `CultureInfo.CurrentCulture`사용 합니다. 를 `CultureInfo.InvariantCulture` 사용 하는 것이 약간 더 좋습니다.

```csharp
Thread.CurrentThread.CurrentCulture  = new CultureInfo ("de");
Console.WriteLine ($"Today is: {DateTime.Now}"); //"21.05.2015 13:52:51"
Console.WriteLine ($"Today is: {DateTime.Now.ToString(CultureInfo.InvariantCulture)}"); //"05/21/2015 13:52:51"
```

## <a name="initialization"></a>초기화

C#6은 속성, 필드 및 멤버를 지정 하는 다양 한 방법을 제공 합니다.

### <a name="auto-property-initialization"></a>Auto 속성 초기화

이제 필드와 동일한 간결한 방식으로 Auto 속성을 초기화할 수 있습니다. 변경할 수 없는 자동 속성은 getter만 쓸 수 있습니다.

```csharp
class ToDo
{
    public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
    public DateTime Created { get; } = DateTime.Now;
```

생성자에서 getter 전용 auto 속성의 값을 설정할 수 있습니다.

```csharp
class ToDo
{
    public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
    public DateTime Created { get; } = DateTime.Now;
    public string Description { get; }

    public ToDo (string description)
    {
        this.Description = description; //Can assign (only in constructor!)
    }
```

이러한 자동 속성 초기화는 일반적인 공간 절약 기능이 며, 개발자에 게 개체의 불변성을 강조 하는 데 도움이 됩니다.

### <a name="index-initializers"></a>인덱스 이니셜라이저

C#6은 인덱서를 포함 하는 형식에서 키와 값을 모두 설정할 수 있게 해 주는 인덱스 이니셜라이저를 소개 합니다. 일반적으로 다음과 같은 스타일 `Dictionary`의 데이터 구조를 사용할 수 있습니다.

```csharp
partial void ActivateHandoffClicked (WatchKit.WKInterfaceButton sender)
{
    var userInfo = new NSMutableDictionary {
        ["Created"] = NSDate.Now,
        ["Due"] = NSDate.Now.AddSeconds(60 * 60 * 24),
        ["Task"] = Description
    };
    UpdateUserActivity ("com.xamarin.ToDo.edit", userInfo, null);
    statusLabel.SetText ("Check phone");
}
```

### <a name="expression-bodied-function-members"></a>식 본문 함수 멤버

람다 함수에는 몇 가지 장점이 있으며, 그 중 하나는 단지 공간을 절약 하는 것입니다. 마찬가지로 식 본문 클래스 멤버는 작은 함수를 이전 버전 C# 6에서 사용할 수 있었던 것 보다 약간 더 간략하게 표현할 수 있도록 합니다.

식 본문 함수 멤버는 일반적인 블록 구문이 아닌 람다 화살표 구문을 사용 합니다.

```csharp
public override string ToString () => $"{FirstName} {LastName}";
```

람다 화살표 구문은 explicit `return`를 사용 하지 않습니다. 을 반환 `void`하는 함수의 경우 식도 문 이어야 합니다.

```csharp
public void Log(string message) => System.Console.WriteLine($"{DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}: {message}");
```

식 본문 멤버는 여전히 메서드에 대해 지원 되지만 속성에는 `async` 적용 되지 않는 규칙에 적용 됩니다.

```csharp
//A method, so async is valid
public async Task DelayInSeconds(int seconds) => await Task.Delay(seconds * 1000);
//The following property will not compile
public async Task<int> LeisureHours => await Task.FromResult<char> (DateTime.Now.DayOfWeek.ToString().First()) == 'S' ? 16 : 5;
```

## <a name="exceptions"></a>예외

이에 대 한 두 가지 방법이 없습니다. 예외 처리는 오른쪽에 표시 하기 어렵습니다. 6의 C# 새로운 기능을 사용 하면 보다 유연 하 고 일관 된 예외 처리가 가능 합니다.

### <a name="exception-filters"></a>예외 필터

정의에 따라 예외는 비정상적인 상황에서 발생 하며, 특정 형식의 예외를 발생 시킬 수 있는 *모든* 방법에 대 한 이유 및 코드를 이해 하기 어려울 수 있습니다. C#6은 런타임 평가 필터를 사용 하 여 실행 처리기를 보호 하는 기능을 제공 합니다. 이 작업은 일반 `when (bool)` `catch(ExceptionType)` 선언 뒤에 패턴을 추가 하 여 수행 됩니다. 다음에서 필터는 다른 구문 분석 오류와는 달리 `date` 매개 변수와 관련 된 구문 분석 오류를 구분 합니다.

```csharp
public void ExceptionFilters(string aFloat, string date, string anInt)
{
    try
    {
        var f = Double.Parse(aFloat);
        var d = DateTime.Parse(date);
        var n = Int32.Parse(anInt);
    } catch (FormatException e) when (e.Message.IndexOf("DateTime") > -1) {
        Console.WriteLine ($"Problem parsing \"{nameof(date)}\" argument");
    } catch (FormatException x) {
        Console.WriteLine ("Problem parsing some other argument");
    }
}
```

### <a name="await-in-catchfinally"></a>catch에서 대기 합니다. 마지막으로 ...

5에서 C# 도입 된 기능은해당언어에대한게임교환기입니다.`async` 5 C# `catch` `async/await` 에서 및 블록`finally` 에는를 사용할 수 없습니다. 이기능값을지정하면성가신`await` 가 발생 합니다. C#6 다음 코드 조각과 같이 프로그램을 통해 일관성 있게 비동기 결과를 대기 수 있도록 이러한 제한을 제거 합니다.

```csharp
async void SomeMethod()
{
    try {
        //...etc...
    } catch (Exception x) {
        var diagnosticData = await GenerateDiagnosticsAsync (x);
        Logger.log (diagnosticData);
    } finally {
        await someObject.FinalizeAsync ();
    }
}
```

## <a name="summary"></a>요약

이 C# 언어는 개발자가 생산성을 높일 뿐만 아니라 바람직한 방법과 지원 도구를 홍보 하기 위해 계속 진화 하 고 있습니다. 이 문서는 6의 C# 새로운 언어 기능에 대 한 개요를 제공 하 고 사용 방법을 간략하게 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [6의 C# 새로운 언어 기능](https://github.com/dotnet/roslyn/wiki/New-Language-Features-in-C%23-6)

