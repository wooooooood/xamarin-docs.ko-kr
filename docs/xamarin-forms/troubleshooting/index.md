---
title: 문제 해결
description: 일반적인 오류 조건 및 해결 방법
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63291951-7375-4CBF-BCC3-2E4AD157A2C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: b38e33e05b0bb9d40582611857671d6617023b35
ms.sourcegitcommit: 4691b48f14b166afcec69d1350b769ff5bf8c9f6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728319"
---
# <a name="troubleshooting"></a>문제 해결

_일반적인 오류 조건 및 해결 방법_

## <a name="error-unable-to-find-a-version-of-xamarinforms-compatible-with"></a>오류: "Xamarin과 호환 되는 버전을 찾을 수 없습니다."

Xamarin.ios 솔루션 또는 Xamarin.ios Android 앱 프로젝트에서 모든 NuGet 패키지를 업데이트 하는 경우 **패키지 콘솔** 창에 다음 오류가 나타날 수 있습니다.

```csharp
Attempting to resolve dependency 'Xamarin.Android.Support.v7.AppCompat (= 23.3.0.0)'.
Attempting to resolve dependency 'Xamarin.Android.Support.v4 (= 23.3.0.0)'.
Looking for updates for 'Xamarin.Android.Support.v7.MediaRouter'...
Updating 'Xamarin.Android.Support.v7.MediaRouter' from version '23.3.0.0' to '23.3.1.0' in project 'Todo.Droid'.
Updating 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0' to 'Xamarin.Android.Support.v7.MediaRouter 23.3.1.0' failed.
Unable to find a version of 'Xamarin.Forms' that is compatible with 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0'.
```

### <a name="what-causes-this-error"></a>이 오류의 원인은 무엇 인가요?

Mac용 Visual Studio (또는 Visual Studio)는 Xamarin.ios NuGet *패키지 ge 및 모든 해당 종속성*에 대 한 업데이트를 사용할 수 있음을 나타낼 수 있습니다. Xamarin Studio에서 솔루션의 **패키지** 노드는 다음과 같을 수 있습니다 (버전 번호는 다를 수 있음).

![](images/updates-available.png "Android Project Packages Folder")

_모든_ 패키지를 업데이트 하려고 하면이 오류가 발생할 수 있습니다.

Android 프로젝트를 대상/컴파일 버전의 Android 6.0 (API 23)로 설정 했기 때문입니다. Xamarin.ios는 Android 지원 패키지의 *특정* 버전에 대해 하드 종속성이 있습니다. 업데이트 된 버전의 패키지를 사용할 수 있지만 Xamarin 양식이 반드시 호환 되는 것은 아닙니다.

이 경우 종속성이 호환 되는 버전에 유지 되도록 하려면 **xamarin.ios** _패키지만 업데이트 해야_ 합니다. 프로젝트에 추가한 다른 패키지는 Android 지원 패키지를 업데이트 하지 않는 한 개별적으로 업데이트 될 수도 있습니다.

> [!NOTE]
> Xamarin.ios **2.3.4 이상을 사용** 하는 경우 android 프로젝트의 target/Compile 버전이 android 7.0 (API 24) 이상으로 설정 되 면 위에서 언급 한 하드 종속성이 더 이상 적용 되지 않으며 xamarin.ios 패키지와 독립적으로 지원 패키지를 업데이트할 수 있습니다.

### <a name="fix-remove-all-packages-and-re-add-xamarinforms"></a>Fix: 모든 패키지를 제거 하 고 Xamarin. Forms를 다시 추가 합니다.

Xamarin. **지원** 패키지가 호환 되지 않는 버전으로 업데이트 된 경우 가장 간단한 해결 방법은 다음과 같습니다.

1. Android 프로젝트에서 모든 NuGet 패키지를 수동으로 삭제 한 다음
2. **Xamarin Forms** 패키지를 다시 추가 합니다.

그러면 다른 패키지의 *올바른* 버전이 자동으로 다운로드 됩니다.

이 프로세스에 대 한 비디오를 보려면이 [포럼 게시물](https://forums.xamarin.com/discussion/comment/170012/#Comment_170012)을 참조 하세요.
