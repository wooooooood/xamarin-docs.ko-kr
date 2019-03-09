---
title: Android.Support.v7.AppCompat - 제공된 이름과 일치하는 리소스를 찾을 수 없습니다. attr ‘android:actionModeShareDrawable’
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: ff2a586773e33a1f4cf78657c3c69c22e79ed047
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670235"
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat - 제공된 이름과 일치하는 리소스를 찾을 수 없습니다. attr ‘android:actionModeShareDrawable’

1. Android 5.0 (API 21) SDK는 Android SDK Manager를 통해 뿐만 아니라 최신 추가 기능을 다운로드 했는지 확인 합니다.

2. CompileSdkVersion 21로 설정 된 응용 프로그램을 컴파일하는 있는지 확인 합니다. 필요에 따라 21도로 targetSdkVersion를 설정할 수 있습니다.

3. API 19와 같은 이전 버전에 필요한 경우 Nuget 페이지에서 발견 된 각 버전을 다운로드 하세요.

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

*참고*: 수동으로 설치한 경우이 패키지 관리자 콘솔을 통해, Xamarin.Android.Support.v4의 동일한 버전을 설치할 수도 있습니다 있는지 확인

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

스택 오버플로 참조: [https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>참고 항목

- [어떤 Android SDK 패키지를 설치해야 하나요?](~/android/troubleshooting/questions/install-android-sdk-packages.md)

