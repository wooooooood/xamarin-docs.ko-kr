---
title: "통합된 API"
description: "새 스타일 API 쉽게 그 어느 때 보다 Mac 및 iOS로 있도록 동일한 32와 64 비트 응용 프로그램을 지원 하기 위해 이진 사이 코드를 공유할 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 14311617-1BC2-42CC-AF3F-9F97733EE2D0
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: c31b6e25350089874b5baf868dfe19e12ab4d531
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="unified-api"></a>통합된 API

_새 스타일 API 쉽게 그 어느 때 보다 Mac 및 iOS로 있도록 동일한 32와 64 비트 응용 프로그램을 지원 하기 위해 이진 사이 코드를 공유할 수 있습니다._

IOS 및 Mac 간 공유 코드를 개선 하 고, 32와 64 비트에서 사용할 수 있는 개발자가 단일 코드 베이스를 할 수 있도록 초기 2015 도입 되었습니다 Xamarin.iOS 및 Xamarin.Mac 제품 통합 API 호출에서 새 API를.

> [!IMPORTANT]
> **클래식 프로필 사용 중단:** 클래식 프로필 (monotouch.dll)에서 기능을 사용 하지 않으려는 점진적으로 시작 되며 새 플랫폼 Xamarin.iOS에 추가 됩니다. 예를 들어 비 NRC (새-ref-개수) 옵션이 제거 되었습니다. NRC 모두 통합 된 응용 프로그램을 항상 사용할 수 있는 (즉, 비 NRC 않은 옵션) 및에 알려진된 문제가 없습니다. 이후 릴리스에서 가비지 수집기 Boehm를 사용 하는 옵션이 제거 됩니다. 절대 통합 된 응용 프로그램에 사용할 수 없는 옵션 이기도 합니다. 클래식 지원을 완전히 제거할 Xamarin.iOS 10.0 릴리스와 함께 다음 지원은 예약 됩니다.

## <a name="overviewoverviewmd"></a>[개요](overview.md)

기초가 되는 통합 API에 설명 하 고 자세히 클래식 API 간의 차이점을 설명 합니다. 통합 API에 대 한 변경 내용을 이해 하려면이 페이지를 참조 하십시오.

## <a name="updating-classic-api-based-apps"></a>클래식 API 기반 응용 프로그램 업데이트

플랫폼에 대 한 관련 지침을 따르세요.

- [기존 앱 업데이트](updating-apps.md)
- [기존 iOS 앱 업데이트](updating-ios-apps.md)
- [기존 Mac 앱 업데이트](updating-mac-apps.md)
- [기존 Xamarin.Forms 앱 업데이트](updating-xamarin-forms-apps.md)
- [바인딩을 Unified API로 마이그레이션](update-binding.md)

## <a name="tips-for-updating-code-to-the-unified-apiupdating-tipsmd"></a>[코드를 Unified API로 업데이트하는 팁](updating-tips.md)

마이그레이션하는 응용 프로그램이 무엇 인지에 관계 없이, 체크 아웃 [이러한 팁](updating-tips.md) 통합 API를 성공적으로 업데이트할 수 있습니다.

## <a name="library-split"></a>라이브러리 분할

이 시점부터 Api 두 가지 방법으로 표시 됩니다.

-  **클래식 API:** 32 비트 (전용)으로 제한 된 노출 하 고는 `monotouch.dll` 및 `XamMac.dll` 어셈블리입니다.
-  **통합된 API:** 에서 사용할 수 있는 단일 api 32와 64 비트 개발을 지 원하는 `Xamarin.iOS.dll` 및 `Xamarin.Mac.dll` 어셈블리입니다.

즉, 엔터프라이즈에 대 한 새로운 Api를 계속 사용할 수 있습니다는 기존 클래식 Api 처럼 영원히 사이트 간이나 두 있습니다 유지 유지 됩니다 (하지 대상으로 하는 앱 스토어), 개발자 업그레이드할 수 있습니다.

<a name="namespace-changes" />

## <a name="namespace-changes"></a>Namespace 변경 내용

IOS 및 Mac 제품 간에 코드 공유를 발생 하는 충돌을 줄이기 위해 제품에서 Api에 대 한 네임 스페이스를 변경 하 고 있습니다.

에서는 삭제 하 고 접두사 "MonoTouch" 우리의 iOS 제품과 "MonoMac"에서 데이터 형식에 Mac 제품에서 합니다.

조건부 컴파일에 문의 하지 않고 iOS 및 Mac 플랫폼 간에 코드 공유를 간단 하 게 하 고 소스 코드 파일 맨 위에 있는 노이즈 감소 합니다.

-  **클래식 API:** 네임 스페이스를 사용 하 여 `MonoTouch.` 또는 `MonoMac.` 접두사입니다.
-  **통합된 API:** 네임 스페이스 접두사

## <a name="api-changes"></a>API 변경

통합 API 사용 되지 않는 메서드를 제거 하 고 몇 가지 인스턴스가 있었던 오타 API 이름에 클래식 Api에서 원래 MonoTouch 및 MonoMac 네임 스페이스에 바인딩된 경우입니다. 이러한 인스턴스는 새 통합 Api에서 수정 하 고 구성 요소, iOS 및 Mac 응용 프로그램에서 업데이트 해야 합니다. 충돌할 수 있는 가장 일반적인 구성 목록은 다음과 같습니다.

|클래식 API 메서드 이름|통합 된 API 메서드 이름|
|--- |--- |
|`UINavigationController.PushViewControllerAnimated()`|`UINavigationController.PushViewController()`|
|`UINavigationController.PopViewControllerAnimated()`|`UINavigationController.PopViewController()`|
|`CGContext.SetRGBFillColor()`|`CGContext.SetFillColor()`|
|`NetworkReachability.SetCallback()`|`NetworkReachability.SetNotification()`|
|`CGContext.SetShadowWithColor`|`CGContext.SetShadow`|
|`UIView.StringSize`|`UIKit.UIStringDrawing.StringSize`|

이전의에서 통합 API로 전환할 때 변경 내용 목록은 전체 참조 하십시오 우리의 [클래식 (monotouch.dll) vs (Xamarin.iOS.dll) 통합 API 차이점](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) 설명서입니다.

## <a name="updating-to-unified"></a>업데이트에 통합

이전/분할/사용 되지 않는 API에 몇 가지 **클래식** 에서 사용할 수 없는 **통합** API입니다. 것이 더 쉽게 문제를 해결 하는 `CS0616` 경고 프로그램 (수동 또는 자동화 된)를 시작 하기 전에 업그레이드 해야 합니다. 이후는 `[Obsolete]` 오른쪽 API에 대 한 지침에 메시지 (경고의 일부) 특성입니다.

게시 하는 [ *diff* ](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) 클래식 vs 프로젝트 업데이트 전후의 사용할 수 있는 API의 변경 내용을 통합 합니다. 여전히 수정는 클래식 됩니다 호출을 자주 obsoletes 하 여 (설명서 조회 적은) 시간을 절약할 수 있습니다.

다음이 지침에 따라 [기존 iOS 앱 업데이트](~/cross-platform/macios/unified/updating-ios-apps.md), 또는 [Mac 앱](~/cross-platform/macios/unified/updating-mac-apps.md) 통합 api입니다.
이 페이지의 나머지 부분을 검토 하 고 [이러한 팁](~/cross-platform/macios/unified/updating-tips.md) 코드 마이그레이션하는 방법에 대 한 자세한 내용은 합니다.

### <a name="nuget"></a>NuGet

Xamarin.iOS 클래식 API를 통해 이전에 지원 하 고 있는 NuGet 패키지 게시를 사용 하 여 해당 어셈블리는 **Monotouch10** 플랫폼 모니커입니다.

통합 API 소개-호환 패키지에 대 한 새 플랫폼 식별자 **Xamarin.iOS10**합니다. 기존 NuGet 패키지는 통합 API에 대해 작성 하 여이 플랫폼에 대 한 지원을 추가 하도록 업데이트 해야 합니다.

> [!IMPORTANT]
> 폼에 오류가 있는 경우 _"오류 3 동일한 Xamarin.iOS 프로젝트에 'monotouch.dll'와 'Xamarin.iOS.dll'를 포함할 수 없습니다-'monotouch.dll'에서 참조 하는 동안 'Xamarin.iOS.dll' 명시적으로 참조 되 ' xxx, 버전 = 0.0.000, Culture = neutral, PublicKeyToken = null'"_ 응용 프로그램에 통합 된 Api를 변환한 후는입니다 일반적으로 통합 API에 업데이트 되지 않은 프로젝트에 구성 요소 또는 NuGet 패키지를 포함 합니다. 제거 하려면 기존 구성 요소/NuGet 통합 Api를 지 원하는 버전으로 업데이트 고 깨끗 한 빌드를 수행 해야 합니다.

### <a name="the-road-to-64-bits"></a>64 비트로 Road

지원 32와 64 비트 응용 프로그램 및 프레임 워크에 대 한 정보에 대 한 배경에 대 한 참조는 [32, 64 비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64/index.md)합니다.

 <a name="new-data-types" />

#### <a name="new-data-types"></a>새 데이터 형식

차이의 핵심인 Mac와 iOS Api 사용 하 여 32 비트 및 64 비트 플랫폼에서 64 비트 32 비트 플랫폼에서 항상 되는 아키텍처별 데이터 형식입니다.

예를 들어 Objective-c 매핑하는 `NSInteger` 데이터 형식입니다 `int32_t` 32 비트 시스템 및 `int64_t` 64 비트 시스템에서 합니다.

통합 API에서이 동작에 맞게을 교체 하는 이전을 사용 하는 `int` (.net에서으로 정의 되 고 항상 `System.Int32`)을 새 데이터 형식: `System.nint`합니다.  생각할 수 있습니다 "n" 같은 의미로 "네이티브" 플랫폼의 기본 정수를 입력 합니다.

도입 `nint`, `nuint` 및 `nfloat` 필요한을 기반으로 데이터 형식을 제공 하 여 합니다.

이러한 데이터 형식 변경 하는 방법에 대 한 자세한 내용은 참조는 [네이티브 형식](~/cross-platform/macios/nativetypes.md) 문서.

### <a name="how-to-detect-the-architecture-of-ios-apps"></a>IOS 앱의 아키텍처를 검색 하는 방법

응용 프로그램을 32 비트 또는 64 비트 iOS 시스템에서 실행 되 고 있는지 확인을 해야 하는 경우가 있을 수 있습니다. 아키텍처를 확인 하려면 다음 코드를 사용할 수 있습니다.

```csharp
if (IntPtr.Size == 4) {
    Console.WriteLine ("32-bit App");
} else if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit App");
}
```

<a name="deprecated-apis" />

### <a name="arrays-and-systemcollectionsgeneric"></a>배열 및 System.Collections.Generic

C# 인덱서는 유형의 예상 하기 때문에 `int`를 명시적으로 캐스팅 해야 합니다. `nint` 값을 `int` 컬렉션 또는 배열의 요소에 액세스 하 합니다. 예를 들어:

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

때문에이 동작은 정상적인된 동작에서 캐스트 `int` 를 `nint` 은 64 비트에서 손실 암시적 변환이 수행 되지는 않습니다.

### <a name="converting-datetime-to-nsdate"></a>NSDate 날짜/시간 변환

암시적으로 변환 통합 Api를 사용 하는 경우 `DateTime` 를 `NSDate` 값 더 이상 수행 됩니다. 이러한 값을 다른 한 형식에서 명시적으로 변환 해야 합니다. 이 프로세스를 자동화 하 다음 확장 메서드를 사용할 수 있습니다.

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

내부 Xamarin.iOS 클래식 API (monotouch.dll)는 `[Obsolete]` 특성을 두 가지 방법으로 사용 했습니다.

-  **IOS API를 사용 되지 않음:** 이것이 때 Apple 힌트를 새 스냅숏으로 대체 중인 때문에 API를 사용 하 여 중지할 수 있습니다. 클래식 API는 여전히 세밀 하 게 하 고 종종 필요 (iOS의 이전 버전 지원) 하는 경우.
 이러한 API (및 `[Obsolete]` 특성)를 새 Xamarin.iOS 어셈블리에 포함 됩니다.
-  **잘못 된 API** 일부 API 이름에 오타 했습니다.

호환성을 위해 사용할 수 있는 이전 코드 (monotouch.dll 및 XamMac.dll) 원래 어셈블리에 대 한 보관 하지만 통합 API 어셈블리 (Xamarin.iOS.dll 및 Xamarin.Mac)에서 제거한

<a name="NSObject_ctor" />

### <a name="nsobject-subclasses-ctorintptr"></a>NSObject 하위 클래스.ctor(IntPtr)

모든 `NSObject` 하위 클래스에 허용 하는 생성자는 `IntPtr`합니다. 이 어떻게 기본 ObjC 핸들에서 새 관리 되는 인스턴스를 인스턴스화할 수 있습니다.

이것이 기본에서는 `public` 생성자입니다. 하지만 사용자 코드에서이 기능을 악용할 쉬 웠, 몇 가지를 만드는 예를 들어 관리 되는 인스턴스 단일 ObjC 인스턴스 *또는* 관리 되는 인스턴스를 만드는 (하위 클래스)에 대 한 예상 되는 관리 되는 상태 부족 합니다.

이러한 종류의 문제를 방지 하기 위해는 `IntPtr` 생성자는 이제 `protected` 에 **통합** 서브클래싱에만 사용할 API입니다. 이 방법을 사용 하면 수정/safe API에서 핸들, 즉, 관리 되는 인스턴스를 만드는 데 사용 됩니다.

    var label = Runtime.GetNSObject<UILabel> (handle);

이 API는 (이미 있는 경우) 기존 관리 되는 인스턴스를 반환 합니다 (필수) 하는 경우 새 하나 만들어집니다 또는 합니다. 기본 및 통합 API에서 사용할 수 있는 이미입니다.

`.ctor(NSObjectFlag)` 는 현재 `protected` 하지만이 이와 서브클래싱 외부 거의 사용 되었습니다.

<a name="NSAction" />

### <a name="nsaction-replaced-with-action"></a>작업으로 교체 NSAction

통합 Api와 `NSAction` 표준.NET 위해 제거 되었습니다 `Action`합니다. 때문에 이것이 상당히 개선 `Action` 는 공용.NET 형식, 반면 `NSAction` Xamarin.iOS에 있습니다. 둘 다 같은 방식으로 정확 하 게, distinct와 호환 되지 않는 형식 하 고 하지만 동일한 결과를 얻기 위해 작성 하지 더 많은 코드에서 발생 했습니다.

예를 들어, 다음 코드를 포함 하는 기존 Xamarin 응용 프로그램

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (new NSAction (delegate() {
    ShowDropDownAnimated (tblDataView);
}));
```
이제는 간단한 람다를 교체할 수 있습니다.

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (() => ShowDropDownAnimated(tblDataView));
```

이전에 되는 컴파일러 오류 때문에 프로그램 `Action` 에 할당할 수 없습니다 `NSAction`, 있지만 `UITapGestureRecognizer` 더 길어진는 `Action` 대신는 `NSAction` 통합 Api에서 유효 합니다.

### <a name="custom-delegates-replaced-with-actiont"></a>작업으로 교체 하는 사용자 지정 대리자<T>

**통합** 단순 (예: 하나의 매개 변수)으로.net 대리자 바뀌었는지 `Action<T>`합니다. 예:

    public delegate void NSNotificationHandler (NSNotification notification);

이제 사용할 수는 `Action<NSNotification>`합니다. 이 승격 코드 다시 사용 하 고 Xamarin.iOS와 사용자의 응용 프로그램 내 코드 중복을 줄입니다.

### <a name="taskbool-replaced-with-taskbooleannserror"></a>작업<bool> < 부울, NSError >> 태스크로 대체

**클래식** 몇 가지 비동기 Api 개가 반환 `Task<bool>`합니다. 그러나 일부 그 중 여기서 때 사용 하는 `NSError` 서명 상태의 일부를 즉,이 `bool` 이미 `true` 가져오려는 예외를 catch 했습니다는 `NSError`합니다.

이 패턴에서 변경 된 일부 오류는 흔히 하 고 반환 값은 유용 하지 않아서 **통합** 반환 하는 `Task<Tuple<Boolean,NSError>>`합니다. 이 성공 및 오류는 비동기 호출 중에 발생 한 수를 모두 확인할 수 있습니다.

### <a name="nsstring-vs-string"></a>NSString vs 문자열

일부 상황에서는 일부 상수에서 변경 해야 `string` 를 `NSString`, 예: `UITableViewCell`

**클래식**

    public virtual string ReuseIdentifier { get; }

**Unified**

    public virtual NSString ReuseIdentifier { get; }

.NET 좋습니다 일반적 `System.String` 유형입니다. 그러나 Apple 지침을 불구 하 고 일부 네이티브 API 비교 하는 상수 포인터 (하지 문자열 자체) 하 고으로 상수를 노출 하는 경우에 처리할 수 있는이 `NSString`합니다.

 <a name="protocols" />

### <a name="objective-c-protocols"></a>Objective C 프로토콜

원래 MonoTouch 완벽 하 게 지원 및 일부 ObjC 프로토콜에 대 한 최적화 되지 않은 없는 API는 가장 일반적인 시나리오를 지원 하기 위해 추가 되었습니다. 이 제한은 더 이상 존재 하지 않는 있지만 여러 Api 유지는 이전 버전과 호환성에 대 한 내부 `monotouch.dll` 및 `XamMac.dll`합니다.

이러한 제한 사항이 제거 되 고 통합 Api에 대해 정리 합니다. 대부분의 변경 내용은 다음과 같이 표시 됩니다.

**클래식**

    public virtual AVAssetResourceLoaderDelegate Delegate { get; }

**Unified**

    public virtual IAVAssetResourceLoaderDelegate Delegate { get; }

`I` 의미 접두사 **통합** ObjC 프로토콜에 대 한 특정 형식이 아닌 인터페이스를 노출 합니다. 이것을 위치 하지 않으려는 경우 하위 클래스 Xamarin.iOS 제공 되는 특정 형식의 경우 간편 하 게 합니다.

또한 일부 API를 보다 정확 하 게 하 고 사용 하기 쉬운 예를 들어 허용:

**클래식**

    public virtual void SelectionDidChange (NSObject uiTextInput);

**Unified**

    public virtual void SelectionDidChange (IUITextInput uiTextInput);

이제 이러한 API 설명서를 참조 하지 않고 우리 관리가 더 용이 하며 IDE 코드 완성 프로토콜/인터페이스에 따라 더 유용한 제안 사항과 함께 제공 합니다.

#### <a name="nscoding-protocol"></a>NSCoding 프로토콜

지원 하지 않으므로 하는 경우에 모든 형식-.ctor(NSCoder) 포함 하는 원래 바인딩에 `NSCoding` 프로토콜입니다.  단일 `Encode(NSCoder)` 메서드에 있었던는 `NSObject` 인코딩할 개체입니다.
하지만이 메서드는 인스턴스 NSCoding 프로토콜에 일치 하는 경우에 작동 합니다.

통합 API에서이 해결 했습니다.  새 어셈블리를 하나만 됩니다는 `.ctor(NSCoder)` 형식을 준수 하는 경우 `NSCoding`합니다. 이제 이러한 형식 레이블에도 `Encode(NSCoder)` 준수 하는 메서드는 `INSCoding` 인터페이스입니다.

낮은 미치는 영향: 대부분의 경우에서 이러한 변경 내용이 영향을 주지 응용 프로그램으로 오래 되 고 제거 생성자를 사용할 수 없습니다.

## <a name="further-tips"></a>추가 정보

알아야 할 추가 변경에 나열 된는 [통합 API에 앱을 업데이트 하기 위한 팁](~/cross-platform/macios/unified/updating-tips.md)합니다.

## <a name="sample-code"></a>샘플 코드

년 7 월 31 일을 기준으로이 새로운 API iOS 샘플의 포트에 게시 한 우리는 `magic-types` 에서도 분기 [monotouch 샘플](https://github.com/xamarin/monotouch-samples/commits/magic-types)합니다.

Mac 용 모두에서 샘플을 검사할 우리는 [mac 샘플](https://github.com/xamarin/mac-samples) 리포지토리 (Mavericks/Yosemite에 새로운 Api 표시 됨) 뿐만 아니라 형식 매직 분기에서 32/64 비트 샘플 [mac 샘플](https://github.com/xamarin/monotouch-samples/commits/magic-types)합니다.
-->

## <a name="related-links"></a>관련 링크

- [IOS 앱을 업데이트합니다.](updating-ios-apps.md)
- [Mac 앱 업데이트](updating-mac-apps.md)
- [Xamarin.Forms 앱 업데이트](updating-xamarin-forms-apps.md)
- [바인딩 업데이트](update-binding.md)
- [팁 업데이트](updating-tips.md)
- [클래식 vs 통합 API 차이점](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
