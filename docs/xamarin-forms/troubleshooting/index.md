---
title: 문제 해결
description: 일반 오류 조건 및 해결 방법
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63291951-7375-4CBF-BCC3-2E4AD157A2C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: fbe4fb6fce52636b59a9637ee0150c4c19fcc9da
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60850444"
---
# <a name="troubleshooting"></a>문제 해결

_일반 오류 조건 및 해결 방법_

## <a name="error-unable-to-find-a-version-of-xamarinforms-compatible-with"></a>오류: "... Xamarin.forms 버전 호환 찾을 수 없습니다."

에 다음 오류가 나타날 수 있습니다 합니다 **패키지 콘솔** Xamarin.Forms 솔루션 또는 Xamarin.Forms Android 앱 프로젝트에서 모든 Nuget 패키지를 업데이트 하는 경우 창:

```csharp
Attempting to resolve dependency 'Xamarin.Android.Support.v7.AppCompat (= 23.3.0.0)'.
Attempting to resolve dependency 'Xamarin.Android.Support.v4 (= 23.3.0.0)'.
Looking for updates for 'Xamarin.Android.Support.v7.MediaRouter'...
Updating 'Xamarin.Android.Support.v7.MediaRouter' from version '23.3.0.0' to '23.3.1.0' in project 'Todo.Droid'.
Updating 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0' to 'Xamarin.Android.Support.v7.MediaRouter 23.3.1.0' failed.
Unable to find a version of 'Xamarin.Forms' that is compatible with 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0'.
```

### <a name="what-causes-this-error"></a>이 오류 원인은 무엇 인가요?

Mac (또는 Visual Studio)에 대 한 visual Studio에서 Xamarin.Forms Nuget 패키지에 대 한 업데이트가 있다는 나타낼 수 있습니다 *및 모든 해당 종속성*합니다. Xamarin Studio에서 솔루션의 **패키지** 노드는 다음과 비슷합니다 (버전 번호가 다를 수 있습니다).

![](images/updates-available.png "Android 프로젝트 패키지 폴더")

업데이트 하려는 경우이 오류가 발생할 수 있습니다 _모든_ 패키지 있습니다.

Android 프로젝트는 Android 6.0 (API 23)의 대상/컴파일 버전으로 설정 하거나 아래 Xamarin.Forms에 대 한 강한 종속성 때문에 이것이 *특정* 버전의 Android 패키지를 지원 합니다. 해당 패키지의 업데이트 된 버전을 사용할 수 있습니다, 있지만 Xamarin.Forms 상호 반드시 호환 되지 않습니다.

이 경우 업데이트 해야 _만_ 는 **Xamarin.Forms** 종속성 호환 되는 버전에 남아 있는지 확인 하는 데는이 패키지 있습니다. 프로젝트에 추가한 다른 패키지와 Android 지원 패키지 업데이트를 일으키지 않습니다에 개별적으로 업데이트할 수 있습니다.


> [!NOTE]
> 2.3.4 Xamarin.Forms를 사용 하는 경우 또는 높은 **고** Android 프로젝트의 대상/컴파일 버전 이상 Android 7.0 (API 24)으로 설정 되어 더 이상 위에서 언급 한 하드 종속성을 적용 하 고 지원을 업데이트할 수 없습니다 Xamarin.Forms 패키지와 독립적으로 패키지입니다.


### <a name="fix-remove-all-packages-and-re-add-xamarinforms"></a>해결 방법: 모든 패키지를 제거 하 고 다시 Xamarin.Forms 추가

경우는 **Xamarin.Android.Support** 호환 되지 않는 버전으로 업데이트 된 패키지, 가장 간단한 해결 하는 것:

1. 그런 다음 Android 프로젝트에서 모든 Nuget 패키지를 수동으로 삭제
2. 다시 추가 합니다 **Xamarin.Forms** 패키지 있습니다.

이 자동으로 다운로드 합니다 *올바른* 다른 패키지의 버전입니다.

이 프로세스의 비디오를 보려면이 참조할 [포럼 게시물](https://forums.xamarin.com/discussion/comment/170012/#Comment_170012)합니다.
