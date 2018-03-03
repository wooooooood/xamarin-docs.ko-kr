---
title: "C# 6 새 기능 개요"
description: "최신 버전의 C# 언어-버전 6-계속 더 적은 상용구, 향상 된 명확성을 및 일관성이 언어를 변경 합니다. 클리너 초기화 구문을 사용 하는 기능은 catch/finally 블록 및 null 조건부에서 await? 연산자는 특히 유용 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4B4E41A8-68BA-4E2B-9539-881AC19971B
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 22b03c43509f3e3a55cd36ead5adef79c9086ba4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="c-6-new-features-overview"></a>C# 6 새 기능 개요

_최신 버전의 C# 언어-버전 6-계속 더 적은 상용구, 향상 된 명확성을 및 일관성이 언어를 변경 합니다. 클리너 초기화 구문을 사용 하는 기능은 catch/finally 블록 및 null 조건부에서 await? 연산자는 특히 유용 합니다._

이 문서에서는 C# 6의 새로운 기능을 소개합니다. 모노 컴파일러에서 완전히 지원 됩니다 및 개발자는 새로운 기능을 사용 하 여 모든 Xamarin 대상 플랫폼에서 시작할 수 있습니다.

이 문서는 기본적인 사용을 설명 하는 간단한 C# 6 코드 조각을 포함 합니다.
샘플 응용 프로그램은 모든 Xamarin 대상 플랫폼에서 실행 되 고 다양 한 기능을 실행 하는 명령줄 프로그램입니다.

# <a name="requirements"></a>요구 사항

## <a name="development-environment"></a>개발 환경

### <a name="mac"></a>Mac

* **Mac 용 visual Studio** C# 6에 대 한 지원: 빌드하고 C# 6 기능을 사용 하 여 Xamarin 앱을 컴파일할 수 있습니다.
  에 대해 자세히 알아보세요 [Mac 용 Visual Studio](https://docs.microsoft.com/visualstudio/mac/)합니다.

### <a name="windows"></a>Windows

* **Visual Studio 2015 및 2017** C# 6에 대 한 완벽 하 게 지원 위에 있고 합니다. 이전 버전의 Visual Studio (예:. 2013, 2012)는 C# 6를 지원 하지 않습니다.

* **Windows 용 Xamarin Studio** 편집기에서 C# 6 기능을 지원 하지 않습니다.



## <a name="compiler"></a>컴파일러

모노 C# 6 컴파일러는 모노 4.0 이상에 포함 된 [무료로 다운로드할 수](http://www.mono-project.com/download/)합니다.
Mac 용 visual Studio는 자동으로 시스템에 모노 설치를 업데이트합니다.

Windows 사용자에 게 있어야 [Visual Studio 2015 또는 2017 ^](https://www.visualstudio.com/) 코드를 컴파일하려면 C# 6 (선택한 경우에 Windows 용 Xamarin Studio IDE로) 설치 합니다.

^ 또는  *[Microsoft 빌드 도구 2015](http://www.microsoft.com/en-us/download/details.aspx?id=48159)*  명령에 대 한 명령줄 컴파일 또는 빌드 서버 예입니다.

# <a name="using-c-6"></a>C# 6을 사용 하 여

C# 6 컴파일러가 Mac. 모든 최신 버전의 Visual Studio에 사용
명령줄 컴파일러를 사용 하 여 않았는지 확인 해야 `mcs --version` 4.0 이상이 반환 합니다.
Mac 사용자 용 visual Studio 모노 4 이상 있는 경우 참조 하 여 설치를 확인할 수 **Mac 용 Visual Studio에 대 한 > Mac 용 Visual Studio > 자세한 정보 표시**합니다.

# <a name="less-boilerplate"></a>더 적은 상용구
## <a name="using-static"></a>using static
열거형 및 특정 클래스와 같은 `System.Math`, 정적 값 및 함수의 소유자에 게 주로 됩니다. C# 6에서는 단일 형식의 모든 정적 멤버를 가져올 수 있습니다 `using static` 문. 5, C# 및 C# 6에서 일반적인 삼각 함수를 비교 합니다.

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

`using static` 공용 미치지 않으며 `const` 같은 필드 `Math.PI` 및 `Math.E`, 직접 액세스할 수:

    for (var angle = 0.0; angle <= Math.PI * 2.0; angle += Math.PI / 8) ... //PI is const, not static, so requires Math.PI

## <a name="using-static-with-extension-methods"></a>정적 확장 메서드를 사용 하 여

`using static` 시설 약간 다르게 하 여 작동 확장 메서드입니다. 확장 메서드를 사용 하 여 작성 된 `static`, 연산을 수행할 인스턴스 없이도 의미를 확인 하지 않습니다. 따라서 `using static` 확장 메서드를 대상 형식에 사용할 수 있게 하는 확장 메서드를 정의 하는 형식과 함께 사용 (메서드의 `this` 유형). 예를 들어, `using static System.Linq.Enumerable` 의 API를 확장 하는 데 사용 될 `IEnumerable<T>` LINQ 종류 모두에 표시 하지 않고 개체:

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

이전 예제에서는 동작의 차이 보여 줍니다.: 확장 메서드 `Enumerable.Where` 연결 된 정적 메서드는 동안 배열 `String.Join` 에 대 한 참조 없이 호출 될 수는 `String` 유형입니다.

## <a name="nameof-expressions"></a>nameof 식
참조 하려는 경우에 따라 이름에는 변수 또는 필드를 지정 했기 때문입니다. C# 6에서 `nameof(someVariableOrFieldOrType)` 문자열을 반환 합니다 `"someVariableOrFieldOrType"`합니다. 예를 들어, throw 할 때는 `ArgumentException` 매우 많이 있는 인수가 잘못 되었습니다. 이름:

    throw new ArgumentException ("Problem with " + nameof(myInvalidArgument))

주요 이점은 `nameof` 은 형식 검사를 리팩터링 도구에서 지 원하는와 호환 되는 식이 있습니다. 형식 검사의 `nameof` 식은 경우에 특히 유용 여기서는 `string` 형식을 동적으로 연결 하는 데 사용 됩니다. 예를 들어, iOS에서에서는 `string` 프로토타입 하는 데 사용 하는 형식을 지정 하는 데 사용 되 `UITableViewCell` 개체에 `UITableView`합니다. `nameof` 이 연결에 잘못 된 철자 또는 불량 리팩터링로 인해 실패 하지 않습니다 확인해볼 수 있습니다.:

    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
    {
        var cell = tableView.DequeueReusableCell (nameof(CellTypeA), indexPath);
        cell.TextLabel.Text = objects [indexPath.Row].ToString ();
        return cell;
    }

정규화 된 이름을 전달할 수 있지만 `nameof`, 마지막 요소에만 (마지막 뒤 `.`)이 반환 됩니다. 예를 들어, 데이터 바인딩 Xamarin.Forms에 추가할 수 있습니다.

    var myReactiveInstance = new ReactiveType ();
    var myLabelOld.BindingContext = myReactiveInstance;
    var myLabelNew.BindingContext = myReactiveInstance;
    var myLabelOld.SetBinding (Label.TextProperty, "StringField");
    var myLabelNew.SetBinding (Label.TextProperty, nameof(ReactiveType.StringField));

두 호출 `SetBinding` 동일한 값을 전달 하는: `nameof(ReactiveType.StringField)` 은 `"StringField"`이 아니라 `"ReactiveType.StringField"` 처음 예상 대로 합니다.

# <a name="null-conditional-operator"></a>Null 조건부 연산자
C#에 대 한 이전 업데이트 nullable 형식 및 null 병합 연산자의 개념을 도입 `??` null 허용 값을 처리 하는 경우 기본 코드의 양을 줄일 수 있습니다. C# 6이이 테마 "null 조건부 연산자"와 함께 계속 `?.`합니다. 식의 오른쪽에 있는 개체를 사용 하는 경우 null 조건부 연산자 개체가 없으면 멤버 값을 반환 하는 `null` 및 `null` 그렇지 않은 경우:

    var ss = new string[] { "Foo", null };
    var length0 = ss [0]?.Length; // 3
    var length1 = ss [1]?.Length; // null
    var lengths = ss.Select (s => s?.Length ?? 0); //[3, 0]

(모두 `length0` 및 `length1` 형식으로 유추 `int?`)

마지막 줄에서는 이전 예제에는 `?` null 조건부 연산자와 조합 하는 `??` null 병합 연산자입니다. 새 C# 6 null 조건부 연산자는 반환 `null` 이때 null 병합 연산자 시작 되 고 0을 제공 하 여 배열에에서 두 번째 요소는 `lengths` (하는지 여부를 적절 하지 이면 물론, 문제와 관련) 배열입니다.

Null 조건부 연산자 매우 많은 응용 프로그램에서 상용구 null 검사 필요한 양의 줄여야 합니다.

Null 조건부 연산자 모호성으로 인해 몇 가지 제한이 있습니다. 즉시를 따를 수 없는 한 `?` 괄호로 묶인 인수 목록을 사용 하면 가장 큰 대리자를 사용 하 여 수행할:

    SomeDelegate?("Some Argument") // Not allowed

그러나 `Invoke` 는 구분 하는 데 사용할 수는 `?` 인수 목록에서 스타일러스가 여전히 표시 된 개선 및는 `null`-상용구 블록을 확인 하는 중:

    public event EventHandler HandoffOccurred;
    public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
    {
        HandoffOccurred?.Invoke (this, userActivity.UserInfo);
        return true;
    }


# <a name="string-interpolation"></a>문자열 보간
`String.Format` 함수는 일반적으로 사용 되는 인덱스 형식 문자열에 자리 표시자로 예: `String.Format("Expected: {0} Received: {1}.", expected, received`). 물론, 새 값을 추가 인수를 계산, 자리 표시자, 번호 다시 매기기 및 인수 목록에서 오른쪽 순서로 새 인수를 삽입 된 번거로울 거의 작업을 관련 항상 했습니다.

크게 향상 시켰습니다 C# 6의 새로운 문자열 보간 기능 `String.Format`합니다. 접두사로 추가 하는 문자열의 변수에 직접 이름을 지정할 수는 이제는 `$`합니다. 예를 들면 다음과 같습니다.

    $"Expected: {expected} Received: {received}."

변수 물론, 체크 인 되 고 철자가 잘못 되거나 사용할 수 없는 변수 컴파일러 오류가 발생 합니다.

자리 표시자를 단순 변수 될 필요가 없습니다, 식일 수 있습니다. 이러한 자리 표시자 내에서 인용 부호를 사용할 수 있습니다 *없이* 이러한 따옴표를 이스케이프 합니다. 예를 들어, 확인 된 `"s"` 다음에서:

    var s = $"Timestamp: {DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}"

문자열 보간 맞춤 및의 서식 지정 구문을 지원 `String.Format`합니다. 와 마찬가지로 이전에 작성 `{index, alignment:format}`, C# 6 작성 `{placeholder, alignment:format}`:

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

결과:

    The value is       1.00.
    The value is       2.00.
    The value is       3.00.
    The value is       4.00.
    The value is      12.00.
    The value is 123,456.00.
    Minimum is 1.00.

문자열 보간에 대 한 syntactic sugar는 `String.Format`: 함께 사용할 수 없습니다 `@""` 문자열 리터럴 및와 호환 되지 않는 `const`자리 표시 자가 사용 되는 경우에 합니다.

    const string s = $"Foo"; //Error : const requires value

여전히 문자열 보간 사용 하 여 함수 인수를 빌드 과정의 일반적인 사용 사례에 이스케이프, 인코딩 및 문화권 문제에 대해 주의 필요 합니다. URL 및 SQL 쿼리는, 물론, 중요를 삭제 합니다. 와 마찬가지로 `String.Format`, 보간을 사용 하 여 문자열의 `CultureInfo.CurrentCulture`합니다. 사용 하 여 `CultureInfo.InvariantCulture` 좀 더 장황한 것은:

    Thread.CurrentThread.CurrentCulture  = new CultureInfo ("de");
    Console.WriteLine ($"Today is: {DateTime.Now}"); //"21.05.2015 13:52:51"
    Console.WriteLine ($"Today is: {DateTime.Now.ToString(CultureInfo.InvariantCulture)}"); //"05/21/2015 13:52:51"

# <a name="initialization"></a>초기화
C# 6에는 다양 한 속성, 필드 및 멤버를 지정 하는 간단한 방법 제공 합니다.

## <a name="auto-property-initialization"></a>속성 자동 초기화
이제 필드와 같은 간결한 방식으로 자동 속성을 초기화할 수 있습니다. 변경할 수 없는 자동 속성 getter에만 쓸 수 있습니다.

    class ToDo
    {
        public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
        public DateTime Created { get; } = DateTime.Now;

생성자에는 getter 전용 auto 속성의 값을 설정할 수 있습니다.

    class ToDo
    {
        public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
        public DateTime Created { get; } = DateTime.Now;
        public string Description { get; }

        public ToDo (string description)
        {
           this.Description = description; //Can assign (only in constructor!)
        }

자동 속성에 대해이 초기화는 일반적인 공간 절약 기능 하면서 해당 개체의 불변성 강조 하기 위해 개발자에 게 유용 하 게 있습니다.

## <a name="index-initializers"></a>인덱스 이니셜라이저
C# 6 인덱서가 있는 형식에 키와 값을 설정할 수 있는 인덱스 이니셜라이저를 소개 합니다. 에 대 한 일반적으로 이것이 `Dictionary`-스타일 지정 데이터 구조:

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

## <a name="expression-bodied-function-members"></a>함수 식 본문이 멤버
람다 함수 중 하나는 단순히 공간을 저장 하는 여러 가지 이점을 갖고 있습니다. 마찬가지로, 식 본문이 클래스 멤버에는 이전 버전의 C# 6에 비해 좀 더 간략하게 표시 해야 하는 작은 함수가 허용 합니다.

함수 식 본문이 멤버 기존의 블록 구문을 아닌 람다 화살표 구문을 사용합니다.

      public override string ToString () => $"{FirstName} {LastName}";

람다 화살표 구문에서 사용 하지 않음을 명시적 공지 `return`합니다. 함수가 반환 하는 `void`, 식 문을 수도 있어야 합니다.

    public void Log(string message) => System.Console.WriteLine($"{DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}: {message}");

멤버 식 본문이 규칙이 계속 적용 됩니다는 `async` 메서드 하지만 속성이 아니라에 대 한 지원:

    //A method, so async is valid
    public async Task DelayInSeconds(int seconds) => await Task.Delay(seconds * 1000);
    //The following property will not compile
    public async Task<int> LeisureHours => await Task.FromResult<char> (DateTime.Now.DayOfWeek.ToString().First()) == 'S' ? 16 : 5;


# <a name="exceptions"></a>예외
항목에 대 한 없는 두 가지 방법으로: 예외 처리는 까다로운 합니다. C# 6의 새로운 기능을 보다 유연 하 고 일관 된 예외 처리를 확인합니다.

## <a name="exception-filters"></a>예외 필터
기본적으로 특수 한 상황에서는 예외가 발생 한 이유 및 방법에 대 한 코드를 매우 어려울 수 있습니다 *모든* 특정 유형의 예외가 발생 하는 방법입니다. C# 6 필터를 사용 하면 런타임 평가 실행 처리기를 보호 하는 기능을 소개 합니다. 추가 하 여 이렇게는 `when (bool)` 수직 후 패턴 `catch(ExceptionType)` 선언 합니다. 다음 예제에서 필터와 관련 된 구문 분석 오류를 구별는 `date` 다른 구문 분석 오류가 아닌 매개 변수입니다.

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

## <a name="await-in-catchfinally"></a>... catch에서 await를 마지막으로...
`async` C# 5에 도입 된 기능에는 언어에 대 한 게임 교환기 되었습니다. C# 5에서 `await` 에 허용 되지 않았습니다. `catch` 및 `finally` 의 값을 지정 하는 일을 차단 된 `async/await` 기능. C# 6 비동기 결과 다음 코드 조각에 나와 있는 것 처럼 프로그램을 통해 지속적으로 대기할 수 있도록이 제한을 제거 합니다.

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


# <a name="summary"></a>요약

개발자가 좋은 방법 및 도구를 지 원하는 동안 생산성을 높일 수 있도록 하려면 진화 하는 C# 언어 계속 됩니다. 이 문서에 C# 6에서의 새로운 언어 기능 개요를 지정 하 고 간단 하 게 사용 하는 방법을 제시 합니다.

## <a name="related-links"></a>관련 링크

- [C# 6의 새로운 언어 기능](https://github.com/dotnet/roslyn/wiki/New-Language-Features-in-C%23-6)
- [C# 6을 사용 하 여 통합 문서](https://developer.xamarin.com/workbooks/csharp/csharp6/csharp6.workbook)
