---
title: "API 디자인"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3E52D815-D95D-5510-0D8F-77DAC7E62EDE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 23aa944b88fe3e743b6b29810c29d1843f2efc29
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="api-design"></a>API 디자인


## <a name="overview"></a>개요

핵심 모노의 일부인 기본 클래스 라이브러리 뿐만 아니라 Xamarin.Android 모노를 사용 하 여 네이티브 Android 응용 프로그램을 만들려면 통해 개발자는 다양 한 Android Api에 대 한 바인딩을 포함 되어 있습니다.

Xamarin.Android의 핵심 있습니다 interop 엔진에서 Java와 C# 브리지 당시 이며 C# 또는 다른.NET 언어에서 Java Api에 액세스할 수 있는 개발자에 게 제공 합니다.


## <a name="design-principles"></a>디자인 원칙

몇 가지 Xamarin.Android 바인딩에 대 한 우리의 디자인 원칙

-  준수 하는 [.NET Framework 디자인 지침](http://msdn.microsoft.com/en-us/library/ms229042.aspx)합니다.

-  하위 클래스 Java 클래스를 수 있습니다.

-  하위 클래스 C# 표준 구문을 함께 사용할 수 있어야 합니다.

-  기존 클래스에서 파생 됩니다.

-  체인에 기본 생성자를 호출 합니다.

-  메서드를 재정의 하는 C#의 재정의 시스템으로 수행 되어야 합니다.

-  일반적인 Java 쉽게 작업과 하드 Java 작업이 가능 하 게 합니다.

-  C# 속성으로 JavaBean 속성을 노출 합니다.

-  강력한 형식의 API를 노출 합니다.

    - 형식 안전성을 늘립니다.

    - 런타임 오류를 최소화 합니다.

    - 반환 형식에 IDE intellisense를 얻습니다.

    - IDE 팝업 설명서에 대 한 허용 합니다.

-  Api의 IDE에서 탐색 것을 권장 합니다.

    - Java Classlib 최소화 노출에 대 한 프레임 워크 대안을 사용 합니다.

    - 적절 한 및 적용 가능한 경우 단일 메서드 인터페이스 대신 C# 대리자 (람다 식, 무명 메서드 및 System.Delegate)를 노출 합니다.

    - 임의의 Java 라이브러리를 호출할 수 있는 메커니즘 제공 ( [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/)).



## <a name="assemblies"></a>어셈블리

Xamarin.Android 다양 한 구성 하는 어셈블리를 포함 된 *MonoMobile 프로필*합니다. [어셈블리](~/cross-platform/internals/available-assemblies.md) 페이지에 자세한 정보.

Android 플랫폼에 대 한 바인딩을에 포함 되어는 `Mono.Android.dll` 어셈블리입니다. 사용 중인 Android Api에 대 한 전체 바인딩 Android 런타임 VM과 통신 하 고이 어셈블리에 포함 되어 있습니다.


## <a name="binding-design"></a>바인딩 디자인


### <a name="collections"></a>컬렉션

Android Api 목록, 집합 및 지도 제공 하는 광범위 하 게 java.util 컬렉션을 사용 합니다. 사용 하 여 이러한 요소를 노출는 [System.Collections.Generic](https://developer.xamarin.com/api/namespace/System.Collections.Generic/) 바인딩에의 인터페이스입니다. 기본 매핑은:

-   [java.util.Set<E> ](http://developer.android.com/reference/java/util/Set.html) 시스템 형식에 매핑됩니다 [ICollection<T>](http://msdn.microsoft.com/en-us/library/92t2ye13.aspx), 도우미 클래스 [Android.Runtime.JavaSet<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaSet%601/)합니다.

-   [java.util.List<E> ](http://developer.android.com/reference/java/util/List.html) 시스템 형식에 매핑됩니다 [IList<T>](http://msdn.microsoft.com/en-us/library/5y536ey6.aspx), 도우미 클래스 [Android.Runtime.JavaList<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaList%601/)합니다.

-   [< K, V > java.util.Map](http://developer.android.com/reference/java/util/Map.html) 시스템 형식에 매핑됩니다 [< TKey, TValue > IDictionary](http://msdn.microsoft.com/en-us/library/s4ys34ea.aspx), 도우미 클래스 [Android.Runtime.JavaDictionary < K, V >](https://developer.xamarin.com/api/type/Android.Runtime.JavaDictionary%602/)합니다.

-   [java.util.Collection<E> ](http://developer.android.com/reference/java/util/Collection.html) 시스템 형식에 매핑됩니다 [ICollection<T>](http://msdn.microsoft.com/en-us/library/92t2ye13.aspx), 도우미 클래스 [Android.Runtime.JavaCollection<T>](https://developer.xamarin.com/api/type/Android.Runtime.JavaCollection%601/)합니다.

이러한 형식의 마샬링은 copyless 더 빠르게 용이 하 게 하려면 도우미 클래스를 제공 했습니다. 가능 하면 이러한 제공 제공 프레임 워크 구현 대신 컬렉션을 사용 하는 것이 좋습니다, 같은 [ `List<T>` ](https://developer.xamarin.com/api/type/System.Collections.Generic.List%601/) 또는 [ `Dictionary<TKey, TValue>` ](https://developer.xamarin.com/api/type/System.Collections.Generic.Dictionary%602/)합니다. [Android.Runtime](https://developer.xamarin.com/api/namespace/Android.Runtime/) 구현은 네이티브 Java 컬렉션을 내부적으로 사용 하 고 따라서 필요 하지 않습니다는 Android API 멤버에 전달 하는 경우 네이티브 컬렉션에서 복사 합니다.

예: 전달, 인터페이스 구현을 해당 인터페이스를 수락 하는 Android 메서드에 전달할 수 있습니다는 `List<int>` 에 [ArrayAdapter&lt;int&gt;(컨텍스트, int, IList&lt;int&gt;)](https://developer.xamarin.com/api/constructor/Android.Widget.ArrayAdapter%3CT%3E.ArrayAdapter%3CT%3E/p/Android.Content.Context/System.Int32/System.Collections.Generic.IList%7BT%7D/)생성자입니다. *그러나*, 모든 구현에 대 한 *제외 하 고* Android.Runtime 구현을 위해 여기에 *복사* VM Android 런타임으로 모노 VM에서 목록입니다. Android 런타임 내에서 변경 된 목록 이후인 경우 (예: 호출 하 여는 [ArrayAdapter&lt;T&gt;합니다. Add(T)](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter%3CT%3E.Add/p/T/) 메서드), 이러한 변경 내용을 *되지 것입니다* 관리 코드에 표시 되어야 합니다. 경우는 `JavaList<int>` 된을 사용 하는 해당 변경 내용이 표시 됩니다.

문제나, 컬렉션 인터페이스는 구현 *하지* 위의 항목 중 하나에 나열 된 **도우미 클래스**es [In]만 마샬링할:

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

속성을 적절 한 경우 Java 메서드 변환 됩니다.

-  Java 메서드 쌍 `T getFoo()` 및 `void setFoo(T)` 로 변환 된는 `Foo` 속성입니다. 예: [Activity.Intent](https://developer.xamarin.com/api/property/Android.App.Activity.Intent/)합니다.

-  Java 메서드 `getFoo()` 읽기 전용 Foo 속성으로 변환 됩니다. 예: [Context.PackageName](https://developer.xamarin.com/api/property/Android.Content.Context.PackageName/)합니다.

-  설정 전용 속성이 생성 되지 않습니다.

-  속성은 *하지* 속성 형식은 배열 되는 경우를 생성 합니다.



### <a name="events-and-listeners"></a>이벤트 및 수신기

Android Api Java 기반으로 빌드되고 해당 구성 요소의 이벤트 수신기를 연결 하기 위한 Java 패턴을 따릅니다. 이 패턴에서 사용자를 익명 클래스를 만들고 재정의할 메서드를 선언 해야 하므로 작업은 복잡할 수 경향이, 예를 들어 이것은 Java와 Android에서 작업 수행 방법:

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

참고 위 메커니즘 중 두 가지 Xamarin.Android를 사용할 수 있습니다. 수신기 인터페이스를 구현 하 고 View.SetOnClickListener을 사용한 연결 하거나 클릭 이벤트에 일반적인 C# 패러다임을 통해 만든 대리자를 연결할 수 있습니다.

API 요소에 따라 만듭니다 수신기 콜백 메서드가 void를 반환 하는 경우는 [EventHandler&lt;TEventArgs&gt; ](https://developer.xamarin.com/api/type/System.EventHandler%601/) 위임 합니다. 이러한 수신기 형식에 대 한 위의 예제와 이벤트를 생성합니다. 그러나 수신기 콜백을 반환 void가 아닌 a 및 비- **부울** 값, 이벤트 및 EventHandlers 사용 되지 않습니다. 에서는 대신 콜백 서명에 대 한 특정 대리자 생성을 이벤트 대신 속성을 추가 합니다. 대리자 호출 순서 처리 하 고 처리를 반환 하기 위해서입니다. 이 방법은 Xamarin.iOS API와 함께 수행 되는 동작과 반영 됩니다.

C# 이벤트 또는 속성 자동으로 생성 하는 경우 Android 이벤트 등록 방법:

1. 에 `set` 예를 들어 접두사 [ *설정*OnClickListener](https://developer.xamarin.com/api/member/Android.Views.View.SetOnClickListener/)합니다.

1. 에 `void` 형식을 반환 합니다.

1. 에 하나의 매개 변수가 매개 변수 형식이 인터페이스, 인터페이스에 단 하나의 메서드 및 인터페이스 이름이 끝나는 `Listener` , 예: [View.OnClick *수신기*](https://developer.xamarin.com/api/type/Android.Views.View+IOnClickListener/)합니다.


또한 수신기 인터페이스 메서드는 경우 반환 형식이의 **부울** 대신 **void**의 생성 된 *EventArgs* 하위 클래스는 포함될*Handled* 속성입니다. 값은 *Handled* 속성에 대 한 반환 값으로 사용 됩니다는 *수신기* 메서드를 기본적으로 `true`합니다.

예를 들어 Android [View.setOnKeyListener()](https://developer.xamarin.com/api/member/Android.Views.View.SetOnKeyListener/p/Android.Views.View+IOnKeyListener/) 메서드에 [View.OnKeyListener](https://developer.xamarin.com/api/type/Android.Views.View+IOnKeyListener) 인터페이스 및 [View.OnKeyListener.onKey (보기, int, KeyEvent)](https://developer.xamarin.com/api/member/Android.Views.View+IOnKeyListener.OnKey/p/Android.Views.View/Android.Views.Keycode/Android.Views.KeyEvent/) 메서드는 부울 반환 형식이 있습니다. Xamarin.Android 해당 생성 [View.KeyPress](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) 이벤트는는 [EventHandler&lt;View.KeyEventArgs&gt;](https://developer.xamarin.com/api/type/Android.Views.View+KeyEventArgs/)합니다.
*KeyEventArgs* 클래스에는 [View.KeyEventArgs.Handled](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/) 속성에 대 한 반환 값으로 사용 되는 *View.OnKeyListener.onKey()* 메서드.

다른 메서드와 대리자 기반 연결을 노출 하는 생성자에 대 한 오버 로드를 추가 하려고 합니다. 또한 수신기가 있는 여러 개의 콜백 변환 하는 것이 발견 되는 하므로 개별 콜백 구현 인지, 적절 한 확인 하려면 몇 가지 추가 검사가 필요 합니다. 해당 하는 경우 수신기에 C#을 사용 해야 하지만 인지 확인할 수는 존경 대리자 사용이 포함 된 상태로 전환 하세요. 또한 했던 "수신기" 접미사 없이 인터페이스의 일부 변환에서 대리자 대신 좋지 명백 하는 경우.

모든 수신기 인터페이스를 구현 하는 [ `Android.Runtime.IJavaObject` ](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) 인터페이스를 통해 바인딩, 수신기 클래스에서이 인터페이스를 구현 해야 하므로 구현 세부 사항으로 인해 합니다. 수신기 인터페이스의 서브 클래스에서 구현 하 여이 작업을 수행할 수 있습니다 [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) 또는 다른 겹쳐진 Android 작업 등의 Java 개체입니다.


### <a name="runnables"></a>Runnables

Java에서 사용 된 [java.lang.Runnable](https://developer.xamarin.com/api/type/Java.Lang.Runnable/) 인터페이스는 위임 메커니즘을 제공 합니다. [java.lang.Thread](https://developer.xamarin.com/api/type/Java.Lang.Thread/) 클래스는이 인터페이스의 중요 한 소비자입니다. Android는 API도의 인터페이스에 사용 됩니다.
[Activity.runOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/p/Java.Lang.IRunnable/) 및 [View.post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/p/Java.Lang.IRunnable) 주목할 만한 속합니다.

`Runnable` 단일 void 메서드를 포함 하는 인터페이스 [run ()](https://developer.xamarin.com/api/member/Java.Lang.Runnable.Run%28%29/)합니다. 따라서 인계 C#으로 바인딩을 [System.Action](http://msdn.microsoft.com/en-us/library/system.action.aspx) 위임 합니다. 제공을 허용 하는 바인딩 오버 로드는 `Action` 하 게 사용 하는 모든 API 멤버에 대 한 매개 변수는 `Runnable` 네이티브 API에 예를 들어 [Activity.RunOnUiThread()](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)) 및 [View.Post()](https://developer.xamarin.com/api/member/Android.Views.View.Post/(System.Action)).

부터 [irunnable이](https://developer.xamarin.com/api/type/Java.Lang.IRunnable/) 다양 한 인터페이스를 구현 하 고 따라서 수 있으므로를 대체 하는 대신 현재 위치에서 오버 로드 runnables로 직접 전달 되어야 합니다.


### <a name="inner-classes"></a>내부 클래스

에 두 개의 서로 다른 종류의 Java [중첩 된 클래스를](http://download.oracle.com/javase/tutorial/java/javaOO/nested.html): 정적 클래스 및 static이 아닌 클래스를 중첩 합니다.

Java 정적 중첩된 클래스 C# 중첩 형식 하는 것과 동일합니다.

Static이 아닌 중첩 클래스 라고도 *내부 클래스*는 크게 다릅니다. 이들은 바깥쪽 형식 인스턴스에 대 한 암시적 참조가 포함 하며이 개요의 범위 밖에 다른 차이점) (간에 정적 멤버를 포함할 수 없습니다.

바인딩 및 C# 사용에 관한, 정적 중첩된 클래스의 일반 중첩 된 형식으로 처리 됩니다. 내부 클래스에는 한편, 두 가지 중요 한 차이점 있는데

1. 생성자 매개 변수로 포함 하는 형식에 대 한 암시적 참조가 명시적으로 제공 되어야 합니다.

1. 내부 클래스는 내부 클래스에서 상속 된 경우 *해야* 형식 내에 중첩 될 기본 내부 클래스의 포함 하는 형식에서 상속 되는, 파생 된 형식 제공 해야 합니다 동일한 형식이 C#의 생성자를 포함 하는 형식입니다.


예를 들어는 [Android.Service.Wallpaper.WallpaperService.Engine](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) 내부 클래스입니다. 내부 클래스 이므로 [WallpaperService.Engine() 생성자](https://developer.xamarin.com/api/constructor/Android.Service.Wallpaper.WallpaperService+Engine.Engine/p/Android.Service.Wallpaper.WallpaperService/) 사용에 대 한 참조는 [WallpaperService](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService/) 인스턴스 (비교 및 대조 하 여 Java [WallpaperService.Engine ( ) 생성자](https://developer.xamarin.com/api/type/Android.Service.Wallpaper.WallpaperService+Engine/) 매개 변수를 사용 하는).

내부 클래스의 한 예에서는 파생은 CubeWallpaper.CubeEngine입니다.

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

참고 어떻게 `CubeWallpaper.CubeEngine` 내에 중첩 된 `CubeWallpaper`, `CubeWallpaper` 의 포함 하는 클래스에서 상속 `WallpaperService.Engine`, 및 `CubeWallpaper.CubeEngine` 선언 형식-를 사용 하는 생성자가 `CubeWallpaper` 위에 지정 된 것으로이 경우.


### <a name="interfaces"></a>인터페이스

Java 인터페이스는 세 가지 멤버, C#에서 문제를 일으킬 2 개를 포함할 수 있습니다.

1. 메서드

1. 유형

1. 필드


Java 인터페이스는 두 가지 유형으로 변환 됩니다.

1. 메서드 선언을 포함 하는 인터페이스 (선택 사항)입니다. Java 인터페이스와이 인터페이스의 이름이 같은 *제외 하 고* 도 포함 하는 ' *I* ' 접두사입니다.

1. 모든 필드를 포함 하는 (선택 사항) 정적 클래스는 Java 인터페이스 내에서 선언 합니다.

중첩된 형식 "에 재배치 되므로" 접두사로 바깥쪽 인터페이스 이름의 중첩된 형식 대신 바깥쪽 인터페이스의 형제입니다.

예를 들어는 [android.os.Parcelable](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) 인터페이스입니다.
*Parcelable* 인터페이스 메서드, 중첩된 형식 및 상수를 포함 합니다. *Parcelable* 인터페이스 메서드에 배치 되는 [Android.OS.IParcelable](https://developer.xamarin.com/api/type/Android.OS.IParcelable/) 인터페이스입니다.
*Parcelable* 인터페이스 상수에 배치 되는 [Android.OS.ParcelableConsts](https://developer.xamarin.com/api/type/Android.OS.ParcelableConsts/) 유형입니다. 중첩 된 [android.os.Parcelable.ClassLoaderCreator <t> </t> ](http://developer.android.com/reference/android/os/Parcelable.ClassLoaderCreator.html) 및 [android.os.Parcelable.Creator <t> </t> ](http://developer.android.com/reference/android/os/Parcelable.Creator.html) 형식이 없는 현재 제네릭 지원;의 제한으로 인해 바인딩된 지원 된 경우에 예상 되으로 존재는 *Android.OS.IParcelableClassLoaderCreator* 및 *Android.OS.IParcelableCreator* 인터페이스입니다. 예를 들어 중첩 된 [android.os.IBinder.DeathRecpient](http://developer.android.com/reference/android/os/IBinder.DeathRecipient.html) 로 바인딩된 인터페이스는 [Android.OS.IBinderDeathRecipient](https://developer.xamarin.com/api/type/Android.OS.IBinderDeathRecipient/) 인터페이스입니다.


> [!NOTE]
> Java 인터페이스 상수는 Xamarin.Android 1.9 부터는 <em>중복</em> Java 이식 단순화 하기 위해에서 코드입니다. 이렇게 하면 사용 하는 Java 코드를 포팅하는 작업을 개선 하기 위해 [android 공급자](http://developer.android.com/reference/android/provider/package-summary.html) 상수 인터페이스입니다.

이러한 형식 외에 네 개의 추가 변경 사항이:

1. 상수를 포함 하는 Java 인터페이스와 동일한 이름 가진 형식이 생성 됩니다.

1. 또한 인터페이스 상수를 포함 하는 유형 구현 된 Java 인터페이스에서 제공 하는 모든 상수를 포함 합니다.

1. 상수를 포함 하는 Java 인터페이스를 구현 하는 모든 클래스 구현 된 모든 인터페이스에서 상수를 포함 하는 새 중첩된 InterfaceConsts 형식을 가져옵니다.

1. *상수에 사용* 종류는 이제 사용 되지 않습니다.


에 대 한는 *android.os.Parcelable* 인터페이스, 즉, 이제 것는 [ *Android.OS.Parcelable* ](https://developer.xamarin.com/api/type/Android.OS.Parcelable/) 상수를 포함 하는 형식입니다. 예를 들어는 [Parcelable.CONTENTS_FILE_DESCRIPTOR](http://developer.android.com/reference/android/os/Parcelable.html#CONTENTS_FILE_DESCRIPTOR) 상수 것으로 동의 [ *Parcelable.ContentsFileDescriptor* ](https://developer.xamarin.com/api/field/Android.OS.Parcelable.ContentsFileDescriptor/) 는 로대신상수 *ParcelableConsts.ContentsFileDescriptor* 상수입니다.

상수를 포함 하는 다른 인터페이스를 구현 하는 아직 자세한 상수를 포함 하는 인터페이스에 대 한 모든 상수의 합집합 지금 생성 됩니다. 예를 들어는 [android.provider.MediaStore.Video.VideoColumns](http://developer.android.com/reference/android/provider/MediaStore.Video.VideoColumns.html) 구현는 [android.provider.MediaStore.MediaColumns](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumns/) 인터페이스입니다. 그러나 1.9, 전에 [Android.Provider.MediaStore.Video.VideoColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+Video+VideoColumnsConsts/) 형식에 선언 된 상수에 액세스 하지 못함을 [Android.Provider.MediaStore.MediaColumnsConsts](https://developer.xamarin.com/api/type/Android.Provider.MediaStore+MediaColumnsConsts/)합니다.
결과적으로 Java 식 *MediaStore.Video.VideoColumns.TITLE* 를 C# 식에 바인딩해야 *MediaStore.Video.MediaColumnsConsts.Title* 읽기 없이 검색 하기 어려운 변수인 Java 설명서의 많은 합니다. 1.9, C# 식은 됩니다 [ *MediaStore.Video.VideoColumns.Title*](https://developer.xamarin.com/api/field/Android.Provider.MediaStore+Video+VideoColumns.Title/)합니다.

또한는 [android.os.Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/) Java를 구현 하는 형식 *Parcelable* 인터페이스입니다. 인터페이스를 구현 하므로, 해당 인터페이스에서 모든 상수 예: "통해" 번들 종류 액세스할 수 있는 *Bundle.CONTENTS_FILE_DESCRIPTOR* 완벽 하 게 유효한 Java 식입니다.
이전에이 식을 C#에 이식 하려면 해야 유형을에 구현 되는 모든 인터페이스를 살펴볼 수는 *CONTENTS_FILE_DESCRIPTOR* 에서 제공 합니다. Xamarin.Android 1.9 년부터 상수를 포함 하는 Java 인터페이스를 구현 하는 클래스 갖습니다 중첩 된 *InterfaceConsts* 모든 상속 된 인터페이스 상수를 포함 하는 형식입니다. 이렇게 변환 하면 *Bundle.CONTENTS_FILE_DESCRIPTOR* 를 [ *Bundle.InterfaceConsts.ContentsFileDescriptor*](https://developer.xamarin.com/api/field/Android.OS.Bundle+InterfaceConsts.ContentsFileDescriptor/)합니다.

마지막으로, 형식은 형식을 *상수에 사용* 접미사와 같은 *Android.OS.ParcelableConsts* 는 이제 사용 되지 않음, 새로 도입 된 InterfaceConsts 이외의 중첩 형식입니다. Xamarin.Android 3.0에서 제거 됩니다.


## <a name="resources"></a>자료

이미지, 레이아웃 설명, 이진 blob 및 문자열 사전으로 응용 프로그램에 포함할 수 [리소스 파일](http://developer.android.com/guide/topics/resources/providing-resources.html)합니다.
다양 한 Android Api는에 설계 [리소스 Id에 작동](http://developer.android.com/guide/topics/resources/accessing-resources.html) 이미지 처리를 하는 대신 문자열이 나 이진 blob 직접 합니다.

예를 들어 샘플 Android 응용 프로그램 사용자 인터페이스 레이아웃을 포함 하는 ( `main.axml`), 국제화 테이블 문자열 ( `strings.xml`) 및 일부 아이콘 ( `drawable-*/icon.png`) 응용 프로그램의 "리소스" 디렉터리에 해당 리소스를 보관할 때:

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

네이티브 Android Api 파일 이름와 직접 작동 하지 않으므로 하지만 대신 리소스 Id에서 작동 합니다. 빌드 시스템이 배포에 대 한 리소스를 패키지할 되며 라는 클래스를 생성할 리소스를 사용 하는 Android 응용 프로그램을 컴파일할 때 `Resource` 각각에 대 한 포함 된 리소스의 토큰을 포함 하 합니다. 예를 들어 위의 리소스 레이아웃이는 R 클래스에 노출은 다음과 같습니다.

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

다음 사용 `Resource.Drawable.icon` 참조에는 `drawable/icon.png` 파일을 또는 `Resource.Layout.main` 참조에는 `layout/main.xml` 파일을 또는 `Resource.String.first_string` 사전 파일에 첫 번째 문자열을 참조 하도록 `values/strings.xml`합니다.


## <a name="constants-and-enumerations"></a>상수 및 열거형

네이티브 Android Api 취하 거 나 int의 의미를 확인 하는 상수 필드에 매핑해야 하는 int를 반환 하는 여러 메서드가 있어야 합니다. 이러한 메서드를 사용 하려면 사용자가 어떤 상수는 적절 한 값 보다 작은 적절 확인 하려면 설명서를 참조 하는 데 필요 합니다.

예를 들어 [Activity.requestWindowFeature (int featureID)](http://developer.android.com/reference/android/app/Activity.html#requestWindowFeature(int))합니다.

이러한 경우에.NET 열거형에 관련된 상수를 함께 그룹화 하 고 대신 열거를 수행 하는 메서드를 다시 매핑할 노력 합니다.
이 통해 잠재적인 값의 IntelliSense 선택 영역을 제공할 수 있습니다.

위의 예에서 됩니다: [Activity.RequestWindowFeature (WindowFeatures featureId)](https://developer.xamarin.com/api/member/Android.App.Activity.RequestWindowFeature/p/Android.Views.WindowFeatures/)).

이 옵션은 어떤 상수 함께 속해 파악 하는 매우 수동 프로세스가 하 고 어떤 Api 이러한 상수를 사용 합니다. 열거형으로 표현 된 더 잘 될 API에서 사용 되는 모든 상수에 대 한 버그를 제출 하세요.
