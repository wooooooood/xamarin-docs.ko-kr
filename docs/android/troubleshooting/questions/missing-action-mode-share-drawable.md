---
title: Android.Support.v7.AppCompat - 제공된 이름과 일치하는 리소스를 찾을 수 없습니다. attr ‘android:actionModeShareDrawable’
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: e688bd27d1116b2a77a12ccd6da29ea582053581
ms.sourcegitcommit: 4691b48f14b166afcec69d1350b769ff5bf8c9f6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728111"
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat - 제공된 이름과 일치하는 리소스를 찾을 수 없습니다. attr ‘android:actionModeShareDrawable’

1. Android SDK 관리자를 통해 최신 버전 및 Android 5.0 (API 21) SDK를 다운로드 해야 합니다.

2. CompileSdkVersion가 21로 설정 된 응용 프로그램을 컴파일 하는지 확인 합니다. 필요에 따라 targetSdkVersion를 21로 설정할 수 있습니다.

3. API 19와 같은 이전 버전이 필요한 경우 NuGet 페이지에 있는 해당 버전을 다운로드 하세요.

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

> [!NOTE]
> 패키지 관리자 콘솔을 통해이를 수동으로 설치 하는 경우 동일한 버전의 Xamarin.ios도 설치 해야 합니다.

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

Stack Overflow 참조: [https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>참고 항목

- [어떤 Android SDK 패키지를 설치해야 하나요?](~/android/troubleshooting/questions/install-android-sdk-packages.md)
