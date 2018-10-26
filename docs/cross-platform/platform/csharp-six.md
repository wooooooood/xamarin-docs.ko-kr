---
title: C#6 새 기능 개요
description: 최신 버전의를 C# – 버전 6 – 언어 상용구를 덜, 향상 된 이해를 돕기 위해 자세한 일관성 있고 언어 진화 계속 합니다. 클리너 초기화 구문을 사용 하는 기능 catch/finally 블록 및 null 조건부 await? 연산자는 특히 유용 합니다.
ms.prod: xamarin
ms.assetid: 4B4E41A8-68BA-4E2B-9539-881AC19971B
ms.custom: xamu-video
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 16ee512395b2658b26bc7a489eabecec3656fa93
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115759"
---
# <a name="c-6-new-features-overview"></a>C#6 새 기능 개요

_최신 버전의를 C# – 버전 6 – 언어 상용구를 덜, 향상 된 이해를 돕기 위해 자세한 일관성 있고 언어 진화 계속 합니다. 클리너 초기화 구문을 사용 하는 기능 catch/finally 블록 및 null 조건부 await? 연산자는 특히 유용 합니다._

이 문서에서는의 새로운 기능을 소개 C# 6. Mono 컴파일러에서 완전히 지원 됩니다 및 새로운 기능을 사용 하 여 모든 Xamarin 대상 플랫폼에서 개발자가 시작할 수 있습니다.

이 문서에는의 간략 한 조각이 포함 되어 있습니다.는 C# 기본적인 사용을 보여 주는 6 코드입니다.
샘플 응용 프로그램에는 모든 Xamarin 대상 플랫폼에서 실행 되 고 다양 한 기능을 연습 하는 명령줄 프로그램입니다.


> [!VIDEO https://youtube.com/embed/7UdV7zGPfMU]

**새로운 기능 C# 6으로 [Xamarin University](https://university.xamarin.com/)**


## <a name="development-environment"></a>개발 환경

### <a name="mac"></a>Mac

* **Mac 용 visual Studio** 지원 C# 6: 빌드하고 사용 하 여 Xamarin 앱을 컴파일할 수 있습니다 C# 6 기능입니다.
  에 대해 자세히 알아보세요 [Mac 용 Visual Studio](https://docs.microsoft.com/visualstudio/mac/)합니다.

### <a name="windows"></a>Windows

* **Visual Studio 2015 및 2017** 에 대 한 전체 지원 이상 있고 C# 6입니다. 이전 버전의 Visual Studio 지원 하지 것입니다 C# 6.

* **Windows 용 Xamarin Studio** 없습니다 C# 편집기에서 6 기능입니다.



## <a name="compiler"></a>컴파일러

Mono C# 6 컴파일러는 Mono 4.0 이상에 포함 된 [무료로 다운로드할 수 있습니다](http://www.mono-project.com/download/)합니다.
Mac 용 visual Studio는 자동으로 시스템에서 Mono 설치를 업데이트합니다.

Windows 사용자에 게 있어야 [Visual Studio 2015 또는 2017 ^](https://visualstudio.microsoft.com/) 컴파일하는 데 설치 C# 6 코드 (선택한 경우에 Windows 용 Xamarin Studio IDE로).

^ 또는 *[Microsoft Build Tools 2015](http://www.microsoft.com/download/details.aspx?id=48159)* 명령에 대 한 줄 컴파일 또는 빌드 서버, 예를 들어 있습니다.

## <a name="using-c-6"></a>사용 하 여 C# 6

C# 6 컴파일러는 모든 최신 버전의 Visual Studio for mac
명령줄 컴파일러를 사용 하 여 확인 해야 `mcs --version` 4.0 이상 반환 합니다.
Mac 사용자 용 visual Studio에서 참조 하 여 Mono 4 이상이 있는 경우 설치를 확인할 수 있습니다 **Mac 용 Visual Studio에 대 한 > Mac 용 Visual Studio > 자세한 정보 표시**합니다.

## <a name="less-boilerplate"></a>상용구를 덜
### <a name="using-static"></a>using static
열거형과 같은 특정 클래스 `System.Math`, 정적 값 및 함수의 소유자 주로 됩니다. C# 6, 단일을 사용 하 여 형식의 모든 정적 멤버를 가져올 수 있습니다 `using static` 문입니다. 일반적인 삼각 함수를 비교 C# 5 및 C# 6:

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

`using static` 공용 해도 `const` 같은 필드 `Math.PI` 고 `Math.E`직접 액세스할 수 있는:

```csharp
for (var angle = 0.0; angle <= Math.PI * 2.0; angle += Math.PI / 8) ... 
//PI is const, not static, so requires Math.PI
```

### <a name="using-static-with-extension-methods"></a>정적 확장 메서드를 사용 하 여 사용 하 여

`using static` 시설 약간 다르게 작동 확장 메서드를 사용 하 여 합니다. 확장 메서드를 사용 하 여 작성 됩니다 `static`, 쉽게 작동 하는 데 기반이 인스턴스 없이도 이해 하지 않습니다. 경우에 따라서 `using static` 확장 메서드를 해당 대상 형식에서 사용할 수 있게 확장 메서드를 정의 하는 형식과 함께 사용 됩니다 (메서드의 `this` 형식). 예를 들어 `using static System.Linq.Enumerable` 의 API 확장을 사용할 수 있습니다 `IEnumerable<T>` 모든 LINQ 형식에 표시 하지 않고 개체:

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

동작의 차이점을 설명 하는 이전 예제: 확장 메서드 `Enumerable.Where` 정적 메서드에 배열과 사용 하 여 연결 `String.Join` 에 대 한 참조 없이 호출할 수는 `String` 형식입니다.

### <a name="nameof-expressions"></a>nameof 식
참조 하려는 경우에 따라 이름에는 변수 또는 필드를 지정 했습니다. C# 6 `nameof(someVariableOrFieldOrType)` 문자열을 반환 합니다 `"someVariableOrFieldOrType"`합니다. 예를 들어 throw 할 때를 `ArgumentException` 가능성이 매우는 인수가 잘못 되었습니다. 이름을 지정 하려고 합니다.

```csharp
throw new ArgumentException ("Problem with " + nameof(myInvalidArgument))
```

주된 장점은 `nameof` 식 되는 형식을 확인 하 고 있다는 리팩터링 도구 기반과 호환 됩니다. 형식 검사의 `nameof` 식을 상황에서 특히 시작 됩니다. 여기서는 `string` 형식을 동적으로 연결할 때 사용 됩니다. 예를 들어 iOS에에서는 `string` 프로토타입에 사용 된 유형을 지정 하는 데 사용 됩니다 `UITableViewCell` 개체는 `UITableView`합니다. `nameof` 이 연결 맞춤법 오류 또는 불량 리팩터링 하지 없도록 보장할 수 있습니다.

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (nameof(CellTypeA), indexPath);
    cell.TextLabel.Text = objects [indexPath.Row].ToString ();
    return cell;
}
```

정규화 된 이름을 전달할 수 있지만 `nameof`, 마지막 요소에만 (마지막 `.`)이 반환 됩니다. 예를 들어 데이터 바인딩을 Xamarin.Forms에 추가할 수 있습니다.

```csharp
var myReactiveInstance = new ReactiveType ();
var myLabelOld.BindingContext = myReactiveInstance;
var myLabelNew.BindingContext = myReactiveInstance;
var myLabelOld.SetBinding (Label.TextProperty, "StringField");
var myLabelNew.SetBinding (Label.TextProperty, nameof(ReactiveType.StringField));
```

두 호출 `SetBinding` 동일한 값을 전달 하는: `nameof(ReactiveType.StringField)` 됩니다 `"StringField"`아니라 `"ReactiveType.StringField"` 처음 예상한 대로입니다.

## <a name="null-conditional-operator"></a>Null 조건부 연산자
이전 업데이트를 C# nullable 형식 및 null 병합 연산자의 개념을 도입 `??` nullable 값을 처리 하는 경우 상용구 코드 양을 줄일 수 있습니다. C#6이이 테마 "null 조건부 연산자"를 사용 하 여 계속 `?.`합니다. 개체가 없는 경우 null 조건부 연산자 멤버 값을 반환 식의 오른쪽에 있는 개체를 사용 하면 `null` 고 `null` 그렇지 않은 경우:

```csharp
var ss = new string[] { "Foo", null };
var length0 = ss [0]?.Length; // 3
var length1 = ss [1]?.Length; // null
var lengths = ss.Select (s => s?.Length ?? 0); //[3, 0]
```

(둘 다 `length0` 하 고 `length1` 형식으로 유추 됩니다 `int?`)

에서는 이전 예제에서 마지막 줄을 `?` null 조건부 연산자와 함께에서 `??` null 병합 연산자입니다. 새 C# 6 null 조건부 연산자 반환 `null` 이때 null 병합 연산자로 시작 되 고 제공 하는 0 일 경우 배열의 두 번째 요소에는 `lengths` (여부는 적합 하지 인 물론 배열 문제에 따라 다름).

Null 조건부 연산자 단시간 상용구 많은 응용 프로그램에 필요한 null 검사의 양을 줄여야 합니다.

Null 조건부 연산자 모호성으로 인해 일부 제한이 있습니다. 즉시를 따를 수 없습니다는 `?` 괄호로 묶인 인수 목록이 있는 때 가장 큰 대리자를 사용 하 여 수행 합니다.

```csharp
SomeDelegate?("Some Argument") // Not allowed
```

그러나 `Invoke` 구분 하는 데 사용할 수는 `?` 인수 목록에서 통해는 여전히 표시 된 향상 된 기능 및를 `null`-상용구 블록 검사:

```csharp
public event EventHandler HandoffOccurred;
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    HandoffOccurred?.Invoke (this, userActivity.UserInfo);
    return true;
}
```

## <a name="string-interpolation"></a>문자열 보간
`String.Format` 함수는 일반적으로 인덱스 자리 표시자로 사용 형식 문자열의 예를 들어 `String.Format("Expected: {0} Received: {1}.", expected, received`). 물론, 새 값 추가 인수를 계산 하 고 자리 표시자를 번호 다시 매기기 인수 목록에서 올바른 시퀀스에서 새 인수 삽입 성가신 거의 작업을 관련 항상 했습니다.

C#6의 새로운 특징 문자열 보간 기능을 크게 향상 `String.Format`합니다. 변수 접두사로 문자열에서 직접 이름을 지정할 수는 이제는 `$`합니다. 예를 들면 다음과 같습니다.

```csharp
$"Expected: {expected} Received: {received}."
```

변수 물론, 체크 인 되 고 철자가 잘못 되었거나, 사용할 수 없는 변수 컴파일러 오류가 발생 합니다.

자리 표시자를 단순 변수를 사용할 필요가 없습니다, 식일 수 있습니다. 이러한 자리 표시자 내의 따옴표를 사용할 수 있습니다 *없이* 해당 따옴표를 이스케이프 합니다. 예를 들어 확인 된 `"s"` 다음에서:

```csharp
var s = $"Timestamp: {DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}"
```

문자열 보간 맞춤 및 서식 지정 구문을 지원 `String.Format`합니다. 이전에 작성 하듯이 `{index, alignment:format}`, C# 작성 하는 6 `{placeholder, alignment:format}`:

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

    The value is       1.00.
    The value is       2.00.
    The value is       3.00.
    The value is       4.00.
    The value is      12.00.
    The value is 123,456.00.
    Minimum is 1.00.

문자열 보간은에 대 한 syntactic sugar `String.Format`: 함께 사용할 수 없습니다 `@""` 문자열 리터럴 및와 호환 되지 않습니다 `const`자리 표시 자가 되는 경우에:

```csharp
const string s = $"Foo"; //Error : const requires value
```

문자열 보간 사용 하 여 함수 인수를 구축 하는 일반적인 사용 사례에서 이스케이프, 인코딩 및 문화권 문제에 대해 주의 해야 합니다. SQL 쿼리와 URL 인 물론, 삭제 하는 데 중요 합니다. 와 마찬가지로 `String.Format`, 문자열 보간을 사용 하는 `CultureInfo.CurrentCulture`합니다. 사용 하 여 `CultureInfo.InvariantCulture` 좀 더 단어는:

```csharp
Thread.CurrentThread.CurrentCulture  = new CultureInfo ("de");
Console.WriteLine ($"Today is: {DateTime.Now}"); //"21.05.2015 13:52:51"
Console.WriteLine ($"Today is: {DateTime.Now.ToString(CultureInfo.InvariantCulture)}"); //"05/21/2015 13:52:51"
```

## <a name="initialization"></a>초기화

C#6에는 다양 한 속성, 필드 및 멤버를 지정 하는 간단한 방법 제공 합니다.

### <a name="auto-property-initialization"></a>Auto 속성 초기화

Auto 속성 필드와 동일한 간결한 방식으로 초기화 될 수 있습니다. 변경할 수 없는 auto 속성 getter만을 사용 하 여 작성할 수 있습니다.

```csharp
class ToDo
{
    public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
    public DateTime Created { get; } = DateTime.Now;
```

생성자에는 getter 전용 auto 속성의 값을 설정할 수 있습니다.

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

Auto 속성이 초기화가 일반적인 공간 절약 기능 및 해당 개체의 불변성을 강조 하기 위해 개발자에 게 유용 하 게 됩니다.

### <a name="index-initializers"></a>인덱스 이니셜라이저

C#6은 인덱서가 있는 형식에 키와 값을 설정할 수 있도록 하는 인덱스 이니셜라이저를 소개 합니다. 에 대 한 일반적으로 이것이 `Dictionary`-데이터 구조를 스타일:

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

람다 함수에는 그 중 하나는 공간 저장 하 여 몇 가지 이점을 있습니다. 클래스 식 본문 멤버를 이전 버전의 보다 좀 더 간결 하 게 표현할 수 있는 작은 함수를 허용 하는 마찬가지로, C# 6.

식 본문 함수 멤버는 기존의 블록 구문이 아니라 람다 화살표 구문을 사용합니다.

```csharp
public override string ToString () => $"{FirstName} {LastName}";
```

람다 화살표 구문을 사용 하지 않음을 명시적 알림 `return`합니다. 반환 하는 함수에 대 한 `void`, 식 문을 수도 있어야 합니다.

```csharp
public void Log(string message) => System.Console.WriteLine($"{DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}: {message}");
```

식 본문 멤버 규칙이 계속 적용 됩니다는 `async` 메서드 하지만 속성이 아니라 지원 됩니다.

```csharp
//A method, so async is valid
public async Task DelayInSeconds(int seconds) => await Task.Delay(seconds * 1000);
//The following property will not compile
public async Task<int> LeisureHours => await Task.FromResult<char> (DateTime.Now.DayOfWeek.ToString().First()) == 'S' ? 16 : 5;
```

## <a name="exceptions"></a>예외

에 대 한 없는 두 가지 방법으로: 예외 처리 이해 하기 어렵습니다. 새로운 기능 C# 6 보다 유연 하 고 일관 된 예외 처리를 확인 합니다.

### <a name="exception-filters"></a>예외 필터

정의상, 특수 한 상황에서 예외가 발생 하 고 이유 및 방법에 대 한 코드는 매우 어려울 수 있습니다 *모든* 특정 유형의 예외가 발생 하는 방법입니다. C#6은 런타임 평가 필터를 사용 하 여 실행 처리기를 보호 하는 기능을 소개 합니다. 추가 하 여 이렇게를 `when (bool)` 법선 후 패턴 `catch(ExceptionType)` 선언 합니다. 다음에서 필터는와 관련 된 구문 분석 오류를 구별 합니다 `date` 다른 구문 분석 오류와 달리 매개 변수입니다.

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

### <a name="await-in-catchfinally"></a>... catch에서 await를 마지막으로 하는 중...

합니다 `async` 에 도입 된 기능 C# 5 언어용 판도 되었습니다. C# 5 `await` 에서 허용 되지 않았습니다 `catch` 및 `finally` 블록의 값을 지정 하는 불편 합니다 `async/await` 기능. C#6에는 비동기 결과 다음 코드 조각에 나와 있는 것 처럼 프로그램을 통해 지속적으로 대기할 수 있도록 이러한 제한을 제거 합니다.

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

C# 언어 진화 또한 모범 사례를 홍보 하 고 도구를 지원 하는 동안 개발자 생산성을 확인 하 여 계속 합니다. 이 문서에 새 언어 기능의 개요를 지정 하는 C# 6 및 사용 방법에 대해 간략하게 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [새로운 언어 기능은 C# 6](https://github.com/dotnet/roslyn/wiki/New-Language-Features-in-C%23-6)

