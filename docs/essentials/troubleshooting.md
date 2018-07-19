---
title: 'Xamarin.Essentials: 문제 해결'
description: 이 문서에서는 Xamarin.Essentials 라이브러리를 사용 하 여 개발 하는 경우 발생 하는 문제를 해결 하는 방법을 설명 합니다.
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 06/26/2018
ms.openlocfilehash: 8cb18ab029d2fd161c60fda7e130f319b8f0c3ab
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947376"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials: 문제 해결

![시험판 버전 NuGet](~/media/shared/pre-release.png)

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>오류: 버전 충돌이 Xamarin.Android.Support.Compat에 대 한 검색

NuGet 패키지를 업데이트 하는 경우 다음 오류가 발생할 수 있습니다 (새 패키지를 추가 또는) Xamarin.Essentials를 사용 하는 Xamarin.Forms 프로젝트를 사용 하 여:

```
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue. 
 MyApp -> Xamarin.Essentials 0.8.0-preview -> Xamarin.Android.Support.CustomTabs 27.0.2.1 -> Xamarin.Android.Support.Compat (= 27.0.2.1) 
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

두 Nuget에 대 한 일치 하지 않는 종속성이 문제가입니다. 종속성의 특정 버전을 수동으로 추가 하 여이 문제를 해결할 수 있습니다 (이 예제의 **Xamarin.Android.Support.Compat**) 둘 다를 지원할 수 있는 합니다.

이 위해 수동으로 충돌의 원인을 NuGet을 추가 하 고 사용 합니다 **버전** 특정 버전을 선택 하는 목록입니다. 현재 27.0.2.1 Xamarin.Android.Support.Compat & Xamarin.Android.Support.Core.Util NuGet 버전에는이 오류가 해결 됩니다.

가리킵니다 [이 블로그 게시물](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/) 자세한 내용 및 문제를 해결 하는 방법에 대 한 비디오.

문제 또는 버그를 알려 주십시오에서 찾기로 실행 하는 경우는 [Xamarin.Essentials GitHub 리포지토리](http://github.com/xamarin/Essentials)합니다.
