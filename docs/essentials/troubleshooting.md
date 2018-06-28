---
title: 'Xamarin.Essentials: 문제 해결'
description: 이 문서에서는 Xamarin.Essentials 라이브러리와 함께 개발 하는 경우 발생 하는 문제를 해결 하는 방법에 설명 합니다.
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 06/26/2018
ms.openlocfilehash: 3dba315aec2475cb334110ba7555f773f4165aa1
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066736"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials: 문제 해결

![시험판 NuGet](~/media/shared/pre-release.png)

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>오류: 버전 충돌이 Xamarin.Android.Support.Compat에 대 한 검색

NuGet 패키지를 업데이트할 때 다음 오류가 발생할 수 있습니다 (또는 새ֶ ׀) Xamarin.Essentials를 사용 하는 Xamarin.Forms 프로젝트:

```
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue. 
 MyApp -> Xamarin.Essentials 0.7.0.17-preview -> Xamarin.Android.Support.CustomTabs 27.0.2 -> Xamarin.Android.Support.Compat (= 27.0.2) 
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

이 문제는 두 개의 NuGets에 대 한 일치 하지 않는 종속성. 종속성의 특정 버전을 수동으로 추가 하 여이 해결할 수 있습니다 (이 경우 **Xamarin.Android.Support.Compat**) 둘 다를 지원할 수 있는 합니다.

이 위해 수동으로 충돌의 원인을 있는 NuGet을 추가 하 고 사용 된 **버전** 를 특정 버전을 선택 하려면 목록입니다. 현재 버전 27.0.2 Xamarin.Android.Support.Compat NuGet의이 오류를 해결 합니다.

참조 [이 블로그 게시물](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/) 자세한 내용 및 문제를 해결 하는 방법에 대 한 비디오에 대 한 합니다.

모든 문제 또는 버그 오류가 나타나면 보고에서 찾기로 실행 되는 경우는 [Xamarin.Essentials GitHub 리포지토리](http://github.com/xamarin/Essentials)합니다.
