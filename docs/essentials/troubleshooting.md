---
title: 'Xamarin.Essentials: 문제 해결'
description: 이 문서에서는 Xamarin.Essentials 라이브러리를 사용하여 개발하는 경우 발생된 문제를 해결하는 방법을 설명합니다.
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 3440b572514c02c8c2240dc99b09aecfde06fcb9
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898755"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials: 문제 해결

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>오류: Xamarin.Android.Support.Compat에 대한 버전 충돌이 검색되었습니다.

Xamarin.Essentials를 사용하는 Xamarin.Forms 프로젝트로 NuGet 패키지를 업데이트(하거나 새 패키지를 추가)하는 경우 다음 오류가 발생할 수 있습니다.

```error
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue. 
 MyApp -> Xamarin.Essentials 0.8.0-preview -> Xamarin.Android.Support.CustomTabs 27.0.2.1 -> Xamarin.Android.Support.Compat (= 27.0.2.1) 
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

두 NuGet에서 일치하지 않는 종속성이 문제입니다. 둘 다 지원할 수 있는 종속성의 특정 버전을 수동으로 추가하여 이 문제를 해결할 수 있습니다(이 경우에 **Xamarin.Android.Support.Compat**).

이를 위해 수동으로 충돌의 원인인 NuGet을 추가하고 **버전** 목록을 사용하여 특정 버전을 선택합니다. Xamarin.Android.Support.Compat 및 Xamarin.Android.Support.Core.Util NuGet의 현재 버전 27.0.2.1은 이 오류를 해결합니다.

문제를 해결하는 방법에 대한 자세한 내용 및 비디오는 [이 블로그 게시물](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/)을 참조하세요.

문제가 발생하거나 버그를 찾은 경우 [Xamarin.Essentials GitHub 리포지토리](http://github.com/xamarin/Essentials)에서 보고해주세요.
