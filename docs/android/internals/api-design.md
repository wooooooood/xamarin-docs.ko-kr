---
title: Xamarin Android API 디자인 원칙
ms.prod: xamarin
ms.assetid: 3E52D815-D95D-5510-0D8F-77DAC7E62EDE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 2958e456aeb25ba39697ad82500d574907e963e4
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510758"
---
# <a name="xamarinandroid-api-design-principles"></a>Xamarin Android API 디자인 원칙

Mono의 일부인 핵심 기본 클래스 라이브러리 외에도 Xamarin.ios는 개발자가 Mono를 사용 하 여 네이티브 Android 응용 프로그램을 만들 수 있도록 다양 한 Android Api에 대 한 바인딩을 제공 합니다.

Xamarin.ios의 핵심에는 C# 전 세계와 java 세계를 연결 하 고 개발자에 게 또는 다른 .net 언어에서 C# java api에 대 한 액세스 권한을 제공 하는 interop 엔진이 있습니다.


## <a name="design-principles"></a>디자인 원칙

다음은 Xamarin Android 바인딩에 대 한 몇 가지 디자인 원칙입니다.

-  [.NET Framework 디자인 지침](https://docs.microsoft.com/dotnet/standard/design-guidelines/)을 따릅니다.

-  개발자가 Java 클래스를 서브클래싱하는 것을 허용 합니다.

-  서브 클래스는 표준 C# 구문을 사용 해야 합니다.

-  기존 클래스에서 파생 합니다.

-  체인으로 기본 생성자를 호출 합니다.

-  재정의 메서드는의 override 시스템 C#을 사용 하 여 수행 해야 합니다.

-  일반적인 Java 작업을 쉽게 수행 하 고 하드 Java 작업을 수행할 수 있습니다.

-  JavaBean 속성을 속성 C# 으로 노출 합니다.

-  강력한 형식의 API를 노출 합니다.

    - 형식 안전성을 늘립니다.

    - 런타임 오류를 최소화 합니다.

    - 반환 형식에 대 한 IDE intellisense를 가져옵니다.

    - IDE 팝업 설명서를 허용 합니다.

-  Api의 IDE 내 탐색을 권장 합니다.

    - 프레임 워크 대체를 활용 하 여 Java Classlib 노출을 최소화 합니다.

    - 적절 C# 하 고 적용 가능한 경우 단일 메서드 인터페이스 대신 대리자 (람다, 무명 메서드 및 system.object)를 노출 합니다.

    - 임의의 Java 라이브러리 ( [JNIEnv](xref:Android.Runtime.JNIEnv))를 호출 하는 메커니즘을 제공 합니다.


## <a name="assemblies"></a>어셈블리

Xamarin.ios에는 *MonoMobile 프로필*을 구성 하는 많은 어셈블리가 포함 되어 있습니다. [어셈블리](~/cross-platform/internals/available-assemblies.md) 페이지에 자세한 정보가 있습니다.

Android 플랫폼에 대 한 바인딩은 `Mono.Android.dll` 어셈블리에 포함 되어 있습니다. 이 어셈블리에는 Android Api를 사용 하 고 Android 런타임 VM과 통신 하기 위한 전체 바인딩이 포함 되어 있습니다.


## <a name="binding-design"></a>바인딩 디자인


### <a name="collections"></a>컬렉션

Android Api는 java. util 컬렉션을 광범위 하 게 활용 하 여 목록, 집합 및 지도를 제공 합니다. 이러한 요소는 바인딩에서 컬렉션의 [제네릭](xref:System.Collections.Generic) 인터페이스를 사용 하 여 노출 합니다. 기본 매핑은 다음과 같습니다.

-   JavaSet는 시스템 유형 [ICollection<T>](xref:System.Collections.Generic.ICollection`1), 도우미 클래스 Android. c a n. [<E> ](https://developer.android.com/reference/java/util/Set.html) [<T>](xref:Android.Runtime.JavaSet`1)

-   JavaList은 시스템 형식 [IList<T>](xref:System.Collections.Generic.IList`1), 도우미 클래스 Android. [<E> ](https://developer.android.com/reference/java/util/List.html) [<T>](xref:Android.Runtime.JavaList`1)

-   TValue [< K, v >](https://developer.android.com/reference/java/util/Map.html) 시스템 유형 [IDictionary < TKey, >](xref:System.Collections.Generic.IDictionary`2), 도우미 클래스 [JavaDictionary < K, v >](xref:Android.Runtime.JavaDictionary`2)에 매핑됩니다.

-   JavaCollection은 시스템 형식 [ICollection<T>](xref:System.Collections.Generic.ICollection`1), helper 클래스에 매핑됩니다. [<E> ](https://developer.android.com/reference/java/util/Collection.html) [<T>](xref:Android.Runtime.JavaCollection`1)

이러한 유형의 더 빠른 복사를 용이 하 게 하는 도우미 클래스를 제공 했습니다. 가능 하면 또는 [`List<T>`](xref:System.Collections.Generic.List`1) [`Dictionary<TKey, TValue>`](xref:System.Collections.Generic.Dictionary`2)와 같이 프레임 워크에서 제공 하는 구현 대신 제공 된 컬렉션을 사용 하는 것이 좋습니다. [Android Runtime](xref:Android.Runtime) 구현은 내부적으로 네이티브 Java 컬렉션을 사용 하므로 android API 멤버에 전달할 때 네이티브 컬렉션 간에 복사 하지 않아도 됩니다.

인터페이스 구현을 해당 인터페이스를 허용 하는 Android 메서드에 전달할 수 있습니다. 예를 들어 `List<int>` [arrayadapter&lt;int&gt;(Context, int, IList&lt;&gt;int)](xref:Android.Widget.ArrayAdapter`1) 생성자에를 전달 합니다. *그러나*android runtime 구현을 *제외한* 모든 구현에 대해 Mono VM에서 android 런타임 VM으로 목록을 *복사* 하는 작업이 포함 됩니다. 이후 목록이 Android 런타임 내에서 변경 되 면 (예: [&lt;arrayadapter T&gt;를 호출 하 여) Add (T)](xref:Android.Widget.ArrayAdapter`1.Add*) 메서드)는 관리 코드에 표시 *되지* 않습니다. 을 `JavaList<int>` 사용 하는 경우 이러한 변경 내용이 표시 됩니다.

위에 나열 된 **도우미 클래스**중 하나가 *아닌* Rephrased, Collections 인터페이스 구현은 [In]만 마샬링합니다.

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

적절 한 경우 Java 메서드는 속성으로 변환 됩니다.

-  Java 메서드 `T getFoo()` 및 `void setFoo(T)` 는 `Foo` 속성으로 변환 됩니다. 예제: [활동 의도](xref:Android.App.Activity.Intent)입니다.

-  Java 메서드 `getFoo()` 는 읽기 전용 Foo 속성으로 변환 됩니다. 예제: [Context.PackageName](xref:Android.Content.Context.PackageName).

-  설정 전용 속성은 생성 되지 않습니다.

-  속성 형식이 배열인 경우에는 속성이 생성 *되지 않습니다* .



### <a name="events-and-listeners"></a>이벤트 및 수신기

Android Api는 Java 위에 빌드되어 있으며 해당 구성 요소는 이벤트 수신기를 연결 하기 위한 Java 패턴을 따릅니다. 이 패턴은 사용자가 익명 클래스를 만들고 재정의할 메서드를 선언 해야 하기 때문에 복잡할 수 있습니다. 예를 들어 다음은 Java를 사용 하 여 Android에서 작업을 수행 하는 방법입니다.

```csharp
final android.widget.Button button = new android.widget.Button(context);

button.setText(this.count + " clicks!");
button.setOnClickListener (new View.OnClickListener() {
    public void onClick (View v) {
        button.setText(++this.count + " clicks!");
    }
});
```

이벤트를 C# 사용 하는 동일한 코드는 다음과 같습니다.

```csharp
var button = new Android.Widget.Button (context) {
    Text = string.Format ("{0} clicks!", this.count),
};
button.Click += (sender, e) => {
    button.Text = string.Format ("{0} clicks!", ++this.count);
};
```

위의 두 메커니즘 모두 Xamarin. Android에서 사용할 수 있습니다. 수신기 인터페이스를 구현 하 고 View. Setonclick 수신기를 사용 하 여 연결 하거나 일반적인 C# 패러다임을 통해 만든 대리자를 클릭 이벤트에 연결할 수 있습니다.

수신기 콜백 메서드에 void 반환이 있는 경우 [&lt;EventHandler teventargs&gt; ](xref:System.EventHandler`1) 대리자를 기반으로 API 요소를 만듭니다. 이러한 수신기 형식에 대 한 위의 예제와 같은 이벤트를 생성 합니다. 그러나 수신기 콜백이 void가 아닌 값을 반환 하 고 **부울** 이 아닌 값을 반환 하는 경우에는 Events 및 EventHandlers가 사용 되지 않습니다. 대신 콜백 시그니처의 특정 대리자를 생성 하 고 이벤트 대신 속성을 추가 합니다. 그 이유는 대리자 호출 순서를 처리 하 고 처리를 반환 하기 위한 것입니다. 이 방법은 Xamarin.ios API를 사용 하 여 수행 하는 작업을 미러링합니다.

C#이벤트 또는 속성은 Android 이벤트 등록 방법의 경우에만 자동으로 생성 됩니다.

1. 에는 `set` 접두사가 있습니다 (예: [on클릭 수신기 *설정*](xref:Android.Views.View.SetOnClickListener*)).

1. 에는 `void` 반환 형식이 있습니다.

1. 는 매개 변수를 하나만 허용 하 고 매개 변수 형식은 인터페이스 이며 인터페이스에는 메서드가 하나만 있으며 인터페이스 이름은에서 `Listener` 끝납니다. 예를 들어, [View. OnClick *수신기*](xref:Android.Views.View.IOnClickListener)입니다.


또한 수신기 인터페이스 메서드의 반환 형식이 **void**가 아닌 **부울** 인 경우 생성 된 *EventArgs* 하위 클래스에는 *처리* 된 속성이 포함 됩니다. *처리* 된 속성의 값은 *수신기* 메서드의 반환 값으로 사용 되 고 기본값은로 `true`설정 됩니다.

예를 들어 Android [보기. setOnKeyListener ()](xref:Android.Views.View.SetOnKeyListener*) 메서드는 [onKey (view, int, KeyEvent)](xref:Android.Views.View.IOnKeyListener.OnKey*) 메서드를 사용할 때 [뷰](xref:Android.Views.View.IOnKeyListener) . onkeylistener 인터페이스를 허용 하 고, Xamarin.ios는 해당 하는 [system.windows.forms.keyeventargs.handled&lt;&gt;](xref:Android.Views.View.KeyEventArgs)이벤트를 생성 합니다. [KeyPress](xref:Android.Views.View.KeyPress) 이벤트는 EventHandler 뷰입니다.
*System.windows.forms.keyeventargs.handled* 클래스에는 [system.windows.forms.keyeventargs.handled](xref:Android.Views.View.KeyEventArgs.Handled) 속성이 있습니다 .이 속성은 *onKey ()* 메서드에 대 한 반환 값으로 사용 됩니다.

대리자 기반 연결을 노출 하기 위해 다른 메서드 및 ctors에 대 한 오버 로드를 추가 하려고 합니다. 또한 여러 콜백이 있는 수신기는 개별 콜백 구현이 적절 한지 여부를 확인 하기 위해 몇 가지 추가 검사를 수행 해야 하므로 식별 된 대로 변환 합니다. 해당 하는 이벤트가 없는 경우에서 수신기를 사용 해야 합니다 C#. 그러나이 경우에는 관심을 가질 수 있는 모든 것을 고려해 야 합니다. 또한 "수신기" 접미사가 없는 인터페이스를 변환 하는 것은 대리자 대체를 활용 하는 것이 좋습니다.

모든 수신기 인터페이스는을 구현 합니다.[`Android.Runtime.IJavaObject`](xref:Android.Runtime.IJavaObject)
인터페이스는 바인딩의 구현 세부 정보 때문에 수신기 클래스가이 인터페이스를 구현 해야 합니다. 이 작업은 Android 작업과 같은 다른 래핑된 Java 개체 또는 [java](xref:Java.Lang.Object) 의 서브 클래스에 수신기 인터페이스를 구현 하 여 수행할 수 있습니다.


### <a name="runnables"></a>Runnables

Java는 node.js [인터페이스를](xref:Java.Lang.Runnable) 활용 하 여 위임 메커니즘을 제공 합니다. 이 인터페이스의 주목할 만한 소비자는 [java](xref:Java.Lang.Thread) 클래스입니다. Android는 API에도 인터페이스를 사용 했습니다.
[작업. runOnUiThread ()](xref:Android.App.Activity.RunOnUiThread*) 및 [View.post ()](xref:Android.Views.View.Post*) 는 주목할 만한 예입니다.

인터페이스 `Runnable` 에는 단일 void 메서드인 [run ()](xref:Java.Lang.Runnable.Run)이 포함 되어 있습니다. 따라서를 System.object C# 로 바인딩하는 것이 자동으로 [수행](xref:System.Action) 됩니다. 기본 api `Action` 에서를 `Runnable` 사용 하는 모든 API 멤버에 대 한 매개 변수를 허용 하는 바인딩 (예: [runonuithread ()](xref:Android.App.Activity.RunOnUiThread*) 및 [View.Post ())](xref:Android.Views.View.Post*)에 오버 로드를 제공 했습니다.

몇 가지 형식이 인터페이스를 구현 하므로 직접 runnables으로 전달 될 수 있으므로 [IRunnable](xref:Java.Lang.IRunnable) 가능한 오버 로드를 대신 사용할 수 있습니다.


### <a name="inner-classes"></a>내부 클래스

Java에는 정적 중첩 클래스와 비정적 클래스 라는 두 가지 유형의 [중첩 클래스가](http://download.oracle.com/javase/tutorial/java/javaOO/nested.html)있습니다.

Java 정적 중첩 클래스는 C# 중첩 형식과 동일 합니다.

*내부 클래스*라고도 하는 비정적 중첩 클래스는 상당히 다릅니다. 여기에는 바깥쪽 형식의 인스턴스에 대 한 암시적 참조가 포함 되며 정적 멤버를 포함할 수 없습니다 (이 개요의 범위를 벗어나는 다른 차이점).

바인딩 및 C# 사용에 대 한 정적 중첩 클래스는 일반 중첩 형식으로 처리 됩니다. 내부 클래스는 다음과 같은 두 가지 중요 한 차이점이 있습니다.

1. 포함 하는 형식에 대 한 암시적 참조는 생성자 매개 변수로 명시적으로 제공 되어야 합니다.

1. 내부 클래스에서 상속 하는 경우 *내부 클래스는* 기본 내부 클래스의 포함 하는 형식에서 상속 되는 형식 내에 중첩 되어야 하 고 파생 된 형식은 C# 포함 하는 형식과 동일한 형식의 생성자를 제공 해야 합니다.

예를 들어 [WallpaperService](xref:Android.Service.Wallpaper.WallpaperService.Engine) 내부 클래스를 예로 들어 보겠습니다. 내부 클래스 이기 때문에 [WallpaperService () 생성자](xref:Android.Service.Wallpaper.WallpaperService.Engine#ctor) 는 [WallpaperService](xref:Android.Service.Wallpaper.WallpaperService) 인스턴스에 대 한 참조를 사용 합니다 (매개 변수를 사용 하지 않는 Java WallpaperService () 생성자와 비교 및 대조).

내부 클래스의 파생 예는 CubeWallpaper 무늬입니다. Cubewallpaper:

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

`CubeWallpaper` `WallpaperService.Engine` `CubeWallpaper` 는 내 `CubeWallpaper`에서 중첩 되 고,의 포함 하는 클래스에서 상속 `CubeWallpaper.CubeEngine` 되며, 선언 형식을 사용 하는 생성자를 포함 합니다 .이 경우에는 위에 지정 된 것과 같습니다. `CubeWallpaper.CubeEngine`

### <a name="interfaces"></a>인터페이스

Java 인터페이스에는 세 가지 멤버 집합이 포함 될 수 있으며, 그 중 C#두 가지는 다음과 같은 문제를 일으킵니다.

1. 메서드

1. 유형

1. 필드

Java 인터페이스는 다음과 같은 두 가지 형식으로 변환 됩니다.

1. 메서드 선언이 포함 된 (선택적) 인터페이스입니다. 이 인터페이스에는 ' *I* ' 접두사를 포함 하는 것을 *제외* 하 고 Java 인터페이스와 동일한 이름이 있습니다.

1. Java 인터페이스 내에 선언 된 모든 필드를 포함 하는 (옵션) 정적 클래스입니다.

중첩 된 형식은 바깥쪽 인터페이스 이름을 접두사로 사용 하는 중첩 형식 대신 바깥쪽 인터페이스의 형제로 "재배치" 됩니다.

예를 들어 [넣는](xref:Android.OS.Parcelable) 인터페이스를 살펴보겠습니다.
*넣는* 인터페이스에는 메서드, 중첩 형식 및 상수가 포함 되어 있습니다. *넣는* 인터페이스 메서드는 [IParcelable](xref:Android.OS.IParcelable) 인터페이스에 배치 됩니다.
*넣는* 인터페이스 상수는 [ParcelableConsts](xref:Android.OS.ParcelableConsts) 형식에 배치 됩니다. 중첩 된 [넣는. ClassLoaderCreator&lt;t >](https://developer.android.com/reference/android/os/Parcelable.ClassLoaderCreator.html) 및 [> 넣는&lt;](https://developer.android.com/reference/android/os/Parcelable.Creator.html) 형식은 현재 제네릭 지원의 제한 사항으로 인해 바인딩되어 있지 않습니다. 지원 되는 경우 는 *android. IParcelableClassLoaderCreator* 및 *IParcelableCreator* 인터페이스로 제공 됩니다. 예를 들어, 중첩 된 [DeathRecipient](https://developer.android.com/reference/android/os/IBinder.DeathRecipient.html) 인터페이스는 [IBinderDeathRecipient](xref:Android.OS.IBinderDeathRecipient) 인터페이스에 바인딩되어 있습니다.

> [!NOTE]
> Xamarin.ios 1.9부터 java 인터페이스 상수는 Java 코드 포팅 간소화를 위한 노력으로 _중복_ 됩니다. 이를 통해 [android 공급자](https://developer.android.com/reference/android/provider/package-summary.html) 인터페이스 상수를 사용 하는 Java 코드의 이식 기능을 향상 시킬 수 있습니다.

위의 형식 외에도 다음과 같은 4 가지 추가 변경 내용이 있습니다.

1. Java 인터페이스와 이름이 같은 형식이 생성 되어 상수를 포함 합니다.

1. 인터페이스 상수를 포함 하는 형식에는 구현 된 Java 인터페이스에서 제공 되는 모든 상수만 포함 됩니다.

1. 상수를 포함 하는 Java 인터페이스를 구현 하는 모든 클래스는 구현 된 모든 인터페이스의 상수를 포함 하는 새 중첩 된 InterfaceConsts 형식을 가져옵니다.

1. *Consts* 형식은 이제 사용 되지 않습니다.


*넣는* 인터페이스의 경우에는이는 상수를 포함 하는 [*넣는*](xref:Android.OS.Parcelable) 형식입니다. 예를 들어, [넣는. CONTENTS_FILE_DESCRIPTOR](https://developer.android.com/reference/android/os/Parcelable.html#CONTENTS_FILE_DESCRIPTOR) 상수는 *ParcelableConsts Filedescriptor* 상수 대신 [*넣는*](xref:Android.OS.Parcelable.ContentsFileDescriptor) 로 바인딩됩니다.

아직 추가 상수가 포함 된 다른 인터페이스를 구현 하는 상수가 포함 된 인터페이스의 경우 이제 모든 상수의 합집합이 생성 됩니다. 예를 들어, [MediaColumns](xref:Android.Provider.MediaStore.MediaColumns) 인터페이스를 구현 하는 [android](https://developer.android.com/reference/android/provider/MediaStore.Video.VideoColumns.html) . i d. i d. 그러나 1.9 이전 버전의 경우 [MediaColumnsConsts](xref:Android.Provider.MediaStore.MediaColumnsConsts)에 선언 된 상수에 액세스할 수 있는 방법이 없습니다. [Video. videostore](xref:Android.Provider.MediaStore.Video.VideoColumnsConsts) . video.
결과적으로 Java 식 Mediastore를 C# 식의 여러 java 설명서를 읽어 서 검색 하기 어려운 식 *mediastore* . d a. d. 1\.9에서 해당 하 C# 는 식은 [Mediastore. videocolumns입니다](xref:Android.Provider.MediaStore.Video.VideoColumns.Title).

또한 Java *넣는* 인터페이스를 구현 하는 [android. s a s. 번들](xref:Android.OS.Bundle) 형식을 고려 합니다. 인터페이스를 구현 하므로 해당 인터페이스에 대 한 모든 상수는 번들 형식 (예: *CONTENTS_FILE_DESCRIPTOR* 은 완벽 하 게 유효한 Java 식)에 액세스할 수 있습니다.
이전에는이 식을로 C# 이식 하기 위해 구현 된 모든 인터페이스를 확인 하 여 *CONTENTS_FILE_DESCRIPTOR* 에서 가져온 형식을 확인 해야 합니다. Xamarin Android 1.9부터 상수를 포함 하는 Java 인터페이스를 구현 하는 클래스에는 상속 된 모든 인터페이스 상수가 포함 될 중첩 된 *InterfaceConsts* 형식이 있습니다. 이렇게 하면 *CONTENTS_FILE_DESCRIPTOR* 를 [*InterfaceConsts*](xref:Android.OS.Bundle.InterfaceConsts.ContentsFileDescriptor)로 쉽게 변환할 수 있습니다.

마지막으로 *ParcelableConsts* 와 같은 *consts* 접미사가 있는 형식은 새로 도입 된 InterfaceConsts 중첩 형식 이외에는 더 이상 사용 되지 않습니다. Xamarin Android 3.0에서 제거 됩니다.


## <a name="resources"></a>리소스

이미지, 레이아웃 설명, 이진 blob 및 문자열 사전은 응용 프로그램에 [리소스 파일로](https://developer.android.com/guide/topics/resources/providing-resources.html)포함할 수 있습니다.
다양 한 Android Api는 이미지, 문자열 또는 이진 blob을 직접 처리 하는 대신 [리소스 id에서 작동](https://developer.android.com/guide/topics/resources/accessing-resources.html) 하도록 설계 되었습니다.

예를 들어 사용자 인터페이스 레이아웃 ( `main.axml`), 국제화 테이블 문자열 ( `strings.xml`) 및 일부 아이콘 ( `drawable-*/icon.png`)이 포함 된 샘플 Android 앱은 응용 프로그램의 "resources" 디렉터리에 해당 리소스를 유지 합니다.

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

네이티브 Android Api는 파일 이름으로 직접 작동 하지 않고 대신 리소스 Id에 대해 작동 합니다. 리소스를 사용 하는 Android 응용 프로그램을 컴파일할 때 빌드 시스템은 배포를 위해 리소스를 패키지 하 고 포함 된 `Resource` 각 리소스에 대 한 토큰을 포함 하는 라는 클래스를 생성 합니다. 예를 들어 위의 리소스 레이아웃의 경우 R 클래스가 노출 하는 항목은 다음과 같습니다.

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

그런 다음를 사용 `Resource.Drawable.icon` 하 여 `drawable/icon.png` 파일을 참조 하거나 `Resource.Layout.main` `layout/main.xml` 파일을 참조 하거나 `Resource.String.first_string` 사전 파일 `values/strings.xml`의 첫 번째 문자열을 참조 합니다.


## <a name="constants-and-enumerations"></a>상수 및 열거형

네이티브 Android Api에는 int가 의미 하는 것을 확인 하기 위해 상수 필드에 매핑해야 하는 int를 사용 하거나 반환 하는 여러 메서드가 있습니다. 이러한 메서드를 사용 하려면 사용자가 설명서를 참조 하 여 적절 한 값이 되는 상수를 확인 해야 합니다 .이는 이상적인 값 보다 낮습니다.

예를 들어, [작업. requestWindowFeature (Int featureID)](https://developer.android.com/reference/android/app/Activity.html#requestWindowFeature(int))를 참조 하세요.

이러한 경우 관련 상수를 .NET 열거형으로 그룹화 하 고 메서드를 다시 매핑하여 대신 열거형을 사용 합니다.
이렇게 하면 IntelliSense에서 잠재적 값을 선택할 수 있습니다.

위의 예제는 다음과 같습니다. [작업. RequestWindowFeature (WindowFeatures featureId)](xref:Android.App.Activity.RequestWindowFeature*).

이는 함께 사용 되는 상수와 이러한 상수를 사용 하는 Api를 파악 하는 매우 수동 프로세스입니다. API에서 사용 되는 상수에 대 한 버그를 파일에 표시 하 여 열거형으로 더 잘 표현 해 주세요.
