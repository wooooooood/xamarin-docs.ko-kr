---
title: "문제 해결"
description: "일반 오류 조건 및이 문제를 해결 하는 방법"
ms.topic: article
ms.prod: xamarin
ms.assetid: 63291951-7375-4CBF-BCC3-2E4AD157A2C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 23ebefcbd6114b06c39740b3b56f87aeac0b9a00
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting"></a>문제 해결

_일반 오류 조건 및이 문제를 해결 하는 방법_

## <a name="error-unable-to-find-a-version-of-xamarinforms-compatible-with"></a>오류: ". 찾을 수 없습니다 버전 Xamarin.Forms의 호환..."

에 다음과 같은 오류가 나타날 수는 **패키지 콘솔** Xamarin.Forms 솔루션 또는 Xamarin.Forms Android 응용 프로그램 프로젝트에서 모든 Nuget 패키지를 업데이트할 때 창:

```csharp
Attempting to resolve dependency 'Xamarin.Android.Support.v7.AppCompat (= 23.3.0.0)'.
Attempting to resolve dependency 'Xamarin.Android.Support.v4 (= 23.3.0.0)'.
Looking for updates for 'Xamarin.Android.Support.v7.MediaRouter'...
Updating 'Xamarin.Android.Support.v7.MediaRouter' from version '23.3.0.0' to '23.3.1.0' in project 'Todo.Droid'.
Updating 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0' to 'Xamarin.Android.Support.v7.MediaRouter 23.3.1.0' failed.
Unable to find a version of 'Xamarin.Forms' that is compatible with 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0'.
```

### <a name="what-causes-this-error"></a>이 오류 원인은 무엇입니까?

Mac (또는 Visual Studio)에 대 한 visual Studio에서 Xamarin.Forms Nuget packge 수 있는 업데이트가 나타낼 수 있습니다 *및 모든 해당 모든 종속*합니다. Xamarin Studio에서 솔루션의 **패키지** 노드 아래와 같습니다 (버전 번호가 다를 수 있습니다).

![](images/updates-available.png "Android 프로젝트 패키지 폴더")

업데이트 하려는 경우이 오류가 발생할 수 있습니다 _모든_ 패키지 합니다.

Android 프로젝트는 Android 6.0 (API 23)의 대상/컴파일 버전으로 설정 또는 아래 Xamarin.Forms에 하드 종속 때문에 이것이 *특정* 버전은 Android의 패키지를 지원 합니다. 이러한 패키지의 업데이트 된 버전이 있습니다 사용할 수 있는, Xamarin.Forms 반드시 맞지 않습니다.

이 경우 업데이트 해야 _만_ 는 **Xamarin.Forms** 종속성 호환 되는 버전에 그대로 남아 있게 이렇게 하면으로 패키지 합니다. 프로젝트에 추가 된 기타 패키지 업데이트 하기 위한 Android 지원 패키지를 일으키지으로도 개별적으로 업데이트할 수 있습니다.


> [!NOTE]
> 2.3.4 Xamarin.Forms를 사용 하는 경우 또는 더 높은 **및** Android 프로젝트의 대상/컴파일 버전 이상 Android 7.0 (API 24)으로 설정 되어 더 이상 위에서 언급 한 강한 종속성을 적용 하 고 지원을 업데이트할 수 없습니다 Xamarin.Forms 패키지와 독립적으로 패키지입니다.


### <a name="fix-remove-all-packages-and-re-add-xamarinforms"></a>모든 패키지를 제거 하 고 다시 Xamarin.Forms를 추가 하는 fix:

경우는 **Xamarin.Android.Support** 가장 간단한 문제 해결, 패키지 호환 되지 않는 버전으로 업데이트 되었습니다.

1. 다음 Android 프로젝트에서 모든 Nuget 패키지를 수동으로 삭제
2. 다시 추가 **Xamarin.Forms** 패키지 합니다.

자동으로 다운로드 됩니다는 *올바른* 다른 패키지의 버전입니다.

이 프로세스의 비디오를 보려면이 참조 하 여 [포럼 게시물](https://forums.xamarin.com/discussion/comment/170012/#Comment_170012)합니다.
