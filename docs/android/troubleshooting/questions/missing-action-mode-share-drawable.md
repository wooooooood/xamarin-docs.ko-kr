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
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "75728111"
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat - 제공된 이름과 일치하는 리소스를 찾을 수 없습니다. attr ‘android:actionModeShareDrawable’

1. Android SDK 관리자를 통해 최신 추가 항목과 Android 5.0(API 21) SDK를 다운로드했는지 확인합니다.

2. compileSdkVersion을 21로 설정한 상태로 애플리케이션을 컴파일하고 있는지 확인합니다. 선택적으로 targetSdkVersion도 21로 설정할 수 있습니다.

3. API 19와 같은 이전 버전이 필요한 경우, NuGet 페이지에서 해당 버전을 다운로드합니다.

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

> [!NOTE]
> 패키지 관리자 콘솔을 통해 수동으로 설치할 경우, 동일한 버전의 Xamarin.Android.Support.v4도 설치해야 합니다.

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

Stack Overflow 참조: [https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>관련 항목

- [어떤 Android SDK 패키지를 설치해야 하나요?](~/android/troubleshooting/questions/install-android-sdk-packages.md)
