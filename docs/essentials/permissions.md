---
title: 'Xamarin.Essentials: 사용 권한'
description: 이 문서에서는 런타임 권한을 확인하고 요청하는 기능을 제공하는 Xamarin.Essentials의 Permissions 클래스에 대해 설명합니다.
ms.assetid: 34062D84-3E55-4AF7-A688-8551068B1E57
author: jamesmontemagno
ms.author: jamont
ms.date: 01/06/2020
ms.openlocfilehash: 21f2079ace4adae6fd84d89426e5d66692af2a0a
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "78289801"
---
# <a name="xamarinessentials-permissions"></a>Xamarin.Essentials: 사용 권한

**Permissions** 클래스는 런타임 권한을 확인하고 요청하는 기능을 제공합니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-permissions"></a>권한 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

## <a name="checking-permissions"></a>권한 확인

권한의 현재 상태를 확인하려면 상태를 가져올 특정 권한과 함께 `CheckStatusAsync` 메서드를 사용합니다.

```csharp
var status = await Permissions.CheckStatusAsync<Permissions.LocationWhenInUse>();
```

필요한 권한이 선언되지 않은 경우에는 `PermissionException`이 throw됩니다.

## <a name="requesting-permissions"></a>권한 요청

사용자에게 권한을 요청하려면 요청할 특정 권한과 함께 `RequestAsync` 메서드를 사용합니다. 사용자가 이전에 권한을 부여하고 취소하지 않은 경우 이 메서드는 즉시 `Granted`를 반환하고 대화 상자를 표시하지 않습니다. 

```csharp
var status = await Permissions.RequestAsync<Permissions.LocationWhenInUse>();
```

필요한 권한이 선언되지 않은 경우에는 `PermissionException`이 throw됩니다. 

일부 플랫폼에서 권한 요청은 한 번만 활성화될 수 있습니다. 권한이 `Denied` 상태인지 확인하고 사용자에게 수동으로 설정하도록 요청하려면 개발자가 추가 프롬프트를 처리해야 합니다.

## <a name="permission-status"></a>권한 상태

`CheckStatusAsync` 또는 `RequestAsync`를 사용하는 경우 다음 단계를 결정하는 데 사용되는 `PermissionStatus`가 반환됩니다.

* 알 수 없음 - 권한이 알 수 없는 상태입니다.
* 거부됨 - 사용자가 권한 요청을 거부했습니다.
* 사용 안 함 - 디바이스에서 이 기능을 사용할 수 없습니다.
* 부여됨 - 사용자가 권한을 부여했거나 권한이 자동으로 부여됩니다.
* 제한됨 - 제한된 상태입니다.

## <a name="available-permissions"></a>사용 가능한 권한

Xamarin.Essentials는 가능한 한 많은 권한을 추상화하려고 하지만 각 운영 체제는 서로 다른 집합의 런타임 권한을 가집니다. 또한 일부 권한에 대해 단일 API를 제공할 수 있다는 차이점이 있습니다. 다음은 현재 사용할 수 있는 권한에 대한 가이드입니다.

아이콘 지침:

* ![전체 지원](~/media/shared/yes.png "전체 지원") - 지원됨
* ![지원 안 함](~/media/shared/no.png "지원되지 않음 또는 필요 없음") - 지원되지 않음/필요하지 않음

| 사용 권한 | Android | iOS | UWP | watchOS | tvOS | Tizen |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: 
| CalendarRead   | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원 안 함](~/media/shared/no.png "UWP 지원 안 함") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원 안 함](~/media/shared/no.png "Tizen 지원 안 함") |
| CalendarWrite | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원 안 함](~/media/shared/no.png "UWP 지원 안 함") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원 안 함](~/media/shared/no.png "Tizen 지원 안 함") |
| 카메라 | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원 안 함](~/media/shared/no.png "UWP 지원 안 함") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") |
| ContactsRead | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원 안 함](~/media/shared/no.png "Tizen 지원 안 함") |
| ContactsWrite | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원 안 함](~/media/shared/no.png "Tizen 지원 안 함") |
| 손전등 | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원 안 함](~/media/shared/no.png "iOS 지원 안 함") | ![UWP 지원 안 함](~/media/shared/no.png "UWP 지원 안 함") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") |
| LocationWhenInUse | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원](~/media/shared/yes.png "tvOS 지원")  | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") |
| LocationAlways | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") |
| 미디어 | ![Android 지원 안 함](~/media/shared/no.png "Android 지원 안 함") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원 안 함](~/media/shared/no.png "UWP 지원 안 함") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원 안 함](~/media/shared/no.png "Tizen 지원 안 함") |
| 마이크 | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원](~/media/shared/yes.png "Tizen 지원") |
| 전화 | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원 안 함](~/media/shared/no.png "UWP 지원 안 함") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원 안 함](~/media/shared/no.png "Tizen 지원 안 함") |
| 사진 | ![Android 지원 안 함](~/media/shared/no.png "Android 지원 안 함") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원 안 함](~/media/shared/no.png "UWP 지원 안 함") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원](~/media/shared/yes.png "tvOS 지원") | ![Tizen 지원 안 함](~/media/shared/no.png "Tizen 지원 안 함") |
| 미리 알림 | ![Android 지원 안 함](~/media/shared/no.png "Android 지원 안 함") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원 안 함](~/media/shared/no.png "UWP 지원 안 함") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원 안 함](~/media/shared/no.png "Tizen 지원 안 함") |
| 센서 | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원](~/media/shared/yes.png "UWP 지원") | ![watchOS 지원](~/media/shared/yes.png "watchOS 지원") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원 안 함](~/media/shared/no.png "Tizen 지원 안 함") |
| Sms | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원 안 함](~/media/shared/no.png "UWP 지원 안 함") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원 안 함](~/media/shared/no.png "Tizen 지원 안 함") |
| Speech | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원](~/media/shared/yes.png "iOS 지원") | ![UWP 지원 안 함](~/media/shared/no.png "UWP 지원 안 함") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원 안 함](~/media/shared/no.png "Tizen 지원 안 함") |
| StorageRead | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원 안 함](~/media/shared/no.png "iOS 지원 안 함") | ![UWP 지원 안 함](~/media/shared/no.png "UWP 지원 안 함") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원 안 함](~/media/shared/no.png "Tizen 지원 안 함") |
| StorageWrite | ![Android 지원](~/media/shared/yes.png "Android 지원") | ![iOS 지원 안 함](~/media/shared/no.png "iOS 지원 안 함") | ![UWP 지원 안 함](~/media/shared/no.png "UWP 지원 안 함") | ![watchOS 지원 안 함](~/media/shared/no.png "watchOS 지원 안 함") | ![tvOS 지원 안 함](~/media/shared/no.png "tvOS 지원 안 함") | ![Tizen 지원 안 함](~/media/shared/no.png "Tizen 지원 안 함") |

권한이 ![지원되지 않음](~/media/shared/no.png "지원되지 않음")으로 표시되는 경우 확인 또는 요청 시 항상 `Granted`를 반환합니다.

## <a name="general-usage"></a>일반적인 사용법
다음은 권한을 처리하기 위한 일반적인 사용 패턴입니다.

```csharp
public async Task<PermissionStatus> CheckAndRequestPermissionAsync<TPermission>()
{
    var status = await Permissions.CheckStatusAsync<Permissions.LocationWhenInUse>();
    if (status != PermissionStatus.Granted)
    {
        status = await Permissions.RequestAsync<Permissions.LocationWhenInUse>();
    }

    // Additionally could prompt the user to turn on in settings

    return status;
}
```

각 권한 형식에는 메서드를 직접 호출할 수 있도록 생성된 인스턴스가 있을 수 있습니다.

```csharp
public async Task GetLocationAsync()
{
    var status = await CheckAndRequestPermissionAsync(new Permissions.LocationWhenInUse());
    if (status != PermissionStatus.Granted)
    {
        // Notify user permission was denied
        return;
    }

    var location = await Geolocation.GetLocationAsync();
}

public async Task<PermissionStatus> CheckAndRequestPermissionAsync<T>(T permission)
            where T : BasePermission
{
    var status = await permission.CheckStatusAsync();
    if (status != PermissionStatus.Granted)
    {
        status = await permission.RequestAsync();
    }

    return status;
}
```

## <a name="extending-permissions"></a>권한 확장

권한 API는 Xamarin.Essentials에 포함되지 않은 추가 유효성 검사 또는 권한이 필요한 애플리케이션을 위해 유연하고 확장 가능하도록 만들어졌습니다. `BasePermission`에서 상속되는 새 클래스를 만들고 필요한 추상 메서드를 구현합니다. 결과 

```csharp
public class MyPermission : BasePermission
{
    // This method checks if current status of the permission
    public override Task<PermissionStatus> CheckStatusAsync()
    {
        throw new System.NotImplementedException();
    }

    // This method is optional and a PermissionException is often thrown if a permission is not declared
    public override void EnsureDeclared()
    {
        throw new System.NotImplementedException();
    }

    // Requests the user to accept or deny a permission
    public override Task<PermissionStatus> RequestAsync()
    {
        throw new System.NotImplementedException();
    }
}
```

특정 플랫폼에서 권한을 구현하는 경우 `BasePlatformPermission` 클래스를 상속할 수 있습니다. 그러면 자동으로 선언을 확인하는 추가 플랫폼 도우미 메서드가 제공됩니다.

## <a name="platform-implementation-specifics"></a>플랫폼 구현 관련 정보

# <a name="android"></a>[Android](#tab/android)

권한은 Android 매니페스트 파일에 일치하는 특성이 설정되어 있어야 합니다.

자세한 내용은 [Xamarin Android 권한](https://docs.microsoft.com/xamarin/android/app-fundamentals/permissions) 설명서를 참조하세요.

# <a name="ios"></a>[iOS](#tab/ios)

권한은 `Info.plist` 파일에 일치하는 문자열이 있어야 합니다. 권한 요청 후 요청이 거부되면 해당 권한을 두 번째 요청해도 팝업이 더 이상 표시되지 않습니다. iOS의 애플리케이션 설정 화면에서 수동으로 설정을 조정하도록 사용자에게 메시지를 표시해야 합니다.

[iOS 보안 및 개인 정보 보호 기능](https://docs.microsoft.com/xamarin/ios/app-fundamentals/security-privacy) 설명서를 참조하세요.

# <a name="uwp"></a>[UWP](#tab/uwp)

권한은 패키지 매니페스트에 일치하는 기능이 선언되어 있어야 합니다.

자세한 내용은 [앱 기능 선언](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) 설명서를 참조하세요.

--------------

## <a name="api"></a>API

- [권한 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Permissions)
- [권한 API 설명서](xref:Xamarin.Essentials.Permissions)

