---
title: ''Xamarin.Essentials: 클립보드" descriiption: '이 문서에서는 애플리케이션 간에 텍스트를 복사하여 시스템 클립보드에 붙여넣을 수 있는 Xamarin.Essentials의 Clipboard 클래스를 설명합니다.'
ms.assetid: author: ms.author: ms.date: ms.custom: no-loc:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---

# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials: 클립보드

**Clipboard** 클래스를 사용하여 애플리케이션 간에 텍스트를 복사하여 시스템 클립보드에 붙여넣을 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-clipboard"></a>클립보드 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

**클립보드**에 현재 붙여넣을 준비가 된 텍스트가 있는지 확인합니다.

```csharp
var hasText = Clipboard.HasText;
```

**클립보드**의 텍스트를 설정합니다.

```csharp
await Clipboard.SetTextAsync("Hello World");
```

**클립보드**에서 텍스트를 읽습니다.

```csharp
var text = await Clipboard.GetTextAsync();
```

클립보드의 콘텐츠가 변경될 때마다 이벤트가 트리거됩니다.

```csharp
public class ClipboardTest
{
    public ClipboardTest()
    {
        // Register for clipboard changes, be sure to unsubscribe when needed
        Clipboard.ClipboardContentChanged += OnClipboardContentChanged;
    }

    void OnClipboardContentChanged(object sender, EventArgs    e)
    {
        Console.WriteLine($"Last clipboard change at {DateTime.UtcNow:T}";);
    }
}
```

> [!TIP]
> 클립보드에 대한 액세스는 기본 사용자 인터페이스 스레드에서 이루어져야 합니다. 기본 사용자 인터페이스 스레드에서 메서드를 호출하는 방법을 보려면 [MainThread](~/essentials/main-thread.md) API를 참조하세요.

## <a name="api"></a>API

- [클립보드 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [클립보드 API 문서](xref:Xamarin.Essentials.Clipboard)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Clipboard-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
