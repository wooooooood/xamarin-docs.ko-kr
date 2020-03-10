---
title: Unified API 개요
description: Xamarin의 Unified API를 사용 하면 Mac과 iOS 간에 코드를 공유 하 고, 32 및 64 비트 응용 프로그램을 동일한 이진으로 지원할 수 있습니다.
ms.prod: xamarin
ms.assetid: 5F0CEC18-5EF6-4A99-9DCF-1A3B57EA157C
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 8402a48602dd94578e688faeb038aec69684e7d4
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78917554"
---
# <a name="unified-api-overview"></a>Unified API 개요

Xamarin의 Unified API를 사용 하면 Mac과 iOS 간에 코드를 공유 하 고, 32 및 64 비트 응용 프로그램을 동일한 이진으로 지원할 수 있습니다. Unified API은 기본적으로 새 Xamarin.ios 및 Xamarin.ios 프로젝트에서 사용 됩니다.

> [!IMPORTANT]
> Unified API 앞에 있는 Xamarin Classic API은 더 이상 사용 되지 않습니다.
>
> - Classic API (monotouch.dialog)를 지원 하기 위한 최신 버전의 Xamarin.ios는 Xamarin.ios 9.10입니다.
> - Xamarin.ios는 여전히 Classic API을 지원 하지만 더 이상 업데이트 되지 않습니다. 개발자는 더 이상 사용 되지 않으므로 응용 프로그램을 Unified API로 이동 해야 합니다.

## <a name="updating-classic-api-based-apps"></a>Classic API 기반 앱 업데이트

플랫폼과 관련 된 지침을 따르세요.

- [기존 앱 업데이트](updating-apps.md)
- [기존 iOS 앱 업데이트](updating-ios-apps.md)
- [기존 Mac 앱 업데이트](updating-mac-apps.md)
- [기존 Xamarin.Forms 앱 업데이트](updating-xamarin-forms-apps.md)
- [바인딩을 Unified API로 마이그레이션](update-binding.md)

## <a name="tips-for-updating-code-to-the-unified-api"></a>[코드를 Unified API로 업데이트하는 팁](updating-tips.md)

마이그레이션하는 응용 프로그램에 관계 없이 Unified API로 업데이트 하는 데 도움이 되는 [다음 팁](updating-tips.md) 을 확인 하세요.

## <a name="library-split"></a>라이브러리 분할

이 시점부터 Api는 다음과 같은 두 가지 방법으로 표시 됩니다.

- **Classic API:** 32 비트 (만)로 제한 되 고 `monotouch.dll` 및 `XamMac.dll` 어셈블리에 노출 됩니다.
- **Unified API:** `Xamarin.iOS.dll` 및 `Xamarin.Mac.dll` 어셈블리에서 사용할 수 있는 단일 API를 사용 하 여 32 및 64 비트 개발을 모두 지원 합니다.

즉, 엔터프라이즈 개발자 (앱 스토어를 대상으로 하지 않음)의 경우 계속 해 서 계속 유지 관리 하거나 새 Api로 업그레이드할 수 있으므로 기존 클래식 Api를 계속 사용할 수 있습니다.

<a name="namespace-changes" />

## <a name="namespace-changes"></a>네임 스페이스 변경

Mac 및 iOS 제품 간에 코드를 공유 하는 충돌을 줄이기 위해 제품의 Api에 대 한 네임 스페이스를 변경 합니다.

IOS 제품의 접두사 "Monotouch.dialog"와 데이터 형식에 대 한 Mac 제품의 "MonoMac"를 삭제 합니다.

이렇게 하면 조건부 컴파일을 수행 하지 않고 Mac 및 iOS 플랫폼 간에 코드를 보다 간단 하 게 공유할 수 있으며, 소스 코드 파일의 맨 위에 노이즈가 줄어듭니다.

- **Classic API:** 네임 스페이스는 `MonoTouch.` 또는 `MonoMac.` 접두사를 사용 합니다.
- **Unified API:** 네임 스페이스 접두사가 없습니다.

## <a name="runtime-defaults"></a>런타임 기본값

기본적으로 Unified API는 개체 소유권 추적을 위해 **SGen** 가비지 수집기 및 [새 참조 계산](~/ios/internals/newrefcount.md) 시스템을 사용 합니다. 이와 동일한 기능이 Xamarin.ios로 이식 되었습니다.

이는 개발자가 이전 시스템에 직면 하 고 [메모리 관리](~/cross-platform/deploy-test/memory-perf-best-practices.md)를 용이 하 게 하는 다양 한 문제를 해결 합니다.

Classic API에 대해서도 새 Refcount를 사용 하도록 설정할 수 있지만 기본값은 보수적인 이며 사용자가 변경할 필요가 없습니다. Unified API를 통해 기본값을 변경 하 고 개발자가 코드를 리팩터링 하 고 다시 테스트할 때 모든 기능을 동시에 제공할 수 있습니다.

## <a name="api-changes"></a>API 변경

Unified API는 더 이상 사용 되지 않는 메서드를 제거 하 고, 클래식 Api에서 원래 Monotouch.dialog 및 MonoMac 네임 스페이스에 바인딩되는 경우 API 이름에 오타가 있는 몇 가지 인스턴스가 있습니다. 이러한 인스턴스는 새로운 통합 Api에서 수정 되었으며 구성 요소, iOS 및 Mac 응용 프로그램에서 업데이트 해야 합니다. 다음은 실행할 수 있는 가장 일반적인 항목 목록입니다.

|Classic API 메서드 이름|Unified API 메서드 이름|
|--- |--- |
|`UINavigationController.PushViewControllerAnimated()`|`UINavigationController.PushViewController()`|
|`UINavigationController.PopViewControllerAnimated()`|`UINavigationController.PopViewController()`|
|`CGContext.SetRGBFillColor()`|`CGContext.SetFillColor()`|
|`NetworkReachability.SetCallback()`|`NetworkReachability.SetNotification()`|
|`CGContext.SetShadowWithColor`|`CGContext.SetShadow`|
|`UIView.StringSize`|`UIKit.UIStringDrawing.StringSize`|

클래식에서 Unified API로 전환할 때 전체 변경 내용 목록은 [클래식 (monotouch.dialog) Vs 통합 (xamarin.ios) API 차이점](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md) 설명서를 참조 하세요.

## <a name="updating-to-unified"></a>통합으로 업데이트

**클래식** 의 여러 이전/중단/사용 되지 않는 Api는 **통합** api에서 사용할 수 없습니다. `[Obsolete]` 특성 메시지 (경고의 일부)를 제공 하 여 올바른 API를 안내 하므로 (수동 또는 자동) 업그레이드를 시작 하기 전에 `CS0616` 경고를 수정 하는 것이 더 쉬울 수 있습니다.

프로젝트 업데이트 전이나 후에 사용할 수 있는 클래식 및 통합 API 변경 내용의 [*차이*](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md) 를 게시 하 고 있습니다. 클래식에서 obsoletes 호출을 수정 하는 것은 종종 시간 보호기 (문서 조회 감소)입니다.

다음 지침에 따라 [기존 iOS 앱](~/cross-platform/macios/unified/updating-ios-apps.md)또는 [Mac 앱](~/cross-platform/macios/unified/updating-mac-apps.md) 을 Unified API로 업데이트 합니다.
코드 마이그레이션에 대 한 자세한 내용은이 페이지의 나머지 부분을 검토 하 고 [이러한 팁](~/cross-platform/macios/unified/updating-tips.md) 을 참조 하십시오.

### <a name="nuget"></a>NuGet

이전에 **Monotouch10** 플랫폼 모니커를 사용 하 여 어셈블리를 게시 한 Classic API 통해 xamarin.ios를 지 원하는 NuGet 패키지입니다.

Unified API에는 호환 되는 패키지 ( **iOS10**)에 대 한 새 플랫폼 식별자가 도입 되었습니다. Unified API에 대해 빌드하여 기존 NuGet 패키지를 업데이트 하 여이 플랫폼에 대 한 지원을 추가 해야 합니다.

> [!IMPORTANT]
> 오류가 있는 경우 _"오류 3에 ' monotouch.dialog '과 ' xamarin.ios '를 모두 포함할 수 없습니다. iOS 프로젝트-' xamarin.ios '는 명시적으로 참조 되지만 ' monotouch.dialog '는 ' xxx ' Version = 0.0.000, Culture = 중립, PublicKeyToken = null '에 의해 참조_ 되는 반면, 응용 프로그램을 통합 된 api로 변환한 후에는 일반적으로 Unified API 업데이트 되지 않은 구성 요소 또는 NuGet 패키지가 프로젝트에 포함 되어 있기 때문입니다. 기존 component/NuGet을 제거 하 고, 통합 Api를 지 원하는 버전으로 업데이트 하 고, 클린 빌드를 수행 해야 합니다.

### <a name="the-road-to-64-bits"></a>64 비트로 이동

32 및 64 비트 응용 프로그램 지원에 대 한 배경 및 프레임 워크에 대 한 정보는 [32 및 64 Bit 플랫폼 고려 사항](~/cross-platform/macios/32-and-64/index.md)을 참조 하세요.

 <a name="new-data-types" />

#### <a name="new-data-types"></a>새 데이터 형식

차이점의 핵심에서 Mac 및 iOS Api는 모두 32 비트 플랫폼의 경우 항상 32 비트이 고 64 비트 플랫폼에서는 64 비트가 있는 아키텍처 관련 데이터 형식을 사용 합니다.

예를 들어, 목표-C는 32 비트 시스템의 `int32_t`에 `NSInteger` 데이터 형식을 매핑하고 64 비트 시스템에서 `int64_t` 합니다.

이 동작을 일치 시키기 위해 Unified API의 이전 `int` 사용 (.NET은 항상 `System.Int32`로 정의 됨)을 새 데이터 형식 (`System.nint`)으로 바꿉니다.  "N"을 의미 "native"로 간주할 수 있으므로 플랫폼의 네이티브 정수 형식입니다.

`nint`, `nuint` 및 `nfloat`를 소개 하 고, 필요한 경우 위에 구축 된 데이터 형식을 제공 합니다.

이러한 데이터 형식 변경에 대해 자세히 알아보려면 [네이티브 형식](~/cross-platform/macios/nativetypes.md) 문서를 참조 하세요.

### <a name="how-to-detect-the-architecture-of-ios-apps"></a>IOS 앱의 아키텍처를 검색 하는 방법

응용 프로그램이 32 비트 또는 64 비트 iOS 시스템에서 실행 되 고 있는지 확인 해야 하는 경우가 있을 수 있습니다. 아키텍처를 확인 하는 데 사용할 수 있는 코드는 다음과 같습니다.

```csharp
if (IntPtr.Size == 4) {
    Console.WriteLine ("32-bit App");
} else if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit App");
}
```

<a name="deprecated-apis" />

### <a name="arrays-and-systemcollectionsgeneric"></a>배열 및 System.object

인덱서에 C# 는 `int`형식이 필요 하기 때문에 컬렉션 또는 배열의 요소에 액세스 하려면 `nint` 값을 `int`로 명시적으로 캐스팅 해야 합니다. 다음은 그 예입니다.

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

이는 예상 된 동작입니다. `int`에서 `nint`로의 캐스팅이 64 비트의 손실 이기 때문에 암시적 변환이 수행 되지 않습니다.

### <a name="converting-datetime-to-nsdate"></a>DateTime을 NSDate로 변환

통합 Api를 사용 하는 경우 `DateTime`를 `NSDate` 값으로 암시적으로 변환 하는 것이 더 이상 수행 되지 않습니다. 이러한 값은 한 형식에서 다른 형식으로 명시적으로 변환 해야 합니다. 다음 확장 메서드를 사용 하 여이 프로세스를 자동화할 수 있습니다.

```csharp
public static DateTime NSDateToDateTime(this NSDate date)
{
    // NSDate has a wider range than DateTime, so clip
    // the converted date to DateTime.Min|MaxValue.
    double secs = date.SecondsSinceReferenceDate;
    if (secs < -63113904000)
        return DateTime.MinValue;
    if (secs > 252423993599)
        return DateTime.MaxValue;
    return (DateTime) date;
}

public static NSDate DateTimeToNSDate(this DateTime date)
{
    if (date.Kind == DateTimeKind.Unspecified)
        date = DateTime.SpecifyKind (date, /* DateTimeKind.Local or DateTimeKind.Utc, this depends on each app */)
    return (NSDate) date;
}

```

<a name="deprecated-typos" />

### <a name="deprecated-apis-and-typos"></a>사용 되지 않는 Api 및 오타가 있습니다.

Xamarin.ios 클래식 API (monotouch.dialog) 내에서 `[Obsolete]` 특성은 다음 두 가지 방법으로 사용 되었습니다.

- **사용 되지 않는 IOS API:** 이는 Apple 힌트가 API 사용을 중지 하는 것입니다 .이는 새 항목으로 대체 되기 때문입니다. 이전 버전의 iOS를 지 원하는 경우에는 Classic API 계속 해 서 필요한 경우가 많습니다.
 이러한 API (및 `[Obsolete]` 특성)는 새 Xamarin.ios 어셈블리에 포함 됩니다.
- **잘못 된 API** 일부 API는 이름에 오타가 있습니다.

원본 어셈블리 (monotouch.dialog 및 XamMac)의 경우 이전 코드를 호환성에 사용할 수 있도록 유지 했지만 Unified API 어셈블리 (Xamarin.ios 및 Xamarin.ios)에서 제거 되었습니다.

<a name="NSObject_ctor" />

### <a name="nsobject-subclasses-ctorintptr"></a>NSObject 서브 클래스 .ctor (IntPtr)

모든 `NSObject` 서브 클래스에는 `IntPtr`를 허용 하는 생성자가 있습니다. 이는 네이티브 ObjC 핸들에서 새로운 관리 되는 인스턴스를 인스턴스화할 수 있는 방법입니다.

클래식에서는 `public` 생성자 였습니다. 그러나 사용자 코드에서이 기능을 사용 하는 것은 간단 합니다. 예 *를 들어 단일 ObjC 인스턴스에 대해* 여러 개의 관리 되는 인스턴스를 만들거나 필요한 관리 되는 상태 (서브 클래스의 경우)가 없는 관리 되는 인스턴스를 만들 수 있습니다.

이러한 종류의 문제를 방지 하기 위해 `IntPtr` 생성자는 이제 **통합** API에서 `protected` 하 여 서브클래싱에만 사용할 수 있습니다. 이렇게 하면 올바른/안전 API를 사용 하 여 핸들에서 관리 되는 인스턴스를 만들 수 있습니다. 즉,

```csharp
var label = Runtime.GetNSObject<UILabel> (handle);
```

이 API는 기존 관리 되는 인스턴스를 반환 하거나 (이미 있는 경우) 새 인스턴스 (필요한 경우)를 만듭니다. 클래식 API와 통합 API 모두에서 이미 사용할 수 있습니다.

이제 `.ctor(NSObjectFlag)` `protected` 수도 있지만이는 하위 클래스 외부에서 거의 사용 되지 않았습니다.

<a name="NSAction" />

### <a name="nsaction-replaced-with-action"></a>NSAction이 작업으로 대체 됨

통합 Api를 사용 하면 표준 .NET `Action`를 위해 `NSAction` 제거 되었습니다. 이는 `Action` 일반적인 .NET 형식 이지만 `NSAction`는 Xamarin.ios와 관련 되어 있기 때문에 크게 향상 된 기능입니다. 두 항목은 모두 정확히 동일한 작업을 수행 하지만 형식이 서로 다르며 호환 되지 않는 형식 이므로 동일한 결과를 얻기 위해 더 많은 코드를 작성 해야 했습니다.

예를 들어 기존 Xamarin 응용 프로그램에 다음 코드가 포함 된 경우입니다.

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (new NSAction (delegate() {
    ShowDropDownAnimated (tblDataView);
}));
```

이제 간단한 람다로 바꿀 수 있습니다.

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (() => ShowDropDownAnimated(tblDataView));
```

이전에는 `Action` `NSAction`에 할당할 수 없기 때문에 컴파일러 오류가 발생 했지만 `UITapGestureRecognizer` 이제는 통합 Api에서 유효한 `NSAction` 대신 `Action`를 사용 합니다.

### <a name="custom-delegates-replaced-with-actiont"></a>사용자 지정 대리자를 작업\<T로 대체 >

**통합** 된 몇 가지 간단한 (예: 하나의 매개 변수) .net 대리자는 `Action<T>`로 대체 되었습니다. 예를 들어

```csharp
public delegate void NSNotificationHandler (NSNotification notification);
```

이제를 `Action<NSNotification>`사용할 수 있습니다. 이 코드를 승격 하면 Xamarin.ios와 사용자 고유의 응용 프로그램 내에서 코드 중복을 다시 사용 하 고 줄일 수 있습니다.

### <a name="taskbool-replaced-with-taskbooleannserror"></a>작업\<bool >는 Task < Boolean, NSError >로 바뀝니다 >

**클래식** 에는 `Task<bool>`를 반환 하는 비동기 api가 있었습니다. 그러나이 중 일부는 `NSError` 시그니처의 일부일 때를 사용할 수 있습니다. 즉, `bool` 이미 `true` 되었으며 `NSError`를 얻기 위해 예외를 catch 해야 합니다.

일부 오류는 매우 일반적 이며 반환 값은 유용 하지 않기 때문에 `Task<Tuple<Boolean,NSError>>`를 반환 하도록 **통합** 에서이 패턴이 변경 되었습니다. 이렇게 하면 비동기 호출 중에 발생 했을 수 있는 성공 및 오류를 모두 확인할 수 있습니다.

### <a name="nsstring-vs-string"></a>NSString vs 문자열

일부 경우에는 일부 상수를 `string`에서 `NSString`으로 변경 해야 했습니다 (예: `UITableViewCell`

**클래식**

```csharp
public virtual string ReuseIdentifier { get; }
```

**통합형**

```csharp
public virtual NSString ReuseIdentifier { get; }
```

일반적으로 .NET `System.String` 형식을 선호 합니다. 그러나 Apple 지침에도 불구 하 고, 일부 네이티브 API는 상수 포인터 (문자열 자체 아님)를 비교 하며이는 상수를 `NSString`으로 노출 하는 경우에만 작동할 수 있습니다.

 <a name="protocols" />

### <a name="objective-c-protocols"></a>목표-C 프로토콜

원래 Monotouch.dialog는 ObjC 프로토콜에 대 한 완전 한 지원을 제공 하지 않았으며, 가장 일반적인 시나리오를 지원 하기 위해 최적화 되지 않은 몇 가지 API가 추가 되었습니다. 이러한 제한 사항은 더 이상 존재 하지 않지만 이전 버전과의 호환성을 위해 `monotouch.dll` 및 `XamMac.dll`내부에 몇 가지 Api가 유지 됩니다.

이러한 제한 사항은 통합 Api에서 제거 되 고 정리 되었습니다. 대부분의 변경 내용은 다음과 같습니다.

**클래식**

```csharp
public virtual AVAssetResourceLoaderDelegate Delegate { get; }
```

**통합형**

```csharp
public virtual IAVAssetResourceLoaderDelegate Delegate { get; }
```

`I` 접두사는 지정 된 형식 대신 ObjC 프로토콜에 대 한 인터페이스 **를 제공 하** 는 것을 의미 합니다. 이렇게 하면 Xamarin.ios에서 제공 하는 특정 형식을 서브 클래스 하지 않으려고 할 수 있습니다.

또한 일부 API는 보다 정확 하 고 사용 하기 쉽습니다. 예를 들면 다음과 같습니다.

**클래식**

```csharp
public virtual void SelectionDidChange (NSObject uiTextInput);
```

**통합형**

```csharp
public virtual void SelectionDidChange (IUITextInput uiTextInput);
```

이러한 API는 이제 설명서를 참조 하지 않고 더 쉽게 사용할 수 있으며, IDE 코드 완성 기능을 통해 프로토콜/인터페이스를 기반으로 하는 더 유용한 제안 사항을 제공할 수 있습니다.

#### <a name="nscoding-protocol"></a>NSCoding 프로토콜

원래 바인딩에는 `NSCoding` 프로토콜을 지원 하지 않는 경우에도 모든 형식에 대 한 .ctor (NSCoder)가 포함 되어 있습니다.  개체를 인코딩하기 위해 `NSObject`에 단일 `Encode(NSCoder)` 메서드가 있습니다.
그러나이 메서드는 인스턴스가 NSCoding 프로토콜에 맞춘 경우에만 작동 합니다.

Unified API에서는이 문제를 해결 했습니다.  형식이 `NSCoding`를 따르는 경우에만 새 어셈블리에 `.ctor(NSCoder)` 있습니다. 또한 이러한 형식에는 이제 `INSCoding` 인터페이스를 따르는 `Encode(NSCoder)` 메서드가 있습니다.

낮은 영향: 대부분의 경우이 변경은 이전에 제거 된 생성자를 사용할 수 없기 때문에 응용 프로그램에 영향을 주지 않습니다.

## <a name="further-tips"></a>추가 팁

에 대해 알아 두어야 할 추가 변경 내용은 [Unified API 앱을 업데이트 하기 위한 팁](~/cross-platform/macios/unified/updating-tips.md)에 나와 있습니다.


## <a name="related-links"></a>관련 링크

- [IOS 앱 업데이트](updating-ios-apps.md)
- [Mac 앱 업데이트](updating-mac-apps.md)
- [Xamarin Forms 앱 업데이트](updating-xamarin-forms-apps.md)
- [바인딩 업데이트](update-binding.md)
- [팁 업데이트](updating-tips.md)
- [클래식 vs Unified API 차이점](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
- [플랫폼 간 앱에서의 네이티브 형식 작업](~/cross-platform/macios/native-types-cross-platform.md)
