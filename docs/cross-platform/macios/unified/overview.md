---
title: 통합 된 API 개요
description: Xamarin의 통합 API Mac 및 iOS 지원 32 비트 및 64 비트 응용 프로그램 간에 동일한 이진 파일을 사용 하 여 코드를 공유할 수 있습니다.
ms.prod: xamarin
ms.assetid: 5F0CEC18-5EF6-4A99-9DCF-1A3B57EA157C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 1d159d280bd3b8855c32e3e437dfdefcbe0463cb
ms.sourcegitcommit: 4859da8772dbe920fdd653180450e5ddfb436718
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50235027"
---
# <a name="unified-api-overview"></a>통합 된 API 개요

Xamarin의 통합 API Mac 및 iOS 지원 32 비트 및 64 비트 응용 프로그램 간에 동일한 이진 파일을 사용 하 여 코드를 공유할 수 있습니다. 통합 API는 새 Xamarin.iOS 및 Xamarin.Mac 프로젝트에는 기본적으로 사용 됩니다.

> [!IMPORTANT]
> 앞에 통합 API를 사용 하 여 Xamarin 클래식 API는 더 이상 사용 되지 않습니다. 
> - 클래식 API (monotouch.dll)를 지원 하기 위해 Xamarin.iOS의 마지막 버전 9.10 Xamarin.iOS 했습니다.
> - Xamarin.Mac을 클래식 API를 계속 지원 되지만 더 이상으로 업데이트 됩니다. 되지 하므로 개발자가 응용 프로그램 통합 API로 이동 해야 합니다.

## <a name="updating-classic-api-based-apps"></a>클래식 API 기반 앱 업데이트

플랫폼에 대 한 관련 지침을 따르세요.

- [기존 앱 업데이트](updating-apps.md)
- [기존 iOS 앱 업데이트](updating-ios-apps.md)
- [기존 Mac 앱 업데이트](updating-mac-apps.md)
- [기존 Xamarin.Forms 앱 업데이트](updating-xamarin-forms-apps.md)
- [바인딩을 Unified API로 마이그레이션](update-binding.md)

## <a name="tips-for-updating-code-to-the-unified-apiupdating-tipsmd"></a>[코드를 Unified API로 업데이트하는 팁](updating-tips.md)

마이그레이션하는 응용 프로그램에 관계 없이, 체크 아웃 [이러한 팁](updating-tips.md) 통합 API로 성공적으로 업데이트할 수 있습니다.

## <a name="library-split"></a>라이브러리 분할

이 시점에서 두 가지 방법으로 Api는 표시 됩니다.

-  **클래식 API:** 32 비트 (전용) 제한 및 노출 합니다 `monotouch.dll` 및 `XamMac.dll` 어셈블리.
-  **통합된 API:** 에서 사용할 수 있는 단일 API 사용 하 여 모두 32 비트 및 64 비트 개발을 지원 합니다 `Xamarin.iOS.dll` 및 `Xamarin.Mac.dll` 어셈블리입니다.

즉, 엔터프라이즈에 대 한 새로운 Api를 개발자 (없습니다 대상으로 하는 앱 스토어), 영원히 또는 있습니다 유지 유지는 대로 기존 클래식 Api를 사용 하 여 계속 수 있습니다 업그레이드할 수 있습니다.

<a name="namespace-changes" />

## <a name="namespace-changes"></a>Namespace 변경

Mac 및 iOS 제품 간에 코드를 공유 마찰을 줄이기 위해 제품의 Api에 대 한 네임 스페이스를 변경 합니다.

에서는 삭제 하 고 접두사 "MonoTouch" iOS 제품 "MonoMac"의 데이터 형식에 따라 Mac 제품에서.

이 간편 하 게 조건부 컴파일을 사용 하지 않고도 Mac 및 iOS 플랫폼 간에 코드 공유 및 소스 코드 파일의 맨 위에 있는 노이즈를 줄일 수 있습니다.

-  **클래식 API:** 네임 스페이스 사용 `MonoTouch.` 또는 `MonoMac.` 접두사입니다.
-  **통합 된 API:** 네임 스페이스 접두사가 없는

## <a name="runtime-defaults"></a>런타임 기본값

기본적으로 통합 API는 **SGen** 가비지 수집기 및 [새 참조 횟수](~/ios/internals/newrefcount.md) 개체 소유권을 추적 하기 위한 시스템입니다. Xamarin.Mac로 동일한 기능으로 이식 되었습니다.

많은 개발자가 이전 시스템을 사용 하 여 직면 한도 쉽게 수행할 수 있는 문제를 해결할 [메모리 관리](~/cross-platform/deploy-test/memory-perf-best-practices.md)합니다.

새 Refcount 클래식 API에 대해서도 사용 하도록 설정할 수는 있지만 기본 보수적인 이며 사용자가 변경할 필요가 없습니다. 통합 API를 사용 하 여 기본값을 변경 하는 기회를 하 고 동시에 개발자가 모든 향상 된 기능을 제공 합니다. 이러한 리팩터링 및 코드를 다시 테스트 합니다.

## <a name="api-changes"></a>API 변경 내용

통합 API는 사용 되지 않는 메서드를 제거 하 고 몇 가지 인스턴스가 있습니다 클래식 Api의 원래 MonoTouch 및 MonoMac 네임 스페이스에 바인딩된 경우 API 이름에 오타가 있었던 합니다. 이러한 인스턴스는 새 통합 Api에서 해결 되었습니다 하 고 구성 요소, iOS 및 Mac 응용 프로그램에서 업데이트 해야 합니다. 가장 일반적으로 발생할 수의 목록을 다음과 같습니다.

|클래식 API 메서드 이름|통합 된 API 메서드 이름|
|--- |--- |
|`UINavigationController.PushViewControllerAnimated()`|`UINavigationController.PushViewController()`|
|`UINavigationController.PopViewControllerAnimated()`|`UINavigationController.PopViewController()`|
|`CGContext.SetRGBFillColor()`|`CGContext.SetFillColor()`|
|`NetworkReachability.SetCallback()`|`NetworkReachability.SetNotification()`|
|`CGContext.SetShadowWithColor`|`CGContext.SetShadow`|
|`UIView.StringSize`|`UIKit.UIStringDrawing.StringSize`|

클래식에서 통합 API로 전환 하는 경우 변경의 전체 목록을 참조 하세요. 당사의 [클래식 (monotouch.dll) vs 통합 (Xamarin.iOS.dll) API 차이점](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) 설명서.

## <a name="updating-to-unified"></a>업데이트에 통합

이전/분류/사용 되지 않는 몇 가지 API **클래식** 에서 사용할 수 없는 합니다 **통합** API. 해결 하기가 수는 `CS0616` 경고 프로그램 (수동 또는 자동)를 시작 하기 전에 업그레이드 해야 하므로 `[Obsolete]` 오른쪽 api 안내 메시지 (경고의 일부) 특성입니다.

게시 하는 것에 [ *diff* ](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) 클래식 vs 통합 프로젝트 업데이트 전후 사용할 수 있는 API 변경 내용입니다. 여전히 수정는 클래식 됩니다에 대 한 호출을 자주 obsoletes 하 여 (작은 문서 조회) 시간을 절약할 수 있습니다.

다음이 지침에 따라 [기존 iOS 앱을 업데이트](~/cross-platform/macios/unified/updating-ios-apps.md), 또는 [Mac 앱](~/cross-platform/macios/unified/updating-mac-apps.md) 통합 API로 합니다.
이 페이지의 나머지 부분을 검토 하 고 [이러한 팁](~/cross-platform/macios/unified/updating-tips.md) 코드 마이그레이션하는 방법에 대 한 자세한 내용은 합니다.

### <a name="nuget"></a>NuGet

이전에 클래식 API를 통해 Xamarin.iOS 지원 되는 NuGet 패키지를 사용 하 여 해당 어셈블리를 게시 합니다 **Monotouch10** 플랫폼 모니커입니다.

Unified API 소개-호환 패키지에 대 한 새 플랫폼 식별자 **Xamarin.iOS10**합니다. 기존 NuGet 패키지를 Unified API에 대해 구성 하 여이 플랫폼에 대 한 지원을 추가 하도록 업데이트 해야 합니다.

> [!IMPORTANT]
> 폼에 오류가 있는 경우 _"오류 3 동일한 Xamarin.iOS 프로젝트에서 'monotouch.dll' 및 'Xamarin.iOS.dll'를 포함할 수 없습니다-'Xamarin.iOS.dll'는 'monotouch.dll'에서 참조 하는 동안 명시적으로 참조 됩니다 ' xxx, 버전 = 0.0.000, Culture = neutral, PublicKeyToken = null'"_ Unified Api로 응용 프로그램으로 변환 후 일반적으로 통합 API로 업데이트 되지 않은 프로젝트에서 구성 요소 또는 NuGet 패키지 사용 하지 때문입니다. 기존 구성 요소/NuGet을 제거 하 고, 통합 Api를 지 원하는 버전으로 업데이트 하 고, 클린 빌드를 수행 해야 합니다.

### <a name="the-road-to-64-bits"></a>64 비트가는 길

32 비트 및 64 비트 응용 프로그램 및 프레임 워크에 대 한 정보를 지원 하기 위해 백그라운드에 대 한 참조를 [32 및 64 비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64/index.md)합니다.

 <a name="new-data-types" />

#### <a name="new-data-types"></a>새 데이터 형식

차이의 핵심인 Mac 및 iOS Api 모두는 항상 32 비트에서 32 비트 플랫폼 및 64 비트 플랫폼에서 64 비트는 아키텍처 관련 데이터 형식을 사용 합니다.

예를 들어, Objective-c로 매핑하는 `NSInteger` 데이터 형식을 `int32_t` 32 비트 시스템 및 `int64_t` 64 비트 시스템에서.

통합 API에서이 동작은 일치를 교체 하는 이전을 사용 하는 `int` (항상으로 정의 되는.NET `System.Int32`)을 새 데이터 형식: `System.nint`합니다.  있습니다 생각할 수 있습니다 "n" 의미 "네이티브" 플랫폼의 기본 정수를 입력 합니다.

도입한 `nint`, `nuint` 고 `nfloat` 필요한 경우 이러한 기반으로 구축 된 데이터 형식으로 제공 합니다.

이러한 데이터 형식 변경에 대 한 자세한 내용은 참조는 [네이티브 형식을](~/cross-platform/macios/nativetypes.md) 문서.

### <a name="how-to-detect-the-architecture-of-ios-apps"></a>IOS 앱의 아키텍처를 검색 하는 방법

응용 프로그램을 32 비트 또는 64 비트 iOS 시스템에서 실행 되 고 있는지 알고 해야 하는 경우가 있을 수 있습니다. 아키텍처를 확인 하려면 다음 코드를 사용할 수 있습니다.

```csharp
if (IntPtr.Size == 4) {
    Console.WriteLine ("32-bit App");
} else if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit App");
}
```

<a name="deprecated-apis" />

### <a name="arrays-and-systemcollectionsgeneric"></a>배열 및 System.Collections.Generic

때문에 C# 인덱서의 형식을 예상 `int`를 명시적으로 캐스팅 해야 `nint` 값을 `int` 컬렉션이 나 배열의 요소에 액세스 하려면. 예를 들어:

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

왜냐하면 예상된 동작에서 캐스팅 `int` 에 `nint` 는 64 비트에서 손실, 암시적 변환이 수행 되지 않습니다.

### <a name="converting-datetime-to-nsdate"></a>NSDate 날짜/시간 변환

암시적으로 변환 된 통합 Api를 사용 하는 경우 `DateTime` 에 `NSDate` 값 이상으로 수행 됩니다. 이러한 값을 명시적으로 다른 형식 간에 변환 될 해야 합니다. 이 프로세스를 자동화 하려면 다음 확장 메서드를 사용할 수 있습니다.

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

### <a name="deprecated-apis-and-typos"></a>사용 되지 않는 Api 및 오타

내부 Xamarin.iOS 클래식 (monotouch.dll) API는 `[Obsolete]` 특성을 두 가지 방법으로 사용 했습니다.

-  **IOS API를 사용 되지 않음:** 때 최신 버전으로 대체 되는 때문에 API를 사용 하 여 중지 하는 데 Apple 힌트입니다. 클래식 API 이것도 이며 종종 필요 (iOS의 이전 버전 지원) 하는 경우.
 이러한 API (및 `[Obsolete]` 특성) 새 Xamarin.iOS 어셈블리에 포함 되어 있습니다.
-  **잘못 된 API** 일부 API 이름에 오타를 했습니다.

호환성을 위해 사용할 수 있는 기존 코드를 유지 하면서 원래 어셈블리 (monotouch.dll 및 XamMac.dll)에 대 한 하지만 Xamarin.iOS.dll과 Xamarin.Mac 통합 API 어셈블리에서 제거한

<a name="NSObject_ctor" />

### <a name="nsobject-subclasses-ctorintptr"></a>NSObject 서브 클래스.ctor(IntPtr)

모든 `NSObject` 서브 클래스에 허용 하는 생성자는 `IntPtr`합니다. 이것이 어떻게 네이티브 ObjC 핸들에서 새 관리 되는 인스턴스를 인스턴스화할 수 것입니다.

이것이 클래식에서을 `public` 생성자입니다. 하지만이 사용자 코드에서이 기능을 오용 하기는 몇 가지를 만드는 예를 들어 관리 되는 인스턴스 단일 ObjC 인스턴스 *또는* 는 관리 되는 인스턴스 만들기 (하위 클래스)에 대 한 예상 되는 관리 되는 상태 부족 합니다.

이러한 유형의 문제를 방지 하려면 합니다 `IntPtr` 생성자는 이제 `protected` 에 **통합** 하위 클래스에 대해서만 사용할 수 있도록 API. 그러면 수정/safe API 즉, 핸들에서 관리 되는 인스턴스를 만드는 데 사용 됩니다.

    var label = Runtime.GetNSObject<UILabel> (handle);

이 API (이미 있는 경우) 기존 관리 되는 인스턴스를 반환 합니다 (필수) 하는 경우 새로 만듭니다. 클래식 및 통합 된 API에서 사용할 수 있는 이미입니다.

유의 합니다 `.ctor(NSObjectFlag)` 이기도 `protected` 하는데이 하위 클래스 외부에서 거의 사용 했습니다.

<a name="NSAction" />

### <a name="nsaction-replaced-with-action"></a>작업을 사용 하 여 대체 NSAction

Unified Api를 통해 `NSAction` 표준.NET 위해 제거 되었습니다 `Action`합니다. 이 중대 한 개선 사항은 때문 `Action` 반면 공용.NET 형식에는 `NSAction` xamarin.ios 것 이었습니다. 정확히 동일한 작업을 모두 수행 없지만 distinct 및 호환 되지 않는 형식 같은 결과 쓸 수 있도록 더 많은 코드에서 발생 합니다.

예를 들어 다음과 같이 기존 Xamarin 응용 프로그램에 다음 코드가 포함 되어 있습니다.

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (new NSAction (delegate() {
    ShowDropDownAnimated (tblDataView);
}));
```
이제 간단한 람다를 사용 하 여 교체할 수 있습니다.

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (() => ShowDropDownAnimated(tblDataView));
```

이전 되는 컴파일러 오류 때문에 `Action` 할당할 수 없습니다 `NSAction`, 있지만 `UITapGestureRecognizer` 이제는 `Action` 대신는 `NSAction` Unified Api에서 유효 합니다.

### <a name="custom-delegates-replaced-with-actiont"></a>작업으로 교체 하는 사용자 지정 대리자<T>

**통합** (예: 하나의 매개 변수) 간단한.net의 대리자 대체 됨 `Action<T>`합니다. 예:

    public delegate void NSNotificationHandler (NSNotification notification);

이제 사용할 수는 `Action<NSNotification>`합니다. 이 승격 코드 다시 사용 하 고 Xamarin.iOS 응용 프로그램과 사용자 고유의 응용 프로그램 내에서 코드 중복을 줄입니다.

### <a name="taskbool-replaced-with-taskbooleannserror"></a>작업<bool> < 부울, NSError >> 태스크로 대체

**클래식** 있었습니다 일부 비동기 Api 반환 `Task<bool>`합니다. 하지만 일부 그 중 여기서 사용 하는 `NSError` 서명의 즉 속해를 `bool` 이미 `true` 가져오려는 예외를 catch 해야 하 고는 `NSError`.

이 패턴에서 변경 된 일부 오류는 매우 일반적 이므로 반환 값에 유용 하지 않아서 **통합** 반환할는 `Task<Tuple<Boolean,NSError>>`합니다. 따라서 성공 및 비동기 호출 하는 동안 발생 했을 수 있는 모든 오류를 모두 확인할 수 있습니다.

### <a name="nsstring-vs-string"></a>NSString vs 문자열

경우에 따라에서 일부 상수에서 변경 해야 `string` 에 `NSString`예 `UITableViewCell`

**클래식**

    public virtual string ReuseIdentifier { get; }

**통합**

    public virtual NSString ReuseIdentifier { get; }

.NET 좋습니다 일반적 `System.String` 형식입니다. 그러나 일부 네이티브 API는 상수 포인터 (하지 자체 문자열)을 비교 하는 데 Apple 지침에도 불구 하 고 있으며으로 상수를 노출 하는 경우만 작동할 수 있습니다 `NSString`합니다.

 <a name="protocols" />

### <a name="objective-c-protocols"></a>Objective-c 프로토콜

원래 MonoTouch 전체 지원 ObjC 프로토콜에 대 한 일부 최적이 아닌 없는, API의 가장 일반적인 시나리오를 지원 하기 위해 추가 되었습니다. 이 제한은 더 이상 존재 하지 않습니다 하지만 여러 Api 유지는 이전 버전과 호환성을 위해 내부 `monotouch.dll` 고 `XamMac.dll`입니다.

이러한 제한 사항은 제거 되 고 Unified Api에서 정리 합니다. 대부분의 변경 내용을 다음과 같이 표시 됩니다.

**클래식**

    public virtual AVAssetResourceLoaderDelegate Delegate { get; }

**통합**

    public virtual IAVAssetResourceLoaderDelegate Delegate { get; }

합니다 `I` 수단을 접두사 **통합** ObjC 프로토콜에 대 한 특정 형식 대신 인터페이스를 노출 합니다. 이 원하지 않는 하위 Xamarin.iOS에서 제공 되는 특정 형식의 경우 완화 됩니다.

또한 허용 되는 일부 API를 더욱 정확 하 고 사용 하기 쉬운 예를 들어 다음과 같습니다.

**클래식**

    public virtual void SelectionDidChange (NSObject uiTextInput);

**통합**

    public virtual void SelectionDidChange (IUITextInput uiTextInput);

이러한 API 설명서를 참조 하지 않고 쉽게 우리에 게 이제 되며 IDE 코드 완료 프로토콜/인터페이스를 기반으로 하는 유용한 제안과 함께 제공 됩니다.

#### <a name="nscoding-protocol"></a>NSCoding 프로토콜

우리의 원래 바인딩을 지원 하지 않는 경우에는.ctor(NSCoder)-모든 형식에 대 한 포함 된 `NSCoding` 프로토콜입니다.  단일 `Encode(NSCoder)` 에 표시 된 메서드는 `NSObject` 개체를 인코딩할 합니다.
하지만이 메서드는 인스턴스 NSCoding 프로토콜을 따르도록 하는 경우에 작동 합니다.

Unified API에서이 수정 했습니다.  새 어셈블리는 하나만 합니다 `.ctor(NSCoder)` 형식을 준수 하는 경우 `NSCoding`합니다. 또한 이러한 형식을 이제는 `Encode(NSCoder)` 준수 하는 메서드는 `INSCoding` 인터페이스입니다.

낮은 영향: 대부분의 경우에서이 변경 영향을 주지 않습니다 응용 프로그램 이전, 제거, 생성자를 사용할 수 없습니다.

## <a name="further-tips"></a>추가 팁

알아야 할 추가 변경 내용은에 나와 합니다 [통합 API로 앱을 업데이트 하기 위한 팁](~/cross-platform/macios/unified/updating-tips.md)합니다.

## <a name="sample-code"></a>샘플 코드

년 7 월 31 일을 기준으로이 새 API로 iOS 샘플의 포트에 게시 한 것은 `magic-types` 에서 분기 [monotouch 샘플](https://github.com/xamarin/monotouch-samples/commits/magic-types)합니다.

Mac 용 둘 다에서 샘플을 확인 합니다 [mac 샘플](https://github.com/xamarin/mac-samples) magic 형식 분기에서 32/64 비트 샘플 뿐만 아니라 (Mavericks/Yosemite에서 새 Api를 표시) 리포지토리 [mac 샘플](https://github.com/xamarin/monotouch-samples/commits/magic-types)합니다.

## <a name="related-links"></a>관련 링크

- [IOS 앱 업데이트](updating-ios-apps.md)
- [Mac 앱 업데이트](updating-mac-apps.md)
- [Xamarin.Forms 앱 업데이트](updating-xamarin-forms-apps.md)
- [바인딩 업데이트](update-binding.md)
- [팁 업데이트](updating-tips.md)
- [클래식 vs 통합 API 차이점](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
- [플랫폼 간 앱에서의 네이티브 형식 작업](~/cross-platform/macios/native-types-cross-platform.md)
