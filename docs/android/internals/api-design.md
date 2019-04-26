---
title: Xamarin.Android API Design Principles
ms.prod: xamarin
ms.assetid: 3E52D815-D95D-5510-0D8F-77DAC7E62EDE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: e762a286069d5ef1db90f3c45808eee0a7a04a7f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60954287"
---
# <a name="xamarinandroid-api-design-principles"></a>Xamarin.Android API Design Principles


## <a name="overview"></a>개요

가장 핵심적인 Mono의 일부인 기본 클래스 라이브러리 외에도 Xamarin.Android 개발자 Mono를 사용 하 여 네이티브 Android 응용 프로그램을 만들 수 있도록 다양 한 Android Api에 대 한 바인딩을 제공 합니다.

Xamarin.Android의 핵심 있습니다 interop 엔진 Java 대상과 브리지 C# 세계 이며 다른.NET 언어 또는 C#에서 Java Api에 대 한 액세스를 사용 하 여 개발자에 게 제공 합니다.


## <a name="design-principles"></a>디자인 원칙

이 Xamarin.Android 바인딩에 대 한 디자인 원칙은 중 일부

-  따라야 합니다 [.NET Framework 디자인 지침](https://docs.microsoft.com/dotnet/standard/design-guidelines/)합니다.

-  서브 클래스 Java 클래스를 수 있습니다.

-  서브 클래스는 C# 표준 구문을 사용 하 여 작동 해야 합니다.

-  기존 클래스에서 파생 됩니다.

-  체인에 기본 생성자를 호출 합니다.

-  C#의 재정의 시스템을 사용 하 여 메서드를 재정의 해야 합니다.

-  쉬우며 일반적인 Java 작업 및 하드 Java 작업 가능 하 게 합니다.

-  C# 속성으로 JavaBean 속성을 노출 합니다.

-  강력한 형식의 API를 노출 합니다.

    - 형식 안전성을 늘립니다.

    - 런타임 오류를 최소화 합니다.

    - 반환 형식은 IDE intellisense를 가져옵니다.

    - IDE 팝업 설명서에 대 한 허용합니다.

-  IDE에서 탐색을 api는 것이 좋습니다.

    - Java Classlib 최소화 노출에 대 한 프레임 워크 대안을 활용 합니다.

    - 적절 하 고 해당 하는 경우에 단일 메서드 인터페이스 대신 C# 대리자 (람다, 무명 메서드 및 System.Delegate)를 노출 합니다.

    - 임의의 Java 라이브러리를 호출 하는 메커니즘이 제공 ( [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)).


## <a name="assemblies"></a>어셈블리

Xamarin.Android를 구성 하는 어셈블리의 여러 포함 된 *MonoMobile 프로필*합니다. 합니다 [어셈블리](~/cross-platform/internals/available-assemblies.md) 페이지에 자세한 정보가 있습니다.

에 포함 된 Android 플랫폼에 대 한 바인딩을 `Mono.Android.dll` 어셈블리입니다. 사용 중인 Android Api에 대 한 전체 바인딩와 통신 하는 Android 런타임 VM 사용 하 여이 어셈블리에 포함 되어 있습니다.


## <a name="binding-design"></a>디자인 바인딩


### <a name="collections"></a>컬렉션

Android Api 목록, 집합 및 지도 제공 하는 광범위 하 게 java.util 컬렉션을 활용 합니다. 사용 하 여 이러한 요소를 위해 노출 합니다 [System.Collections.Generic](xref:System.Collections.Generic) 바인딩의 인터페이스입니다. 기본 매핑은 다음과 같습니다.

-   [java.util.Set<E> ](https://developer.android.com/reference/java/util/Set.html) 시스템 형식에 매핑됩니다 [ICollection<T>](xref:System.Collections.Generic.ICollection`1), 도우미 클래스 [Android.Runtime.JavaSet<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaSet%601/)합니다.

-   [java.util.List<E> ](https://developer.android.com/reference/java/util/List.html) 시스템 형식에 매핑됩니다 [IList<T>](xref:System.Collections.Generic.IList`1), 도우미 클래스 [Android.Runtime.JavaList<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaList%601/)합니다.

-   [< K, V > java.util.Map](https://developer.android.com/reference/java/util/Map.html) 시스템 형식에 매핑됩니다 [< TKey, TValue > IDictionary](xref:System.Collections.Generic.IDictionary`2), 도우미 클래스 [Android.Runtime.JavaDictionary < K, V >](https://developer.xamarin.com/api/type/Android.Runtime.JavaDictionary%602/)합니다.

-   [java.util.Collection<E> ](https://developer.android.com/reference/java/util/Collection.html) 시스템 형식에 매핑됩니다 [ICollection<T>](xref:System.Collections.Generic.ICollection`1), 도우미 클래스 [Android.Runtime.JavaCollection<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaCollection%601/)합니다.

이러한 형식의 마샬링 copyless 빠르게를 용이 하 게 하기 위한 도우미 클래스를 준비 했습니다. 가능한 경우 제공 된 프레임 워크 구현 대신 컬렉션을 제공 하는이 사용 하는 것이 좋습니다와 같은 [ `List<T>` ](xref:System.Collections.Generic.List`1) 하거나 [ `Dictionary<TKey, TValue>` ](xref:System.Collections.Generic.Dictionary`2)합니다. 합니다 [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/) 구현을 네이티브 Java 컬렉션을 내부적으로 사용 되며 따라서 필요 하지 않습니다는 Android API 멤버에 전달 하는 경우 기본 컬렉션에서 복사 합니다.

인터페이스 구현을 해당 인터페이스를 수락 하는 Android 메서드에 전달, 예: 전달 수를 `List<int>` 에 [ArrayAdapter&lt;int&gt;(컨텍스트, int, IList&lt;int&gt;)](https://developer.xamarin.com/api/constructor/Android.Widget.ArrayAdapter%3CT%3E.ArrayAdapter%3CT%3E/p/Android.Content.Context/System.Int32/System.Collections.Generic.IList%7BT%7D/)생성자입니다. *그러나*, 모든 구현에 대 한 *제외 하 고* Android.Runtime 구현은이 포함 됩니다 *복사* 는 Android 런타임 VM에 Mono VM 목록입니다. 목록 뒷부분에 나오는 경우 Android 런타임 내에서 변경 (예: 호출 하 여는 [ArrayAdapter&lt;T&gt;합니다. Add(T)](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter%3CT%3E.Add/p/T/) 메서드), 해당 변경 내용을 *것입니다* 관리 코드에 표시 됩니다. 경우는 `JavaList<int>` 된 사용 내용이 표시 됩니다.

조항을, 컬렉션 인터페이스는 구현 *되지* 나열 된 위의 하나 **도우미 클래스**es [In]만 마샬링할:

```csharp
// This fails:
var badSource  = new List<int> { 1, 2, 3 };
var badAdapter = new ArrayAdapter<int>(context, textViewResourceId, badSource);
badAdapter.Add (4);
if (badSource.Count != 4) // true
    throw new InvalidOperationException ("this is thrown");

// this works:
var goodSource  = new JavaList<int> { 1, 2, 3 };
var goodAdapter = new ArrayAdapter<int> (context, textViewResourceId, goodSource);
goodAdapter.Add (4);
if (goodSource.Count != 4) // false
    throw new InvalidOperationException ("should not be reached.");
```


### <a name="properties"></a>속성

Java 메서드를 속성으로 해당 하는 경우 변환 됩니다.

-  Java 메서드 쌍 `T getFoo()` 하 고 `void setFoo(T)` 으로 변환 되는 `Foo` 속성입니다. 예제: [Activity.Intent](https://developer.xamarin.com/api/property/Android.App.Activity.Intent/)합니다.

-  다음 Java 메서드의 경우 `getFoo()` 읽기 전용 Foo 속성으로 변환 됩니다. 예제: [Context.PackageName](https://developer.xamarin.com/api/property/Android.Content.Context.PackageName/).

-  집합 전용 속성이 생성 되지 않습니다.

-  속성은 *되지* 속성 형식이 배열 되는 경우 생성 합니다.



### <a name="events-and-listeners"></a>수신기 및 이벤트

Java를 기반으로 하는 Android Api 및 해당 구성 요소 이벤트 수신기를 연결 하기 위한 Java 패턴을 따릅니다. 이 패턴에서 사용자가 익명 클래스를 만들고를 재정의할 메서드를 선언할 필요 하므로 작업은 복잡할 수 경향이, 예를 들어,이 Java 사용 하 여 Android에서 작업은 수행 하는 방법을:

```csharp
final android.widget.Button button = new android.widget.Button(context);

button.setText(this.count + " clicks!");
button.setOnClickListener (new View.OnClickListener() {
    public void onClick (View v) {
        button.setText(++this.count + " clicks!");
    }
});
```

이벤트를 사용 하 여 C#에서 해당 하는 코드는 다음과 같습니다.

```csharp
var button = new Android.Widget.Button (context) {
    Text = string.Format ("{0} clicks!", this.count),
};
button.Click += (sender, e) => {
    button.Text = string.Format ("{0} clicks!", ++this.count);
};
```

Xamarin.Android와 함께 사용할 수 있는 위의 메커니즘 중 두 참고 합니다. 수신기 인터페이스를 구현 하 고 View.SetOnClickListener를 사용 하 여 연결 하거나 클릭 이벤트에 일반적인 C# 패러다임 중 하나를 통해 만든 대리자를 연결할 수 있습니다.

API 요소를 기반으로 만들겠습니다. 수신기 콜백 메서드는 void 반환에 있는 경우는 [EventHandler&lt;TEventArgs&gt; ](xref:System.EventHandler`1) 위임 합니다. 이러한 수신기 형식에 대 한 위의 예제와 같이 이벤트를 생성합니다. 그러나 수신기 콜백을 반환 void가 아닌 및 비- **부울** 값, 이벤트 및 EventHandlers 사용 되지 않습니다. 대신 콜백의 서명에 대 한 특정 대리자를 생성 하 고 이벤트 대신 속성을 추가 합니다. 이유는 대리자 호출 순서를 사용 하 여 처리 하 고 처리를 반환 하는 것입니다. 이 방법은 수행 되는 Xamarin.iOS API를 사용 하 여 반영 됩니다.

C# 이벤트 또는 속성 자동으로 생성 하는 경우 Android 이벤트 등록 메서드:

1. 에 `set` 접두사, 예를 들어 [ *설정*OnClickListener](https://developer.xamarin.com/api/member/Android.Views.View.SetOnClickListener/)합니다.

1. 에 `void` 형식을 반환 합니다.

1. 하나의 매개 변수를 수락 매개 변수 형식은 인터페이스, 인터페이스에 하나만 메서드 및 인터페이스 이름이 끝나는 `Listener` , 예를 들어 [View.OnClick *수신기*](https://developer.xamarin.com/api/type/Android.Views.View+IOnClickListener/)합니다.


또한 수신기 인터페이스 메서드 경우 형식이 반환 **부울** 대신 **void**에 생성 된 *EventArgs* 서브 클래스를 포함됩니다*처리* 속성입니다. 값을 *처리* 속성은 반환 값으로 사용 합니다 *수신기* 메서드와 해당 기본값은 `true`합니다.

예를 들어, Android [View.setOnKeyListener()](https://developer.xamarin.com/api/member/Android.Views.View.SetOnKeyListener/p/Android.Views.View+IOnKeyListener/) 메서드에서 [View.OnKeyListener](https://developer.xamarin.com/api/type/Android.Views.View+IOnKeyListener) 인터페이스와 [View.OnKeyListener.onKey (보기, int, KeyEvent)](https://developer.xamarin.com/api/member/Android.Views.View+IOnKeyListener.OnKey/p/Android.Views.View/Android.Views.Keycode/Android.Views.KeyEvent/) 메서드는 부울 반환 형식이 있습니다. Xamarin.Android 해당 생성 [View.KeyPress](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) 이벤트에는 [EventHandler&lt;View.KeyEventArgs&gt;](https://developer.xamarin.com/api/type/Android.Views.View+KeyEventArgs/)합니다.
*KeyEventArgs* 클래스에는 [View.KeyEventArgs.Handled](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) 속성에 대 한 반환 값으로 사용 되는 합니다 *View.OnKeyListener.onKey()* 메서드.

다른 메서드 및 대리자 기반 연결을 노출 하는 생성자 오버 로드를 추가 하려고 합니다. 또한 수신기가 여러 콜백이 있는 경우 개별 콜백 구현 이므로, 변환 하는 것이 발견 되 결정할 몇 가지 추가 검사가 필요 합니다. 해당 이벤트가 없습니다 있으면 수신기 C#에서 사용할 되지만 하세요 존경 대리자 사용을 가질 수를 생각 하는 합니다. 또한 대리자 대신에서 도움이 되는 선택 취소 했을 때 "수신기" 접미사 없이 인터페이스의 일부 변환 완료 된 합니다.

모든 수신기 인터페이스를 구현 합니다 [`Android.Runtime.IJavaObject`](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/)
인터페이스는 수신기 클래스는이 인터페이스를 구현 해야 하므로 바인딩 구현 세부 사항 때문입니다. 수신기 인터페이스의 서브 클래스에서 구현 하 여 이렇게 [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) 다른 래핑된 Android 활동 등의 Java 개체입니다.


### <a name="runnables"></a>Runnables

Java를 활용 합니다 [java.lang.Runnable](https://developer.xamarin.com/api/type/Java.Lang.Runnable/) 위임 메커니즘을 제공 하는 인터페이스입니다. 합니다 [java.lang.Thread](https://developer.xamarin.com/api/type/Java.Lang.Thread/) 클래스는이 인터페이스의 주요 소비자입니다. Android 에서도 API의 인터페이스를 채택 했습니다.
[Activity.runOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/p/Java.Lang.IRunnable/) 하 고 [View.post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/p/Java.Lang.IRunnable) 은 중요 한 예입니다.

합니다 `Runnable` 단일 void 메서드를 포함 하는 인터페이스 [run ()](https://developer.xamarin.com/api/member/Java.Lang.Runnable.Run%28%29/)합니다. 따라서 인계 서 C#에서 바인딩을 [System.Action](xref:System.Action) 위임 합니다. 바인딩에서 허용 하는 오버 로드를 준비 했습니다를 `Action` 을 사용 하는 모든 API 멤버에 대 한 매개 변수를 `Runnable` 네이티브 API의 예를 들어 [Activity.RunOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)) 및 [View.Post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/(System.Action)).

남겨둔 합니다 [irunnable이](https://developer.xamarin.com/api/type/Java.Lang.IRunnable/) 유형인 인터페이스를 구현 하 고 따라서 수 있으므로를 대체 하는 대신 현재 위치에서 오버 로드 runnables로 직접 전달 합니다.


### <a name="inner-classes"></a>내부 클래스

Java에 두 가지 유형의 [중첩 클래스](http://download.oracle.com/javase/tutorial/java/javaOO/nested.html): 정적 클래스 및 비정적 클래스를 중첩 합니다.

Java의 정적 중첩된 클래스를 C#의 중첩 형식 동일합니다.

비정적 중첩 클래스 라고도 *내부 클래스가*를 크게 다릅니다. 바깥쪽 형식 인스턴스에 대 한 암시적 참조가 포함 하며 (이 개요의 범위를 벗어나는 다른 차이점) 간에 정적 멤버를 포함할 수 없습니다.

바인딩 및 C# 사용 하더라도 정적 중첩된 클래스는 중첩된의 일반 형식으로 처리 됩니다. 내부 클래스는 한편, 두 가지 중요 한 차이가 있습니다.

1. 생성자 매개 변수로 포함 하는 형식에 대 한 암시적 참조를 명시적으로 제공 되어야 합니다.

1. 내부 클래스는 내부 클래스에서 상속 하는 경우 *해야* 형식 내에 중첩 될 기본 내부 클래스의 포함 하는 형식에서 상속 되는 및 파생된 된 형식 생성자를 제공 해야 C#으로 동일한 유형의 형식을 포함 하 합니다.


예를 들어 합니다 [Android.Service.Wallpaper.WallpaperService.Engine](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) 내부 클래스입니다. 내부 클래스 이므로 합니다 [WallpaperService.Engine() 생성자](https://developer.xamarin.com/api/constructor/Android.Service.Wallpaper.WallpaperService+Engine.Engine/p/Android.Service.Wallpaper.WallpaperService/) 에 대 한 참조를 [WallpaperService](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService/) 인스턴스 (비교 및 대조 java [WallpaperService.Engine ( ) 생성자](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) 매개 변수를 사용 하는).

내부 클래스는 예제에서는 파생 CubeWallpaper.CubeEngine 같습니다.

```csharp
class CubeWallpaper : WallpaperService {
        public override WallpaperService.Engine OnCreateEngine ()
        {
                return new CubeEngine (this);
        }

        class CubeEngine : WallpaperService.Engine {
                public CubeEngine (CubeWallpaper s)
                        : base (s)
                {
                }
        }
}
```

참고 어떻게 `CubeWallpaper.CubeEngine` 내에 중첩 `CubeWallpaper`를 `CubeWallpaper` 의 포함 하는 클래스에서 상속 `WallpaperService.Engine`, 및 `CubeWallpaper.CubeEngine` 선언 형식-를 사용 하는 생성자가 `CubeWallpaper` 위에서 지정한으로이 경우.


### <a name="interfaces"></a>인터페이스

Java 인터페이스에는 세 가지 C#에서 문제를 일으킬 수 있는 두 개의 멤버를 포함할 수 있습니다.

1. 메서드

1. 유형

1. 필드


Java 인터페이스는 두 가지 유형으로 변환 됩니다.

1. 메서드 선언을 포함 (선택 사항) 인터페이스입니다. Java 인터페이스와이 인터페이스의 이름이 같은 *제외한* 있습니다를 ' *있습니까* ' 접두사.

1. 모든 필드를 포함 하는 (선택 사항) 정적 클래스는 Java 인터페이스 내에서 선언 합니다.

중첩된 형식 "에 재배치 되므로" 접두사로 바깥쪽 인터페이스 이름의 중첩된 형식 대신 바깥쪽 인터페이스의 형제입니다.

예를 들어 합니다 [android.os.Parcelable](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) 인터페이스입니다.
합니다 *Parcelable* 인터페이스 메서드 및 중첩된 형식 상수를 포함 합니다. 합니다 *Parcelable* 인터페이스 메서드는 배치 합니다 [Android.OS.IParcelable](https://developer.xamarin.com/api/type/Android.OS.IParcelable/) 인터페이스입니다.
합니다 *Parcelable* 인터페이스 상수에 배치 되는 [Android.OS.ParcelableConsts](https://developer.xamarin.com/api/type/Android.OS.ParcelableConsts/) 형식입니다. 중첩 [android.os.Parcelable.ClassLoaderCreator <t> </t> ](https://developer.android.com/reference/android/os/Parcelable.ClassLoaderCreator.html) 하 고 [android.os.Parcelable.Creator <t> </t> ](https://developer.android.com/reference/android/os/Parcelable.Creator.html) 유형은 현재 없습니다 제네릭 지원과;의 제한으로 인해 바인딩된 으로 표시 됩니다, 지원 된 경우에 *Android.OS.IParcelableClassLoaderCreator* 하 고 *Android.OS.IParcelableCreator* 인터페이스입니다. 예를 들어 중첩 [android.os.IBinder.DeathRecpient](https://developer.android.com/reference/android/os/IBinder.DeathRecipient.html) 인터페이스로 바인딩되어 합니다 [Android.OS.IBinderDeathRecipient](https://developer.xamarin.com/api/type/Android.OS.IBinderDeathRecipient/) 인터페이스입니다.


> [!NOTE]
> Java 인터페이스 상수는 Xamarin.Android 1.9부터 <em>중복</em> Java 포팅 간소화 하기 위해에서 코드입니다. 사용 하는 이식 Java 코드를 개선 하기 위해 이렇게 [android 공급자](https://developer.android.com/reference/android/provider/package-summary.html) 상수 인터페이스입니다.

위의 형식 외에 4 개의 추가 변경 내용이:

1. Java 인터페이스와 동일한 이름 가진 형식 상수를 포함 하도록 생성 됩니다.

1. 또한 인터페이스 상수를 포함 하는 형식이 구현된 하는 Java 인터페이스에서 제공 되는 모든 상수를 포함 합니다.

1. 상수를 포함 하는 Java 인터페이스를 구현 하는 모든 클래스에는 모든 구현 된 인터페이스의 상수를 포함 하는 새 중첩된 InterfaceConsts 형식을 가져옵니다.

1. 합니다 *비용* 형식 이제 사용 되지 않습니다.


에 대 한 합니다 *android.os.Parcelable* 인터페이스, 즉, 이제 것을 [ *Android.OS.Parcelable* ](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) 상수를 포함 하는 형식입니다. 예를 들어 합니다 [Parcelable.CONTENTS_FILE_DESCRIPTOR](https://developer.android.com/reference/android/os/Parcelable.html#CONTENTS_FILE_DESCRIPTOR) 상수로 바인딩될 합니다 [ *Parcelable.ContentsFileDescriptor* ](https://developer.xamarin.com/api/field/Android.OS.Parcelable.ContentsFileDescriptor/) 상수를 대신 합니다  *ParcelableConsts.ContentsFileDescriptor* 상수입니다.

포함 된 다른 인터페이스를 구현 하는 상수 아직 자세한 상수를 포함 하는 인터페이스에 대 한 모든 상수 합한 이제 생성 됩니다. 예를 들어 합니다 [android.provider.MediaStore.Video.VideoColumns](https://developer.android.com/reference/android/provider/MediaStore.Video.VideoColumns.html) 구현 인터페이스는 [android.provider.MediaStore.MediaColumns](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumns/) 인터페이스입니다. 그러나 1.9를 이전 합니다 [Android.Provider.MediaStore.Video.VideoColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+Video+VideoColumnsConsts/) 형식에 대해 선언 된 상수에 액세스 하는 방법은 없습니다 [Android.Provider.MediaStore.MediaColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumnsConsts/)합니다.
결과적으로 Java 표현식 *MediaStore.Video.VideoColumns.TITLE* C# 식에 바인딩해야 *MediaStore.Video.MediaColumnsConsts.Title* 읽지도 않고 검색 하기 어려운는 Java 설명서 다양 합니다. 1.9에 해당 하는 C# 식 됩니다 [ *MediaStore.Video.VideoColumns.Title*](https://developer.xamarin.com/api/field/Android.Provider.MediaStore+Video+VideoColumns.Title/)합니다.

또한 고려해 야 합니다 [android.os.Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/) Java를 구현 하는 형식 *Parcelable* 인터페이스입니다. 예를 들어 해당 인터페이스의 모든 상수는 "통해" 번들 형식에 액세스할 수 있는 인터페이스를 구현 하기 때문 *Bundle.CONTENTS_FILE_DESCRIPTOR* 완벽 하 게 유효한 Java 식입니다.
이전에이 식을 포트를 C# 연결 된 모든 인터페이스는 형식에서 참조 하기 위해 구현 되는 확인 해야 합니다 *CONTENTS_FILE_DESCRIPTOR* 에서 제공 합니다. Xamarin.Android 1.9부터 상수를 포함 하는 Java 인터페이스를 구현 하는 클래스 해야 중첩 *InterfaceConsts* 모든 상속 된 인터페이스 상수를 포함 하는 형식입니다. 그러면 변환 *Bundle.CONTENTS_FILE_DESCRIPTOR* 하 [ *Bundle.InterfaceConsts.ContentsFileDescriptor*](https://developer.xamarin.com/api/field/Android.OS.Bundle+InterfaceConsts.ContentsFileDescriptor/)합니다.

마지막으로 사용 하 여 형식를 *비용* 접미사와 같은 *Android.OS.ParcelableConsts* 사용 되지 않음, 새로 도입된 된 InterfaceConsts 이외의 중첩 형식은 이제 됩니다. Xamarin.Android 3.0에서 제거 됩니다.


## <a name="resources"></a>자료

이미지, 레이아웃 설명, 이진 blob 및 문자열 사전으로 응용 프로그램에 포함할 수 [리소스 파일](https://developer.android.com/guide/topics/resources/providing-resources.html)합니다.
다양 한 Android Api는 데 사용할 [리소스 Id 작업할](https://developer.android.com/guide/topics/resources/accessing-resources.html) 이미지로 처리 하는 대신 문자열이 나 이진 blob 직접.

예를 들어 샘플 Android 앱을 사용자 인터페이스 레이아웃을 포함 하는 ( `main.axml`)은 국제화 테이블 문자열 ( `strings.xml`) 및 일부 아이콘 ( `drawable-*/icon.png`) 응용 프로그램의 "리소스" 디렉터리에 해당 리소스를 보관할 때:

    Resources/
        drawable-hdpi/
            icon.png

        drawable-ldpi/
            icon.png

        drawable-mdpi/
            icon.png

        layout/
            main.axml

        values/
            strings.xml

네이티브 Android Api 파일 이름으로 직접 작동 하지 않습니다 하지만 대신 리소스 Id에서 작동 합니다. 빌드 시스템 배포에 대 한 리소스 패키지 되며 라는 클래스를 생성 합니다. 리소스를 사용 하는 Android 응용 프로그램을 컴파일할 때 `Resource` 각각 포함 된 리소스에 대 한 토큰을 포함 하는 합니다. 예를 들어 위의 리소스 레이아웃의 경우 다음과 같습니다. 해당 R 클래스에 노출 되는 내용

```csharp
public class Resource {
    public class Drawable {
        public const int icon = 0x123;
    }

    public class Layout {
        public const int main = 0x456;
    }

    public class String {
        public const int first_string = 0xabc;
        public const int second_string = 0xbcd;
    }
}
```

다음 사용 `Resource.Drawable.icon` 참조를 `drawable/icon.png` 파일 또는 `Resource.Layout.main` 참조를 `layout/main.xml` 파일 또는 `Resource.String.first_string` 사전 파일의 첫 번째 문자열을 참조 하도록 `values/strings.xml`.


## <a name="constants-and-enumerations"></a>상수 및 열거형

네이티브 Android Api에 수행 하거나 int의 의미를 확인 하는 상수 필드에 매핑해야 하는 int를 반환 하는 여러 메서드가 있습니다. 이러한 메서드를 사용 하려면 사용자가 이상적인 되는 상수는 적절 한 값을 확인 하려면 설명서를 참조 해야 합니다.

예를 들어 [Activity.requestWindowFeature (int featureID)](https://developer.android.com/reference/android/app/Activity.html#requestWindowFeature(int))합니다.

이러한 경우.NET 열거형에 관련된 상수를 함께 그룹화 하 여 열거형을 대신 수행할 메서드를 다시 매핑할 다해야 합니다.
이 작업을 수행 하면 IntelliSense 다양 한 잠재적인 값을 제공할 수 있습니다.

위의 예제는 다음과 같이 계산 됩니다. [(WindowFeatures featureId) Activity.RequestWindowFeature](https://developer.xamarin.com/api/member/Android.App.Activity.RequestWindowFeature/p/Android.Views.WindowFeatures/)합니다.

이 상수는 서로 파악 하는 매우 수동 프로세스가 하 고 어떤 Api 이러한 상수를 사용 합니다. 열거형으로 표현 된 더 잘 될 API에서 사용 되는 모든 상수에 대 한 버그를 제출 하세요.
