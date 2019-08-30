---
title: Xamarin.Essentials Launcher
description: Xamarin.Essentials의 Launcher 클래스를 사용하면 애플리케이션이 시스템을 통해 URI를 열 수 있습니다.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 08/20/2019
ms.openlocfilehash: 276e4d9bc1294984a73ef2214cf9c1fd6c3bb89b
ms.sourcegitcommit: 9a46ee759ec4a738da348e8f8904d0f482ef0f25
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70060110"
---
# <a name="xamarinessentials-launcher"></a>Xamarin.Essentials: 시작 관리자

**Launcher** 클래스를 사용하면 애플리케이션이 시스템을 통해 URI를 열 수 있습니다. 다른 애플리케이션의 사용자 지정 URI 체계에 대한 딥 링크를 설정할 때 주로 사용됩니다. 브라우저에서 웹 사이트를 열려면 **[Browser](open-browser.md)** API를 참조해야 합니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-launcher"></a>Launcher 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

Launcher 기능을 사용하려면 `OpenAsync` 메서드를 호출하고 열려는 `string` 또는 `Uri`를 전달합니다. 필요에 따라 `CanOpenAsync` 메서드를 사용하여 디바이스의 애플리케이션이 URI 스키마를 처리할 수 있는지 확인할 수 있습니다.

```csharp
public class LauncherTest
{
    public async Task OpenRideShareAsync()
    {
        var supportsUri = await Launcher.CanOpenAsync("lyft://");
        if (supportsUri)
            await Launcher.OpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

그러면 매개 변수를 열 수 있는지 확인하고 열 수 있는 경우 여는 `TryOpenAsync`를 사용하여 단일 호출로 결합할 수 있습니다.

```csharp
public class LauncherTest
{
    public async Task<bool> OpenRideShareAsync()
    {
        return await Launcher.TryOpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

## <a name="files"></a>파일

이 기능을 사용하면 앱에서 다른 앱을 열고 파일을 볼 수 있도록 요청할 수 있습니다. Xamarin.Essentials는 자동으로 파일 형식(MIME)을 검색하고 파일을 열도록 요청합니다.

다음은 디스크에 텍스트를 작성하고 열도록 요청하는 샘플입니다.

```csharp
var fn = "File.txt";
var file = Path.Combine(FileSystem.CacheDirectory, fn);
File.WriteAllText(file, "Hello World");

await Launcher.OpenAsync(new OpenFileRequest
{
    File = new ReadOnlyFile(file)
});
```

## <a name="platform-differences"></a>플랫폼의 차이점

# <a name="androidtabandroid"></a>[Android](#tab/android)

`CanOpenAsync`에서 반환된 작업이 즉시 완료됩니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

이 디바이스의 대상 애플리케이션이 사용자 애플리케이션의 `OpenAsync`에 의해 열린 적이 없는 경우 iOS에서 앱이 열 수 있도록 허용하라는 메시지를 사용자에게 한 번 표시합니다.

`CanOpenAsync`에서 반환된 작업이 즉시 완료됩니다.

[여기](xref:UIKit.UIApplication.CanOpenUrl*)에서 iOS 구현에 대한 자세한 정보를 확인할 수 있습니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

플랫폼의 차이점이 없습니다.

-----

## <a name="api"></a>API

- [Launcher 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Launcher)
- [Launcher API 문서](xref:Xamarin.Essentials.Launcher)
